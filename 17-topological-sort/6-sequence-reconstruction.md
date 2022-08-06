# Sequence Reconstruction
https://leetcode.com/problems/sequence-reconstruction/
## Solution
This problem is represents a topological sort with some modifications:
1. We will have to iterate over each subsequence and store the relationship of each consecutive pair of numbers that differ from one another.
2. We only care if it's possible to reconstruct the exact original sequence. Meaning if there are multiple topological sort orders then we return false. Thus if at any point we have more than a single number on the queue, we can return false.
3. Finally we must ensure that the topological sort we come up with matches the original sequence so we will check each value using an incrementing index.
## Code (kotlin)
```kotlin
class Solution {
    fun sequenceReconstruction(nums: IntArray, sequences: List<List<Int>>): Boolean {
        val graph = mutableMapOf<Int, MutableList<Int>>()
        val inDeg = mutableMapOf<Int, Int>()
        sequences.forEach { seq ->
            for (i in 0 until seq.size - 1) {
                val parent = seq[i]
                val child = seq[i + 1]
                inDeg.getOrPut(parent, { 0 })
                if (parent != child) {
                    graph.getOrPut(parent, { mutableListOf() }).add(child)
                    inDeg[child] = inDeg.getOrDefault(child, 0) + 1
                }
            }
            inDeg.getOrPut(seq[seq.size - 1], { 0 })
        }
        if (inDeg.keys.size != nums.size) return false // not enough numbers
        if (graph.keys.size == nums.size) return false // we have a cyclic dependency
        val q = ArrayDeque<Int>()
        inDeg.entries.forEach { (num, deg) ->
            if (deg == 0) q.addLast(num)
        }
        var i = 0
        while (q.size == 1) {
            val parent = q.removeFirst()
            if (nums[i++] != parent) return false
            graph[parent]?.forEach { child ->
                inDeg[child] = (inDeg[child] ?: 0) - 1
                if (inDeg[child] == 0) q.addLast(child)
            }
        }
        return if (i == nums.size) return true else false
    }
}
```
## Complexity
### Time
`O(n + e)` where n is the amount of distinct numbers, and e is the number of edges we have
### Space
`O(n + e)`
