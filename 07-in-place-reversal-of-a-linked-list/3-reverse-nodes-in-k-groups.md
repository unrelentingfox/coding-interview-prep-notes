# Reverse nodes in k groups
Given the head of a linked list, reverse the nodes of the list k at a time, and return the modified list.

k is a positive integer and is less than or equal to the length of the linked list. If the number of nodes is not a multiple of k then left-out nodes, in the end, should remain as it is.

You may not alter the values in the list's nodes, only nodes themselves may be changed.

Example 1:
> Input: head = [1,2,3,4,5], k = 2  
Output: [2,1,4,3,5]

Example 2:
> Input: head = [1,2,3,4,5], k = 3  
Output: [3,2,1,4,5]

Constraints:
* The number of nodes in the list is n.
* `1 <= k <= n <= 5000`
* `0 <= Node.val <= 1000`
Follow-up: Can you solve the problem in O(1) extra memory space?

## Solution

## Code
https://leetcode.com/submissions/detail/713862827/
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
        while (curr != null) {
            if (i % k == 0) {
                curr = reverseKNodes(curr, k)
                prev?.next = curr
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
        return if (reversals < k) {
            reverseKNodes(prev, reversals) // not long enough, undo reversals
        } else {
            head?.next = curr // tack on remaining nodes
            prev
        }
    }
}
```
## Complexity
### Time
`O(n)`
### Space
`O(1)`
