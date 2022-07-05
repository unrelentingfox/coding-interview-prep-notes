# Bitonic Array Max (Mountain Array)
Find the maximum value in a given Bitonic array. An array is considered bitonic if it is monotonically increasing and then monotonically decreasing. Monotonically increasing or decreasing means that for any index i in the array arr[i] != arr[i+1].

Example 1:
```
Input: [1, 3, 8, 12, 4, 2]
Output: 12
Explanation: The maximum number in the input bitonic array is '12'.
```
Example 2:
```
Input: [3, 8, 3, 1]
Output: 8
```
Example 3:
```
Input: [1, 3, 8, 12]
Output: 12
```
Example 4:
```
Input: [10, 9, 8]
Output: 10
```
## Solution
This problem can be solved using a binary search. We know that the array starts out increasing so we can shrink the start of our search space whenever we encounter a midpoint `m` (left mid) where `arr[m] < arr[m + 1]`. As long as we are using left-mid for our midpoint we will never get a midpoint where `m + 1 > arr.size` since a bitonic array must have a length of `>= 3`

(Explanation from course)  
A bitonic array is a sorted array; the only difference is that its first part is sorted in ascending order and the second part is sorted in descending order. We can use a similar approach as discussed in Order-agnostic Binary Search. Since no two consecutive numbers are same (as the array is monotonically increasing or decreasing), whenever we calculate the middle, we can compare the numbers pointed out by the index middle and middle+1 to find if we are in the ascending or the descending part. So:

1. If `arr[middle] > arr[middle + 1]`, we are in the second (descending) part of the bitonic array. Therefore, our required number could either be pointed out by middle or will be before middle. This means we will be doing: `end = middle`.
2. If `arr[middle] < arr[middle + 1]`, we are in the first (ascending) part of the bitonic array. Therefore, the required number will be after middle. This means we will be doing: `start = middle + 1`.
We can break when `start == end`. Due to the two points mentioned above, both start and end will be pointing at the maximum number of the bitonic array.

## Code (kotlin)
Submission link: https://leetcode.com/submissions/detail/738693459/
```
class Solution {
    fun peakIndexInMountainArray(arr: IntArray): Int {
        var s = 0
        var e = arr.size - 1
        while (s < e) {
            var m = s + (e - s) / 2
            if (arr[m] < arr[m + 1]) {
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
Submission link: https://leetcode.com/submissions/detail/738699256/
```
class Solution {
    public int peakIndexInMountainArray(int[] arr) {
        int s = 0;
        int e = arr.length - 1;
        while (s < e) {
            int m = s + (e - s) / 2;
            if (arr[m] < arr[m + 1]) {
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
`O(log n)`
### Space
`O(1)`
