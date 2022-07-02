# Number Range
Given an array of integers nums sorted in non-decreasing order, find the starting and ending position of a given target value.

If target is not found in the array, return [-1, -1].

You must write an algorithm with O(log n) runtime complexity.

Example 1:
```
Input: nums = [5,7,7,8,8,10], target = 8
Output: [3,4]
```
Example 2:
```
Input: nums = [5,7,7,8,8,10], target = 6
Output: [-1,-1]
```
Example 3:
```
Input: nums = [], target = 0
Output: [-1,-1]
```
Constraints:
* `0 <= nums.length <= 105`
* `-109 <= nums[i] <= 109`
* `nums is a non-decreasing array.`
* `-109 <= target <= 109`
## Solution
We can use a standard binary search for this one to find a single instance of the target. Then from there we can just expand start and end pointers in each direction until the first and last index of the target.
## Code (kotlin)
Submission link: https://leetcode.com/submissions/detail/736950894/
```
class Solution {
    fun searchRange(nums: IntArray, target: Int): IntArray {
        if (nums.isEmpty()) return intArrayOf(-1,-1)
        var s = 0
        var e = nums.size - 1
        while (s < e) {
            var m = s + (e - s) / 2
            if (nums[m] < target) {
                s = m + 1
            } else {
                e = m
            }
        }
        return if (nums[s] == target) {
            while (s > 0 && nums[s - 1] == target) s--
            while (e < nums.size - 1 && nums[e + 1] == target) e++
            intArrayOf(s, e)
        } else {
            intArrayOf(-1, -1)
        }
    }
}
```
## Code (java)
Submission link: https://leetcode.com/submissions/detail/736956343/
```
class Solution {
    public int[] searchRange(int[] nums, int target) {
        if (nums.length == 0) return new int[] {-1, -1};
        int s = 0;
        int e = nums.length - 1;
        while (s < e) {
            int m = s + (e - s) / 2;
            if (nums[m] < target) {
                s = m + 1;
            } else {
                e = m;
            }
        }
        if (nums[s] == target) {
            while(s > 0 && nums[s - 1] == target) s--;
            while(e < nums.length -1 && nums[e + 1] == target) e++;
            return new int[] {s, e};
        } else {
            return new int[] {-1, -1};
        }
    }
}
```
## Complexity
### Time
`O(log n)`
### Space
`O(1)`
