# Task Scheduling Order
https://leetcode.com/problems/course-schedule-ii/submissions/
## Solution
We can find a proper course schedule order by performing a topological sort like the last problem.

The only difference here that is important is we need to reverse the order of the dependency in order to traverse it correctly. Since we are given a list of course -> preReq. That means we need to take the preReqs first. So our graph is preReq -> course.
## Code (kotlin)
```kotlin
class Solution {
    fun findOrder(numCourses: Int, prerequisites: Array<IntArray>): IntArray {
        val graph = prerequisites.groupBy( { (_, preReq) -> preReq }, { (course, _) -> course } ) // preReq to courses it unlocks
        if (graph.keys.size == numCourses) return intArrayOf() // all courses are preReqs
        val preReqCount = prerequisites.groupingBy { (course, _) -> course }.eachCount().toMutableMap() // count of each courses preReqs
        val q = ArrayDeque<Int>()
        repeat(numCourses) { if (preReqCount[it] == null) q.addLast(it) } // add courses with no preReqs
        val order = ArrayList<Int>(numCourses)
        while (q.isNotEmpty()) {
            val preReq = q.removeFirst()
            order.add(preReq)
            graph[preReq]?.forEach { course ->
                val count = preReqCount[course] ?: 0
                if (count > 1) {
                    preReqCount[course] = count - 1
                } else {
                    preReqCount.remove(course)
                    q.addLast(course)
                }
            }
        }
        return if (order.size != numCourses) intArrayOf() else order.toIntArray()
    }
}
```
## Complexity
### Time
`O(v + e)` v is num of courses and e is num of edges
### Space
`O(v + e)`
