# Binary Tree Path Sum II
Given the root of a binary tree and an integer targetSum, return all root-to-leaf paths where the sum of the node values in the path equals targetSum. Each path should be returned as a list of the node values, not node references.

A root-to-leaf path is a path starting from the root and ending at any leaf node. A leaf is a node with no children.

Example 1:
> Input: root = [5,4,8,11,null,13,4,7,2,null,null,5,1], targetSum = 22  
Output: [[5,4,11,2],[5,8,4,5]]  
Explanation: There are two paths whose sum equals targetSum:  
5 + 4 + 11 + 2 = 22  
5 + 8 + 4 + 5 = 22

Example 2:
> Input: root = [1,2,3], targetSum = 5  
Output: []

Example 3:
> Input: root = [1,2], targetSum = 0  
Output: []

Constraints:
* The number of nodes in the tree is in the range [0, 5000].
* `-1000 <= Node.val <= 1000`
* `-1000 <= targetSum <= 1000`

## Solution
This problem follows the Binary Tree Path Sum pattern. We can follow the same DFS approach. There will be two differences:
1. Every time we find a root-to-leaf path, we will store it in a list.
1. We will traverse all paths and will not stop processing after finding the first path.

## Code
Submission link: https://leetcode.com/submissions/detail/720814862/
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
    fun pathSum(root: TreeNode?, targetSum: Int): List<List<Int>> {
        val result = mutableListOf<List<Int>>()
        pathSumRec(root, targetSum, listOf(), result)
        return result
    }

    fun pathSumRec(root: TreeNode?, targetSum: Int, path: List<Int>, result: MutableList<List<Int>>) {
        if (root == null) {
        } else if (root.left == null && root.right == null && root.`val` == targetSum) {
            result.add(path + listOf(root.`val`))
        } else {
            val newTarget = targetSum - root.`val`
            val newPath = path + listOf(root.`val`)
            pathSumRec(root.left, newTarget, newPath, result)
            pathSumRec(root.right, newTarget, newPath, result)
        }
    }
}
```

## Complexity
### Time
The time complexity of the above algorithm is `O(N^2)`, where `N` is the total number of nodes in the tree. This is due to the fact that we traverse each node once (which will take `O(N)`), and for every leaf node, we might have to store its path (by making a copy of the current path) which will take `O(N)`.

We can calculate a tighter time complexity of `O(NlogN)` from the space complexity discussion below.
### Space

If we ignore the space required for the allPaths list, the space complexity of the above algorithm will be `O(N)` in the worst case. This space will be used to store the recursion stack. The worst-case will happen when the given tree is a linked list (i.e., every node has only one child).

##### How can we estimate the space used for the allPaths array? Take the example of the following balanced tree:
Here we have seven nodes (i.e., `N = 7`). Since, for binary trees, there exists only one path to reach any leaf node, we can easily say that total root-to-leaf paths in a binary tree can’t be more than the number of leaves. As we know that there can’t be more than `(N+1)/2` leaves in a binary tree, therefore the maximum number of elements in allPaths will be `O((N+1)/2) = O(N)`. Now, each of these paths can have many nodes in them. For a balanced binary tree (like above), each leaf node will be at maximum depth. As we know that the depth (or height) of a balanced binary tree is `O(logN)` we can say that, at the most, each path can have `logN` nodes in it. This means that the total size of the allPaths list will be `O(N*logN)`. If the tree is not balanced, we will still have the same worst-case space complexity.

From the above discussion, we can conclude that our algorithm’s overall space complexity is `O(N*logN)`.

Also, from the above discussion, since for each leaf node, in the worst case, we have to copy `log(N)` nodes to store its path; therefore, the time complexity of our algorithm will also be `O(N*logN)`.
