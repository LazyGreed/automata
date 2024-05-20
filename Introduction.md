# Week 01

## Course Outline

### Limitations of software:

* __Undecidable__ - no program can do it (prevent user from writing bad words, etc)
* __Intractable__ - there are programs, but no fast programs (require exponential years of development)

Automata Theory gives us the tools, make us avoid the problems in the first place

### What to expect

1. We'll learn how to deal formally with discrete systems.
    * Proofs - We never really prove a program correct, but we need to be thinking of why a tricky technique really works.

2. We'll gain experience with abstract models and constructions
    * Models layered software architectures

3. Automata Theory - Gateway Drug
    * before UNIX was working on compiling regular expressions

4. Regular Languages and their descriptors
    * deterministic finite automata, non-deterministic finite automata, regular expressions
    * algorithms to decide questions about regular languages, e.g., is it empty?
    * closure properties of regular languages

5. Context-Free Languages and their descriptors
    * Context-free grammars, pushdown automata
    * Decision and closure properties

6. Recursive and recursively enumerable languages
    * Turing machines, decidability of problems
    * The limit of what can be computed

7. Intractable problems
    * Problems that (appear to) require exponential time
    * NP-completeness and beyond

## Informal introduction to finite automata

### What is a Finite Automaton?

* A formal system.
* Remembers only a finite amount of information.
* Information represented by its _state_.
* State changes in response to _inputs_.
* Rules that tell how the state changes in response to inputs are called _transitions_.

### Why study Finite Automaton?

* Used for both design and verification of circuits and communication protocols.
* Used for many text-processing applications.
* An important component of compilers.
* Describes simple patterns of events, etc.

### Example: Tennis

* ___Match___ = 3-5 sets
* ___Set___ = 6 or more games

__Scoring a Game:__
* One person serves throughout the game.
* To win, you must score at least 4 points.
* You also must win by at least 2 points.
* Inputs are s = "server wins point" and o = "opponent wins point".

