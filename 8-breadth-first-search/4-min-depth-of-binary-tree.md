# Minimum Depth of Binary Tree
Given a binary tree, find its minimum depth.

The minimum depth is the number of nodes along the shortest path from the root node down to the nearest leaf node.

`Note: A leaf is a node with no children.`

Example 1:
> Input: root = [3,9,20,null,null,15,7]  
Output: 2

Example 2:
> Input: root = [2,null,3,null,4,null,5,null,6]  
Output: 5

Constraints:
* The number of nodes in the tree is in the range [0, 105].
* `-1000 <= Node.val <= 1000`

## Solution
This solution is the same as the previous ones except we are ending our search early when we find a node with no children.
## Code
Submission link: https://leetcode.com/submissions/detail/720035296/
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
    fun minDepth(root: TreeNode?): Int {
        if (root == null) return 0
        var depth = 1
        val q = ArrayDeque<TreeNode>().also { it.addLast(root) }
        while (q.isNotEmpty()) {
            val levelSize = q.size
            repeat (levelSize) {
                val curr = q.removeFirst()
                if (curr.left == null && curr.right == null) {
                    return depth
                }
                curr.left?.let { q.addLast(it) }
                curr.right?.let { q.addLast(it) }
            }
            depth++
        }
        throw IllegalStateException("Invalid tree")
    }
}
```
## Complexity
### Time
`O(n)`
### Space
`O(n)`
