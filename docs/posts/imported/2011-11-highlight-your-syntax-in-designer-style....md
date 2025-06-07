---
authors:
  - serdar

title: "Highlight your syntax in Designer style..."

slug: highlight-your-syntax-in-designer-style...

categories:
  - Portfolio

date: 2011-11-23T01:15:00Z

tags:
  - javascript
  - open-source
  - openntf
  - xsnippets
---

For a while, I am part of a fantastic team, including [Niklas Heidloff](http://heidloff.net/), [Frank van der Linden](http://www.domino-weblog.nl/) and [Bruce Elgort](http://bruceelgort.com/). We are working with pleasure on an [OpenNTF](http://www.openntf.org "OpenNTF") project named "[XSnippets](http://www.openntf.org/blogs/openntf.nsf/d6plinks/NHEF-8NNBK7)". As you may already know, [Beta version of XSnippets](http://openntf.org/xsnippets) has been online for a week. Meanwhile, we also welcomed [René Winkelmeyer](http://blog.winkelmeyer.com/). He will take care of the most shiny part of XSnippets: [Designer plugin](http://blog.winkelmeyer.com/web/blog.nsf/entry.xsp?permalink=integration-of-xsnippets-into-domino-designer).
<!-- more -->
![A picture named M2](../../images/imported/highlight-your-syntax-in-designer-style-M2.gif)

So far, I am very happy about the community reaction... Tweets, blogs and personal feedbacks are making us very motivated and delighted...

One of the most important features of this project is that we are not only developing a database, we are also documenting how we are doing... So far, we had very important experiences like 4 different developers from 4 different countries (3 time zones) working and coordinating together. We are using SVN for code sharing, LotusLive Activities for documentation, communication and follow-up, mockups for visualization, etc.

There will be blogs and videos about what we are doing...

I got many questions about syntax highlighting. I'll cover this issue today and you will have some give-aways from this post :)

Syntax highlighting is one of the most important parts of the project. We have considered many different perspectives like server-side, as-you-type, etc. There are lots of resources, widgets and libraries out there. Our criterias were licensing, customization, xml-support and flexibility.

After digging, we found Alex Gorbatchev's [Syntax Highlighter](http://alexgorbatchev.com/SyntaxHighlighter/) library. It was also used by [Keith Strickland](http://xprentice.gbs.com/) on his [OpenNTF](http://www.openntf.org "OpenNTF") project [XBlog](http://xblog.openntf.org/).

The library is very easy to deploy, but a bit difficult to customize. Because you need to know regular expressions and javascript very well. After a long effort, I customized its brushes according to Designer coloring. It's not perfect but enough to use on snippets.

XPages source code and Lotusscript didn't exist on its repository. So we had to create them. Lotusscript was easy. It's based on VBScript. The only thing we needed was the list of all keywords which I found very easily :)

However, XPages was a bit complicated. It's based on XML but there were problems. Because in Domino Designer, coloring scheme is different from common XML. For instance, CDATA sections normally highlighted in XML. But since XPages has code inside those tags, Designer starts highlighting at the end of CDATA identifier. Normally, it's not that difficult to exclude an identifier in Regex. But client-side javascript (ECMA) does not support lookbehinds. So I had to use some javascript magic.

Previous paragraph may not mean anything to you. I can understand you; before XSnippets, I didn't know anything about those :)

Anyway. enough talking...

You may use this on your blog. Currently it supports many of blog engines. For Wordpress and Blogger, you may read detailed instructions on [Syntax Highlighter](http://alexgorbatchev.com/SyntaxHighlighter/) project site. I will provide necessary CSS and Brush files for Designer-compatible coloring and syntax highlighting. I am working on embedding this on Domino Blog but Domino Blog template messes with HTML and I couldn't find a descent solution for XSP highlighting. There is a problem with "\&lt;!\[CDATA\[ ... \]\]\&gt;" processing. I'll blog when I solve this issue :)

Anyway, you'll need these files:

- [shThemeXSnippets.css](https://github.com/OpenNTF/XSnippets/blob/master/xsnippets-disk/syntaxhighlighter/css/shThemeXSnippets.css) for fonts and colors specific to Domino Designer.
- [shBrushXsp.js](https://github.com/OpenNTF/XSnippets/blob/master/xsnippets-disk/syntaxhighlighter/js/shBrushXsp.js) for XPages source code.
- [shBrushJScript_custom.js](https://github.com/OpenNTF/XSnippets/blob/master/xsnippets-disk/syntaxhighlighter/js/shBrushJScript_custom.js) for Javascript
- [shBrushLscript.js](https://github.com/OpenNTF/XSnippets/blob/master/xsnippets-disk/syntaxhighlighter/js/shBrushLscript.js) for Lotusscript
- [shBrushCss_custom.js](https://github.com/OpenNTF/XSnippets/blob/master/xsnippets-disk/syntaxhighlighter/js/shBrushCss_custom.js) for CSS (some corrections needed here)
- [shBrushJava_custom.js](https://github.com/OpenNTF/XSnippets/blob/master/xsnippets-disk/syntaxhighlighter/js/shBrushJava_custom.js) for Java (not very good)

You will also need **shCore.css** , **shCore.js** , **shAutoloader.js** and other brush files you need from the Syntax Highlighter package.

I'm open to any questions about this. Use responsibly and don't forget to give some credit to Alex Gorbatchev...
