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

$$P(X = x_i) = \sum_{j}^{} P(X = x_i, Y = y_i)$$

If we have two random variable, x and y, the probability that x is equal to some value x<sub>i</sub> (which is some value that the variable will take on), then we sum up over j, where j is all the possible values that y can take on. It's essentially the same as the other formula. It finds the probability of x based on x and all values of y summed up.

### Example

Take the joint-probability image again. We wan't to know P(C = cloud), if it's cloudy. Marginalization says that in order to know that, we need to consider the other variable, the idea that it's rainy. It's either raining or not raining, and we just sum up those values. So P(C = cloudy) = P(C = cloud, R = rain) + P(C = cloud, R = !rain) = 0.08 + 0.32 = 0.40.

**Conditioning**: P(a) = P(a|b)P(b) + P(a|!b)P(!b). Very similar to the margenalization rule, but instead we have acces to their conditional probabilities. So if we want to know the probability of a happening, and there's another variable b, either b happens or doesn't happen, and so the probability of a happeneing is the probability that a happens given b and the probability that b happens at all plus the probability that a happens given b doesn't happen times the probability that b doesn't happen. Conditioning also has a rule for multiple possible values.

$$P(X = x_i) = \sum_{j}^{} P(X = x_i | Y = y_i)P(Y = y_i)$$

It's just adding up all those conditional probabilities.

## Models.

Oh boy.

To take all these different ideas of probabilities and represent them inside our AI agent, we use probabalistic models.

## Bayesian Networks

A bayesian network is a data structure that represents the dependencies among random variables. Most variables aren't completely independent of each other; they're all connected in some sort of way. Like if it's rainy today, it might increase the likelihood of my flight or train getting delayed.

A Bayesian network is a directed graph, individual nodes wtih arrows and edges that connect nodes and point in a particular direction. Each node represents a random variable in a Bayesian network. An arrow from X to Y means X is a parent of Y. Each node X has a probability distribution P(X | Parents(x)). An easy way to understand the parents is to imagine it as cause and effect, with the parents being the causes.

### Example:

<img width="184" alt="bayesiannatwork" src="https://github.com/user-attachments/assets/b1148f93-62ca-4984-90b6-1c3a43c908f3" />

Imagine we want to get to an appointment. We either attend it, or miss it. There are a number of factors that this Bayesian network represents, the **nodes** being the different variables. Rain affects Maintenance, Maintenence affects the train whcih is also affected by rain, and the train affects if I get to my appointment. Theres a bunch of dependence on parent nodes, as seen from the graph. The whole idea is to create a probability distribution for all the nodes based on their parents.

Starting with rain, it has no dependencies, no conditions, so it just has some plain probabilities. Let's say the chances are <0.7, 0.2, 0.1>, for none, light, and heavy respectively.

Now let's check out maintenence. Maintenence is yes or no. The heavier the rain is, the less likely it is that there will be maintenence. Now, it'll be a conditional probability.

<img width="173" alt="maintaintrack" src="https://github.com/user-attachments/assets/e4167a13-3147-4ddb-b002-09875536f005" />

Now, we move on to train, which is either on time or delayed. It depends on both the rain and maintenance.

<img width="161" alt="Screenshot 2025-01-25 235028" src="https://github.com/user-attachments/assets/e2b681a9-defa-49dd-9838-bc2cec1baca8" />

Each pair of R and M has a probability of happening of 1, and when they do happen, those are the probabilities you get. You can use real-world data to actually get the data.

Finally, we see my probability of making it to my long-awaited appointment. The other parts of the network don't directly affect if I make it, but they do indirectly affect it. Still, what we do is we just take the probability of being on time and attending, on time and missing, delayed and attending, and delayed and missing.

Now we can get to computing some probabilities.

Take our chances of having light rain, P(light). We can just take the probability.

But what if we wanted more complex joint probabilities? Let's take the probability of light rain and no track maintenance, P(light, no). It would be P(light)P(no|light). The probability of light rain times the probability of no maintenance given light rain. We can keep going with this cycle, like P(light, no, delayed) is P(light)P(no|light)P(delayed|light, no). We say proabbility of delayed given light rain and no maintenance because the train has two parents, but if we also wanted the probability of missing the train, we would just us that entire thing times P(miss|delayed) since it's dependent upon just the rain being delayed or not.

# Inference

Back to the main show.

What we really want to do is to get new pieces of info using inference: given things we know to be True, can we draw conclusions. Now given probabilities, can we figure out other probabilities using inference?

## Inference parts
- Query X: variable for which to compute distribution
- Evidence variables E: observed variables for event e
- Hidden variable Y: non-evidence, non-query variable
- Goal: calculate P(X|e)

### Example:

P(Appointment|light, no)

Our query is Appointment, our observed variables are light rain and no maintenance. The hidden variable is whether or not the train is delayed.

Remember, a conditional probability is proportional to the joint probability. P(a|b) = cP(a,b), where c is a constant. So P(Appointment|light, no) = cP(Appointment,light,no). But we need all four of the variables, including our hidden variable.

And this is where marginalization comes in handy. There's only two ways we can get any configuration of an appointment, Either all this happens and the train is on time, or it's delayed. So the probability becomes c[P(appointment, light, no, on time) + P(appointment, light, no, delayed)].

## Inference by Enumeration

This formula that we way is called inference by enumeration.

$$P(X = e) = aP(X, e) = a\sum_{y}^{} P(X, e, y)$$

X is the query variable, e is the evidence, y ranges over values of hidden variables, and a normalizes the result.
 # Code
 
 The source code is in a folder next to this readme file. Making use of libraries, we can do things like this. We can absolutely make it ourselves, but someone already made it, so it's best to use theirs instead of remaking everything.

