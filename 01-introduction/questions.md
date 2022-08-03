Copied from: https://gist.github.com/tykurtz/3548a31f673588c05c89f9ca42067bc4

## 2. Sliding Window
1. https://leetcode.com/problems/maximum-subarray/ # Close enough
2. https://leetcode.com/problems/minimum-size-subarray-sum/
3. https://leetcode.com/problems/longest-substring-with-at-most-k-distinct-characters/
4. https://leetcode.com/problems/fruit-into-baskets/
5. https://leetcode.com/problems/longest-substring-without-repeating-characters/
6. https://leetcode.com/problems/longest-repeating-character-replacement/
7. https://leetcode.com/problems/max-consecutive-ones-iii/
8. https://leetcode.com/problems/permutation-in-string/
9. https://leetcode.com/problems/find-all-anagrams-in-a-string/
10. https://leetcode.com/problems/minimum-window-substring/
10. https://leetcode.com/problems/substring-with-concatenation-of-all-words/

## 3. Two Pointers

1. https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/
2. https://leetcode.com/problems/remove-duplicates-from-sorted-array/
3. https://leetcode.com/problems/squares-of-a-sorted-array/
4. https://leetcode.com/problems/3sum/
5. https://leetcode.com/problems/3sum-closest/
6. https://leetcode.com/problems/3sum-smaller/
7. https://leetcode.com/problems/subarray-product-less-than-k/ #solve as if you were returning all of the subarrays instead of just counting them
8. https://leetcode.com/problems/sort-colors/
9. https://leetcode.com/problems/4sum/
10. https://leetcode.com/problems/backspace-string-compare/
11. https://leetcode.com/problems/shortest-unsorted-continuous-subarray/

## 4. Fast & Slow pointers
1. https://leetcode.com/problems/linked-list-cycle/
2. https://leetcode.com/problems/linked-list-cycle-ii/
3. https://leetcode.com/problems/happy-number/
4. https://leetcode.com/problems/middle-of-the-linked-list/
5. https://leetcode.com/problems/palindrome-linked-list/
6. https://leetcode.com/problems/reorder-list/
7. https://leetcode.com/problems/circular-array-loop/

## 5. Merge Intervals
1. https://leetcode.com/problems/merge-intervals/
2. https://leetcode.com/problems/insert-interval/
3. https://leetcode.com/problems/interval-list-intersections/
4. https://leetcode.com/problems/meeting-rooms/ or https://www.lintcode.com/problem/920/
5. https://leetcode.com/problems/meeting-rooms-ii/ or https://www.lintcode.com/problem/919/
6. https://leetcode.com/problems/corporate-flight-bookings/ or https://www.hackerrank.com/challenges/crush/problem or https://www.lintcode.com/problem/850/
7. https://leetcode.com/problems/employee-free-time/

## 6. Cyclic Sort

1. Couldn't find equivalent for the first question. The second question below encompasses the first one though. See https://leetcode.com/problems/missing-number/discuss/859510/C%2B%2B-O(N)-O(1)-using-Cyclic-Sort for how grokking the coding interview approached these problems. It uses the fact that we can sort the array in O(n) without comparison operators
2. https://leetcode.com/problems/missing-number/
3. https://leetcode.com/problems/find-all-numbers-disappeared-in-an-array/
4. https://leetcode.com/problems/find-all-duplicates-in-an-array/
5. https://leetcode.com/problems/set-mismatch/
6. https://leetcode.com/problems/first-missing-positive/
7. Same as last problem except return k missing positive numbers instead of the first https://leetcode.com/problems/kth-missing-positive-number/

## 7. In-place Reversal of a LinkedList
1. https://leetcode.com/problems/reverse-linked-list/
2. https://leetcode.com/problems/reverse-linked-list-ii/
3. https://leetcode.com/problems/reverse-nodes-in-k-group/
4. Next question is the same, but alternate each subgroup
5. https://leetcode.com/problems/rotate-list/

## 8. Tree Breadth First Search
1. https://leetcode.com/problems/binary-tree-level-order-traversal/
2. https://leetcode.com/problems/binary-tree-level-order-traversal-ii/
3. https://leetcode.com/problems/binary-tree-zigzag-level-order-traversal/
4. https://leetcode.com/problems/minimum-depth-of-binary-tree/
5. No equivalent.. this one is close: https://leetcode.com/problems/inorder-successor-in-bst/
6. https://leetcode.com/problems/populating-next-right-pointers-in-each-node-ii/ disregard the rule about no extra space for this pattern
7. Next question is the same, but connect end nodes to the next level instead of null
8. https://leetcode.com/problems/binary-tree-right-side-view/

## 9. Tree Depth First Search
1. https://leetcode.com/problems/path-sum/
2. https://leetcode.com/problems/path-sum-ii/
3. https://leetcode.com/problems/sum-root-to-leaf-numbers/
4. https://leetcode.com/problems/check-if-a-string-is-a-valid-sequence-from-root-to-leaves-path-in-a-binary-tree/
5. https://leetcode.com/problems/path-sum-iii/
6. https://leetcode.com/problems/diameter-of-binary-tree/
7. https://leetcode.com/problems/binary-tree-maximum-path-sum/

