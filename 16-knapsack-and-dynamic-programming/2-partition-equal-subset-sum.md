# Partition Equal Subset Sum
https://leetcode.com/problems/partition-equal-subset-sum/
# Solution BRUTE FORCE
Using the constraints of the problem we know that if we can create two equal sum subsets, that means that we should be able to create one subset who's sum is equal to the original array's sum divided by 2.

That means we can restate the problem as: Return true if we can create a subset who's sum is equal to half of the sum of the original subset.

A brute force solution would be to for every index `i` decide to either add, or discard that number.
## Code (kotlin)
```kotlin
class Solution {
    fun canPartition(nums: IntArray): Boolean {
        val totalSum = nums.reduce{ acc, num -> acc + num }
        if (totalSum % 2 == 1) return false // total sum is odd, so we can't possibly split this into two sets
        val target = totalSum / 2
        fun dfs(i: Int, sum: Int): Boolean {
            return if (i == nums.size || sum >= target) {
                sum == target
            } else {
                dfs(i + 1, sum) || dfs(i + 1, sum + nums[i])
            }
        }
        return dfs(0, 0)
    }
}
```
## Complexity
### Time
`O(n^2)`
### Space
`O(n^2)`
# Solution MEMORIZATION
We can improve out time complexity by using memorization and performing the same top-down search.
## Code (kotlin)
```kotlin
class Solution {
    fun canPartition(nums: IntArray): Boolean {
        val totalSum = nums.reduce{ acc, num -> acc + num }
        if (totalSum % 2 == 1) return false // total sum is odd, so we can't possibly split this into two sets
        val target = totalSum / 2
        val dp = Array(nums.size) { Array<Boolean?>(target + 1) { null }}
        fun dfs(i: Int, sum: Int): Boolean {
            return if (i == nums.size || sum >= target) {
                sum == target
            } else if (dp[i][sum] != null) {
                dp[i][sum]!!
            } else {
                dfs(i + 1, sum) || dfs(i + 1, sum + nums[i])
            }
        }
        return dfs(0, 0)
    }
}
```
## Complexity
### Time
`O(n * t)` where n is the number of nums and t is the sum of the input array / 2
### Space
`O(n * t)`
# Solution DP BOTTOM UP
Finally we can convert this problem into a bottom up DP problem
## Code (kotlin)
```kotlin
class Solution {
    fun canPartition(nums: IntArray): Boolean {
        val totalSum = nums.reduce{ acc, num -> acc + num }
        if (totalSum % 2 == 1) return false // total sum is odd, so we can't possibly split this into two sets
        val target = totalSum / 2
        val dp = Array<BooleanArray>(nums.size) { BooleanArray(target + 1) { 
            t -> t == 0 || nums[0] == t
        }}
        for (i in 1 until nums.size) {
            for (t in 1 until target + 1) {
                dp[i][t] = dp[i - 1][t] || (t >= nums[i] && dp[i - 1][t - nums[i]])
            }
        }
        return dp[nums.size - 1][target]
    }
}
```
## Complexity
### Time
`O(n * t)` where n is the number of nums and t is the sum of the input array / 2
### Space
`O(n * t)`
# Solution DP BOTTOM UP final optimization
Our space complexity is still as bad as the memorization technique, to improve we can use a 1 dimensional array instead.

However in order to do this we will actually need to traverse the target values backwards. This is because we can only choose a number once. Walk through the example below by hand to understand. See if you can collapse the 2D array going left to right.
```
Input: [1,2,5,2]

      targets
nums  0 1 2 3 4 5
   1  1 1 0 0 0 0
   2  1 1 1 1 0 0
   5  1 1 1 1 0 1
   2  1 1 1 1 1 1
```

## Code (kotlin)
```kotlin
class Solution {
    fun canPartition(nums: IntArray): Boolean {
        val totalSum = nums.reduce{ acc, num -> acc + num }
        if (totalSum % 2 == 1) return false // total sum is odd, so we can't possibly split this into two sets
        val target = totalSum / 2
        val dp = BooleanArray(target + 1) { t -> t == 0 || t == nums[0] }
        for (i in 1 until nums.size) {
            for (t in target downTo 1) {
                dp[t] = dp[t] || (t >= nums[i] && dp[t - nums[i]])
            }
        }
        return dp[target]
    }
}
```
## Complexity
### Time
`O(n * t)` where n is the number of nums and t is the sum of the input array / 2
### Space
`O(t + 1)` since we only have a 1D array
