# Path with Given Sequence
Given a binary tree and a number sequence, find if the sequence is present as a root-to-leaf path in the given tree.

Example 1:
```
   1
  / \
 7   9
    / \
   2   9
```
> Sequence: [1, 9, 9]  
Output: true Explanation: The tree has a path 1 -> 9 -> 9.

Example 2:
```
     1
    / \
   0   1
   |   | \
   1   6  5
```
> Sequence: [1, 0, 7]  
Output: false Explanation: The tree does not have a path 1 -> 0 -> 7.

> Sequence: [1, 1, 6]  
> Output: true Explanation: The tree has a path 1 -> 1 -> 6.
## Solution
## Code
Submission link: N/A
```
package com.ca.gl.recon.export

import io.smallrye.common.constraint.Assert.assertFalse
import io.smallrye.common.constraint.Assert.assertTrue
import org.junit.jupiter.api.Test

internal class TreeNode(var `val`: Int) {
    var left: TreeNode? = null
    var right: TreeNode? = null
}

internal object PathWithGivenSequence {
    tailrec fun findPath(root: TreeNode?, sequence: IntArray, index: Int = 0): Boolean {
        return if (root == null) {
            index == sequence.size
        } else if (index >= sequence.size || root.`val` != sequence[index]) {
            false
        } else {
            val newIndex = index + 1
            findPath(root.left, sequence, newIndex) || findPath(root.right, sequence, newIndex)
        }
    }

    @Test
    fun test() {
        val root = TreeNode(1)
        root.left = TreeNode(0)
        root.right = TreeNode(1)
        root.left!!.left = TreeNode(1)
        root.right!!.left = TreeNode(6)
        root.right!!.right = TreeNode(5)
        assertFalse(findPath(root, intArrayOf(1, 0, 7)))
        println("Tree has path sequence: " + findPath(root, intArrayOf(1, 0, 7)))
        assertTrue(findPath(root, intArrayOf(1, 1, 6)))
        println("Tree has path sequence: " + findPath(root, intArrayOf(1, 1, 6)))
    }
}
```
## Complexity
### Time
`O(n)`
### Space
`O(n)`
