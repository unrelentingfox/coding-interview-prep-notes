# The classic 0/1 Knapsack problem
Given the weights and profits of ‘N’ items, we are asked to put these items in a knapsack with a capacity ‘C.’ The goal is to get the maximum profit out of the knapsack items. Each item can only be selected once, as we don’t have multiple quantities of any item.

Let’s take Merry’s example, who wants to carry some fruits in the knapsack to get maximum profit. Here are the weights and profits of the fruits:
```
Items: { Apple, Orange, Banana, Melon }
Weights: { 2, 3, 1, 4 }
Profits: { 4, 5, 3, 7 }
Knapsack capacity: 5
```
Let’s try to put various combinations of fruits in the knapsack, such that their total weight is not more than 5:
```
Apple + Orange (total weight 5) => 9 profit
Apple + Banana (total weight 3) => 7 profit
Orange + Banana (total weight 4) => 8 profit
Banana + Melon (total weight 5) => 10 profit
```
This shows that Banana + Melon is the best combination as it gives us the maximum profit, and the total weight does not exceed the capacity.

# Problem Statement
Given two integer arrays to represent weights and profits of ‘N’ items, we need to find a subset of these items which will give us maximum profit such that their cumulative weight is not more than a given number ‘C.’ Each item can only be selected once, which means either we put an item in the knapsack or we skip it.

# Brute Force solution
We can simply try all combinations of the objects. Meaning we will write a recursive function where at each object we will decide from 2 options:

1. include the object
2. skip the object

we will continue this recursive iteration until we have met or exceeded our capacity.

## Pseudo code
```
for each item 'i'
  create a new set which INCLUDES item 'i' if the total weight does not exceed the capacity, and
     recursively process the remaining capacity and items
  create a new set WITHOUT item 'i', and recursively process the remaining items
return the set from the above two sets with higher profit
```
## Complexity
Time: `O(n^2)` where n is the number of objects we can choose from  
Space: `O(n^2)`

# Top Down DP - Memorization (recursive)
Memoization is when we store the results of all the previously solved sub-problems and return the results from memory if we encounter a problem that has already been solved.

Since we have two changing values (capacity and currentIndex) in our recursive function knapsackRecursive(), we can use a two-dimensional array to store the results of all the solved sub-problems. As mentioned above, we need to store results for every sub-array (i.e., for every possible index ‘i’) and every possible capacity ‘c.’

## Complexity
Time: `O(nc)` where c is the capacity + 1. Our memorization array ensures we don't visit more than `nc` sub-problems
Space: `O(nc)` since we have to remember up to `nc` sub-problems

# Bottom Up DP - (iterative)

Let’s try to populate our `dp[][]` array from the above solution by working in a bottom-up fashion. Essentially, we want to find the maximum profit for every sub-array and every possible capacity. This means that `dp[i][c]` will represent the maximum knapsack profit for capacity ‘c’ calculated from the first ‘i’ items.

So, for each item at index ‘i’ `(0 <= i < items.length)` and capacity ‘c’ `(0 <= c <= capacity)`, we have two options:

1. Exclude the item at index ‘i.’ In this case, we will take whatever profit we get from the sub-array excluding this `item => dp[i-1][c]`
2. Include the item at index ‘i’ if its weight is not more than the capacity. In this case, we include its profit plus whatever profit we get from the remaining capacity and from remaining `items => profit[i] + dp[i-1][c-weight[i]]`

Finally, our optimal solution will be maximum of the above two values:
```
dp[i][c] = max (dp[i-1][c], profit[i] + dp[i-1][c-weight[i]])
```
## Complexity
Time: `O(nc)` since we will be solving our way up all of the possible sub problems
Space: `O(nc)`

This is the same complexity as the previous iteration, here are the benefits of each:
* **Bottom Up** - implemented iteratively so we can avoid the recursive overhead and possibility of stack overflow
* **Top Down** - we only have to solve neccessary sub-problems, not ALL of the sub-problems. So in some cases this algorithm can be more efficient

## Printing Nodes
As we know, the final profit is at the bottom-right corner. Therefore, we will start from there to find the items that will be going into the knapsack.

As you remember, at every step, we had two options: include an item or skip it. If we skip an item, we take the profit from the remaining items (i.e., from the cell right above it); if we include the item, then we jump to the remaining profit to find more items.

Let’s understand this from the above example:
1. ‘22’ did not come from the top cell (which is 17); hence we must include the item at index ‘3’ (which is item ‘D’).
1. Subtract the profit of item ‘D’ from ‘22’ to get the remaining profit ‘6’. We then jump to profit ‘6’ on the same row.
1. ‘6’ came from the top cell, so we jump to row ‘2’.
1. Again, ‘6’ came from the top cell, so we jump to row ‘1’.
1. ‘6’ is different from the top cell, so we must include this item (which is item ‘B’).
1. Subtract the profit of ‘B’ from ‘6’ to get profit ‘0’. We then jump to profit ‘0’ on the same row. As soon as we hit zero remaining profit, we can finish our item search.
1. Thus, the items going into the knapsack are {B, D}.

