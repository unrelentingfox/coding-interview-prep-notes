# Task Scheduling
https://leetcode.com/problems/course-schedule/submissions/
## Solution
In order to determine if we can complete all of the courses is if there is no cycles in the DIGRAPH. To determine this we can use a topological sort.
## Code (kotlin)
```kotlin
class Solution {
    fun canFinish(numCourses: Int, prerequisites: Array<IntArray>): Boolean {
        val graph = prerequisites.groupBy({ it[0] }, { it[1] })
        if (graph.keys.size == numCourses) return false // every course has pre-reqs so we can't start anywhere
        val inDeg = prerequisites.groupingBy { it[1] }.eachCount().toMutableMap()
        val q = ArrayDeque<Int>()
        repeat(numCourses) { if (inDeg[it] == null) q.addLast(it) }
        var count = 0
        while(q.isNotEmpty()) {
            val curr = q.removeFirst()
            count++
            graph[curr]?.forEach { child ->
                inDeg[child]?.let {
                    if (it == 1) {
                        inDeg.remove(child)
                        q.addLast(child)
                    } else inDeg[child] = it - 1
                }
            }
        }
        return count == numCourses
    }
}
```
## Complexity
### Time
`O(v + e)` v is num of courses and e is num of edges
### Space
`O(v + e)`
