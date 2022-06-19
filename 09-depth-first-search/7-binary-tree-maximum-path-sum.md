# Binary Tree Maximum Path Sum
A path in a binary tree is a sequence of nodes where each pair of adjacent nodes in the sequence has an edge connecting them. A node can only appear in the sequence at most once. Note that the path does not need to pass through the root.

The path sum of a path is the sum of the node's values in the path.

Given the root of a binary tree, return the maximum path sum of any non-empty path.

Example 1:
> Input: root = [1,2,3]  
Output: 6  
Explanation: The optimal path is 2 -> 1 -> 3 with a path sum of 2 + 1 + 3 = 6.

Example 2:
> Input: root = [-10,9,20,null,null,15,7]  
Output: 42  
Explanation: The optimal path is 15 -> 20 -> 7 with a path sum of 15 + 20 + 7 = 42.

Constraints:
* The number of nodes in the tree is in the range [1, 3 * 104].
* `-1000 <= Node.val <= 1000`

## Solution (recursive)
This problem follows the Binary Tree Path Sum pattern and shares the algorithmic logic with Tree Diameter. We can follow the same DFS approach. The only difference will be to ignore the paths with negative sums. Since we need to find the overall maximum sum, we should ignore any path which has an overall negative sum.
## Code
Submission link: https://leetcode.com/problems/binary-tree-maximum-path-sum/submissions/
```
class Solution {
    fun maxPathSum(root: TreeNode?): Int {
        var maxPathSum = Int.MIN_VALUE
        fun maxConnectedPath(root: TreeNode?): Int {
            return if (root == null) {
                0
            } else {
                val left = maxOf(0, maxConnectedPath(root.left))
                val right = maxOf(0, maxConnectedPath(root.right))
                val localMaxPathSum = left + root.`val` + right
                maxPathSum = maxOf(maxPathSum, localMaxPathSum)
                maxOf(left, right) + root.`val`
            }
        }
        maxConnectedPath(root)
        return maxPathSum
    }
}
```
## Complexity
### Time
`O(n)`
### Space
`O(n)`

## Solution (Iterative)
## Code
Submission link: https://leetcode.com/submissions/detail/726222072/
```
class Solution {
    fun maxPathSum(root: TreeNode?): Int {
        var maxPathSum = Int.MIN_VALUE
        if (root == null) return maxPathSum // shouldn't happen
        else if (root.left == null && root.right == null) return root?.`val`
        val stack = ArrayDeque<TreeNode>()
        val maxConnectedPath = mutableMapOf<TreeNode, Int>()
        stack.push(root)
        while (stack.isNotEmpty()) {
            val curr = stack.peek()
            val left = curr.left
            val right = curr.right
            if (left != null && !maxConnectedPath.contains(left)) {
                stack.push(left)
            } else if (right != null && !maxConnectedPath.contains(right)) {
                stack.push(right)
            } else {
                stack.pop()
                val left = maxOf(0, left?.let { maxConnectedPath.remove(it) } ?: 0)
                val right = maxOf(0, right?.let { maxConnectedPath.remove(it) } ?: 0)
                val localMaxPathSum = left + curr.`val` + right
                maxPathSum = maxOf(maxPathSum, localMaxPathSum)
                maxConnectedPath.put(curr, maxOf(left, right) + curr.`val`)
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
