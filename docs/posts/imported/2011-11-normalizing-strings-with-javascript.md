---
authors:
  - serdar

title: "Normalizing strings with JavaScript"

slug: normalizing-strings-with-javascript

categories:
  - Tips & Tricks

date: 2011-11-14T20:50:42+02:00

tags:
  - domino-dev
  - javascript
  - xpages
---

If you have worked on Domino blog template, you may know the issue. You have a descriptive text like title for a content. You want to convert it to a different string to be used in a URL or file name.
<!-- more -->
DominoBlog template is using a simple formula for this:

```
@ReplaceSubstring(@ReplaceSubstring(@ReplaceSubstring(pagename;"?":"&":"@":"£":"$":"%":"^":"<":">":"*":"/":"'":"#":"~":"(":")":"+":"=":"!":";":"\"":":":",":"|":"\\";"");"--":" - ":" ";"-");" - ":"--":"---";"-")
```

This simple formula works in many times if you are living in USA :)

What missing is normalization. I mean, if your title contains accented characters (like Ş, İ, é, ã, etc.) they have to be normalized somehow.

Java has a great feature for this. The following example clears illegal entries and accented characters from the string and return perfectly normalized string.

Of course, if it's Java, it would not handle the famous little dotless i (ı) character, so we are hacking that :)

Do not underestimate this dotless i problem, it would be [too dangerous](http://gizmodo.com/382026/a-cellphones-missing-dot-kills-two-people-puts-three-more-in-jail)!!!

```js
function convert2Uri(name) {
 var result:String=name.toLowerCase();
 var pattern:java.util.regex.Pattern;
 
 result=@ReplaceSubstring(result,
   ["?","&","@","£","$","%","^","<",">","*","/","'","#","~","(",")","+","=","!",";","\"",":",",","|","\\","{","}","[","]"],
   [""]
 );

 result=@ReplaceSubstring(result,
   ["--"," - "," "],
   ["-"]
 );
 
 result=@ReplaceSubstring(result,
   [" - ","--","---"],
   ["-"]
 );

 result=java.text.Normalizer.normalize(result, java.text.Normalizer.Form.NFKD);
 
 pattern = java.util.regex.Pattern.compile("\\p{InCombiningDiacriticalMarks}+");
 result=pattern.matcher(result).replaceAll("");
 
 // dotless i cannot be normalized by Java!:
 pattern = java.util.regex.Pattern.compile("\\u0131");
 result=pattern.matcher(result).replaceAll("i");
 
 return result;
}"
```
