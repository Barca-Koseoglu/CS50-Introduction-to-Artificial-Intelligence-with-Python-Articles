# Lecture 1: Knowledge

## Knowledge
A lot of intelligence is based on knowledge, especially for human intelligence. People *know* information, and using that information, they're able to draw conclusions and reason about the information in order to figure out how to do something or figure out another piece of information.

With AI, we want to take knowledge and find a way to reason with and act upon it.

**Knowldge-based agents**: agents that reason by operating on internal representations of knowledge.

## What do we mean by reasoning based on knowldge?

Take a look at these three sentences:

*If it didn't rain, Harry visited Hagrid today.*

*Harry visited Hagrid or Dumbledore today, but not both.*

*Harry visited Dumbledore today.*

These are all pieces of informations; facts that we know. We humans can try to reason about this; since Harry visited Dumbledore today, he **didn't visit Hagrid today**. Using this new piece of information we infered from the given information, we can also say that **it rained today**, because if it didn't, Harry would've visited Hagrid, but we **know** Harry didn't visit Hagrid.

This brings the question: how can we make our AI **logical**? How can we make it reason like we can?

One thing to note is that humans normally reason with logic using language. AI, on the other hand, can't actually understand language, so it relies on more formal ways to reason with logic:

**Sentence**: an assertion about the world in a knowldege representation language; it states a fact that AI can understand in its own language.

**Propositional logic**: Logic based on statements about the world. **propositional symbols** are symbols- most likely letters- that represent a fact about the world. For example, *P* might represent the fact that it rained today.

**Logcial connectives**: symbols that help us connect propositional symbols together to reason more facts that might exist. 

### Types of logical connectives:

**NOT (!)**

<img width="150" alt="notpic" src="https://github.com/user-attachments/assets/da468bbf-af6f-446e-9527-b3a3c9d2e8b3">

This is called a truth table, which demonstrates what NOT means when we attach it to a sentences. If P is false, NOT make it true, so P = False, !P == True. If P is True, then !P is False. It essentially makes the sentence the opposite of what it is, kind of like English.

**AND (^)**

<img width="212" alt="andtable" src="https://github.com/user-attachments/assets/ea7fffc3-af35-4981-a8a7-fd3e7546a0f4">

AND **combines** two different sentences together. In the diagram, P and Q are two different sentences. P^Q is True if and only if both P and Q are True. If either one or both are False, then P^Q is False.

**OR (v)**

<img width="207" alt="ortable" src="https://github.com/user-attachments/assets/5b1207a6-0e52-4e21-b93a-2823554db923">

OR also combines two sentences together, written P v Q. If at least one of the sentences are True, then P v Q will be True. If both are False, it will return False.

**Implication (->)**

<img width="371" alt="impgraph" src="https://github.com/user-attachments/assets/de7b4c98-520c-4aa4-b822-aef28a8b2bdd" />

Implication is when P IMPLIES Q. That means P makes a statment about Q. So if P is True, whatever Q is will be the answer. Let's say if it is raining, I will be indoors. This is P -> Q. Where P is True and Q is True. If Q is False, that means that if it's raining, I will be someplace other than indoors, which makes the statement False. Now, if P is False, then it doesnt affect Q at all. P makes no claim at all. If "if it is raining, I will remain indoors" is said, and it isn't raining, then the statment isn't making any claim, so whatever Q is, whether it's staying indoors or outdoors, will be True.

**Biconditional (<->)**

<img width="196" alt="bicon" src="https://github.com/user-attachments/assets/21c78eaa-cc62-43d0-a028-41f9520676c7" />

A Biconditional says "if and only if." So you could say I will be indoors, if and only if it's raining. It demonstrates what will happen if it isn't raining. in this case you won't be indoors no matter what. If I am indoors, it's also reasonable to include that it's also raining. They both imply each other, that's why it's only true if both of them are the same.

These five are going to form the core of propositional logical, the language we use in order to describe ideas and reason abou those ideas.

