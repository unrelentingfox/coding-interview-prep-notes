# Employee Free Time
https://www.lintcode.com/problem/850/

## Description
We are given a list schedule of employees, which represents the working time for each employee.

Each employee has a list of non-overlapping Intervals, and these intervals are in sorted order.

Return the list of finite intervals representing common, positive-length free time for all employees, also in sorted order.

The Intervals is an 1d-array. Each two numbers shows an interval. For example, [1,2,8,10] represents that the employee works in [1,2] and [8,10].

Also, we wouldn't include intervals like [5, 5] in our answer, as they have zero length.

## Approach
For this algorithm all we really care about is empty space between all of the different schedules. To do this we can solve the problem very similary to how we solve meeting-rooms 1 and 2. Using the following algorithm:

1. Gather all of the start and end times for all employees' meetings into two lists
2. Sort both of those lists in ascending order
3. Then we iterate over both lists at the same time counting how many concurrent meetings there are. Since both lists are sorted, if we encounter a start value that is less than the next end value, that means we have started a new meeting. When the meeting count becomes 0 this means we've found an open window from end - start.
4. The loop pseudocode is as follows:
   1. When `start < end` then increment our meeting count by 1 and move to the next start value
   2. Else decrement the meeting count
      1. if the meeting count is 0 that means we've found an empty slot starting at the current end value and ending at the current start value.
```
/**
 * Definition of Interval:
 * class Interval {
 *     var start: Int = 0
 *     var end: Int = 0
 *     constructor(start: Int, end: Int) {
 *         this.start = start
 *         this.end = end
 *     }
 * }
 */

class Solution {
    /**
     * @param schedule: a list schedule of employees
     * @return: Return a list of finite intervals 
     */
    fun employeeFreeTime(schedule: Array<IntArray>): List<Interval?> {
        val starts = mutableListOf<Int>()
        val ends = mutableListOf<Int>()
        schedule.forEach {
            it.forEachIndexed { i, time ->
                if (i % 2 == 0) {
                    starts.add(time)
                } else {
                    ends.add(time)
                }
            }
        }
        starts.sort()
        ends.sort()
        val result = mutableListOf<Interval>()
        var si = 0
        var ei = 0
        var meetings = 0
        while (si < starts.size && ei < ends.size) {
            val start = starts[si]
            val end = ends[ei]
            if (start < end) {
                meetings++
                si++
            } else {
                meetings--
                if (meetings == 0 && end != start) {
                    result.add(Interval(end, start))
                }
                ei++
            }
        }
        return result
    }
}
```
