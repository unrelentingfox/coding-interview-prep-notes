# Single Number
https://leetcode.com/problems/single-number/
## Solution
This problem can be solved using the XOR operator. Since any number xor itself == 0, we know that if we xor all of the numbers in the array, then the result will be whatever number is by itself.
## Code (kotlin)
Submission link: https://leetcode.com/submissions/detail/742994276/
```kotlin
class Solution {
    fun singleNumber(nums: IntArray): Int {
        var result = 0
        for (i in 0 until nums.size) {
            result = result xor nums[i]
        }
        return result
    }
}
```
## Code (java)
Submission link: https://leetcode.com/submissions/detail/742997331/
```java
class Solution {
    public int singleNumber(int[] nums) {
        int result = 0;
        for (int i = 0; i < nums.length; i++) {
            result ^= nums[i];
        }
        return result;
    }
}
```
## Complexity
### Time
`O(n)`
### Space
`O(1)`
