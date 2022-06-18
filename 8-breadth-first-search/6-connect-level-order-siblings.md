# Connect Level Order Siblings
You are given a perfect binary tree where all leaves are on the same level, and every parent has two children. The binary tree has the following definition:
```
struct Node {
  int val;
  Node *left;
  Node *right;
  Node *next;
}
```
Populate each next pointer to point to its next right node. If there is no next right node, the next pointer should be set to NULL.

Initially, all next pointers are set to NULL.

Example 1:
> Input: root = [1,2,3,4,5,6,7]  
Output: [1,#,2,3,#,4,5,6,7,#]  
Explanation: Given the above perfect binary tree (Figure A), your function should populate each next pointer to point to its next right node, just like in Figure B. The serialized output is in level order as connected by the next pointers, with '#' signifying the end of each level.

Example 2:
> Input: root = []  
Output: []

Constraints:
* The number of nodes in the tree is in the range [0, 212 - 1].
* `-1000 <= Node.val <= 1000`

## Solution
This solution follows the pattern except we will need to keep track of the previous node so we can connect it.
## Code
Submission link: https://leetcode.com/submissions/detail/720688660/
```kotlin
/**
 * Definition for a Node.
 * class Node(var `val`: Int) {
 *     var left: Node? = null
 *     var right: Node? = null
 *     var next: Node? = null
 * }
 */

class Solution {
    fun connect(root: Node?): Node? {
        if (root == null) return root
        val q = ArrayDeque<Node>()
        q.addLast(root)
        while (q.isNotEmpty()) {
            val levelSize = q.size
            var prev: Node? = null
            repeat (levelSize) {
                val curr = q.removeFirst()
                curr.left?.let { q.addLast(it) }
                curr.right?.let { q.addLast(it) }
                prev?.let { it.next = curr }
                prev = curr
            }
        }
        return root
    }
}
```
## Complexity
### Time
`O(n)`
### Space
`O(n)`
## Alternative Solution if it's a perfect Binary tree
If it's a perfect binary tree we can take advantage of the fact that every node has 2 children and traverse the tree without the need of a queue using the next pointer. **Making this algorithm O(1) extra space!** Detailed description here: https://leetcode.com/problems/populating-next-right-pointers-in-each-node/discuss/1654181/C%2B%2BPythonJava-Simple-Solution-w-Images-and-Explanation-or-BFS-%2B-DFS-%2B-O(1)-Optimized-BFS
## Code
Submission link: https://leetcode.com/submissions/detail/720709293/

```kotlin
/**
 * Definition for a Node.
 * class Node(var `val`: Int) {
 *     var left: Node? = null
 *     var right: Node? = null
 *     var next: Node? = null
 * }
 */

class Solution {
    fun connect(root: Node?): Node? {
        var levelStart = root
        while (levelStart != null) {
            var curr = levelStart
            while (curr != null) {
                curr.left?.let { it.next = curr?.right }
                curr.right?.let { it.next = curr?.next?.left }
                curr = curr.next
            }
            levelStart = levelStart.left
        }
        return root
    }
}
```
## Complexity
### Time
`O(n)`
### Space
`O(1)` since we no longer need to keep track of a queue
