---
title: Notes on the complex roots of unity
link: https://eli.thegreenplace.net/2026/notes-on-the-complex-roots-of-unity/
source: eli-thegreenplace-net
published: 2026-10-18T01:18:00Z
updated: 2026-10-18T08:18:06.134012Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- Eli Bendersky
labels:
- math
- misc
summary: 'One of the most wonderful aspects of math is how deep we can get by studying seemingly trivial questions. Consider the roots of unity, where "unity" simply means the number 1. Basics: roots of unity for real numbers Studying the roots of unity usually begins in middle school: what is …'
content: extracted
html: 2026-10-18-notes-on-the-complex-roots-of-unity.html
remote_preview:
  url: https://eli.thegreenplace.net/images/logosmall.png
extra:
  content:interactive: true
---

One of the most wonderful aspects of math is how deep we can get by studying seemingly trivial questions. Consider the roots of unity, where "unity" simply means the number 1.

## Basics: roots of unity for real numbers

Studying the roots of unity usually begins in middle school: what is the square root of 1? Equivalently, what are the solutions to this equation?

Well, there are two answers: 1 and -1.

Similarly, we can ask the question for higher roots; the third roots are solutions to the equation:

And so on. When we’re only dealing with real numbers, that’s pretty much the whole story. For odd-degree roots of 1, the only answer is 1; for even-degree roots of 1, the answer is either 1 or -1.

When we extend our domain to the complex numbers, things become much more interesting.

## Basics: roots of unity for complex numbers

