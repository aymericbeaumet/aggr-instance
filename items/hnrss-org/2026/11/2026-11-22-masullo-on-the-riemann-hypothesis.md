---
title: Masullo on the Riemann Hypothesis
link: https://johncarlosbaez.wordpress.com/2026/11/22/masullo-on-the-riemann-hypothesis/
source: hnrss-org
published: 2026-11-22T10:59:56Z
updated: 2026-11-22T10:59:56Z
first_seen: 2026-11-23T09:43:22.615423405Z
authors:
- xiaoyu2006
content: extracted
html: 2026-11-22-masullo-on-the-riemann-hypothesis.html
remote_preview:
  url: https://s0.wp.com/latex.php?latex=%5Cdisplaystyle%7B++%5Czeta%28s%29+%3D+%5Csum_%7Bn%3D1%7D%5E%5Cinfty+%5Cfrac%7B1%7D%7Bn%5Es%7D+%3D+%5Cprod_p+%5Cfrac%7B1%7D%7B1+-+p%5E%7B-s%7D%7D+%7D+&bg=ffffff&fg=333333&s=0&c=20201002&zoom=4.5
  alt: \displaystyle{ \zeta(s) = \sum_{n=1}^\infty \frac{1}{n^s} = \prod_p \frac{1}{1 - p^{-s}} }
---

On November 20, 2026, two attempts to prove the Riemann Hypothesis appeared. One, by Levent Alpöge working with Claude, exists mainly as 17 million lines of Lean code. The other, by Alessandro Masullo, is a 940-page human-written manuscript that Ravi Vakil posted on his blog:

