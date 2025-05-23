---
{"dg-publish":true,"permalink":"/garden/some-random-java-knowledge/"}
---

05.04.25
![7pp55ytrrpy31.webp](/img/user/images/7pp55ytrrpy31.webp)
courtesy of [r/ProgrammerHumor](https://www.reddit.com/r/ProgrammerHumor/comments/dwfk2x/memejava/)

I ran into a need to debug some Java code recently, and guess what? I HAVENT TOUCHED JAVA IN YEARS coz Python too OP. But all things said I think Im gonna end up learning a bunch of new stuff, so I'll be documenting everything that I learn here.
## Difference between shallow and deep copies of an object

### The technical definition of the terms
In a **shallow copy**, a copy of the original object is stored and only the reference address is finally copies
In a **deep copy**, a copy of the original object, as well as the repetitive copies both are stored.

### The actual understandable definition
Both of them, as their names suggests makes copies of the object. The key difference is based on how they handle internal states, references and values.
Let's take an example of a book that is being shared between me and you, and we have a bookmark in the book as well. When a **shallow copy** is made, we each get a copy of the book, but we **share** the bookmark. So lets say I read till page 50, when you open your copy, the bookmark would be at page 50 as well. But when a **deep copy** is made, we each get a copy of the book, **AND** separate bookmarks. SO if I were to read the book to page 50, your bookmark would still be at page 0.

So when do we use each of these?
- Use a shallow copy when the object only has simple values like **numbers or strings**.
- Use a deep copy when the object also **contains list, maps, or other objects** within them.

## Post and Pre-Incrementation

Java lets you write both `i++` and `++i` in a `for` loop. And if you're like me the first time you saw it, you probably went:

> _"Wait—do they not do the same thing?"_

Well, yes and no.

Here's a simple `for` loop:

```java
for (int i = 0; i < 5; i++) {
    System.out.println(i);
}
```

Now switch it to pre-increment:

```java
for (int i = 0; i < 5; ++i) {
    System.out.println(i);
}
```

**Output? Same.**  
So what's the catch?

The difference lies in **when** the value is incremented:

- `i++` → returns `i`, then increments.
    
- `++i` → increments `i`, then returns.
    

But in a `for` loop like this, the increment part isn't using the return value. It's just _"hey, increment i after this iteration"_ — so both behave the same.

Where they start to behave differently is inside expressions:

```java
int i = 0;
int a = i++;
int b = ++i;
System.out.println(a + " " + b);  // Output: 0 2
```

Here, `a` gets 0 (i before increment), and `b` gets 2 (i incremented _before_ assignment). Subtle, but important.


