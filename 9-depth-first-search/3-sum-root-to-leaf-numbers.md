# Sum Root to Leaf Numbers
You are given the root of a binary tree containing digits from 0 to 9 only.

Each root-to-leaf path in the tree represents a number.

For example, the root-to-leaf path `1 -> 2 -> 3` represents the number 123.
Return the total sum of all root-to-leaf numbers. Test cases are generated so that the answer will fit in a 32-bit integer.

A leaf node is a node with no children.

Example 1:
> Input: root = [1,2,3]  
Output: 25  
Explanation:  
The root-to-leaf path 1->2 represents the number 12.  
The root-to-leaf path 1->3 represents the number 13.  
Therefore, sum = 12 + 13 = 25.

Example 2:
> Input: root = [4,9,0,5,1]  
Output: 1026  
Explanation:  
The root-to-leaf path 4->9->5 represents the number 495.  
The root-to-leaf path 4->9->1 represents the number 491.  
The root-to-leaf path 4->0 represents the number 40.  
Therefore, sum = 495 + 491 + 40 = 1026.

Constraints:
* The number of nodes in the tree is in the range [1, 1000].
* `0 <= Node.val <= 9`
* The depth of the tree will not exceed 10.

## Solution
This problem follows the Binary Tree Path Sum pattern. We can follow the same DFS approach. The additional thing we need to do is to keep track of the number representing the current path.

How do we calculate the path number for a node? Taking the first example mentioned above, say we are at node `7`. As we know, the path number for this node is `17`, which was calculated by: `1 * 10 + 7 => 17`. We will follow the same approach to calculate the path number of each node.

## Code
Submission link: https://leetcode.com/submissions/detail/720836624/
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
    fun sumNumbers(root: TreeNode?, digits: Int = 0, result: Int = 0): Int {
        return if (root == null) {
            result
        } else {
            val newDigits = digits * 10 + root.`val`
            if (root.left == null && root.right == null) {
                result + newDigits
            } else {
                sumNumbers(root.right, newDigits, sumNumbers(root.left, newDigits, result))
            }
        }
    }
}
```
## Complexity
### Time
`O(n)`
### Space
`O(n)`
