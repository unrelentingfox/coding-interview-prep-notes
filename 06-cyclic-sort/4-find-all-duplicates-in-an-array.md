# Find all duplicates in an array
Given an integer array nums of length n where all the integers of nums are in the range [1, n] and each integer appears once or twice, return an array of all the integers that appears twice.

You must write an algorithm that runs in O(n) time and uses only constant extra space.

Example 1:
> Input: nums = [4,3,2,7,8,2,3,1]  
Output: [2,3]

Example 2:
> Input: nums = [1,1,2]  
Output: [1]

Example 3:
> Input: nums = [1]  
Output: []

Constraints:
* `n == nums.length`
* `1 <= n <= 105`
* `1 <= nums[i] <= n`
* Each element in nums appears once or twice.

## Solution
The solution for this problem is identical to the solution for [#3](./3-find-all-numbers-disappeared-in-an-array.md) except instead of recording the missing numbers, we are recording the duplicates.
## Code
```
class Solution {
    fun findDuplicates(nums: IntArray): List<Int> {
        val duplicates = mutableListOf<Int>()
        var i = 0
        while (i < nums.size) {
            val pos = nums[i] - 1
            if (pos != i && nums[i] != nums[pos]) {
                swap(nums, i, pos)
            } else i++
        }
        for (i in 0 until nums.size) {
            val num = i + 1
            if (num != nums[i]) {
                duplicates.add(nums[i])
            }
        }
        return duplicates
    }

    private fun swap(nums: IntArray, a: Int, b: Int) {
        val swap = nums[b]
        nums[b] = nums[a]
        nums[a] = swap
    }
}
```
## Complexity
### Time
`O(n)`
### Space
`O(1)` not including the output list