## Additional terms

**model**: assignment of a truth value to every propositional symbol, creating a "possible world". So if we have P and Q, where P claims it's raining and Q claims it's Tuesday, then the model could show a possible world where P is True and Q is False by assigning them the values (P=True, Q=False). If there are N vairables, the number of possible worlds is 2^n, because each variable can be 2 things, True or False.

**knowledge Base**: A set of sentences known by a knowledge-based agent. We give our AI agent information about the world, like what's True and False.

**Entailment**: α ⊨ β. In every model in which sentence α is True, sentence β is also True. "Alpha entails Beta". This means in every model, in every possible world, this stands. This helps us draw conclusions. So if alpha is something like "I know that it's Tuesday in January", then a reasonable beta could be "I know for sure that it's january". The first sentence *entails* the second statement. It's this idea of entailment that we want. We'd like our AI to infer and draw out conclusions from sentences we give it.

### Inference example

P: it is a Tuesday.

Q: it is raining.

R: Harry will go for a run.

KB (knowledge base): (P ^ !Q) -> R (if it is Tuesday AND it is NOT raining, then that IMPLIES that Harry will go for a run.). P (P is True, it is Tuesday). !Q (Q means it's raining, so in order for !Q to be True, then it can not be raining).

Using these, we can draw inferences. P ^ !Q is only True is both P and !Q are True. We know that both of those statements are True from the knowldge base, so since the left part is all True, that IMPLIES the right part is also True. So we can infer that R is True, and we can add it into our knowldge base. This is the beginning of an **Inference Algorithm**. The question we want to ask, with a given query Alpha, does KB entail alpha? KB ⊨ α? This is equivalent to saying "given the information in KB, can we conclude that Alpha is True?"

## How do we create these algorithms, though?

Turns our there are many different algorithms. One of the simplest algorithms is called **model checking**. Remember, a model is a possible world, where there are many possible worlds where diferent things might be True or False and we can enumerate all of them.

So if our model checking algorithm wants to determine if KB ⊨ α, it will enumerate all possible models and if in every model where KB is True, α is True, then KB entails α. 

<img width="328" alt="adaptation" src="https://github.com/user-attachments/assets/cf8aa2ad-7f4a-499c-bc78-c0b2243b432d" />

Take this, as an example. Our sentences are on the top, our knowledge base is there, and our query is R: Harry will go for a run. When we model check, we see all possible combinations of True and False. Taking all our sentences, there a 8 possible worlds. Now, we gotta find a world that allows our KB to be True and our query to be True. The only one that works is the third one from the bottom. That's because P and Q being False and True satisfies everything in the knowledge base, and the first sentence in our knowledge base, being True, implies that R is True, thus KB entails R.

## Knowledge Engineering

The process of taking a problem and figuring our what propositional symbols to use in order to encode an idea is called knowledge engineering. Software engineers take a problem and try to find out how to represent that knowledge using a computer. If we can take a general problem and break it down into a way that computers understand that with logic, then we can use an inference algorithm like model checking to actually solve it.

### Example: Clue

<img width="325" alt="clue'" src="https://github.com/user-attachments/assets/cdb14f41-c2a0-4fe9-bff5-97e8ce1302e5" />

This is a game where there's a number of different people, a number of different rooms, and a number of different weapons. Three of these; one person, one room, and one weapon, is the solution to the mystery or trying to find the murderer. At the start of the game, one person room and weapon are randomly picked and placed into a sealed envelope. We want to figure out what's inside that envelope. We try to figure it out by looking at some, but not all, of the cards.

We begin by thinking about what propositional symbols we'll need. We can use the names of everything, like mustard, library, etc, as the symbols. Now we can create knowledge. (mustard v plum v scarlet). This encodesthat one of the three here are murderers. We also know (bathroom v kitchen v library). We also can say (knife v revolver v wrench). As the game progresses, people get various cards, and they can use those cards to deduce information. For example, if we have the plum card in our hand, then we know plum can't be the murderer because the card isn't in the envelope. so in this case we might know !plum, the murderer isn't plum.

Let's say a card is revealed that we don't see after making a guess, mustard was in the library and killed with the revolver. That means at least on thing we said couldn't be in the envelope: (!mustard v !library v !revolver).

In code, this would be broken down into ifs and else's and some objects. Basically, adding info after finding a way to represent that info helps us solve the problem. Using the inference algorithm, adding bits of info could help us solve a lot of the problem, especially with things like (!mustard v !library v !revolver) since adding two more bits of info could help us find MORE info from this statement.

### Example: logic puzzles

- Gilderoy, Minerva, Pomona and Horace each belong to a different one of the four houses: Gryffindor, Hufflepuff, Ravenclaw, and Slytherin House.
- Gilderoy belongs to Gryffindor or Ravenclaw
- Pomona does not belong in slytherin
- Minerva belongs to Gryffindor

<img width="262" alt="propsym" src="https://github.com/user-attachments/assets/5582d8a6-6fea-434f-8c7e-580e8313a9fb" />

The propositional symbols this time arounda are more complex. There's a lot of them, signifying the possibility of each one of the kids being in a household.

So we can think of sentences now. The basic notion here tells us if someone is in a school, then that person isn't in any other school and no other person is in that school, for example pomonaslytherin -> !pomonahufflepuff, or minervaravenclaw -> !gilderoyravenclaw. With the info given to us, we could write (gilderoy gryffindor v gilderoyravenclaw). Using these sentences, we can draw inferences about the world.

Using python, a bunch of for loops are used for this, especially the implications.

### Examples: Mastermind

Mastermind is a game in which you try to figure our the order different colors go by trying to make predictions about it. Let's say there's only 4 colors, red green blue and yellow, and they're in some order but we don't know it. We have to guess, and we will be told if it's correct or not. If two of the four for our guess were correct, then we might try to swap two around, and maybe that time none of them would be in the correct position.

We as humans could reason that if none of them are correct now, but two were correct before, then that must mean we can swap the two again and then swap the other pair to get all of them in the proper order.

The way this would be done in python would be to literally put each ordering of the color into a data structure, seeing them as potential solutions, and starting out with an ordering. Once it gets feedback, it cancels out all the options that could absolutely not work and picks another ordering, continuing this cycle until it ends. If it starts out with 1 part correct, it would input (red0 v blue1 v yellow2 v green3), see if that changes anything, then continue with another random guess, using the past predicitons to narrow down future ones.

These are great and all, but model checking is really slow, since it take O(2<sup>n</sup>) time to finish. If we get much more data, time problems will arise.

## Inference Rules

These are rules that we can apply to take knowledge that already exists and translate it to new forms of knowledge.

Imagine a horizontal line. Anything above it is a premise that's True, and anything below the line will be the conclusion that we arrive at after applying logic.

<img width="251" alt="modeus" src="https://github.com/user-attachments/assets/8661269b-c269-4264-91ec-0a816fdf19ab" />

This is how it looks. This inference rule is called **Modus Ponens**. If we know alpha implies beta, aka if alpha then beta, and we also know that alpha is true, then we conclude that beta is also true. This is totally different from model checking, where instead of checking every possible world, we're just dealing with the knowledge we know of.

Another rule is **And Elimination**.

<img width="303" alt="andelm" src="https://github.com/user-attachments/assets/c053df41-b513-4f73-8dcc-9184abf51045" />

If alpha and beta are both true, we can conclude that alpha is true, or beta is true. Literally, if two things are true, one of those things are true. It's very obvious from a human viewpoint, but computers need this kind of info.

This rule is called **Double Negation Elimination**

<img width="329" alt="dnegel" src="https://github.com/user-attachments/assets/e3cbcdcf-e87e-482f-9a14-94601b8f2725" />

If we have two negatives inside our premise, then we can just remove them. !(!a)). Not, not alpha, meaning just alpha

