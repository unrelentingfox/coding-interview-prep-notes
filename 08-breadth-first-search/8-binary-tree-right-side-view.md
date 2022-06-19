# Binary Tree Right Side View
Given the root of a binary tree, imagine yourself standing on the right side of it, return the values of the nodes you can see ordered from top to bottom.

Example 1:
>Input: root = [1,2,3,null,5,null,4]  
Output: [1,3,4]

Example 2:
>Input: root = [1,null,3]  
Output: [1,3]

Example 3:
>Input: root = []  
Output: []

Constraints:
* The number of nodes in the tree is in the range [0, 100].
* `-100 <= Node.val <= 100`
## Solution
This is the same as all of the other level order traversals except we are just capturing the last node in each level.
## Code
Submission link: https://leetcode.com/submissions/detail/720754605/
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
    fun rightSideView(root: TreeNode?): List<Int> {
        val result = mutableListOf<Int>()
        if (root == null) return result
        val q = ArrayDeque<TreeNode>()
        q.addLast(root)
        while (q.isNotEmpty()) {
            val levelSize = q.size
            repeat (levelSize) { i ->
                q.removeFirst()?.let { curr ->
                    if (i == levelSize - 1) result.add(curr.`val`)
                    curr.left?.let { q.addLast(it) }
                    curr.right?.let { q.addLast(it) }
                }
            }
        }
        return result
    }
}
```
## Complexity
### Time
`O(n)`
### Space
`O(n)`
