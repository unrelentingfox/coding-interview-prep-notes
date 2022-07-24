# Least Number of Unique Integers after K Removals
https://leetcode.com/problems/least-number-of-unique-integers-after-k-removals/
## Solution
We can use a minHeap and the greedy approach for this problem:
1. calculate frequency of each number
2. add all the numbers to the minHeap
3. keep track of removal count as you poll the heap and add it's frequency to your count
## Code (kotlin)
```kotlin
class Solution {
    fun findLeastNumOfUniqueInts(arr: IntArray, k: Int): Int {
        val freq = arr.toList().groupingBy { it }.eachCount()
        val min = PriorityQueue<Int>()
        freq.entries.forEach { entry ->
            min.offer(entry.value)
        }
        var removals = 0
        while (removals < k && min.isNotEmpty()) {
            min.poll()?.let {
                removals += it
                if (removals > k) min.offer(it)
            }
        }
        return min.size
    }
}
```
## Complexity
### Time
`O(n log n)`
### Space
`O(n)`
