# Subsets II - Subsets with duplicates
## Solution
## Code
Submission link: https://leetcode.com/submissions/detail/728554498/
```
class Solution {
    fun subsetsWithDup(nums: IntArray): List<List<Int>> {
        nums.sort() // modifying the input, could be replaced by creating a new list with sorted()
        val result = mutableListOf(listOf<Int>())
        var newSets = listOf<List<Int>>()
        for (i in 0 until nums.size) {
            if (i > 0 && nums[i - 1] == nums[i]) {
                newSets = newSets.map { it + nums[i] }
            } else {
                newSets = result.map { it + nums[i] }
            }
            result.addAll(newSets)
        }
        return result
    }
}
```
## Complexity
### Time
Since, in each step, the number of subsets doubles (if not duplicate) as we add each element to all the existing subsets, therefore, we will have a total of `O(2^N)` subsets, where `N` is the total number of elements in the input set. And since we construct a new subset from an existing set, therefore, the time complexity of the above algorithm will be `O(N*2^N)`.
### Space
All the additional space used by our algorithm is for the output list. Since, at most, we will have a total of `O(2^N)` subsets, and each subset can take up to `O(N)` space, therefore, the space complexity of our algorithm will be `O(N*2^N)`.
