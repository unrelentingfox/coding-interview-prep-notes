# Kth largest element in a stream
https://leetcode.com/problems/kth-largest-element-in-a-stream/submissions/
## Solution
This problem follows the Top ‘K’ Elements pattern and shares similarities with Kth Smallest number.

We can follow the same approach as discussed in the ‘Kth Smallest number’ problem. However, we will use a Min Heap (instead of a Max Heap) as we need to find the Kth largest number.
## Code (kotlin)
```kotlin
class KthLargest(private val k: Int, nums: IntArray) {
    private val min = PriorityQueue<Int>()
    init {
        nums.map(this::add)
    }

    fun add(`val`: Int): Int {
        if (min.size < k) {
            min.offer(`val`)
        } else if (`val` > min.peek()) {
            min.poll()
            min.offer(`val`)
        }
        return min.peek()
    }
}
```
## Complexity
### Time
`O(n log k)` for the whole thing, or `O(log k)` for add by it'self
### Space
`O(k)`
