---
{"dg-publish":true,"permalink":"/garden/chapter-2-building-micrograd/"}
---

05.04.25

### The basic Value Object
```python
class Value:
    def __init__(self, data):
        self.data = data

    def __repr__(self):
        return f"Value(data={self.data})"
    
    def __add__(self, other):
        out = Value(self.data + other.data)
        return out
    
    def __mul__(self, other):
        out = Value(self.data * other.data)
        return out
```
This the basic form of the Value object, this is what will be keeping track of all the variables that we are going to be using to build micrograd.
Another interesting thing that I would point out regarding this is that I surprised how much the python interpreter actually does for you. Just defining an add and mul methods lets me use the + and * symbols without any hassle.

Now when i do something like this:
```python
a = Value(2.0)
b = Value(-3.0)
c = Value(10.0)

d = a*b+c
print(d)
```
This is the output that I get
![Pasted image 20250406214238.png](/img/user/images/Pasted%20image%2020250406214238.png)
So internally python is actually doing this: `(a.__mul__(b).__add__(c)`.  which imo is super cool.


