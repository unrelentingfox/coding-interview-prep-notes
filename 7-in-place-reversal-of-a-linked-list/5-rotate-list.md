# Rotate List
Given the head of a linked list, rotate the list to the right by k places.

Example 1:
> Input: head = [1,2,3,4,5], k = 2  
Output: [4,5,1,2,3]

Example 2:
> Input: head = [0,1,2], k = 4  
Output: [2,0,1]

Constraints:
* The number of nodes in the list is in the range [0, 500].
* `-100 <= Node.val <= 100`
* `0 <= k <= 2 * 109`

## Solution
This problem doesn't really relate to reversing a list.. Another way of defining the rotation is to take the sub-list of ‘k’ ending nodes of the LinkedList and connect them to the beginning. 

The cases to consider here are as follows (l = length of the list):
* `K <= 0`, nothing to do
* `l <= 1`, nothing to do
* `l > k`, this is the common case
* `l == k`, a full rotation of all nodes would result in no changes
* `l < k`, this means that we can take `k % l` since the assertion from the previous bullet point

To do this the steps are as follows:
* Find the end of the linked list, and count the length as you go
* Connect the end of the list to the beginning creating a cyclic list
* Then find the position of the new head relative to the original head: `length - k % length`
* Move to the new head position in the cycle.
* Disconnect the prev node and return the new head.
## Code
Submission link: https://leetcode.com/submissions/detail/719930560/
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
    fun rotateRight(head: ListNode?, k: Int): ListNode? {
        if (head?.next == null || k <= 0) return head
        var length = 1
        var prev = head
        var curr = prev?.next
        while (curr != null) {
            prev = curr
            curr = curr?.next
            length++
        }
        val newHeadPosition = length - k % length
        return if (newHeadPosition != 0) {
            prev?.next = head // circular list
            curr = head
            var i = 0
            while (i < newHeadPosition) {
                prev = curr
                curr = curr?.next
                i++
            }
            prev?.next = null
            curr
        } else {
            head
        }
    }
}
```
## Complexity
### Time
`O(n)`
### Space
`O(1)`
