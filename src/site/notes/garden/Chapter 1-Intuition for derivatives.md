---
{"dg-publish":true,"permalink":"/garden/chapter-1-intuition-for-derivatives/"}
---

05.04.25

# The Derivative

## Slopes of a function
Lets  consider a simple function like this 
```python
def f(x):

return 3*x**2-4*x+5

xs = np.arange(-5,5,0.25)
ys = f(xs)
plt.plot(xs,ys)
plt.show()
```
The output of which looks like this:
![Pasted image 20250405004232.png](/img/user/images/Pasted%20image%2020250405004232.png)

## Derivative of a single input function

**What is the meaning of the derivative?**
A derivative essentially measure the sensitivity of a function to a small change in its input.

Let's illustrate it with an example
```python
h = 0.001
x = 3.0
f(x+h)
```
![Pasted image 20250405004412.png](/img/user/images/Pasted%20image%2020250405004412.png)
In the example given above, for a small change h = 0.001 in the input of 3.0, how would the function values change? It should be slightly greater as indicated by the graph, which shows a steeper slope. the function is going up from the x value 2.

**What changes when the input becomes neagtive? how do the magnitude and the direction of the slope change?**

From the graph, x values in the negative direction have greater values, hence the magnitude is larger, but in that region, the slope is decreasing, hence the derivative is -ve and higher
```python
h = 0.001
x = - 3.0
print(f(x+h))

slope = (f(x+h)-f(x))/h
slope
```
![Pasted image 20250405004522.png](/img/user/images/Pasted%20image%2020250405004522.png)

## Derivatives of function with multiple inputs
This will be our set of parameters
```python
a = 2.0
b = -3.0
c = 10.0
  
d = a*b+c #this is our function
print(d)
```
### Applying a small change the value of a
```python
h = 0.0001
a = 2.0
b = -3.0
c = 10.0

d1 = a*b+c
a+=h
d2 = a*b+c

  

print(f"d1: {d1}")
print(f"d2: {d2}")-
print(f"slope: {(d2-d1)/h}")
```
![Pasted image 20250405004941.png](/img/user/images/Pasted%20image%2020250405004941.png)
**What happened here?**
When the change to a was made, the derivatives w.rt a would mean that the value of b, would naturally dominate; since d(ab+c)/da = b. So as the value of b is -3, the value of the slope that we got is also close to -3.

### Applying a small change to b
```python
h = 0.0001
a = 2.0
b = -3.0
c = 10.0

d1 = a*b+c
b+=h
d2 = a*b+c

  

print(f"d1: {d1}")
print(f"d2: {d2}")-
print(f"slope: {(d2-d1)/h}")
```
![Pasted image 20250405005212.png](/img/user/images/Pasted%20image%2020250405005212.png)
**What happened here?**
Now in this case taking the derivative would get us a value of a, which is what our slope calculation got us, which is 2.

### Applying a small change to c
```python
h = 0.0001
a = 2.0
b = -3.0
c = 10.0

d1 = a*b+c
c+=h
d2 = a*b+c

  

print(f"d1: {d1}")
print(f"d2: {d2}")-
print(f"slope: {(d2-d1)/h}")
```
![Pasted image 20250405005335.png](/img/user/images/Pasted%20image%2020250405005335.png)
**What happened here?**
Now that we are differentiating w.r.t to c, as c is a contant the value of the expression would collapse down to 1, hence the answer of 0.999 which is *approx.* 1.