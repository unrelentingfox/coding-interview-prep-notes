# Binary Tree Level Order Traversal
Given the root of a binary tree, return the level order traversal of its nodes' values. (i.e., from left to right, level by level).

Example 1:
>Input: root = [3,9,20,null,null,15,7]  
Output: [[3],[9,20],[15,7]]

Example 2:
>Input: root = [1]  
Output: [[1]]

Example 3:
>Input: root = []  
Output: []

Constraints:
* The number of nodes in the tree is in the range [0, 2000].
* `-1000 <= Node.val <= 1000`

## Solution
Since we need to traverse all nodes of each level before moving onto the next level, we can use the Breadth First Search (BFS) technique to solve this problem.

We can use a Queue to efficiently traverse in BFS fashion. Here are the steps of our algorithm:
* Start by pushing the root node to the queue.
* Keep iterating until the queue is empty.
* In each iteration, first count the elements in the queue (let’s call it levelSize). We will have these many nodes in the current level.
* Next, remove levelSize nodes from the queue and push their value in an array to represent the current level.
* After removing each node from the queue, insert both of its children into the queue.
* If the queue is not empty, repeat from step 3 for the next level.

## Code
Submission link: https://leetcode.com/submissions/detail/719980094/
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
    fun levelOrder(root: TreeNode?): List<List<Int>> {
        val result = mutableListOf<List<Int>>()
        if (root == null) return result
        var q = ArrayDeque<TreeNode>()
        q.addLast(root)
        while (q.isNotEmpty()) {
            val size = q.size
            val row = mutableListOf<Int>()
            repeat (size) {
                q.removeFirst().let { curr ->
                    row.add(curr.`val`)
                    curr.left?.let { q.addLast(it) }
                    curr.right?.let { q.addLast(it) }
                }
            }
            result.add(row)
        }
        return result
    }
}
```
## Complexity
### Time
`O(n)` since we have to visit every node
### Space
`O(n)` because we need to hold a list of all of the elements, and we also need a queue of atleast n/2 (max at the bottom level).
