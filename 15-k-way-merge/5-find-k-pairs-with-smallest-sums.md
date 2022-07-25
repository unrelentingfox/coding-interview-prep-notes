# Find k pairs with the smallest sum
This problem follows the K-way merge pattern and we can follow a similar approach as discussed in Merge K Sorted Lists.

We can go through all the numbers of the two input arrays to create pairs and initially insert them all in the heap until we have ‘K’ pairs in Min Heap. After that, if a pair is bigger than the top (smallest) pair in the heap, we can remove the smallest pair and insert this pair in the heap.

We can optimize our algorithms in two ways:
1. Instead of iterating over all the numbers of both arrays, we can iterate only the first ‘K’ numbers from both arrays. Since the arrays are sorted in descending order, the pairs with the maximum sum will be constituted by the first ‘K’ numbers from both the arrays.
2. As soon as we encounter a pair with a sum that is smaller than the smallest (top) element of the heap, we don’t need to process the next elements of the array. Since the arrays are sorted in descending order, we won’t be able to find a pair with a higher sum moving forward.
## Solution
## Code (kotlin)
```kotlin
class Solution {
    fun kSmallestPairs(nums1: IntArray, nums2: IntArray, k: Int): List<List<Int>> {
        val max = PriorityQueue<IntArray>(compareByDescending{it[2]})
        for (i in 0 until minOf(k, nums1.size)) {
            for (j in 0 until minOf(k, nums2.size)) {
                val curr = intArrayOf(nums1[i], nums2[j], nums1[i] + nums2[j])
                if (max.size < k) {
                    max.offer(curr)
                } else {
                    if (curr[2] < max.peek()[2]) {
                        max.poll()
                        max.offer(curr)
                    }
                }
            }
        }
        return max.map { it.dropLast(1) }
    }
}
```
## Code (java)
```java
```
## Complexity
### Time
add k elements to the heap `O(k log k)`
add check and possibly add k^2 elements to the heap `O(k^2 log k)`
`O(k^2 log k)`
### Space
`O(k)`
