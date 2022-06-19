# First missing positive
Given an unsorted integer array nums, return the smallest missing positive integer.

You must implement an algorithm that runs in O(n) time and uses constant extra space.

Example 1:
> Input: nums = [1,2,0]  
Output: 3

Example 2:
> Input: nums = [3,4,-1,1]  
Output: 2

Example 3:
> Input: nums = [7,8,9,11,12]  
Output: 1

Constraints:
* `1 <= nums.length <= 5 * 105`
* `-231 <= nums[i] <= 231 - 1`

## Solution
This problem follows the cyclic sort pattern pretty clear cut. The only difference being that the numbers are unbounded, meaning they can be less than 0 and larger than the array size. However since we only care about positive numbers within the range `1-(n + 1)` we can simply ignore these numbers. So any number where `0 <= num[i] - 1 < nums.size` holds true we can sort.

Then we can just iterate through our sorted list expecting that `nums[i]` will equal `i + 1`, and if we encounter an `i` where this is not the case, then return `i + 1` as the first missing positive number.

If we iterate through the list finding no missing positive numbers that means that every number from 1 to the size of the array existst in the array. So the first missing positive number is the size of the array + 1.
## Code
```
class Solution {
    fun firstMissingPositive(nums: IntArray): Int {
        var i = 0
        while (i < nums.size) {
            val position = nums[i] - 1
            if (0 <= position && position < nums.size && nums[i] != nums[position]) {
                swap(nums, i, position)
            } else {
                i++
            }
        }
        for (i in 0 until nums.size) {
            val expectedNum = i + 1
            if (nums[i] != expectedNum) {
                return expectedNum
            }
        }
        return nums.size + 1 // all numbers 1-nums.size exist in the array
    }

    private fun swap(nums: IntArray, a: Int, b:Int) {
        nums[a] = nums[b].also{ nums[b] = nums[a] }
    }
}
```
## Complexity
### Time
`O(n)`
### Space
`O(1)`