## Approximate Inference

This method is actually really slow when it comes to a lot of variables or unknowns. So, we try to approximate our result.

## Sampling

In the process of sampling, we take a sample of all the variables. How? We're going to sample each node according to it's probability distribution. So for rain, we'll use something like a random number generator to pick based on the probabilites. When we move on to maintenance, we'll only sample the probabilities of the one we picked for rain, let's say we picked R = None. Then we keep going. After finishing one sample, we take multiple samples. Then, when faced with a question like P(Train = on time), we can just approximate it instead of finding the exact probability.

<img width="607" alt="sampling" src="https://github.com/user-attachments/assets/c477f26a-692a-49c9-be26-88a01ebbe47c" />

The highligted samples are ones where the train's on time, so in 6/8 cases, that's the likelihood the train is on time. The more samples we take, the better that estimate gets.

But that was an unconditional probability. What about a conditional one? So maybe P(Rain = light | Train = on time). So given the same set of samples, we only care about the ones when the train is on time. And now, looking at the cases when the train is on time, we pick the two where the train also has light rain, giving us 2/6 = 1/3 possible cases (remember, we disregard the samples that were delayed because we don't need them.

This sampling is called rejection sampling, and there are more ways to sample. One problem this type could have is if the evidence we're looking for is very unlikely.

## Likelihood Weighting

Rather than sample everything, we fix the values for the evdience variables, then sample the non-evidence variables using conditional proabbilities in the Bayesian network, then weight each sample by its **likelihood**: the probability of all of the evidence. We got to make sure that if our evidence is extremely unlikely, we account for how unlikely it is.

### Example:

P(Rain = light | train = on time)

We first fix our evidence variable, then go through the same-old process. After sampling the variables, we weight our fixed value based on the evidence, or parents. In this case, it would have a weight of 0.6.

## Uncertainty over time

We've dealt with variables that have probabiltiies, but what if those variables changed over time? For instance, the sky goes from no rain to light rain to heavy rain.

So we have sunny and rainy days; we want to know the probability that it will be sunny tomorrow, the day after that, or the day after that.

## Markov models

To achieve this, we're going to have a randome variable for every possible time step, like using days.

X<sub>t</sub>: Weather at time t. You can see that this might take an insane amount of data, given trying to predict the weather tomorrow with, for example, 3 years of evidence.

When we're trying to do this inference of a computer, reasonably, it's helpful to make simplifying assumptions, even if they aren't truly accurate.

**Markov assumption**: The assumption that the current state depends on only a finite fixed number of previous states. Instead of using all of history's weather data, we can just use yesterday's, for example.

**Markov Chain**: A sequence of random variables where the distribution of each variable follows the Markov assumption. A very simple example is today's weather predicting tomorrow's weather.

<img width="325" alt="todtmr" src="https://github.com/user-attachments/assets/873c9701-953d-483a-9cb7-06c39931f1a6" />

Just taking real-life assumptions, this is a possible way to represent the probabilties of tomorrow's weather. This is called a **Transtition model**. Using it, we can begin to construct a Markov chain.

<img width="593" alt="markovchain" src="https://github.com/user-attachments/assets/e0afadfe-a677-4395-a32d-08a6fdf0c2e9" />

## Sensor Models

In practice, though, knowing for certain the exact state of the world isn't known, but we can sense information somehow, computers and AI can use sensors.

Using sensors, we try to find the hidden state, the underlying True state of the world with observations that AI can make. For example, a hidden state might be a robot's position, if it's exploring unknown territory, but it does have an observation- robot sensor data- to help it sense and collect data on things around it. We can infer from our observations our hidden state, and the hidden state effects our observations.

Another hidden state could be worlds spoken, but AI can only observe audio waveforms. Again, a hidden state is seen in user engagement, and AI observes this with website analytics. Our example is weather, our hidden state is the weather, and just imagine that the only sensor that our AI has is a security camera looking at a building entrance, and we try to infer what the weather is- based on whether or not employees are using umbrellas.

## Hidden Markov Model

A hidden Markov model is a Markov model for a system with hidden states that generate some observed event.

<img width="343" alt="hiidenmark" src="https://github.com/user-attachments/assets/a1a28622-27cc-4d23-b4a8-b596bb978a1d" />

This is literally just using the observation to predict what the underlying state is.

### Sensor Markov Assumption

This is the assumption that the evidence variable depends on only the corresponding state. This means people bringing umbrellas or not is entirely dependent on if it's sunny or rainy today.

<img width="596" alt="superhidemarky" src="https://github.com/user-attachments/assets/add32e8b-ca46-44c2-9fa9-d9f63b040128" />

Instead of sun-sun-rain-rain-rain, we have the upper level, representing the underlying state of the world, and the lower level, showing each of the states of X producing the observation that people brought or did not bring umbrellas.

## Tasks

These are some tasks you might want to do given these kinds of information, like infering stuff about the future or the past of these hidden states.

<img width="340" alt="tasks" src="https://github.com/user-attachments/assets/6e4dd08a-b659-4f98-849b-7e2bd7847819" />

# Credits

This article is completely written by me and demonstrates everything I learned from lecture 1 of the Harvard CS50 Introduction to Artificial Intelligence with Python course on EdX: https://learning.edx.org/course/course-v1:HarvardX+CS50AI+1T2020/home. The photos were taken from the slides provided in the course.

This lecture can also be found on YouTube: https://www.youtube.com/watch?v=D8RRq3TbtHU&t=6869s