## 10. Two Heaps
1. https://leetcode.com/problems/find-median-from-data-stream/
2. https://leetcode.com/problems/sliding-window-median/
3. https://leetcode.com/problems/ipo/
4. https://leetcode.com/problems/find-right-interval/

## 11. Subsets
1. https://leetcode.com/problems/subsets/
2. https://leetcode.com/problems/subsets-ii/
3. https://leetcode.com/problems/permutations/
4. https://leetcode.com/problems/letter-case-permutation/
5. https://leetcode.com/problems/generate-parentheses/
6. https://leetcode.com/problems/generalized-abbreviation/ or https://www.lintcode.com/problem/779/
7. https://leetcode.com/problems/different-ways-to-add-parentheses/
8. https://leetcode.com/problems/unique-binary-search-trees-ii/
9. https://leetcode.com/problems/unique-binary-search-trees/

## 12. Modified Binary Search
1. https://leetcode.com/problems/binary-search/  # Close enough. The grokking problem allows sorted input arrays as ascending or descending, which only introduces a simple check
2. https://leetcode.com/problems/closest-binary-search-tree-value/ Problem is find index of smallest element greater or equal to input value
3. https://leetcode.com/problems/find-smallest-letter-greater-than-target/
4. https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/
5. https://leetcode.com/problems/search-in-a-sorted-array-of-unknown-size/
6. https://leetcode.com/problems/find-k-closest-elements/ (with K == 1)
7. https://leetcode.com/problems/peak-index-in-a-mountain-array/
8. https://leetcode.com/problems/find-in-mountain-array/
9. https://leetcode.com/problems/search-in-rotated-sorted-array/
10. Similar to previous, but find the number of rotations of the array, similar to https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/

## 13. Bitwise XOR
1. https://leetcode.com/problems/single-number/
2. https://leetcode.com/problems/single-number-iii/
3. https://leetcode.com/problems/complement-of-base-10-integer/
4. https://leetcode.com/problems/flipping-an-image/

## 14. Top 'K' elements
1. Same as second question, but the first Grokking question wants the top K instead of the bottom K
2. https://leetcode.com/problems/kth-largest-element-in-an-array
3. https://leetcode.com/problems/k-closest-points-to-origin/
4. https://leetcode.com/problems/minimum-cost-to-connect-sticks/
5. https://leetcode.com/problems/top-k-frequent-elements/
6. https://leetcode.com/problems/sort-characters-by-frequency/
7. https://leetcode.com/problems/kth-largest-element-in-a-stream/
8. https://leetcode.com/problems/find-k-closest-elements/
9. https://leetcode.com/problems/least-number-of-unique-integers-after-k-removals/ closest leetcode or https://www.geeksforgeeks.org/maximum-distinct-elements-removing-k-elements/ for exact
10. https://www.geeksforgeeks.org/sum-elements-k1th-k2th-smallest-elements/ no leetcode equivalent found
11. https://leetcode.com/problems/reorganize-string/
12. https://leetcode.com/problems/rearrange-string-k-distance-apart/
13. https://leetcode.com/problems/task-scheduler/
14. https://leetcode.com/problems/maximum-frequency-stack/

## 15. K-way merge
1. https://leetcode.com/problems/merge-k-sorted-lists/
2. Same as previous, but return the Kth smallest number
3. https://leetcode.com/problems/kth-smallest-element-in-a-sorted-matrix/
4. https://leetcode.com/problems/smallest-range-covering-elements-from-k-lists/
5. https://leetcode.com/problems/find-k-pairs-with-smallest-sums/ small difference, grokking has different sort order and wants the largest

## 16. 0/1 Knapsack
1. https://www.educative.io/courses/grokking-dynamic-programming-patterns-for-coding-interviews/RM1BDv71V60
2. https://www.educative.io/courses/grokking-dynamic-programming-patterns-for-coding-interviews/3jEPRo5PDvx or https://leetcode.com/problems/partition-equal-subset-sum/
3. https://www.educative.io/courses/grokking-dynamic-programming-patterns-for-coding-interviews/3j64vRY6JnR
4. https://leetcode.com/problems/last-stone-weight-ii/ similar concept, but problem description is more abstract.
5. https://leetcode.com/problems/combination-sum-ii/ similar, but return the number of combinations instead of the combinations
6. https://leetcode.com/problems/target-sum/
7. BONUS : Not in grokking, but I still found this very useful https://leetcode.com/problems/ones-and-zeroes/

## 17. Topological Sort
1. First problem is identical to the following three
2. https://leetcode.com/problems/course-schedule/
3. https://leetcode.com/problems/course-schedule-ii/
4. Same as above, but return all instead of any
5. https://leetcode.com/problems/alien-dictionary/
6. https://leetcode.com/problems/sequence-reconstruction/description/
7. https://leetcode.com/problems/minimum-height-trees/
