---
authors:
  - serdar

title: "XPages Tip: Realizing the Cost of a Value Binding..."

slug: xpages-tip-realizing-the-cost-of-a-value-binding...

categories:
  - Tips & Tricks

date: 2014-09-08T09:45:00+02:00

tags:
  - domino-dev
  - xpages
---

A little tip on XPages development...

We are binding values everywhere on our pages. Are you optimizing the way you use them?

Basic value binding is straight forward. If you use a data source, scope variable or any other property/map/list object from your bean, etc, you are good. However if you 'compute' something to get a value, that computation has a processor and a memory cost.
<!-- more -->
Let's see an example here. Consider the following code, where we want to get some input via a combobox:

```xml
<xp:div rendered="#{javascript:getValues().size!=0}">
       <xp:label
               value="Input:"
               id="label1"
               for="myField" />
       <xp:comboBox
               id="myField"
               rendered="#{javascript:myDoc.isEditable()}"
               value="#{myDoc.myField}"
               readonly="#{javascript:getValues().size==1}"
               defaultValue="#{javascript:getValues().size==1?getValues().get(1):''}">
               <xp:selectItem
                       itemLabel="&gt;&gt; Select"
                       itemValue=""
                       id="selectItem1">
               </xp:selectItem>
               <xp:selectItems id="selectItems1">
                       <xp:this.value><![CDATA[#{javascript:getValues()}]]></xp:this.value>
               </xp:selectItems>
       </xp:comboBox>
       <xp:text
               rendered="#{javascript:!myDoc.isEditable()}"
               value="#{myDoc.myField}">
       </xp:text>
</xp:div>
```


We have a SSJS function "***getValues()*** " which generates a list combining some values from the form and maybe from the user name (I have overused it just to make my point obvious). Now, can you guess the number of times this function gets called?

When the page is loaded, it will be called **11 times** !!! If you do partial refresh on the specific div, or do a full refresh, it will be called 11 times again. Even if you use partial refresh on another part of the page, it will be called 6 times or so (Always consider using partial execution!).

If we use a couple of lookups within the method, that would be a disaster.

So far, caching seems inevitable, right?

If it's something you want to use across the application, the session or the view, you know you should cache expensive computations within a scope variable or a bean. But sometimes, what you need is very specific to the context of the current page and it seems meaningless to cache those values across any level. Here, you may be acting lazy and also underestimating some undercover mechanisms that might multiply the cost of your computation by several times.

For instance, you get a country, state and city inputs at the page, you compose a value listing depending on the user. So you would be displaying a user's stores in the specific area, and to add more complication, you are looking for his team members and looking for their stores as well. In such cases, it would be really difficult to engineer a good caching mechanism and such a task would be error-prone.

Before caching, you can at least decrease the number of times it gets called by using a panel. Let's change our code a little bit:

```xml
<xp:panel
       id="div">
       <xp:this.dataContexts>
               <xp:dataContext
                       var="valueList"
                       value="#{javascript:getValues()}">
               </xp:dataContext>
       </xp:this.dataContexts>
       <xp:div
               rendered="#{javascript:valueList.size!=0}">
               <xp:label
                       value="Input:"
                       id="label1"
                       for="myField" />
               <xp:comboBox
                       id="myField"
                       rendered="#{javascript:myDoc.isEditable()}"
                       value="#{myDoc.myField}"
                       readonly="#{javascript:valueList.size==1}"
                       defaultValue="#{javascript:valueList.size==1?valueList.get(1):''}">
                       <xp:selectItem
                               itemLabel="&gt;&gt; Select"
                               itemValue=""
                               id="selectItem1">
                       </xp:selectItem>
                       <xp:selectItems
                               id="selectItems1">
                               <xp:this.value><![CDATA[#{javascript:valueList}]]></xp:this.value>
                       </xp:selectItems>
               </xp:comboBox>
               <xp:text
                       rendered="#{javascript:!myDoc.isEditable()}"
                       value="#{myDoc.myField}">
               </xp:text>
       </xp:div>
</xp:panel>
```


Here, we have wrapped our div with a panel and which holds a dataContext element. This is going to be valid only within the boundries of this panel.

BTW, It's a really handy approach to use panels in such cases. Panels are 'independent islands' that can keep their own data sources. For instance, you may have a sub-sub-sub-sub-document somewhere in your page. You don't need to define a global data source in the parent view context. You can use a panel, define your data source within that panel and you can render it only when you need it, so your data source will not be processed if your panel is hidden.

<br />

In this updated case, our values will get loaded twice on the first load, instead of 11. Good progress. But it will run four times on the partial refresh. Clearly it's not enough for many cases.

The real question is: Why is it running so many times? The answer lies beneath how JSF works.

In the first launch, it runs once when it's building the component tree. It needs to find out which elements will be rendered and which will not. Then the RENDER_RESPONSE phase comes in. Now it's time to render all components so it computes every value bindings again.

In a Partial refresh, it runs several times, because first, it needs to apply request values. Some stuff probably changed on the browser-side, and all value-bindings should be updated (field values will be written, scope values will be set, etc.). Then it will process validations, because the written values on the previous phase was tentative. Some components accepted those values as temporary (that's why we call *component.getSubmittedValue()* , instead of *component.getValue()* in some cases). All value-bindings computed again.

After validation passed, it will update model values, which means that all validation results (also converter results) will be submitted. Of course it computed values again. Finally, it will render the response and compute everything one last time.

Runtime Value-binding computations happen differently depending on which type of attributes you are using or how you declare them. If you want to see the complete picture by JSF phases, read the fantastic blog series of my friend Paul Withers ([part1](http://www.intec.co.uk/understanding-partial-execution-part-one-refresh/), [part2](http://www.intec.co.uk/understanding-partial-execution-part-two-execution/), [part3](http://www.intec.co.uk/understanding-partial-execution-part-three-jsf-lifecycle/)). This blog series is particularly important for knowing best practices on using different value bindings in different declarations and using partial execution on action elements.

<br />

So far, we have agreed on the cost of computations on our page. In some cases, it would be difficult to cache complex data lookups. But don't be lazy.

One way is using a cached lookup like in [this snippet](http://openntf.org/XSnippets.nsf/snippet.xsp?id=dblookup-dbcolumn-with-cache-sort-and-unique) (or it's [Java version](http://openntf.org/XSnippets.nsf/snippet.xsp?id=pure-java-version-of-dblookup-dbcolumn-with-cache-sort-and-unique) for your beans). This is going to lower the cost of all lookups used during calculation. Or you can use requestScope to cache values. Because it gets cleared for every request!

One reminder here, using caching will need a special attention on cases where you want to use some values from the page. "getValues()" method is going to be cached on the first run and that's not going to be on the RENDER_RESPONSE phase, where we hold the final values for everything. The first time it computes our values will be either the initialization or the apply request phase. So you need to be careful what you are consuming from the page, e.g. default values will not be written into components, validations and conversions will not be finalized, etc.

I also want to introduce some more tricks around value-bindings. I see some stackoverflow questions that make me think developers are making wrong assumptions on how JSF rendering process works. For those, especially from the classical domino development background, entire life-cycle processing and data/back-end relation may not seem natural, comparing to the old paradigm. But the most important point is that this new paradigm is entirely compatible to the modern application development frameworks. It might seem difficult at start :)
