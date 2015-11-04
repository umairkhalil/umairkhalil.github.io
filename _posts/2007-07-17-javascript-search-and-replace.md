---
layout: post
title: Javascript search and replace
tags: [javascript]
---

Search and replace all instances of a substring within a string and return the result e.g. to replace all instances of 'word' in a string with 'new' use: resultString = replace(origString, 'word', 'new');

```javascript
function replace(str, original, replacement) {
   var result;
   result = "";
   while(str.indexOf(original) != -1) {
       if (str.indexOf(original) > 0) {
           result = result + str.substring(0, str.indexOf(original)) + replacement;
       }
       else {
           result = result + replacement;
       }
       str = str.substring(str.indexOf(original) + original.length, str.length);
   }
   return result + str;
}
```
