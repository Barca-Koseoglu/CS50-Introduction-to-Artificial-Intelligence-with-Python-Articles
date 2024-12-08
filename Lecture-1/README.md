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

