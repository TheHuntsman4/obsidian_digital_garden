---
{"dg-publish":true,"permalink":"/garden/ll-ms/a-short-note-on-lo-ra/"}
---

25.04.25

## Introduction
I am currently working on an NLP project regarding agentic AI. I was looking at methods of [[garden/LLMs/Fine-Tuning\|Fine-Tuning]] LLMs, and that is when it hit me.I have been seeing the number of parameters on the latest LLMS and to say that they are getting massive, would be an understatement. We literally went from a billion params to a trillion in a very short time. So how do you load such a huge model with limited resources like me? That was the question that lead me to learn more about Low Rank Adaptation, or LoRA for short!

There are some more terms that we should be familiar with which will help a bit more, they are:
- Rank
- Precision
- Quantisation

### Rank
The rank of a matrix is the number of independent rows or columns in it. When a column is dependent, it just means that, that particular column can be expressed as a linear combination of the other columns. 
Example:
```
	M = [[1,2, 3]
		[1, 5, 6]
		[3, 4, 7]]
```
As you can see the last column, [3, 6, 7] can be expressed as the sum of the each number in the first two columns; [1, 1, 3]+[2,5,4] = [3,6,7]

Hence the rank of this particular matrix is 2, as there are two independent columns

### Precision
Precision in the most simplest sense is the number of values after the decimal point. The more numbers there are, the more precise that value.
### Quantisation
Quantisation is a method to lower the precision of a value from something like a 32 bit number to a 16 bit number while conserving the value and meaning behind the initial value as much as possible.

Now you may be thinking, why would you want to do Quantisation? Why lower the precision, more precision should be better right? The answer to that is, yes more precision is good, but keep this in mind, the more precise a number is, the more space it will take up, and when trying to fine tune a massive network, which is almost impossible to load up on a normal run of the mill machine, you'd like to have as much of your memory as possible. Hence Quantisation is used to change the number from lets say a **FloatingPoint32** datatype to a integer16 or INT16 value, while preserving the meaning of that value as much as possible.  

## How does LoRA work?

So now that we have an idea about what Rank, Precision, and Quantisation mean, let's get into the main thing — Low Rank Adaptation or LoRA.

When you fine-tune a large model normally, what you’re doing is tweaking the weights (basically the parameters) of the entire model. But when the model is huge, like GPT-3 or something, there’s no way you can store and update all of that on a normal machine. Fine-tuning that monster end-to-end is just not practical unless you have cloud GPUs for days.

This is where LoRA comes in.  
Instead of updating **all** the weights, LoRA freezes the original model and adds small trainable matrices _on top_ of it. Think of it like this: you're not painting over the whole building, you're just sticking tiny colorful stickers that change how the building looks a little bit — without touching the original paint.

And here’s where “Low Rank” kicks in. Those new matrices that LoRA adds aren’t full-sized ones — they’re much smaller, lower rank approximations. Remember how we talked about Rank earlier? Low rank means the matrices are a lot simpler — fewer independent rows and columns — so they take way less memory and compute to work with.

At training time, instead of adjusting billions of parameters, you're just learning a few thousand or million new ones through these small matrices. And at inference time (when you use the model), you can either:

- Merge these matrices back into the original weights, or
- Keep them separate and do a lightweight update on-the-fly.

Either way, it's super efficient compared to traditional fine-tuning.

**In short**:

- Freeze the original model.
- Add small trainable low-rank matrices.
- Fine-tune only these small parts.
- Save _huge_ amounts of memory, time, and compute.

LoRA basically cracked the code on making fine-tuning massive models accessible to people like you and me without needing some God-tier server farm.

