# Find all missing numbers
Given an array nums of n integers where nums[i] is in the range [1, n], return an array of all the integers in the range [1, n] that do not appear in nums.

Example 1:
> Input: nums = [4,3,2,7,8,2,3,1]  
Output: [5,6]

Example 2:
>Input: nums = [1,1]  
Output: [2]

Constraints:

* `n == nums.length`
* `1 <= n <= 105`
* `1 <= nums[i] <= n`

Follow up: Could you do it without extra space and in O(n) runtime? You may assume the returned list does not count as extra space.

## Solution
This is the perfect problem for cyclic sort. We know that there are n numbers in the range 1-n, this means we can implicitly assign the numbers to their indexes, i.e. index is num - 1. Thus we can simply iterate through the numbers and in scenarios where `nums[i] - i != i && nums[nums[i] - 1] != nums[i]` we swap the numbers in i and nums[i] - 1.

Then it's just a matter of iterating again and checking whether `nums[i] == i - 1` and when it does not, we add i - 1 to the list.

## Code
```
class Solution {
    fun findDisappearedNumbers(nums: IntArray): List<Int> {
        val result = mutableListOf<Int>()
        var i = 0
        while (i < nums.size) {
            val pos = nums[i] - 1
            if (pos != i && nums[pos] != nums[i]) {
                swap(nums, i, pos)
            } else {
                i++
            }
        }
        for (i in 0 until nums.size) {
            val num = i + 1
            if (nums[i] != num) {
                result.add(num)
            }
        }
        return result
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
We have to iterate through the list once to sort, and again to count the missing numbers so the time complexity is `O(n)`
### Space
If we don't count the output list, then we are using constant space `O(1)`
