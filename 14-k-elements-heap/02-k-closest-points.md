# K closest points
https://leetcode.com/problems/k-closest-points-to-origin/
## Solution
This can be solved similar to the previous problems, we can use a maxheap to store the first k elements, then iterate throught the remaining elements swapping in elements that are closer than the top of the maxHeap.
## Code (kotlin)
```kotlin
import kotlin.math.pow

class Solution {
    fun kClosest(points: Array<IntArray>, k: Int): Array<IntArray> {
        val maxDist = PriorityQueue<Int>(compareByDescending {
            dist(points[it])
        })
        repeat (points.size) { i ->
            if (i < k) {
                maxDist.offer(i)
            } else if (dist(points[i]) < dist(points[maxDist.peek()])) {
                maxDist.poll()
                maxDist.offer(i)
            }
        }
        return maxDist.map { points[it] }.toTypedArray()
    }

    fun dist(point: IntArray): Double {
        return Math.sqrt(point[0].toDouble().pow(2) + point[1].toDouble().pow(2))
    }
}
/*
*/
```
## Code (java)
```java
class Solution {
    public int[][] kClosest(int[][] points, int k) {
        PriorityQueue<int[]> max = new PriorityQueue((a, b) -> dist((int[]) b).compareTo(dist((int[]) a)));
        for (int i = 0; i < k; i++) {
            max.offer(points[i]);
        }
        for (int i = k; i < points.length; i++) {
            int[] curr = points[i];
            if (dist(curr) < dist(max.peek())) {
                max.poll();
                max.offer(curr);
            }
        }
        int[][] result = new int[k][2];
        for (int i = k - 1; i >= 0; i--) {
            result[i] = max.poll();
        }
        return result;
    }

    public Double dist(int[] point) {
        return Math.sqrt(Math.pow(point[0], 2.0) + Math.pow(point[1], 2.0));
    }
}
```
## Complexity
### Time
`O(n)` go through all points
*
`O(log k)` insert into priority queue
=
`O(n log k)` which is better than sorting the entire list which would be O(n log n)
### Space
`O(k)`
