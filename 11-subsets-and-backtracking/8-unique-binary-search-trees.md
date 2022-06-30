# Unique Binary Search Trees
Given an integer n, return all the structurally unique BST's (binary search trees), which has exactly n nodes of unique values from 1 to n. Return the answer in any order.

Example 1:
```
 1    1        2        3    3
  \    \      / \      /    /
   3    2    1   3    2    1
  /      \           /      \
 2        3         1        2

Input: n = 3
Output: [[1,null,2,null,3],[1,null,3,2],[2,1,3],[3,1,null,null,2],[3,2,null,1]]
```
Example 2:
```
Input: n = 1
Output: [[1]]
```

Constraints:
* `1 <= n <= 8`
## Solution
This problem follows the Subsets pattern and is quite similar to Evaluate Expression. Following a similar approach, we can iterate from 1 to ‘n’ and consider each number as the root of a tree. All smaller numbers will make up the left sub-tree and bigger numbers will make up the right sub-tree. We will make recursive calls for the left and right sub-trees.
## Code (Recursive)
Submission link: https://leetcode.com/submissions/detail/732208733/
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
    fun generateTrees(n: Int): List<TreeNode?> {
        return generateTrees(1, n + 1)
    }

    private fun generateTrees(start: Int, end: Int): List<TreeNode?> {
        return if (start == end) {
            listOf(null)
        } else {
            val result = mutableListOf<TreeNode>()
            for (i in start until end) {
                val leftTrees = generateTrees(start, i)
                val rightTrees = generateTrees(i + 1, end)
                leftTrees.forEach { left ->
                    rightTrees.forEach { right ->
                        val root = TreeNode(i)
                        root.left = left
                        root.right = right
                        result.add(root)
                    }
                }
            }
            result
        }
    }
}
```
## Complexity
### Time
`O(n * 2^n)`
### Space
`O(2^n)`
