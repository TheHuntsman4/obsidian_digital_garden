---
{"dg-publish":true,"permalink":"/garden/ll-ms/fine-tuning/"}
---

25.04.25
## What is Fine-Tuning?

Fine-tuning is one of those terms that gets thrown around a lot in AI circles, and for good reason — it’s kind of a cheat code. Imagine you have a model that’s already been trained on a massive dataset (like billions of sentences or thousands of images). Now, instead of starting from scratch, you _nudge_ this model in a new direction by training it on a smaller, specific dataset. That process is called **fine-tuning**.

Let’s take an example. Say you have a vision model trained to recognize animals in general. But now you want it to specifically identify different species of snakes. Training from scratch? Not ideal — you’d need a ton of data and compute. Fine-tuning? Way faster, way more efficient. You just train the existing model a bit more using snake-specific images, and boom — it starts acting like a herpetologist (I may have taken this example to flex that I learnt this word).

Same deal with LLMs. They’re trained on everything from Wikipedia to Reddit. But if you want one that talks like a lawyer or understands biomedical text, fine-tuning helps you get there without breaking the bank (or your GPU).

So yeah, in simple terms — **fine-tuning is how you teach an AI model to specialize**. Like giving a generalist a crash course in your niche.
