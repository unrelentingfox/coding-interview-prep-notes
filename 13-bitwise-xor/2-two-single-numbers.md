# Two Single Numbers
https://leetcode.com/problems/single-number-iii/
## Solution
This problem is quite similar to Single Number, the only difference is that, in this problem, we have two single numbers instead of one. Can we still use XOR to solve this problem?

Let’s assume num1 and num2 are the two single numbers. If we do XOR of all elements of the given array, we will be left with XOR of num1 and num2 as all other numbers will cancel each other because all of them appeared twice. Let’s call this XOR n1xn2. Now that we have XOR of num1 and num2, how can we find these two single numbers?

As we know that num1 and num2 are two different numbers, therefore, **they should have at least one bit different between them**. If a bit in n1xn2 is ‘1’, this means that num1 and num2 have different bits in that place, as we know that we can get ‘1’ only when we do XOR of two different bits, i.e., `1 XOR 0 = 0 XOR 1 = 1`

We can take any arbitrary bit which is ‘1’ in n1xn2 and partition all numbers in the given array into two groups based on that bit. One group will have all those numbers with that bit set to ‘0’ and the other with the bit set to ‘1’. This will ensure that num1 will be in one group and num2 will be in the other. We can take XOR of all numbers in each group separately to get num1 and num2, as all other numbers in each group will cancel each other. Here are the steps of our algorithm:

1. Taking XOR of all numbers in the given array will give us XOR of num1 and num2, calling this XOR as n1xn2.
2. Find any bit which is set in n1xn2. We can take the rightmost bit which is ‘1’. Let’s call this rightmostSetBit.
3. Iterate through all numbers of the input array to partition them into two groups based on rightmostSetBit. Take XOR of all numbers in both the groups separately. Both these XORs are our required numbers.

## Code (kotlin)
Submission link: https://leetcode.com/submissions/detail/743011923/
```kotlin
class Solution {
    fun singleNumber(nums: IntArray): IntArray {
        val twoNumsXord = nums.reduce { acc, num -> acc xor num }

        var rightMostSetBit = 1
        while (twoNumsXord and rightMostSetBit == 0) { // while bit is not set
            rightMostSetBit = rightMostSetBit shl 1
        }

        var num1 = twoNumsXord
        var num2 = twoNumsXord
        nums.forEach { num ->
            if (num and rightMostSetBit == 0) { // bit is not set
                num1 = num1 xor num
            } else { // bit is set
                num2 = num2 xor num
            }
        }
        return intArrayOf(num1, num2)
    }
}
```
## Code (java)
Submission link: https://leetcode.com/submissions/detail/743033026/
```java
class Solution {
    public int[] singleNumber(int[] nums) {
        int twoNumsXord = Arrays.stream(nums).reduce(0, (acc, num) -> acc ^= num);
        int rightMostSetBit = 1;
        while ((twoNumsXord & rightMostSetBit) == 0) {
            rightMostSetBit = rightMostSetBit << 1;
        }
        int num1 = twoNumsXord;
        int num2 = twoNumsXord;
        for (int curr : nums) {
            if ((curr & rightMostSetBit) == 0) {
                num1 ^= curr;
            } else {
                num2 ^= curr;
            }
        }
        return new int[] {num1, num2};
    }
}
```
## Complexity
### Time
`O(n)`
### Space
`O(1)`