• Ravi Vakil, [Hurried announcement: Alessandro Masullo GRH](https://math216.wordpress.com/2026/11/20/hurried-announcement-alessandro-masullo-grh/), *Math 216* blog, November 20, 2026.

• Alessandro Maria Masullo, [The Music of the Primes](https://sites.google.com/view/tmotp/home?authuser=0), draft, November 20, 2026.

While there’s been an enormous outpouring about the drama of this situation, so far it’s hard to find good simple accounts of Masullo’s paper. Let me try to fill this gap by sketching Masullo’s proof strategy in a very simplified way, along with some problems people have noted.

Since I am not an expert on number theory, all this is very tentative and I could be making serious mistakes. Please let me know if you see some!

If you also want a sketch of the Alpöge / Claude attempt, check out these:

• Michael Rubinstein, [The Alpöge-Claude proof of the Riemann hypothesis explained for the Riemann zeta function](https://www.math.uwaterloo.ca/~mrubinst/alpoge_claude_grh/zeta-paper.pdf).

• Jimmie Adriazola, [An end-to-end reconstruction of the Alpöge–Claude route to the Riemann hypothesis](https://github.com/jimjamzola/RH-Alpoge-Reconstruction).

### Zeros as poles

Okay—here we go! The Riemann zeta function is

![\displaystyle{  \zeta(s) = \sum_{n=1}^\infty \frac{1}{n^s} = \prod_p \frac{1}{1 - p^{-s}} }](https://s0.wp.com/latex.php?latex=%5Cdisplaystyle%7B++%5Czeta%28s%29+%3D+%5Csum_%7Bn%3D1%7D%5E%5Cinfty+%5Cfrac%7B1%7D%7Bn%5Es%7D+%3D+%5Cprod_p+%5Cfrac%7B1%7D%7B1+-+p%5E%7B-s%7D%7D+%7D+&bg=ffffff&fg=333333&s=0&c=20201002&zoom=4.5)

for ![\mathrm{Re}(s) > 1,](https://s0.wp.com/latex.php?latex=%5Cmathrm%7BRe%7D%28s%29+%3E+1%2C&bg=ffffff&fg=333333&s=0&c=20201002&zoom=4.5) and it extends to a meromorphic function on the whole complex plane. The Riemann Hypothesis says that all its ‘nontrivial’ zeros have real part ![1/2.](https://s0.wp.com/latex.php?latex=1%2F2.&bg=ffffff&fg=333333&s=0&c=20201002&zoom=4.5)

Masullo’s argument works with the zeta function’s ‘logarithmic derivative’, ![\zeta'/\zeta.](https://s0.wp.com/latex.php?latex=%5Czeta%27%2F%5Czeta.&bg=ffffff&fg=333333&s=0&c=20201002&zoom=4.5) Taking the log of the product formula and differentiating gives

![\displaystyle{  -\frac{\zeta'(s)}{\zeta(s)} = \sum_{n=1}^\infty \frac{\Lambda(n)}{n^s} }](https://s0.wp.com/latex.php?latex=%5Cdisplaystyle%7B++-%5Cfrac%7B%5Czeta%27%28s%29%7D%7B%5Czeta%28s%29%7D+%3D+%5Csum_%7Bn%3D1%7D%5E%5Cinfty+%5Cfrac%7B%5CLambda%28n%29%7D%7Bn%5Es%7D+%7D+&bg=ffffff&fg=333333&s=0&c=20201002&zoom=4.5)

where ![\Lambda(n) = \log p](https://s0.wp.com/latex.php?latex=%5CLambda%28n%29+%3D+%5Clog+p&bg=ffffff&fg=333333&s=0&c=20201002&zoom=4.5) if ![n](https://s0.wp.com/latex.php?latex=n&bg=ffffff&fg=333333&s=0&c=20201002&zoom=4.5) is a power of a prime ![p,](https://s0.wp.com/latex.php?latex=p%2C&bg=ffffff&fg=333333&s=0&c=20201002&zoom=4.5) and ![0](https://s0.wp.com/latex.php?latex=0&bg=ffffff&fg=333333&s=0&c=20201002&zoom=4.5) otherwise. So the right side can be written as a sum over primes. On the other hand, a function’s logarithmic derivative has a simple pole at each zero of the function. Roughly speaking,

![\displaystyle{  -\frac{\zeta'(s)}{\zeta(s)} = \frac{1}{s-1} - \sum_\rho \frac{1}{s - \rho} + \text{(boring terms)} , }](https://s0.wp.com/latex.php?latex=%5Cdisplaystyle%7B++-%5Cfrac%7B%5Czeta%27%28s%29%7D%7B%5Czeta%28s%29%7D+%3D+%5Cfrac%7B1%7D%7Bs-1%7D+-+%5Csum_%5Crho+%5Cfrac%7B1%7D%7Bs+-+%5Crho%7D+%2B+%5Ctext%7B%28boring+terms%29%7D+%2C+%7D+&bg=ffffff&fg=333333&s=0&c=20201002&zoom=4.5)

where ![\rho](https://s0.wp.com/latex.php?latex=%5Crho&bg=ffffff&fg=333333&s=0&c=20201002&zoom=4.5) ranges over the zeros of the Riemann zeta function. This second formula is a sum over zeros.

Setting these two expressions equal gives what number theorists call an ‘explicit formula’: an exact identity linking primes and zeros of the zeta function.

This is what Masullo and others mean by “the music of the primes”: each zero contributes one oscillating wave to the distribution of primes. You can see my animations of this here:

• John Baez, [Riemann’s explicit formulas](https://math.ucr.edu/home/baez/mathematical/riemann_explicit_formula/explicit_formulas.html).

In these terms, the Riemann Hypothesis says:

> ![\zeta'/\zeta](https://s0.wp.com/latex.php?latex=%5Czeta%27%2F%5Czeta&bg=ffffff&fg=333333&s=0&c=20201002&zoom=4.5) has no poles with real part greater than ![1/2,](https://s0.wp.com/latex.php?latex=1%2F2%2C&bg=ffffff&fg=333333&s=0&c=20201002&zoom=4.5) except the one at ![s = 1.](https://s0.wp.com/latex.php?latex=s+%3D+1.&bg=ffffff&fg=333333&s=0&c=20201002&zoom=4.5)

Masullo’s strategy for proving this is to find a new formula for the logarithmic derivative of the Riemann zeta function, one that visibly has no poles to the left of the line ![\mathrm{Re}(s) = 1/2.](https://s0.wp.com/latex.php?latex=%5Cmathrm%7BRe%7D%28s%29+%3D+1%2F2.&bg=ffffff&fg=333333&s=0&c=20201002&zoom=4.5) It’s called a ‘trace formula’.

### The generalized Riemann hypothesis

But wait! Neither Alpöge and Masullo are content to prove the Riemann hypothesis: they want to prove the *generalized* Riemann Hypothesis. For ordinary integers, this concerns the functions

![\displaystyle{  L(s,\chi) = \sum_{n=1}^\infty \frac{\chi(n)}{n^s} }](https://s0.wp.com/latex.php?latex=%5Cdisplaystyle%7B++L%28s%2C%5Cchi%29+%3D+%5Csum_%7Bn%3D1%7D%5E%5Cinfty+%5Cfrac%7B%5Cchi%28n%29%7D%7Bn%5Es%7D+%7D+&bg=ffffff&fg=333333&s=0&c=20201002&zoom=4.5)

where ![\chi](https://s0.wp.com/latex.php?latex=%5Cchi&bg=ffffff&fg=333333&s=0&c=20201002&zoom=4.5) is a ‘Dirichlet character’. This is a function on the natural numbers that is periodic with some period ![N,](https://s0.wp.com/latex.php?latex=N%2C&bg=ffffff&fg=333333&s=0&c=20201002&zoom=4.5) obeys

![\chi(mn) = \chi(m)\chi(n)](https://s0.wp.com/latex.php?latex=%5Cchi%28mn%29+%3D+%5Cchi%28m%29%5Cchi%28n%29&bg=ffffff&fg=333333&s=0&c=20201002&zoom=4.5)

for all ![m, n,](https://s0.wp.com/latex.php?latex=m%2C+n%2C&bg=ffffff&fg=333333&s=0&c=20201002&zoom=4.5) vanishes on numbers that share a factor with ![N,](https://s0.wp.com/latex.php?latex=N%2C&bg=ffffff&fg=333333&s=0&c=20201002&zoom=4.5) and has its other values being roots of unity. The Riemann zeta function is the simplest case: here ![\chi(n) = 1](https://s0.wp.com/latex.php?latex=%5Cchi%28n%29+%3D+1&bg=ffffff&fg=333333&s=0&c=20201002&zoom=4.5) for all ![n.](https://s0.wp.com/latex.php?latex=n.&bg=ffffff&fg=333333&s=0&c=20201002&zoom=4.5)

One version of the generalized Riemann Hypothesis says:

> every zero of ![L(s,\chi)](https://s0.wp.com/latex.php?latex=L%28s%2C%5Cchi%29&bg=ffffff&fg=333333&s=0&c=20201002&zoom=4.5) with ![0 < \mathrm{Re}(s) < 1](https://s0.wp.com/latex.php?latex=0+%3C+%5Cmathrm%7BRe%7D%28s%29+%3C+1&bg=ffffff&fg=333333&s=0&c=20201002&zoom=4.5) lies on the line ![\mathrm{Re}(s) = 1/2.](https://s0.wp.com/latex.php?latex=%5Cmathrm%7BRe%7D%28s%29+%3D+1%2F2.&bg=ffffff&fg=333333&s=0&c=20201002&zoom=4.5)

This implies many results about primes in arithmetic progressions. For example, Hardy and Littlewood showed it implies that primes equal to 3 mod 4 are more common than primes 1 mod 4. They also showed it implies every odd number > 5 is a sum of three primes! (Helfgott found an alternative, more complicated proof in 2013.)

But Masullo also wants more: he wants to prove the analogous statement for L-functions of general [Hecke characters](https://en.wikipedia.org/wiki/Hecke_character). These involve a choice of [algebraic number field](https://en.wikipedia.org/wiki/Algebraic_number_field) ![F](https://s0.wp.com/latex.php?latex=F&bg=ffffff&fg=333333&s=0&c=20201002&zoom=4.5), which gives a [ring of integers](https://en.wikipedia.org/wiki/Algebraic_number_field#Algebraicity,_and_ring_of_integers) generalizing the usual integers. This further generalization implies a [huge number of results](https://en.wikipedia.org/wiki/Generalized_Riemann_hypothesis#Consequences_of_GRH) in number theory, computational complexity, and so on.

Okay—now back to trace formulas.

### Trace formulas

To prove that a Hecke ![L](https://s0.wp.com/latex.php?latex=L&bg=ffffff&fg=333333&s=0&c=20201002&zoom=4.5)-function has no nontrivial zeros except on the line ![\mathrm{Re}(s) = 1/2,](https://s0.wp.com/latex.php?latex=%5Cmathrm%7BRe%7D%28s%29+%3D+1%2F2%2C&bg=ffffff&fg=333333&s=0&c=20201002&zoom=4.5) Masullo comes up with a new formula for its logarithm ![L'/L.](https://s0.wp.com/latex.php?latex=L%27%2FL.&bg=ffffff&fg=333333&s=0&c=20201002&zoom=4.5) He claims it makes sense without poles all the way to the line ![\mathrm{Re}(s) = 1/2.](https://s0.wp.com/latex.php?latex=%5Cmathrm%7BRe%7D%28s%29+%3D+1%2F2.&bg=ffffff&fg=333333&s=0&c=20201002&zoom=4.5) This formula is an example of a ‘trace formula’.

The simplest trace formula in the world is this: for a square matrix ![A](https://s0.wp.com/latex.php?latex=A&bg=ffffff&fg=333333&s=0&c=20201002&zoom=4.5),

![\displaystyle{  \sum_i \lambda_i = \sum_i A_{ii} . }](https://s0.wp.com/latex.php?latex=%5Cdisplaystyle%7B++%5Csum_i+%5Clambda_i+%3D+%5Csum_i+A_%7Bii%7D+.+%7D+&bg=ffffff&fg=333333&s=0&c=20201002&zoom=4.5)

The left side is the sum of the eigenvalues of ![A.](https://s0.wp.com/latex.php?latex=A.&bg=ffffff&fg=333333&s=0&c=20201002&zoom=4.5) This is the ‘spectral side’ of the trace formula. The right side is the sum of the diagonal entries, the ‘geometric side’. They are equal, even though they are computed in different ways.

For operators on infinite-dimensional spaces the same idea works, with care. The trace of an integral operator with kernel ![k(x,y)](https://s0.wp.com/latex.php?latex=k%28x%2Cy%29&bg=ffffff&fg=333333&s=0&c=20201002&zoom=4.5) is the integral of ![k(x,x)](https://s0.wp.com/latex.php?latex=k%28x%2Cx%29&bg=ffffff&fg=333333&s=0&c=20201002&zoom=4.5) over the diagonal. In the famous [Selberg trace formula](https://en.wikipedia.org/wiki/Selberg_trace_formula), the spectral side involves eigenvalues of the Laplacian on a hyperbolic surface, while the geometric side involves the lengths of closed geodesics. Believe it or not, closed geodesics behave a lot like primes! This analogy has inspired many attempts on the Riemann Hypothesis.

In 1952, André Weil reformulated the explicit formula for primes and zeros in this spirit. He also showed that the generalized Riemann Hypothesis is equivalent to a certain functional, built from the explicit formula, being positive. This is called [Weil’s positivity criterion](https://en.wikipedia.org/wiki/Weil's_criterion).

In the late 1990s, Alain Connes went further. He found an operator on a space built from the adeles of a number field whose trace formula is the explicit formula. In his words,

> This reduces the Riemann hypothesis to the validity of the trace formula….

If you want to understand what Masullo is doing, I recommend this paper:

• Alain Connes, [Trace formula in noncommutative geometry and the zeros of the Riemann zeta function](https://arxiv.org/abs/math/9811068).

Masullo works in the same setting as Connes, but with some key technical novelties. Very roughly:

**The space.** Masullo works with functions on the [idele class group](https://en.wikipedia.org/wiki/Idele_group) of a number field ![F.](https://s0.wp.com/latex.php?latex=F.&bg=ffffff&fg=333333&s=0&c=20201002&zoom=4.5) This is a group that packages together all the ways of measuring the size of elements of ![F](https://s0.wp.com/latex.php?latex=F&bg=ffffff&fg=333333&s=0&c=20201002&zoom=4.5): one for each prime of ![F](https://s0.wp.com/latex.php?latex=F&bg=ffffff&fg=333333&s=0&c=20201002&zoom=4.5), and also one for each way of embedding ![F](https://s0.wp.com/latex.php?latex=F&bg=ffffff&fg=333333&s=0&c=20201002&zoom=4.5) in the real or complex numbers. Each test function ![\varphi](https://s0.wp.com/latex.php?latex=%5Cvarphi&bg=ffffff&fg=333333&s=0&c=20201002&zoom=4.5) on this group gives an operator ![A(\varphi)](https://s0.wp.com/latex.php?latex=A%28%5Cvarphi%29&bg=ffffff&fg=333333&s=0&c=20201002&zoom=4.5) by convolution. This operator acts on the space of ‘all’ complex functions on the idele class group modulo a subspace cleverly chosen to focus attention on the zeros of the Hecke L-function.

**The spectral side.** The trace of ![A(\varphi)](https://s0.wp.com/latex.php?latex=A%28%5Cvarphi%29&bg=ffffff&fg=333333&s=0&c=20201002&zoom=4.5) is a sum over the zeros of the Hecke L-function. Each zero ![\rho](https://s0.wp.com/latex.php?latex=%5Crho&bg=ffffff&fg=333333&s=0&c=20201002&zoom=4.5) contributes the [Mellin transform](https://en.wikipedia.org/wiki/Mellin_transform) of ![\varphi](https://s0.wp.com/latex.php?latex=%5Cvarphi&bg=ffffff&fg=333333&s=0&c=20201002&zoom=4.5) evaluated at that zero. The Mellin transform is the multiplicative version of the Fourier transform:

![\displaystyle{ \int_0^\infty \varphi(t)\, t^{s} \, dt/t }](https://s0.wp.com/latex.php?latex=%5Cdisplaystyle%7B+%5Cint_0%5E%5Cinfty+%5Cvarphi%28t%29%5C%2C+t%5E%7Bs%7D+%5C%2C+dt%2Ft+%7D&bg=ffffff&fg=333333&s=0&c=20201002&zoom=4.5)

**The geometric side.** The same trace is an integral of a kernel over the diagonal. Most of Masullo’s 940 pages go into computing this integral by a new route. The kernel is split into pieces, one for each prime. For a test function supported on ![t > 1](https://s0.wp.com/latex.php?latex=t+%3E+1&bg=ffffff&fg=333333&s=0&c=20201002&zoom=4.5), a Poisson summation argument makes the contribution of each sufficiently large prime vanish. Only finitely many primes are left over. Their contributions are handled by a local version of a trace formula due to Connes.

Weil’s criterion, which many earlier attempts in this tradition aimed at, plays no role. As Masullo puts it in section 1.4.6:

> **Weil’s Positivity Criterion is not used.**

The boldface font is his.

### The punchline

Now for the key move. Fix a complex number ![z](https://s0.wp.com/latex.php?latex=z&bg=ffffff&fg=333333&s=0&c=20201002&zoom=4.5) and consider the function

![\displaystyle{  \varphi_z(t) = \begin{cases} t^{-z} & t > 1 \\ 0 & t < 1 . \end{cases} }](https://s0.wp.com/latex.php?latex=%5Cdisplaystyle%7B++%5Cvarphi_z%28t%29+%3D+%5Cbegin%7Bcases%7D+t%5E%7B-z%7D+%26+t+%3E+1+%5C%5C+0+%26+t+%3C+1+.+%5Cend%7Bcases%7D+%7D+&bg=ffffff&fg=333333&s=0&c=20201002&zoom=4.5)

Its Mellin transform is easy to compute:

![\displaystyle{  \int_1^\infty t^{-z} \, t^{s} \, \frac{dt}{t} = \frac{1}{z - s} \qquad \text{when } \mathrm{Re}(s) < \mathrm{Re}(z) . }](https://s0.wp.com/latex.php?latex=%5Cdisplaystyle%7B++%5Cint_1%5E%5Cinfty+t%5E%7B-z%7D+%5C%2C+t%5E%7Bs%7D+%5C%2C+%5Cfrac%7Bdt%7D%7Bt%7D+%3D+%5Cfrac%7B1%7D%7Bz+-+s%7D+%5Cqquad+%5Ctext%7Bwhen+%7D+%5Cmathrm%7BRe%7D%28s%29+%3C+%5Cmathrm%7BRe%7D%28z%29+.+%7D+&bg=ffffff&fg=333333&s=0&c=20201002&zoom=4.5)

So when you plug ![\varphi_z](https://s0.wp.com/latex.php?latex=%5Cvarphi_z&bg=ffffff&fg=333333&s=0&c=20201002&zoom=4.5) into the trace formula, the spectral side becomes a sum of terms like ![1/(z - \rho)](https://s0.wp.com/latex.php?latex=1%2F%28z+-+%5Crho%29&bg=ffffff&fg=333333&s=0&c=20201002&zoom=4.5), one for each zero ![\rho.](https://s0.wp.com/latex.php?latex=%5Crho.&bg=ffffff&fg=333333&s=0&c=20201002&zoom=4.5) This is exactly the sum over zeros that we saw in the formula for the logarithmic derivative, now for ![L'/L](https://s0.wp.com/latex.php?latex=L%27%2FL&bg=ffffff&fg=333333&s=0&c=20201002&zoom=4.5) instead of ![\zeta'/\zeta.](https://s0.wp.com/latex.php?latex=%5Czeta%27%2F%5Czeta.&bg=ffffff&fg=333333&s=0&c=20201002&zoom=4.5) Meanwhile, the prime-by-prime pieces of the geometric side produce terms like ![\log p](https://s0.wp.com/latex.php?latex=%5Clog+p&bg=ffffff&fg=333333&s=0&c=20201002&zoom=4.5) times powers of ![p^{-z}.](https://s0.wp.com/latex.php?latex=p%5E%7B-z%7D.&bg=ffffff&fg=333333&s=0&c=20201002&zoom=4.5) That is the sum over primes for ![L'/L.](https://s0.wp.com/latex.php?latex=L%27%2FL.&bg=ffffff&fg=333333&s=0&c=20201002&zoom=4.5) So with this test function, the trace formula becomes the explicit formula for ![L'/L](https://s0.wp.com/latex.php?latex=L%27%2FL&bg=ffffff&fg=333333&s=0&c=20201002&zoom=4.5) itself.

If that were the whole story, nothing would be gained: that identity was already known! The claimed gain is this. Masullo's way of computing the geometric side gives an expression that is supposed to stay bounded, and hence holomorphic in ![z](https://s0.wp.com/latex.php?latex=z&bg=ffffff&fg=333333&s=0&c=20201002&zoom=4.5), in a region where the naive sum over primes diverges. Specifically, it is supposed to stay holomorphic everywhere to the right of the line ![\mathrm{Re}(z) = 1/2.](https://s0.wp.com/latex.php?latex=%5Cmathrm%7BRe%7D%28z%29+%3D+1%2F2.&bg=ffffff&fg=333333&s=0&c=20201002&zoom=4.5) A holomorphic function has no poles. So ![L'/L](https://s0.wp.com/latex.php?latex=L%27%2FL&bg=ffffff&fg=333333&s=0&c=20201002&zoom=4.5) would have no poles to the right of this line, and ![L](https://s0.wp.com/latex.php?latex=L&bg=ffffff&fg=333333&s=0&c=20201002&zoom=4.5) would have no zeros there. An equation relating ![L(s,\chi)](https://s0.wp.com/latex.php?latex=L%28s%2C%5Cchi%29&bg=ffffff&fg=333333&s=0&c=20201002&zoom=4.5) to ![L(1-s,\bar\chi)](https://s0.wp.com/latex.php?latex=L%281-s%2C%5Cbar%5Cchi%29&bg=ffffff&fg=333333&s=0&c=20201002&zoom=4.5) then rules out zeros to the left of ![\mathrm{Re}(z) = 1/2.](https://s0.wp.com/latex.php?latex=%5Cmathrm%7BRe%7D%28z%29+%3D+1%2F2.&bg=ffffff&fg=333333&s=0&c=20201002&zoom=4.5) All nontrivial zeros would lie on the line. That’s the generalized Riemann hypothesis!

But—you guessed it—there is a technical wrinkle. The function ![\varphi_z](https://s0.wp.com/latex.php?latex=%5Cvarphi_z&bg=ffffff&fg=333333&s=0&c=20201002&zoom=4.5) jumps at ![t = 1](https://s0.wp.com/latex.php?latex=t+%3D+1&bg=ffffff&fg=333333&s=0&c=20201002&zoom=4.5), and it is not compactly supported. So it is not a legal test function for the trace formula. Masullo therefore approximates it by smooth, compactly supported functions, built from a cutoff that increases to the characteristic function of ![(1,\infty).](https://s0.wp.com/latex.php?latex=%281%2C%5Cinfty%29.&bg=ffffff&fg=333333&s=0&c=20201002&zoom=4.5) He then takes a limit. To pass to the limit, he needs the approximating expressions to be *uniformly bounded* on compact sets. Then a standard compactness theorem for holomorphic functions, Montel's theorem, gives a subsequence converging to a holomorphic limit.

So the argument seems to have two hard parts. First, the trace formula itself, with Masullo’s new computation of the geometric side: this is what most of the 940 pages are devoted to. Second, the final limiting step: does the geometric side really stay under control as the cutoff approaches the jump at ![t = 1?](https://s0.wp.com/latex.php?latex=t+%3D+1%3F&bg=ffffff&fg=333333&s=0&c=20201002&zoom=4.5) If either part fails, the proof fails.

### Some possible problems

Vakil asked people to check Masullo’s argument. They did, and within hours problems were reported:

**A problem at the prime 2.** [An AI-assisted review organized by Eric Naslund](https://github.com/enaslund/rh-verification) tests one of the manuscript's theorems on a test function concentrated near the number 2. The theorem predicts the answer log 2. But the review argues that the limit used to compute it involves two conditions that no point can satisfy simultaneously, so the limit is 0.

**A functional analysis step.** Participants in an [online checking effort](https://github.com/mccorvie/riemann-masullo/issues/2469) claim to have spotted other mistakes. For example, on page 444 Masullo extends some terminology for bases from the setting of ‘Schauder bases’, where the coordinate projections are uniformly bounded, to the more general ‘Markushevich bases’, where they need not be. He calls a basis ‘regular’ if its vectors stay a fixed distance away from zero, and adds that “in particular” the functionals extracting coordinates are then bounded. For Schauder bases this is true. For Markushevich bases it’s apparently false. Supposedly this false claim is used later in the manuscript. Maybe the specific bases Masullo uses have the needed bound for other reasons.

These objections may turn out to be fixable or mistaken. And remember, the whole paper is openly a draft. For example, Chapter 13, where the generalized Riemann hypothesis should finally be deduced, contains only placeholders where the theorem should be! Brian Conrad, who has been reading the manuscript for months, told *Scientific American* it is undergoing major revision. The original plan was for Masullo to spend a year polishing the text with Conrad before any announcement. Alpöge's claim forced a hasty release.

### Why it’s interesting anyway

Even if Masullo's argument fails, it's worth understanding. It tries to carry out a well-known strategy: realize the zeros as the spectrum of an operator, realize the primes as the "closed orbits" on the other side of a trace formula, and read off the generalized Riemann hypothesis from that duality. It is quite unlike Alpöge's argument, which transplants Stepanov’s proof from finite fields to number fields and uses heavy machinery from automorphic forms. So even if one of these proofs turns out to be right, it won't make the other one redundant.

And if Masullo's argument can't be saved, we will learn exactly where this particular version of the trace-formula dream breaks down. That is what happens to most attempts on the Riemann Hypothesis. This one gets to happen in the glare of public view, at high speed.

 This entry was posted on Sunday, November 22nd, 2026 at 12:47 am and is filed under [mathematics](https://johncarlosbaez.wordpress.com/category/mathematics/). You can follow any responses to this entry through the [RSS 2.0](https://johncarlosbaez.wordpress.com/2026/11/22/masullo-on-the-riemann-hypothesis/feed/) feed. You can [leave a response](https://johncarlosbaez.wordpress.com/2026/11/22/masullo-on-the-riemann-hypothesis/#respond), or [trackback](https://johncarlosbaez.wordpress.com/2026/11/22/masullo-on-the-riemann-hypothesis/trackback/) from your own site.
