# Sort characters by frequency
https://leetcode.com/problems/sort-characters-by-frequency/
## Solution
This problem follows the Top ‘K’ Elements pattern, and shares similarities with Top ‘K’ Frequent Numbers.

We can follow the same approach as discussed in the Top ‘K’ Frequent Numbers problem. First, we will find the frequencies of all characters, then use a max-heap to find the most occurring characters.
## Code (kotlin)
```kotlin
class Solution {
    fun frequencySort(s: String): String {
        val freq = s.toCharArray().fold(mutableMapOf<Char, Int>()) { acc, curr ->
            acc.put(curr, acc.getOrDefault(curr, 0) + 1)
            acc
        }
        val max = PriorityQueue<Map.Entry<Char, Int>>(compareByDescending { it.value })
        max.addAll(freq.entries)
        val result = StringBuilder()
        while (max.isNotEmpty()) {
            val curr = max.poll()
            repeat (curr.value) {
                result.append(curr.key)
            }
        }
        return result.toString()
    }
}
```
## Complexity
### Time
The time complexity of the above algorithm is `O(D*logD)` where ‘D’ is the number of distinct characters in the input string. This means, in the worst case, when all characters are unique the time complexity of the algorithm will be `O(N*logN)` where ‘N’ is the total number of characters in the string.
### Space
`O(n)` in the worst case we have to store every character in the map and heap
