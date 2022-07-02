# Search in a Sorted Array of Unknown Size
This is an interactive problem.

You have a sorted array of unique elements and an unknown size. You do not have an access to the array but you can use the ArrayReader interface to access it. You can call ArrayReader.get(i) that:

returns the value at the ith index (0-indexed) of the secret array (i.e., secret[i]), or
returns 231 - 1 if the i is out of the boundary of the array.
You are also given an integer target.

Return the index k of the hidden array where secret[k] == target or return -1 otherwise.

You must write an algorithm with O(log n) runtime complexity.

Example 1:
```
Input: secret = [-1,0,3,5,9,12], target = 9
Output: 4
Explanation: 9 exists in secret and its index is 4.
```
Example 2:
```
Input: secret = [-1,0,3,5,9,12], target = 2
Output: -1
Explanation: 2 does not exist in secret so return -1.
```
Constraints:
* `1 <= secret.length <= 104`
* `-104 <= secret[i], target <= 104`
* `secret is sorted in a strictly increasing order.`
## Solution
In order to solve this problem we first need to find a value for end. Then we can just use a normal binary search from there.
1. Start with 0 and 1 as your start and end values
2. While `reader.get(end) < target` we will set start to equal `end + 1` and double the end
3. Once end reaches a number that is greater than target (either one within the search space or Int.MAX_VALUE) then we can simply do a binary search from there.
## Code (kotlin)
Submission link: https://leetcode.com/submissions/detail/736976825/
```
/**
 * // This is ArrayReader's API interface.
 * // You should not implement it, or speculate about its implementation
 * class ArrayReader {
 *     fun get(index: Int): Int {}
 * }
 */
class Solution {
    fun search(reader: ArrayReader, target: Int): Int {
        var s = 0
        var e = 1
        while (reader.get(e) < target) {
            s = e + 1
            e *= 2
        }
        while (s < e) {
            var m = s + (e - s) / 2
            if (reader.get(m) < target) {
                s = m + 1
            } else {
                e = m
            }
        }
        return if (reader.get(s) == target) s else -1
    }
}
```
## Code (java)
Submission link: https://leetcode.com/submissions/detail/736983893/
```
/**
 * // This is ArrayReader's API interface.
 * // You should not implement it, or speculate about its implementation
 * interface ArrayReader {
 *     public int get(int index) {}
 * }
 */
class Solution {
    public int search(ArrayReader reader, int target) {
        int s = 0;
        int e = 1;
        while (reader.get(e) < target) {
            s = e + 1;
            e *= 2;
        }
        while (s < e) {
            int m = s + (e - s) / 2;
            if (reader.get(m) < target)
                s = m + 1;
            else
                e = m;
        }
        if (reader.get(s) == target)
            return s;
        else
            return -1;
    }
}
```
## Complexity
### Time
`O(log n)`
### Space
`O(1)`
