# Unique Binary Search Tree Count
Given an integer n, return the number of structurally unique BST's (binary search trees) which has exactly n nodes of unique values from 1 to n.

Example 1:
```
 1    1        2        3    3
  \    \      / \      /    /
   3    2    1   3    2    1
  /      \           /      \
 2        3         1        2

Input: n = 3
Output: 5
```
Example 2:
```
Input: n = 1
Output: 1
```
Constraints:
* `1 <= n <= 19`

## Solution (from the course, not optimal)
This problem is similar to Structurally Unique Binary Search Trees. Following a similar approach, we can iterate from 1 to ‘n’ and consider each number as the root of a tree and make two recursive calls to count the number of left and right sub-trees.
## Code
Submission link: https://leetcode.com/submissions/detail/736858738/
```
class Solution {
    fun numTrees(n: Int): Int {
        return if (n <= 1) {
            1
        } else {
            var trees = 0
            for (i in 1..n) {
                val left = numTrees(i - 1)
                val right = numTrees(n - i)
                trees += left * right
            }
            return trees
        }
    }
}
```
## Complexity
### Time
`O(n * 2^n)`
### Space
`O(2^n)`

