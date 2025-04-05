---
{"dg-publish":true,"permalink":"/garden/some-random-java-knowledge/"}
---

05.04.25

# Difference between shallow and deep copies of an object

## The technical definition of the terms
In a **shallow copy**, a copy of the original object is stored and only the reference address is finally copies
In a **deep copy**, a copy of the original object, as well as the repetitive copies both are stored.

## The actual understandable definition
Both of them, as their names suggests makes copies of the object. The key difference is made how it handles internal states and references.
Let's take an example of a book that is being shared between me and you, and we have a bookmark in the book as well. When a **shallow copy** is made, we each get a copy of the book, but we **share** the bookmark. So lets say I read till page 50, when you open your copy, the bookmark would be at page 50 as well. But when a **deep copy** is made, we each get a copy of the book, **AND** separate bookmarks. SO if i were to read the book to page 50, your bookmark would still be at page 0.

So when do we use each of these?
- Use a shallow copy when the object only has simple values like **numbers or strings**.
- Use a deep copy when the object also **contains list, maps, or other objects** within them.

