# Alient Dictionary
https://leetcode.com/problems/alien-dictionary/
## Solution
Since the given words are sorted lexicographically by the rules of the alien language, we can always compare two adjacent words to determine the ordering of the characters. Take Example-1 above: ["ba", "bc", "ac", "cab"]

Take the first two words "ba" and "bc". Starting from the beginning of the words, find the first character that is different in both words: it would be 'a' from "ba" and 'c' from "bc". Because of the sorted order of words (i.e. the dictionary!), we can conclude that 'a' comes before 'c' in the alien language.
Similarly, from "bc" and "ac", we can conclude that 'b' comes before 'a'.
These two points tell us that we are actually asked to find the topological ordering of the characters, and that the ordering rules should be inferred from adjacent words from the alien dictionary.

This makes the current problem similar to Tasks Scheduling Order, the only difference being that we need to build the graph of the characters by comparing adjacent words first, and then perform the topological sort for the graph to determine the order of the characters.
## Code (kotlin)
```kotlin
class Solution {
    fun alienOrder(words: Array<String>): String {
        val graph = mutableMapOf<Char, MutableList<Char>>()
        val inDeg = mutableMapOf<Char, Int>()
        // initialize every character inDeg
        words.forEach { word ->
            word.forEach { char ->
                inDeg[char] = 0
            }
        }
        // initialize graph with dependencies based on rules
        for (row in 0 until words.size - 1) {
            val w1 = words[row]
            val w2 = words[row + 1]
            val overlap = minOf(w1.length, w2.length)
            for (col in 0 until overlap) {
                val parent = w1[col]
                val child = w2[col]
                if (parent != child) {
                    graph.getOrPut(parent, { ArrayList() }).add(child)
                    inDeg[child] = inDeg.getOrDefault(child, 0) + 1
                    // only the first differing character controls the sort order
                    break
                }
                // fail early if any of words are not sorted by length like we would expect
                if (col == overlap - 1 && w1.length > w2.length) return ""
            }
        }
        // fail early if all characters have a dependency
        if (graph.size == inDeg.size) return ""
        val q = ArrayDeque<Char>()
        // standard topological sort
        inDeg.entries.forEach { (char, count) ->
            if (count == 0) q.add(char)
        }
        val result = StringBuilder()
        while(q.isNotEmpty()) {
            val parent = q.removeFirst()
            result.append(parent)
            graph[parent]?.forEach { child ->
                inDeg[child] = inDeg.getOrDefault(child, 0) - 1
                if (inDeg[child] == 0) {
                    q.add(child)
                }
            }
        }
        return if (result.length == inDeg.size) result.toString() else ""
    }
}
```
## Complexity
### Time
`O(C + N)` where C is the number of characters and N is the number of 'rules' or edges in our DAG
### Space
`O(C + N)`
