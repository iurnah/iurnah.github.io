# Depth-First Search

## Algorithm

DFS explore aggressively along a single path and only backtrack when necessary.
It use a stack and usually implemented using recursion.

```text tab="The psudo code"
DFS(graph G, start vertex s)
-- mark s as explored
-- for every edge (s,v) :
    -- if v unexplored
    -- DFS(G,v)
```

## DFS properties

1. Complexity $\Theta(|V| + |E|)$
2. The "finish time" compute the topological order of a DAG.
3. It build a depth-first tree or forest, CLRS called predecessor subgraph.
4. The discovery and finishing times have parenthesis structure. (parenthesis theorem)
5. Nesting of descendants intervals: $u.d < v.d < v.f < u.f$.
    * $d$: discovery time
    * $f$: finish time.
6. If vertex $v$ is a descendant of vertex $u$, at the time of discovery of $u$,
there is a path from $u$ to $v$ isn't been visited (all white path) (White-path theorem)

## Topological sort

* __Motivation:__ sequence tasks while respecing all precedence constraints.
* If graph G __exists__ a directed cycle, no topological ordering.
* Every directed acyclic graph must have a __sink__ vertex.

```text tab="The psudo code"
DFS-Loop (graph G)
-- mark all nodes unexplored
-- current_label = n [to keep track of ordering]
-- for each vertex
    -- if v not yet explored [in previous DFS call ]
    -- DFS(G,v)

DFS(graph G, start vertex s)
-- for every edge (s,v)
    -- if v not yet explored
    -- mark v explored
    -- DFS(G,v)
-- set f(s) = current_label
-- current_label = current_label - 1
```

One possible improvement is to use three flags to distinguish the `non-visited`,
`visiting`, and `visited` nodes. It detects circle when run into a vertex marked
as `visiting` as the current vertex is in `visiting` state. Because in DAG, the
property of DFS states that __a descendant have to be visited before its
predecessor finished__. For example, DFS start from $s$, and just discovered
vertex $v$, recursive call on vertex $v$ discovred the $s$ in "visiting" state,
this indicate there exists a cycle.

### Course Schedule

### Course Schedule II

* What if the given graph have a single vertex?

### Sequence Reconstruction

* How to check whether the topological sorting is unique or not?
* How to output all the unique topological sorted sequence of vertices?

#### Strongly connected components

## Problem that cannot be solve by BFS and DFS at the same time

## Single source shortest path problems

### Problem definition

$$
\delta(s, v) =
\begin{cases}
  \min\{w(p): p  \text{ of } u \rightarrow v \}) & \text{if } \exists \text{ any such path}\\
 \infty & \text{otherwise (unreachable)} \\
\end{cases}
$$

### Negative edge and Negative edge circle

## Problems

### 339. Nested List Weight Sum

```C++ tab="DFS solution"
/**
 * // This is the interface that allows for creating nested lists.
 * // You should not implement it, or speculate about its implementation
 * class NestedInteger {
 *   public:
 *     // Constructor initializes an empty nested list.
 *     NestedInteger();
 *
 *     // Constructor initializes a single integer.
 *     NestedInteger(int value);
 *
 *     // Return true if this NestedInteger holds a single integer, rather than a nested list.
 *     bool isInteger() const;
 *
 *     // Return the single integer that this NestedInteger holds, if it holds a single integer
 *     // The result is undefined if this NestedInteger holds a nested list
 *     int getInteger() const;
 *
 *     // Set this NestedInteger to hold a single integer.
 *     void setInteger(int value);
 *
 *     // Set this NestedInteger to hold a nested list and adds a nested integer to it.
 *     void add(const NestedInteger &ni);
 *
 *     // Return the nested list that this NestedInteger holds, if it holds a nested list
 *     // The result is undefined if this NestedInteger holds a single integer
 *     const vector<NestedInteger> &getList() const;
 * };
 */
class Solution {
public:
    int depthSum(vector<NestedInteger>& nestedList) {
        return depthSumHelper(nestedList, 1);
    }

    int depthSumHelper(vector<NestedInteger>& nestedList, int depth) {

        int result = 0;
        for (auto curr: nestedList) {
            if (curr.isInteger()) {
                result += (curr.getInteger() * depth);
            } else {
                result += depthSumHelper(curr.getList(), depth + 1);
            }

        }

        return result;
    }
};
```

### 364. Nested List Weight Sum II

```C++ tab="C++"
class Solution {
public:
    int depthSumInverse(vector<NestedInteger>& nestedList) {
        int sum = 0;
        vector<int> results;

        /* 1st place we iterate through a list */
        for (auto nestedInt : nestedList) {
            dfs_helper(nestedInt, 0, results);
        }

        /* calculate the results */
        for (int i = results.size() - 1, level = 1; i >= 0; i--, level++) {
            sum += results[i] * level;
        }

        return sum;
    }

    void dfs_helper(NestedInteger& nestedList, int level, vector<int>& results) {
        if (results.size() < level + 1) results.resize(level + 1);
        if (nestedList.isInteger()) {
            results[level] += nestedList.getInteger();
        } else {
            for (auto ni : nestedList.getList()) { /* 2nd place we iterate through a list */
                dfs_helper(ni, level + 1, results);
            }
        }
    }
};
```

## Reference

1. [Graph Data Structure And Algorithms](http://www.geeksforgeeks.org/graph-data-structure-and-algorithms/) GeeksforGeeks