# Binary Tree Path Sum
Given the root of a binary tree and an integer targetSum, return true if the tree has a root-to-leaf path such that adding up all the values along the path equals targetSum.

A leaf is a node with no children.

Example 1:
> Input: root = [5,4,8,11,null,13,4,7,2,null,null,null,1], targetSum = 22  
Output: true  
Explanation: The root-to-leaf path with the target sum is shown.

Example 2:
> Input: root = [1,2,3], targetSum = 5  
Output: false  
Explanation: There two root-to-leaf paths in the tree:  
(1 --> 2): The sum is 3.  
(1 --> 3): The sum is 4.  
There is no root-to-leaf path with sum = 5.

Example 3:
> Input: root = [], targetSum = 0  
Output: false  
Explanation: Since the tree is empty, there are no root-to-leaf paths.

Constraints:
* The number of nodes in the tree is in the range [0, 5000].
* `-1000 <= Node.val <= 1000`
* `-1000 <= targetSum <= 1000`

## Solution
As we are trying to search for a root-to-leaf path, we can use the Depth First Search (DFS) technique to solve this problem.

To recursively traverse a binary tree in a DFS fashion, we can start from the root and at every step, make two recursive calls one for the left and one for the right child.

Here are the steps for our Binary Tree Path Sum problem:

Start DFS with the root of the tree.
1. If the current node is not a leaf node, do two things:
1. Subtract the value of the current node from the given number to get a new sum => S = S - node.value
1. Make two recursive calls for both the children of the current node with the new number calculated in the previous step.
1. At every step, see if the current node being visited is a leaf node and if its value is equal to the given number ‘S’. If both these conditions are true, we have found the required root-to-leaf path, therefore return true.
1. If the current node is a leaf but its value is not equal to the given number ‘S’, return false.

## Code
Submission link: https://leetcode.com/submissions/detail/720792509/
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
    tailrec fun hasPathSum(root: TreeNode?, targetSum: Int): Boolean {
        return if (root == null) {
            false
        } else if (root.left == null && root.right == null && root.`val` == targetSum) {
            true
        } else {
            val newTarget = targetSum - root.`val`
            hasPathSum(root.left, newTarget) || hasPathSum(root.right, newTarget)
        }
    }
}
```
## Complexity
### Time
`O(n)` where n is the number of nodes
### Space
`O(n)` in the worst case (every node has one child) we would have to store all of the nodes in either the call-stack or the stack generated using tailrec.

