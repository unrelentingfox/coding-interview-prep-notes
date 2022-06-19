# Binary Tree Level Order Traversal II
Given the root of a binary tree, return the bottom-up level order traversal of its nodes' values. (i.e., from left to right, level by level from leaf to root).

Example 1:
>Input: root = [3,9,20,null,null,15,7]  
Output: [[15,7],[9,20],[3]]

Example 2:
>Input: root = [1]  
Output: [[1]]

Example 3:
> Input: root = []  
Output: []

Constraints:
* The number of nodes in the tree is in the range [0, 2000].
* `-1000 <= Node.val <= 1000`

## Solution
This solution is identical to the previous problem, except instead of adding levels at the end of the result list, we just add them to the beginning. We also use a LinkedList for our result since inserting at the front of a linked list is an O(1) operation.

## Code
Submission link: https://leetcode.com/submissions/detail/719998316/
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
    fun levelOrderBottom(root: TreeNode?): List<List<Int>> {
        val result = LinkedList<List<Int>>()
        if (root != null) {
            val q = ArrayDeque<TreeNode>().also { it.addLast(root) }
            while (q.isNotEmpty()) {
                val size = q.size
                val level = ArrayList<Int>(size)
                repeat (size) {
                    q.removeFirst().let { curr ->
                        level.add(curr.`val`)
                        curr.left?.let { q.addLast(it) }
                        curr.right?.let { q.addLast(it) }
                    }
                }
                result.addFirst(level)
            }
        }
        return result
    }
}
```
## Complexity
### Time
`O()`
### Space
`O()`
