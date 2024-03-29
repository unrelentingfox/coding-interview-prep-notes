# Shortest Continuous Subarray
https://leetcode.com/problems/shortest-unsorted-continuous-subarray/
## Solution
As we know, once an array is sorted (in ascending order), the smallest number is at the beginning and the largest number is at the end of the array. So if we start from the beginning of the array to find the first element which is out of sorting order i.e., which is smaller than its previous element, and similarly from the end of array to find the first element which is bigger than its previous element, will sorting the subarray between these two numbers result in the whole array being sorted?

Let’s try to understand this with Example-2 mentioned above. In the following array, what are the first numbers out of sorting order from the beginning and the end of the array:

    [1, 3, 2, 0, -1, 7, 10]
Starting from the beginning of the array the first number out of the sorting order is ‘2’ as it is smaller than its previous element which is ‘3’.
Starting from the end of the array the first number out of the sorting order is ‘0’ as it is bigger than its previous element which is ‘-1’
As you can see, sorting the numbers between ‘3’ and ‘-1’ will not sort the whole array. To see this, the following will be our original array after the sorted subarray:

    [1, -1, 0, 2, 3, 7, 10]
The problem here is that the smallest number of our subarray is ‘-1’ which dictates that we need to include more numbers from the beginning of the array to make the whole array sorted. We will have a similar problem if the maximum of the subarray is bigger than some elements at the end of the array. To sort the whole array we need to include all such elements that are smaller than the biggest element of the subarray. So our final algorithm will look like:

From the beginning and end of the array, find the first elements that are out of the sorting order. The two elements will be our candidate subarray.
Find the maximum and minimum of this subarray.
Extend the subarray from beginning to include any number which is bigger than the minimum of the subarray.
Similarly, extend the subarray from the end to include any number which is smaller than the maximum of the subarray.
## Code (kotlin)
```kotlin
class Solution {
    fun findUnsortedSubarray(nums: IntArray): Int {
        var left = 0
        var right = nums.size - 1
        while (left < nums.size - 1 && nums[left] <= nums[left + 1]) left += 1
        if (left == nums.size - 1) return 0 // whole array is sorted
        while (right > 0 && nums[right] >= nums[right - 1]) right -= 1
        var max = Int.MIN_VALUE
        var min = Int.MAX_VALUE
        for (i in left..right) { // find max and min within window
            max = maxOf(max, nums[i])
            min = minOf(min, nums[i])
        }
        while (left > 0 && nums[left - 1] > min) left -= 1
        while (right < nums.size - 1 && nums[right + 1] < max) right += 1
        return right - left + 1
    }
}
```
## Complexity
### Time
`O(n)`
### Space
`O(1)`
