# Optimization

Optimization is about choosing the best option from multiple options.

## Local search

Local search is a search algorithm that maintains a single node and seached by moving to a neighboring node.

This is most applicable when we don't care about a path and we just care about the solution.

For example, imagine we have houses and hospitals. There are a bunch of houses on a 2 by 2 grid and we want to place some hospitals on the grid, thinking about having the houses be the shortest distance away from a hospital.

One way to do this is to use the Manhattan distance (explained in lecture 0). You then add up all those manhattan distances to get a **cost**.

![image](https://github.com/user-attachments/assets/1abfe4a9-475e-4d90-875f-ef37ede34375)

This is an example of a state we can be in. Calculating the manhattan distances, we get 17. We want to try to minimize this cost as much as possible.

You can generalize these problems by thinking about them as a **state-space landscape** problem.

![image](https://github.com/user-attachments/assets/f3761aae-2453-4c4b-a2f1-9c947b47dd00)

Here, each of the vertical bars represent a state we could be in. The height represents some function or value of the state. If we're trying to find a global maximum (the highest bars), we use an objective function to measure how good a given state is. We're then trying to find a global minimum, we use a cost function to see how *bad* something is rather than good. The taller, the worse, whereas for the maximum, the taller the better.

So in a local search, we maintain a single state and move to one of its neigbhbor states. In the case of hospitals, we move a hospital one unit. It has got to be close to our currrent space.

## Hill climbing
The algorithm we use to implement this is called **hill climbing**. We start at a particular node and go to the highest neighbor, or the lowest depedning on if it's an objective or cost function. When we get to a point where both neighbors are higher or lower, we then have our solution.

Some psuedocode:

![image](https://github.com/user-attachments/assets/d96372ae-b19f-4942-9ca1-b7f7cb84a4c0)

Although the hill climbing algorithm might improve your cost, it doesn't always give you the best outcome. The problem is if we try to find the global maxima, we could get stuck at a local maxima (calculus 1 taught me a LOT about maxima). Another problem could be a plateau, a flat space of local maxima. We might get stuck since none of the neighbors are actually better. Even a shoulder could hurt us, where it's just 3 of the same costs.

## Hill climbing variants

![image](https://github.com/user-attachments/assets/ab0eb01a-444d-4490-a0fc-cafae35a5784)

All of these still run the same risk of getting stuck at a fake bext solution, so random restart and local beam search minimize that risk.

I've provided the some code for these from the course resources if you want to check them out. It's in this folder.

## Simulated Annealing

When we get stuck, we want to find a way to dislodge ourselves from the local maxima. This is called **simulated annealing**. It's modeled after the real physical process of annealing where a system of particals gets heated up but after some time it cools down and settles in some final position.

The idea of this is to sometimes make bad moves in order to find a potential global maxima.

Early on, higher "temperature": more likely to accept neighbors that are worse than the current state.

Later on, lower "temperature": less likely to accept neighbors that are worse than the current state.

Pseudocode:

![image](https://github.com/user-attachments/assets/a633bb47-df01-426c-9db3-f521f03c6fdd)

*max* is the amount of times we want to run it by the way.

Again, this allows us to dilodge ourselves if we get stuck at a local maxima.

### Traveling salesman problem

This is a very famous problem in computer science. Imagine we have a bunch of cities represented as dots, and we want to go through all the cities and end up back where we started while minimizing the total distance traveled.

This problem is known as an NP-complete problem, which are problems that have no known efficient way to be solved. They're computationally expensive. SO we're trying to find an approximation of the solution, like something close to the best but not exactly the best.

![image](https://github.com/user-attachments/assets/d16abdfd-9ae3-4372-9b6a-3a32a9e3fd71)

What does it mean to have a neighbor of this state? What we can say is a neighbor is what happens when we pick two edges and switch them. Then we keep doing this until we end up at the best solution we can find.

![image](https://github.com/user-attachments/assets/b14e7ac0-70d5-4e11-af7b-8feba61de086)

## Linear programming

This is another type of problem where we try to optimize a mathematical function.

- We try to minimize a cost function c<sub>1</sub>x<sub>1</sub> + c<sub>2</sub>x<sub>2</sub> + ... + c<sub>n</sub>x<sub>n</sub>
- We do it with constraints of form a<sub>1</sub>x<sub>1</sub> + a<sub>2</sub>x<sub>2</sub> + ... + a<sub>n</sub>x<sub>n</sub> <= b or = b
- It has bounds for each variable l<sub>i</sub> <= x<sub>i</sub> <= u<sub>i</sub>
