---
title: 'A Man from the Future: Why Johnny Can''t Stop Abstracting'
link: http://muratbuffalo.blogspot.com/2026/10/the-man-from-future-why-johnny-cant.html
source: hnrss-org
published: 2026-10-26T17:49:55Z
updated: 2026-10-26T17:49:55Z
first_seen: 2026-10-26T23:45:44.074426996Z
authors:
- aray07
labels:
- book-review
content: extracted
html: 2026-10-26-a-man-from-the-future-why-johnny-can-t-stop-abstracting.html
remote_preview:
  url: https://blogger.googleusercontent.com/img/a/AVvXsEgUV8paJsfBDCdaizCxyZLf_tgGZc_L1c6HYhST-bupV3snzItwcj-3TzIJnc20dDeTmoFeMaEHxyZpxrqsgI80le-78m0BN7QqcMGPBkhwTLTu-BaWTc0DQ9rpXsag-tonr1n-TYZXz2sRYCk7HQaB94VGFboZ8sPvIL9_mIPGhBpOqL3B_42ueVNyq9o=w144-h144-p-k-no-nu
  alt: Image
---

> *I have come to suspect that to most people thinking is painful. Some of us are addicted to thinking. Some of us find it a necessity. Johnny enjoyed it. I even have the suspicion that he enjoyed almost nothing else.*
>
> *-- Edward Teller, eulogizing John von Neumann*

I listened to Ananyo Bhattacharya's "[The Man from the Future: The Visionary Life of John von Neumann](https://wwnorton.com/books/the-man-from-the-future)" in my car during the many commute hours Bay Area traffic afforded me. I thought I should share the Johnny phenomena here.

