# Reorganize String
https://leetcode.com/problems/reorganize-string/
## Solution
We can solve this using the Greedy approach. Our best chance of generating the string would be to always pick from the most frequent character at any given time. We also want to make sure that we don't add a character twice in a row, to do this we can keep track of the previous character used, and don't return it to the heap until the next character has been used.
1. add all the characters to a max heap by occurrence.
2. initialize prev to null
3. while the heap is not empty
  * add the curr character and decrement it's frequency
  * add prev character back in if it's frequency is greater than 0
  * set prev = curr
4. by the end of the loop if the last character remaining still had occurrences, it will not have been added back to the heap, therefore our resulting string will not be the expected length, in this case we can return "" instead
## Code (kotlin)
```kotlin
class Solution {
    fun reorganizeString(s: String): String {
        if (s.length == 1) return s
        val freq = s.groupingBy { it }.eachCount().toMutableMap()
        val max = PriorityQueue<Pair<Char, Int>>(compareByDescending{ it.second })
        freq.entries.forEach { max.offer(it.key to it.value) }
        var result = StringBuilder()
        var prev: Pair<Char, Int>? = null
        while (max.isNotEmpty()) {
            val curr = max.poll()
            result.append(curr.first)
            prev?.let {
                if (it.second > 0) {
                    max.offer(it)
                }
            }
            prev = curr.first to curr.second - 1
        } // if we only have one unique character left and it still has occurrences, then it will be set to prev and we won't have completed the string
        return if (result.length == s.length) result.toString() else ""
    }
}
```
## Code (java)
```java
```
## Complexity
### Time
`O(n log u)` where u is the number of unique characters in the input but since big O is the worst case it would be `O(n log n)`
### Space
`O(n)`
