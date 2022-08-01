# Minimum subset sum difference
https://leetcode.com/problems/last-stone-weight-ii/
## Solution Brute Force
For each number, we can either add it to subset a or subset b, then once we've chosen for every number we can return the absolute difference between a and b.
### Code
```kotlin
class Solution {
    fun lastStoneWeightII(stones: IntArray): Int {
        val totalSum = stones.reduce { acc, num -> acc + num }
        fun dfs(i: Int, sum1: Int, sum2: Int): Int {
            return if (i == stones.size) {
                Math.abs(sum1 - sum2)
            } else {
                val add1 = dfs(i + 1, sum1 + stones[i], sum2)
                val add2 = dfs(i + 1, sum1, sum2 + stones[i])
                minOf(add1, add2)
            }
        }
        return dfs(0, 0, 0)
    }
}
```
### Complexity
#### Time
`O(n^2)`
#### Space
`O(n^2)`
## Solution Memorization
We can use memoization to overcome the overlapping sub-problems.

We will be using a two-dimensional array to store the results of the solved sub-problems. We can uniquely identify a sub-problem from 'i' and 'sum1' as **'sum2' will always be the sum of the remaining numbers.**

Thus we can also remove sum2 from the function entirely.
### Code
```kotlin
class Solution {
    fun lastStoneWeightII(stones: IntArray): Int {
        val totalSum = stones.reduce { acc, num -> acc + num }
        val dp = Array<IntArray>(stones.size) { IntArray(totalSum + 1) { -1 } }
        fun dfs(i: Int, sum: Int): Int {
            return if (i == stones.size) {
                Math.abs(sum - (totalSum - sum))
            } else if (dp[i][sum] != -1) {
                dp[i][sum]
            } else {
                val add = dfs(i + 1, sum + stones[i])
                val result = if (add == 0) add else {
                    val skip = dfs(i + 1, sum)
                    minOf(add, skip)
                }
                dp[i][sum] = result
                result
            }
        }
        return dfs(0, 0)
    }
}
```
### Complexity
#### Time
`O(n*ts)` where ts is the total sum of the array
#### Space
`O(n*ts)`
## Solution Bottom-Up DP
Finally in order to transform this problem into the 0/1 knapsack problem we can think about the problem like this:

The lowest possible difference would be 0, which is creating two subsets who's values are both equal to totalSum / 2. If we can create just one subset who's sum is equal to totalSum / 2, then we know that we can create the other.

Thus the problem becomes, find the subset `a` whos sum is closest to totalSum / 2, then find the other subset `b = totalSum - a` and find the absolute difference between subset `a` and `b`

The first part of that problem matches the 0/1 knapsack problem exactly, except if we can't create a subset who's sum is exactly the target, we will iterate over our dp array until we find the closest value that we can create.

### Code
```kotlin
class Solution {
    fun lastStoneWeightII(stones: IntArray): Int {
        val totalSum = stones.reduce { acc, num -> acc + num }
        val target = totalSum / 2
        val dp = BooleanArray(target + 1) { t ->
                stones[0] == t // handle the base case
        }
        dp[0] = true // we can always create a subset equal to 0
        for (i in 1 until stones.size) {
            for (t in target downTo 1) {
                val weight = stones[i]
                dp[t] = dp[t] || (t >= weight && dp[t - weight])
            }
        }
        var closestSum = target
        while (!dp[closestSum] && closestSum >= 0) closestSum-- // find sum closest to target that we can create a subset of
        val otherSum = totalSum - closestSum
        return Math.abs(closestSum - otherSum)
    }
}
```
### Complexity
#### Time
`O(n*t)` where ts is totalSum / 2
#### Space
`O(t)`
