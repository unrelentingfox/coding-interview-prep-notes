# Rotation Count (same as Find Minimum in rotated array)
Given an array of numbers which is sorted in ascending order and is rotated ‘k’ times around a pivot, find ‘k’.

You can assume that the array does not have any duplicates.

Example 1:
```
Input: [10, 15, 1, 3, 8]
Output: 2
```
Example 2:
```
Input: [4, 5, 7, 9, 10, -1, 2]
Output: 5
 ```
Example 3:
```
Input: [1, 3, 8, 10]
Output: 0
Explanation: The array has been not been rotated.
```
## Solution
The rotation count of an array is the same as finding the index of the minimum value of an array.

To do this we can write a binary search to find the max value `i` then take `(i + 1) % length` to find the minimum value. If the max value is at the end of the array then that value will be 0.

## Code (kotlin)
Submission link: https://leetcode.com/submissions/detail/739585752/
```
class Solution {
    fun findMin(nums: IntArray): Int {
        if (nums[0] <= nums[nums.size - 1]) return nums[0]
        var s = 0
        var e = nums.size - 1
        while (s < e) {
            var m = s + (e - s) / 2
            if (nums[s] <= nums[m] && nums[m] < nums[m + 1]) {
                s = m + 1
            } else {
                e = m
            }
        }
        return nums[(s + 1) % nums.size]
    }
}
```
## Code (java)
Submission link: https://leetcode.com/submissions/detail/739593538/
```
class Solution {
    public int findMin(int[] nums) {
        if (nums[0] <= nums[nums.length - 1]) return nums[0];
        int s = 0;
        int e = nums.length - 1;
        while (s < e) {
            int m = s + (e - s) / 2;
            if (nums[s] <= nums[m] && nums[m] < nums[m + 1]) {
                s = m + 1;
            } else {
                e = m;
            }
        }
        return nums[(s + 1) % nums.length];
    }
}
```
## Complexity
### Time
`O(log n)`
### Space
`O(1)`
