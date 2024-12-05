# Lecture 1: Knowledge

## Knowledge
A lot of intelligence is based on knowledge, especially for human intelligence. People *know* information, and using that information, they're able to draw conclusions and reason about the information in order to figure out how to do something or figure out another piece of information.

With AI, we want to take knowledge and find a way to reason with and act upon it.

**Knowldge-based agents**: agents that reason by operating on internal representations of knowledge.

## What do we mean by reasoning based on knowldge?

Take a look at these three sentences. Assume they're all true:

*If it didn't rain, Harry visited Hagrid today.*

*Harry visited Hagrid or Dumbledore today, but not both.*

*Harry visited Dumbledore today.*

These are all pieces of informations; facts that we know. We humans can try to reason about this; since Harry visited Dumbledore today, he **didn't visit Hagrid today**. Using this new piece of information we infered from the given information, we can also say that **it rained today**, because if it didn't, Harry would've visited Hagrid, but we **know** Harry didn't visit Hagrid.

This brings the question: how can we make our AI **logical**? How can we make it reason like we can?

One thing to note is that humans normally reason with logic using language. AI, on the other hand, can't actually understand language, so it relies on more formal ways to reason with logic:

**Sentence**: an assertion about the world in a knowldege representation language; it states a fact that AI can understand in its own language.

**Propositional logic**: Logic based on statements about the world. **propositional symbols** are symbols- most likely letters- that represent a fact about the world. For example, *P* might represent the fact that it rained today.

**Logcial connectives**: symbols that help us connect propositional symbols together to reason more facts that might exist. 
