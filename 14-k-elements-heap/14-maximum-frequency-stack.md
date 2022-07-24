# Maximum Frequency Stack
https://leetcode.com/problems/maximum-frequency-stack/
## Solution
This problem follows the Top 'K' Elements pattern, and shares similarities with Top 'K' Frequent Numbers.

We can use a Max Heap to store the numbers. Instead of comparing the numbers we will compare their frequencies so that the root of the heap is always the most frequently occurring number. There are two issues that need to be resolved though:

1. How can we keep track of the frequencies of numbers in the heap? When we are pushing a new number to the Max Heap, we don't know how many times the number has already appeared in the Max Heap. To resolve this, we will maintain a HashMap to store the current frequency of each number. Thus whenever we push a new number in the heap, we will increment its frequency in the HashMap and when we pop, we will decrement its frequency.
2. If two numbers have the same frequency, we will need to return the number which was pushed later while popping. To resolve this, we need to attach a sequence number to every number to know which number came first.

In short, we will keep three things with every number that we push to the heap:
```
1. number // value of the number
2. frequency // current frequency of the number when it was pushed to the heap
3. sequenceNumber // a sequence number, to know what number came first
```
## Code (kotlin)
```kotlin
class FreqStack() {
    private var insertOrder = 0
    private val max = PriorityQueue<Entry>(
        compareByDescending<Entry>{ it.freq }.thenByDescending{ it.insertOrder }
    )
    private val freqMap = mutableMapOf<Int, Int>()

    fun push(num: Int) {
        val freq = freqMap.getOrDefault(num, 0) + 1
        freqMap.put(num, freq)
        max.offer(Entry(num, freq, insertOrder++))
    }

    fun pop(): Int {
        val num = max.poll().num
        freqMap.get(num)?.let{
            if (it > 1) freqMap.put(num, it - 1)
            else freqMap.remove(num)
        }
        return num
    }

    private data class Entry(
        val num: Int,
        val freq: Int,
        val insertOrder: Int
    )
}
```
## Complexity
### Time
push: `O(log n)`
pop: `O(log n)`
### Space
`O(n)`