I knew about von Neumann being a very influential mathematician/scientist/engineer but the full list still left me bedazzled. In his 53 years of life (1903-1957), von Neumann seeded so many fields. [Every time there was an important discovery in physics, mathematics or computation, John von Neumann was there.](https://www.youtube.com/watch?v=17mNnWn2AsM) Set theory and the foundations of mathematics. The mathematical formulation of quantum mechanics. Operator algebras (still called von Neumann algebras). Ergodic theory. Game theory. Expected utility theory (which is the foundation of most of modern mathematical economics). The Monte Carlo method. The stored-program computer architecture that every machine you have ever touched is built on. Numerical weather prediction. Fault-tolerant computing. Self-reproducing automata (cellular automata), and arguably, the logical structure of molecular biology, four years before Watson and Crick. There is also, unfortunately, the implosion lens for the Nagasaki bomb, and a decade of hawkish nuclear strategy.

## Early years

At six, Janos (the OG hungarian name for Johnny) could divide eight-digit numbers in his head and hold a conversation in ancient Greek. He later added Latin, French, German, English, and Yiddish.  The rumor was that he remembered everything he ever read. Many years later Herman Goldstine decided to test this by asking him to recite "A Tale of Two Cities." Von Neumann started on chapter one and had to be asked to stop fifteen minutes later.

Budapest around 1900 produced an absurd cluster of minds: von Neumann, Eugene Wigner, Edward Teller, Leo Szilard, Paul Erdős, Theodore von Kármán, George Pólya. When several of them turned up at Los Alamos and realized they had gone to high school together, Szilard joked that Hungary was a cover story for a Martian landing. But even inside [the Martian cluster](https://en.wikipedia.org/wiki/The_Martians_\(scientists\)), von Neumann was the outlier. Wigner, a Nobel laureate himself, was asked at Los Alamos to explain the Hungarian phenomenon and said there is only one phenomenon that is worthy of explanation, and that phenomenon is John von Neumann. The cluster may be less mysterious than it looks: Budapest was one of the few genuinely welcoming places in Europe for Jewish families, and it pushed them toward the professions the nobility considered beneath them, mathematics among them. The schools then did the rest, with a nationwide math competition and top quality high school teachers.

As for the Johnny phenomenon, from the pedagogical perspective, the following stood out to me from Johnny's early years. The first is the dinner table. Von Neumann's father Miksa (Max), a lawyer turned banker so successful that the Emperor ennobled him for service to the national economy, ran the evening meal as a seminar. He would start by going through his own day: the cases he argued, the deals he negotiated, whatever was on his mind. Then each of the three boys would present on what they had been reading. The whole family would then argue about Heine's poetry, or antisemitism in Europe, or the paradoxes of omnipotence. Sometimes Max would bring home business clients, and the boys were allowed to grill them about whatever they wanted funded. Frequent guests included the director of Budapest's Theoretical Physics Institute and one of Freud's students.

The second is the languages. Max knew the good times could end, told his sons so explicitly, and wanted them able to survive anywhere in any language. He was preparing them to flee. He was right. When John was fifteen, the communists took over Hungary and targeted rich families like his. A few months later the counterrevolutionaries beat the communists and then massacred thousands of Jews for supposedly collaborating. The von Neumanns survived by leaving the country at the right moments, and possibly by being too rich.

Johnny's first ten years included no formal schooling at all, because organized primary schools were not a tradition there yet. He had governesses, private teachers, and his father's enormous library, and he apparently used it to memorize a 44-volume German history of the world, which he would later deploy to freak people out at parties.

Finally, when he did get to the Fasori Gymnasium at eleven, the math teacher, László Rátz, concluded fairly quickly that he could not teach this kid and arranged a private tutor. The tutor was Gábor Szegő, who went on to chair the math department at Stanford. Szegő is said to have come away from the first lessons in tears at what the boy could already do. The results of this tutelage showed up a couple years later. At seventeen Johnny gave a new definition of the ordinal numbers, where each number simply is the set of all the numbers before it. Zero is the empty set. One is the set containing zero. Two is the set containing zero and one. And so on up into the infinite ordinals, with nothing but the empty set and a pair of braces to build from. If you think about it, this is a very computer-science-y way to define numbers: an inductive datatype with one constructor.

He pushed this further in his doctoral thesis a few years later. Set theory at the time was still suffering from the paradoxes, Russell's set of all sets that do not contain themselves being the famous one. Johnny's fix was essentially a type system for sets: the ordinary ones you can put inside other sets, and the oversized ones you can talk about but never use as an element. The paradox then becomes a type error. Note that it is the same abstraction move in both times: take an object everyone thinks they understand, and replace it with the smallest thing that behaves the same way.

Johnny kept doing the same thing in field after field for the next thirty years: arrive, find the smallest set of objects and axioms the mess is actually about, write them down so that everyone can finally argue about the right thing, and then leave before the theorems fell out. Teller said Johnny enjoyed thinking, but I think the addiction was much more specialized than that. His real superpower was "[the art of abstraction](https://muratbuffalo.blogspot.com/2023/09/beyond-code-tla-and-art-of-abstraction.html)".

Look at the list from the introduction again with this in mind. Matrix mechanics and wave mechanics looked like rival theories until he noticed they were two coordinate systems on the same Hilbert space. Economists thought comparing preferences across people was hopeless until he asked what minimal consistency axioms a chooser has to satisfy, and derived a cardinal utility scale from them in an afternoon. Self-reproduction looked like it needed an infinite recursion of blueprints until he separated "read the tape as instructions" from "copy the tape as data."

## Making quantum mechanics rigorous

Having finished his PhD at 21, Johnny arrived at Göttingen at 22 because it was the world capital of mathematics, under Hilbert. The timing couldn't have been better. Heisenberg, one year older, was down the hall having just invented matrix mechanics. A year later Schrödinger produced wave mechanics in Vienna. The two gave the same answers and implied completely different pictures of reality, and physicists were uneasy with both: matrices were unfamiliar mathematics then, and nobody could say what the waves were. Von Neumann's first move was to prove the two were the same theory in different forms. Then he wrote "Mathematical Foundations of Quantum Mechanics" (1932), which set the whole thing up rigorously in Hilbert space. The author of the book Bhattacharya, who did physics as an undergrad, points out that this is still the most watertight formulation we have.

Once the math was clean, the measurement problem became more visible. Where does the wave function actually collapse? At the apparatus, the detector, the retina, or the conscious observer? Von Neumann declined to take a position, but proved it does not matter mathematically where you draw the line because the answer comes out the same. That line is now called the von Neumann cut.

## Game theory and economics

Nothing much came of game theory until von Neumann proved the minimax theorem in 1928, in his early twenties. Every two-player zero-sum game has a solution: either a strategy one player can pursue to win, or a mixed strategy with randomness in it (e.g., rock-paper-scissors). Then he lost interest and wandered off, as was his habit.

He came back when Oskar Morgenstern turned up at Princeton. Morgenstern, an economist, was stuck on what is essentially a liveness bug in economic modeling: you intervene in an economy, people adjust their behavior in response, which invalidates the model you used to design the intervention, so you build a new model, which they also adjust to. An infinite loop. Morgenstern had concluded that all attempts to model economies must fail, which made him unpopular right up until the Wall Street crash made him look prescient. Von Neumann had the tools to attack the problem. Over about eighteen months they produced the 600-page "Theory of Games and Economic Behavior" (1944) and founded the field. Johnny did this as a hobby. During those same years he was designing implosion lenses at Los Alamos and building the first programmable computers. He would drop in on Morgenstern for a long session and then vanish back into the rest of his life.

Two things from this work are still everywhere. The first is the axiomatization of expected utility. Putting human preferences on a cardinal scale and comparing them across people was considered more or less impossible, and von Neumann sat down one afternoon and did it. This relates uncomfortably with [the longtermism material from the Cal Newport/Émile Torres conversation I wrote up recently](https://muratbuffalo.blogspot.com/2026/10/what-do-frontier-labs-actually-believe.html). Expected value math is a hell of a drug.

The second is its limited model. His framework is built on zero-sum games and on coalitions: given n players, which coalitions are stable? Most of his book is about that, but a lot of the world is not zero-sum. Nash's [non-cooperative equilibrium](https://en.wikipedia.org/wiki/Non-cooperative_game_theory), which generalizes minimax, is what opened that up, and von Neumann rejected it essentially out of hand when Nash walked into his office. To him it was a trivial step, a fixed-point argument he could see through instantly. He misjudged the importance of the non-cooperative approach. Bhattacharya flags a nice irony here: von Neumann, the alleged Cold War hawk and the model for Dr. Strangelove, built a theory centered on people talking to each other and forming working coalitions. It was Nash, who had trouble with people, who formalized everyone optimizing alone. [All models are wrong](https://en.wikipedia.org/wiki/All_models_are_wrong), but some are useful.

## The computer

In 1944 Johnny ran into Herman Goldstine on a train platform at Aberdeen, found out about the ENIAC, and attached himself to the project. In 1945 he wrote the "[First Draft of a Report on the EDVAC](https://en.wikipedia.org/wiki/First_Draft_of_a_Report_on_the_EDVAC)", laying out the stored-program architecture: one memory holding both instructions and data, a control unit, an arithmetic unit, I/O. That is now the architecture of virtually all computers, from the desktops to smartphones, eighty years later.

Von Neumann was adamant that the technical details go into the public domain, and circulating that draft is exactly what made the architecture unpatentable. So Johnny was also an early proponent of Open Source. He did the same with the IAS machine design, which got copied worldwide and produced a run of machines with goofy names: MANIAC at Los Alamos, JOHNNIAC at RAND, ILLIAC at Illinois, SILLIAC in Sydney, WEIZAC in Israel. For a man who gets painted as the cold rationalist hawk, putting the foundational design of the computing industry into the commons on purpose is a generous act.

He also, with Ulam, invented the [Monte Carlo method](https://en.wikipedia.org/wiki/Monte_Carlo_method), then needed random numbers for it and had to invent pseudorandom generation. And he put the first serious application on these machines himself. The IAS Meteorology Project ran the first computerized weather forecast in 1950. Numerical weather prediction, and by extension climate modeling, is a von Neumann production.

## Reliable organisms from unreliable components

In 1952 von Neumann gave a lecture series at Caltech titled "Probabilistic Logics and the Synthesis of Reliable Organisms from Unreliable Components." The vacuum tubes of the day failed constantly, and the standard engineering response was to make better tubes. His response was to ask whether you could build an arbitrarily reliable machine out of components that are guaranteed to fail, using redundancy and majority voting, and then to work out how much redundancy you actually need. That is one of the founding ideas of fault-tolerant computing, and gives rise to replicated state machine, quorums, and Paxos.

## Self-reproducing automata

In the late 1940s, Johnny started asking [what it would take for a machine to build a copy of itself](https://en.wikipedia.org/wiki/Von_Neumann_universal_constructor). The naive worry is infinite recursion: the machine needs a blueprint of itself, and that blueprint must include a blueprint of the blueprint, and so on down forever. His solution (worked out with Ulam's suggestion to set it in a discrete cellular framework) was to split the role of the description. You have a universal constructor plus a tape describing it. The constructor reads the tape as "instructions" and builds the described machine. Then it copies the tape as uninterpreted "data" and attaches the copy to the offspring. One description is used twice in two completely different modes. This is the stored-program idea again, three years after EDVAC: the same bits are instructions when you execute them and data when you copy them.

He published this in 1948 and 1949. Watson and Crick published the structure of DNA in 1953. So von Neumann derived, from pure logic, the necessary architecture of biological reproduction. That there must be a stored description. That it must be read in two distinct ways. That this separation is precisely what makes open-ended evolution possible. Translation and transcription. Genotype and phenotype. He got the shape of the answer years before anyone knew what the molecule was.

The 29-state cellular automaton he built to prove it was also, incidentally, the first cellular automaton. Conway's Game of Life, Wolfram's entire research program, artificial life, and a foundations of what people now call synthetic biology all descend from his formal model. "Theory of Self-Reproducing Automata" was published posthumously in 1966.

His last work, also published posthumously, was "The Computer and the Brain" (1958). In it he observes that the brain achieves what it does with terrible arithmetic precision and enormous parallelism, which is the opposite of the machines he had just finished building. There he argues that whatever language the nervous system computes in, it is not mathematics as we understand mathematics. We are still working on this last one!

## Personality

Johnny loved parties, loud music, and fast cars. He drove crowded roads like a maniac, and got bored and slowed down on open ones. He wrecked a car roughly every year, usually a Cadillac, "because no one would sell me a tank". An intersection in Princeton was christened Von Neumann Corner for the number of accidents he had there. One of his accident reports says: "I was proceeding down the road. The trees on the right were passing me in orderly fashion at 60 miles an hour. Suddenly one of them stepped in my path. Boom!"

He also did his best work in chaos, and played German march music at volume in his office at Princeton, generating complaints from neighbors who included Albert Einstein.

His detractors called him cold, calculating, and ruthless. He did not join the Manhattan Project colleagues who agonized afterward, and instead he pushed the government toward bigger hydrogen bombs. He advocated an immediate preemptive nuclear strike on the USSR: "If you say why not bomb them tomorrow, I say why not today? If you say today at five o'clock, I say why not one o'clock?" Very brutal. And he said it in public, more than once, to journalists, while sitting on the Atomic Energy Commission. But notice that, this is rooted in his abstraction habit. Model the Cold War as a two-player zero-sum game, assume the Soviets get the bomb soon and would use it, and minimax says strike first. The math checks out, but the problem is the very simplistic model he uses. Stanislaw Ulam, who worked beside him for twenty years, put it as kindly as it can be put: "There was perhaps an inclination to take a too exclusively rational point of view about the cases of historical events."

Paradoxically, the same man was remarkably generous with people. He moved heaven and earth for those he thought were promising: Turing and Mandelbrot both benefited, and he mentored David Blackwell, a Black statistician Princeton did not want, and pushed until they took him. Blackwell wrote later about how much it mattered. Johnny put his own reputation on the line testifying for Oppenheimer, whose politics he did not share. A friend described him as "always gentle, always kind, always penetrating and always magnificently lucid." Teller said: "Von Neumann would carry on a conversation with my 3-year-old son, and the two of them would talk as equals, and I sometimes wondered if he used the same principle when he talked to the rest of us."

Which brings me back to the Teller line I opened with: Johnny enjoyed thinking, and Teller suspected he enjoyed almost nothing else. That is a lovely thing to say about a man at his funeral. It is also the setup for the cruelest possible ending, and Teller knew it when he said it. The cancer, probably from the radiation he absorbed standing around at weapons tests, started in the bone and went to the brain. He had spent fifty years as the fastest mind in any room he entered, and he had to watch that go. Teller: "When \[Johnny\] was dying of cancer, his brain was affected. I think that he suffered from this loss more than I have seen **any human to suffer in any other circumstance**".

Johnny remained a game theorist to the end. After a lifetime of culturally Jewish atheism, he asked to be baptized. His daughter attributed it to Pascal's Wager: a small probability of a much better afterlife, against a trivial cost.

## Can we survive technology?

In 1955 von Neumann wrote an essay called "Can We Survive Technology?" He spends most of it on nuclear war, but he touches on global warming. In 1955! And a few years earlier, in a conversation Ulam recalled in his obituary, he had already coined "singularity" (yes, that [singularity](https://en.wikipedia.org/wiki/Technological_singularity)) for a point where technological progress accelerates past the possibility of predicting what comes next.

This is what he actually argues about whether you can solve this by banning the dangerous technology:

> *In looking for a solution, it is well to exclude one pseudosolution at the start. The crisis will not be resolved by inhibiting this or that apparently particularly obnoxious form of technology. For one thing, the parts of technology, as well as of the underlying sciences, are so intertwined that in the long run nothing less than a total elimination of all technological progress would suffice for inhibition.*

He goes on to say that a ban would have to be worldwide to work, and prohibition of invention runs against the entire ethos of the industrial age, and is not going to be heeded anyways.

So what does he offer instead?

> *What safeguard remains? Apparently only day-to-day — or perhaps year-to-year — opportunistic measures, a long sequence of small, correct decisions. \[...\] For progress there is no cure. Any attempt to find automatically safe channels for the present explosive variety of progress must lead to frustration. The only safety possible is relative, and it lies in an intelligent exercise of day-to-day judgment.*

And then he names the qualities required: "patience, flexibility, intelligence". So, here is von Neumann, the master of abstraction, with no abstraction to offer. Maybe through his bout of applying minimax reasoning to nuclear strategy, he learned what a formal model is worth when the assumptions are shaky and the stakes are civilizational.
