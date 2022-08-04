# Topological Sort
Topological Sort of a directed graph (a graph with unidirectional edges) is a linear ordering of its vertices such that for every directed edge (U, V) from vertex U to vertex V, U comes before V in the ordering.

Given a directed graph, find the topological ordering of its vertices.

Example 1:
```
Input: Vertices=4, Edges=[3, 2], [3, 0], [2, 0], [2, 1]
Output: Following are the two valid topological sorts for the given graph:
1) 3, 2, 0, 1
2) 3, 2, 1, 0
```
Example 2:
```
Input: Vertices=5, Edges=[4, 2], [4, 3], [2, 0], [2, 1], [3, 1]
Output: Following are all valid topological sorts for the given graph:
1) 4, 2, 3, 0, 1
2) 4, 3, 2, 0, 1
3) 4, 3, 2, 1, 0
4) 4, 2, 3, 1, 0
5) 4, 2, 0, 3, 1
```
Example 3:
```
Input: Vertices=7, Edges=[6, 4], [6, 2], [5, 3], [5, 4], [3, 0], [3, 1], [3, 2], [4, 1]
Output: Following are all valid topological sorts for the given graph:
1) 5, 6, 3, 4, 0, 1, 2
2) 6, 5, 3, 4, 0, 1, 2
3) 5, 6, 4, 3, 0, 2, 1
4) 6, 5, 4, 3, 0, 1, 2
5) 5, 6, 3, 4, 0, 2, 1
6) 5, 6, 3, 4, 1, 2, 0
```
## Solution
The basic idea behind the topological sort is to provide a partial ordering among the vertices of the graph such that if there is an edge from U to V then U≤V i.e., U comes before V in the ordering. Here are a few fundamental concepts related to topological sort:

1. Source: Any node that has no incoming edge and has only outgoing edges is called a source.

2. Sink: Any node that has only incoming edges and no outgoing edge is called a sink.

3. So, we can say that a topological ordering starts with one of the sources and ends at one of the sinks.

3. A topological ordering is possible only when the graph has no directed cycles, i.e. if the graph is a Directed Acyclic Graph (DAG). If the graph has a cycle, some vertices will have cyclic dependencies which makes it impossible to find a linear ordering among vertices.

To find the topological sort of a graph we can traverse the graph in a Breadth First Search (BFS) way. We will start with all the sources, and in a stepwise fashion, save all sources to a sorted list. We will then remove all sources and their edges from the graph. After the removal of the edges, we will have new sources, so we will repeat the above process until all vertices are visited.

## Code (kotlin)
```kotlin
object Main {
    fun sort(vertices: Int, edges: Array<IntArray>): List<Int> {
        val result = ArrayList<Int>(vertices)
        if (vertices <= 0) return result
        // create graph
        val graph = edges.groupBy({ it[0] }, { it[1] })
        // create inDegree
        val inDegree = edges.groupingBy { it[1] }.eachCount().toMutableMap()
        // add sources
        val sources = ArrayDeque<Int>(vertices)
        repeat(vertices) { i -> if (inDegree[i] == null) sources.add(i) }

        while (sources.isNotEmpty()) {
            val curr = sources.removeFirst()
            result.add(curr)
            graph[curr]?.forEach { child ->
                inDegree[child]?.let {
                    val newVal = it - 1
                    if (newVal == 0) {
                        inDegree.remove(child)
                        sources.add(child)
                    } else {
                        inDegree[child] = newVal
                    }
                }
            }
        }

        return if (result.size != vertices) {
            emptyList() // cannot sort the entire graph so there is a cycle
        } else result
    }

    @JvmStatic
    fun main(args: Array<String>) {
        var result = sort(4, arrayOf(intArrayOf(3, 2), intArrayOf(3, 0), intArrayOf(2, 0), intArrayOf(2, 1)))
        println(result)
        result =
            sort(5, arrayOf(intArrayOf(4, 2), intArrayOf(4, 3), intArrayOf(2, 0), intArrayOf(2, 1), intArrayOf(3, 1)))
        println(result)
        result = sort(
            7,
            arrayOf(
                intArrayOf(6, 4),
                intArrayOf(6, 2),
                intArrayOf(5, 3),
                intArrayOf(5, 4),
                intArrayOf(3, 0),
                intArrayOf(3, 1),
                intArrayOf(3, 2),
                intArrayOf(4, 1)
            )
        )
        println(result)
    }
}
```
## Code (java)
```java
import java.util.*;

class TopologicalSort {
  public static List<Integer> sort(int vertices, int[][] edges) {
    List<Integer> sortedOrder = new ArrayList<>();
    if (vertices <= 0)
      return sortedOrder;

    // a. Initialize the graph
    HashMap<Integer, Integer> inDegree = new HashMap<>(); // count of incoming edges for every vertex
    HashMap<Integer, List<Integer>> graph = new HashMap<>(); // adjacency list graph
    for (int i = 0; i < vertices; i++) {
      inDegree.put(i, 0);
      graph.put(i, new ArrayList<Integer>());
    }

    // b. Build the graph
    for (int i = 0; i < edges.length; i++) {
      int parent = edges[i][0], child = edges[i][1];
      graph.get(parent).add(child); // put the child into it's parent's list
      inDegree.put(child, inDegree.get(child) + 1); // increment child's inDegree
    }

    // c. Find all sources i.e., all vertices with 0 in-degrees
    Queue<Integer> sources = new LinkedList<>();
    for (Map.Entry<Integer, Integer> entry : inDegree.entrySet()) {
      if (entry.getValue() == 0)
        sources.add(entry.getKey());
    }

    // d. For each source, add it to the sortedOrder and subtract one from all of its children's in-degrees
    // if a child's in-degree becomes zero, add it to the sources queue
    while (!sources.isEmpty()) {
      int vertex = sources.poll();
      sortedOrder.add(vertex);
      List<Integer> children = graph.get(vertex); // get the node's children to decrement their in-degrees
      for (int child : children) {
        inDegree.put(child, inDegree.get(child) - 1);
        if (inDegree.get(child) == 0)
          sources.add(child);
      }
    }

    if (sortedOrder.size() != vertices) // topological sort is not possible as the graph has a cycle
      return new ArrayList<>();

    return sortedOrder;
  }

  public static void main(String[] args) {
    List<Integer> result = TopologicalSort.sort(4,
        new int[][] { new int[] { 3, 2 }, new int[] { 3, 0 }, new int[] { 2, 0 }, new int[] { 2, 1 } });
    System.out.println(result);

    result = TopologicalSort.sort(5, new int[][] { new int[] { 4, 2 }, new int[] { 4, 3 }, new int[] { 2, 0 },
        new int[] { 2, 1 }, new int[] { 3, 1 } });
    System.out.println(result);

    result = TopologicalSort.sort(7, new int[][] { new int[] { 6, 4 }, new int[] { 6, 2 }, new int[] { 5, 3 },
        new int[] { 5, 4 }, new int[] { 3, 0 }, new int[] { 3, 1 }, new int[] { 3, 2 }, new int[] { 4, 1 } });
    System.out.println(result);
  }
}
```
## Complexity
### Time
`O(v + e)` where v is the number of vertices and e is the number of edges
### Space
`O(v + e)`
