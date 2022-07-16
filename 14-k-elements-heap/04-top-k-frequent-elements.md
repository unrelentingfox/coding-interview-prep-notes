# Top k frequent elements
https://leetcode.com/problems/top-k-frequent-elements/
## Solution
We can solve this using a minheap.
1. calculate frequency map
2. iterate over k map entries adding the elements to the minHeap sorted by frequency
3. iterate over the remaining entries looking for entries where the frequency is larger than the top of the minHeap
## Code (kotlin)
```kotlin
class Solution {
    fun topKFrequent(nums: IntArray, k: Int): IntArray {
        val freq: Map<Int, Int> = nums.toTypedArray().groupingBy { it }.eachCount()
        val min = PriorityQueue<Int>(compareBy { freq[it] })
        freq.entries.forEachIndexed { i, entry ->
            if (i < k) {
                min.offer(entry.key)
            } else if (entry.value > freq[min.peek()]!!) {
                min.poll()
                min.offer(entry.key)
            }
        }
        return min.toIntArray()
    }
}
```
## Complexity
### Time
frequency map = `O(n)`
min heap = `O(n log k)`
`O(n + n log k)`
### Space
`O(n)`
