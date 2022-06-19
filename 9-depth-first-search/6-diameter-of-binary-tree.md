# Diameter of Binary Tree
Given the root of a binary tree, return the length of the diameter of the tree.

The diameter of a binary tree is the length of the longest path between any two nodes in a tree. This path may or may not pass through the root.

The length of a path between two nodes is represented by the number of edges between them.

Example 1:
> Input: root = [1,2,3,4,5]  
Output: 3  
Explanation: 3 is the length of the path [4,2,1,3] or [5,2,1,3].

Example 2:
> Input: root = [1,2]  
Output: 1

Constraints:
* The number of nodes in the tree is in the range [1, 104].
* `-100 <= Node.val <= 100`

## Solution (recursive)
This problem follows the Binary Tree Path Sum pattern. We can follow the same DFS approach. There will be a few differences:

1. At every step, we need to find the height of both children of the current node. For this, we will make two recursive calls similar to DFS.
2. The height of the current node will be equal to the maximum of the heights of its left or right children, plus ‘1’ for the current node.
3. The tree diameter at the current node will be equal to the height of the left child plus the height of the right child plus ‘1’ for the current node: `diameter = leftTreeHeight + rightTreeHeight + 1`. To find the overall tree diameter, we will use a class level variable. This variable will store the maximum diameter of all the nodes visited so far, hence, eventually, it will have the final tree diameter.

## Code
Submission link: https://leetcode.com/submissions/detail/725485556/
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
    var diameter = 0

    fun diameterOfBinaryTree(root: TreeNode?): Int {
        return if (root == null) {
            0
        } else {
            height(root)
            val result = diameter
            diameter = 0
            return result
        }
    }

    fun height(root: TreeNode?): Int {
        return if (root == null) {
            0
        } else {
            val left = height(root.left)
            val right = height(root.right)
            diameter = maxOf(diameter, left + right)
            return maxOf(left, right) + 1
        }
    }
}
```
## Complexity
### Time
`O(n)`
### Space
`O(n)`
## Solution (Iterative, post-order)
We can also solve this problem iteratively using a stack and a hashMap to keep track of node heights.

## Code
Submission link: https://leetcode.com/submissions/detail/725495070/
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
    fun diameterOfBinaryTree(root: TreeNode?): Int {
        val stack = ArrayDeque<TreeNode>()
        val nodeToHeight = mutableMapOf<TreeNode, Int>()
        var diameter = 0
        stack.push(root)
        while (stack.isNotEmpty()) {
            val curr = stack.peek() // only peek for post-order traversal
            if (curr.left != null && !nodeToHeight.contains(curr.left)) {
                stack.push(curr.left)
            } else if (curr.right != null && !nodeToHeight.contains(curr.right)) {
                stack.push(curr.right)
            } else {
                stack.pop() // remove curr, starting post-order operation
                val leftHeight = curr.left?.let { nodeToHeight.getOrDefault(it, 0) } ?: 0
                val rightHeight = curr.right?.let { nodeToHeight.getOrDefault(it, 0) } ?: 0
                val currHeight = maxOf(leftHeight, rightHeight) + 1
                nodeToHeight.put(curr, currHeight)
                diameter = maxOf(diameter, leftHeight + rightHeight)
                // limit map to tree height by removing children after calculating height of parent
                curr.left?.let { nodeToHeight.remove(it) }
                curr.right?.let { nodeToHeight.remove(it) }
            }
        }
        return diameter
    }
}
```
## Complexity
### Time
`O(n)`
### Space
`O(n)` for the stack and the map
