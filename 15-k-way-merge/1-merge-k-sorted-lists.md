# Merge K sorted Lists
https://leetcode.com/problems/merge-k-sorted-lists/submissions/
## Solution
A brute force solution could be to add all elements of the given ‘K’ lists to one list and sort it. If there are a total of ‘N’ elements in all the input lists, then the brute force solution will have a time complexity of `O(N*logN)` as we will need to sort the merged list. Can we do better than this? How can we utilize the fact that the input lists are individually sorted?

If we have to find the smallest element of all the input lists, we have to compare only the smallest (i.e. the first) element of all the lists. Once we have the smallest element, we can put it in the merged list. Following a similar pattern, we can then find the next smallest element of all the lists to add it to the merged list.

The best data structure that comes to mind to find the smallest number among a set of ‘K’ numbers is a Heap. Let’s see how can we use a heap to find a better algorithm.

1. We can insert the first element of each array in a Min Heap.
2. After this, we can take out the smallest (top) element from the heap and add it to the merged list.
3. After removing the smallest element from the heap, we can insert the next element of the same list into the heap.
4. We can repeat steps 2 and 3 to populate the merged list in sorted order.

## Code (kotlin)
```kotlin
class Solution {
    fun mergeKLists(lists: Array<ListNode?>): ListNode? {
        val min = PriorityQueue<ListNode>(compareBy{it.`val`})
        var resultHead = ListNode(0)
        var resultTail = resultHead
        lists.filterNotNull().forEach {
            min.offer(it)
        }
        while (min.isNotEmpty()) {
            min.poll()?.let { curr ->
                resultTail.next = curr
                resultTail = curr
                curr.next?.let {
                    min.offer(it)
                }
            }
        }
        return resultHead.next
    }
}
```
## Complexity
### Time
`O(n log k)`
### Space
`O(k)`
