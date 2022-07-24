# Kth smallest element in matrix
https://leetcode.com/problems/kth-smallest-element-in-a-sorted-matrix/
## Solution
One solution to this problem would be exactly the same as kth smallest element in an array. Except we just interate through the matrix. However that solution would be O(n log k). We can do better than that.

We can solve this using the k way merge technique and merge the rows together. Except instead of actually merging the rows we will just keep a count of how many values we have seen so far, and return the kth value.
## Code (kotlin)
```kotlin
class Solution {
    fun kthSmallest(matrix: Array<IntArray>, k: Int): Int {
        val min = PriorityQueue<Pair<Int,Int>>(compareBy { (r, c) -> matrix[r][c] } )
        repeat(matrix.size) { r ->
            min.offer(r to 0)
        }
        var count = 1
        while (min.isNotEmpty()) {
            min.poll()?.let { (r, c) ->
                if (count++ < k) {
                    if (c + 1 < matrix.size) min.offer(r to c + 1)
                } else {
                    return matrix[r][c]
                }
            }
        }
        throw IllegalArgumentException("Expected atleast k elemenets")
    }
}
```
## Complexity
### Time
`O(k log n)` where n is the number of input arrays
### Space
`O(n)`
