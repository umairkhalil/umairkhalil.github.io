---
layout: post
title: Repeat string multiple times
tags: [python,ruby,java,clojure]
---

Sometimes you need to repeat a string multiple times, say for printing a dashed line to the console or log. Here's how to do it without typing dash multiple times.

In Python:

```python
print("-"*80)
```

In Ruby:

```ruby
puts "-"*80
```

In Java:

```java
System.out.println(String.format("%0" + 80 + "d", 0).replace("0", "-"));
System.out.println(new String(new char[80]).replace("\0", "-"));
```

In Clojure:

```clojure
(prn (apply str (repeat 80 "-")))
```
