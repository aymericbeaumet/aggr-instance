---
title: Chemistry is Actually Geometry
link: https://chillphysicsenjoyer.substack.com/p/chemistry-is-actually-geometry
source: hnrss-org
published: 2026-11-16T20:22:18Z
updated: 2026-11-16T20:22:18Z
first_seen: 2026-11-17T09:02:46.426369911Z
authors:
- sebg
content: extracted
html: 2026-11-16-chemistry-is-actually-geometry.html
extra:
  comments_url: https://news.ycombinator.com/item?id=50522643
  num_comments: 108
  points: 216
---

Theoretical Biologists Should Know About Feinberg’s Deficiency Theorems

When a set of molecules combine to make a different set of molecules, we get a **chemical reaction**. Reactions can release heat or change colour. They can form weird patterns. And most importantly, the concentrations of molecules in the reaction can evolve in surprising ways.

Life itself **is** a chemical process. Biochemical reactions are complex, and yet appear to be very robust in a wide range of environments. To build synthetic forms with life-like behaviour, we would likely need theory that tells us how to manufacture the many quirks of chemical processes at will¹, or engineer *arbitrary* properties at low cost. To do this, a model of complex chemical systems is required.²

But this is difficult. Models of complicated biochemical processes involve huge numbers of differential equations and even more *parameters*; many of which we can't estimate. A model of the Krebs biochemical cycle might include hundreds of ***rate constants,*** which are the theoretical speeds at which each of the sub-reactions occurs. Unfortunately, if we experimented with such processes in a closed vat, it would be impossible to estimate these rate constants with any certainty.

Large numbers of estimated parameters are bad. We should be looking for better abstractions that depend on fewer parameters. It is true that such abstractions might force us to sacrifice precision, and only leave us with qualitative predictions. But for processes that are as noisy as biology, this tradeoff seems worth it. We should be able to point to an abstract *structure* behind a complex biochemical model and say 'we expect that this organism can adapt to *these* scenarios, but not *those* '.

As said in Martin Feinberg's *Foundations of Chemical Reaction Network Theory* \[2\]:

> Since each new network of chemical reactions gives rise to its own complicated system of differential equations, it becomes apparent that, in the absence of an overarching theory, we would be forced to study complex chemical systems on a case-by-case basis, and each new case would be fraught with terrible analytical difficulties. What’s needed is a way of looking at things from a broader and more general perspective.

Without theory we are blind to the landscape of what is actually possible.

An increasingly natural abstraction for chemical processes is networks, geometry and topology. The goal of this essay is to describe some results in network science and geometry that can inform us about properties of chemical reactions through fewer parameters. I found Feinberg's seminal *Foundations of Chemical Reaction Network Theory* \[2\] quite dense for a beginner like me. In this essay I will be introducing the key results of this book from a less formal and more intuitive standpoint. I will assume knowledge of basic linear algebra, and include many examples. **Nothing I write below is original work**. It's my simplified model of what I have read in chemical reaction network theory, especially that of Martin Feinberg \[2\]. All mistakes are mine.

I hope essays of this form will help us reason about chemical reaction networks more easily, and therefore push us towards more abstract theories of biochemistry. Who knows, maybe biology is actually just geometry too.

Chemical systems have a wide range of surprising behaviours, illustrating demand for a more abstract model. One such behaviour is that they can **oscillate**. This is when the concentration, pH, or any other property of the reaction exhibits periodic cycles that are stable. The Belousov-Zhabotinsky and Briggs-Rauscher reactions are examples where the concentration of a substance in the reaction oscillates.

The Belousov-Zhabotinsky reaction (Belousov 1959; Zhabotinsky 1964; Field, Körös & Noyes 1972) \[3\] was one of the first chemical oscillators to be constructed. This is a chemical system that involves at least a bromate and an acid, and during this reaction the concentration of Br- oscillates. It was originally motivated by Boris Pavlovich Belousov when he tried to find a non-organic analogue to the Krebs cycle.

The Briggs–Rauscher oscillator (Briggs & Rauscher 1973) \[4\] is a chemical oscillator where the concentration of iodide and iodine in the system oscillates. The mechanism behind why has been revised several times in Noyes and Furrow (1982) \[5\], De Kepper and Epstein (1982) \[6\], and Schmitz and Furrow (2016) \[7\].

