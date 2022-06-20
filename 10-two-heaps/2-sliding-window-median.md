# Sliding Window Median
The median is the middle value in an ordered integer list. If the size of the list is even, there is no middle value. So the median is the mean of the two middle values.

* For examples, if arr = [2,3,4], the median is 3.
* For examples, if arr = [1,2,3,4], the median is (2 + 3) / 2 = 2.5.
You are given an integer array nums and an integer k. There is a sliding window of size k which is moving from the very left of the array to the very right. You can only see the k numbers in the window. Each time the sliding window moves right by one position.

Return the median array for each window in the original array. Answers within 10-5 of the actual value will be accepted.

Example 1:
```
Input: nums = [1,3,-1,-3,5,3,6,7], k = 3
Output: [1.00000,-1.00000,-1.00000,3.00000,5.00000,6.00000]
Explanation:
Window position                Median
---------------                -----
[1  3  -1] -3  5  3  6  7        1
 1 [3  -1  -3] 5  3  6  7       -1
 1  3 [-1  -3  5] 3  6  7       -1
 1  3  -1 [-3  5  3] 6  7        3
 1  3  -1  -3 [5  3  6] 7        5
 1  3  -1  -3  5 [3  6  7]       6
```
Example 2:
```
Input: nums = [1,2,3,4,2,3,1,4,2], k = 3
Output: [2.00000,3.00000,3.00000,3.00000,2.00000,3.00000,2.00000]
```
Constraints:
* `1 <= k <= nums.length <= 105`
* `-231 <= nums[i] <= 231 - 1`

## Solution
We can use the utility we built in the last problem with the addition of a remove function.
## Code
Submission link: https://leetcode.com/submissions/detail/726351028/
```
class Solution {
    fun medianSlidingWindow(nums: IntArray, k: Int): DoubleArray {
        val result = DoubleArray(nums.size - k + 1)
        val window = MedianWindow()
        var start = 0
        var end = 0
        while(end < nums.size) {
            window.add(nums[end++])
            if (end >= k) {
                result[start] = window.getMedian() ?: 0.0
                window.remove(nums[start++])
            }
        }
        return result
    }

    private class MedianWindow {
        private val low = PriorityQueue<Int>(compareByDescending{it})
        private val high = PriorityQueue<Int>() // will have extra number if odd
        private var even = true

        fun add(num: Int) { // O(1)
            if (even) {
                low.offer(num)
                high.offer(low.poll())
            } else {
                high.offer(num)
                low.offer(high.poll())
            }
            even = !even
        }

        fun remove(num: Int) { // O(log n)
            if (even && low.contains(num)) {
                low.remove(num)
            } else if (even && high.contains(num)) {
                high.remove(num)
                high.offer(low.poll())
            } else if (low.contains(num)) {
                low.remove(num)
                low.offer(high.poll())
            } else if(high.contains(num)) {
                high.remove(num)
            }
            even = !even
        }

        fun getMedian(): Double? {
            val highMin = high.peek()
            val lowMin = low.peek()
            return if (even && highMin != null && lowMin != null) {
                highMin / 2.0 + lowMin / 2.0
            } else {
                high.peek()?.toDouble()
            }
        }
    }
}
```
## Complexity
### Time
`O(n * k)`
Where `n` is the total number of elements in the input array and `K` is the size of the sliding window. This is due to the fact that we are going through all the `N` numbers and, while doing so, we are doing two things:
* Inserting/removing numbers from heaps of size ‘K’. This will take `O(logK)`
* Removing the element going out of the sliding window. This will take `O(K)` as we will be searching this element in an array of size `k` (i.e., a heap).
### Space
`O(k)` because we will be storing k elements in our window
