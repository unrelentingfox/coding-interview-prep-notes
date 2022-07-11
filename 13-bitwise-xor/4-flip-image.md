# Flipping an Image
https://leetcode.com/problems/flipping-an-image/
## Solution
* Flip: We can flip the image in place by replacing ith element from left with the ith element from the right.
* Invert: We can take XOR of each element with 1. If it is 1 then it will become 0 and if it is 0 then it will become 1.

## Code (kotlin)
```kotlin
class Solution {
    fun flipAndInvertImage(image: Array<IntArray>): Array<IntArray> {
        val size = image.size
        image.forEach { row ->
            var start = 0
            while (start < (size + 1) / 2) {
                val swap = row[start] xor 1
                val end = size - start - 1
                row[start++] = row[end] xor 1
                row[end] = swap
            }
        }
        return image
    }
}

```
## Code (java)
```java
class Solution {
    fun flipAndInvertImage(image: Array<IntArray>): Array<IntArray> {
        val size = image.size
        image.forEach { row ->
            var start = 0
            while (start < (size + 1) / 2) {
                val swap = row[start] xor 1
                val end = size - start - 1
                row[start++] = row[end] xor 1
                row[end] = swap
            }
        }
        return image
    }
}
```
## Complexity
### Time
`O(n)` n being the number of bits in the image.
### Space
`O(1)`
