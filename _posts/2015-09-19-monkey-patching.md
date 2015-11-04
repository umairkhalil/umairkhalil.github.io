---
layout: post
title: Monkey patching
tags: [ruby,python]
---

Monkey patching refers to dynamic modification of a class at runtime. It's not a good practice. Here's what it looks like in code.

In ruby:

```ruby
# Adding a method to an instance of a class

a = "monkey"

def a.scramble
    tr "a-z", "b-za"
end

puts a.scramble
```

In python:

```python
import types
# Adding a method to an instance of a class

# builtin types cannot be modified so creating a subclass
class ModifiableString(str):
    def __new__(cls, *args, **kw):
        return str.__new__(cls, *args, **kw)

a = ModifiableString("monkey")

def scramble(self):
    return self.translate(str.maketrans('abcdefghijklmnopqrstuvwxyz', 'bcdefghijklmnopqrstuvwxyza'))

a.scramble = types.MethodType(scramble, a)

print (a.scramble())
```
