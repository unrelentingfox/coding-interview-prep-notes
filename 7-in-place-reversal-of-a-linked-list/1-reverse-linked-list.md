# Reverse a linked list
Given the head of a singly linked list, reverse the list, and return the reversed list.

Example 1:
> Input: head = [1,2,3,4,5]  
Output: [5,4,3,2,1]

Example 2:
> Input: head = [1,2]  
Output: [2,1]

Example 3:
> Input: head = []  
Output: []

## Solution

## Code Iterative
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
    fun reverseList(head: ListNode?): ListNode? {
        var curr = head
        var prev: ListNode? = null
        while (curr != null) {
            val next = curr?.next
            curr?.next = prev
            prev = curr
            curr = next
        }
        return prev
    }
}
```
## Complexity
### Time
`O(n)`
### Space
`O(1)`
## Code Tail Recursive
```
class Solution {
    fun reverseList(head: ListNode?): ListNode? {
        return reverseList(head, null)
    }

    tailrec fun reverseList(head: ListNode?, prev: ListNode?): ListNode? {
        return if (head == null) {
            prev
        } else {
            val next = head?.next
            head.next = prev
            reverseList(next, head)
        }
    }
}
```
## Complexity
### Time
`O(n)`
### Space
`O(1)` tailrecursion makes the compiler rewrite this function iteratively

## Code Recursive (bad)
```
class Solution {
    fun reverseList(head: ListNode?): ListNode? {
        return reverse(head)
    }

    fun reverse(head: ListNode?): ListNode? {
        if (head?.next == null) {
            return head
        }
        val newHead = reverseList(head?.next)
        head?.next?.next = head
        head?.next = null
        return newHead
    }
}
```
## Complexity
### Time
`O(n)`
### Space
`O(n)` since each call is added to the callstack