Let's say 'if it's raining, then Harry is inside'. Then we can conlude that either the fact that it isn't raining is True, or the fact that Harry is inside is True. If-then statements are concluded to be or statments. This is called **Implication Elimination**. So if a -> b, then we can conclude that !a v b.

Biconditionals are also eliminable. Take 'it is rainging if and only if harry is inside.' This concludes that if it's raining, then Harry is inside, and if harry is inside, then it's raining. This is called **Biconditional Elimination**. a <-> b concludes that (a -> b) ^ (b -> a).

This logic is literally just taking symbols and turning them into other symbols.

'It is not true that both harry and ron passed the test.' The conclusion that we can draw is that at least one of them did not pass the test. This on'es fancy: **De Morgan's Law**. Basically, we can take the and, change it to an or, and move around the nots. So, !(a ^ b) concludes that !a v !b.

Also De Morgan's law is !(a v b) concludes that !a ^ !b.

**Distributive law**: Like math, (a ^ (b v y)) implies that (a ^ b) v (a ^ y). Also, (a v (b ^ y)) implies that (a v b) ^ (a v y)

### How can we do this, though?

Actual, you'll find the answer in lecture 0: Search. Recall that search problems have an intitial state with actions that take them from one state to another, with a transition model that actually shows you how to get from one state to the other, and a goal test to see if you've reached your goal, and a path cost function to see how many steps you had to take or how costly it was.

