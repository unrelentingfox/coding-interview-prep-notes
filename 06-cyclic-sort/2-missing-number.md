# Missing Number
Given an array nums containing n distinct numbers in the range [0, n], return the only number in the range that is missing from the array.

Example 1:
> Input: nums = [3,0,1]  
Output: 2  
Explanation: n = 3 since there are 3 numbers, so all numbers are in the range [0,3]. 2 is the missing number in the range since it does not appear in nums.

Example 2:
> Input: nums = [0,1]  
Output: 2  
Explanation: n = 2 since there are 2 numbers, so all numbers are in the range [0,2]. 2 is the missing number in the range since it does not appear in nums.

Example 3:
> Input: nums = [9,6,4,2,3,5,7,0,1]  
Output: 8  
Explanation: n = 9 since there are 9 numbers, so all numbers are in the range [0,9]. 8 is the missing number in the range since it does not appear in nums.

## Solution
This problem can be solved using a cyclic sort.

Since the numbers are in the range 0 - n instead of 1 - n, the numbers will equal their index instead of being equal to their index + 1. This means that we will have to ignore n since we can not place it using it as an index. If the list includes n then n will be moved to the position of the missing number
## Code (kotlin)
```
class Solution {
    fun missingNumber(nums: IntArray): Int {
        var i = 0
        while (i < nums.size) {
            if (nums[i] < nums.size && nums[i] != i) {
                swap(nums, nums[i], i)
            } else i++
        }
        for (i in 0 until nums.size) {
            if (nums[i] != i) {
                return i
            }
        }
        return nums.size
    }

    fun swap(nums: IntArray, a: Int, b: Int) {
        val swap = nums[b]
        nums[b] = nums[a]
        nums[a] = swap
    }
}
```
