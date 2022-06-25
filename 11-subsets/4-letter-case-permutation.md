# Letter Case Permutation
Given a string s, you can transform every letter individually to be lowercase or uppercase to create another string.

Return a list of all possible strings we could create. Return the output in any order.

Example 1:
```
Input: s = "a1b2"
Output: ["a1b2","a1B2","A1b2","A1B2"]
```
Example 2:
```
Input: s = "3z4"
Output: ["3z4","3Z4"]
```
Constraints:
* `1 <= s.length <= 12`
* `s consists of lowercase English letters, uppercase English letters, and digits.`
## Solution
This problem follows the Subsets pattern and can be mapped to Permutations.

Let’s take "ab7c" and generate all the permutations. Following a BFS approach, we will consider one character at a time. Since we need to preserve the character sequence, we can start with the actual string and process each character (i.e., make it upper-case or lower-case) one by one:

1. Starting with the actual string: "ab7c"
1. Processing the first character (‘a’), we will get two permutations: "ab7c", "Ab7c"
1. Processing the second character (‘b’), we will get four permutations: "ab7c", "Ab7c", "aB7c", "AB7c"
1. Since the third character is a digit, we can skip it.
1. Processing the fourth character (‘c’), we will get a total of eight permutations: "ab7c", "Ab7c", "aB7c", "AB7c", "ab7C", "Ab7C", "aB7C", "AB7C"

Let’s analyze the permutations in the 3rd and the 5th step. How can we generate the permutations in the 5th step from the permutations in the 3rd step?

If we look closely, we will realize that in the 5th step, when we processed the new character (‘c’), we took all the permutations of the previous step (3rd) and changed the case of the letter (‘c’) in them to create four new permutations.
## Code
Submission link: https://leetcode.com/submissions/detail/731219587/
```
class Solution {
    fun letterCasePermutation(s: String): List<String> {
        var perms = mutableListOf(s)
        repeat (s.length) { i ->
            if (s[i].isLetter()) { // skip digits
                repeat (perms.size) { permIndex ->
                    val perm = perms[permIndex].toCharArray()
                    if (perm[i].isLowerCase()) {
                        perm[i] = perm[i].toUpperCase()
                    } else {
                        perm[i] = perm[i].toLowerCase()
                    }
                    perms.add(String(perm))
                }
            }
        }
        return perms
    }
}
```
## Complexity
### Time
Since we can have `2^N` permutations at the most and while processing each permutation we convert it into a character array, the overall time complexity of the algorithm will be `O(L*2^N)`. Where `N` is the number of characters in the string and `L` is the length of the string.
### Space
All the additional space used by our algorithm is for the output list. Since we can have a total of `O(2^N)` permutations, the space complexity of our algorithm is `O(N*2^N)`.

# A word on permutations
The number of permutations can be calculated by `k^n` where k is the number of options you have and n is the length.

For example
* the number of permutations for a 5 digit number (0-9) would be `10^5` or `10 * 10 * 10 * 10 * 10`
* the number of permutations for a lowercase string of length 5 would be `26^5`
* the number of permutations of a string that includes lowercase and uppercase would be `52^5`

If the elements in the permutation need to be unique then it changes to `10 * 9 * 8 * 7 * 6` for a 5 digit number with unique digits.

