---
{"dg-publish":true,"permalink":"/garden/chapter-2-building-micrograd/"}
---

05.04.25

### The Value Object
### The basic version
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
This is the Value object, which is going to be  keeping track of all the variables that we are going to be using to build and interact using micrograd.
Another interesting thing that I would like to point out regarding this is that I am surprised how much easy the python interpreter makes life for your. Just defining an add and mul methods lets me use the + and * symbols without any hassle.

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
So internally python is actually doing this: `(a.__mul__(b)).__add__(c)`.  which imo is super cool.

### Complex version
```python
class Value:
    def __init__(self, data, _children=(), _op=''):
        self.data = data
        self._prev = set(_children)
        self._op = _op

    def __repr__(self):
        return f"Value(data={self.data})"
    
    def __add__(self, other):
        out = Value(self.data + other.data, (self, other), '+')
        return out
    
    def __mul__(self, other):
        out = Value(self.data*other.data, (self, other), '*')
        return out
```

Now the Value object has the `_children` and `_op` arguments. The `_children` argument keeps track of all the variables and previous values that were used to come to the final result. The `_op` argument keeps track of the last operation applied to the value. So now when we perform any complex math-y thing, we will have a full history on where the value came from and how the value came to be.

Now the question is why we would need to keep track of the any of this in the first place. Well as the name suggests, we will eventually have to figure out the gradients, for which we will need to get the derivative for each step w.r.t to each participating variable.

The current Value object is thus able to do a forward pass properly, the thing left to do is back propagation.

### Visualizing a forward pass
For the visualization, I will be using the same code from the video
![Pasted image 20250406223025.png](/img/user/images/Pasted%20image%2020250406223025.png)


