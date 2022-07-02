# Find Smallest Letter Great
Given a characters array letters that is sorted in non-decreasing order and a character target, return the smallest character in the array that is larger than target.

Note that the letters wrap around.

For example, if target == 'z' and letters == ['a', 'b'], the answer is 'a'.

Example 1:
```
Input: letters = ["c","f","j"], target = "a"
Output: "c"
```
Example 2:
```
Input: letters = ["c","f","j"], target = "c"
Output: "f"
```
Example 3:
```
Input: letters = ["c","f","j"], target = "d"
Output: "f"
```
Constraints:
* `2 <= letters.length <= 104`
* `letters[i] is a lowercase English letter.`
* `letters is sorted in non-decreasing order.`
* `letters contains at least two different characters.`
* `target is a lowercase English letter.`
## Solution
This problem is identical to the previous one except we are exluding characters that equal the target, so our checks become `<=` instead of `<`. And instead of returning -1 if we can't find a character, we are just returning the first character.
## Code (kotlin)
Submission link: https://leetcode.com/submissions/detail/736929195/
```
class Solution {
    fun nextGreatestLetter(letters: CharArray, target: Char): Char {
        var s = 0
        var e = letters.size - 1
        if (letters[e] <= target) return letters[0]
        while (s < e) {
            var m = s + (e - s) / 2
            if (letters[m] <= target) {
                s = m + 1
            } else {
                e = m
            }
        }
        return letters[s]
    }
}
```
## Code (java)
Submission link: https://leetcode.com/submissions/detail/736932682/
```
class Solution {
    public char nextGreatestLetter(char[] letters, char target) {
        int s = 0;
        int e = letters.length - 1;
        if (letters[e] <= target) return letters[0];
        while (s < e) {
            int m = s + (e - s) / 2;
            if (letters[m] <= target) {
                s = m + 1;
            } else {
                e = m;
            }
        }
        return letters[s];
    }
}
```
## Complexity
### Time
`O(log n)`
### Space
`O(1)`
