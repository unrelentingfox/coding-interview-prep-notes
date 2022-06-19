# Reverse alternating K-element Sub-list
Given the head of a LinkedList and a number ‘k’, reverse every alternating ‘k’ sized sub-list starting from the head.

If, in the end, you are left with a sub-list with less than ‘k’ elements, reverse it too.
## Solution
This is basically idential to the previous problem except we need to alternate between sorting and not sorting.. do this by adding a groups counter and checking when `groups % 2 == 0`.

Also we are reversing nodes even if there are less than k so the reverseKNodes function is slightly simplified.
## Code
```
/**
 * Example:
 * var li = ListNode(5)
 * var v = li.`val`
 * Definition for singly-linked list.
 * class ListNode(var `val`: Int) {
 *     var next: ListNode? = null
 * }
 */
class Solution {
    fun reverseKGroup(head: ListNode?, k: Int): ListNode? {
        if (head?.next == null || k < 2) return head
        val newHead = reverseKNodes(head, k)
        var prev = newHead
        var curr = prev?.next
        var i = 1
        var groups = 1
        while (curr != null) {
            if (i % k == 0) {
                if (groups % 2 == 0) {
                    curr = reverseKNodes(curr, k)
                    prev?.next = curr
                }
                groups++
            }
            prev = curr
            curr = curr?.next
            i++
        }
        return newHead
    }

    private fun reverseKNodes(head: ListNode?, k: Int): ListNode? {
        if (head?.next == null || k < 2) return head
        var prev: ListNode? = null
        var curr = head
        var reversals = 0
        while (curr != null && reversals < k) {
            val next = curr.next
            curr.next = prev
            prev = curr
            curr = next
            reversals++
        }
        head?.next = curr // tack on remaining nodes 
        return prev
    }
}
```
## Complexity
### Time
`O(n)`
### Space
`O(1)`
