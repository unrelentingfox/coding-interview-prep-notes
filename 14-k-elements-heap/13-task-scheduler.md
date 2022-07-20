# Task Scheduler
https://leetcode.com/problems/task-scheduler/
## Solution
This problem follows the Top 'K' Elements pattern and is quite similar to Rearrange String K Distance Apart. We need to rearrange tasks such that same tasks are 'K' distance apart.

Following a similar approach, we will use a Max Heap to execute the highest frequency task first. After executing a task we decrease its frequency and put it in a waiting list. In each iteration, we will try to execute as many as k+1 tasks. For the next iteration, we will put all the waiting tasks back in the Max Heap. If, for any iteration, we are not able to execute k+1 tasks, the CPU has to remain idle for the remaining time in the next iteration.
## Code (kotlin)
```kotlin
class Solution {
    fun leastInterval(tasks: CharArray, n: Int): Int {
        val max = PriorityQueue<Int>(compareByDescending { it })
        tasks.toList().groupingBy{ it }.eachCount().map { (task, count) ->
            max.offer(count)
        }
        var time = 0
        while (max.isNotEmpty()) {
            val waitList = ArrayList<Int>(n + 1 / 2)
            var remainingTime = n + 1
            while (remainingTime > 0 && max.isNotEmpty()) {
                time++
                val task = max.poll()
                if (task > 1) waitList.add(task - 1)
                remainingTime--
            }
            max.addAll(waitList)
            if (max.isNotEmpty()) {
                time += remainingTime
            }
        }
        return time
    }
}
```
## Complexity
### Time
`O(n log n)`
### Space
`O(n)`
