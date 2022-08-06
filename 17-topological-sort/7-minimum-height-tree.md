# Minimum Height Tree
https://leetcode.com/problems/minimum-height-trees/
## Solution
If we consider solving a trivial case where our graph is a path graph (a linked list), then finding the MHT would be as simple as returning the middle of the list (either 1 or 2 nodes).

To solve that case we could use two pointers, one at each end and increment each pointer until the distance between the two pointers is 1 or 2. Those remaining nodes are our answer.

We can use a similar solution when there are multiple paths. We can perform a topological sort from the leaf nodes. Each level of our search will contain all of the leaves, then we remove those leaves and all the new leaves to our queue. We know we have a total of n nodes, so we will perform this traversal until our remaining nodes is `<= 2`

1. Create an adjacency map from the edges, where we add each pair to eachother's adjacency lists.
2. Add all of the leaves to our queue (edges who's adjacency list size is 1)
3. Initialize a counter at n, decrement the counter for each node visited
4. Process the entire set of leaves, for the children of each node, we will remove the parent from the child's adjacency list, then if that list size becomes 1 it is a leaf and we will add it to the next level for our traversal
5. Once our counter becomes 2 or 1 the remaining leaves on the queue will be our solution
### Code (kotlin)
```kotlin
class Solution {
    fun findMinHeightTrees(n: Int, edges: Array<IntArray>): List<Int> {
        if (n == 1) return listOf(0)
        val graph = mutableMapOf<Int, MutableSet<Int>>()
        edges.forEach { (a, b) ->
            graph.getOrPut(a, { mutableSetOf() }).add(b)
            graph.getOrPut(b, { mutableSetOf() }).add(a)
        }
        val q = ArrayDeque<Int>()
        repeat(n) { if (graph[it]?.size == 1) q.add(it) }
        var remainingNodes = n
        while (remainingNodes > 2) {
            val level = q.size
            repeat(level) {
                val curr = q.removeFirst()
                remainingNodes--
                graph[curr]?.forEach { child ->
                    graph[child]?.remove(curr)
                    if (graph[child]?.size == 1) {
                        q.add(child)
                    }
                }
            }
        }
        return q.toList()
    }
}
```
### Complexity
#### Time
`O(n + e)` where n is the number of nodes, and e is the number of edges since we know we are only visiting each node once. However in the constraints of thie problem we know that edges = n - 1 which makes it `O(n + n - 1)` which is asymtotically equivalent to `O(n)`
#### Space
`O(n)`
## Improved speed Array vs Map
Since the nodes are integers, instead of `Map<Int, Set<Int>>` we can use `Array<Set<Int>>` where the indexes of the array are nodes from `0 to n - 1`.

Additionally since we are traversing one level at a time, we don't even need to use a Queue, we could keep a List of leaves, then inside our iteration add new leaves to a separate list, then at the end set our original list to equal the new one.
### Code (kotlin)
```kotlin
class Solution {
    fun findMinHeightTrees(n: Int, edges: Array<IntArray>): List<Int> {
        if (n == 1) return listOf(0)
        val graph = Array<MutableSet<Int>>(n + 1) { HashSet() }
        for ((a,b) in edges) {
            graph[a].add(b)
            graph[b].add(a)
        }
        var leaves = ArrayList<Int>()
        for (i in 0 until n) {
            if (graph[i].size == 1) leaves.add(i)
        }
        var remainingNodes = n
        while (remainingNodes > 2) { // we will end up with either 1 or 2 nodes that are the result
            val newLeaves = ArrayList<Int>()
            for (curr in leaves) {
                remainingNodes--
                for (child in graph[curr]) {
                    graph[child].remove(curr)
                    if (graph[child].size == 1) {
                        newLeaves.add(child)
                    }
                }
            }
            leaves = newLeaves
        }
        return leaves
    }
}
```
