---
layout: post
title: DSL by mapping strings to methods
tags: [ruby,python,java,c-sharp]
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

In java:

```java

import java.util.Arrays;
import java.util.HashMap;
import java.util.Map;

public class Main {
    public interface Command {
        void execute(String[] parameters);
    }

    public static void main(String[] args) {
        final String input_file_read =
                "command1 parameter1\n" +
                "command2 parameter1 parameter2\n" +
                "command3";

        // Using java 8 for lambdas, otherwise use interface and method name
        Map<String, Command> DSL = new HashMap<String, Command>() { {
            put("command1", (String[] parameters) -> {
                    System.out.println(Arrays.toString(parameters));
                }
            );
            put("command2", (String[] parameters) -> {
                    System.out.println(Arrays.toString(parameters));
                }
            );
            put("command3", (String[] parameters) -> {
                    System.out.println(Arrays.toString(parameters));
                }
            );
        } };

        String[] lines = input_file_read.split("\n");
        for (String line : lines) {
            String[] tokens = line.split(" ");
            if (DSL.containsKey(tokens[0])) {
                DSL.get(tokens[0]).execute(tokens);
            } else {
                System.out.println(tokens[0] + " command not found");
            }
        }
    }
}
```

In C#:

```csharp

using System;
using System.Collections.Generic;

namespace dsl
{
    class Program
    {
        public static void Main(string[] args)
        {
            const String input_file_read = 
@"command1 parameter1
command2 parameter1 parameter2
command3";

            // Using C# 3.0 for lambdas, requires .Net 3.5 and greater
            var DSL = new Dictionary<String, Action<String[]>>() {
              { "command1", (parameters) => {
                  Console.WriteLine(String.Join(" ", parameters));
                } },
                { "command2", (parameters) => {
                  Console.WriteLine(String.Join(" ", parameters));
                } },
                { "command3", (parameters) => {
                  Console.WriteLine(String.Join(" ", parameters));
                } },
            };
            
            String[] lines = input_file_read.Split('\n');
            foreach (String line in lines) {
                String[] tokens = line.Split(' ');
                if (DSL.ContainsKey(tokens[0])) {
                    DSL[tokens[0]](tokens);
                } else {
                    Console.WriteLine(tokens[0] + " command not found");
                }
            }
        }
    }
}
```
