# Subarray Sum Equals K
Given a set of positive numbers, determine if a subset exists whose sum is equal to a given number ‘S’.

Example 1:
```
Input: {1, 2, 3, 7}, S=6
Output: True
The given set has a subset whose sum is '6': {1, 2, 3}
```
Example 2:
```
Input: {1, 2, 7, 1, 5}, S=10
Output: True
The given set has a subset whose sum is '10': {1, 2, 7}
```
Example 3:
```
Input: {1, 3, 4, 8}, S=6
Output: False
The given set does not have any subset whose sum is equal to '6'.
```
## Solution
This problem is exactly the same as the previous problem. So we can solve this using recursive brute force or memorization method, then we can also solve using DP with a 2D array and DP with a 1 dimensional array.
## Code (java)
```java
class SubsetSum {

  public boolean canPartition(int[] nums, int sum) {
    boolean[] dp = new boolean[sum + 1];
    for (int i = 0; i < dp.length; i++) dp[i] = false;
    dp[0] = true; // we can always create a set of 0
    for (int num : nums) {
      for (int s = sum; s > 0; s--) {
        dp[s] = dp[s] || (s >= num && dp[s - num]);
      }
    }
    return dp[sum];
  }

  public static void main(String[] args) {
    SubsetSum ss = new SubsetSum();
    int[] num = { 1, 2, 3, 7 };
    System.out.println(ss.canPartition(num, 6));
    num = new int[] { 1, 2, 7, 1, 5 };
    System.out.println(ss.canPartition(num, 10));
    num = new int[] { 1, 3, 4, 8 };
    System.out.println(ss.canPartition(num, 6));
  }
}
```
## Complexity
### Time
`O(n*sum)`
### Space
`O(sum)`
