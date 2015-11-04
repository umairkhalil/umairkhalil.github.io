---
layout: post
title: Accumulator generator
tags: [python,clojure]
---

Paul Graham's accumulator generator. foo generates an accumulator function and returns it.

In python:

```python
class foo:
    def __init__(self, n):
        self.n = n
    def __call__(self, i):
        self.n += i
        return self.n

x = foo(1) # 1
x(5) # 1+5
foo(3)
print (x(2.3)) # 1+5+2.3
```

In clojure:

```clojure
(defn foo [n]
  (let [acc (atom n)]
    (fn [i] (swap! acc + i))))

(def x (foo 1)) ; 1
(x 5) ; 1+5
(println (x 2.3)) ; 1+5+2.3
```