## One Final Optimization
So far we have used a 2d array for our sub-problems. From what we learned about printing the nodes, we don't actually need a `nc` array for this problem, we can use a 1 dimensional array and when calculating the remaining weight, we can use `(capacity - weights[i]) % weights[i]`. This is because we can only choose each item once, so if we have a capacity higher than or equal to the current weight, it makes no difference in the result.

Then we are just overwriting the existing value in the array whenever we find a higher profit.

# All Solutions Implemented in Kotlin
```kotlin
import kotlin.system.measureNanoTime

internal class Knapsack {
    fun solveKnapsackBrute(
        profits: IntArray,
        weights: IntArray,
        capacity: Int,
        i: Int = 0
    ): Int {
        return if (i == profits.size) {
            0
        } else {
            val capacityWith = capacity - weights[i]
            val with = if (capacityWith >= 0)
                profits[i] + solveKnapsackBrute(profits, weights, capacityWith, i + 1)
            else 0
            val without = solveKnapsackBrute(profits, weights, capacity, i + 1)
            return maxOf(with, without)
        }
    }

    fun solveKnapsackMemory(
        profits: IntArray,
        weights: IntArray,
        capacity: Int,
        seen: Array<IntArray> = Array(profits.size) { IntArray(capacity + 1) {-1} },
        i: Int = 0
    ): Int {
        return if (i == profits.size) {
            0
        } else if (seen[i][capacity] > -1) {
            seen[i][capacity]
        } else {
            val capacityWith = capacity - weights[i]
            val with = if (capacityWith >= 0)
                profits[i] + solveKnapsackMemory(profits, weights, capacityWith, seen,  i + 1)
            else 0
            val without = solveKnapsackMemory(profits, weights, capacity, seen,  i + 1)
            return maxOf(with, without)
        }
    }

    fun solveKnapsackDp(
        profits: IntArray,
        weights: IntArray,
        capacity: Int,
    ): Int {
        val maxProfits: Array<IntArray> = Array(profits.size) { IntArray(capacity + 1) {-1} } // [item][capacity]
        repeat(profits.size) { i ->
            repeat(capacity + 1) { c ->
                if (i == 0) {
                    maxProfits[i][c] = if (weights[i] <= c) profits[i] else 0
                } else {
                    val prevMax = maxProfits[i - 1][c]
                    maxProfits[i][c] = if (weights[i] <= c) {
                        maxOf(profits[i] + maxProfits[i - 1][c - weights[i]], prevMax)
                    } else {
                        prevMax
                    }
                }
            }
        }
//        print(maxProfits)
//        printItems(weights, maxProfits)
        return maxProfits[profits.size - 1][capacity]
    }

    fun solveKnapsackDpLessMemory(
        profits: IntArray,
        weights: IntArray,
        capacity: Int,
    ): Int {
        val maxProfits: Array<Int> = Array(capacity + 1) {
                c -> if (weights[0] <= c) profits[0] else 0
        } // i to profit
        for (i in 1 until profits.size) {
            for (c in maxProfits.indices) {
                val newProfit = if (weights[i] <= c) {
                    val remainingCapacity = (c - weights[i]) % weights[i] // we can only choose i once, so any capacity larger than weights[i] doesn't matter.
                    profits[i] + maxProfits[remainingCapacity]
                } else 0
                if (newProfit > maxProfits[c]) { // if new is greater than existing
                    maxProfits[c] = newProfit
                }
            }
        }
        return maxProfits[capacity]
    }

    companion object {
        @JvmStatic
        fun main(args: Array<String>) {
            val ks = Knapsack()
            val profits = intArrayOf(1, 6, 10, 16)
            val weights = intArrayOf(1, 2, 3, 5)
            run(profits, weights, 6, "BRUTE", ks::solveKnapsackBrute)
            run(profits, weights, 7, "BRUTE", ks::solveKnapsackBrute)
            run(profits, weights, 6, "MEM", ks::solveKnapsackMemory)
            run(profits, weights, 7, "MEM", ks::solveKnapsackMemory)
            run(profits, weights, 6, "DP", ks::solveKnapsackDp)
            run(profits, weights, 7, "DP", ks::solveKnapsackDp)
            run(profits, weights, 6, "DP O(c) space", ks::solveKnapsackDpLessMemory)
            run(profits, weights, 7, "DP O(c) space", ks::solveKnapsackDpLessMemory)
        }

        fun run(profits: IntArray, weights: IntArray, capacity: Int, name: String, func: (a: IntArray, b: IntArray, c: Int) -> Int) {
            val result: Int
            val time = measureNanoTime {
                result = func(profits, weights, capacity)
            }
            println("$name: $capacity ---> $result in $time")
        }
    }
}
```
