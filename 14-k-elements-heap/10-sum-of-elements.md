# Sum of Elements
https://www.geeksforgeeks.org/sum-elements-k1th-k2th-smallest-elements/
## Solution
We can either use a minHeap and all all of the values to the heap, then poll until we reach k1, then add till we reach k2.

However a slightly better solution would be to use a max heap to keep track of a k2 - 1 sized heap since we won't need any more values than that.
## Code (java)
```java
import java.util.*;

class SumOfElements {
  public static int findSumOfElements(int[] nums, int k1, int k2) {
    PriorityQueue<Integer> max = new PriorityQueue<Integer>((a, b) -> b.compareTo(a));
    int heapSize = k2 - 1; // we only need the k2 - 1 value for adding
    for (int i = 0; i < heapSize; i++) {
      max.offer(nums[i]);
    }
    for (int i = heapSize; i < nums.length; i++) {
      if (nums[i] < max.peek()) {
        max.poll();
        max.offer(nums[i]);
      }
    }
    int result = 0;
    for (int i = heapSize; i > k1 && !max.isEmpty(); i--) {
      result += max.poll();
    }
    return result;
  }

  public static void main(String[] args) {
    int result = SumOfElements.findSumOfElements(new int[] { 1, 3, 12, 5, 15, 11 }, 3, 6);
    System.out.println("Sum of all numbers between k1 and k2 smallest numbers: " + result);

    result = SumOfElements.findSumOfElements(new int[] { 3, 5, 8, 7 }, 1, 4);
    System.out.println("Sum of all numbers between k1 and k2 smallest numbers: " + result);
  }
}
```
## Complexity
### Time
`O(n log k2)`
### Space
`O(k2)`