We can treat the sentences as states inside of a search problem.

## Theorum Proving

Proving a query to be true uses theorum proving:

- initial state: starting knowledge base
- actions: inference rules
- transition model: new knowledge base after inference
- goal test: check statement we're trying to prove
- path cost function: number of steps in proof

More info on how this actually is a search problem can be found in lecture 0. Search problems are extremely versatile, so solving them is essential.

## Resolution

Resolution is based off of an inference rule: let's say (ron is in the great hall) v (hermione is in the library). Also, ron is not in the great hall. This infers that Hermione must be in the library, since one of the parts of our OR statement was False, the other MUST be True. They're opposites of each other. P v Q and !P, then Q. This is powerful in the sense that P v Q1 v Q2 v ... v Qn and !P implies Q1 v Q2 v ... v Qn. 

We can generalize this further. let's say (ron is in the great hall) v (hermione is in the library) and (ron is not in the great hall) v (harry is sleeping). Based on these two, they both cancel each other out, so we can conlude that (hermione is in the library) v (harry is sleeping). P v Q and !P v R implies Q v R. They could also be multiple symbols

**Clause**: A disjunction of literals. A disjunction means it's a bunch of things that are connected with OR. Like P v Q v R. A literal is a proposiitional symbol, or the opposite of one. This gives us that ability to make sentences known as:

**conjunctive normal form**: Logical sentence that is a conjunction of clauses. Conjunction means AND usage. The clauses have OR, and are connected with AND. So (A v B v C) ^ (D v !E) ^ (F v G), for example. We can turn any sentence in logic into this form.

## Conversion to CNF

- Firstly, eliminate biconditionals using biconditional elimination.
- Eliminate implications using implication elimination.
- Move ! (NOT) inwards using De Morgan's Laws, like !(A ^ B) into !B v !B.
- Then all we're left with is ANDs, so use the distributive law to distribute v wherever possible

### Example

(P v Q) -> R

First, eliminate biconditionals, but there aren't any so we move on.

Second, eliminate implications. We would have !(P v Q) v R.

Then move ! inwards, so we have (!P ^ !Q) v R

Finally, use the distributive law. (!P v R) ^ (!Q v R)

Voila. But why?

Because the clauses are inputs to the reolution, when you have two clauses where there's conflict, you can resolve them to draw a new conclusion, called:

## Inference by Resolution

It's just the same idea. P v Q and !P v R infers Q v R.

P v Q v S and !P v R v S infers Q v R v S, doubles don't matter

What if P and !P? Then, we're left with nothing... it's called an empty clause, and that's always False.

SO

