---
layout: post
title: php read and print csv file
tags: [php,csv]
---

Read and print the entire contents of a CSV file

```php
<?php
$row = 1;
$handle = fopen("test.csv", "r");
while (($data = fgetcsv($handle, 1000, ",")) !== FALSE) {
   $num = count($data);
   echo "<p> $num fields in line $row: <br /></p>\n";
   $row++;
   for ($c=0; $c < $num; $c++) {
       echo $data[$c] . "<br />\n";
   }
}
fclose($handle);
?>
```
