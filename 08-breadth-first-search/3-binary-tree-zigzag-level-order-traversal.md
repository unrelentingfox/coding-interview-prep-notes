# Problem
Given the root of a binary tree, return the zigzag level order traversal of its nodes' values. (i.e., from left to right, then right to left for the next level and alternate between).

Example 1:
>Input: root = [3,9,20,null,null,15,7]  
Output: [[3],[20,9],[15,7]]

Example 2:
>Input: root = [1]  
Output: [[1]]

Example 3:
>Input: root = []  
Output: []

Constraints:
* The number of nodes in the tree is in the range [0, 2000].
* `-100 <= Node.val <= 100`

## Solution
This solution is identical to the previous two except we need to alternate between inserting to the beginning of the level list and inserting at the end of the level list. To do this we can just set a boolean value and reverse it every loop.
## Code
Submission link: https://leetcode.com/submissions/detail/720026087/
```kotlin
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
    fun zigzagLevelOrder(root: TreeNode?): List<List<Int>> {
        val result = mutableListOf<List<Int>>()
        if (root == null) return result
        val q = ArrayDeque<TreeNode>().also { it.addLast(root) }
        var leftToRight = true
        while (q.isNotEmpty()) {
            val levelSize = q.size
            val level = ArrayList<Int>(levelSize)
            repeat (levelSize) {
                val curr = q.removeFirst()
                if (leftToRight) {
                    level.add(curr.`val`)
                } else {
                    level.add(0, curr.`val`)
                }
                curr.left?.let { q.addLast(it) }
                curr.right?.let { q.addLast(it) }
            }
            result.add(level)
            leftToRight = !leftToRight
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
