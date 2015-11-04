---
layout: post
title: php mysql password hashing algorithm
tags: [php,mysql,algorithm]
---

Not the greatest password hashing algorithm but better than plain text. $nr and $nr2 can be initialized to different values.

```php
<?php
function mysql_password($passStr) {
       $nr=0x17654321;
       $nr2=0x12345671;
       $add=7;
       $charArr = preg_split("//", $passStr);
 
       foreach ($charArr as $char) {
               if (($char == '') || ($char == ' ') || ($char == '\t')) continue;
               $charVal = ord($char);
               $nr ^= ((($nr & 63) + $add) * $charVal) + ($nr << 8);
               $nr &= 0x7fffffff;
               $nr2 += ($nr2 << 8) ^ $nr;
               $nr2 &= 0x7fffffff;
               $add += $charVal;
       }
 
       return sprintf("%08x%08x", $nr, $nr2);
}
?>
```
