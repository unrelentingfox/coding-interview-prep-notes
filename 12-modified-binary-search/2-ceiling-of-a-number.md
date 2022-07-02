# Ceiling of a number
Given an array of numbers sorted in an ascending order, find the ceiling of a given number ‘key’. The ceiling of the ‘key’ will be the smallest element in the given array greater than or equal to the ‘key’.

Write a function to return the index of the ceiling of the ‘key’. If there isn’t any ceiling return -1.

Example 1:
```
Input: [4, 6, 10], key = 6
Output: 1
Explanation: The smallest number greater than or equal to '6' is '6' having index '1'.
```
Example 2:
```
Input: [1, 3, 8, 10, 15], key = 12
Output: 4
Explanation: The smallest number greater than or equal to '12' is '15' having index '4'.
```
Example 3:
```
Input: [4, 6, 10], key = 17
Output: -1
Explanation: There is no number greater than or equal to '17' in the given array.
```
Example 4:
```
Input: [4, 6, 10], key = -1
Output: 0
Explanation: The smallest number greater than or equal to '-1' is '4' having index '0'.
```
## Solution
This solution is the same as the previous problem, since at our loop goes until `s < e` that means at the end s and e will point to the same thing, and whatever number is either less than key, or it's the smallest number that is `>=` key.
## Code (Recursive)
Submission link: N/A
```
class CeilingOfANumber {
  public static int searchCeilingOfANumber(int[] arr, int key) {
    int s = 0;
    int e = arr.length - 1;
    if (key > arr[e]) // if the 'key' is bigger than the biggest element
      return -1
    while (s < e) {
      int m = s + (e - s) / 2; // left mid
      if (arr[m] < key) {
        s = m + 1;
      } else {
        e = m;
      }
    }
    if (arr[s] >= key) {
      return s;
    } else {
      return -1;
    }
  }

  public static void main(String[] args) {
    System.out.println(CeilingOfANumber.searchCeilingOfANumber(new int[] { 4, 6, 10 }, 6));
    System.out.println(CeilingOfANumber.searchCeilingOfANumber(new int[] { 1, 3, 8, 10, 15 }, 12));
    System.out.println(CeilingOfANumber.searchCeilingOfANumber(new int[] { 4, 6, 10 }, 17));
    System.out.println(CeilingOfANumber.searchCeilingOfANumber(new int[] { 4, 6, 10 }, -1));
  }
}
```
## Complexity
### Time
`O(log n)`
### Space
`O(1)`
