# Find in Bitonic (mountain) Array
Given a Bitonic array, find if a given ‘key’ is present in it. An array is considered bitonic if it is monotonically increasing and then monotonically decreasing. Monotonically increasing or decreasing means that for any index i in the array `arr[i] != arr[i+1]`.

Write a function to return the index of the ‘key’. If the ‘key’ appears more than once, return the smaller index. If the ‘key’ is not present, return -1.

Example 1:
```
Input: [1, 3, 8, 4, 3], key=4
Output: 3
```
Example 2:
```
Input: [3, 8, 3, 1], key=8
Output: 1
```
Example 3:
```
Input: [1, 3, 8, 12], key=12
Output: 3
```
Example 4:
```
Input: [10, 9, 8], key=10
Output: 0
```
## Solution
For this problem we can split this into three binary searches.
1. First we will find the peak of the bitonic array
2. Then we will do an ascending binary search on the left half of the array including the peak, if we find the value we can stop here.
3. Finally if we didn't find the value then we will do a descending binary search on the right half of the array excluding the peak.
## Code (kotlin)
Submission link: https://leetcode.com/submissions/detail/739485373/
```
/**
 * // This is MountainArray's API interface.
 * // You should not implement it, or speculate about its implementation
 * class MountainArray {
 *     fun get(index: Int): Int {}
 *     fun length(): Int {}
 * }
 */
class Solution {
    fun findInMountainArray(target: Int, arr: MountainArray): Int {
        var s = 0
        var e = arr.length() - 1
        while (s < e) {
            var m = s + (e - s) / 2
            if (arr.get(m) < arr.get(m + 1)) {
                s = m + 1
            } else {
                e = m
            }
        }
        val peak = s
        val left = search(target, arr, 0, peak, true)
        return if (arr.get(left) == target) {
            left
        } else {
            val right = search(target, arr, peak + 1, arr.length() - 1, false)
            if (arr.get(right) == target) {
                right
            } else {
                -1
            }
        }
    }

    private fun search(target: Int, arr: MountainArray, start: Int, end: Int, asc: Boolean): Int {
        var s = start
        var e = end
        while (s < e) {
            var m = s + (e - s) / 2
            if ((asc && arr.get(m) < target) || (!asc && arr.get(m) > target)) {
                s = m + 1
            } else {
                e = m
            }
        }
        return s
    }
}
```
## Code (java)
Submission link: https://leetcode.com/submissions/detail/739496664/
```
/**
 * // This is MountainArray's API interface.
 * // You should not implement it, or speculate about its implementation
 * interface MountainArray {
 *     public int get(int index) {}
 *     public int length() {}
 * }
 */

class Solution {
    public int findInMountainArray(int target, MountainArray arr) {
        int s = 0;
        int e = arr.length() - 1;
        while (s < e) {
            int m = s + (e - s) / 2;
            if (arr.get(m) < arr.get(m + 1)) {
                s = m + 1;
            } else {
                e = m;
            }
        }
        // s == e and they are pointing at the peak
        int left = search(target, arr, 0, s, true);
        if (arr.get(left) == target) {
            return left;
        } else {
            int right = search(target, arr, s + 1, arr.length() - 1, false);
            if (arr.get(right) == target) {
                return right;
            } else {
                return -1;
            }
        }
    }

    private int search(int target, MountainArray arr, int s, int e, boolean asc) {
        while (s < e) {
            int m = s + (e - s) / 2;
            if ((asc && arr.get(m) < target) || (!asc && arr.get(m) > target)) {
                s = m + 1;
            } else {
                e = m;
            }
        }
        return s;
    }
}
```
## Complexity
### Time
`O(log n)` technically this would be O(3 log n) since we have to search the array 3 times but we can drop that constant.
### Space
`O(1)`
