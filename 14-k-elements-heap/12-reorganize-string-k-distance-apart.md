# Reorganize String k distance apart
https://leetcode.com/problems/rearrange-string-k-distance-apart/
## Solution
We can solve this exactly the same as the previous problem. The only difference being instead of keeping track of one previous value, we keep track of a queue of size k previous values.
## Code (kotlin)
```kotlin
class Solution {
    fun rearrangeString(s: String, k: Int): String {
        val max = PriorityQueue<Pair<Char, Int>>(compareByDescending { it.second })
        s.groupingBy { it }.eachCount().forEach { (char, count) -> 
            max.offer(char to count)
        }
        val result = StringBuilder()
        val prev = ArrayDeque<Pair<Char, Int>>()
        while (max.isNotEmpty()) {
            val curr = max.poll()
            result.append(curr.first)
            prev.offer(curr.first to curr.second - 1)
            if (prev.size >= k) {
                prev.poll()?.let { if (it.second > 0) max.offer(it) }
            }
        }
        return if (result.length == s.length) result.toString() else ""
    }
}
```
## Code (java)
```java
```
## Complexity
### Time
`O(n log n)`
### Space
`O(n)`
