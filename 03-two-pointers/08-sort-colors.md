# Sort Colors
https://leetcode.com/problems/sort-colors/submissions/
## Solution
The brute force solution will be to use an in-place sorting algorithm like Heapsort which will take `O(N*logN)`. Can we do better than this? Is it possible to sort the array in one iteration?

We can use a Two Pointers approach while iterating through the array. Let’s say the two pointers are called low and high which are pointing to the first and the last element of the array respectively. So while iterating, we will move all 0s before low and all 2s after high so that in the end, all 1s will be between low and high.

## Code (kotlin)
```kotlin
class Solution {
    fun sortColors(nums: IntArray): Unit {
        var s = 0
        var e = nums.size - 1
        var i = s
        while (i <= e) {
            if (nums[i] == 0) {
                swap(nums, i++, s++)
            } else if (nums[i] == 1) {
                i++
            } else if (nums[i] == 2) {
                swap(nums, i, e--)
            }
        }
    }

    private fun swap(nums: IntArray, a: Int, b: Int) {
        val swap = nums[a]
        nums[a] = nums[b]
        nums[b] = swap
    }
}
```
## Code (java)
```java
```
## Complexity
### Time
`O(n)`
### Space
`O(1)`
