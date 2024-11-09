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

# Source Code
We understand how BFS and DFS works in pseudocode, so [here](https://github.com/Barca-Koseoglu/CS50-Introduction-to-Artificial-Intelligence-with-Python-Articles/blob/main/Lecture-0/Lecture%20Source%20Code/maze.py) it is in python.

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

Utilizing this, another informed search algorithm is called *A** search. It expands a node with the lowest value of *g(n)* + *h(n)*, where g(n) is the cost to reach the node and h(n) is the estimated cost to the goal (Manhatten distance). Instead of just considering a heuristic, we consider the cost to reach the node as well. Adding those two and going with the smallest node will give us the shortest path while traversing fewer nodes than BFS.

<img width="248" alt="lecture0-7" src="https://github.com/user-attachments/assets/77023c03-9bc5-449a-84dc-4658958e7e3f">

This would be how our agent would get to point B using the A* algorithm.

A* is optimal if h(n) is admissible (it never overestimates the true cost) and h(n) is consistent (for every node n and successor n' with step cost c, h(n) <= h(n')+c)

A* is fairly easy to implement, but choosing the heurisitc can be a challenge. The better the heurisitc is, the fewer states we'll have to explore., and the better the algorithm gets. The heuristic also has to satisfy the constraints. A* uses quite a bit of memory, but there are other versions of A* with a better heuristic that use less memory.

# Search Algorithms in Adversarial Situations.

Sometimes in search situations, we'll have to deal with **Adversarial Situations**. We have an agent trying to make intelligent decisions, but there's someone else that is fighting against it that has an opposite goal.

For example, take tic-tac-toe. In a game, you try to get three x's or o's in a straight line in a 3x3 grid. Let's say our AI agent is x and goes first. It puts an x in the middle. The opponent, our agent's adversary, puts an o in the top middle. Then, our agent puts an X in the top right. The adversary has the opposite objective of our AI: to make it lose. So the adversary would try to stop our AI from winning, putting an o in the bottom left. But now, X has a clever move, which is to put an x in the middle right. Now x has two ways to win the game. The adversary can't win.

The algorithm used to deal with adversarial situations is called **minimax**.

## Minimax

Computers don't understand what "win" and "lose" means. They just understand what numbers are. They also know if certain numbers are bigger, smaller, or equal to other numbers. So how can we translate a game on a grid to something numerical, something that the computer can understand.

What we can do for a tic-tac-toe game is take the ways that a game might unfold and assign a value to it. O wins (-1), X wins (1), or a draw ensues (0).

We'll call the O player the min player, and the X player the max player. The reason why is because the max player is trying to maximize the score, and the min player is trying to minimize the score. The score, in this case, ranges from -1 to 1. The max player is trying to get to 1 (X winning) while the min player is trying to get to -1 (O winning). But if winning for x (1) is not possible, it still tries to maximize the score, with 0 being the next biggest number (trying to get a draw).

## Tic-Tac-Toe
These are all the parts needed to encode it in an AI:

S<sub>0</sub>: initial state, how the game begins

Player(s): returns which player to move in state s

Actions(s): returns legal moves in state s

Result(s, a): returns state after action a taken in state s, the transition model

Terminal(s): checks if state s is a terminal state, so if the game has ended

Utility(s): final numerical value for terminal state s, to determine who won or lost or if there was a draw

Minimax is a recursive algorithm. Imagine we're in the middle of the game and it's O's turn to make a move. We're O. X is trying to maximize the score, and O is trying to minimize it. Since we're in the middle of the game, we don't yet know the value. We have to consider all legal moves we can make. Then, if the game hasn't ended yet, we go to the new states and look at it from the opposite perspective, trying to maximize our move since it's X's turn. Then, if we get a value of 1, the previous gameboard also has a value of 1, since it can only lead to X winning.

Now, as the O player, we know if we make a move that goes to a max-value path, we will lose. But if we pick the min-value path, we will win, or worst case get a draw.