[![](https://substackcdn.com/image/fetch/$s_!iOMP!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe7041259-2946-447c-99f3-02b2be01cfa4_643x556.jpeg)](https://substackcdn.com/image/fetch/$s_!iOMP!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe7041259-2946-447c-99f3-02b2be01cfa4_643x556.jpeg)

\*Oscillogram of iodide concentration in the Briggs–Rauscher reaction, recorded by Briggs and Rauscher in June 1972. Image: Thomas Briggs, via [Wikimedia Commons](https://commons.wikimedia.org/wiki/File:B-ROscGraph2.jpg), licence CC BY-SA 3.0.

There are also chemical clocks where the pH in the system oscillates, as opposed to chemical concentrations. This happens in the urea–urease clock system (Hu et al. 2010) \[8\]. Chemistry can also form patterns; the Liesegang rings system of pattern formation (Liesegang 1896) \[9\] is a reaction-diffusion process that causes coloured stripes in a test tube.

The Lotka-Volterra equations in ecology were originally motivated by Alfred Lotka's attempts to find a theory of oscillations in chemical networks \[10\]. This was known as the Lotka network (Lotka 1920) \[11\].

In biochemistry, we have cycles where some of the outputs are used again in the inputs. The Krebs cycle (Krebs & Johnson 1937) \[12\] continuously recycles oxaloacetate. The Calvin cycle regenerates ribulose-1,5-bisphosphate (Bassham et al. 1954) \[13\].

We also have oscillators in biochemistry. One of the earliest discovered examples of oscillatory behaviour in a biochemical system was glycolysis, discovered by Duysens and Amesz in 1957 \[14\]. In this process, the metabolites from yeast (alcohol) exhibit oscillations. One such model of this was the Higgins–Sel'kov oscillator model (Higgins 1964; Sel'kov 1968) \[15\].

Alan Rendall explains this from his blog \[16\]

> If yeast cells are fed with glucose they convert it into alcohol. Those of us who appreciate alcoholic beverages can be grateful to them for that. In the presence of a supply of glucose with a small constant rate alcohol is produced at a constant rate. When the supply rate is increased something more interesting happens. The output starts to undergo periodic oscillations although the input is constant. It is not that the yeast cells are using some kind of complicated machine to produce these. If the cells are broken down to make yeast extract the effect persists. In fact for yeast extract the oscillations go away again for very high concentrations of glucose, an effect not seen for intact cells. This difference is not important for the basic mechanism of production of oscillations.

The KaiC phosphorylation cycle (Nakajima et al. 2005; Rust et al. 2007) \[17\] is another biochemical oscillator. In this system, the fraction of the number of KaiC proteins with a phosphate group attached oscillates as well.

Other strange behaviours I have found include 'futile cycles' (Hell & Rendall 2015) \[18\] and 'bi-stability' (Gardner, Cantor & Collins 2000; Ferrell 2002; Craciun, Tang & Feinberg 2006) \[19\]\[20\]. We also have synchronisation across multiple cells. C.H. Waddington's systems from his multiple texts on development (Waddington 1956, 1957) \[21\] exhibit positive feedback and autocatalysis. We have the Huang-Ferrell Model of MAPK Signaling (Huang & Ferrell 1996) \[22\].

To reason about chemical systems, we need a way to write them sensibly on paper, or on a computer. It turns out that there are many representations, each with pros and cons. These representations can offer different levels of *abstraction*, an idea presented by Peter Stadler in his 2023 presentation at Santa Fe \[23\].

The most physically rich representation of chemical reaction would be a quantum one. Given two molecules, we would write out the coordinates of all of the nuclei and electrons and then solve how the wave-function evolves under the time-dependent Schrodinger equation.

\\begin{aligned} i\\hbar\\,\\frac{\\partial \\Psi}{\\partial t} = \\Big\[ &\\underbrace{-\\sum\_{i=1}^{n}\\frac{\\hbar^2}{2m\_e}\\nabla\_{i}^{2}}\_{\\text{electron kinetic energy}} \\;\\underbrace{-\\sum\_{\\alpha=1}^{M}\\frac{\\hbar^2}{2M\_\\alpha}\\nabla\_{\\alpha}^{2}}\_{\\text{nuclear kinetic energy}} \\\\\[1ex\] &+ \\underbrace{V(\\mathbf r\_1,\\dots,\\mathbf r\_n;\\ \\mathbf R\_1,\\dots,\\mathbf R\_M)}\_{\\text{Coulomb, every pair}}\\Big\]\\Psi \\end{aligned}

\\Psi = \\Psi(\\mathbf r\_1,\\dots,\\mathbf r\_n;\\ \\mathbf R\_1,\\dots,\\mathbf R\_M;\\ t)

[![](https://substackcdn.com/image/fetch/$s_!yr66!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F6ab980ec-f61d-4e72-ac63-cefd1607182c_1219x636.png)](https://substackcdn.com/image/fetch/$s_!yr66!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F6ab980ec-f61d-4e72-ac63-cefd1607182c_1219x636.png)

This is probably overkill - simpler representations can still give us modelling value. We can also represent chemical reactions the way we're used to in high school, and still make meaningful deductions.

\\mathrm{N\_2(g)} + 3\\,\\mathrm{H\_2(g)} \\;\\rightleftharpoons\\; 2\\,\\mathrm{NH\_3(g)}, \\qquad \\Delta H = -92\\ \\mathrm{kJ\\,mol^{-1}}

For the biologists out there, another way we can represent them is with chemical reaction diagrams in biochemistry.

[![](https://substackcdn.com/image/fetch/$s_!7jxU!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F880cbcee-3a48-480e-8571-e5a206c5e705_1088x1604.png)](https://substackcdn.com/image/fetch/$s_!7jxU!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F880cbcee-3a48-480e-8571-e5a206c5e705_1088x1604.png)

We can also represent them with a Petri net (Baez & Biamonte 2018; Baez 2011) \[24\]\[25\], which encodes the reaction constant, reactants and products

[![](https://substackcdn.com/image/fetch/$s_!mQsl!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F54025a97-19e9-464e-a640-81b29f96640b_899x519.png)](https://substackcdn.com/image/fetch/$s_!mQsl!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F54025a97-19e9-464e-a640-81b29f96640b_899x519.png)

From Azimuth by John Baez

And finally, we have the most abstract representation, where we 'add' compounds together with sums and arrows to get a different set. In this essay, we'll be using this particular representation to model concentrations. I will explain in much more detail in the next section.

\\begin{aligned} X + Y &\\xrightarrow{\\alpha} XY \\\\ \\mathrm{ATP} &\\xrightarrow{\\beta} \\mathrm{ADP} + \\mathrm{P\_i} \\\\ X + \\mathrm{ATP} &\\xrightarrow{\\gamma} X\\mathrm{P\_i} + \\mathrm{ADP} \\\\ X\\mathrm{P\_i} + Y &\\xrightarrow{\\delta} XY + \\mathrm{P\_i} \\end{aligned}

Our end goal is to model the evolution of the **concentration** of each substance in a chemical reaction represented in this way. Concentrations are how many molecules we have for a compound in the network within a fixed volume.

[![](https://substackcdn.com/image/fetch/$s_!D95D!,w_1456,c_limit,f_auto,q_auto:good,fl_lossy/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd17ec784-8a70-46c3-b480-903fd716c86a_1000x520.gif)](https://substackcdn.com/image/fetch/$s_!D95D!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd17ec784-8a70-46c3-b480-903fd716c86a_1000x520.gif)

A lot of the time, the concentrations of each reaction settle to a steady state called an equilibrium. The diagram below shows the trajectory of a set of initial concentrations in a reversible triangle reaction with three distinct compounds. Each axis represents the concentration of one of the three compounds, and each of the lines shows the different trajectories starting at different concentrations. In this scenario, no matter where the initial concentrations start from on the surface, they converge to the same point.

[![](https://substackcdn.com/image/fetch/$s_!K-xU!,w_1456,c_limit,f_auto,q_auto:good,fl_lossy/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F29358177-634d-46ce-96d2-984326cc5fb8_800x700.gif)](https://substackcdn.com/image/fetch/$s_!K-xU!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F29358177-634d-46ce-96d2-984326cc5fb8_800x700.gif)

A given chemical reaction system could also have multiple equilibria, and it's not clear how to predict how many equilibria exist. The equilibria could be ***stable***, in which small perturbations still force the trajectory to go back to the those points, or they could be ***unstable***. For example, Edelstein's network (Edelstein 1970) \[26\] has three equilibria, two of which are stable, and one of which is unstable.³

[![](https://substackcdn.com/image/fetch/$s_!Iwrr!,w_1456,c_limit,f_auto,q_auto:good,fl_lossy/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F00feab31-2936-418a-a5db-adb1221dcd0e_850x700.gif)](https://substackcdn.com/image/fetch/$s_!Iwrr!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F00feab31-2936-418a-a5db-adb1221dcd0e_850x700.gif)

Sometimes equilibria don't exist. If so, then what is the nature of the dynamics? Are chaotic solutions permitted? Do we see the dynamics of the concentrations moving around in an unpredictable fashion? Are periodic solutions permitted? It turns out, these are all possible.

The following network is the Oregonator reduction of the Belousov-Zhabotinsky reaction (Field & Noyes 1974) \[27\], with X = HBrO₂, Y = Br⁻, Z = Ce⁴⁺ and the pool chemicals A = BrO₃⁻, B = malonic acid held constant. In this reaction, the trajectory of the concentration converges onto a periodic limit cycle, where the concentrations continually follow a loop. This is shown in the animation below.

\\begin{aligned} A + Y &\\to X + P &&\\quad \\mathrm{BrO\_3^-} + \\mathrm{Br^-} \\to \\mathrm{HBrO\_2} + \\mathrm{HOBr} \\\\ X + Y &\\to 2P &&\\quad \\mathrm{HBrO\_2} + \\mathrm{Br^-} \\to 2\\,\\mathrm{HOBr} \\\\ A + X &\\to 2X + 2Z &&\\quad \\mathrm{BrO\_3^-} + \\mathrm{HBrO\_2} \\to 2\\,\\mathrm{HBrO\_2} + 2\\,\\mathrm{Ce^{4+}} \\qquad \\text{(autocatalysis)} \\\\ 2X &\\to A + P &&\\quad 2\\,\\mathrm{HBrO\_2} \\to \\mathrm{BrO\_3^-} + \\mathrm{HOBr} \\\\ B + Z &\\to fY &&\\quad \\mathrm{MA} + \\mathrm{Ce^{4+}} \\to f\\,\\mathrm{Br^-} \\end{aligned}

[![](https://substackcdn.com/image/fetch/$s_!HqH5!,w_1456,c_limit,f_auto,q_auto:good,fl_lossy/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F7f859cf0-e6d2-4703-8a87-a06434355741_800x700.gif)](https://substackcdn.com/image/fetch/$s_!HqH5!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F7f859cf0-e6d2-4703-8a87-a06434355741_800x700.gif)

To reason about when these funky behaviours with concentrations occur, we take the approach of modelling chemical reactions as **geometry**. We will first define the key objects like *species, reaction vectors, stoichiometric subspaces, and stoichiometric classes,* in terms of linear algebra.

After that, we will define what an **equilibrium** is through the lens of intersecting geometries. We will then work towards the **deficiency zero theorem**, which allows us to put a bound on the number of equilibrium points purely from its network structure and geometry.

In our representation, a **chemical reaction network** is a set of **reactions**. A **reaction** is a transformation of a set of compounds called *reactants*, to another set of compounds called *products*. Each reaction is written as a *formal sum* of letters that maps to a different formal sum of letters. A chemical reaction network is represented by a list of these reactions.

This is a chemical reaction network:

\\begin{aligned} A &\\rightleftharpoons 2B \\\\ A + C &\\rightleftharpoons D \\\\ D &\\to B + E \\\\ B + E &\\to A + C \\end{aligned}

The forward-and-back arrows are shorthand for two mappings back and forth.

A \\rightleftharpoons B \\qquad \\text{can be written as} \\qquad \\begin{aligned} A &\\to B \\\\ B &\\to A \\end{aligned}

The plus sign is **not** addition in the mathematical sense. Rather, it's 'formal sum', and we don't have subtractions between the letters. These letters also can have integer coefficients, which specify how much of that object is used up in the chemical reaction. These are called **stoichiometric coefficients**. So in the network below, the stoichiometric coefficient of B is 2, and the coefficient of A is 1.

2B \\to A

Sometimes, we add constants above and below the arrows to show how *fast* each reaction in the network occurs. These are called **rate constants**.

\\begin{aligned} A \\;&\\underset{k\_{-1}}{\\overset{k\_1}{\\rightleftharpoons}}\\; 2B \\\\ A + C \\;&\\underset{k\_{-2}}{\\overset{k\_2}{\\rightleftharpoons}}\\; D \\\\ D \\;&\\xrightarrow{k\_3}\\; B + E \\\\ B + E \\;&\\xrightarrow{k\_4}\\; A + C \\end{aligned}

The individual letters in the reaction network are called species. The **species** in the network are the chemically distinct compounds. These could represent both individual elements or compounds, but what makes species different is that their chemical structure needs to differ. For example, for the reaction

2\\,\\mathrm{Na} + 2\\,\\mathrm{HCl} \\to 2\\,\\mathrm{NaCl} + \\mathrm{H\_2}

even though Na is an element found in NaCl, we consider the two substances as separate. And so the individual species are {Na, HCl, NaCl, H₂}. But note that a chemical reaction network need not actually represent specific compounds, nor reactions in real life. We can just use letters to be general. For example, the Lotka network is

\\begin{aligned} X &\\to 2X \\\\ X + Y &\\to 2Y \\\\ Y &\\to 0 \\end{aligned}

The species in this network are {X, Y} and are unspecified. 0 does not count as a species; Y mapping to zero is meant to model Y leaving the system. X undergoes auto-catalysis because X goes to 2X. X and Y combine to make 2Y, and Y decays, which is shown by its arrow to zero.

Feinberg opens his textbook \[2\] with the toy network example

\\begin{aligned} A &\\rightleftharpoons 2B \\\\ A + C &\\rightleftharpoons D \\\\ D &\\to B + E \\\\ B + E &\\to A + C \\end{aligned}

In this network, the species are {A, B, C, D, E}. In the 'oregonator' cyclic system

\\begin{aligned} A + Y &\\to X + P \\\\ X + Y &\\to 2P \\\\ A + X &\\to 2X + 2Z \\\\ 2X &\\to A + P \\\\ B + Z &\\to fY \\end{aligned}

the species are X = HBrO₂, Y = Br⁻ and Z = Ce⁴⁺, with A = BrO₃⁻ and B = malonic acid, and P = HOBr.

A **chemical reaction network** is then a set of mappings from a **sum of species** to another sum of species. These are represented by arrows. For the Lotka network, the chemical reaction network is the set below. The set comprises pairs of sums on each side.

\\mathcal{R} = \\{\\, X \\to 2X,\\quad X + Y \\to 2Y,\\quad Y \\to 0 \\,\\}

The formal sums on the head and tails of the arrows are called **complexes**. So for the Lotka network,

\\begin{aligned} X &\\to 2X \\\\ X + Y &\\to 2Y \\\\ Y &\\to 0 \\end{aligned}

the complexes are {X, 2X, X+Y, 2Y, Y, 0}. *X* and *2X* are considered as **separate** complexes - it doesn't matter that they're multiples of the same species. *X+Y* is also a complex. For Feinberg's network

\\begin{aligned} A &\\rightleftharpoons 2B \\\\ A + C &\\rightleftharpoons D \\\\ D &\\to B + E \\\\ B + E &\\to A + C \\end{aligned}

the complexes are {A, 2B, A+C, D, B+E}, five in all. Again, it doesn't matter that terms like A and A+C contain the same species - they are counted as separate complexes. For a given chemical reaction, the reactant complex is the complex at the start of the arrow. The product complex is the complex at the head of the arrow.

\\underbrace{A + C}\_{\\text{reactant complex}} \\;\\longrightarrow\\; \\underbrace{D}\_{\\text{product complex}}

By considering the individual species as dimensions in a **vector space**, we can write out **complexes as vectors**. For example, in a reaction network with species {A, B, C}, the following complexes can be written as the following vectors in three dimensional space

A + C = \\begin{pmatrix} 1 \\\\ 0 \\\\ 1 \\end{pmatrix}, \\qquad B = \\begin{pmatrix} 0 \\\\ 1 \\\\ 0 \\end{pmatrix}, \\qquad 2B = \\begin{pmatrix} 0 \\\\ 2 \\\\ 0 \\end{pmatrix}

In the Lotka network, the complexes can be represented as the vectors

X = \\begin{pmatrix} 1 \\\\ 0 \\end{pmatrix}, \\qquad Y = \\begin{pmatrix} 0 \\\\ 1 \\end{pmatrix}, \\qquad X + Y = \\begin{pmatrix} 1 \\\\ 0 \\end{pmatrix} + \\begin{pmatrix} 0 \\\\ 1 \\end{pmatrix} = \\begin{pmatrix} 1 \\\\ 1 \\end{pmatrix}, \\qquad 2X = \\begin{pmatrix} 2 \\\\ 0 \\end{pmatrix}

**Reaction vectors** are the main construct used in chemical reaction networks. For a given reaction in a network, reaction vectors are the vector we get when we subtract the reagent complex from the product complex. So for a given reaction **y** → **y′**, the reaction vector is given by

\\underbrace{\\mathbf{y}}\_{\\text{reactant complex}} \\;\\longrightarrow\\; \\underbrace{\\mathbf{y}'}\_{\\text{product complex}} \\qquad\\Longrightarrow\\qquad \\underbrace{\\,\\mathbf{y}' - \\mathbf{y}\\,}\_{\\text{reaction vector}}

For example, given the reaction

A \\to B

Our reaction vector is

B - A = \\begin{pmatrix} 0 \\\\ 1 \\end{pmatrix} - \\begin{pmatrix} 1 \\\\ 0 \\end{pmatrix} = \\begin{pmatrix} -1 \\\\ 1 \\end{pmatrix}

Or more generally,

\\mathbf{y} = \\begin{pmatrix} y\_1 \\\\ y\_2 \\\\ \\vdots \\\\ y\_N \\end{pmatrix}, \\qquad \\mathbf{y}' = \\begin{pmatrix} y'\_1 \\\\ y'\_2 \\\\ \\vdots \\\\ y'\_N \\end{pmatrix}, \\qquad \\mathbf{y}' - \\mathbf{y} = \\begin{pmatrix} y'\_1 - y\_1 \\\\ y'\_2 - y\_2 \\\\ \\vdots \\\\ y'\_N - y\_N \\end{pmatrix}, \\qquad N = \\text{number of species}

Here **y** and **y′** are both vectors in the species space, one coordinate per species, and the subtraction is ordinary vector subtraction: positive entries are species made by the reaction, negative entries are species used up.

For the Lotka network,

\\begin{aligned} X &\\to 2X \\\\ X + Y &\\to 2Y \\\\ Y &\\to 0 \\end{aligned}

we first write each complex as vectors in (X, Y). Then, for each reaction, we subtract the vectors of the reactants from the vectors of the product. Each of the three reactions has the following reaction vector

\\begin{aligned} X \\to 2X \\quad&\\mapsto\\quad 2X - X = X &&= \\begin{pmatrix} 1 \\\\ 0 \\end{pmatrix} \\\\\[1ex\] X + Y \\to 2Y \\quad&\\mapsto\\quad 2Y - (X + Y) = -X + Y &&= \\begin{pmatrix} -1 \\\\ 1 \\end{pmatrix} \\\\\[1ex\] Y \\to 0 \\quad&\\mapsto\\quad 0 - Y = -Y &&= \\begin{pmatrix} 0 \\\\ -1 \\end{pmatrix} \\end{aligned}

We can plot each of the reaction vectors on the two dimensional plane.

[![](https://substackcdn.com/image/fetch/$s_!oC7o!,w_1456,c_limit,f_auto,q_auto:good,fl_lossy/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4a07245e-608c-4290-aa16-a8909ee3310a_600x600.gif)](https://substackcdn.com/image/fetch/$s_!oC7o!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4a07245e-608c-4290-aa16-a8909ee3310a_600x600.gif)

For Feinberg's first network \[2\]

\\begin{aligned} A &\\rightleftharpoons 2B \\\\ A + C &\\rightleftharpoons D \\\\ D &\\to B + E \\\\ B + E &\\to A + C \\end{aligned}

our reaction vectors are

\\begin{aligned} 2B - A &= -A + 2B &&= \\begin{pmatrix} -1 \\\\ 2 \\\\ 0 \\\\ 0 \\\\ 0 \\end{pmatrix} \\\\\[1ex\] D - (A + C) &= -A - C + D &&= \\begin{pmatrix} -1 \\\\ 0 \\\\ -1 \\\\ 1 \\\\ 0 \\end{pmatrix} \\\\\[1ex\] (B + E) - D &= B - D + E &&= \\begin{pmatrix} 0 \\\\ 1 \\\\ 0 \\\\ -1 \\\\ 1 \\end{pmatrix} \\\\\[1ex\] (A + C) - (B + E) &= A - B + C - E &&= \\begin{pmatrix} 1 \\\\ -1 \\\\ 1 \\\\ 0 \\\\ -1 \\end{pmatrix} \\end{aligned}

We have now successfully mapped a chemical reaction network to a set of **reaction vectors**. Vectors put us in the territory of linear algebra, and the first thing to do is compute the **span** of the reaction vectors. The **span** of a vector space is the set of points that you could reach with a linear combination of those vectors. Once we get the span of the vectors, we can also calculate what its dimension is.

From the Lotka network,

\\begin{aligned} X &\\to 2X \\\\ X + Y &\\to 2Y \\\\ Y &\\to 0 \\end{aligned}

our reaction vectors are

2X - X = X = \\begin{pmatrix} 1 \\\\ 0 \\end{pmatrix}, \\qquad 2Y - (X + Y) = -X + Y = \\begin{pmatrix} -1 \\\\ 1 \\end{pmatrix}, \\qquad 0 - Y = -Y = \\begin{pmatrix} 0 \\\\ -1 \\end{pmatrix}

The vectors above span the whole of two dimensional space, which means that any point in the x-y plane can be accessed through some linear combination of the reaction vectors.

[![](https://substackcdn.com/image/fetch/$s_!0ZYP!,w_1456,c_limit,f_auto,q_auto:good,fl_lossy/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F0d3c8393-7504-4cc9-b589-5dfa1a92d9f2_650x600.gif)](https://substackcdn.com/image/fetch/$s_!0ZYP!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F0d3c8393-7504-4cc9-b589-5dfa1a92d9f2_650x600.gif)

It is not always the case that two reactions can span two dimensions. We might have a two species network with reaction vectors that don't necessarily span a 2 dimensional subspace. If we had the following network, both of the reactions lead to reaction vectors that are multiples of each other. Consider the network

\\begin{aligned} A &\\to B \\\\ B &\\to A \\end{aligned}

The reaction vectors are

B - A = \\begin{pmatrix} -1 \\\\ 1 \\end{pmatrix}, \\qquad A - B = \\begin{pmatrix} 1 \\\\ -1 \\end{pmatrix} = -\\begin{pmatrix} -1 \\\\ 1 \\end{pmatrix}

but in this case the second reaction vector is just the negative of the first reaction vector. The stoichiometric subspace is just the line, and it's one dimensional.

[![](https://substackcdn.com/image/fetch/$s_!YOJK!,w_1456,c_limit,f_auto,q_auto:good,fl_lossy/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fee8cf20c-4a7a-4faa-8ac8-cea9ad06798c_600x600.gif)](https://substackcdn.com/image/fetch/$s_!YOJK!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fee8cf20c-4a7a-4faa-8ac8-cea9ad06798c_600x600.gif)

Let's look at some three species reactions. A set of three reactions might just give a one dimensional subspace. This happens when all the vectors are also linear multiples of each other. The network with three species

\\begin{aligned} C &\\to A + B \\\\ A + B &\\to C \\\\ 2C &\\to 2A + 2B \\end{aligned}

has the following reaction vectors:

\\begin{aligned} \\mathbf{v}\_1 &= (A + B) - C &&= \\begin{pmatrix} 1 \\\\ 1 \\\\ -1 \\end{pmatrix} \\\\\[1ex\] \\mathbf{v}\_2 &= C - (A + B) &&= \\begin{pmatrix} -1 \\\\ -1 \\\\ 1 \\end{pmatrix} \\\\\[1ex\] \\mathbf{v}\_3 &= (2A + 2B) - 2C &&= \\begin{pmatrix} 2 \\\\ 2 \\\\ -2 \\end{pmatrix} \\end{aligned}

but these reaction vectors are all linear multiples of a single vector. The second reaction vector is the negative of the first vector. The third vector is twice of the first.

\\mathbf{v}\_2 = -\\,\\mathbf{v}\_1, \\qquad \\mathbf{v}\_3 = 2\\,\\mathbf{v}\_1

S = \\operatorname{span}\\{\\mathbf{v}\_1, \\mathbf{v}\_2, \\mathbf{v}\_3\\} = \\operatorname{span}\\{\\mathbf{v}\_1\\}

S = \\left\\{\\, t \\begin{pmatrix} 1 \\\\ 1 \\\\ -1 \\end{pmatrix} : t \\in \\mathbb{R} \\right\\}, \\qquad s = 1

[![](https://substackcdn.com/image/fetch/$s_!ypLc!,w_1456,c_limit,f_auto,q_auto:good,fl_lossy/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F662676b3-db7c-4bac-bcee-d86bebf5b2c6_800x700.gif)](https://substackcdn.com/image/fetch/$s_!ypLc!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F662676b3-db7c-4bac-bcee-d86bebf5b2c6_800x700.gif)

On the other hand, the following network with three species

\\begin{aligned} A &\\to B \\\\ B &\\to C \\\\ A &\\to C \\end{aligned}

has the following reaction vectors:

\\mathbf{w}\_1 = B - A = \\begin{pmatrix} -1 \\\\ 1 \\\\ 0 \\end{pmatrix}, \\qquad \\mathbf{w}\_2 = C - B = \\begin{pmatrix} 0 \\\\ -1 \\\\ 1 \\end{pmatrix}, \\qquad \\mathbf{w}\_3 = C - A = \\begin{pmatrix} -1 \\\\ 0 \\\\ 1 \\end{pmatrix}

In this example, the first two reaction vectors add to the third. This means that only **two** of the reaction vectors are linearly independent. The span of the subspace is given by the span of the first two reaction vectors, and our stoichiometric subspace dimension is two.

\\mathbf{w}\_1 + \\mathbf{w}\_2 = \\mathbf{w}\_3, \\qquad S = \\operatorname{span}\\{\\mathbf{w}\_1, \\mathbf{w}\_2, \\mathbf{w}\_3\\} = \\operatorname{span}\\{\\mathbf{w}\_1, \\mathbf{w}\_2\\}, \\qquad s = 2

Feinberg's first network has four reaction vectors.

\\begin{aligned} \\mathbf{u}\_1 &= 2B - A &&= \\begin{pmatrix} -1 \\\\ 2 \\\\ 0 \\\\ 0 \\\\ 0 \\end{pmatrix} \\\\\[1ex\] \\mathbf{u}\_2 &= D - (A + C) &&= \\begin{pmatrix} -1 \\\\ 0 \\\\ -1 \\\\ 1 \\\\ 0 \\end{pmatrix} \\\\\[1ex\] \\mathbf{u}\_3 &= (B + E) - D &&= \\begin{pmatrix} 0 \\\\ 1 \\\\ 0 \\\\ -1 \\\\ 1 \\end{pmatrix} \\\\\[1ex\] \\mathbf{u}\_4 &= (A + C) - (B + E) &&= \\begin{pmatrix} 1 \\\\ -1 \\\\ 1 \\\\ 0 \\\\ -1 \\end{pmatrix} \\end{aligned}

In general, it is not obvious how to compute the set of linearly independent sets of vectors given a set. To do this, we use an algorithm to find the dimension of this subspace and which vectors are linearly independent. We first write out the vectors side by side.

N = \\begin{pmatrix} \\mathbf{u}\_1 & \\mathbf{u}\_2 & \\mathbf{u}\_3 & \\mathbf{u}\_4 \\end{pmatrix} = \\begin{pmatrix} -1 & -1 & 0 & 1 \\\\ 2 & 0 & 1 & -1 \\\\ 0 & -1 & 0 & 1 \\\\ 0 & 1 & -1 & 0 \\\\ 0 & 0 & 1 & -1 \\end{pmatrix}

We then repeatedly add and subtract rows between each other until the matrix is in 'row echelon form', a form of matrix from which it is easy to read out the rank.

\\begin{pmatrix} -1 & -1 & 0 & 1 \\\\ 2 & 0 & 1 & -1 \\\\ 0 & -1 & 0 & 1 \\\\ 0 & 1 & -1 & 0 \\\\ 0 & 0 & 1 & -1 \\end{pmatrix} \\xrightarrow{\\ R\_1 \\to -R\_1\\ } \\begin{pmatrix} 1 & 1 & 0 & -1 \\\\ 2 & 0 & 1 & -1 \\\\ 0 & -1 & 0 & 1 \\\\ 0 & 1 & -1 & 0 \\\\ 0 & 0 & 1 & -1 \\end{pmatrix}

\\xrightarrow{\\ R\_2 \\to R\_2 - 2R\_1\\ } \\begin{pmatrix} 1 & 1 & 0 & -1 \\\\ 0 & -2 & 1 & 1 \\\\ 0 & -1 & 0 & 1 \\\\ 0 & 1 & -1 & 0 \\\\ 0 & 0 & 1 & -1 \\end{pmatrix} \\xrightarrow{\\ R\_2 \\leftrightarrow R\_3,\\ \\ R\_2 \\to -R\_2\\ } \\begin{pmatrix} 1 & 1 & 0 & -1 \\\\ 0 & 1 & 0 & -1 \\\\ 0 & -2 & 1 & 1 \\\\ 0 & 1 & -1 & 0 \\\\ 0 & 0 & 1 & -1 \\end{pmatrix}

\\xrightarrow{\\ R\_1 \\to R\_1 - R\_2,\\ \\ R\_3 \\to R\_3 + 2R\_2,\\ \\ R\_4 \\to R\_4 - R\_2\\ } \\begin{pmatrix} 1 & 0 & 0 & 0 \\\\ 0 & 1 & 0 & -1 \\\\ 0 & 0 & 1 & -1 \\\\ 0 & 0 & -1 & 1 \\\\ 0 & 0 & 1 & -1 \\end{pmatrix}

\\xrightarrow{\\ R\_4 \\to R\_4 + R\_3,\\ \\ R\_5 \\to R\_5 - R\_3\\ } \\begin{pmatrix} 1 & 0 & 0 & 0 \\\\ 0 & 1 & 0 & -1 \\\\ 0 & 0 & 1 & -1 \\\\ 0 & 0 & 0 & 0 \\\\ 0 & 0 & 0 & 0 \\end{pmatrix}

N = \\begin{pmatrix} -1 & -1 & 0 & 1 \\\\ 2 & 0 & 1 & -1 \\\\ 0 & -1 & 0 & 1 \\\\ 0 & 1 & -1 & 0 \\\\ 0 & 0 & 1 & -1 \\end{pmatrix} \\;\\xrightarrow{\\ \\text{row reduce}\\ }\\; \\begin{pmatrix} 1 & 0 & 0 & 0 \\\\ 0 & 1 & 0 & -1 \\\\ 0 & 0 & 1 & -1 \\\\ 0 & 0 & 0 & 0 \\\\ 0 & 0 & 0 & 0 \\end{pmatrix}, \\qquad s = \\operatorname{rank} N = 3

\\text{pivots in columns } 1, 2, 3; \\qquad \\text{column } 4 = -(\\text{column } 2) - (\\text{column } 3)

In this case Feinberg's first network only spans the first three species, and so its stoichiometric subspace is of dimension three.

Now we have some vector machinery to describe reactions, let's do some dynamics. Given a reaction network, how would the concentrations of each species evolve? Given that we have *N* species, it makes sense to represent the concentrations of each species as a vector. Since concentrations can't be negative, this vector is restricted to the positive space.

\\mathbf{c} = \\begin{pmatrix} c\_A \\\\ c\_B \\\\ c\_C \\\\ \\vdots \\end{pmatrix} \\in \\mathbb{R}^N\_{\\ge 0}

In the most general form, this vector evolves as an ordinary differential equation. We assume that its dynamics is only governed by the current state at the time, and assume that the process has no 'memory'. This means we can write out dynamics as

\\dot{\\mathbf{c}} = K(\\mathbf{c})

In components, we have

\\frac{d}{dt}\\begin{pmatrix} c\_A \\\\ c\_B \\\\ c\_C \\\\ \\vdots \\end{pmatrix} = \\begin{pmatrix} K\_A(c\_A, c\_B, c\_C, \\dots) \\\\ K\_B(c\_A, c\_B, c\_C, \\dots) \\\\ K\_C(c\_A, c\_B, c\_C, \\dots) \\\\ \\vdots \\end{pmatrix}, \\qquad \\dot c\_i = K\_i(\\mathbf{c}), \\quad i = A, B, C, \\dots

where ***K*** is a function that has the same number of components as the number of species.

We can apply some physical reasoning to constrain our dynamics even further. Since our chemical reaction network is characterised by the reaction vectors, we can split K(c) into a sum of separate functions, each a function of each reaction vector. And this means that we can write our equation as

\\dot{\\mathbf{c}} = \\mathcal{K}\_{\\mathbf{y}\_1 \\to \\mathbf{y}\_1'}(\\mathbf{c})\\,(\\mathbf{y}\_1' - \\mathbf{y}\_1) + \\mathcal{K}\_{\\mathbf{y}\_2 \\to \\mathbf{y}\_2'}(\\mathbf{c})\\,(\\mathbf{y}\_2' - \\mathbf{y}\_2) + \\cdots = \\sum\_{\\mathbf{y} \\to \\mathbf{y}' \\in \\mathcal{R}} \\mathcal{K}\_{\\mathbf{y} \\to \\mathbf{y}'}(\\mathbf{c})\\,(\\mathbf{y}' - \\mathbf{y})

This split is not immediately obvious! In fact, this is Feinberg's **definition** of a kinetic system \[2\]. The assumption here is that the only way a species can change is through the reaction vectors.

There is also another physical constraint though. In our kinetics, if any of the concentrations of the species in a given reaction rate **K** are zero, then there are no kinetics. This is justified on physical grounds - if there are no reactants then the reaction can't happen.

\\mathcal{K}\_{\\mathbf{y} \\to \\mathbf{y}'}(\\mathbf{c}) > 0 \\;\\iff\\; c\_i > 0 \\text{ for every species } i \\text{ with } y\_i > 0

In addition, since the arrow in a reaction already defines the direction, we can enforce that

\\mathcal{K}\_{\\mathbf{y} \\to \\mathbf{y}'}(\\mathbf{c}) \\;\\ge\\; 0 \\quad \\text{for all } \\mathbf{c} \\in \\mathbb{R}^N\_{\\ge 0}

So for the Lotka network, with a particular kinetics,

\\begin{aligned} X &\\xrightarrow{k\_1} 2X \\\\ X + Y &\\xrightarrow{k\_2} 2Y \\\\ Y &\\xrightarrow{k\_3} 0 \\end{aligned}

our differential equation takes the particular form

\\dot{\\mathbf{c}} = \\underbrace{k\_1\\, c\_X}\_{\\mathcal{K}\_{X \\to 2X}} \\begin{pmatrix} 1 \\\\ 0 \\end{pmatrix} + \\underbrace{k\_2\\, c\_X c\_Y}\_{\\mathcal{K}\_{X+Y \\to 2Y}} \\begin{pmatrix} -1 \\\\ 1 \\end{pmatrix} + \\underbrace{k\_3\\, c\_Y}\_{\\mathcal{K}\_{Y \\to 0}} \\begin{pmatrix} 0 \\\\ -1 \\end{pmatrix}

which in components is

\\begin{aligned} \\dot c\_X &= k\_1\\, c\_X - k\_2\\, c\_X c\_Y \\\\ \\dot c\_Y &= k\_2\\, c\_X c\_Y - k\_3\\, c\_Y \\end{aligned}

We usually solve such systems with a computer by just writing a loop to compute the right hand side in discrete time-steps. The evolution of the components of the concentration vector is shown below. The first is the trajectory in phase space, and the second is a time-series.

[![](https://substackcdn.com/image/fetch/$s_!U5bb!,w_1456,c_limit,f_auto,q_auto:good,fl_lossy/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fce1d4c1b-6a98-4d43-8eaf-de5f1598df18_650x600.gif)](https://substackcdn.com/image/fetch/$s_!U5bb!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fce1d4c1b-6a98-4d43-8eaf-de5f1598df18_650x600.gif)

[![](https://substackcdn.com/image/fetch/$s_!XfBs!,w_1456,c_limit,f_auto,q_auto:good,fl_lossy/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2800679b-3aca-4d96-8bf9-15d76035b661_800x420.gif)](https://substackcdn.com/image/fetch/$s_!XfBs!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2800679b-3aca-4d96-8bf9-15d76035b661_800x420.gif)

Now that we have our dynamics, we can now define **equilibrium** in terms of concentrations. This is when the change in concentration is zero, and the point in concentration space is constant. By setting our concentration equation to zero, we get

0 = \\dot{\\mathbf{c}} = \\sum\_{\\mathbf{y} \\to \\mathbf{y}' \\in \\mathcal{R}} \\mathcal{K}\_{\\mathbf{y} \\to \\mathbf{y}'}(\\mathbf{c}^\*)\\,(\\mathbf{y}' - \\mathbf{y})

If the initial concentration of a chemical reaction network is zero, then trivially, nothing happens. And so we tend to be interested in equilibrium points c\* where each element is positive.

Under the rules of kinetics, this means that each of the K terms in the equilibrium equation are positive. Usually, setting **ċ** to zero gives us an elaborate polynomial function of the elements of **c**, of which we try to find the zero.

\\dot{\\mathbf{c}} = \\mathbf{P}(\\mathbf{c}) = \\begin{pmatrix} P\_1(c\_1, \\dots, c\_N) \\\\ \\vdots \\\\ P\_N(c\_1, \\dots, c\_N) \\end{pmatrix}

\\text{find } \\mathbf{c}^\* \\in \\mathbb{R}^N\_{>0} \\text{ with } \\mathbf{P}(\\mathbf{c}^\*) = \\mathbf{0}

\\mathcal{K}\_{\\mathbf{y} \\to \\mathbf{y}'}(\\mathbf{c}^\*) > 0 \\text{ for all } \\mathbf{y} \\to \\mathbf{y}' \\in \\mathcal{R}

Now that we have definitions for kinetics and equilibria, let's apply it to the most common type of kinetics for a chemical system. A system has **mass action kinetics** when the kinetic term is proportional to the product of concentrations of the reactant species, up to exponents. The exponent for each species is given by the stoichiometric coefficient in front of that.

[![](https://substackcdn.com/image/fetch/$s_!GexH!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F09c23fc2-150c-41b0-91fa-755060bc2079_1285x688.png)](https://substackcdn.com/image/fetch/$s_!GexH!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F09c23fc2-150c-41b0-91fa-755060bc2079_1285x688.png)

Mathematically, our kinetic term for a reaction vector is given by

\\mathcal{K}\_{\\mathbf{y} \\to \\mathbf{y}'}(\\mathbf{c}) = k\_{\\mathbf{y} \\to \\mathbf{y}'} \\prod\_{i=1}^{N} c\_i^{\\,y\_i}

The total kinetics is given by the sum of the mass action products

\\dot{\\mathbf{c}} = \\sum\_{\\mathbf{y} \\to \\mathbf{y}' \\in \\mathcal{R}} k\_{\\mathbf{y} \\to \\mathbf{y}'} \\left( \\prod\_{i=1}^{N} c\_i^{\\,y\_i} \\right) (\\mathbf{y}' - \\mathbf{y})

***Mass action kinetics*** satisfies our general kinetic assumptions above. If any of the concentrations of our reactant species is zero, then the product itself is zero. The function is also monotonic in each of the individual concentrations. If we choose a reactant species and increase its concentration, then the kinetic factor increases as well.

Mass action kinetics are the most common form of kinetics assumed in chemical reaction networks. It's so general that there is a cottage industry of mathematicians trying to decide if a given dynamics problem can be written in terms of mass action kinetics!

Our first example is the simple network where A reacts irreversibly to B. We only have one reaction vector

A \\xrightarrow{k} B, \\qquad \\mathbf{y} = \\begin{pmatrix} 1 \\\\ 0 \\end{pmatrix}, \\quad \\mathbf{y}' = \\begin{pmatrix} 0 \\\\ 1 \\end{pmatrix}, \\quad \\mathbf{y}' - \\mathbf{y} = \\begin{pmatrix} -1 \\\\ 1 \\end{pmatrix}

The reactant vector y has a coefficient 1 for the species A. Under mass action kinetics, our kinetics is just proportional to the *A* concentration with an exponent of 1. And so our kinetic term is

\\mathcal{K}\_{A \\to B}(\\mathbf{c}) = k\\, c\_A^{\\,1} c\_B^{\\,0} = k\\, c\_A

If we substitute we get

\\dot{\\mathbf{c}} = \\begin{pmatrix} \\dot c\_A \\\\ \\dot c\_B \\end{pmatrix} = \\mathcal{K}\_{A \\to B}(\\mathbf{c})\\,(\\mathbf{y}' - \\mathbf{y}) = k\\, c\_A \\begin{pmatrix} -1 \\\\ 1 \\end{pmatrix}

with component equations

\\begin{aligned} \\dot c\_A &= -k\\, c\_A \\\\ \\dot c\_B &= k\\, c\_A. \\end{aligned}

To solve the dynamics, we solve the differential equation. The concentration A has the standard exponential solution, and the second line is solved by integrating the first.

c\_A(t) = c\_A(0)\\, e^{-kt}, \\qquad c\_B(t) = c\_B(0) + c\_A(0)\\left(1 - e^{-kt}\\right)

c\_A(t) + c\_B(t) = c\_A(0) + c\_B(0)

And so the dynamics is given by what we would expect - the concentration of A declines, and tends to zero. Since A is being converted to B, the total concentration of A and B remains constant, since it's the whole mixture.

[![](https://substackcdn.com/image/fetch/$s_!PyNB!,w_1456,c_limit,f_auto,q_auto:good,fl_lossy/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F482fb313-b31a-4eda-b5c9-2df9745d1b26_700x480.gif)](https://substackcdn.com/image/fetch/$s_!PyNB!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F482fb313-b31a-4eda-b5c9-2df9745d1b26_700x480.gif)

As written above, the **equilibrium locus** is when the left hand side of this equation is set to zero. In this case, c\_A is forced to be zero, and c\_B is an arbitrary constant.

\\{\\mathbf{c}^\* : \\dot{\\mathbf{c}} = \\mathbf{0}\\} = \\left\\{ \\begin{pmatrix} 0 \\\\ c\_B \\end{pmatrix} : c\_B \\ge 0 \\right\\}, \\qquad \\lim\_{t \\to \\infty} \\mathbf{c}(t) = \\begin{pmatrix} 0 \\\\ c\_A(0) + c\_B(0) \\end{pmatrix}

If we had the network where the reactant had a stoichiometric coefficient of two, then the kinetics increase with the square of the concentration of the reactant. If we had two monomers of species A reacting irreversibly to a dimer B, then our reaction vector is

2A \\xrightarrow{k} B, \\qquad \\mathbf{y} = \\begin{pmatrix} 2 \\\\ 0 \\end{pmatrix}, \\quad \\mathbf{y}' = \\begin{pmatrix} 0 \\\\ 1 \\end{pmatrix}, \\quad \\mathbf{y}' - \\mathbf{y} = \\begin{pmatrix} -2 \\\\ 1 \\end{pmatrix}

Our kinetics goes as the square of the concentration

\\mathcal{K}\_{2A \\to B}(\\mathbf{c}) = k\\, c\_A^{\\,2} c\_B^{\\,0} = k\\, c\_A^{2}

And in components this gives

\\dot{\\mathbf{c}} = \\begin{pmatrix} \\dot c\_A \\\\ \\dot c\_B \\end{pmatrix} = k\\, c\_A^{2} \\begin{pmatrix} -2 \\\\ 1 \\end{pmatrix}

\\begin{aligned} \\dot c\_A &= -2k\\, c\_A^{2} \\\\ \\dot c\_B &= k\\, c\_A^{2} \\end{aligned}

Again, the equilibrium set of this equation is when c\_A is forced to be zero, and c\_B is an arbitrary constant.

[![](https://substackcdn.com/image/fetch/$s_!Suq3!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F35648829-daea-4819-a874-cc3effd61bc5_939x965.png)](https://substackcdn.com/image/fetch/$s_!Suq3!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F35648829-daea-4819-a874-cc3effd61bc5_939x965.png)

There are more complicated examples of loci of equilibria. Consider the network for a reversible process of a monomer bonding to form a dimer, and back again.

A \\;\\underset{k\_{-1}}{\\overset{k\_1}{\\rightleftharpoons}}\\; 2B

This consists of two separate reactions, and so our system looks like

\\dot{\\mathbf{c}} = \\mathcal{K}\_{A \\to 2B}(\\mathbf{c}) \\begin{pmatrix} -1 \\\\ 2 \\end{pmatrix} + \\mathcal{K}\_{2B \\to A}(\\mathbf{c}) \\begin{pmatrix} 1 \\\\ -2 \\end{pmatrix}

If we assume mass action kinetics we get that

\\dot{\\mathbf{c}} = k\_1\\, c\_A \\begin{pmatrix} -1 \\\\ 2 \\end{pmatrix} + k\_{-1}\\, c\_B^{2} \\begin{pmatrix} 1 \\\\ -2 \\end{pmatrix}

\\begin{aligned} \\dot c\_A &= -k\_1\\, c\_A + k\_{-1}\\, c\_B^{2} \\\\ \\dot c\_B &= 2k\_1\\, c\_A - 2k\_{-1}\\, c\_B^{2} \\end{aligned}

and therefore our equilibrium condition is

k\_1\\, c\_A^\* = k\_{-1}\\, (c\_B^\*)^2

We can write down the equilibrium conditions geometrically on the concentration axis. In the case of the above, we have a curve that is quadratic.

k\_1 c\_A^\* = k\_{-1} (c\_B^\*)^2 \\quad\\Longleftrightarrow\\quad c\_A^\* = \\frac{k\_{-1}}{k\_1}\\,(c\_B^\*)^2

[![](https://substackcdn.com/image/fetch/$s_!TMti!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd43753e3-65d8-445c-b0da-5fb069899dec_963x938.png)](https://substackcdn.com/image/fetch/$s_!TMti!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd43753e3-65d8-445c-b0da-5fb069899dec_963x938.png)

Feinberg \[2\] uses the example of Edelstein's network to illustrate the locus of equilibria in three dimensions. Edelstein's network is

\\begin{aligned} A \\;&\\underset{k\_{-1}}{\\overset{k\_1}{\\rightleftharpoons}}\\; 2A \\\\ A + B \\;&\\underset{k\_{-2}}{\\overset{k\_2}{\\rightleftharpoons}}\\; C \\\\ C \\;&\\underset{k\_{-3}}{\\overset{k\_3}{\\rightleftharpoons}}\\; B \\end{aligned}

k\_1 = 8.5, \\qquad k\_{-1} = k\_2 = k\_{-2} = k\_3 = 1, \\qquad k\_{-3} = 0.2

Setting the left hand side to zero gives us the equilibrium condition

\\begin{aligned} 0 &= k\_1 a - k\_{-1} a^2 - (k\_2 a b - k\_{-2} c) \\\\ 0 &= -(k\_2 a b - k\_{-2} c) + (k\_3 c - k\_{-3} b) \\\\ 0 &= (k\_2 a b - k\_{-2} c) - (k\_3 c - k\_{-3} b) \\end{aligned}

\\Longrightarrow\\qquad \\begin{pmatrix} k\_2 a & -k\_{-2} \\\\ -k\_{-3} & k\_3 \\end{pmatrix} \\begin{pmatrix} b \\\\ c \\end{pmatrix} = \\begin{pmatrix} k\_1 a - k\_{-1} a^2 \\\\ k\_1 a - k\_{-1} a^2 \\end{pmatrix}

Rewriting

\\begin{aligned} b(a) &= \\frac{(k\_3 + k\_{-2})\\,(k\_1 a - k\_{-1} a^2)}{k\_2 k\_3\\, a - k\_{-2} k\_{-3}} \\\\\[1.5ex\] c(a) &= \\frac{(k\_2 a + k\_{-3})\\,(k\_1 a - k\_{-1} a^2)}{k\_2 k\_3\\, a - k\_{-2} k\_{-3}} \\end{aligned}

\\frac{k\_{-2} k\_{-3}}{k\_2 k\_3} < a < \\frac{k\_1}{k\_{-1}}

\\text{locus of equilibria} = \\{\\, (a,\\ b(a),\\ c(a)) : 0.2 < a < 8.5 \\,\\} \\cup \\{(0, 0, 0)\\}

\\text{equilibria in the class } b + c = T:\\qquad b(a) + c(a) = T

Therefore, the set of equilibrium concentrations for the Edelstein network is this worm looking line.

[![](https://substackcdn.com/image/fetch/$s_!666u!,w_1456,c_limit,f_auto,q_auto:good,fl_lossy/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F1f8d7110-c8c5-44bb-8eff-55c23bd5625e_800x700.gif)](https://substackcdn.com/image/fetch/$s_!666u!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F1f8d7110-c8c5-44bb-8eff-55c23bd5625e_800x700.gif)

Suppose we started with an initial concentration and let our reaction evolve. If we integrate our kinetic equation, we only need to integrate the scalar kinetic terms before it

\\mathbf{c}(t) - \\mathbf{c}(0) = \\int\_0^t \\dot{\\mathbf{c}}(\\tau)\\, d\\tau = \\sum\_{\\mathbf{y} \\to \\mathbf{y}' \\in \\mathcal{R}} \\left( \\int\_0^t \\mathcal{K}\_{\\mathbf{y} \\to \\mathbf{y}'}(\\mathbf{c}(\\tau))\\, d\\tau \\right) (\\mathbf{y}' - \\mathbf{y}) \\;\\in\\; S

The form of our kinetics says that it can only evolve within the span of the reaction vectors, because each term is precisely a reaction vector.

The last term is within the stoichiometric subspace. And so

\\mathbf{c}(t) \\in \\mathbf{c}(0) + S \\quad \\text{for all } t \\ge 0

And so this means, given an initial concentration, the set of concentrations that we can reach is the stoichiometric subspace, translated by the initial concentration vector. These translated subspaces are called **stoichiometric compatibility classes**.

Consider the network, with the following stoichiometric subspace.

A \\rightleftharpoons 2B, \\qquad S = \\operatorname{span}\\left\\{ \\begin{pmatrix} -1 \\\\ 2 \\end{pmatrix} \\right\\}, \\qquad s = 1

If we were to start from (1, 1), then the space in which the kinetics can evolve is the line parallel to the stoichiometric subspace, that intersects with (1, 1).

This means, that if we were to start at a concentration c\_0, then the reaction can take us along the line below. It may not take us across the whole line, but constrains where we *could* go. Concentrations can't be negative, and so the compatibility class lies only in the non-negative region.

[![](https://substackcdn.com/image/fetch/$s_!GP6j!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F0b3bd43e-5d0e-47cf-9dc0-7ab90d0e2d45_1047x925.png)](https://substackcdn.com/image/fetch/$s_!GP6j!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F0b3bd43e-5d0e-47cf-9dc0-7ab90d0e2d45_1047x925.png)

And so the stoichiometric compatibility classes look like parallel lines in the non-negative quadrant.

[![](https://substackcdn.com/image/fetch/$s_!XsP4!,w_1456,c_limit,f_auto,q_auto:good,fl_lossy/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fb862f480-df23-47d2-ae46-1de61394adda_650x600.gif)](https://substackcdn.com/image/fetch/$s_!XsP4!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fb862f480-df23-47d2-ae46-1de61394adda_650x600.gif)

If we consider the network given by

\\begin{aligned} A &\\to B \\\\ B &\\to C \\\\ A &\\to C \\end{aligned}

our reaction vectors are given by

\\mathbf{w}\_1 = B - A = \\begin{pmatrix} -1 \\\\ 1 \\\\ 0 \\end{pmatrix}, \\qquad \\mathbf{w}\_2 = C - B = \\begin{pmatrix} 0 \\\\ -1 \\\\ 1 \\end{pmatrix}, \\qquad \\mathbf{w}\_3 = C - A = \\begin{pmatrix} -1 \\\\ 0 \\\\ 1 \\end{pmatrix}

the third reaction vector is a linear combination of the first two,

S = \\operatorname{span}\\{\\mathbf{w}\_1, \\mathbf{w}\_2\\} = \\{(a, b, c) : a + b + c = 0\\}, \\qquad s = 2

the stoichiometric subspace is given by the two dimensional plane.

[![](https://substackcdn.com/image/fetch/$s_!sBJK!,w_1456,c_limit,f_auto,q_auto:good,fl_lossy/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa57db03d-a522-4fbc-bab6-701b97942023_800x700.gif)](https://substackcdn.com/image/fetch/$s_!sBJK!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa57db03d-a522-4fbc-bab6-701b97942023_800x700.gif)

If we start from a given concentration c\_0, then as our concentration evolves, the concentration vector must stay within the plane that contains c\_0. In the animation below, we start at a given point in three dimensions. The plane at that point translated from the stoichiometric subspace is the set of possible concentrations that it can travel in.

[![](https://substackcdn.com/image/fetch/$s_!iV_-!,w_1456,c_limit,f_auto,q_auto:good,fl_lossy/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F6eadbd18-7a96-467b-b449-d774629bcb9d_800x700.gif)](https://substackcdn.com/image/fetch/$s_!iV_-!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F6eadbd18-7a96-467b-b449-d774629bcb9d_800x700.gif)

For different initial concentrations, the classes are translations of this plane. In the image below, the sheets represent the compatibility classes for different initial concentrations.

[![](https://substackcdn.com/image/fetch/$s_!OG7v!,w_1456,c_limit,f_auto,q_auto:good,fl_lossy/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F542f0e5a-b2aa-4857-88e3-09f42f6966fc_800x700.gif)](https://substackcdn.com/image/fetch/$s_!OG7v!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F542f0e5a-b2aa-4857-88e3-09f42f6966fc_800x700.gif)

Now that we have intuition on the geometry, we can move on to some basics of network structure. This will be the final piece of the puzzle before stating our main theorem, the deficiency zero theorem. The first definition we need is the linkage class \[2\]. For example, Feinberg's first network

\\begin{aligned} A &\\rightleftharpoons 2B \\\\ A + C &\\rightleftharpoons D \\\\ D &\\to B + E \\\\ B + E &\\to A + C \\end{aligned}

can be drawn graphically with arrows between the complexes:

[![](https://substackcdn.com/image/fetch/$s_!h1Xv!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F86809a25-067b-47ba-95ec-19cc7bdfde97_1153x458.png)](https://substackcdn.com/image/fetch/$s_!h1Xv!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F86809a25-067b-47ba-95ec-19cc7bdfde97_1153x458.png)

If we remove the arrows, the diagram looks like this.

[![](https://substackcdn.com/image/fetch/$s_!3i9M!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F66977828-702b-4dd7-8ae6-a30c1160e652_1153x458.png)](https://substackcdn.com/image/fetch/$s_!3i9M!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F66977828-702b-4dd7-8ae6-a30c1160e652_1153x458.png)

The **linkage classes** are the sets of complexes that are connected, ignoring the direction of arrows. So Feinberg's first network has two linkage classes. The two linkage classes are shown in different colours. The linkage classes are

\\mathcal{L}\_1 = \\{\\,A,\\ 2B\\,\\}, \\qquad \\mathcal{L}\_2 = \\{\\,A + C,\\ D,\\ B + E\\,\\}, \\qquad \\ell = 2

Which graphically look like:

[![](https://substackcdn.com/image/fetch/$s_!yxQJ!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F47c2d94c-e932-4620-81bb-894f635c747a_1153x458.png)](https://substackcdn.com/image/fetch/$s_!yxQJ!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F47c2d94c-e932-4620-81bb-894f635c747a_1153x458.png)

This network

\\begin{aligned} A &\\rightleftharpoons B \\\\ B &\\rightleftharpoons C \\\\ C &\\rightleftharpoons A \\end{aligned}

has one linkage class because all of the nodes are connected.

[![](https://substackcdn.com/image/fetch/$s_!7071!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F524b23bb-a240-44c8-bcdd-a91c6b85d5ac_758x676.png)](https://substackcdn.com/image/fetch/$s_!7071!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F524b23bb-a240-44c8-bcdd-a91c6b85d5ac_758x676.png)

This network has three linkage classes because none of the reactions share a common complex!

\\begin{aligned} X &\\to 2X \\\\ X + Y &\\to 2Y \\\\ Y &\\to 0 \\end{aligned}

![](https://substackcdn.com/image/fetch/$s_!RCtD!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4dc7c903-9d69-431f-b2f4-de6be655bdec_1351x245.png)

Just like in chemistry, we also have the notion of reversibility in a chemical reaction network. A reaction is **reversible** if, for all **y** and **y′**, if **y → y′**, then **y′ → y**. So the following network is reversible, because B reacts to A when A reacts to B.

A \\rightleftharpoons B

This network is also reversible, since every species that reacts to another species has the inverse included.

\\begin{aligned} A &\\rightleftharpoons B \\\\ B &\\rightleftharpoons C \\end{aligned}

The following network is not reversible

\\begin{aligned} A &\\to B \\\\ B &\\to C \\\\ C &\\to A \\end{aligned}

because even though A reacts to B, **B doesn't react to A directly.** But it turns out that there is a weaker definition. We say a complex y **ultimately reacts** to another complex y' if there is a path of complexes such that

\\mathbf{y} \\to \\mathbf{a}\_1 \\to \\mathbf{a}\_2 \\to \\cdots \\to \\mathbf{a}\_n \\to \\mathbf{y}'

So in this network

\\begin{aligned} X &\\to Y \\\\ Y &\\to Z \\end{aligned}

we find that X → Y → Z, so X ⇒ Z.

[![](https://substackcdn.com/image/fetch/$s_!nnHS!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F0b0460b0-ccda-42f4-bebe-a79f9f8cefeb_1022x497.png)](https://substackcdn.com/image/fetch/$s_!nnHS!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F0b0460b0-ccda-42f4-bebe-a79f9f8cefeb_1022x497.png)

A reaction is weakly reversible if for all y, if y ultimately reacts to y', then y' also ultimately reacts to y.

The reaction below is **weakly reversible**

\\begin{aligned} A &\\to B \\\\ B &\\to C \\\\ C &\\to A \\end{aligned}

But it's **not reversible** because even though A reacts to B, B doesn't react to A.

We now have the idea of a stoichiometric compatibility class and an equilibrium locus. And so for arbitrary kinetics, if we start from a given initial concentration, we know two things.

1. The concentration will evolve somewhere within the stoichiometric compatibility class, and

2. Equilibrium points will lie somewhere on the locus of equilibrium.

This means that for a given starting concentration, ***if*** the **concentration vector does converge to a limit**, it must converge to a point within the geometric intersection of its stoichiometric compatibility class and the locus of equilibria. Note that it is not necessary that the concentration vector converges to a limit at all; we might have a limit cycle.

[![](https://substackcdn.com/image/fetch/$s_!Psld!,w_1456,c_limit,f_auto,q_auto:good,fl_lossy/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fda9a7a62-195d-47c7-918e-7ca1edfaad07_800x700.gif)](https://substackcdn.com/image/fetch/$s_!Psld!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fda9a7a62-195d-47c7-918e-7ca1edfaad07_800x700.gif)

Let's try to calculate these intersection points explicitly for a simple network.

B \\xrightarrow{k\_1} A, \\qquad A + 2B \\xrightarrow{k\_2} 3B

First, calculate our reaction vectors.

A - B = \\begin{pmatrix} 1 \\\\ -1 \\end{pmatrix}, \\qquad 3B - (A + 2B) = \\begin{pmatrix} -1 \\\\ 1 \\end{pmatrix}

The reaction vectors span a line. The stoichiometric compatibility classes are therefore the following line segments.

S = \\operatorname{span}\\left\\{ \\begin{pmatrix} 1 \\\\ -1 \\end{pmatrix} \\right\\}

c\_A + c\_B = T

From the kinetics point of view, mass action kinetics gives

\\begin{aligned} \\dot c\_A &= k\_1 c\_B - k\_2 c\_A c\_B^{2} \\\\ \\dot c\_B &= -k\_1 c\_B + k\_2 c\_A c\_B^{2} \\end{aligned}

To calculate the locus of equilibrium, we set the left hand sides of this equation to zero. This means that our locus of equilibrium is given by the following plot

0 = k\_1 c\_B - k\_2 c\_A c\_B^{2} \\;\\Longrightarrow\\; c\_B = 0 \\quad \\text{or} \\quad c\_A c\_B = \\frac{k\_1}{k\_2}

[![](https://substackcdn.com/image/fetch/$s_!74_w!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F48a6c327-b1f1-45d1-ab12-702d811f0a7a_963x938.png)](https://substackcdn.com/image/fetch/$s_!74_w!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F48a6c327-b1f1-45d1-ab12-702d811f0a7a_963x938.png)

Our stoichiometric classes are given by the set of lines parallel to the stoichiometric subspace, which look like the following

[![](https://substackcdn.com/image/fetch/$s_!_dhy!,w_1456,c_limit,f_auto,q_auto:good,fl_lossy/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2f709752-3e27-4273-a770-9d7cf59c02e1_650x600.gif)](https://substackcdn.com/image/fetch/$s_!_dhy!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2f709752-3e27-4273-a770-9d7cf59c02e1_650x600.gif)

This means that we can either have zero, one or two solutions depending on where the class lies.

[![](https://substackcdn.com/image/fetch/$s_!mPSb!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4fca27f7-570c-4aab-acce-278dd438fa89_963x938.png)](https://substackcdn.com/image/fetch/$s_!mPSb!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4fca27f7-570c-4aab-acce-278dd438fa89_963x938.png)

We can count the number of equilibrium solutions. The stoichiometric classes can be rewritten as the following, for an arbitrary value of *T*

c\_A + c\_B = T

If we substitute that into our equilibrium locus, we get

(T - c\_B)\\,c\_B = \\frac{k\_1}{k\_2}

This is a quadratic equation with solution

c\_B = \\frac{T \\pm \\sqrt{T^2 - 4k\_1/k\_2}}{2}

The concentration can't be imaginary, which means the term in the square root can't be negative. And so for a solution to exist we require that

T^2 \\ge \\frac{4k\_1}{k\_2}

And also, T can't be negative since our stoichiometric classes only live in the positive section. This leads us to the only equilibrium condition being

T \\ge 2\\sqrt{\\frac{k\_1}{k\_2}}

Solving a quadratic equation is easy. But unfortunately, most network systems are so complex that we can't easily get the number of equilibria. For example, suppose we had the horrible looking mass action kinetic equation

\\begin{aligned} \\dot a &= k\_1 a - k\_{-1} a^2 - k\_2 a b + k\_{-2} c \\\\ \\dot b &= -k\_2 a b + k\_{-2} c + k\_3 c - k\_{-3} b \\\\ \\dot c &= k\_2 a b - k\_{-2} c - k\_3 c + k\_{-3} b \\end{aligned}

How many equilibria are there in a given stoichiometric class, and what is their nature? It's unclear, and to do this we would have to solve some pretty horrible equations, with little general insight into what makes these systems special. This is where the deficiency zero theorem comes in, and it's the final result of this essay.

The *deficiency zero theorem* allows us to make meaningful statements about the number of equilibria in a network, under some conditions \[2\]\[28\]. The *deficiency* of a network is given by its number of complexes, minus the number of linkage classes, minus the dimension of the stoichiometric subspace it has. This is often denoted by the Greek letter delta.

\\delta = \\underbrace{n}\_{\\text{complexes}} \\;-\\; \\underbrace{\\ell}\_{\\text{linkage classes}} \\;-\\; \\underbrace{s}\_{\\dim S}

The deficiency zero theorem says that if we have a deficiency zero network, then two things could happen, based on whether the network is weakly reversible or not.

If a network is not weakly reversible, then for any kinetics, the system doesn't have a positive equilibrium or a cyclic trajectory where all species concentrations are positive. By positive equilibrium, it means an equilibrium concentration vector in which every element is strictly positive. It still could in theory converge to equilibria where one component is zero.

[![](https://substackcdn.com/image/fetch/$s_!5SAm!,w_1456,c_limit,f_auto,q_auto:good,fl_lossy/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc19254ea-ae0d-4836-961d-0adba371dd84_1100x520.gif)](https://substackcdn.com/image/fetch/$s_!5SAm!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc19254ea-ae0d-4836-961d-0adba371dd84_1100x520.gif)

But suppose it **is** weakly reversible. Then for mass action kinetics, we have that each positive stoichiometric compatibility class has precisely one equilibrium, that equilibrium is **asymptotically stable**; and there is no nontrivial cyclic composition trajectory along which all species concentrations are positive.

***Asymptotically stable*** means that if we start at a concentration point near enough the equilibrium point, then the trajectory of the concentration vector should converge to that equilibrium point. What the statement does not say is whether this is true for an arbitrary concentration point; it is still possible that the concentration doesn't converge at all to either a point or a cycle. The **global attractor conjecture** however posits that arbitrary starting concentrations will tend to equilibria in the long time limit, but this is still unproven.

For example, the reversible monomer to dimer reaction is weakly reversible, has deficiency zero, and so our equilibrium locus only intersects **once** with our stoichiometric compatibility classes.

[![](https://substackcdn.com/image/fetch/$s_!4SNL!,w_1456,c_limit,f_auto,q_auto:good,fl_lossy/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F6a6b1c04-4e50-46ba-9db5-82c9add633c9_700x620.gif)](https://substackcdn.com/image/fetch/$s_!4SNL!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F6a6b1c04-4e50-46ba-9db5-82c9add633c9_700x620.gif)

This can be summarised in this tree diagram.

[![](https://substackcdn.com/image/fetch/$s_!FVXq!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F47181b8f-3115-4f3a-9f72-44a904b8ec7b_1483x845.png)](https://substackcdn.com/image/fetch/$s_!FVXq!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F47181b8f-3115-4f3a-9f72-44a904b8ec7b_1483x845.png)

Let's take Feinberg's first toy network \[2\]. It looks really complicated, and it's not clear at all what equilibria this should have, if any!

\\begin{aligned} A &\\rightleftharpoons 2B \\\\ A + C &\\rightleftharpoons D \\\\ D &\\to B + E \\\\ B + E &\\to A + C \\end{aligned}

But let's compute the deficiency of this network and see where it can help us. We can count five complexes

\\{\\,A,\\ 2B,\\ A + C,\\ D,\\ B + E\\,\\}, \\qquad n = 5

Our reaction vectors are

\\begin{aligned} \\mathbf{u}\_1 &= 2B - A &&= \\begin{pmatrix} -1 \\\\ 2 \\\\ 0 \\\\ 0 \\\\ 0 \\end{pmatrix} \\\\\[1ex\] \\mathbf{u}\_2 &= D - (A + C) &&= \\begin{pmatrix} -1 \\\\ 0 \\\\ -1 \\\\ 1 \\\\ 0 \\end{pmatrix} \\\\\[1ex\] \\mathbf{u}\_3 &= (B + E) - D &&= \\begin{pmatrix} 0 \\\\ 1 \\\\ 0 \\\\ -1 \\\\ 1 \\end{pmatrix} \\\\\[1ex\] \\mathbf{u}\_4 &= (A + C) - (B + E) &&= \\begin{pmatrix} 1 \\\\ -1 \\\\ 1 \\\\ 0 \\\\ -1 \\end{pmatrix} \\end{aligned}

In our last section on stoichiometric subspaces, we calculated the rank of these reaction vectors by reducing them to row echelon form. We found that the row echelon form only contained three non-zero rows. That means that the dimension of the stoichiometric subspace is 3.

s = \\dim \\operatorname{span}\\{\\mathbf{u}\_1, \\mathbf{u}\_2, \\mathbf{u}\_3, \\mathbf{u}\_4\\} = \\operatorname{rank} N = 3

By inspection, we have two linkage classes shown below.

\\mathcal{L}\_1 = \\{\\,A,\\ 2B\\,\\}, \\qquad \\mathcal{L}\_2 = \\{\\,A + C,\\ D,\\ B + E\\,\\}, \\qquad \\ell = 2

[![](https://substackcdn.com/image/fetch/$s_!yxQJ!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F47c2d94c-e932-4620-81bb-894f635c747a_1153x458.png)](https://substackcdn.com/image/fetch/$s_!yxQJ!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F47c2d94c-e932-4620-81bb-894f635c747a_1153x458.png)

The deficiency of this network is given by subtracting 2, the number of linkage classes, and 3, the dimension of the stoichiometric subspace, from 5, the number of complexes. This gives us a deficiency of zero.

\\delta = n - \\ell - s = 5 - 2 - 3 = 0

And it's easy to check that the reaction network is weakly reversible. The deficiency zero theorem then tells us that we only have **one positive equilibrium** per stoichiometric compatibility class. This is a **super cool** statement, because the original mass action kinetics for Feinberg's first network look really complicated and messy!

\\begin{aligned} \\dot c\_A &= -k\_1 c\_A + k\_{-1} c\_B^{2} - k\_2 c\_A c\_C + k\_{-2} c\_D + k\_4 c\_B c\_E \\\\ \\dot c\_B &= 2k\_1 c\_A - 2k\_{-1} c\_B^{2} + k\_3 c\_D - k\_4 c\_B c\_E \\\\ \\dot c\_C &= -k\_2 c\_A c\_C + k\_{-2} c\_D + k\_4 c\_B c\_E \\\\ \\dot c\_D &= k\_2 c\_A c\_C - k\_{-2} c\_D - k\_3 c\_D \\\\ \\dot c\_E &= k\_3 c\_D - k\_4 c\_B c\_E \\end{aligned}

Let's check our previous network

B \\xrightarrow{k\_1} A, \\qquad A + 2B \\xrightarrow{k\_2} 3B

In our example, we found that stoichiometric compatibility classes could contain zero, one or two equilibria. It has deficiency 1.

\\{\\,B,\\ A,\\ A + 2B,\\ 3B\\,\\}, \\qquad n = 4

\\mathcal{L}\_1 = \\{\\,B,\\ A\\,\\}, \\qquad \\mathcal{L}\_2 = \\{\\,A + 2B,\\ 3B\\,\\}, \\qquad \\ell = 2

s = 1

\\delta = n - \\ell - s = 4 - 2 - 1 = 1

Similarly for the Michaelis–Menten system \[29\], we have

E + S \\;\\underset{k\_{-1}}{\\overset{k\_1}{\\rightleftharpoons}}\\; ES \\;\\xrightarrow{k\_2}\\; E + P

Our complexes are

\\{\\,E + S,\\ ES,\\ E + P\\,\\}, \\qquad n = 3

Our reaction vectors are

\\begin{aligned} ES - (E + S) &= \\begin{pmatrix} -1 \\\\ -1 \\\\ 1 \\\\ 0 \\end{pmatrix} \\\\\[1ex\] (E + S) - ES &= \\begin{pmatrix} 1 \\\\ 1 \\\\ -1 \\\\ 0 \\end{pmatrix} \\\\\[1ex\] (E + P) - ES &= \\begin{pmatrix} 1 \\\\ 0 \\\\ -1 \\\\ 1 \\end{pmatrix} \\end{aligned}

\\text{in the order } (E,\\ S,\\ ES,\\ P)

s = \\dim \\operatorname{span}\\left\\{ \\begin{pmatrix} -1 \\\\ -1 \\\\ 1 \\\\ 0 \\end{pmatrix}, \\begin{pmatrix} 1 \\\\ 0 \\\\ -1 \\\\ 1 \\end{pmatrix} \\right\\} = 2

And we can count the linkage classes below

\\mathcal{L}\_1 = \\{\\,E + S,\\ ES,\\ E + P\\,\\}, \\qquad \\ell = 1

[![](https://substackcdn.com/image/fetch/$s_!onJ0!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F174cdd57-dee1-482d-a0f3-6a84a7b2d572_947x348.png)](https://substackcdn.com/image/fetch/$s_!onJ0!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F174cdd57-dee1-482d-a0f3-6a84a7b2d572_947x348.png)

This means that our deficiency of this network is given by

\\delta = n - \\ell - s = 3 - 1 - 2 = 0

However, it's not weakly reversible because E+S ultimately reacts to E+P, but E+P doesn't ultimately react to E+S. And so, it can't have an equilibrium concentration that is strictly positive in all its components.

Here is a table of the deficiencies of other chemical reaction networks we've talked about!

[![](https://substackcdn.com/image/fetch/$s_!dPeD!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fef148587-16ac-4a2c-b5bb-8f986299bc78_1746x1710.png)](https://substackcdn.com/image/fetch/$s_!dPeD!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fef148587-16ac-4a2c-b5bb-8f986299bc78_1746x1710.png)

I hope this piece provides some tools to reason geometrically about chemical systems. My next experiments aim to test some of these theorems physically.

To internalise this content even more, my approach was to get Claude to repeatedly drill me on different kinds of chemical reaction networks. I intentionally added a chapter on vocabulary to practice my muscle memory on picking out features like linkage networks, complexes and stoichiometric subspaces. I wrote a quiz [here](https://claude.ai/artifact/MrpAUjgAN81KFwHTQjkAzZ), and if you want a deeper intuition I suggest you try some of the problems!

[![](https://substackcdn.com/image/fetch/$s_!pwxp!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd869b87e-a945-4bab-a293-bd7d99180d22_941x990.png)](https://substackcdn.com/image/fetch/$s_!pwxp!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd869b87e-a945-4bab-a293-bd7d99180d22_941x990.png)

*Thank you to [David Jordan](https://www.livingphysics.org/) for continued support.*

\[1\] Build-a-Cell. "Synthetic cells 101". https://www.buildacell.org/101 (accessed 2026-11-10).

\[2\] Feinberg, M. (2019). *Foundations of Chemical Reaction Network Theory*. Applied Mathematical Sciences, vol. 202. Springer, Cham. https://doi.org/10.1007/978-3-030-03858-8

\[3\] Belousov, B. P. (1959). "A periodic reaction and its mechanism". *Sbornik Referatov po Radiatsionnoi Meditsine za 1958*, 145–147. Medgiz, Moscow. Zhabotinsky, A. M. (1964). "Periodic processes of malonic acid oxidation in a liquid phase". *Biofizika* **9**, 306–311. Field, R. J., Körös, E. & Noyes, R. M. (1972). "Oscillations in chemical systems. II. Thorough analysis of temporal oscillation in the bromate–cerium–malonic acid system". *J. Am. Chem. Soc.* **94** (25), 8649–8664.

\[4\] Briggs, T. S. & Rauscher, W. C. (1973). "An oscillating iodine clock". *J. Chem. Educ.* **50** (7), 496.

\[5\] Noyes, R. M. & Furrow, S. D. (1982). "The oscillatory Briggs–Rauscher reaction. 3. A skeleton mechanism for oscillations". *J. Am. Chem. Soc.* **104** (1), 45–48.

\[6\] De Kepper, P. & Epstein, I. R. (1982). "A mechanistic study of oscillations and bistability in the Briggs–Rauscher reaction". *J. Am. Chem. Soc.* **104** (1), 49–55.

\[7\] Schmitz, G. & Furrow, S. D. (2016). "Kinetics of the iodate reduction by hydrogen peroxide and relation with the Briggs–Rauscher and Bray–Liebhafsky oscillating reactions". *Phys. Chem. Chem. Phys.* **18**, 7318–7326.

\[8\] Hu, G., Pojman, J. A., Scott, S. K., Wrobel, M. M. & Taylor, A. F. (2010). "Base-catalyzed feedback in the urea–urease reaction". *J. Phys. Chem. B* **114** (44), 14059–14063.

\[9\] Liesegang, R. E. (1896). "Ueber einige Eigenschaften von Gallerten". *Naturwissenschaftliche Wochenschrift* **11** (30), 353–362.

\[10\] Lotka, A. J. (1910). "Contribution to the theory of periodic reactions". *J. Phys. Chem.* **14** (3), 271–274.

\[11\] Lotka, A. J. (1920). "Undamped oscillations derived from the law of mass action". *J. Am. Chem. Soc.* **42** (8), 1595–1599.

\[12\] Krebs, H. A. & Johnson, W. A. (1937). "The role of citric acid in intermediate metabolism in animal tissues". *Enzymologia* **4**, 148–156.

\[13\] Bassham, J. A., Benson, A. A., Kay, L. D., Harris, A. Z., Wilson, A. T. & Calvin, M. (1954). "The path of carbon in photosynthesis. XXI. The cyclic regeneration of carbon dioxide acceptor". *J. Am. Chem. Soc.* **76** (7), 1760–1770.

\[14\] Duysens, L. N. M. & Amesz, J. (1957). "Fluorescence spectrophotometry of reduced phosphopyridine nucleotide in intact cells in the near-ultraviolet and visible region". *Biochim. Biophys. Acta* **24** (1), 19–26.

\[15\] Higgins, J. (1964). "A chemical mechanism for oscillation of glycolytic intermediates in yeast cells". *Proc. Natl. Acad. Sci. USA* **51** (6), 989–994. Sel'kov, E. E. (1968). "Self-oscillations in glycolysis. 1. A simple kinetic model". *Eur. J. Biochem.* **4** (1), 79–86.

\[16\] Rendall, A. D. (2012). "Albert Goldbeter and glycolytic oscillations". *Hydrobates* blog, 21 Jan 2012. https://alanrendall.wordpress.com/2012/01/21/albert-goldbeter-and-glycolytic-oscillations/

\[17\] Nakajima, M., Imai, K., Ito, H., Nishiwaki, T., Murayama, Y., Iwasaki, H., Oyama, T. & Kondo, T. (2005). "Reconstitution of circadian oscillation of cyanobacterial KaiC phosphorylation in vitro". *Science* **308** (5720), 414–415. Rust, M. J., Markson, J. S., Lane, W. S., Fisher, D. S. & O'Shea, E. K. (2007). "Ordered phosphorylation governs oscillation of a three-protein circadian clock". *Science* **318** (5851), 809–812.

\[18\] Hell, J. & Rendall, A. D. (2015). "A proof of bistability for the dual futile cycle". *Nonlinear Anal. Real World Appl.* **24**, 175–189. arXiv:1404.0394.

\[19\] Gardner, T. S., Cantor, C. R. & Collins, J. J. (2000). "Construction of a genetic toggle switch in *Escherichia coli* ". *Nature* **403**, 339–342. Ferrell, J. E. (2002). "Self-perpetuating states in signal transduction: positive feedback, double-negative feedback and bistability". *Curr. Opin. Cell Biol.* **14** (2), 140–148.

\[20\] Craciun, G., Tang, Y. & Feinberg, M. (2006). "Understanding bistability in complex enzyme-driven reaction networks". *Proc. Natl. Acad. Sci. USA* **103** (23), 8697–8702.

\[21\] Waddington, C. H. (1956). *Principles of Embryology*. George Allen & Unwin, London. Waddington, C. H. (1957). *The Strategy of the Genes*. George Allen & Unwin, London.

\[22\] Huang, C.-Y. F. & Ferrell, J. E. (1996). "Ultrasensitivity in the mitogen-activated protein kinase cascade". *Proc. Natl. Acad. Sci. USA* **93** (19), 10078–10083.

\[23\] Stadler, P. F. (2023). Presentation at the Santa Fe Institute \[video\]. YouTube, https://www.youtube.com/watch?v=Cj8ybe6LKQ4 (entry added 2026-11-15; the video title could not be fetched to check, description taken from the essay text).

\[24\] Baez, J. C. & Biamonte, J. D. (2018). *Quantum Techniques in Stochastic Mechanics*. World Scientific, Singapore. arXiv:1209.3632. (Petri nets as reaction networks, chs. 1–2.)

\[25\] Baez, J. C. (2011). "Network theory (part 17)". *Azimuth* blog, 12 Nov 2011. https://johncarlosbaez.wordpress.com/2011/11/12/network-theory-part-17/

\[26\] Edelstein, B. B. (1970). "Biochemical model with multiple steady states and hysteresis". *J. Theor. Biol.* **29** (1), 57–62.

\[27\] Field, R. J. & Noyes, R. M. (1974). "Oscillations in chemical systems. IV. Limit cycle behavior in a model of a real chemical reaction". *J. Chem. Phys.* **60** (5), 1877–1884.

\[28\] Feinberg, M. (1987). "Chemical reaction network structure and the stability of complex isothermal reactors—I. The deficiency zero and deficiency one theorems". *Chem. Eng. Sci.* **42** (10), 2229–2268.

\[29\] Michaelis, L. & Menten, M. L. (1913). "Die Kinetik der Invertinwirkung". *Biochem. Z.* **49**, 333–369.

Not cited in the text (kept from the earlier list):

\[30\] Zaikin, A. N. & Zhabotinsky, A. M. (1970). "Concentration wave propagation in two-dimensional liquid-phase self-oscillating system". *Nature* **225**, 535–537.

\[31\] Winfree, A. T. (1972). "Spiral waves of chemical activity". *Science* **175** (4022), 634–636.

\[32\] Alberty, R. A. & Goldberg, R. N. (1992). "Standard thermodynamic formation properties for the adenosine 5'-triphosphate series". *Biochemistry* **31** (43), 10610–10615.

\[33\] Krajewska, B. (2009). "Ureases I. Functional, catalytic and kinetic properties: A review". *J. Mol. Catal. B: Enzym.* **59** (1–3), 9–21.

\[34\] Feliu, E., Rendall, A. D. & Wiuf, C. (2020). "A proof of unlimited multistability for phosphorylation cycles". *Nonlinearity* **33** (11), 5629–5658. arXiv:1904.02983.

\[35\] Chang, R. L., Ghamsari, L., Manichaikul, A., Hom, E. F. Y., Balaji, S., Fu, W., Shen, Y., Hao, T., Palsson, B. Ø., Salehi-Ashtiani, K. & Papin, J. A. (2011). "Metabolic network reconstruction of *Chlamydomonas* offers insight into light-driven algal metabolism". *Mol. Syst. Biol.* **7**, 518.

\[36\] Kowald, A. & Kirkwood, T. B. L. (1996). "A network theory of ageing: the interactions of defective mitochondria, aberrant proteins, free radicals and scavengers in the ageing process". *Mutat. Res.* **316** (5–6), 209–236.

\[37\] Gladyshev, V. N. (2016). "Aging: progressive decline in fitness due to the rising deleteriome adjusted by genetic, environmental, and stochastic processes". *Aging Cell* **15** (4), 594–602.

\[38\] Harman, D. (1956). "Aging: a theory based on free radical and radiation chemistry". *J. Gerontol.* **11** (3), 298–300.

\[39\] Kirkwood, T. B. L. (1977). "Evolution of ageing". *Nature* **270**, 301–304.

\[40\] Prigogine, I. & Lefever, R. (1968). "Symmetry breaking instabilities in dissipative systems. II". *J. Chem. Phys.* **48** (4), 1695–1700.

\[41\] Banaji, M. & Boros, B. (2023). "The smallest bimolecular mass action reaction networks admitting Andronov–Hopf bifurcation". *Nonlinearity* **36** (2), 1398–1433.

\[42\] Soloveichik, D., Seelig, G. & Winfree, E. (2010). "DNA as a universal substrate for chemical kinetics". *Proc. Natl. Acad. Sci. USA* **107** (12), 5393–5398.

\[43\] Srinivas, N., Parkin, J., Seelig, G., Winfree, E. & Soloveichik, D. (2017). "Enzyme-free nucleic acid dynamical systems". *Science* **358** (6369), eaal2052.

\[44\] McCauley, E., Nisbet, R. M., Murdoch, W. W., de Roos, A. M. & Gurney, W. S. C. (1999). "Large-amplitude cycles of Daphnia and its algal prey in enriched environments". *Nature* **402**, 653–656.

\[45\] Poole, W., Pandey, A., Shur, A., Tuza, Z. A. & Murray, R. M. (2022). "BioCRNpyler: Compiling chemical reaction networks from biomolecular parts in diverse contexts". *PLoS Comput. Biol.* **18** (4), e1009987.

\[46\] Feinberg, M. (1979). *Lectures on Chemical Reaction Networks*. Mathematics Research Center, University of Wisconsin–Madison. (Earlier lecture-note version of \[2\]; opens with the same toy network.)

**¹** We could build chemical reactions with arbitrary properties at lower cost. For example, if we understood the chemistry of bistable switches, I might be able to make a cheap chemical switch at home. Because reagants are expensive, understanding minimal reaction networks that yield interesting effects would allow me to do experiments with less resources.

**²** Chemical reaction network theory is currently being used at the build-a-cell project, where mass action kinetics is claimed to be an expressive way of doing things \[1\].

**³**

\\begin{aligned} A \\;&\\underset{k\_{-1}}{\\overset{k\_1}{\\rightleftharpoons}}\\; 2A \\\\ A + B \\;&\\underset{k\_{-2}}{\\overset{k\_2}{\\rightleftharpoons}}\\; C \\\\ C \\;&\\underset{k\_{-3}}{\\overset{k\_3}{\\rightleftharpoons}}\\; B \\end{aligned}
