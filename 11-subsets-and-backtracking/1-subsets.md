# Subsets
Given an integer array nums of unique elements, return all possible subsets (the power set).

The solution set must not contain duplicate subsets. Return the solution in any order.

Example 1:
```
Input: nums = [1,2,3]
Output: [[],[1],[2],[1,2],[3],[1,3],[2,3],[1,2,3]]
```
Example 2:
```
Input: nums = [0]
Output: [[],[0]]
```
Constraints:
* `1 <= nums.length <= 10`
* `-10 <= nums[i] <= 10`
* `All the numbers of nums are unique.`
## Solution (BFS)
To generate all subsets of the given set, we can use the Breadth First Search (BFS) approach. We can start with an empty set, iterate through all numbers one-by-one, and add them to existing sets to create new subsets.

Let’s take the example-2 mentioned above to go through each step of our algorithm:

Given set: [1, 5, 3]

1. Start with an empty set: [[]]
1. Add the first number (1) to all the existing subsets to create new subsets: [[], [1]];
1. Add the second number (5) to all the existing subsets: [[], [1], [5], [1,5]];
1. Add the third number (3) to all the existing subsets: [[], [1], [5], [1,5], [3], [1,3], [5,3], [1,5,3]].

Since the input set has distinct elements, the above steps will ensure that we will not have any duplicate subsets.
## Code
Submission link: https://leetcode.com/submissions/detail/728515571/
```
class Solution {
    fun subsets(nums: IntArray): List<List<Int>> {
       val result = mutableListOf<List<Int>>(listOf())
       nums.forEach { num ->
           val size = result.size
           for (i in 0 until result.size) {
               result.add(result[i] + num)
           }
       }
       return result
    }
}
```
Submission link: https://leetcode.com/submissions/detail/728518684/
```
class Solution {
    fun subsets(nums: IntArray): List<List<Int>> {
       val result = mutableListOf<List<Int>>(listOf())
       nums.forEach { num ->
           val newSubsets = result.map { it + num }
           result.addAll(newSubsets)
       }
       return result
    }
}
```
## Complexity
### Time
Since, in each step, the number of subsets doubles as we add each element to all the existing subsets, therefore, we will have a total of `O(2^N)` subsets, where `N` is the total number of elements in the input set. And since we construct a new subset from an existing set, therefore, the time complexity of the above algorithm will be `O(N*2^N)`.
### Space
All the additional space used by our algorithm is for the output list. Since we will have a total of `O(2^N)` subsets, and each subset can take up to `O(N)` space, therefore, the space complexity of our algorithm will be `O(N*2^N)`.

## Solution (Backtracking DFS)
Another way we can solve this problem is by recursively backtracking which is usually a brute force solution, but since we need to generate every possible outcome, it is the most efficient solution.

The idea is that for each recursive call we will make a choice, giving us our left and right subtrees. The choice is whether to add the current number or not.
## Code (Recursive)
Submission link: https://leetcode.com/submissions/detail/734708713/
```
class Solution {
    fun subsets(
        nums: IntArray,
        i: Int = 0,
        curr: LinkedList<Int> = LinkedList(),
        result: MutableList<List<Int>> = mutableListOf()
    ): List<List<Int>> {
        return if (i == nums.size) {
            result.also { it.add(curr.toList()) }
        } else {
            val next = i + 1
            subsets(nums, next, curr.also { it.addFirst(nums[i]) }, result)
            subsets(nums, next, curr.also { it.removeFirst() }, result)
            result
        }
    }
}
```
## Complexity
### Time
`O(2^n)` since we have two choices, thus two branches in our tree we have a binary tree.
### Space
`O(2^n)` used by the result set, and the call stack
