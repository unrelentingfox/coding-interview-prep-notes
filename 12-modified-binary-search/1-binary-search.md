# Binary Search
Given an array of integers nums which is sorted in ascending order, and an integer target, write a function to search target in nums. If target exists, then return its index. Otherwise, return -1.

You must write an algorithm with O(log n) runtime complexity.

Example 1:
```
Input: nums = [-1,0,3,5,9,12], target = 9
Output: 4
Explanation: 9 exists in nums and its index is 4
```
Example 2:
```
Input: nums = [-1,0,3,5,9,12], target = 2
Output: -1
Explanation: 2 does not exist in nums so return -1
```
Constraints:
* `1 <= nums.length <= 104`
* `-104 < nums[i], target < 104`
* `All the integers in nums are unique.`
* `nums is sorted in ascending order.`
## Solution
See this answer: https://leetcode.com/problems/binary-search/discuss/423162/Binary-Search-101
## Code
Submission link: https://leetcode.com/submissions/detail/736897914/
```
class Solution {
    fun search(nums: IntArray, target: Int): Int {
        var start = 0
        var end = nums.size - 1
        while (start < end) {
            val midpoint = start + (end - start) / 2
            if(target > nums[midpoint]) {
                start = midpoint + 1
            } else {
                end = midpoint
            }
        }
        return if(target == nums[start]) start else -1
    }
}
```
## Complexity
### Time
`O(log n)` because we are halving our search space each iteration
### Space
`O(1)`
