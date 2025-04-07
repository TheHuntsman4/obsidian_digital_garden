---
{"dg-publish":true,"permalink":"/garden/how-the-f-do-activation-functions-work/"}
---

07.04.25
# How the f*** do activation functions work

So you’ve built a neural net. It’s got layers. It's got weights. Maybe it even learns stuff. But somewhere along the line, someone told you:

> _“Don't forget the activation function.”_

And you were like,  
**“Okay???”**  
But deep down, you were thinking:  
**“How the f*** do activation functions actually work?”**

Let’s break it down.

---

### What are activation functions anyway?

In very blunt terms:  
Activation functions are little mathematical bouncers at the club called "Your Neural Net." Every time a neuron computes its weighted sum, the activation function decides whether that neuron gets to party (i.e. "activate") or not.

More formally:  
They take the output of a neuron and squash/transform it into a form that's usable by the next layer. That’s it.

---

### But wait, why not just use raw values?

Because that would turn your entire network into one big linear thing.  
Like this:

```python
output = W3*(W2*(W1*x))
```

Chaining linear operations just gives... another linear operation. No matter how many layers you have, the net result is still just:

```python
y = A*x + B
```

**No curves, no bends, no interesting decision boundaries. Just a boring ol' line.**

Activation functions bring in the non-linearity. They give your network the ability to learn _actual complex stuff_ — like images, speech, or the fact that sometimes you gotta press snooze twice.

---

### The Usual Suspects

Here’s the lineup:

#### 1. **ReLU (Rectified Linear Unit)**

```python
f(x) = max(0, x)
```

Most popular kid in the class. Easy to compute, works well, doesn’t saturate in the positive direction.

But it _dies_ sometimes (literally called **dying ReLU**), where neurons get stuck outputting zero forever.

#### 2. **Sigmoid**

```python
f(x) = 1 / (1 + exp(-x))
```

S-shaped, squashes everything between 0 and 1. Used to be hot, now mostly retired except in output layers (e.g. for binary classification).  
Also: suffers from **vanishing gradients**, which means it learns _really_ slow when inputs go too far from zero.

#### 3. **Tanh**

```python
f(x) = (exp(x) - exp(-x)) / (exp(x) + exp(-x))
```

Same squiggly vibe as sigmoid, but ranges from -1 to 1. Centered at 0, so it’s slightly better behaved.  
Still... not immune to vanishing gradients either.

#### 4. **Leaky ReLU**

```python
f(x) = x if x > 0 else 0.01*x
```

ReLU but with a safety net. Even negative values can pass through a _little_.  
Helps with the dying neuron problem.

#### 5. **Softmax**

Used in the **output layer** of multi-class classification networks. Turns raw scores into probabilities.

---

### Okay but what do they _do_ in code?

Let’s take an example. Here's a fake-ass “neuron”:

```python
def neuron(x, w, b):
    return x*w + b
```

Let's say we add ReLU:

```python
def relu(x):
    return max(0, x)

def neuron_with_relu(x, w, b):
    z = x*w + b
    return relu(z)
```

You can do this with any function. The key idea:

> The activation function modifies the output of a layer **before** passing it to the next.

---

### Visual intuition (aka look at dem curves)

Here’s how a few look:

- **ReLU**: flat for x < 0, then a 45-degree slope after.
    
- **Sigmoid**: smooth S-curve between 0 and 1.
    
- **Tanh**: same as sigmoid, just stretched and centered.
    
- **Leaky ReLU**: ReLU but the left side isn’t totally dead.
    

These curves define how gradients flow back during backprop. So choosing the wrong one can totally ruin learning.

---

### TL;DR

- Activation functions make your neural net _not just a glorified linear regression_.
    
- They inject non-linearity so that your model can learn **interesting** patterns.
    
- Pick the right one depending on what you’re doing:
    
    - Hidden layers? ReLU / Leaky ReLU.
        
    - Output for binary? Sigmoid.
        
    - Output for multi-class? Softmax.
        

Without them, your net would just be... well, **dumb**.

---

Still confused? Honestly, just try plotting them and see how they bend the output space. It’s way cooler to _see_ what they do than to read about it.

If you're using PyTorch or NumPy, try this:

```python
import numpy as np
import matplotlib.pyplot as plt

x = np.linspace(-10, 10, 100)

relu = np.maximum(0, x)
sigmoid = 1 / (1 + np.exp(-x))
tanh = np.tanh(x)
leaky_relu = np.where(x > 0, x, 0.01 * x)

plt.plot(x, relu, label='ReLU')
plt.plot(x, sigmoid, label='Sigmoid')
plt.plot(x, tanh, label='Tanh')
plt.plot(x, leaky_relu, label='Leaky ReLU')
plt.legend()
plt.grid(True)
plt.title("Activation Functions")
plt.show()
```

Go ahead. Run it. See the curves.

---

Moral of the story:  
**Neural nets without activation functions are like jokes without punchlines.**  
They technically exist, but don’t really do anything interesting.



