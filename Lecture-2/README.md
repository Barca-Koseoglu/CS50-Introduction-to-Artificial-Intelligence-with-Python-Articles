# Lecture 2: Uncertainty

In Lecture 1, I talked about giving our AI representable information and finding new information from it using inference.

In reality, though, very rarely are machines able to know things for sure. Oftentimes, there's uncertainty with the data that our Ai is dealing with where it could believe something with some probability, but nothing for certain. We want to be able to use the information it has SOME knowledge about, not all knowledge, to be able to draw conclusions.

SO take for instance a robot, which has some sensors and explores it environment, doesn't exactly know where it is, but based on the data it collects, it can make inferences on it's environment and where it is.

Take weather, too, because it's very unpredictable. You might not know what the weather will be like tomorrow with 100% certainty, but you can infer some probability what tomorrow's weather will be like.

There's a bunch of games that depend on likelihood of events to happen, like games involving rolling dice.

## Probability

Probability ultimately is the idea of **possible worlds**. Omega (ω) represents those possible worlds. The idea of a possible world is, for example, rolling a die can give you six possible worlds, 1 2 3 4 5 or 6. Each of those possible worlds have some probability of being True. We represent that probability like this: P(ω). This means the probability of some possible world, like the probability we roll a five.

There are some axioms for probability. First and foremost, 0 <= P(ω) <= 1. It's basically a percentage. 0 means something is impossible, like rolling a 7 on a six sided die, and 1 means it will always happen, like rolling a number from 1-6 inclusive has a probabiltity of 1, cause it will always happen.

#### Scary looking math:

$$\sum_{ω∈Ω}^{} P(ω)=1$$

I hope you know what sigma notation is. If you don't it literally just refers to summation. This equation means summing the probabilites of all the possible worlds equals 1. Big omega represents the set of all possible worlds.

Take the dice again, for example. The set of 6 worlds all have an equal probability of being roled, and when adding that probability up, it equals 1. The probability for one of a role is 1/6, where 1 in 6 numbers can be roled. Each number has this probability, 1/6, and adding them all up gives us 1. So P(2) = 1/6.

Now imagine we have two die. We now don't just care about what the individual roll is, but the sum of the two rolls. How de we begin to reason about what the probability looks like?

We could first consider what all the possible world are. In this case, they're just every combination of the two die that are possible. There are 36 possible worlds, and all of them are equally likely. But that doesn't necessarily mean that all of the sums are equally likely. Take the worlds that sum to 3; 1,2 and 2,1 could be the dice roles, and compared to getting a sum of 2, where 1,1 is the only roll, the sum of 3 is more likely.

<img width="586" alt="sumdie" src="https://github.com/user-attachments/assets/81cb6d5b-a513-4140-9721-4fd7d563f537" />

This shows the sums of all possible numbers. Notice how 7 appears more than any other sum, and 1 and 12 appear the least.

P(sum of 12) = 1/36

P(sum of 7) = 6/36 = 1/6

These sorts of judgements are called **undconditional probabilities**

## Uncoditional Probability

An unconditional probability is a degree of belief in a propsition in the absence of any other evidence. If I roll a die, what're the chances I roll a two, for example.

When training our AI, we didn't deal with unconditional probability, we dealt with CONDITIONAL probability, where the is already some degree of belief in a proposition given some evidence that has already been revealed.

## Conditional Probability

P(a|b). The thing on the left-hand side is what we want the probability of. Here, we want the probability of a being True. The righ side of the vertical bar is the evidence, the info we already know about. "What is the probability of A given B?"

P(rain today | rain yesterday), for example. Or, P(route change | traffic conditions). P(disease | test results). Conditional probability comes up everywhere. Unconditional probability is more like "what is the probability they have the disease but with no test results? Just the probability they have the disease?"

So how do we calculate it?

The formula is P(a|b) = P(a^b)/P(b), where ^ means AND. The way to think about this is that we want to find the worlds where A and B are True, and divide that by the number of worlds that B is True, because it doesn't matter if A is True or not, we just care about the worlds where B is True.

