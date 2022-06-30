# Generalized Abreviation
A word's generalized abbreviation can be constructed by taking any number of non-overlapping and non-adjacent substrings and replacing them with their respective lengths.

* For example, "abcde" can be abbreviated into:
  * "a3e" ("bcd" turned into "3")
  * "1bcd1" ("a" and "e" both turned into "1")
  * "5" ("abcde" turned into "5")
  * "abcde" (no substrings replaced)
* However, these abbreviations are invalid:
  * "23" ("ab" turned into "2" and "cde" turned into "3") is invalid as the substrings chosen are adjacent.
  * "22de" ("ab" turned into "2" and "bc" turned into "2") is invalid as the substring chosen overlap.
  * Given a string word, return a list of all the possible generalized abbreviations of word. Return the answer in any order.

Example 1:
```
Input: word = "word"
Output: ["4","3d","2r1","2rd","1o2","1o1d","1or1","1ord","w3","w2d","w1r1","w1rd","wo2","wo1d","wor1","word"]
```
Example 2:
```
Input: word = "a"
Output: ["1","a"]
```
Constraints:
* `1 <= word.length <= 15`
* `word consists of only lowercase English letters.`
## Solution
This problem follows the Subsets pattern and can be mapped to Balanced Parentheses. We can follow a similar BFS approach.

Let’s take Example-1 mentioned above to generate all unique generalized abbreviations. Following a BFS approach, we will abbreviate one character at a time. At each step, we have two options:

1. Add the current character to the abreviation
2. Skip the current character (inrementing the current abreviation count if any)

Following these two rules, let’s abbreviate BAT:

1. Start with an empty word: “”
2. At every step, we will take all the combinations from the previous step and apply the two abbreviation rules to the next character.
3. Take the empty word from the previous step and add the first character to it. We can either add the character or skip it. This gives us two new words: 1, B.
4. In the next iteration, let’s add the second character. Applying the two rules on 1 will give us 2 and 1A. Applying the above rules to the other combination B gives us B1 and BA.
5. The final iteration will give us: 3, 2T, 1A1, 1AT, B2, B1T, BA1, BAT

## Code (Recursive)
Submission link: https://leetcode.com/submissions/detail/732120491/
```
class Solution {
    fun generateAbbreviations(
        word: String,
        i: Int = 0,
        abv: StringBuilder = StringBuilder(word.length),
        skips: Int = 0,
        result: MutableList<String> = mutableListOf()
    ): List<String> {
        return if (i == word.length) {
            if (skips > 0) abv.append(skips)
            result.add(abv.toString())
            result
        } else {
            val newAbv = StringBuilder(abv)
            if (skips > 0) newAbv.append(skips)
            newAbv.append(word[i])
            generateAbbreviations(word, i + 1, newAbv, 0, result)
            generateAbbreviations(word, i + 1, abv, skips + 1, result)
            result
        }
    }
}
```
## Code (Iterative)
Submission link: https://leetcode.com/submissions/detail/732133392/
```
class Solution {
    private data class Abv(val i: Int, val builder: StringBuilder, val skips: Int)

    fun generateAbbreviations(word: String): List<String> {
        val result = mutableListOf<String>()
        val q = ArrayDeque<Abv>()
        q.offer(Abv(0, StringBuilder(word.length / 2), 0))
        while (q.isNotEmpty()) {
            val curr = q.poll()
            if (curr.i == word.length) {
                if (curr.skips > 0) curr.builder.append(curr.skips)
                result.add(curr.builder.toString())
            } else {
                val next = curr.i + 1
                // skip char
                q.add(Abv(next, StringBuilder(curr.builder), curr.skips + 1))
                // add char
                if (curr.skips > 0) curr.builder.append(curr.skips)
                curr.builder.append(word[curr.i])
                q.add(Abv(next, curr.builder, 0))
            }
        }
        return result
    }
}
```
## Complexity
### Time
`O(n * 2^n)` because there are 2 options for each index of the word meaning we will end up with a maximum of `2^n` permutations in the end. Since this is essentially a binary tree, there are `2^n` leaf nodes and `2^n - 1` intermediate nodes so the total number of nodes visited is `2^n + 2^n -1` which is asymtotically equivalent to `O(2^n)`. Then it takes `O(n)` to copy the abreviation each time so that makes our overall complexity `O(n * 2^n)`.
### Space
`O(n * 2^n)` because extra space for the output would be a list of `2^n` permutations of up to `n` length.
