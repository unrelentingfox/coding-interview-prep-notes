# Connect all level order siblings
Given a binary tree, connect each node with its level order successor. The last node of each level should point to the first node of the next level.
## Solution

This problem is the same as the previous one except we need to connect the levels together rather than connecting the end of the level to null. Only change is we do not reset prev to null.
## Code
Submission link: N/A
```
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
        var prev: Node? = null
        while (q.isNotEmpty()) {
            val levelSize = q.size
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

