# Find Right Interval
You are given an array of intervals, where intervals[i] = [starti, endi] and each starti is unique.

The right interval for an interval i is an interval j such that startj >= endi and startj is minimized. Note that i may equal j.

Return an array of right interval indices for each interval i. If no right interval exists for interval i, then put -1 at index i.

Example 1:
```
Input: intervals = [[1,2]]
Output: [-1]
Explanation: There is only one interval in the collection, so it outputs -1.
```
Example 2:
```
Input: intervals = [[3,4],[2,3],[1,2]]
Output: [-1,0,1]
Explanation: There is no right interval for [3,4].
The right interval for [2,3] is [3,4] since start0 = 3 is the smallest start that is >= end1 = 3.
The right interval for [1,2] is [2,3] since start1 = 2 is the smallest start that is >= end2 = 2.
```
Example 3:
```
Input: intervals = [[1,4],[2,3],[3,4]]
Output: [-1,2,-1]
Explanation: There is no right interval for [1,4] and [3,4].
The right interval for [2,3] is [3,4] since start2 = 3 is the smallest start that is >= end1 = 3.
```

Constraints:
* `1 <= intervals.length <= 2 * 104`
* `intervals[i].length == 2`
* `-106 <= starti <= endi <= 106`
* `The start point of each interval is unique.`
## Solution
A brute force solution could be to take one interval at a time and go through all the other intervals to find the next interval. This algorithm will take `O(N^2)` where `N` is the total number of intervals. Can we do better than that?

We can utilize the Two Heaps approach. We can push all intervals into two heaps: one heap to sort the intervals on maximum start time (let's call it maxStartHeap) and the other on maximum end time (let's call it maxEndHeap). We can then iterate through all intervals of the maxEndHeap to find their next interval. Our algorithm will have the following steps:

1. Take out the top (having highest end) interval from the maxEndHeap to find its next interval. Let's call this interval topEnd.
2. Find an interval in the maxStartHeap with the closest start greater than or equal to the start of topEnd. Since maxStartHeap is sorted by 'start' of intervals, it is easy to find the interval with the highest 'start'. Let's call this interval topStart.
3. Add the index of topStart in the result array as the next interval of topEnd. If we can't find the next interval, add '-1' in the result array.
4. Put the topStart back in the maxStartHeap, as it could be the next interval of other intervals.
5. Repeat steps 1-4 until we have no intervals left in maxEndHeap.
## Code
Submission link: https://leetcode.com/submissions/detail/727682615/
```
class Solution {
    fun findRightInterval(intervals: Array<IntArray>): IntArray {
        val result = IntArray(intervals.size)
        val maxEnd = PriorityQueue<Int>(compareByDescending{intervals[it][END]})
        val maxStart = PriorityQueue<Int>(compareByDescending{intervals[it][START]})
        for (i in 0 until intervals.size) {
            maxEnd.offer(i)
            maxStart.offer(i)
        }
        while (maxEnd.isNotEmpty()) {
            val curr = maxEnd.poll()
            var rightInterval: Int? = null
            while (maxStart.isNotEmpty() && intervals[maxStart.peek()][START] >= intervals[curr][END]) {
                rightInterval = maxStart.poll()
            }
            if (rightInterval != null) {
                result[curr] = rightInterval
                maxStart.offer(rightInterval)
            } else {
                result[curr] = -1
            }
        }
        return result
    }

    companion object {
        const val START = 0
        const val END = 1
    }
}
```
## Complexity
### Time
`O()`
### Space
`O()`
