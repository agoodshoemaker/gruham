# BFS

## N1: Basic idea

The soul of the BFS is the construction of a tree called the BFS-tree. The algorithm is a description of this construction. Other approaches do not focus on the tree. CLRS, for example, discusses the whole algorithm without referring to the tree. It is only at the end of the exposition that the BFS-tree is invoked. I find the approach presented in CLRS to be cumbersome, though it is an excellent description that is mathematically rigorous.

For a graph $G$, directed or undirected, and a source vertex, this is the BFS algorithm presented as a process to construct a tree called the BFS-tree.

$BFS(G, \text{source})$

- The tree is rooted at `source` and this will be level-$0$.
- Assume that the first $i$ levels of the tree have already been constructed.
- Level-$(i+1)$ of the tree is created as follows:
  - Pick up each node in level-$i$ from left to right.
  - Call a node in this level $u$ and mark it as *visited*.
    - Enumerate all the neighbours of $u$ that haven't been *visited* so far. 
    - Add all these nodes in level-$(i + 1)$ from left to right.



## N2: Preliminary analysis

The first question that pops up is this: does the algorithm terminate? Yes, the algorithm has to terminate as each vertex in $G$ is enumerated at most once. It would be worthwhile to introduce two attributes for each node in the graph:

- `visited`
- `explore`

A node that is visited has already been enumerated or added to one of the levels. A node that is explored is a visited node all of whose neighbours have been visited. A node could find itself in three states:

- not visited
- visited but not explored
- explored

At the end of the algorithm, the claim is that all nodes that are reachable from the $\text{source}$ are explored. Moreover, every node passes through all three stages in chronological order: it starts off as `not visited`, it then becomes `visited but not explored` and finally it is `explored`. In CLRS, these states are colour coded as white, grey and black respectively.



## N3: Implementation

Since nodes are processed from top to bottom, from level-$0$ to level-$h$, and left to right in each level, it becomes clear that the earliest node to be inserted in a level is the first to be picked up for exploration in that particular level. This suggests a queue as natural choice of data structure for storing the visited nodes that are pending exploration. At any stage in the algorithm, the queue consists of visited notes that are yet to be explored (stage-2).
