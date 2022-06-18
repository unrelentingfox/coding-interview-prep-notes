# Count path sums (path-sum-iii)
Given the root of a binary tree and an integer targetSum, return the number of paths where the sum of the values along the path equals targetSum.

The path does not need to start or end at the root or a leaf, but it must go downwards (i.e., traveling only from parent nodes to child nodes).

Example 1:
> Input: root = [10,5,-3,3,2,null,11,3,-2,null,1], targetSum = 8  
Output: 3  
Explanation: The paths that sum to 8 are shown.

Example 2:
> Input: root = [5,4,8,11,null,13,4,7,2,null,null,5,1], targetSum = 22  
Output: 3

Constraints:
* The number of nodes in the tree is in the range [0, 1000].
* `-109 <= Node.val <= 109`
* `-1000 <= targetSum <= 1000`

## Solution
This problem follows the Binary Tree Path Sum pattern. We can follow the same DFS approach. But there will be four differences:

1. We will keep track of the current path in a list which will be passed to every recursive call.

2. Whenever we traverse a node we will do two things:
    * Add the current node to the current path.
    * As we added a new node to the current path, we should find the sums of all sub-paths ending at the current node. If the sum of any sub-path is equal to ‘S’ we will increment our path count.
3. We will traverse all paths and will not stop processing after finding the first path.

4. Remove the current node from the current path before returning from the function. This is needed to Backtrack while we are going up the recursive call stack to process other paths.

## Code
Submission link: https://leetcode.com/submissions/detail/725435786/
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
    fun pathSum(root: TreeNode?, targetSum: Int, path: LinkedList<Int> = LinkedList()): Int {
        return if (root == null) {
            return 0
        } else {
            path.addFirst(root.`val`)
            val sums = countTargetSumsFromStart(targetSum, path)
            val leftSums = pathSum(root.left, targetSum, path)
            val rightSums = pathSum(root.right, targetSum, path)
            path.removeFirst()
            return sums + leftSums + rightSums
        }
    }

    fun countTargetSumsFromStart(targetSum: Int, nums: List<Int>): Int {
        var sum = 0
        var count = 0
        nums.forEach { num ->
            sum += num
            if (sum == targetSum) count++
        }
        return count
    }
}
```
## Complexity
### Time
The time complexity of the above algorithm is `O(N^2)` in the worst case, where `N` is the total number of nodes in the tree. This is due to the fact that we traverse each node once, but for every node, we iterate the current path. The current path, in the worst case, can be `O(N)` (in the case of a skewed tree). But, if the tree is balanced, then the current path will be equal to the height of the tree, i.e., `O(logN)`. So the best case of our algorithm will be `O(NlogN)`.
### Space
The space complexity of the above algorithm will be `O(N)`. This space will be used to store the recursion stack. The worst case will happen when the given tree is a linked list (i.e., every node has only one child). We also need `O(N)` space for storing the currentPath in the worst case.

Overall space complexity of our algorithm is `O(N)`.
