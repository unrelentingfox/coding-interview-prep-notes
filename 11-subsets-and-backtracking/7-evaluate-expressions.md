# Different ways to Evaluate Expressions
Given a string expression of numbers and operators, return all possible results from computing all the different possible ways to group numbers and operators. You may return the answer in any order.

The test cases are generated such that the output values fit in a 32-bit integer and the number of different results does not exceed 104.

Example 1:
```
Input: expression = "2-1-1"
Output: [0,2]
Explanation:
((2-1)-1) = 0
(2-(1-1)) = 2
```
Example 2:
```
Input: expression = "2*3-4*5"
Output: [-34,-14,-10,-10,10]
Explanation:
(2*(3-(4*5))) = -34
((2*3)-(4*5)) = -14
((2*(3-4))*5) = -10
(2*((3-4)*5)) = -10
(((2*3)-4)*5) = 10
```

Constraints:
* `1 <= expression.length <= 20`
* `expression consists of digits and the operator '+', '-', and '*'.`
* `All the integer values in the input expression are in the range [0, 99].`

## Solution
This problem follows the Subsets pattern and can be mapped to Balanced Parentheses. We can follow a similar BFS approach.

Let's take Example-1 mentioned above to generate different ways to evaluate the expression.

1. We can iterate through the expression character-by-character.
1. we can break the expression into two halves whenever we get an operator (+, -, \*).
1. The two parts can be calculated by recursively calling the function.
1. Once we have the evaluation results from the left and right halves, we can combine them to produce all results.

Also see this good explanation: https://leetcode.com/problems/different-ways-to-add-parentheses/discuss/1294189/Easy-Solution-Faster-than-100-or-Explained-With-Diagrams
## Code (Recursive)
Submission link: https://leetcode.com/submissions/detail/732166380/
```
class Solution {
    fun diffWaysToCompute(expression: String): List<Int> {
        val result = mutableListOf<Int>()
        repeat (expression.length) { i ->
            val op = expression[i]
            if (!op.isDigit()) {
                val leftResults = diffWaysToCompute(expression.substring(0, i))
                val rightResults = diffWaysToCompute(expression.substring(i + 1))
                leftResults.forEach { left ->
                    rightResults.forEach { right ->
                        when (op) {
                            '+' -> result.add(left + right)
                            '-' -> result.add(left - right)
                            '*' -> result.add(left * right)
                        }
                    }
                }
            }
        }
        if (result.isEmpty()) result.add(expression.toInt()) // expression is a number
        return result
    }
}
```
## Complexity
### Time
Since there are `2^n` different combinations per operator and we will have to iterate over expressions of up to n length each iteration the estimated complexity is `O(n * n^2)` worst case. The actual time complexity `(O(4^n/\sqrt{n}))` is bounded by the Catalan number and is beyond the scope of a coding interview.
### Space
Space complexity is `O(n^2)` because we will be storing that many integers.
## Alternative Solution
The problem has overlapping subproblems, as our recursive calls can be evaluating the same sub-expression multiple times. To resolve this, we can use memoization and store the intermediate results in a HashMap. In each function call, we can check our map to see if we have already evaluated this sub-expression before.

This would be trading memory usage for speed.
