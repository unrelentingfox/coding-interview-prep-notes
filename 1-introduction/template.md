# Problem
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
    tailrec fun findPath(root: TreeNode?, sequence: IntArray, index: Int = 0, sibling: TreeNode? = null): Boolean {
        return if (root == null) {
            index == sequence.size
        } else if (root.`val` != sequence[index]) {
            findPath(sibling, sequence, index)
        } else {
            findPath(root.right, sequence, index + 1, root.left)
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
`O()`
### Space
`O()`