Let's, once again, go back to the dice. With the two-dice sum problem, we want to figure out P(sum 12 | one of them being 6). Given that one of the die is 6, what is the probability of the sum being 12? The probability of one of the die being 6 is 1/6, so we only care about the worlds where the die, lets say they're blue and red, of red color is 6. All the other red die rolls are irrelevant. So what's the probability of P(sum of 12 | red 6)? It's 1/36. Using the formula (becaue we only care abou the world's where the red die is 6), we get 1/36 / 1/6 = 6/36 = 1/6.

Another way to write the formula is by getting rid of the fraction, P(a ^ b) = P(b)P(a|b), or P(a^b) = P(a)P(b|a).

## Random Variables

Random variables are variables in probability theory with domains of possible values they can take on. For example, the variable set *roll* can have six possible values, {1,2,3,4,5,6}. *Weather* could be the set of weather, and the things inside these variables don't always have to have the same probability: {sun,cloud,rain,wind,snow}.

If we want to know the probabitlity that the random vairable takes on each of those values, we can use **probability distribution**. A probability distribution might be like P(flight = on time) = 0.6, P(flight = delayed) = 0.3, P(flight = canceled) = 0.1. Summing the all up, you get 1. A more concise way of showing this is using **vectors**, whcih are sequences of values. So P(flight) = <0.6, 0.3, 0.1>. Order does matter.

### Some more ideas to know

**Independence**: The knowledge that one event occurs does not affect the probability of the other event. For example, rolling the red die does not affect the outcome of the blue die. P(a^b) = P(a)P(b|a). The probabiltiy of a and be happening is equal to the probability of a happening times the probability of b happening given a. So given the likelihood of a happening, we can use that info to see how likely it is that b happens. But since we know a doesn't affect b, it's just P(a)P(b). For example, two red rolls are not independent because you can't get two values from rolling red once.

## Bayes' Rule

Let's derive it.

P(a^b) = P(b)P(a|b)

P(a^b) = P(a)P(b|a)

Set the equal to each other. P(a)P(b|a) = P(b)P(a|b)

P(b|a) = P(b)P(a|b)/P(a)

This is Bayes' rule. It's very important because you can use the probability of a given b to figure out the porbability of b given a.

### Example

It's cloudy in the morning, and rainy in the evening. Given clouds in the morning, what's the porbability of rain in the afternoon?

Let's imagine we have access to some information:
- 80% of rainy afternoons start with cloudy mornings.
- 40% of days have cloudy mornings
- 10% of days have rainy afternoons

P(rain|clouds) = P(clouds|rain)P(rain)/P(clouds). Justing doing the math, (.8)(.1)/.4 = .2. Knowing P(visible effect | unknown cause) we can calulate P(unknown cause | visible effect).

## Joint probability

Joint probabiltiy is considering the likelihood of multiple events simultaneously.

<img width="367" alt="jointdistri" src="https://github.com/user-attachments/assets/7d6db85a-aca6-44eb-b45d-ad69830247ca" />

Multiplying them all together gets us their joint probabilities.

Using those probabilities, we can draw other pieces of information about other things like conditional probability.

P(C|rain) = P(C,rain)/P(rain). The comma and AND can be used interchangeably. Dividing by the P of rain is just some numerical constant. Oftentimes, we can just not worry about what the exact value of it is and just know that it is a constant value. Sometimes we can just represent it as alpha (a): aP(C, rain). We just simplified 1/P(rain) into a constant, which is a. So P(C|rain) is proportional to aP(C,rain). a<.08, .02>. We know that probabilities must add up to 1. In this case, they add up to .1; that's where our alpha comes in. we can figure out what alpha is by making it so that a(.1) = 1, so alpha is 10. Multiplying both number by 10 gives us <.8, .2>.

## Probability Rules

**Negation**: P(!a) = 1 - P(a). What's the probablitiy of event a NOT happening. It happens or it doesn't and adding up those porbabilities must give you 1.

**Inclusion-exclusion**: P(a v b) = P(a) + P(b) - P(a ^ b). For example, what's the probability of rolling a 6 with the red dice or blue dice? Noramally, you'd think it's just adding them together, but actually you're overcounting. If red and blue both roll 6, then it we would be counting it as 2 instead of 1.

**Margenalization**: P(a) = P(a,b) + P(a, !b). Given a variable called b we don't know much about, we know it either happens or doesn't happen. So in order to calculate the probability of a, either a happens and b happens or a happens and b doesn't happen.

Sometime b might not just be an event that did or didn't happen, it could be a broader probability distribution where there are multiple possible values. So we don't just sum up b and not b, we sum up all the other possible values it could take on.

$$P(X = x_i) = \sum_{j}^{} P(X = x_i, Y = y_i$$

If we have two random variable, x and y, the probability that x is equal to some value x<sub>i</sub> (which is some value that the variable will take on), then we sum up over j, where j is all the possible values that y can take on. It's essentially the same as the other formula. It finds the probability of x based on x and all values of y summed up.

Here's an example:
