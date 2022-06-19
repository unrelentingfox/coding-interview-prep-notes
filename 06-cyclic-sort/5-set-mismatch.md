# Set Mismatch
You have a set of integers s, which originally contains all the numbers from 1 to n. Unfortunately, due to some error, one of the numbers in s got duplicated to another number in the set, which results in repetition of one number and loss of another number.

You are given an integer array nums representing the data status of this set after the error.

Find the number that occurs twice and the number that is missing and return them in the form of an array.

Example 1:
> Input: nums = [1,2,2,4]  
Output: [2,3]

Example 2:
> Input: nums = [1,1]  
Output: [1,2]

Constraints:
* `2 <= nums.length <= 104`
* `1 <= nums[i] <= 104`

## Solution
Identical to the previous two questions, but in the second loop, since there is only one duplicate number, we immediately return the duplicate number, and the number that it is taking the place of.
## Code
```
class Solution {
    fun findErrorNums(nums: IntArray): IntArray {
        var i = 0
        while (i < nums.size) {
            val pos = nums[i] - 1
            if (pos != i && nums[i] != nums[pos]) {
                swap(nums, pos, i)
            } else i++
        }
        for (i in 0 until nums.size) {
            val num = i + 1
            if (num != nums[i]) {
                return intArrayOf(nums[i], num)
            }
        }
        return intArrayOf()
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
`O(1)`
