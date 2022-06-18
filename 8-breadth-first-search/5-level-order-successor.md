# Level Order Successor
Given a binary tree and a node, find the level order successor of the given node in the tree. The level order successor is the node that appears right after the given node in the level order traversal.

Example 1:
```
Input:  1  , 3
       / \
      2   3
     / \
    4   5

Output: 4
```
## Solution
This is very similar to the previous ones except when we find the key, we simply return the next value in the traversal.
## Code
Submission link: N/A
```
/**
 * Example:
 * var ti = TreeNode(5)
 * var v = ti.`val`
 * Definition for a binary tree node.
 * class TreeNode(var `val`: Int) {
 *     var left: TreeNode? = null
 *     var right: TreeNode? = null
 * }
 */
class Solution {
    fun levelOrderSuccessor(root: TreeNode, key: Int): Int {
        val q = ArrayDeque<TreeNode>()
        q.addLast(root)
        var found == false
        while (q.isNotEmpty()) {
            val curr = q.removeFirst()
            curr.left?.let { q.addLast(it) }
            curr.right?.let { q.addLast(it) }
            if (curr.`val` == key) break
        }
        return q.removeFirst()
    }
}
```
## Complexity
### Time
`O(n)`
### Space
`O(n)`
