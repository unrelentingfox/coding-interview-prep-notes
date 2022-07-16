# Connect Sticks
https://leetcode.com/problems/minimum-cost-to-connect-sticks/
## Solution
In this problem, following a greedy approach to connect the smallest sticks first will ensure the lowest cost. We can use a Min Heap to find the smallest sticks following a similar approach as discussed in Kth Smallest Number. Once we connect two sticks, we need to insert the resultant stick back in the Min Heap so that we can connect it with the remaining sticks.
## Code (kotlin)
```kotlin
class Solution {
    fun connectSticks(sticks: IntArray): Int {
        val min = PriorityQueue<Int>()
        sticks.forEach { min.offer(it) }
        var cost = 0
        while (min.size > 1) {
            val combined = min.poll() + min.poll()
            cost += combined
            min.offer(combined)
        }
        return cost
    }
}
```
## Code (java)
```java
class Solution {
    fun connectSticks(sticks: IntArray): Int {
        val min = PriorityQueue<Int>()
        sticks.forEach { min.offer(it) }
        var cost = 0
        while (min.size > 1) {
            val combined = min.poll() + min.poll()
            cost += combined
            min.offer(combined)
        }
        return cost
    }
}
```
## Complexity
### Time
`O(n log n)`
### Space
`O(n)`
