# Lecture 0: Search

AI uses a bunch of different types of techniques when it does things like recognize a person's face or understand 
human speech. Search is one of them. We want an AI to search for solutions to problems ranging from finding the 
shortest path from point A to B to figuring out how to play a game. We want our AI to be able to know, represent, 
and draw inferences, aka additional conclusions, from information. We also want our AI to be able to deal with 
uncertainty, optimize for a goal, learn from data, use neural networks, and understand human language. This article 
will take a deeper look at search.

## Search

Search problems can come in many different shapes and sizes, like the classic 15 puzzle with the sliding tiles. 
The puzzle initially is mixed up, and we need to find out moves to solve it. Another example is a maze problem. 
A real world problem could be driving directions; what is the best way to get from point A to B considering traffic, 
weather, etc?

#### Some Terms

**Agent**: an entity that percieves its environment and acts upon that environment, like an AI.

**State**: a configuration of the agent and its environment, like a mixed up 15 square puzzle.

**Initial State**: the state in which the agent begins.

**Actions**: choices that can be made in a state. More precisely, the function *Actions(s)* will return the set 
of actions that can be executed in state *s*. For example, in the case of the 15 puzzle, the actions could be to 
move a tile left, right, up, and down.

**Transition Model**: a description of what a state results from performing any applicable action in any state. 
More formally, *Result(s, a)* returns the state resulting from performing action *a* in state *s*.

**State Space**: the set of all states reachable from the initial state by any sequence of actions, like a diagram
of individual states and arrows connecting them. Simplified to a graph, they're nodes and edges that connect nodes. 
Arrows are actions we can take, and nodes are states.

**Goal Test**: a way to determine whether a given state is a goal state, for example, driving directions. Check 
if a state is the same as the address the user typed in. If it is, the problem is solved.

**Path Cost**: a numerical cost associated with a given path. For example, if you wanted directions from point A 
to B and an AI gave you a very long route with lots of unnecessary routes, you would be pretty angry. That's why 
we give every path to a solution a numerical cost, which is a number that shows how expensive it is to take that 
option. We'd like to minimize the path cost. Graphically, the edges can have a number associated with it (weighted 
graphs).

**Solution**: a sequence of actions that leads from the initial state to a goal state.

**Optimal solution**: a solution that has the lowest path cost among all solutions

### Search problems have:
- an initial state,
- actions,
- a transition model,
- a goal test,
- and a path cost functions

# Solving Search Problems

After defining the problem, how can we solve it? Firstly, how can we represent the data? When we try to package 
a bunch of similar data together, we use a data structure called a **node**.

A node keeps track of a bunch of different values, mainly a state, a parent (node that generated this node),
an action (action applied to parent to get node), and a path cost from the initial state to the node.

## Approach:

Simply, we start at one state and explore from there. The intuition is that from a given state, there are 
multiple options we can take, and we're just going to explore those options. We're going to consider all of 
those options and store it into a data structure called frontier (representing everything we can explore next 
that we haven't yet explored).

So we begin with a frontier that contains only the intiai state. Then repeat:
- if the frontier is empty, there is no solution
- Remove a node from the frontier, looking at where we can get to next
- If a node contains a goal state, return the solution
- Expand the node, add resulting nodes to the frontier

## Example:
<img width="142" alt="github-lecture0-image1" src="https://github.com/user-attachments/assets/b0541aeb-84e3-4047-b701-bc2cd296ad31">

How can we solve this? First, we will put node A as the initial state, then repeat the rest of the steps until 
we get the solution. 

Is the frontier empty? No, so we remove node A from the frontier. 

Does node A contain the goal state? No, so we expand the node, adding the resulting node (B) to the frontier.

Then, just repeat those steps. Remove B, put in C and D. Remove C, put in E. Remove D, put in F. Remove E,
and boom. Since E is the goal state, we return E and get our solution.

## So what can go wrong?

Imagine the same problem, but there's also an arrow from B to A.

Starting from A, we can get to B. Then from B, we can get to A, C, and D in the frontier.

If we choose to remove A, then we put B in the frontier again, which results in the frontier soon having A, C, 
D, C, and D in it. If you're not careful, you could go through an infinite loop

To deal with this problem, we have to somehow keep track of what states we've visited. So if we've already 
explored a state, there's no reason to go back to it

## Revised Approach

- start with a frontier that contains the initial state
- start with an empty set, which contains places we've already explored
- Repeat:
  - If the frontier is empty, then no solution
  - Remove a node from the frontier
  - If the node contains the goal state, return the solution
  - Add the node to the explore state
  - Expand the node. Add the reulting nodes to the frontier if they aren't already in the frontier or the set

### But, which node do we remove?

Before, we randomly picked a node to remove. But random isn't dependable or logical. So, we use a **stack**
to store the nodes. Our frontier data structure will be a stack. The last node we put in frontier will be the
first we remove.

## Depth-First Search

The revised approach we used with a stack is a depth-first search algorithm. This means we always expand the 
deepest node in the frontier (last in, first out).

## Breadth-First Search

Very similar to depth first search, except it always expands the shallowest node in the frontier. Basically, 
it uses a **Queue** instead of a stack, which is a first-in first-out data type.

### DFS Example
<img width="250" alt="lecture0-image2" src="https://github.com/user-attachments/assets/e3966d66-132f-41ed-8c35-e019c1e0813f">

We want to get from A to B. Black cells are where our agent can move, light cells are walls. Our agent, A, can 
only use the actions left, right, up, and down.

### DFS solution

Using DFS, once our agent finds a fork in the road, it will continue down one of the sides until it hits a 
dead end. Then, it will go back to the earliest fork and proceed until it reaches a dead end. It continues 
this until it finds B.

As long as there are a finite amount of places, DFS will always work

Will it be optimal? No. It's just checking down a path until it reaches a dead end or the goal state. So, 
only if you're really lucky. It doesn't have a reason for picking one place to go to.

### BFS Example
<img width="151" alt="lecture0-3" src="https://github.com/user-attachments/assets/455e7134-5aa5-48c1-88c4-ece3526badc3">

BFS, visually, looks very different from DFS. It will start by looking at the nodes 1 away from the A, so the 
square on top and to the right. Then it explores the nodes immediately next to those nodes, and so forth.

It's kind of like going through all of the nodes at the same time, bouncing between them.

It does find the optimal solution, but it has to explore a lot of states.

# Code
We understand how BFS and DFS works in pseudocode, so here's and idea of what it would look like using Python

## Human Intuition

DFS wants to exhaust a particular path then move on to the next. BFS is more shallow, trying every step it can take then going from there. But what would a human, like you and I, do?

Using the DFS image, after encountering the first fork in the road, what might a human do? Visually, I'd want to go right since it gets me closer to the endpoint.

We want our AI to be a little more intelligent, trying to make progress towards the goal.

There are two types of search algorithms:
- **Uninformed Search**: search strategy that uses no problem-specific knowledge. BFS and DFS are both examples of this
- **Informed Search**: search strategy that uses problem-specific knowledge to find solutions more efficiently. In the case of the maze, it's like going to the left square because it's graphically (assuming the maze is in a 2D space) closer to the endpoint. Of course, this doesn't guarantee to get the solution better or fast if the maze has only one solution that happens to be a very long path far away from the endpoint. But this reasoning might be very helpful to our AI agent

There're different types of informed search algorithms. One such algorithm is **greedy best-first search**, which is a search algorithm that expands the node that is closest to the goal, as estimated by a heuristic function *h(n)*.

If we don't know exactly what route is closest to the goal, we can use an estimate, otherwise known as a heuristic h(n) take a state of input and returns the estimate of how close the agent is to the goal

### Heuristic Function
<img width="248" alt="lecture0-4" src="https://github.com/user-attachments/assets/b68c614a-f37e-45ab-9755-a4ef14c2845b">

The heurisitc function has to answer questions like which is a better position to be in to get to B, C or D?

Any human could look at this and see that D looks betters than C. But why is D better? If you ignore the walls, D in terms of coordinates is closer to B. The distance between them is shorter than the distance between C and B.

This type of heurisitc is called the Manhattan distance, a count of how many steps veritcally and horizontally it would take to get to the goal, assuming no walls are there. Mathematically, we subtract the x and y value of B with the x and y value of C/D and add them.

D would take only six steps while C would take 14, so D is closer. Now we have an approach: we can pick which mode to remove from the frontier by calculating the shortest Manhattan distances of the nodes. If the node leads to a dead end, then we go on to the next shortest distance node

### Visual representation
<img width="455" alt="lecture0-5" src="https://github.com/user-attachments/assets/f8d2d8ff-bd33-4caa-bf29-b88fdde813bd">

This image shows each box labeled with its Manhatten distance. Note that the distances are estimations, since there are walls we have to consider. As the agent goes, it looks at it's node options and pick the smaller one. One small flaw that could arise is if two nodes are deemed equal and the smallest. If that happens, we have no choice but to just pick one.

Ultimately, we ended up doing better than the BFS and DFS algorithms by taking advantage of this heurisitc.

But is this algorithm **optimal**? Will it always find **the shortest path**?

<img width="247" alt="lecture0-6" src="https://github.com/user-attachments/assets/8e589a3e-e7a3-462c-9853-991e6858a2a5">

Well, no, as this maze provides a counterexample to finding the optimal path. This algorithm find the best decision *locally*.

## Thinking in a different way

We want the shortest path, the **optimal** path. So how can we do this?

Notice in the last image the first fork in the road. the GBFS algorithm went with 11, since it's closer to the end than 13. But the route it ultimately took was not the shortest. Notice something, though. As it went down the route with smaller values, the values gradually went up to a maximum of 12. Once the agent gets there, it has already taken a lot of steps. If it went the other way, it would've landed on 13, but with much fewer steps.

If we take the steps into consideration, the greedy route appears to be way longer.

Utilizing this, another informed search algorithm is called A* search. It expands a node with the lowest value of *g(n)* + *h(n)*, where g(n) is the cost to reach the node and h(n) is the estimated cost to the goal (Manhatten distance).
