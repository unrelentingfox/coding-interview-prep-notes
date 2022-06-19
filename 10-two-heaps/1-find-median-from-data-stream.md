# Find Median From Data Stream
## Solution
As we know, the median is the middle value in an ordered integer list. So a brute force solution could be to maintain a sorted list of all numbers inserted in the class so that we can efficiently return the median whenever required. Inserting a number in a sorted list will take O(N) time if there are ‘N’ numbers in the list. This insertion will be similar to the Insertion sort. Can we do better than this? Can we utilize the fact that we don’t need the fully sorted list - we are only interested in finding the middle element?

Assume ‘x’ is the median of a list. This means that half of the numbers in the list will be smaller than (or equal to) ‘x’ and half will be greater than (or equal to) ‘x’. This leads us to an approach where we can divide the list into two halves: one half to store all the smaller numbers (let’s call it smallNumList) and one half to store the larger numbers (let’s call it largeNumList). The median of all the numbers will either be the largest number in the smallNumList or the smallest number in the largeNumList. If the total number of elements is even, the median will be the average of these two numbers.

The best data structure that comes to mind to find the smallest or largest number among a list of numbers is a Heap. Let’s see how we can use a heap to find a better algorithm.

We can store the first half of numbers (i.e., smallNumList) in a Max Heap. We should use a Max Heap as we are interested in knowing the largest number in the first half.
We can store the second half of numbers (i.e., largeNumList) in a Min Heap, as we are interested in knowing the smallest number in the second half.
Inserting a number in a heap will take `O(logN)`, which is better than the brute force approach.
At any time, the median of the current list of numbers can be calculated from the top element of the two heaps.
## Code
Submission link: https://leetcode.com/submissions/detail/726282208/
```
/**
 * Your MedianFinder object will be instantiated and called as such:
 * var obj = MedianFinder()
 * obj.addNum(num)
 * var param_2 = obj.findMedian()
 */
class MedianFinder() {
    val lowMax = PriorityQueue<Int>(compareByDescending{it})
    val highMin = PriorityQueue<Int>()

    fun addNum(num: Int) {
        val mid = lowMax.peek()
        if (mid == null || num < mid) {
            lowMax.offer(num)
        } else {
            highMin.offer(num)
        }
        if (lowMax.size > highMin.size + 1) {
            val swap = lowMax.poll()
            highMin.offer(swap)
        } else if (highMin.size > lowMax.size) {
            val swap = highMin.poll()
            lowMax.offer(swap)
        }
    }

    fun findMedian(): Double? {
        return if (lowMax.isNotEmpty() && lowMax.size == highMin.size) {
            val lowMid = lowMax.peek()
            val highMid = highMin.peek()
            (lowMid + highMid).toDouble() / 2
        } else {
            lowMax.peek()?.toDouble()
        }
    }

}
```
## Complexity
### Time
addNum - `O(log n)`
findMedian - `O(1)`
### Space
`O(n)`
## Alternative Solution
From: https://leetcode.com/problems/find-median-from-data-stream/discuss/74047/JavaPython-two-heap-solution-O(log-n)-add-O(1)-find
## Code
Submission link: https://leetcode.com/submissions/detail/726305837/
```
/**
 * Your MedianFinder object will be instantiated and called as such:
 * var obj = MedianFinder()
 * obj.addNum(num)
 * var param_2 = obj.findMedian()
 */
class MedianFinder() {
    val leftMax = PriorityQueue<Int>(compareByDescending{it})
    val rightMin = PriorityQueue<Int>()
    var even = true

    fun addNum(num: Int) {
        if (even) {
            rightMin.offer(num)
            leftMax.offer(rightMin.poll()) // leftMax will contain 1 more than rightMin when odd
        } else {
            leftMax.offer(num)
            rightMin.offer(leftMax.poll())
        }
        even = !even
    }

    fun findMedian(): Double? {
        return if (even) {
            val lowMid = leftMax.peek()
            val highMid = rightMin.peek()
            (lowMid + highMid).toDouble() / 2
        } else {
            leftMax.peek().toDouble()
        }
    }

}
```
## Complexity
### Time
addNum - `O(log n)`
findMedian - `O(1)`
### Space
`O(n)`
