# Permutations
Given an array nums of distinct integers, return all the possible permutations. You can return the answer in any order.

Example 1:
```
Input: nums = [1,2,3]
Output: [[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]
```
Example 2:
```
Input: nums = [0,1]
Output: [[0,1],[1,0]]
```
Example 3:
```
Input: nums = [1]
Output: [[1]]
```
Constraints:
* `1 <= nums.length <= 6`
* `-10 <= nums[i] <= 10`
* `All the integers of nums are unique.`
## Solution
This problem follows the Subsets pattern and we can follow a similar Breadth First Search (BFS) approach. However, unlike Subsets, every permutation must contain all the numbers.

Let’s take the example-1 mentioned above to generate all the permutations. Following a BFS approach, we will consider one number at a time:

1. If the given set is empty then we have only an empty permutation set: []
1. Let’s add the first element (1), the permutations will be: [1]
1. Let’s add the second element (3), the permutations will be: [3,1], [1,3]
1. Let’s add the third element (5), the permutations will be: [5,3,1], [3,5,1], [3,1,5], [5,1,3], [1,5,3], [1,3,5]
1. Let’s analyze the permutations in the 3rd and 4th step. How can we generate permutations in the 4th step from the permutations of the 3rd step?

If we look closely, we will realize that when we add a new number (5), we take each permutation of the previous step and insert the new number in every position to generate the new permutations. For example, inserting ‘5’ in different positions of [3,1] will give us the following permutations:

1. Inserting ‘5’ before ‘3’: [5,3,1]
1. Inserting ‘5’ between ‘3’ and ‘1’: [3,5,1]
1. Inserting ‘5’ after ‘1’: [3,1,5]

## Code
Submission link: https://leetcode.com/submissions/detail/731160984/
```
class Solution {
    fun permute(nums: IntArray): List<List<Int>> {
        var perms = listOf(listOf<Int>())
        nums.forEach { num ->
            val newPerms = mutableListOf<List<Int>>()
            perms.forEach { perm ->
                // add each number in every possible position 0 - n
                for (i in 0..perm.size) {
                    val newPerm = LinkedList(perm)
                    newPerm.add(i, num)
                    newPerms.add(newPerm)
                }
            }
            perms = newPerms
        }
        return perms
    }
}
```
## Complexity
### Time
We know that there are a total of `N!` permutations of a set with ‘N’ numbers. In the algorithm above, we are iterating through all of these permutations with the help of the two ‘for’ loops. In each iteration, we go through all the current permutations to insert a new number in them on line 17 (line 23 for C++ solution). To insert a number into a permutation of size ‘N’ will take `O(N)`, which makes the overall time complexity of our algorithm `O(N*N!)`.
### Space
All the additional space used by our algorithm is for the result list and the queue to store the intermediate permutations. If you see closely, at any time, we don’t have more than `N!` permutations between the result list and the queue. Therefore the overall space complexity to store `N!` permutations each containing `N` elements will be `O(N*N!)`.
