% Quantum computing
% Boaz Barak

#  Quantum computing


> # { .objectives }
* Main aspects in which quantum mechanics differs from local deterministic theories. \
* Model of quantum circuits, or equivalently QNAND programs \
* Simon's Algorithm: an example of potential exponential speedup using quantum computers that predated and inspired Shor's factoring algorithm.



>_"We always have had (secret, secret, close the doors!) ... a great deal of difficulty in understanding the world view that quantum mechanics represents ... It has not yet become obvious to me that there's no real problem. ...  Can I learn anything from asking this question about computers--about this may or may not be mystery as to what the world view of quantum mechanics is?"_ , Richard Feynman, 1981

>_"The only difference between a probabilistic classical world and the equations of the quantum world is that somehow or other it appears as if the probabilities would have to go negative"_, Richard Feynman, 1981

There were two schools of natural philosophy in ancient Greece.
_Aristotle_ believed that objects have an _essence_ that explains their behavior, and a theory of the natural world has to refer to the _reasons_ (or "final cause" to use Aristotle's language) as to why they exhibit certain phenomena.
_Democritus_ believed in a purely mechanistic explanation of the world.
In his view, the universe was ultimately composed of elementary particles (or _Atoms_) and  our observed phenomena arise from the interactions between these particles according to some local rules.
Modern science (arguably starting with Newton) has embraced Democritus' point of view, of a mechanistic or "clockwork" universe of particles and forces acting upon them.


While the classification of particles and forces evolved with  time, to a large extent the "big picture" has not changed from  Newton till Einstein.
In particular it was held as an axiom that if we knew fully the current _state_ of the universe (i.e., the particles and their properties such as location and velocity) then we could predict its future state at any point in time.
In computational language, in all these theories the state of a system with $n$ particles could be stored in an array of $O(n)$ numbers, and predicting the evolution of the system  can be done by running some efficient (e.g., $poly(n)$ time) computation on this array.

## The double slit experiment


Alas, in the beginning of the 20th century, several experimental results were calling into question the "billiard ball" theory of the world.
One such experiment is the famous [double slit](https://en.wikipedia.org/wiki/Double-slit_experiment) experiment.
One way to describe it is as following.
Suppose that we buy one of those baseball pitching machines, and aim it at a soft plastic wall, but put a metal barrier between the machine and the wall that has a single slit.
If we shoot baseballs at the wall, then some of the baseballs would bounce off the metal barrier, while some would make it through the slit and dent the plastic wall.
If we now carve out an additional slit in the metal barrier then more balls would get through, and so the plastic wall would be _even more dented_.
(See [doublebaseballfig](){.ref})


![In the "double baseball experiment" we shoot baseballs from a gun at a soft wall through a hard barrier that has one or two slits open in it. There is only "constructive interference" in the sense that the dent in each position in the wall when both slits are open is the sum of the dents when each slit is open on its own.](../figure/double_baseball2.png){#doublebaseballfig .class width=300px height=300px}


So far this is pure common sense, and it is indeed (to my knowledge) an accurate description of what happens when we shoot baseballs at a wall.
However, this is not the same when we shoot _photons_.
Amazingly, if we shoot with a "photon gun" (i.e., lasers) at a wall equipped with photon detectors through some barrier, then (as shown [doubleslitfig](){.ref}) we sometimes see  _fewer_ hits when the two slits are open than one only ones of them is!.^[A nice illustrated description of the double slit experiment appears in   [this video](https://www.youtube.com/watch?v=DfPeprQ7oGc).]
In particular there are positions in the wall that are hit when the first slit is open, hit when the  second gun is open, but are _not hit at all when both slits are open!_.



![The setup of the double slit experiment in the case of photon or electron guns. We see also _destructive_ interference in the sense that there are some positions on the wall that get _fewer_ hits when both slits are open than they get when only one of the slits is open. Image credit: Wikipedia.](../figure/double-slit-setup.PNG){#doubleslitfig .class width=300px height=300px}


It seems as if each photon coming out of the gun is aware of the global setup of the experiment, and behaves differently if two slits are open than if only one is.
If we try to "catch the photon in the act" and place  a detector right next to each slit so we can see exactly the path each photon takes  then something even more bizarre happens.
The mere fact that we _measure_ the  path changes the photon's behavior, and now this "destructive interference" pattern is gone and the number of times a position is hit when two slits are open is the sum of the number of times it is hit when each slit is open.


> # { .pause }
You should read the paragraphs above more than once and make sure you appreciate how truly mind boggling these results are.


## Quantum amplitudes


The double slit and other experiments ultimately forced scientists to accept the following picture of the world.
Let us go back to the baseball experiment.
Suppose that the probability a ball passes through the left slit is $p_L$ and the probability that it passes through the right slit is $p_R$.  Then, if we shoot $N$ balls out of each gun, we expect the wall  will be hit $(p_L+p_R)N$ times.

In the quantum world, it can sometimes be the case that in both the first and second case the wall is hit with positive probabilities $p_L$ and $p_R$ respectively but somehow when both slits are open the wall (or a particular position in it) is not hit at all. It's almost as if the probabilities can "cancel each other out".

To understand the way we model this in quantum mechanics, it is helpful to think of a "lazy evaluation" approach to probability.
We can think of a probabilistic experiment such as shooting a baseball through two slits  in two different ways:

* When a ball is shot, "nature" tosses a coin and decides if it will go through the left slit (which happens with a certain probability $p_L$), right slit (which happens with a certain probability $p_R$),  or bounce back. If it passes through one of the slits then it will hit the wall.
Later we can look at the wall and find out whether or not this event happened, but the fact that the event happened or not is determined independently of whether or not we look at the wall.

* The other viewpoint is that when a ball is shot, "nature" computes the probabilities $p_L$ and $p_R$ as before, but does _not_ yet "toss the coin" and determines what happened.
Only when  we actually look at the wall, nature tosses a coin and with probability $p_L+p_R$ ensures we see a dent. That is, nature uses "lazy evaluation", and only determines the result of a probabilistic experiment when we decide to _measure_ it.

While the first scenario seems much more natural, the end result in both is the same (the wall is hit with probability $p_L+p_R$) and so the question of  whether we should model nature as following the first scenario or second one  seems like asking about the proverbial tree that falls in the forest with no one hearing about it.

However, when we want to describe the double slit experiment with photons rather than baseballs, it is the second scenario that lends itself better to a quantum generalization.
Quantum mechanics associates a number $\alpha$ known as an _amplitude_ with each probabilistic experiment.
This number $\alpha$ can be _negative_, and in fact even _complex_.
We never observe the amplitudes directly, since whenever we _measure_ an event with amplitude $\alpha$, nature tosses a coin and determines that the event happens with probability $|\alpha|^2$.
However, the sign (or in the complex case, phase) of the amplitudes can affect whether two different events have _constructive_ or _destructive_ interference.

::: { .pause }
If you don't find the above description confusing and unintuitive, you probably didn't get it.
Please make sure to re-read the above paragraphs until you are thoroughly confused.
:::

Specifically, consider an event that can either occur or not (e.g. "detector number 17 was hit by a photon").
In classical probability, we model this by a probability distribution over the two outcomes: a pair of non-negative numbers $p$ and $q$ such that $p+q=1$, where $p$ corresponds to  the probability that the event occurs and $q$ corresponds to the probability that the event does not occur.
In quantum mechanics, we model this also by  pair of numbers, which we call _amplitudes_. This is  a pair of (potentially negative or even complex) numbers $\alpha$ and $\beta$ such that $|\alpha|^2 + |\beta|^2 =1$.
The probability that the event occurs is $|\alpha|^2$ and the probability that it does not occur is $|\beta|^2$.[^quantum]
In isolation, these negative or complex numbers don't matter much, since we anyway square them to obtain probabilities.
But the interaction of positive and negative amplitudes can result in surprising _cancellations_ where somehow combining two scenarios where an event happens with positive probability results in a scenario where it  never does.


Quantum mechanics is a mathematical theory that allows us to calculate and predict the results of the double-slit and many other experiments.
If you think of quantum mechanics as an explanation as to what "really" goes on in the world, it can be rather confusing.
However, if you simply "shut up and calculate" then it works amazingly well at predicting  experimental results.
In particular, in the double slit experiment, for any position in the wall, we can compute  numbers $\alpha$ and $\beta$ such that photons from the first and second slit  hit that position with probabilities $|\alpha|^2$ and $|\beta|^2$ respectively.
When we open both slits, the probability that the position will be hit is proportional to $|\alpha+\beta|^2$, and so in particular, if $\alpha=-\beta$ then it will be the case that, despite being hit when _either_ slit one or slit two are open, the position is _not hit at all_ when they both are.
If you are confused by quantum mechanics,  you are not alone: for decades people have been trying to come up with [explanations](https://en.wikipedia.org/wiki/Interpretations_of_quantum_mechanics) for "the underlying reality" behind quantum mechanics, from [Bohmian mechanics](https://en.wikipedia.org/wiki/De_Broglie%E2%80%93Bohm_theory) to [Many worlds](https://en.wikipedia.org/wiki/Many-worlds_interpretation) as well as many others.
However, none of these interpretation have gained universal acceptance and all of those (by design) yield the same experimental predictions.
Thus at this point many scientists prefer to just ignore the question of what is the "true reality" and go back to simply "shutting up and calculating".

[^quantum]: I should warn that we are making here many simplifications. In particular while quantum amplitudes can be _complex_ numbers, we will usually restrict attention to _real_ (though potentially negative) numbers.  A;most all of the power and subtleties of quantum mechanics and quantum computing arise from allowing _negative_  numbers, and the generalization from real to complex numbers is much less important. We will also be focusing on so called "pure" quantum states, and ignore the fact that generally the states of a quantum subsystem are _mixed_ states that are a convex combination of pure states and can be described by a so called _density matrix_. This issue does not arise as much in quantum algorithms precisely because the goal is for a quantum computer is to be an isolated system that can evolve to continue to be in a pure state; in real world quantum computers however there will be interference from the outside world that causes the state to become mixed and increase its so called "von Neumann entropy". Fighting this interference and the second law of thermodynamics is much of what the challenge of building quantum computers is all about . More generally, this lecture is not meant to be a complete or accurate description of quantum mechanics, quantum information theory, or quantum computing, but rather just give a sense of the main points where it differs  from classical computing.

## Bell's Inequality

There is something weird about quantum mechanics.
In 1935 [Einstein, Podolsky and Rosen (EPR)](http://plato.stanford.edu/entries/qt-epr/) tried to pinpoint this issue by highlighting a previously unrealized corollary of this theory.
They showed that the idea that nature does not determine the results of an experiment until it is measured results in so called  "spooky action at a distance".
Namely, making a measurement of one object may  instantaneously  effect the state (i.e., the vector of amplitudes) of another object in the other end of the universe.


Since the vector of amplitudes  is just a mathematical abstraction,  the EPR paper was considered to be merely a thought experiment for philosophers to be concerned about, without bearing on experiments.
This changed when in 1965 John Bell showed an actual experiment to test the predictions of EPR and hence pit intuitive common sense against the quantum mechanics.
Quantum mechanics won: it turns out that it _is_ in fact possible to use measurements to create correlations between the states of objects far removed from one another that cannot be explained by any prior theory.
Nonetheless, since the results of these experiments are so obviously wrong to anyone that has ever sat in an armchair,  that there are still a number of [Bell denialists](http://www.scottaaronson.com/blog/?p=2464) arguing that this can't be true and quantum mechanics is wrong.

So, what is this Bell's Inequality?
Suppose that Alice and Bob try to convince you they have telepathic ability, and they aim to prove it via the following experiment.
Alice and Bob will be in separate closed rooms.[^paranoid]
You will interrogate Alice and your associate will interrogate Bob.
You choose a random bit $x\in\{0,1\}$ and your associate chooses a random $y\in\{0,1\}$.
We let $a$ be Alice's response and $b$ be Bob's response.
We say that Alice and Bob win this experiment if $a \oplus b = x \wedge y$.
In other words, Alice and Bob need to output two bits that _disagree_ if $x=y=1$ and _agree_ otherwise.


[^paranoid]: If you are extremely paranoid about Alice and Bob communicating with one another, you can coordinate with your assistant to perform the experiment exactly at the same time, and make sure that the rooms are sufficiently far apart (e.g., are on two different continents, or maybe even one is on the moon and another is on earth) so that Alice and Bob couldn't communicate to each other in time the results of their respective coins even if they do so at the speed of light.


Now if Alice and Bob are not telepathic, then they need to agree in advance on some strategy.
It's not hard for Alice and Bob to succeed with probability $3/4$: just always output the same bit.
However, by doing some case analysis, we can show that no matter what strategy they use, Alice and Bob cannot succeed with higher probability than that:^[[bellthm](){.ref} below assumes that Alice and Bob use _deterministic_ strategies $f$ and $g$ respectively. More generally, Alice and Bob could use a _probabilistic_ strategy, or equivalently, each could choose $f$ and $g$ from some  _distributions_ $\mathcal{F}$ and $\mathcal{G}$ respectively.  However the _averaging principle_ ([averagingprinciplerem](){.ref}) implies that if all possible deterministic strategies succeed with probability at most $3/4$, then the same is true for probabilistic strategies.]

> # {.theorem title="Bell's Inequality" #bellthm}
For every two functions $f,g:\{0,1\}\rightarrow\{0,1\}$, $\Pr_{x,y \in \{0,1\}}[  f(x) \oplus g(y) = x \wedge y] \leq 3/4$.

::: {.proof data-ref="bellthm"}
Since the probability is taken over all four choices of $x,y \in \{0,1\}$, the only way the theorem can be be violated if if there exist  two functions $f,g$ that satisfy
$$f(x) \oplus g(y) = x \wedge y \;(*)$$
for all the four choices of  $x,y \in \{0,1\}^2$.
Let's plug in all these four choices and see what we get (below we use the equalities $z \oplus 0 = z$, $z \wedge 0=0$ and $z \wedge 1 = z$):

$$
\begin{aligned}
f(0) &\oplus g(0) &= 0\;\;\;\; &(\text{plugging in } x=0,y=0) \\
f(0) &\oplus g(1) &= 0\;\;\;\; &(\text{plugging in } x=0,y=1) \\
f(1) &\oplus g(0) &= 0\;\;\;\; &(\text{plugging in } x=1,y=0) \\
f(1) &\oplus g(1) &= 1\;\;\;\; &(\text{plugging in } x=1,y=1)
\end{aligned}
$$

If we XOR together the first and second equalities we get $g(0) \oplus g(1) = 0$ while if we XOR together the third and fourth equalities we get $g(0) \oplus g(1) = 1$, thus obtaining a contradiction.
:::



[^CHSH]: This form of Bell's game was shown by [Clauser, Horne, Shimony, and Holt](https://goo.gl/wvJGZU).

An amazing [experimentally verified](http://arxiv.org/abs/1508.05949) fact is that quantum mechanics allows for "telepathy".[^telepathy]
Specifically, it has been shown that using the weirdness of quantum mechanics, there is in fact a strategy for Alice and Bob to succeed in this game with probability larger than $3/4$  (in fact, they can succeed with probability about $0.85$, see [bellstrategy](){.ref}).


[^telepathy]: More accurately, one either has to give up on a "billiard ball type" theory of the universe or believe in telepathy (believe it or not, some scientists went for the [latter option](https://en.wikipedia.org/wiki/Superdeterminism)).

## Quantum weirdness



Some of the counterintuitive properties that arise from  quantum mechanics include:

* **Interference** - As we've seen, quantum amplitudes can "cancel each other out".

* **Measurement** -   The idea that amplitudes are negative as long as "no one is looking" and "collapse" (by squaring them) to positive probabilities when they are _measured_ is deeply disturbing. Indeed, as shown by EPR and Bell, this leads to various strange outcomes such as "spooky actions at a distance", where we can create correlations between the results of measurements in  places far removed. Unfortunately (or fortunately?) these strange outcomes have been confirmed experimentally.

* **Entanglement** - The notion that two parts of the system could be connected in this weird way where measuring one will affect the other is known as _quantum entanglement_.

As counter-intuitive as these concepts are, they have been experimentally confirmed, so we just have to live with them.

> # {.remark title="More on quantum" #quantumsources}
The discussion in this  lecture is quite brief and somewhat superficial.
The chapter on quantum computation in my [book with Arora](http://theory.cs.princeton.edu/complexity/) (see [draft here](http://theory.cs.princeton.edu/complexity/ab_quantumchap.pdf)) is one
relatively short resource that contains essentially everything we discuss here and more.
See also this [blog post of Aaronson](http://www.scottaaronson.com/blog/?p=208) for a high level explanation of Shor's algorithm which ends with links to several more detailed expositions.
[This lecture](http://www.scottaaronson.com/democritus/lec14.html) of Aaronson contains a great discussion of the feasibility of quantum computing (Aaronson's [course lecture notes](http://www.scottaaronson.com/democritus/default.html) and the [book](http://www.amazon.com/Quantum-Computing-since-Democritus-Aaronson/dp/0521199565) that they spawned are fantastic reads as well).
The videos of [Umesh Variani'z EdX course](https://www.youtube.com/playlist?list=PLDAjb_zu5aoFazE31_8yT0OfzsTcmvAVg) are an accessible and recommended introduction to quantum computing.
See the "bibliographical notes" section at the end of this lecture for more resources.

## Quantum computing and computation - an executive summary.

One of the strange aspects of the quantum-mechanical picture of the world is that unlike in the billiard ball example, there is no obvious algorithm to simulate the evolution of $n$ particles over $t$ time periods in $poly(n,t)$ steps.
In fact, the natural way to simulate $n$ quantum particles will require a number of steps that is _exponential_ in $n$.
This is a huge headache for scientists that actually need to do these calculations in practice.

In the 1981, physicist Richard Feynman proposed  to "turn this lemon to lemonade" by making the following almost tautological observation:


>If a physical system cannot be simulated by a computer in $T$ steps, the system can be considered as performing a computation that would take more than $T$ steps.

So, he asked whether one could design a quantum system such that its outcome $y$ based on the initial condition $x$ would be some function $y=f(x)$ such that **(a)** we don't know how to efficiently compute in any other way, and **(b)** is actually useful for something.[^Feynman]
In 1985, David Deutsch formally suggested the notion of a quantum Turing machine, and the model has been since refined in works of Detusch and Josza and Bernstein and Vazirani.
Such a system is now known as a _quantum computer_.

[^Feynman]: As its title suggests, Feynman's [lecture](https://www.cs.berkeley.edu/~christos/classics/Feynman.pdf) was actually focused on the other side of simulating physics with a computer. However,  he mentioned that as a "side remark" one could wonder if it's possible to simulate physics with a new kind of computer - a "quantum computer" which would "not [be] a Turing machine, but a machine of a different kind". As far as I know, Feynman did not suggest that such a computer could be useful for computations completely outside the domain of quantum simulation. Indeed, he was more interested in the question of whether quantum mechanics could be simulated by a classical computer.


For a while these hypothetical quantum computers seemed useful for one of two things.
First, to provide a general-purpose mechanism to  simulate a variety of the real quantum systems that people care about, such as various interactions inside molecules in quantum chemistry.
Second, as a challenge to the _Extended Church Turing hypothesis_ which says that every physically realizable computation device can be modeled (up to polynomial overhead) by Turing machines (or equivalently, NAND++ / NAND<< programs).

Quantum chemistry is important (and in particular understanding it can be a bottleneck for designing new materials, drugs, and more), but it is still a rather niche area within the broader context of computing (and even scientific computing) applications.
Hence for a while most researchers (to the extent they were aware of it), thought of quantum computers as a theoretical curiosity that  has little bearing to practice, given that this theoretical "extra power" of quantum computer seemed to offer little advantage in the majority of the  problems people  want to solve in areas such as  combinatorial optimization, machine learning,  data structures, etc..

To some extent this is still true today. As far as we know, quantum computers, if built, will _not_ provide exponential speed ups for 95% of the applications of computing.[^overhead]
In particular, as far as we know, quantum computers will _not_ help us solve $\mathbf{NP}$ complete problems in polynomial or even sub-exponential time, though  _Grover's algorithm_ ( [quantumnp](){.ref}) does yield a quadratic advantage in many cases.





However, there is one cryptography-sized exception:
In 1994 Peter Shor showed that quantum computers can solve the integer factoring and discrete logarithm in polynomial time.
This result has captured the imagination of a great many people, and completely energized research into quantum computing.
This is both because the hardness of these particular problems provides the foundations for securing such a huge part of our communications (and these days, our economy), as well as it was a powerful demonstration that quantum computers could turn out to be useful for problems that a-priori seemd to have nothing to do with quantum physics.

As we'll discuss later, at the moment there are several intensive efforts to construct large scale quantum computers.
It seems safe to say that, as far as we know, in the next five years or so there will not be a quantum computer large enough to factor, say, a $1024$ bit number, but there it is quite possible that some quantum computer will be built that is strong enough to achieve some task that is too inefficient to achieve with a non-quantum or "classical" computer  (or at least requires far more resources classically than it would for this computer).
When and if such a computer is  built that can break reasonable parameters of Diffie Hellman, RSA and elliptic curve cryptography is anybody's guess.
It could also be a "self destroying prophecy" whereby the existence of a small-scale quantum computer would cause everyone to shift away to lattice-based crypto which in turn will diminish the motivation to invest the huge resources needed to build a large scale quantum computer.[^legacy]

[^legacy]: Of course, given that [we're still hearing](http://blog.cryptographyengineering.com/2016/03/attack-of-week-drown.html) of attacks exploiting "export grade" cryptography that was supposed to disappear in 1990's, I imagine that we'll still have products running 1024 bit RSA when everyone has a quantum laptop.

[^overhead]: This "95 percent" is a figure of speech, but not completely so. At the time of this writing, cryptocurrency mining  electricity consumption is estimated to use up at least [70Twh or 0.3 percent of the world's production](https://digiconomist.net/bitcoin-energy-consumption), which is about [2 to 5 percent](http://www.mdpi.com/2078-1547/6/1/117/html) of the total energy usage for the computing industry. All the current cryptocurrencies will be broken by quantum computers. Also, for many web servers the TLS protocol (which based on the current non-lattice based systems would be completely broken by quantum computing) is responsible [for about 1 percent of the CPU usage](https://goo.gl/mHpYpm).


::: {.remark title="Quantum computing and $\mathbf{NP}$" #quantumnp}
Despite popular accounts of quantum computers as having variables that can take "zero and one at the same time"  and therefore can "explore an exponential number of possibilities simultaneously", their true power is much more subtle and nuanced.
In particular, as far as we know, quantum computers do _not_ enable us to solve $\mathbf{NP}$ complete problems such as 3SAT in polynomial or even sub-exponential time.
However, [Grover's search algorithm](https://goo.gl/NQVLLF) does give a more modest advantage (namely, quadratic) for quantum computers over classical ones for problems in $\mathbf{NP}$.
In particular, due to Grover's search algorithm, we know that the $k$-SAT problem for $n$ variables can be solved in time $O(2^{n/2}poly(n))$ on a quantum computer for every $k$.
In contrast, the best known algorithms for $k$-SAT on a classical computer take roughly $2^{(1-\tfrac{1}{k})n}$ steps.
:::



## Quantum systems

Before we talk about _quantum_ computing, let us recall how we physically realize "vanilla" or _classical_ computing.
We model a _logical bit_ that can equal $0$ or a $1$ by some physical system that can be in one of two states.
For example, it might be a wire with high or low voltage, charged or uncharged capacitor, or even (as we saw) a pipe with or without a flow of water, or the presence or absence of a soldier crab.
A _classical_ system of $n$ bits is composed of $n$ such "basic systems", each of which can be in either a "zero" or "one" state.
We can model the state of such a system by a string $s \in \{0,1\}^n$.
If we perform an operation such as writing to the 17-th bit the NAND of the 3rd and 5th bits, this corresponds to applying a _local_ function to $s$ such as setting $s_{17} = 1 - s_3\cdot s_5$.

In the _probabilistic_ setting, we would model the state of the system by a _distribution_.
For an individual bit, we could model it by a pair of non-negative numbers $\alpha,\beta$ such that $\alpha+\beta=1$, where $\alpha$ is the probability that the bit is zero and $\beta$ is the probability that the bit is one.
For example,  applying the _negation_ (i.e., NOT) operation to this bit  corresponds to mapping the pair $(\alpha,\beta)$ to $(\beta,\alpha)$ since the probability that $NOT(\sigma)$ is equal to $1$ is the same as the probability that $\sigma$ is equal to $0$.
This means that we can think of the NOT function as the linear map $N:\R^2 \rightarrow \R^2$ such that $N \begin{pmatrix} \alpha \\ \beta \end{pmatrix} = \begin{pmatrix} \beta \\ \alpha \end{pmatrix}$ or equivalently as the matrix $\begin{pmatrix} 0 & 1 \\ 1 & 0 \end{pmatrix}$.

If we think of the $n$-bit system as a whole, then since the $n$ bits can take  one of  $2^n$ possible values, we model the state of the system as a vector $p$ of $2^n$ probabilities.
For every $s\in \{0,1\}^n$, we denote by $e_s$ the $2^n$ dimensional vector that has $1$ in the coordinate corresponding  to $s$ (identifying it with a number in $[2^n]$), and so can write  $p$ as $\sum_{s\in \{0,1\}^n} p_s e_s$ where $p_s$ is the probability that the system is in the state $s$.


Applying the operation above of setting the $17$-th bit to the NAND of the 3rd and 5th bits, corresponds to transforming the vector $p$ to the vector $Fp$ where $F:\R^{2^n} \rightarrow \R^{2^n}$ is the linear map that maps $e_s$ to $e_{s_0\cdots s_{16}(1-s_3\cdot s_5)s_{18}\cdots s_{n-1}}$.^[Since $\{ e_s \}_{s\in \{0,1\}^n}$ is a _basis_ for $R^{2^n}$, it sufficies to define the map $F$ on vectors of this form.]

::: { .pause }
Please make sure you understand why performing the operation will take a system in state $p$ to a system in the state $Fp$.
Understanding the evolution of probabilistic systems is a prerequisite to understanding the evolution of quantum systems.

If your linear algebra is a bit rusty, now would be a good time to review it, and in particular make sure you are comfortable with the notions of _matrices_, _vectors_, (orthogonal and orthonormal) _bases_, and _norms_.
:::

### Quantum amplitudes

In the quantum setting, the state of an individual bit (or "qubit", to use quantum parlance) is modeled by a pair of numbers $(\alpha,\beta)$ such that $|\alpha|^2 + |\beta|^2 = 1$.
While in general these numbers can be _complex_, for the rest of this chapter, we will often assume they are _real_ (though potentially negative), and hence often drop the absolute value operator.
(This  turns out not to make much of a difference in explanatory power.)
As before, we think of $\alpha^2$ as the probability that the bit equals $0$ and $\beta^2$ as the probability that the bit equals $1$.
As we did  before, we can model the NOT operation by the map $N:\R^2 \rightarrow \R^2$ where $N(\alpha,\beta)=(\beta,\alpha)$.

Following quantum tradition, instead of using $e_0$ and $e_1$ as we did above, from now on we will denote the vector $(1,0)$ by $|0\rangle$  and the vector $(0,1)$ by $|1\rangle$ (and moreover, think of these as column vectors).
This is known as the Dirac "ket" notation.
This means that NOT is the unique linear map $N:\R^2 \rightarrow \R^2$ that satisfies $N |0\rangle=|1\rangle$ and $N |1\rangle =|0\rangle$.
In other words, in the quantum case, as in the probabilistic case,  NOT corresponds to the matrix
$$
N = \begin{pmatrix} 0 & 1 \\ 1 & 0 \end{pmatrix} \;.
$$



In classical computation, we typically think that there are only two operations that we can do on a single bit: keep it the same or negate it.
In the quantum setting, a single bit operation corresponds to any linear map $OP:\R^2 \rightarrow \R^2$ that is _norm preserving_ in the sense that  for every $\alpha,\beta$,  if we apply $OP$ to the vector $\begin{pmatrix} \alpha \\ \beta \end{pmatrix}$ then we obtain a vector $\begin{pmatrix} \alpha' \\ \beta' \end{pmatrix}$ such that  $\alpha'^2 + \beta'^2 = \alpha^2 + \beta^2$.
Such a linear map $OP$ corresponds to a [unitary](https://en.wikipedia.org/wiki/Unitary_matrix) two by two matrix.^[As we mentioned,  quantum mechanics actually models states as vectors with _complex_ coordinates. However, this does not make any qualitative difference to our discussion.]
Keeping the bit the same corresponds to the matrix $I = \begin{pmatrix} 1&0\\ 0&1 \end{pmatrix}$ and (as we've seen) the NOT operations corresponds to the matrix $N = \begin{pmatrix} 0&1\\ 1&0 \end{pmatrix}$.
But there are other operations we can use as well.
One such useful operation is the _Hadamard_ operation, which corresponds to the matrix
$$H = \tfrac{1}{\sqrt{2}} \begin{pmatrix} +1 & +1\\ +1 & -1 \end{pmatrix} \;. $$

In fact it turns out that Hadamard is all that we need to add to a classical universal basis to achieve the full power of quantum computing.

### Recap

The _state_ of  a _quantum system_ of $n$ qubits is modeled by an $2^n$ dimensional vector $v$ of unit norm (i.e., squares of all coordinates sums up to $1$), which we write as $\sum_{x\in \{0,1\}^n} v_x |x \rangle$ where $|x\rangle$ is the column vector that has $0$ in all coordinates except the one corresponding to $x$ (identifying $\{0,1\}^n$ with the numbers $\{0,\ldots,2^n-1\}$).
We use the convention that if $a,b$ are strings of lengths $k$ and $\ell$ respectively then we can write the $2^{k+\ell}$ dimensional vector with $1$ in the $ab$-th coordinate and zero elsewhere not just as  $|ab\rangle$  but also as $|a\rangle |b \rangle$.
In particular, for every $x\in \{0,1\}^n$, we can write the vector $|x\rangle$ also as $|x_0\rangle |x_1\rangle \cdots |x_{n-1} \rangle$.
This notation satisfies certain nice distributive laws such as $|a\rangle(|b\rangle + |b'\rangle)|c \rangle = |abc \rangle + |ab'c\rangle$.

A _quantum operation_ on such a system is modeled by a $2^n \times 2^n$ _unitary matrix_ $U$ (one that satisfies $UU^\top = I$ where $U^\top$ is the _transpose_ operation, or conjugate transpose for complex matrices).
If the system is in state $v$ and we apply to it the operation $U$, then the new state of the system is $Uv$.

When we _measure_ an $n$-qubit system in a state $v= \sum_{x\in \{0,1\}^n} v_x |x \rangle$, then we observe the value $x\in \{0,1\}^n$ with probability $|v_x|^2$.


### Analysis of Bell's Inequality (optional)

Now that we have the notation in place, we can show a strategy for showing "quantum telepathy".
Recall that in the classical case, Alice and Bob can succeed in the "Bell Game" with probability at most $3/4 = 0.75$.
We now show that quantum mechanics allows them to succeed with probability at least $0.8$.^[The strategy we show is not the best one. Alice and Bob can in fact  succeed with probability $\cos^2(\pi/8) \sim 0.854$.]


> # {.lemma #bellstrategy}
There is a 2-qubit quantum state $s\in \R^4$ so that if Alice has access to the first qubit of $s$, can manipulate and measure it and output $a\in \{0,1\}$ and Bob has access to the second qubit of $s$ and can manipulate and measure it and output $b\in \{0,1\}$ then
$\Pr[ a \oplus b = x \wedge y ] \geq 0.8$.

::: {.proof data-ref="bellstrategy"}
The main  idea is for Alice and Bob to first prepare a 2-qubit quantum system in the state (up to normalization)
$|00\rangle+|11\rangle$ (this is known as an _EPR pair_).
Alice takes the first qubit in this system to her room, and Bob takes the qubit to his room.
Now, when Alice receives $x$ if $x=0$ she does nothing and if $x=1$ she applies the unitary map $R_{\pi/8}$ to her qubit where $R_\theta = \begin{pmatrix} cos \theta & \sin -\theta \\ \sin \theta & \cos \theta \end{pmatrix}$ is the unitary operation corresponding to rotation in the plane with angle $\theta$.
When Bob receives $y$, if $y=0$ he does nothing and if $y=1$ he applies the unitary map $R_{-\pi/8}$ to his  qubit.
Then each one of them measures their qubit and sends this as their response.


Recall that to win the game Bob and Alice want their outputs to be more likely to differ if $x=y=1$ and to be more likely to agree otherwise.
We will split the analysis in one case for each of the four possible values of $x$ and $y$.

__Case 1: $x=y=0$__ If $x=y=0$ then the state does not change.
Because the state is   $|00\rangle + |11\rangle$, the measurements of Bob and Alice will always agree (if Alice measures $0$ then the state collapses to $|00 \rangle$ and so Bob measures $0$ as well, and similarly for $1$).
Hence in the case $x=y=1$, Alice and Bob always  win.

__Case 2: $x=0$,$y=1$__ If $x=0$ and $y=1$ then after Alice measures her bit, if she gets $0$ then Bob's state is equal to $-\cos (\pi/8)|0\rangle-\sin(\pi/8)|1\rangle$ which will equal $0$ with probability $\cos^2 (\pi/8) \geq 0.85$. The analysis when Alice gets $1$ from her measurement is the same, and hence they will win with probability $\cos^2 (\pi/8)$ as well. The same analysis also shows that Alice and Bob will win with probability $\cos^2 (\pi/8)$ in __Case 3__, where $x=1$ and $y=0$.


__Case 4: $x=y=1$__ For the case that $x=1$ and $y=1$, direct calculation via trigonometric identities yields that all four options for $(a,b)$ are equally likely and hence in this case $a=b$ with probability $0.5$.


Together we see that the overall probability of winning the game is at least $\tfrac{1}{4}\cdot 1 + \tfrac{1}{2}\cdot 0.85 + \tfrac{1}{4} \cdot 0.5 =0.8$.
:::


> # {.remark title="Quantum vs probabilistic strategies" #quantumprob}
It is instructive to understand what is it about quantum mechanics that enabled this gain in Bell's Inequality. For this, consider the following analogous probabilistic strategy for Alice and Bob. They agree that each one of them output $0$ if he or she get $0$ as input and outputs $1$ with probability $p$ if they get $1$ as input. In this case one can see that their success probability would be $\tfrac{1}{4}\cdot 1 + \tfrac{1}{2}(1-p)+\tfrac{1}{4}[2p(1-p)]=0.75 -0.5p^2 \leq 0.75$. The quantum strategy we described above can be thought of as a variant of the probabilistic strategy for parameter $p$ set to  $\sin^2 (\pi/8)=0.15$. But in the case $x=y=1$, instead of disagreeing only with probability $2p(1-p)=1/4$, because we can use these negative probabilities in the quantum world and rotate the state in opposite directions, and hence  the probability of disagreement ends up being $\sin^2 (\pi/4)=0.5$.

## Quantum computation

Recall that in the classical setting, we modeled computation as obtained by a sequence of _basic operations_.
We had two types of computational models:

* _Non uniform models of computation_ such as Boolean circuits and NAND program, where a finite function $f:\{0,1\}^n \rightarrow \{0,1\}$ is computable in size $T$ if it can be expressed as a combination of $T$ basic operations (gates in a circuit or lines in a NAND program)

* _Uniform models of computation_ such as Turing machines and NAND++ programs, where an infinite function $F:\{0,1\}^* \rightarrow \{0,1\}$ is computable in time $T(n)$ if there is a single algorithm that on input $x\in \{0,1\}^n$ evaluates $F(x)$ using at most $T(n)$ basic steps.


When considering _efficient computation_, we defined the class $\mathbf{P}$ to consist of all infinite functions $F:\{0,1\}^* \rightarrow \{0,1\}$ that can be computed by a Turing machine or NAND++ program in time $p(n)$  for some polynomial $p(\cdot)$.
We defined the class  $\mathbf{P_{/poly}}$ to consists of all infinite functions $F:\{0,1\}^* \rightarrow \{0,1\}$ such that for every $n$, the restriction $F_n$ of $F$ to $\{0,1\}^n$ can be computed by a Boolean circuit or NAND program of size at most $p(n)$ for some polynomial $p(\cdot)$.

We will do the same for _quantum computation_, focusing mostly on the _non uniform_ setting of quantum circuits, since that is simpler, and already illustrates the important differences with classical computing.


### Quantum circuits and QNAND

A _quantum circuit_ is analogous to a Boolean circuit, and can be described as a directed acyclic graph.
One crucial difference that the _out degree_ of every vertex in a quantum circuit is at most one.
This is because we cannot "reuse" quantum states without _measuring_ them (which collapses their "probabilities").
Therefore, we cannot use the same bit as input for two different gates.^[This is known as the [No Cloning Theorem](https://goo.gl/jCVtEY).]
Another more technical difference is that to express our operations as unitary matrices, we will need to make sure all our gates are _reversible_.
This is not hard to ensure.
For example, in the quantum context, instead of thinking of $NAND$ as a (non reversible) map from $\{0,1\}^2$ to $\{0,1\}$, we will think of it as the reversible  map on _three_ qubits that maps $a,b,c$ to $a,b,c\oplus NAND(a,b)$.
Equivalently, the NAND operation corresponds to  the unitary map $U_{NAND}$ on $\R^{2^3}$ such that (identifying $\{0,1\}^3$ with $[8]$) for every $a,b,c \in \{0,1\}$, if $|abc\rangle$ is the basis element with $1$ in the $abc$-th coordinate and zero elsewhere, then $U |abc\rangle =|ab(c \oplus NAND(a,b))\rangle$.^[Readers familiar with quantum computing should note that $U_{NAND}$ is a close variant of the so called [Toffoli gate](https://goo.gl/BE7aVG) and so QNAND programs correspond to quantum circuits with the Hadamard and Toffoli gates.]

If we have an $n$ qubit system, then for $i,j,k \in [n]$, we will denote by $U_{NAND}^{i,j,k}$ as the $2^n \times 2^n$ unitary matrix that corresponds to applying $U_{NAND}$ to the $i$-th, $j$-th, and $k$-th  bits, leaving the others intact.
That is, for every $v = \sum_{x\in \{0,1\}^n} v_x |x \rangle$,
$U_{NAND}^{i,j,k}v = \sum_{x\in \{0,1\}^n}v_x |x_0 \cdots x_{k-1}(x_k \oplus NAND(x_i,x_j))x_{k+1} \cdots x_{n-1} \rangle$.

As mentioned above, we will also use the _Hadamard_ or $HAD$ operation,
A _quantum circuit_ is obtained by applying a sequence of $U_{NAND}$ and $HAD$ gates, which correspond to the matrix
$$H = \tfrac{1}{\sqrt{2}} \begin{pmatrix} +1 & +1\\ +1 & -1 \end{pmatrix} \;. $$
Another way to write define $H$ is that for $b \in \{0,1\}$,  $H |b\rangle = \tfrac{1}{\sqrt{2}}|0\rangle + \tfrac{1}{\sqrt{2}} (-1)^b |1\rangle$.
We define $HAD^i$ to be the $2^n \times 2^n$ unitary matrix that applies $HAD$ to the $i$-th qubit and leaves the others intact.
Using the ket notation, we can write this as
$$
HAD^i \; \sum_{x\in \{0,1\}^n} v_x |x \rangle  \;=\; \tfrac{1}{\sqrt{2}}\sum_{x\in \{0,1\}^n}|x_0 \cdots x_{i-1}\rangle \left(|0\rangle + (-1)^{x_i}|1\rangle\right)|x_i \cdots x_{n-1} \rangle \;.
$$

A _quantum circuit_ is obtained by composing these basic operations on some $m$ qubits.
If $m \geq n$, we use a circuit to compute a function $f:\{0,1\}^n \rightarrow \{0,1\}$:

* On input $x$, we initialize the system to hold $x_0,\ldots,x_{n-1}$ in the first $n$ qubits, and initialize all remaining $m-n$ qubits to zero.

* We execute each elementary operation one by one.

* At the end of the computation, we _measure_ the system, and output the result of the last qubit (i.e. the qubit in location $m-1$).^[For simplicity we restrict attention to functions with a single bit of output, though the definition of quantum circuits naturally extends to circuits with multiple outputs.]

* We say that the circuit _computes_ $f$ is the probability that this output equals $f(x)$ is at least $2/3$. Note that this probability is obtained by summing up the squares of the amplitudes of all coordinates in the final state of the system corresponding to vectors $|y \rangle$ where $y_{m-1}=f(x)$.



Formally this is defined as follows:


::: {.definition title="Quantum circuit" #quantumcircuitdef}
A _quantum circuit of $m$ inputs and $s$ gates_  over the $\{ UNAND, HAD \}$ basis is a sequence of $s$ unitary $2^n \times 2^n$ matrices $U_0,\ldots,U_{s-1}$ such that each matrix $U_\ell$ is either of the form $NAND^{i,j,k}$ for $i,j,k\in [n]$ or $HAD^i$ for $i\in [n]$.

A quantum circuit _computes_ a function $f:\{0,1\}^n \rightarrow \{0,1\}$ if the following is true for every $x\in \{0,1\}^n$:

Let $v$ be the vector
$$v \;=\; U_{s-1}U_{s-2}\cdots U_1 U_0 |x0^{m-n} \rangle$$
and write $v$ as $\sum_{y\in \{0,1\}^m} v_y |y \rangle$.
Then
$$
\sum_{y \in \{0,1\}^m \text{ s.t. } y_{m-1}=f(x)} |v_y|^2 \geq \frac{2}{3} \;.
$$
:::

::: { .pause }
Please stop here and see that this definition makes sense.
:::

Once we have the notion of quantum circuits, we can define the quantum analog of $\mathbf{P_{/poly}}$ (i.e., define the class of functions computable by _polynomial size quantum circuits_) as follows:

> # {.definition title="$\mathbf{BQP_{/poly}}$" #QBPpoly}
Let $F:\{0,1\}^* \rightarrow \{0,1\}$.
We say that $F\in \mathbf{BQP_{/poly}}$ if there exists some polynomial $p:\N \rightarrow \N$ such that for every $n\in \N$, if $F_n$ is the restriction of to inputs of length $n$, then there is a quantum circuit of size at most $p(n)$ that computes $F_n$.


::: {.remark title="The obviously exponential fallacy" #exponential}
A priori it might seem "obvious" that quantum computing is exponentially powerful, since to perform a quantum computation on $n$ bits we need to maintain the $2^n$ dimensional state vector and apply $2^n\times 2^n$ matrices to it.
Indeed popular descriptions of quantum computing (too) often say something along the lines that the difference between quantum and classical computer is that a classical bit can either be zero or one while a qubit can be in both states at once, and so in many qubits a quantum computer can perform exponentially many computations at once.

Depending on how you interpret it, this description is either false or would apply equally well to _probabilistic computation_, even though we've already seen that every randomized algorithm can be simulated by a similar-sized circuit, and in fact we conjecture that $\mathbf{BPP}=\mathbf{P}$.

Moreover, this "obvious" approach for simulating a quantum computation will take not just exponential time but _exponential space_ as well, while it is not hard to show that using a simple recursive formula one can calculate the final quantum state using _polynomial space_ (in physics  this is known as "Feynman path integrals").
So, the exponentially long vector description by itself does not imply that quantum computers are exponentially powerful.
Indeed, we cannot _prove_ that they are (i.e., as far as we know, every QNAND program could be simulated by a NAND program with polynomial overhead), but we do have some problems (integer factoring most prominently) for which they do provide exponential speedup over the currently best _known_ classical (deterministic or probabilistic) algorithms.
:::



###  QNAND programs (optional)

Just like in the classical case, there is an equivalence between circuits and straightline programs, and so we can define the programming language QNAND that is the quantum analog of our NAND programming language.
To do so, we only add a single operation: `HAD(foo)` which applies the single-bit operation $H$ to the variable `foo`.
We also use the following interpretation to make `NAND` reversible: `foo = NAND(bar,blah)` means that we modify `foo` to be the XOR of its original value and the NAND of `bar` and `blah`.
(In other words, apply the $8$ by $8$ unitary transformation $U_{NAND}$ defined above to the three qubits corresponding to `foo`, `bar` and `blah`.)
If `foo` is initialized to zero then this makes no difference.

If $P$ is a QNAND program with $n$ input variables, $\ell$ workspace variables, and $m$ output variables, then running it on the input $x\in \{0,1\}^n$ corresponds to setting up a system with $n+m+\ell$ qubits and performing the following process:

1. We initialize the input variables `X[`$0$`]` $\ldots$ `X[`$n-1$`]` to $x_0,\ldots,x_{n-1}$ and all other variables to $0$.

2. We  execute the program line by line,  applying the corresponding physical operation $H$ or $U_{NAND}$ to the qubits that are referred to by the line.

3. We _measure_ the output variables `Y[`$0$`]`, $\ldots$, `Y[`$m-1$`]` and output the result (if there is more than one output then we measure more variables).


This seems quite simple, but maintaining the qubits in a way that we can apply the operations on one hand, but we don't accidentally measure them or corrupt them in another way, is a significant engineering challenge.

### Uniform computation

Just as in the classical case, we can define _uniform_ computational models.
For example, we can define the _QNAND++ programming language_ to be QNAND augmented with loops and arrays just like NAND++  is obtained from NAND.
Using this we can define the class $\mathbf{BQP}$ which is the uniform analog of $\mathbf{BQP_{/poly}}$.
Just as in the classical case $\mathbf{BPP} \subseteq \mathbf{P_{/poly}}$, in the quantum case $\mathbf{BQP} \subseteq \mathbf{BQP_{/poly}}$.
Similarly, people have defined  quantum analogs of Turing machines.

Another way to define $\mathbf{BQP}$ is the following: a function $F:\{0,1\}^* \rightarrow \{0,1\}$ is in $\mathbf{BQP}$ if __(1)__ $F\in \mathbf{BQP_{/poly}}$ and __(2)__ moreover for every $n$, the quantum circuit that verifies this can be generated by a _classical polynomial time NAND++ program_ (or, equivalently, a polynomial-time Turing machine).
We use this definition here, though an equivalent one can be made using QNAND++ or quantum Turing machines:

> # {.definition title="The class $\mathbf{BQP}$" #BQPdef}
Let $F:\{0,1\}^* \rightarrow \{0,1\}$.
We say that $F\in \mathbf{BQP}$ if there exists a polynomial time NAND++ program $P$ such that for every $n$, $P(1^n)$ is the description of a quantum circuit $C_n$ that computes the restriction of $F$ to $\{0,1\}^n$.







> # { .pause }
One way to verify that you've understood these definitions it to see that you can prove __(1)__ $\mathbf{P} \subseteq \mathbf{BQP}$ and in fact the stronger statement $\mathbf{BPP} \subseteq \mathbf{BQP}$, __(2)__  $\mathbf{BQP} \subseteq \mathbf{EXP}$, and __(3)__ For every $\mathbf{NP}$-complete function $F$, if $F\in \mathbf{BQP}$ then $\mathbf{NP} \subseteq \mathbf{BQP}$.

The relation between $\mathbf{NP}$ and $\mathbf{BQP}$ is not known (see also [quantumnp](){.ref}).
It is widely believed that $\mathbf{NP} \not\subseteq \mathbf{BQP}$ but it is  [possible](https://eccc.weizmann.ac.il/report/2018/107/) that they are incomparable, in the sense that $\mathbf{NP} \nsubseteq \mathbf{BQP}$ (and in particular no $\mathbf{NP}$-complete function belongs to $\mathbf{BQP}$) but also $\mathbf{BQP} \nsubseteq \mathbf{NP}$ (and there are some interesting candidates for such problems).


It can be shown that $QNANDEVAL$ (evaluating a quantum circuit on an input) is computable by a polynomial size QNAND program, and moreover this program can even be generated _uniformly_ and hence $QNANDEVAL$ is in $\mathbf{BQP}$.
This allows us to "port" many of the results of classical computational complexity into the quantum realm as well.

::: {.remark title="Restricting attention to circuits" #quantumnonuniformrem}
Because the non uniform model is a little cleaner to work with, in the rest of this chapter we mostly restrict attention to this model, though all the algorithms  we discuss can be implemented in uniform computation as well.
:::









## Physically realizing quantum computation

To realize quantum computation one needs to create a system with $n$ independent binary states (i.e., "qubits"), and be able to manipulate small subsets of two or three of these qubits to change their state.
While by the way we defined operations above it might seem that one needs to be able to perform arbitrary unitary operations on these two or three qubits, it turns out that there several choices for _universal sets_ -  a small constant number of gates that generate all others.
The biggest challenge is how to keep the system from being measured and _collapsing_ to a single classical combination of states.
This is sometimes known as the _coherence time_ of the system.
The [threshold theorem](https://courses.cs.washington.edu/courses/cse599d/06wi/lecturenotes19.pdf) says that there is some absolute constant level of errors $\tau$ so that if errors are created at every gate at rate smaller than $\tau$ then we can recover from those and perform arbitrary long computations.
(Of course there are different ways to model the errors and so there are actually several threshold _theorems_ corresponding to various noise models).

There have been several proposals to build quantum computers:

* [Superconducting quantum computers](https://en.wikipedia.org/wiki/Superconducting_quantum_computing) use super-conducting electric circuits to do quantum computation. This is the direction where   [there has been most recent progress](https://arxiv.org/abs/1709.06678) towards "beating" classical computers.

* [Trapped ion quantum computers](https://en.wikipedia.org/wiki/Trapped_ion_quantum_computer) Use the states of an ion to simulate a qubit. People have made some [recent advances](http://iontrap.umd.edu/wp-content/uploads/2016/02/1602.02840v1.pdf) on these computers too. While it's not at all clear that's the right measuring yard, the [current best implementation](http://arxiv.org/abs/1507.08852) of Shor's algorithm (for factoring 15) is done using an ion-trap computer.

* [Topological quantum computers](https://en.wikipedia.org/wiki/Topological_quantum_computer) use a different technology. Topological qubits are more  stable by design and hence error correction is less of an issue, but constructing  them is extremely challenging.

These approaches are not mutually exclusive and it could be that ultimately quantum computers are built by combining all of them together.
In the near future, it seems that we will not be able to achieve full fledged large scale universal quantum computers, but rather more restricted machines, sometimes called "Noisy Intermediate-Scale Quantum Computers" or  "NISQ".
See [this article by John Preskil](https://arxiv.org/abs/1801.00862) for some of the progress and applications of such more restricted devices.






## Shor's Algorithm

Bell's Inequality is powerful demonstration that there is something very strange going on with quantum mechanics.
But could this "strangeness" be of any use to solve computational problems not directly related to quantum systems?
A priori, one could guess the answer is _no_.
In 1994 Peter Shor showed that one would be wrong:

::: {.theorem title="Shor's Algorithm" #shorthm}
There is a polynomial-time quantum algorithm that on input an integer $M$ (represented in base two), outputs the prime factorization of $M$.
:::


Another way to state [shorthm](){.ref}  is that if we define $FACTORING:\{0,1\}^* \rightarrow \{0,1\}$ to be the function that on input a pair of numbers $(M,X)$ outputs $1$ if and only if $M$ has a factor $P$ such that $2 \leq P \leq X$, then $FACTORING$ is in $\mathbf{BQP}$.
This is an exponential improvement over the best known classical algorithms, which take roughly $2^{\tilde{O}(n^{1/3})}$ time, where the $\tilde{O}$ notation hides factors that are polylogarithmic in $n$.
We will not prove [shorthm](){.ref} in this chapter, will  sketch some of the ideas behind the proof.


### Period finding

At the heart of Shor's Theorem  is an efficient quantum algorithm for finding _periods_ of a given function.
For example, a function $f:\R \rightarrow \R$ is _periodic_ if there is some $h>0$ such that $f(x+h)=f(x)$ for every $x$ (e.g., see [periodicfig](){.ref}).

![Top: A periodic function. Bottom: An a-periodic function.](../figure/periodic_vs_aperiodic.png){#periodicfig .class width=300px height=300px}


_Musical notes_ yield one type of periodic function.
When you pull on a string on a musical instrument, it vibrates in a repeating pattern.
Hence, if we plot the speed of the string (and so how much pressure it exerts on the air around it) as a function of time, it will some  periodic function.
The length of the period is known as the _wave length_ of the note.
The _frequency_ is the number of times the function repeats itself within a unit of time.
For example, the "Middle C" note has a frequency of $261.63$ Hertz, which means its period is $1/(261.63)$ seconds.

If we play a _chord_ by playing several notes at once, we get a more complex periodic function obtained by combining the functions of the three notes.
The human ear contain many small hairs, each of which is sensitive to a narrow band of frequencies.
Hence when we hear the sound corresponding to a chord, our ears actually separate it out to the components corresponding to each frequency (see [timefreqfig](){.ref}).


![Left: The air-pressure when playing a "C Major" chord as a function of time. Right: The coefficients of the Fourier transform of the same function, we can see that it is the sum of three freuencies corresponding to the C, E and G notes (261.63, 329.63 and 392 Hertz respectively). Credit: Bjarke Mønsted's [Quora answer](https://www.quora.com/What-is-the-meaning-of-frequency-domain). ](../figure/timefreq.png){#timefreqfig .class width=300px height=300px}

It turns out that (essentially) _every_ periodic function $f:\R \rightarrow \R$ can be decomposed into a sum of simple _wave_  functions (namely functions of the form $x \mapsto \sin(\theta x)$ or $x \mapsto \cos(\theta x)$).
This is known as  the [Fourier Transform](https://en.wikipedia.org/wiki/Fourier_transform) (see [qfourierfig](){.ref}).
The Fourier transform makes it easy to compute the period of a given function: it will simply be the least common multiple of the periods of the constituent waves.


![If $f$ is a periodic function then when we represent it in the Fourier transform, we expect the coefficients corresponding to wavelengths that do not evenly divide the period to be very small, as they would tend to "cancel out".](../figure/quantum_fourier.jpg){#qfourierfig .class width=300px height=300px}


### Periods over general groups

At a high level, Shor's algorithm uses a  procedure known as the _Quantum Fourier Transform_ to recover the period of a function $F_M$ that is related to the input number $M$, and from this period, obtain the factorization of $M$.
However, $F_M$'s domain will not be the real numbers but rather another _group_ $\mathbb{G}$.
Therefore we will now need to take a short detour to (very basic) _group theory_, and define the notion of periodic functions over groups.




::: {.remark title="Group theory" #grouptheorem}
While we define the concepts we use, some   background in group or number theory might be quite helpful for fully understanding this section.

We will not use anything more than the basic properties of finite Abelian groups.
Specifically we use the following notions:
A finite _group_ $\mathbb{G}$ can be thought of as simply a set of elements and some _binary operation_ $\star$ on these elements (i.e., if $g,h \in \mathbb{G}$ then $g \star h$ is an element of $\mathbb{G}$ as well).
The operation satisfies the sort of properties that a product operation does.
It is associative (i.e., $(g \star h)\star f = g \star (h \star f)$) and there is some element $1$ such that $g \star 1 = g$ for all $g$, where for every $g\in \mathbb{G}$ there exists an element $g^{-1}$ such that $g \star g^{-1} = 1$.
A group is  _Abelian_ or _commutative_ if  $g \star h = h \star g$ for all $g,h \in \mathbb{G}$.
We denote by $g^2$ the element $g\star g$, by $g^3$ the element $g \star g \star g$, and so on and so forth.
The _order_ of $g\in \mathbb{G}$ is the smallest natural number $a$ such that $g^a = 1$.
(It can be shown that such a number exists for every $g$ in a finite group, and moreover that it is always smaller than or equal to the size of the group.)
:::

The heart of Shor's Algorithm is an algorithm that is given as input:

1. A group $\mathbb{G}$, in the sense of a (classical) circuit that on input (representations of) two group elements $g,h \in \mathbb{G}$, outputs the representation of $g \star h$, where $\star$ is the group operation.

2. Am circuit computing a   function $f:\mathbb{G} \rightarrow \{0,1\}^*$ such that there exists a _period_ for $f$ which is a (non identity) group element $h$ such that $f(g \star h) = f(g)$ for every $g\in \mathbb{G}$.

and outputs such a period $h$.
In other words, the heart of Shor's Algorithm is the following lemma:

> # {.lemma #shorlem}
For every (efficiently presented) Abelian group $\mathbb{G}$, there is a quantum polynomial time algorithm that given a _periodic_ function $f:\mathbb{G} \rightarrow \{0,1\}^*$ finds a period of $f$.

### From period finding to factoring (optional)

Using the function $f(a)=g^a$ one can use  period finding (for the group of  $\Z_{|\mathbb{G}|}= \{0,1,2,\ldots,|\mathbb{G}|-1\}$ with modular addition) to find the _order_ of any element $g$ in a group $\mathbb{G}$, namely
the smallest number $a$ such that $g^a = 1$.
It turns out that  order finding can be used to factor integers in polynomial time.^[In fact, it can also be used to solve the _discrete logarithm problem_ over arbitrary Abelian groups, which means that quantum computers will break not only  RSA but also Diffie Hellman and Elliptic Curve Cryptography.]

We now sketch how one reduces the factoring to order finding.
However, the reader can feel free to skip this part, as this involves fairly standard number theoretic calculations, that in particular were known well before Shor.
For a number $M\in \N$, we denote by  $\Z^*_M$ as the group corresponding to the set  $\{ X \in [M]\;|\; gcd(X,M)=1 \}$  with the operation being multiplication modulo $M$. It is known as the _multiplicative group_ modulo $M$.
An _order finding algorithm_ for $\Z^*_M$ is given as input a number $X\in \Z^*_M$, and outputs the smallest positive $a$ such that $X^a = 1 (\mod M)$.
It is polynomial time if it runs in time polynomial in the representation of elements in this group (i.e., polynomial in $\log M$).



> # {.lemma #ordertofact}
Suppose that there is a polynomial-time order finding algorithm for $\Z^*_M$ for every integer $M$.
Then there is a polynomial time algorithm to find the factorization of a given integer $M$.


::: {.proof data-ref="ordertofact"}
We just sketch the proof. We restrict attention to the case that $M=PQ$ for two distinct primes $P$ and $Q$. (This turns out to be the most interesting  case for cryptography anyway.)
In this case, it is known that the multiplicative group modulo $M$, denoted as $\Z^*_M$, has size $(P-1)(Q-1) = M - P - Q - 1$.
Let's denote this size by $\varphi(M)$. If we know $\varphi(M)$ then we can compute the number $S=P+Q = M-1- \varphi(M)$, and from this we can recover both $P$ and $Q$ as the solutions to the quadratic equation $x(S-x)=M$.

Hence it suffices to recover $\varphi(M)$.
Now it turns out that for every $X \in \Z^*_M$, the _order_ of $X$ is a divisor of $\varphi(M)$.
If we sample several random $X_0,\ldots,X_{k-1}$ from $\Z^*_M$ and compute their orders $a_0,\ldots,a_{k-1}$, then $\varphi(M)$ will be a multiple of all of them, and in fact it can be shown that it is very likely to be _least common multiple_ of these numbers.
Using this we can recover $\varphi(M)$ (and from it the factorization of $M$) from the orders of several random numbers in $\{0,\ldots,M-1\}$.^[If we sample a  number $X\in \{0,\ldots,M-1\}$ that turns out not to lie in $\Z^*_M$ then we can recover the factorization of $M$ from the greated common divisor of $X$ and $M$.]
:::



## Finding periods of a function: Simon's Algorithm

As mentioned, the main idea behind Shor's algorithm is to use a tool known as the [quantum fourier transform](https://en.wikipedia.org/wiki/Quantum_Fourier_transform).
This is an algorithm that  given a circuit computing the function $f:\mathbb{H}\rightarrow\R$, creates a quantum state over roughly $\log |\mathbb{H}|$ qubits (and hence dimension $|\mathbb{H}|$) that corresponds to the Fourier transform of $f$.
Hence when we measure this state,  we get a group element $h$ with probability proportional to the square of the corresponding Fourier coefficient.
One can show that if $f$ is $h^*$-periodic then we can recover $h^*$ by taking a polynomial number of samples from this distribution.

::: {.remark title="Quantum Fourier Transform" #QFT}
Despite its name, the Quantum Fourier Transform does _not_ actually give a way to compute the Fourier Transform of a function $f:\mathbb{H} \rightarrow \R$.
This would be impossible to do in $\poly(\log |\mathbb{H}|)$ time, as simply writing down the Fourier Transform would require $|\mathbb{H}|$ coefficients.
Rather the Quantum Fourier Transform gives a _quantum state_ where the amplitude corresponding to an element $h$ is equal to the corresponding Fourier coefficient.
This allows to sample from a distribution where $h$ is drawn with probability proportional to the square of its Fourier coefficient.
This is not the same as computing the Fourier transform, but (as we'll see) is good enough for recovering the period.
:::

Shor carried out this approach for the group $\mathbb{H}=\Z^*_M$ for some $M$  but we will show this for the group $\mathbb{H} = \{0,1\}^n$ with the XOR operation.
This case is known as _Simon's algorithm_ (given by Dan Simon in 1994) and actually preceded (and inspired) Shor's algorithm:

> # {.theorem title="Simon's Algorithm" #simons}
If $f:\{0,1\}^n\rightarrow\{0,1\}^*$ is polynomial time computable and satisfies the property that $f(x)=f(y)$ iff $x\oplus y = h^*$ then there exists
a quantum polynomial-time algorithm that outputs a random $h\in \{0,1\}^n$ such that $\sum_{i=0}^{n-1}h_i h^*_i =0 \mod 2$.

Note that given $O(n)$ such samples, we can recover $h^*$ with high probability by solving the corresponding linear equations.

> # {.proofidea data-ref="simons"}
The idea behind the proof is that the _Hadamard_ operation corresponds to the _Fourier transform_ over the group $\{0,1\}^n$ (with the XOR operations).
We can use that to create quantum state over $n$ qubits where the probability of obtaining some value $h$ is proportional to the  coefficient corresponding to $h$ in the Fourier transform of (a real-valued function  related to)  $f$.
We can show that this coefficient will be zero if $h$ is not orthogonal to the period $h^*$ modulo $2$, and hence when we measuer this state we will obtain some $h$ satisfying $\sum_{i=0}^{n-1}h_ih^*_i = 0 \mod 2$.

> # {.proof data-ref="simons"}
We can express the Hadamard operation $HAD$ as follows:
>
$$ HAD|a\rangle = \tfrac{1}{\sqrt{2}}(|0\rangle+(-1)^a|1\rangle) \;.$$
>
Given the state $|0^{n+m}\rangle$ we can apply this map to each one of the first $n$ qubits to get the state
$$2^{-n/2}\sum_{x\in\{0,1\}^n}|x\rangle|0^m\rangle
$$
and then we can apply the gates of $f$ to map this to the state
$$2^{-n/2}\sum_{x\in\{0,1\}^n}|x\rangle|f(x)\rangle \;.$$
>
Now suppose that we apply the $HAD$ operation again to the first $n$ qubits.
We can see that we get the state
$$2^{-n}\sum_{x\in\{0,1\}^n}\prod_{i=0}^{n-1}(|0\rangle+(-1)^{x_i}|1\rangle)|f(x)\rangle \;.
$$
We can use the distributive law and express a  product of the form $\varphi(x)= \prod_{i=0}^{n-1}(|0\rangle+(-1)^{x_i}|1\rangle)|f(x)\rangle$ as the sum of $2^n$ terms, where each term corresponds to picking  either $|0\rangle$ or $(-1)^{x_i}|1\rangle$.
Another way to say it is that this product $\varphi(x)$ is equal to
$$
\sum_{y\in \{0,1\}^n} \prod_{i=0}^{n-1}(-1)^{x_i y_i}|y_i\rangle |f(x)\rangle \;.
$$
Using the equality $(-1)^a(-1)^b = (-1)^{a+b}$ (and the fact that when raising $-1$ to an integer, we only care if it's odd or even) we get that
$$
\varphi(x)=\sum_{y \in \{0,1\}^n} (-1)^{\sum_{i=0}^{n-1}x_iy_i \mod 2} |y\rangle |f(x) \rangle
$$
and therefore the overall state is equal to
$$
2^{-n}\sum_{x\in \{0,1\}^n} \varphi(x) = 2^{-n}\sum_{x\in\{0,1\}^n}\sum_{y\in\{0,1\}^n}(-1)^{{\sum_{i=0}^{n-1}x_iy_i \mod 2}}|y\rangle|f(x)\rangle \;.
$$
Now under our assumptions for every particular $z$ in the image of $f$, there exist exactly two preimages $x$ and $x\oplus h^*$ such that $f(x)=f(x+h^*)=z$.
So, if $\sum_{i=0}^{n-1}h^*_iy_i =0 \mod 2$, we get that $|(-1)^{\sum_{i=0}^{n-1}x_iy_i =0 \mod 2}+(-1)^{\sum_{i=0}^{n-1}(x_i+h^*_i)y_i =0 \mod 2}|=2$ (positive interference) and otherwise we get $|(-1)^{\sum_{i=0}^{n-1}x_iy_i =0 \mod 2}+(-1)^{\sum_{i=0}^{n-1}(x_i+h^*_i)y_i =0 \mod 2}|=0$  (negative interference, i.e., cancellation).
Therefore, if measure the end state then with probability one we the first $n$ bits will be a  string $y$ such that $\sum_{i=0}^{n-1}y_ih^*_i = 0 \mod 2$.


Simon's algorithm seems to really use the special bit-wise structure of the group $\{0,1\}^n$, so one could wonder if it has any relevance for the group $\Z^*_m$ for some exponentially large $m$.
It turns out that the same insights that underlie the well known  Fast Fourier Transform (FFT) algorithm can be used to essentially follow the same strategy for this group as well.


<!--

## Quantum 101

We now present some of the basic notions in quantum information.
It is very useful to contrast these notions to the setting of _probabilistic_ systems and see how "negative probabilities" make a difference.^[This discussion is somewhat brief. The chapter on quantum computation in my [book with Arora](http://theory.cs.princeton.edu/complexity/) (see [draft here](http://theory.cs.princeton.edu/complexity/ab_quantumchap.pdf)) is one
relatively short resource that contains essentially everything we discuss here.
See also this [blog post of Aaronson](http://www.scottaaronson.com/blog/?p=208) for a high level explanation of Shor's algorithm which ends with links to several more detailed expositions.
See also [this lecture](http://www.scottaaronson.com/democritus/lec14.html) of Aaronson for a great discussion of the feasibility of quantum computing (Aaronson's [course lecture notes](http://www.scottaaronson.com/democritus/default.html) and the [book](http://www.amazon.com/Quantum-Computing-since-Democritus-Aaronson/dp/0521199565) that they spawned are fantastic reads as well).]

__States:__ We will consider a simple quantum system that includes $n$ objects (e.g., electrons/photons/transistors/etc..) each of which can be in either an "on" or "off" state - i.e., each of them can encode a single _bit_ of information, but to emphasize the "quantumness" we will call it a _qubit_.
A _probability distribution_ over such a system can be described as a $2^n$ dimensional vector $v$ with non-negative entries summing up to $1$, where for every $x\in\{0,1\}^n$, $v_x$ denotes the probability that the system is in state $x$.
As we mentioned, quantum mechanics allows negative (in fact even complex) probabilities and so a _quantum state_ of the system can be described as a $2^n$ dimensional vector $v$ such that $\|v\|^2 = \sum_x |v_x|^2 = 1$.

__Measurement:__ Suppose that we were in the classical probabilistic setting, and that the $n$ bits are simply random coins.
Thus we can describe the _state_ of the system by the $2^n$-dimensional vector $v$ such that $v_x=2^{-n}$ for all $x$.
If we _measure_ the system and see what the coins came out, we will get the value $x$ with probability $v_x$.
Naturally, if we measure the system twice we will get the same result.
Thus, after we see that the coin is $x$, the new state of the system _collapses_ to a  vector $v$ such that $v_y = 1$ if $y=x$ and $v_y=0$ if $y\neq x$.
In a quantum state, we do the same thing:  _measuring_ a vector $v$ corresponds to turning it with probability $|v_x|^2$ into a vector that has $1$ on coordinate $x$ and zero on all the other coordinates.


__Operations:__ In the classical probabilistic setting, if we have a system in state $v$ and we apply some function $f:\{0,1\}^n\rightarrow\{0,1\}^n$ then this transforms $v$ to the state $w$ such that $w_y = \sum_{x:f(x)=y} v_x$.
Another way to state this, is that $w=M_f v$ where $M_f$ is the matrix such that $M_{f(x),x}=1$ for all $x$ and all other entries are $0$.
If we toss a coin and decide with probability $1/2$ to apply $f$ and with probability $1/2$ to apply $g$, this corresponds to the matrix $(1/2)M_f + (1/2)M_g$.
More generally, the set of operations that we can apply can be captured as the set of convex combinations of all such matrices- this is simply the set of non-negative matrices whose columns all sum up to $1$- the _stochastic_ matrices.
In the quantum case, the operations we can apply to a quantum state are encoded as a _unitary_ matrix, which is a matrix $M$ such that $\|Mv\|=\|v\|$ for all vectors $v$.

__Elementary operations:__ Of course, even in the probabilistic setting, not every function $f:\{0,1\}^n\rightarrow\{0,1\}^n$ is efficiently computable. We think of a function as efficiently computable if it is composed of polynomially many elementary operations, that involve at most $2$ or $3$ bits or so (i.e., Boolean _gates_).
That is, we say that a matrix $M$ is _elementary_ if it only modifies three bits.
That is,  $M$ is obtained by "lifting" some $8\times 8$ matrix $M'$ that operates on three bits $i,j,k$, leaving all the rest of the bits intact.
Formally, given an $8\times 8$ matrix $M'$ (indexed by strings in $\{0,1\}^3$) and three distinct indices $i<j<k \in \{1,\ldots,n\}$ we define the _$n$-lift of $M'$ with indices $i,j,k$_ to be the $2^n\times 2^n$ matrix $M$ such that for every strings $x$ and $y$ that agree with each other on all coordinates except possibly $i,j,k$, $M_{x,y}=M'_{x_ix_jx_k,y_iy_jy_k}$ and otherwise $M_{x,y}=0$.
Note that if $M'$ is of the form $M'_f$ for some function $f:\{0,1\}^3\rightarrow\{0,1\}^3$ then $M=M_g$ where $g:\{0,1\}^n\rightarrow\{0,1\}^n$ is defined as $g(x)=f(x_ix_jx_k)$.
We define $M$ as an _elementary stochastic matrix_ or a _probabilistic gate_ if $M$ is equal to an $n$ lift of some stochastic $8\times 8$ matrix $M'$.
The quantum case is similar: a _quantum gate_ is a $2^n\times 2^n$ matrix that is an $N$ lift of some unitary $8\times 8$ matrix $M'$.
It is an exercise to prove that lifting preserves stochasticity and unitarity. That is,  every probabilistic gate is a stochastic matrix and every quantum gate is a unitary matrix.

__Complexity:__ For every stochastic matrix $M$ we can define its _randomized complexity_, denoted as $R(M)$ to be the minimum number $T$ such that $M$ is can be (approximately) obtained by combining $T$ elemntary probabilistic gates. To be concrete, we can define $R(M)$ to be the minimum $T$ such that there exists $T$ elementary matrices $M_1,\ldots,M_T$
such that for every $x$, $\sum_y |M_{y,x}-(M_T\cdots M_1)_{y,x}|<0.1$.
(It can be shown that $R(M)$ is finite and in fact at most $10^n$ for every $M$; we can do so by writing $M$ as a convex combination of function and writing every function as a composition of functions that map a single string $x$ to $y$, keeping all other inputs intact.)
We will say that a probabilistic process $M$ mapping distributions on $\{0,1\}^n$ to distributions on $\{0,1\}^n$ is  _efficiently classically computable_ if $R(M) \leq poly(n)$.
If $M$ is a unitary matrix, then we define the _quantum complexity_ of $M$, denoted as $Q(M)$, to be the minimum number $T$ such that there are quantum gates $M_1,\ldots,M_T$ satisfying that for every $x$, $\sum_y |M_{y,x}-(M_T \cdots M_1)_{y,x}|^2 < 0.1$.
We say that $M$ is _efficiently quantumly computable_ if $Q(M)\leq poly(n)$.


__Computing functions:__ We have defined what it means for an operator to be probabilistically or quantumly efficiently computable, but we typically are interested in computing some function $f:\{0,1\}^m\rightarrow\{0,1\}^\ell$.
The idea is that we say that $f$ is efficiently computable if the corresponding operator is efficiently computable, except that we also allow to use extra memory and so to embed $f$ in some $n=poly(m)$.
We define $f$ to be  _efficiently classically computable_ if there is some $n=poly(m)$ such that the operator $M_g$ is efficiently classically computable where $g:\{0,1\}^n\rightarrow\{0,1\}^n$ is defined such that  $g(x_1,\ldots,x_n)=f(x_1,\ldots,x_m)$.
In the quantum case we have a slight twist since the operator $M_g$ is not necessarily a unitary matrix.[^reversible]
Therefore we say that $f$ is _efficiently quantumly computable_  if there is $n=poly(m)$ such that the operator $M_q$ is efficiently quantumly computable where $g:\{0,1\}^n\rightarrow\{0,1\}^n$ is defined as
$g(x_1,\ldots,x_n) = x_1\cdots x_m \|( f(x_1\cdots x_m)0^{n-m-\ell}\; \oplus \; x_{m+1}\cdots x_n)$.

[^reversible]: It is a good exercise to verify that for every $g:\{0,1\}^n\rightarrow\{0,1\}^n$, $M_g$ is unitary if and only if $g$ is a permutation.

**Quantum and classical computation:** The way we defined what it means for a function to be efficiently quantumly computable, it might not be clear that if $f:\{0,1\}^m\rightarrow\{0,1\}^\ell$ is a function that we can compute by a polynomial size Boolean circuit (e.g., combining polynomially many AND, OR and NOT gates) then it is also quantumly efficiently computable.
The idea is that for every gate $g:\{0,1\}^2\rightarrow\{0,1\}$ we can define an $8\times 8$ unitary matrix $M_h$ where $h:\{0,1\}^3\rightarrow\{0,1\}^3$ have the form $h(a,b,c)=a,b,c\oplus g(a,b)$.
So, if $f$ has a circuit of $s$ gates, then we can dedicate an extra bit for every one of these gates and then run the corresponding $s$ unitary operations one by one, at the end of which we will get an operator that computes the mapping $x_1,\ldots,x_{m+\ell+s} = x_1\cdots x_m \| x_{m+1}\cdots x_{m+s} \oplus f(x_1,\ldots,x_m)\|g(x_1\ldots x_m)$ where
the  the $\ell+i^{th}$ bit of $g(x_1,\ldots,x_n)$ is the result of applying the $i^{th}$ gate in the calculation of $f(x_1,\ldots,x_m)$.
So this is "almost" what we wanted except that we have this "extra junk" that we need to get rid of. The idea is that we now simply run the same computation again which will basically we mean we XOR another copy of $g(x_1,\ldots,x_m)$ to the last $s$ bits, but since $g(x)\oplus g(x) = 0^s$ we get that we compute the map $x \mapsto x_1\cdots x_m \| (f(x_1,\ldots,x_m)0^s \;\oplus\; x_{m+1}\cdots x_{m+\ell+s})$ as desired.


[^circuit]: It is a good exercise to show that if $M$ is a probabilistic process with $R(M) \leq T$ then there exists a probabilistic circuit  of size, say, $100 T n^2$ that approximately computes $M$ in the sense that for every input $x$, $\sum_{y\in\{0,1\}^n} \left| \Pr[C(x)=y] - M_{x,y} \right| < 1/3$.

-->




<!--
### Bra-ket notation

Quantum computing is very confusing and counterintuitive for many reasons.
But there is also a "cultural" reason why people sometimes find quantum arguments hard to follow.
Quantum folks follow their own special [notation](https://en.wikipedia.org/wiki/Bra%E2%80%93ket_notation) for vectors.
Many non quantum people find it ugly and confusing, while quantum folks secretly wish they people used it all the time, not just for non-quantum linear algebra, but also for restaurant bills and elemntary school math classes.

The notation is actually not so confusing. If $x\in\{0,1\}^n$ then $|x\rangle$ denotes the $x^{th}$ standard basis vector in $2^n$ dimension.
That is $|x\rangle$  $2^n$-dimensional column vector that has $1$ in the $x^{th}$ coordinate and zero everywhere else.
So, we can describe the column vector that has $\alpha_x$ in the $x^{th}$ entry as $\sum_{x\in\{0,1\}^n} \alpha_x |x\rangle$.
One more piece of notation that is useful is that if $x\in\{0,1\}^n$ and $y\in\{0,1\}^m$ then we identify $|x\rangle|y\rangle$ with $|xy\rangle$ (that is, the $2^{n+m}$ dimensional vector that has $1$ in the coordinate corresponding to the concatenation of $x$ and $y$, and zero everywhere else).
This is more or less all you need to know about this notation to follow this lecture.[^bra]

[^bra]: If you are curious, there is an analog notation for _row_ vectors as $\langle x|$. Generally if $u$ is a vector then $|u\rangle$ would be its form as a column vector and $\langle u|$ would be its form as a row product. Hence since $u^\top v = \iprod{u,v}$ the inner product of $u$ and $b$ can be thought of as $\langle u| |v\rangle$ . The _outer product_ (the matrix whose $i,j$ entry is $u_iv_j$) is denoted as $| u\rangle \langle v|$.

A quantum gate is an operation on at most three bits, and so it can be completely specified by what it does to the $8$ vectors $|000\rangle,\ldots,|111\rangle$.
Quantum states are always unit vectors and so we sometimes omit the normalization for convenience; for example we will identify the state $|0\rangle+|1\rangle$ with its normalized version $\tfrac{1}{\sqrt{2}}|0\rangle + \tfrac{1}{\sqrt{2}}|1\rangle$.

-->





<!--

Now for every $x,y$, the state of the two qubits before measurement is the $4$ dimensional vector:
$v_{x,y} = \tfrac{1}{\sqrt{2}}\left[ R_{x\pi/8}|0\rangle \otimes R_{-y\pi/8}|1\rangle \;+\; R_{x\pi/8}|0\rangle \otimes R_{-y\pi/8}|1\rangle  \right] \;(**)$

If $v \in \mathbb{R\rangle^4$ is the state of the two qubits,[^real] then the probability that we get a particular output $(a,b)$ is simply the dot product squared of $v$ with $|ab\rangle$.
Since $|1\rangle=R_{\pi/2}|0\rangle$, and $\iprod{R_\alpha u,R_\beta u\rangle^2 = \cos^2 (\beta-\alpha)$, we get that for every choice of the coins $x,y$ and $a,b$
the probability that we get $a,b$ as output conditioned on $x,y$ is:

$\tfrac{1}{2}\left[ \cos^2(a\pi/2-x\pi/8)\cos^2(b\pi/2+y\pi/8) + \sin^2(a\pi/2-x\pi/8)\sin^2(b\pi/2+y\pi/8) right]$

One can calculate that if $x=y=0$ then this equals $1$ if $a=b$ and $0$ if $a \neq b$, which implies they win the game with probability $1$.
If $x=y=1$ then this equals
which by calculation yields success probability of at least 0.8 QED

[^real]: In general the state of two qubits is a _complex_ $2^2=4$ dimensional vector but in this case since the initial state was real and our transformations are real, the state will always be a real vector with no imaginary components.
-->

<!--
[Bell's overview paper](http://philosophyfaculty.ucsd.edu/faculty/wuthrich/GSSPP09/Files/BellJohnS1981Speakable_BertlmannsSocks.pdf)
-->

<!--
## Grover's Algorithm

Shor's Algorithm is an amazing achievement, but it only applies to very particular problems.
It does not seem to be relevant to breaking AES, lattice based cryptography, or problems not related to quantum computing at all such as scheduling,  constraint satisfaction, traveling salesperson etc.. etc..
Indeed, for the most general form of these search problems, classically we don't how to do anything much better than brute force search, which takes $2^n$ time over an $n$-bit domain. Lev Grover showed that quantum computers can obtain a quadratic improvement over this brute force search, solving SAT in $2^{n/2}$ time.
The effect of Grover's algorithm on cryptography is fairly mild: one essentially needs to double the key lengths of symmetric primitives.
But beyond cryptography, if large scale quantum computers end up being built, Grover search and its variants might end up being some of the most useful computational problems they will tackle.
Grover's theorem is the following:


> # {.theorem title="Grover search" #groverthm}
There is a quantum $O(2^{n/2}poly(n))$-time algorithm that given a $poly(n)$-sized  circuit computing a function $f:\{0,1\}^n\rightarrow\{0,1\}$ outputs a string
$x^*\in\{0,1\}^n$ such that $f(x^*)=1$.

> # {.proof data-ref="groverthm"}
The proof is not hard but we only sketch it here.
The general idea can be illustrated in the case that there exists a single $x^*$ satisfying $f(x^*)=1$.
(There is a classical reduction from the general case to this problem.)
As in Simon's algorithm, we can efficiently initialize an $n$-qubit system to the uniform state $u = 2^{-n/2}\sum_{x\in\{0,1\}^n}|x\rangle$ which has $2^{-n/2}$ dot product with $|x^*\rangle$. Of course if we measure $u$, we only have probability $(2^{-n/2})^2 = 2^{-n}$ of obtaining the value $x^*$.
Our goal would be to use $O(2^{n/2})$ calls to the oracle to transform the  system to a state $v$ with dot product at least some constant $\epsilon>0$ with the state $|x^*\rangle$.
>
It is an exercise to show that using $Had$ gets we can efficiently compute the unitary operator $U$ such that $Uu = u$ and $Uv = -v$ for every $v$ orthogonal to $u$.
Also, using the circuit for $f$, we can efficiently compute the unitary operator $U^*$ such that $U^*|x\rangle=|x\rangle$ for all $x\neq x^*$ and $U^*|x^*\rangle=-|x^*\rangle$.
It turns out that $O(2^{n/2})$ applications of $UU^*$ to $u$ yield a vector $v$ with $\Omega(1)$ inner product with $|x^*\rangle$.
To see why, consider what these operators do in the two dimensional linear subspace spanned by $u$ and $|x^*\rangle$. (Note that the initial state $u$ is in this subspace and all our operators preserve this property.)
Let $u_\perp$ be the unit vector orthogonal to $u$ in this subspace and let $x^*_\perp$ be the unit vector orthogonal to $|x^*\rangle$ in this subspace.
Restricted to this subspace, $U^*$ is a reflection along the axis $x^*_\perp$ and $U$ is a reflection along the axis $u$.
>
Now, let $\theta$ be the angle between $u$ and $x^*_\perp$.
These vectors are very close to each other and so $\theta$ is very small but not zero - it is equal to $\sin^{-1} 2^{-n/2}$ which is roughly $2^{-n/2}$.
Now if our state $v$ has  angle $\alpha \geq 0$  with $u$, then as long as $\alpha$ is not too large (say $\alpha<\pi/8$) then this means that $v$ has angle
$u+\theta$ with $x^*_\perp$.
That means taht $U^*v$ will have angle $-\alpha-\theta$ with $x^*_\perp$ or $-\alpha-2\theta$ with $u$, and hence $UU^*v$ will have angle
$\alpha+2\theta$ with $u$.
Hence in one application from $UU^*$ we move $2\theta$ radians away from $u$, and in $O(2^{-n/2})$ steps the angle between $u$ and our state will be at least some constant $\epsilon>0$.
Since we live in the two dimensional space spanned by $u$ and $|x\rangle$, it would mean that the dot product of our state and $|x\rangle$ will be at least some constant as well.

-->




## Lecture summary


## Exercises

::: {.remark title="Disclaimer" #disclaimerrem}
Most of the exercises have been written in the summer of 2018 and haven't yet been fully debugged. While I would prefer people do not post online solutions to the exercises, I would greatly appreciate if you let me know of any bugs. You can do so by posting a [GitHub issue](https://github.com/boazbk/tcs/issues) about the exercise, and optionally complement this with an email to me with more details about the attempted solution.
:::

::: {.exercise title="Discrete logarithm from order finding" #dlogfromorder}
Show a probabilistic polynomial time classical algorithm that given an Abelian finite group $\mathbb{G}$ (in the form of an algorithm that computes the group operation), a _generator_ $g$ for the group, and an element $h \in \mathbb{G}$, as well  access to a black box that on input $f\in \mathbb{G}$ outputs the _order_ of $f$ (the smallest $a$ such that $f^a =1$), computes the _discrete logarithm_ of $h$ with respect to $g$.
That is the algorithm should output a number $x$ such that $g^x = h$.
See footnote for hint.^[We are given $h=g^x$ and need to recover $x$. TO do so we can compute the order of various elements of the form $h^ag^b$. The order of such an element is a number $c$ satisfying   $c(xa+b) = 0 \pmod{|\mathbb{G}|}$. With a few random examples we will get a non trivial equation on $x$ (where $c$ is not zero modulo $|\mathbb{G}|$) and then we can use our knowledge of $a,b,c$ to recover $x$.]
:::



## Bibliographical notes

Chapters 9 and 10 in the book _Quantum Computing Since Democritus_ give an informal but highly informative introduction to the topics of this lecture and much more.
Shor's and Simon's algorithms are also covered in Chapter 10 of my book with Arora on computational complexity.

There are many excellent videos available online covering some of these materials.
The Fourier transform is covered in this
videos of [Dr. Chris Geoscience](https://youtu.be/EYRmB1aNh9I?t=19s),  [Clare Zhang](https://www.youtube.com/watch?v=Y9pYHDSxc7g) and [Vi Hart](https://www.youtube.com/watch?v=i_0DXxNeaQ0).
More specifically to quantum computing, the videos of  [Umesh Vazirani on the Quantum Fourier Transform](https://www.youtube.com/watch?v=BM429cOogYc) and  [Kelsey Houston-Edwards on Shor's Algorithm](https://www.youtube.com/watch?v=wUwZZaI5u0c) are very recommended.

Chapter 10 in [Avi Wigderson's book](https://www.math.ias.edu/avi/book) gives a high level overview of quantum computing.
Andrew Childs' [lecture notes on quantum algorithms](http://www.cs.umd.edu/~amchilds/qa/qa.pdf), as well as the lecture notes of [Umesh Vazirani](https://inst.eecs.berkeley.edu/~cs191/),
[John Preskill](http://www.theory.caltech.edu/people/preskill/ph229/), and [John Watrous](https://cs.uwaterloo.ca/~watrous/LectureNotes.html)

Regarding quantum mechanics in general, this
[video](https://www.youtube.com/watch?v=DfPeprQ7oGc) illustrates the double slit experiment,
this [Scientific American video](https://www.youtube.com/watch?v=xM3GOXaci7w)  is a nice exposition of Bell's Theorem.
This [talk and panel](https://youtu.be/GdqC2bVLesQ?t=2m51s) moderated by Brian Greene discusses some of the philosophical and technical issues around quantum mechanics and its so called  "measurement problem".
The [Feynmann lecture on the Fourier Transform](http://www.feynmanlectures.caltech.edu/I_50.html) and [quantum mechanics in general](http://www.feynmanlectures.caltech.edu/III_toc.html) are very much worth reading.

The [Fast Fourier Transform](https://en.wikipedia.org/wiki/Fast_Fourier_transform), used as a component in Shor's algorithm, is one of the most useful algorithms across many applications areas.
The stories of its discovery by Gauss in trying to calculate asteroid orbits and rediscovery by Tukey during  the cold war are fascinating as well.




## Further explorations

Some topics related to this lecture that might be accessible to advanced students include: (to be completed)


## Acknowledgements

Thanks to Scott Aaronson for many helpful comments about this chapter.