By the [formal definition](https://en.wikipedia.org/wiki/Root_of_unity), a root of unity is any complex number that yields 1 when raised to a positive integer power .

Rather than looking at *all* the possible roots of 1, it’s often more useful to discuss the roots for a specific given : complex numbers such that .

For example, let’s consider . We’ve already seen that the roots are 1 and -1; these are complex numbers with a zero imaginary part.

How about ? There are four roots: 1, -1, and . For , the three roots [\[1\]](https://eli.thegreenplace.net/2026/notes-on-the-complex-roots-of-unity/#footnote-1) are 1, and . It’s easy enough to check that these indeed give 1 when raised to the third power, but how do we calculate them in the first place?

## Calculating complex roots using polar coordinates

A complex number with magnitude 1 can be represented in polar coordinates [\[2\]](https://eli.thegreenplace.net/2026/notes-on-the-complex-roots-of-unity/#footnote-2) as . If we raise to the -th power, we get:

The magnitude of is still 1, which is essential if we want to be a -th root of unity. Let’s equate to 1:

To find the solutions to this equation, we’ll first want to represent 1 itself in polar coordinates. Let’s use another variable for the angle:

What is the value of ? Thinking about the unit circle, is any integer multiple of - which we’ll express as for :

Back to :

Or:

So the -th roots of unity are [\[3\]](https://eli.thegreenplace.net/2026/notes-on-the-complex-roots-of-unity/#footnote-3):

## Some basic examples

Let’s begin with . We have the following roots:

For :

As expected. It should be similarly simple to calculate the roots for ; give it a try.

## Roots of unity and rotations on the unit circle

What happens when we multiply some root of unity by itself? Since the magnitude of the root is 1, multiplying by it is akin to a *rotation* along the unit circle by the root’s angle. For example, consider from the 3rd roots of unity:

Multiply by itself:

If the angle of is , the angle of is . Let’s multiply by once more:

The result is 1, as expected (after all, is a 3rd root of unity).

This visualization shows how roots of unity appear on the unit circle and how raising them to powers rotates the roots until finally :

The nth roots of unity, equally spaced around the unit circle. Use the root selector below.

Roots *n* Root *k*

The powers of the selected root. Use the power slider below.

Power *m*

Select first; this shows the -th roots as blue dots on the unit circle. Now you can click any of the roots, and see its powers in the lower plot - drag the slidebar to see the rotations. If you spend some time playing with this, you’ll notice that some roots - for example when - go back to 1 when raised to a smaller power than . We’ll revisit this outcome when we discuss the group-theoretic aspects of roots of unity.

## Geometry: roots of unity and regular polygons

As we’ve seen, the roots of unity are evenly spaced along the unit circle on the complex plane. If we connect the roots with straight lines to their immediate neighbors, we’ll notice something interesting:

Adjacent nth roots of unity joined to form a regular polygon on the unit circle.

Roots *n*

For different values of , we get the [Regular Polygons](https://en.wikipedia.org/wiki/Regular_polygon)! It’s pretty easy to prove this if we connect each root with a straight line to the origin. The triangles we get are congruent, because the sides from the origin to the roots are all of length 1, and the central angles are all . Therefore, both the sides of the polygons and their angles are identical, which makes them regular.

## Conjugates of roots of unity

Suppose we have the root of unity:

For some and . If we take the reciprocal of :

Then:

So is also an n-th root of unity. While the powers of are rotating counter-clockwise on the unit circle until they land on 1 for the -th power, the powers of similarly rotate *clockwise*.

also happens to be the complex conjugate of :

So if we have:

As the -th roots of unity, then:

Are also -th roots of unity. In fact, they are the same roots, because for any given positive :

So enumerating the conjugate roots is the same as enumerating the roots themselves in the opposite order.

## Group theory: the cyclic group of roots of unity

Let’s look at the set of (roots of unity) for some :

This set - along with the multiplication operation - forms a *group*. Let’s prove that.

**Closure:** Assume (so and ). Then:

Therefore .

**Identity:** The number , so it can serve as our identity element for the group.

**Associativity:** Taken directly from the associativity of the multiplication operation on complex numbers.

**Inverse:** We’ve just seen in the section on conjugates that is an -th root of unity when is. So the inverse exists and is part of the group.

Because the operation we have is just complex multiplication, we can trivially derive additional properties, like commutativity. Therefore, the group is *Abelian*.

Finally, we can show that the group is also *cyclic* and discuss its *generators*.

For slightly simpler notation, let’s denote . So the group can be represented as:

This makes it obvious that the group is cyclic - all its elements can be expressed as integer powers of . And is a generator for the group.

But interestingly, is not the only generator for this group. Any can be a generator, as long as and are co-prime. For example, consider the group :

Obviously, is a generator for this group. But isn’t! If we raise it to various powers we get:

Since , the powers of repeat after just three steps, so they only reach three out of six group elements.

But if and are co-prime, is a generator; in the case of , we can easily verify that and are the only generators. The roots that are generators for the group are also called the *primitive roots*.

Let’s take the -roots of unity (), denoted as above with:

What’s the sum of all these roots?

This is just the sum of a geometric series with the first term 1 and common ratio . Therefore:

If you look carefully at the visualization shown above - this makes sense geometrically. All the roots of unity are evenly spread around the unit circle, so viewing them as vectors, all components cancel out.

How about the sum of roots raised to some integer power ? Let’s try this for (assuming ):

Using the same geometric series sum formula (with ), we get:

In general, when is not divisible by , the sum is 0. But when *is* divisible by , each power becomes:

And of these sum to . In conclusion:

## Powers of roots of unity as an orthogonal basis

Let’s consider the powers of roots of unity as vectors. We’ll continue using the notation:

We’ll begin with the 0-th power vector - each of the roots raised to power 0:

Then power 1 - just the roots themselves:

Each root raised to power 2:

And so on, until power :

To go full linear algebra mode, we’ll collect these vectors into an matrix:

Let’s calculate the inner product of two matrix rows. Recall that when computing the inner product on vectors over , we conjugate the second element:

But hold on, we’ve just calculated this exact sum in the previous section. We get:

Since the vector indices and only run over , this is equivalent to:

Which is to say: the vectors and are mutually orthogonal when . Since we have a set of orthogonal vectors in , these vectors form a *basis* for - any vector in this space can be represented as a linear combination of . This fact has far reaching consequences in math and engineering, since it plays a critical part in forming the DFT (Discrete Fourier Transform), and is also known as the *Fourier matrix*. But this will be a topic for a separate post.

Another fun thing to notice before we’re finished here: the matrix happens to be *symmetric* (this is trivial to check by observation). Therefore, its columns are also mutually orthogonal.

* * *

[\[1\]](https://eli.thegreenplace.net/2026/notes-on-the-complex-roots-of-unity/#footnote-reference-1)

In general - counting multiplicity - there are exactly complex roots. This is because we’re essentially finding the solutions to the polynomial , which has complex solutions according to the Fundamental theorem of algebra.

[\[2\]](https://eli.thegreenplace.net/2026/notes-on-the-complex-roots-of-unity/#footnote-reference-2)

For some notes on converting between the different representations of complex numbers, see my post on [the Euler formula](https://eli.thegreenplace.net/2024/notes-on-the-euler-formula/).

[\[3\]](https://eli.thegreenplace.net/2026/notes-on-the-complex-roots-of-unity/#footnote-reference-3)

When , , which is the same as . On a unit circle, the roots start repeating once one full cycle is completed; therefore, there are only unique roots with .
