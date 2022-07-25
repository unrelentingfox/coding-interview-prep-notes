# Smallest range covering elements from k sorted lists
https://leetcode.com/problems/smallest-range-covering-elements-from-k-lists/
## Solution
This problem follows the K-way merge pattern and we can follow a similar approach as discussed in Merge K Sorted Lists.

We can start by inserting the first number from all the arrays in a min-heap. We will keep track of the largest number that we have inserted in the heap (let’s call it currentMaxNumber).

In a loop, we’ll take the smallest (top) element from the min-heap andcurrentMaxNumber has the largest element that we inserted in the heap. If these two numbers give us a smaller range, we’ll update our range. Finally, if the array of the top element has more elements, we’ll insert the next element to the heap.

We can finish searching the minimum range as soon as an array is completed or, in other terms, the heap has less than ‘M’ elements.
## Code (kotlin)
```kotlin
class Solution {
    fun smallestRange(nums: List<List<Int>>): IntArray {
        val minHeap = PriorityQueue<Pair<Int, Int>>(compareBy{(r,c) -> nums[r][c]})
        var max = Int.MIN_VALUE
        repeat(nums.size) { r ->
            val c = 0
            max = maxOf(max, nums[r][c])
            minHeap.offer(r to c)
        }
        var start = 0
        var end = Int.MAX_VALUE
        while (minHeap.size == nums.size) {
            minHeap.poll()?.let { (r,c) ->
                val curr = nums[r][c]
                if (max - curr < end - start) {
                    start = curr
                    end = max
                }
                val nextC = c + 1
                if (nextC < nums[r].size) {
                    max = maxOf(max, nums[r][nextC])
                    minHeap.offer(r to nextC)
                }
            }
        }
        return intArrayOf(start, end)
    }
}
```
## Code (java)
```java
```
## Complexity
### Time
`O(n log m)` n is the total number of elements in m arrays
### Space
`O(m)`
