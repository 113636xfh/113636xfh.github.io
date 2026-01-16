---
title: "[Study Notes] Tree-based Search"
date: 2023-12-01
categories:
  - Study Notes
tags:
  - optimization
  - math
  - algorithms
toc_sticky: true
---

Tree-based search algorithms are a family of algorithms that use a tree data structure to systematically explore the state space of a problem to find a solution.

Below are some of the most well-known tree-based search algorithms:

### Depth-First Search (DFS)

DFS explores as far as possible along each branch before backtracking.

```pseudocode
function DFS(node, goalTest):
    if goalTest(node):
        return node
    for child in node.children:
        result = DFS(child, goalTest)
        if result is not null:
            return result
    return null
```

### Breadth-First Search (BFS)

BFS explores all the neighbor nodes at the present depth prior to moving on to nodes at the next depth level.

```pseudocode
function BFS(root, goalTest):
    queue = new Queue()
    queue.enqueue(root)
    while not queue.isEmpty():
        node = queue.dequeue()
        if goalTest(node):
            return node
        for child in node.children:
            queue.enqueue(child)
    return null
```

### Uniform-Cost Search (UCS)

UCS is a variant of Dijkstra's algorithm that expands the least cost node first.

```pseudocode
function UCS(root, goalTest):
    priorityQueue = new PriorityQueue()
    priorityQueue.push(root, 0)
    while not priorityQueue.isEmpty():
        node, cost = priorityQueue.pop()
        if goalTest(node):
            return node
        for child, childCost in node.children:
            priorityQueue.push(child, cost + childCost)
    return null
```

### A* Search

A* search uses a heuristic to estimate the cost from the current node to the goal, and it selects the path that minimizes the sum of the cost so far and this heuristic estimate.

```pseudocode
function AStarSearch(root, goalTest, heuristic):
    priorityQueue = new PriorityQueue()
    root.g = 0
    root.f = heuristic(root)
    priorityQueue.push(root, root.f)
    while not priorityQueue.isEmpty():
        node = priorityQueue.pop()
        if goalTest(node):
            return node
        for child in node.children:
            child.g = node.g + cost(node, child)
            child.f = child.g + heuristic(child)
            priorityQueue.push(child, child.f)
    return null
```

Each of these algorithms has its own advantages and disadvantages, and the choice of algorithm depends on the characteristics of the problem to be solved, such as the size of the state space, the cost of each action, and whether the goal state is known.
