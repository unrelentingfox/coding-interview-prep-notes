# Binary Tree Maximum Path Sum
## Solution (recursive)
## Code
Submission link: N/A
```
```
## Complexity
### Time
`O()`
### Space
`O()`

## Solution (Iterative, post-order)
## Code
Submission link: https://leetcode.com/submissions/detail/725526852/
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
    fun maxPathSum(root: TreeNode?): Int {
        var maxPathSum = Int.MIN_VALUE
        if (root == null) return maxPathSum // shouldn't happen
        else if (root.left == null && root.right == null) return root?.`val`
        val stack = ArrayDeque<TreeNode>()
        val maxPath = mutableMapOf<TreeNode, Int>()
        stack.push(root)
        while (stack.isNotEmpty()) {
            val curr = stack.peek()
            val left = curr.left
            val right = curr.right
            if (left != null && !maxPath.contains(left)) {
                stack.push(left)
            } else if (right != null && !maxPath.contains(right)) {
                stack.push(right)
            } else {
                stack.pop()
                val leftMax = left?.let { maxPath.remove(it) } ?: Int.MIN_VALUE
                val rightMax = right?.let { maxPath.remove(it) } ?: Int.MIN_VALUE
                val currMax = maxOf(0, leftMax, rightMax) + curr.`val`
                maxPath.put(curr, currMax)
                maxPathSum = maxOf(
                    maxPathSum,
                    maxOf(
                        maxOf(leftMax, 0) + curr.`val` + maxOf(rightMax, 0),
                        leftMax,
                        rightMax
                    )
                )
            }
        }
        return maxPathSum
    }
}
```
## Complexity
### Time
`O(n)`
### Space
`O(n)`
