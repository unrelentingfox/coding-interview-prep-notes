# Search In Rotated Sorted Array
There is an integer array nums sorted in ascending order (with distinct values).

Prior to being passed to your function, nums is possibly rotated at an unknown pivot index `k (1 <= k < nums.length)` such that the resulting array is `[nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]]` (0-indexed). For example, [0,1,2,4,5,6,7] might be rotated at pivot index 3 and become [4,5,6,7,0,1,2].

Given the array nums after the possible rotation and an integer target, return the index of target if it is in nums, or -1 if it is not in nums.

You must write an algorithm with O(log n) runtime complexity.

Example 1:
```
Input: nums = [4,5,6,7,0,1,2], target = 0
Output: 4
```
Example 2:
```
Input: nums = [4,5,6,7,0,1,2], target = 3
Output: -1
```
Example 3:
```
Input: nums = [1], target = 0
Output: -1
```
Constraints:
* `1 <= nums.length <= 5000`
* `-104 <= nums[i] <= 104`
* `All values of nums are unique.`
* `nums is an ascending array that is possibly rotated.`
* `-104 <= target <= 104`
## Solution
There are a few different ways we could solve this.
1. We could split it into 2 searches like we did with the bitonic array. Find the max in the array using a binary search, compare start, mid and end to find where target will be, then search that half of the array with a normal binary search.
2. We could take advantage of the fact that if we find a midpoint, either the left or the right side will be fully sorted so we can use that side to check for target. This can then be repeated in it's own binary search.

Let's run with the second option.

1. for each iteration of our binary search we will figure out which side is sorted by comparing start to mid and mid to end
    * if `nums[start] < nums[mid]` then the left side is sorted
    * else the right side is sorted
2. Then from there we need to be able to eliminate one side. Lets create our logic around eliminating the left side **including** mid.
    * if left side is sorted, then we can eliminate it (and mid) if `target < nums[start] || num[mid] < target`
    * if right side is sorted, then we can eliminate the left side (and mid) if `nums[mid] < target && target <= nums[end]`
    * otherwise set `end = mid` since target will be within start to mid inclusive
## Code (kotlin)
Submission link: https://leetcode.com/submissions/detail/739538888/
```
class Solution {
    fun search(nums: IntArray, target: Int): Int {
        var s = 0
        var e = nums.size - 1
        while (s < e) {
            var m = s + (e - s) / 2
            if (nums[s] <= nums[m]) {
                if (target < nums[s] || nums[m] < target) {
                    s = m + 1
                } else {
                    e = m
                }
            } else {
                if (nums[m] < target && target <= nums[e]) {
                    s = m + 1
                } else {
                    e = m
                }
            }
        }
        return if (nums[s] == target) s else -1
    }
}
```
## Complexity
### Time
`O(log n)`
### Space
`O(1)`
