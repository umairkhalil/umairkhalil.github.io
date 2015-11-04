---
layout: post
title: DSL by mapping strings to methods
tags: [ruby,python]
---

A simple dsl can be implemented by mapping strings to methods.

DSL:

```
command1 parameter1
command2 parameter1 parameter2
command3
```

In ruby:

```ruby
input_file = "
command1 'parameter1'
command2 'parameter1', 'parameter2'
command3
"

class DSL
   def command1 (arg1)
    puts ["command1",arg1].join(' ')
   end

   def command2 (arg1, arg2)
    puts ["command2",arg1,arg2].join(' ')
   end

   def command3 ()
    puts "command3"
   end
end

DSL.new.instance_eval input_file
```

In python:

```python
input_file = \
""" command1 parameter1
    command2 parameter1 parameter2
    command3    """
# Using python 3
class DSL(object):
    def command1(*args):
        print('command1 {}'.format(str(args)[1:-1]))

    def command2(*args):
        print('command2 {}'.format(str(args)[1:-1]))

    def command3(*args):
        print('command3 {}'.format(str(args)[1:-1]))

lines = input_file.split('\n')
for line in lines:
    command = line.split()
    getattr(DSL, command[0])(*command[1:])
```