This is the logic of minimax: take all of the options I can take, then put myself into the opponents shoes and consider what move the opponent would make on their turn, and to do that I consider what move I would make after that and so on and so forth (recursion gets a little hard to explain) until I get all the way down to the end of the game. Then, considering the possible values and perspective of both players, pick the values until the game ends. (top game has a value of 1; x should win)

<img width="332" alt="lecture0-8" src="https://github.com/user-attachments/assets/bfc1f743-05ba-4eb2-a1fa-6a3ac7773d32">

## More generally:

<img width="314" alt="lecture0-9" src="https://github.com/user-attachments/assets/9c5647db-d326-41f9-a253-effeb39a6d6c">

Green arrows try to maximize, orange try to minimize. This is what the minimax algorithm could look like in other games.

### Pseudocode

Given a state s:
- MAX picks action a in Actions(s) that produces highest value of MIN-Value(Result(s, a)) *in order to calculate this, you have to know what the min player could do in the best case, and that requires recursion
- MIN picks action a in Actions(s) that produces smallest value of MAX-Value(Result(s, a))

function MAX-value(state):
  if Terminal(state):
    return Utility(state)
  v = -infinity (want it to be as low as possible to always find something bigger than v)
  for action in Actions(state):
    v = max(v, MIN-Value(Result(state, action)))
  return v

For the MIN player, it's just the exact opposite of everything.

This could be a long process with more complex games, so how can we optomize?

## Optimization

Going through every single option could take a long time, so there are some small tricks to improve the efficiency of minimax.

<img width="345" alt="lecture0-10" src="https://github.com/user-attachments/assets/5d6112a4-dffc-46f6-9fc4-b468ac87b251">

This is a very simple game. Up arrows try to maximize, down arrows try to minimize.

Again, going through each and every single value is going to be inefficient. Here, we first choose an action, lets say going down the leftmost path, and choosing the maximum value possible. If we get to the next path and see a number MIN could pick that is smaller than our biggest value, we can immediately disregard it since our original actions **guarantees** a higher score, since our opponent can't do better than 3 if they want to minimize. Of course, our opponent could be stupid and pick 9 (if we go through the middle path), but that's if we leave it up to chance. Looking purely logically, we can immediately disregard the middle path to guarantee a higher score. This allows us to go through less actions and store less states while also finding the best possible action to take. Then we'd go through the rightmost path with the same logic.

This method of optimization for minimax is called **Alpha-Beta Pruning**. It keeps track of the best you can do so far and the worst you can do so far, and prunning is the idea that we can search a tree more efficiently if we don't have to search through everything by removing the guaranteed worse nodes.

But it still isn't good as games get more complex. Take the question; how many possible tic-tac-toe games are there? 255,168 possible games. Now compare it to chess. After just 4 moves each, there are 288 BILLION chess games that can happen. The total is a mindblowing 10<sup>29000<?sup> games.

This is HORRIBLE for the minimax algorithm.

But how can we find a better approach?

## Depth-Limited Minimax

While the normal minimaz checks every single possible layer, this algorithm only checks up to a certain number of moves and stops, because any other moves after that point could be computationally intractable to consider all the options.

But what do we do after those moves and the game still isn't over?

### Evaluation Function

We use a function that estimates the expected utility of the game from a given state. For example, in chess if 1 means white wins, 0 is a draw, and -1 means black wins, an estimated score of 0.8 means white is likely to win, but it is not guaranteed.


# Credits

This article is completely written by me and demonstrates everything I learned from lecture 0 of the Harvard CS50 Introduction to Artificial Intelligence with Python course on EdX: https://learning.edx.org/course/course-v1:HarvardX+CS50AI+1T2020/home. The photos were taken from the slides provided in the course.

The lecture can also be found on Youtube. https://www.youtube.com/watch?v=WbzNRTTrX0g&t=1s