__Tennis Graph:__ 
![Tennis Graph](https://raw.githubusercontent.com/LazyGreed/automata/main/Tennis_Graph.png)
_Each score is equal to 15 Love (point)_

### Acceptance of Inputs

* Given a sequence of inputs (___input string___), start in the start state and follow the transition from each symbol in turn.
* Input is ___accepted___ if you wind up in a final (accepting) after all inputs have been read.

![Example](https://raw.githubusercontent.com/LazyGreed/automata/main/Processing_String.png)

### Language of an Automaton

* The set of strings accepted by an automaton A is the ___language___ of A. It ___accepts___ the string if it leads from start to the final state. Accepted state is the pseudonym for final state.
* Denoted by L(A).
* Different sets of final states -> different languages.
* __Example:__ As designed, L(Tennis) = strings that determine the winner.

## Deterministic Finite Automata

### Alphabets

* An ___alphabets___ is any finite set of symbols.
* __Example:__
    * ASCII, Unicode
    * {0,1} (binary alphabet)
    * {a,b,c}, {s,o}
    * set of signals used by a protocol

### Strings

* A ___string___ over an alphabet $\Sigma$ is a list, each element of which is a member of $\Sigma$.
    * Strings shown with no commas or quotes, e.g., abc or 01101.
    * $\Sigma^*$ = set of all strings over alphabet $\Sigma$.
    * The ___length___ of a string is its number of positions.
    * $\epsilon$ stands for the ___empty string___ (string of length 0).

#### Example: Strings

* {0,1}$^*$ = {$\epsilon$, 0, 1, 00, 01, 10, 11, 000, 001, ...}
* __Subtlely:__ 0 as a string, 0 as a symbol look the same.
    * Context determines the type.

### Languages

* A ___language___ is a subset of $\Sigma^*$ for some alphabet $\Sigma$.
* __Example:__ The set of strings of 0's and 1's with no two consecutive 1's.
* L = {$\epsilon$, 0, 1, 00, 01, 000, 001, 010, 100, 101, 0000, 0001, 0010, 0100, 0101, 1000, 1001, 1010, ...}

### Deterministic Finite Automata

* A formalism for defining languages, consisting of:
    1. A finite set of ___states___ (Q, typically).
    2. An ___input alphabet___ ($\Sigma$, typically).
    3. A ___transition function___ ($\delta$, typically).
    4. A ___start state___ ($q_0$, in Q, typically).
    5. A set of ___final states___ (F $\subseteq$ Q, typically).
        * "Final" and "accepting" are synonyms.

### The Transition Function

* Takes two arguments: a state and an input symbol.
* $\delta$(q, a) = the state that the DFA goes to when it is in state _q_ and input _a_ is recieved.
* ___Note:___ always a next state &#8211; add a ___dead state___ if no transition.

![Example Dead State](https://raw.githubusercontent.com/LazyGreed/automata/main/dead_state.png)

### Graph Representations of DFA's

* Nodes = states.
* Arcs represent transition function.
    * Arc from state p to state q labeled by all those input symbols that have transitions from p to q.
* Arrow labeled "Start" to the start state.
* Final states indicated by double circles.

#### Example: Recognizing Strings Ending in "ing"

![Strings Ending in "ing"](https://raw.githubusercontent.com/LazyGreed/automata/main/ending_ing.png)

#### Example: Protocol for Sending Data

![Sending Data](https://raw.githubusercontent.com/LazyGreed/automata/main/sending_data.png)

![Sending Data Correct](https://raw.githubusercontent.com/LazyGreed/automata/main/sending_data_correct.png)

#### Example: Strings With No 11

![Strings With No 11](https://raw.githubusercontent.com/LazyGreed/automata/main/string_with_11.png)

#### Alternative Representation: Transition Table

![Strings With No 11 Transition Table](https://raw.githubusercontent.com/LazyGreed/automata/main/string_with_no_11_transition_table.png)

### Convention: Strings and Symbols

* ... w, x, y, z are strings.
* a, b, c, ... are single input symbols.

### Extended Transition Function

* We describe the effect of a string of inputs on a DFA by extending $\delta$ to a state and a string.
* __Intuition:__ Extended $\delta$ is computed for state q and inputs a$_1$a$_2$...a$_n$ by following a path in the transition graph, starting at q and selecting the arcs with labels a$_1$, a$_2$, ..., a$_n$ in turn.

### Inductive Definition of Extended $\delta$

* Induction on length of string.
* __Basis:__ $\delta$(q,$\epsilon$) = q
* __Induction:__ $\delta$(q,wa) = $\delta$($\delta$(q,w),a)
    * __Remember:__ w is a string; a is an input symbol, by convention.

#### Example: Extended Delta

![Extended Delta](https://raw.githubusercontent.com/LazyGreed/automata/main/extended_delta.png)

### Delta-hat

* We don't distinguish between the given delta and the extended delta or delta-hat.
* The reason:
* $\hat\delta$(q,a) = $\delta$($\hat\delta$(q, $\epsilon$),a) = $\delta$(q,a)

### Language of a DFA

* Automata of all kinds define languages.
* If A is an automaton, L(A) is its language.
* For a DFA A, L(A) is the set of strings labeling paths from the start state to a final state.
* __Formally:__ L(A) = the set of strings w such that $\delta$(q$_0$,w) is in F.

#### Example: String in a Language

![String in a Language](https://raw.githubusercontent.com/LazyGreed/automata/main/string_in_a_language.png)

* The language of our example DFA is:
\{w | w is in {0,1} $^*$ and w does not have two consecutive 1's}

### Proofs of Set Equivalence

* Often, we need to prove that two descriptions of sets are in fact the same set.
* Here, one set is "the language of this DFA," and the other is "the set of strings of 0's and 1's with no consecutive 1's."
* In general, to prove S = T, we need to prove two parts: S $\subseteq$ T and T $\subseteq$ S. That is:
    1. If w is in S, then w is in T.
    2. If w is in T, then w is in S.
* Here, S = the language of our running DFA, and T = "no consecutive 1's."

#### Part 1: S $\subseteq$ T

* __To Prove:__ if w is accepted by ![Part 1](https://raw.githubusercontent.com/LazyGreed/automata/main/part_1.png) then w has no consecutive 1's.
* Proof is an induction on length of w.
* __Important trick:__ Expand the inductive hypothesis to be more detailed than the statement you are trying to prove.

##### The Inductive Hypothesis

1. If $\delta$(A,w) = A, then w has no consecutive 1's and does not end in 1.
2. If $\delta$(A,w) = B, then w has no consecutive 1's and  ends in a single 1.
* __Basis:__ |w| = 0; i.e., w = $\epsilon$.
    * (1) holds since $\epsilon$ has no 1's at all.
    * (2) holds ___vacuously___, since $\delta$(A,$\epsilon$) is not B.
        * __Important concept:__ If the "if" part of "if..then" is false, the statement is true.

##### Inductive Step

![Inductive Step](https://raw.githubusercontent.com/LazyGreed/automata/main/part_1.png)

* Assume (1) and (2) are true for strings shorter than w, where |w| is at least 1.
* Because w is not empty, we can write w = xa, where _a_ is the last symbol of w, and x is the string that precedes.
* IH is true for x.
* Need to prove (1) and (2) for w = xa.
* (1) for w is: If $\delta$(A,w) = A, then w has no consecutive 1's and does not end in 1.
* Since $\delta$(A,w) = A, $\delta$(A,x) must be A or B, and _a_ must be 0 (look at the DFA).
* By the IH, x has no 11's.
* Thus, w has no 11's and does not end in 1.
* Now, prove (2) for w = xa: If $\delta$(A,w) = B, then w has no 11's and ends in 1.
* Since $\delta$(A,w) = B, $\delta$(A,x) must be A, and _a_ must be 1 (look at DFA).
* By the IH, x has no 11's and does not end in 1.
* Thus, w has no 11's and ends in 1.

#### Part 2: T $\subseteq$ S

* Now, we must prove: if w has no 11's, then w is accepted by ![Part 2](https://raw.githubusercontent.com/LazyGreed/automata/main/part_1.png) 
* ___Contrapositive:___ If w is __not__ accepted by ![](https://raw.githubusercontent.com/LazyGreed/automata/main/part_1.png) then w has no 11.
    * __Key idea:__ contrapositive of "if X then Y" is the equivalent statement "If not Y then not X."

##### Using the Contrapositive

![](https://raw.githubusercontent.com/LazyGreed/automata/main/part_1.png)

* Because there is a unique transition from every state on every input symbol, each w gets the DFA to exactly one state.
* The only way w is not accepted is if it gets to C.
* The only way to get to C [formally: $\delta$(A,w) = C] is if w = x1y, x gets to B, and y is the tail of w that follows what gets to C for the first time.
* If $\delta$(A,x) = B then surely x = z1 for some z.
* Thus, w = z11y and has 11.

### Regular Languages

* A language L is ___regular___ if it is the language accepted by some DFA.
    * __Note:__ the DFA must accept __only__ the strings in L, no others.
* Some languages are not regular.
    * Intuitively, regular languages "cannot count" to arbitrarily high integers.

#### Example: A Nonregular Language

L$_1$ = \{0$^n$ 1$^n$ | n $\geq$ 1}
* __Note:__ a$^i$ is not conventional for i a's.
    * Thus, 0$^4$ = 0000, e.g.
* __Read:__ "The set of strings consisting of n 0's followed by n 1's, such that n is at least 1."
* Thus, L$_1$ = {01, 0011, 000111, ...}

#### Another Example

L$_2$ = \{w | w in {(, )}$^*$ and w is ___balanced___}
* Balanced parentheses are those sequences of parentheses that can appear in an arithmetic expression.
* E.g.: (), ()(), (()), (()()), ...

#### But Many Languages are Regular

* They appear in many contexts and have many useful properties.
* __Example:__ the strings that represent floating point numbers in your favorite language is a regular language.

#### Example: A Regular Language

L$_3$ = \{w | w in {0,1}$^*$ and w, viewed as a binary integer is divisible by 23}
* The DFA:
    * 23 states, named 0, 1, ..., 22.
    * Correspond to the 23 remainders of an integer divided by 23.
    * Start and only final state is 0.

#### Transitions of the DFA for L$_3$

* If string w represents integer i, then assume $\delta$(0,w) = i%23.
* Then w0 represents integer 2i, so we want $\delta$(i%23,0) = (2i)%23.
* Similarly: w1 represents 2i+1, so we want $\delta$(i%23,1) = (2i+1)%23.
* __Example:__ $\delta$(15,0) = 30%23 = 7; $\delta$(11,1) = 23%23 = 0.

#### Another Example

L$_4$ = \{w | w in {0,1}$^*$ and w, viewed as the __reverse__ of a binary integer is divisible by 23}
* __Example:__ 01110100 is in L$_4$, because its reverse, 00101110 is 46 in binary.
* But there is a theorem that says the reverse of a regular language is also regular.
