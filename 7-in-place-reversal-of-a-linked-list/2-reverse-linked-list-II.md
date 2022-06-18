# Reverse a sub-list
Given the head of a singly linked list and two integers `left` and `right` where `left <= right`, reverse the nodes of the list from position `left` to position `right`, and return the reversed list.

Example 1:
> Input: head = [1,2,3,4,5], left = 2, right = 4  
Output: [1,4,3,2,5]

Example 2:
> Input: head = [5], left = 1, right = 1  
Output: [5]

Constraints:
* The number of nodes in the list is n.
* `1 <= n <= 500`
* `-500 <= Node.val <= 500`
* `1 <= left <= right <= n`

## Solution
The problem follows the In-place Reversal of a LinkedList pattern. We can use a similar approach as discussed in Reverse a LinkedList. Here are the steps we need to follow:

1. Skip the first p-1 nodes, to reach the node at position p.
1. Remember the node at position p-1 to be used later to connect with the reversed sub-list.
1. Next, reverse the nodes from p to q using the same approach discussed in Reverse a LinkedList.
1. Connect the p-1 and q+1 nodes to the reversed sub-list.

## Code (java)
```
class ReverseSubList {

  public static ListNode reverse(ListNode head, int p, int q) {
    if (p == q)
      return head;

    // after skipping 'p-1' nodes, current will point to 'p'th node
    ListNode current = head, previous = null;
    for (int i = 0; current != null && i < p - 1; ++i) {
      previous = current;
      current = current.next;
    }

    // we are interested in three parts of the LinkedList, part before index 'p', part between 'p' and 
    // 'q', and the part after index 'q'
    ListNode lastNodeOfFirstPart = previous; // points to the node at index 'p-1'
    // after reversing the LinkedList 'current' will become the last node of the sub-list
    ListNode lastNodeOfSubList = current;
    ListNode next = null; // will be used to temporarily store the next node
    // reverse nodes between 'p' and 'q'
    for (int i = 0; current != null && i < q - p + 1; i++) {
      next = current.next;
      current.next = previous;
      previous = current;
      current = next;
    }

    // connect with the first part
    if (lastNodeOfFirstPart != null)
      lastNodeOfFirstPart.next = previous; // 'previous' is now the first node of the sub-list
    else // this means p == 1 i.e., we are changing the first node (head) of the LinkedList
      head = previous;

    // connect with the last part
    lastNodeOfSubList.next = current;

    return head;
  }

  public static void main(String[] args) {
    ListNode head = new ListNode(1);
    head.next = new ListNode(2);
    head.next.next = new ListNode(3);
    head.next.next.next = new ListNode(4);
    head.next.next.next.next = new ListNode(5);

    ListNode result = ReverseSubList.reverse(head, 2, 4);
    System.out.print("Nodes of the reversed LinkedList are: ");
    while (result != null) {
      System.out.print(result.value + " ");
      result = result.next;
    }
  }
}
```
## Alternative Solution
I created a sub function that accepts head and n where n is the number of reversals. Then it will apply reversals until n is reached, then if there are any remaining nodes we can just tack those onto the new end of the list and return.

Then my solution will iterate through the nodes until the `left` node is reached, and call my reverseNodes function with `left - right` and re-connect the result of that to the previous node if it exists.
## Code
```
class Solution {
    fun reverseBetween(head: ListNode?, left: Int, right: Int): ListNode? {
        if (head?.next == null || left == right) return head
        return if (left == 1) {
           reverseNodes(head, right - left)
        } else {
            var curr = head
            var prev: ListNode? = null
            var i = 1
            while (curr != null && i < left) {
                prev = curr
                curr = curr?.next
                i++
            }
            if (prev != null && curr != null) {
                prev.next = reverseNodes(curr, right - left)
            }
            head
        }
    }

    fun reverseNodes(head: ListNode?, reversals: Int): ListNode? {
        if (head?.next == null) return head
        var curr = head
        var prev: ListNode? = null
        var i = 0
        while (curr != null && i <= reversals) {
            val next = curr?.next
            curr?.next = prev
            prev = curr
            curr = next
            i++
        }
        if (curr != null) { // if there are remaining nodes, add them to head which is the new end
            head?.next = curr
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
