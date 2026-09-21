# Complexity Analysis and Performance Report

This lab compares two correct approaches to the same problem. The goal is not simply to get the right answer, but to observe how algorithmic design changes runtime.

The benchmark output is written in CSV format and can be analyzed in Python, Excel, or a plotting tool.

## Tie-breaking rule for the frequency problem

When two values have the same frequency, the implementation returns the smaller numeric value.

This rule is used in both the naive and efficient implementations so the results are comparable.

## Problem 1 — Duplicate detection

### Algorithm A: Brute force

- Description: compare every pair of values in the array.
- Time complexity: O(n^2)
- Space complexity: O(1)
- Why: for each of n values, the code may compare against up to n - 1 other values.

### Algorithm B: Hash set

- Description: insert each value into a hash set; if a value is already present, a duplicate exists.
- Time complexity: O(n) average case
- Space complexity: O(n)
- Why: each value is inserted and looked up in expected constant time.

### Experimental comparison

1. The efficient implementation is usually faster for large inputs: The experiment shows that hash set implemetation was faster than the brute forse implementation using different sizes. 
2. As input size increases, the brute-force approach grows quadratically.
3. The measured timing should agree with the theoretical prediction: at 1000 values, the brute-force algorithm took about 2.5 million ns on average and the hash set algorithm took about 0.17 million ns. At 100000 values, the brute force took about 11.8 billion ns and the hash set algorithm took about 8.3 million ns. 
4. The gap becomes larger because O(n^2) grows much faster than O(n): The brute force runtime increase at a mush faster rate when the input was increasing, which shows the complexity of O(n^2).
5. The faster method uses extra memory for the hash table: The hash set method might use extra memory but its O(n) time complexity shows a better perfomance for larger inputs.

```mermaid
xychart-beta
    title Problem 3: Input Size vs Execution Time
    x-axis [1000, 10000, 100000, 1000000]
    y-axis "Time (ms)" 0 --> 5000
    line [1.0, 90, 5000, 5000] "Naive Scan"
    line [0.1, 2, 14, 100] "Hash Lookup"
```


## Problem 2 — Most frequent value

### Algorithm A: Naive counting

- Description: for each value, scan the whole array and count occurrences.
- Time complexity: O(n^2)
- Space complexity: O(1)
- Why: each value may require a full pass through the array.

### Algorithm B: Hash table counts

- Description: count frequencies in one pass and then inspect the counts.
- Time complexity: O(n) average case
- Space complexity: O(n)
- Why: hash table operations are expected to be constant time per value.

### Experimental comparison

1. The hash-based solution is expected to win for large arrays: The hash-based solution was mush faster than the naive counting algorithm at all input sizes. 
2. The gap becomes much more obvious as n grows: At 1000 values, the naive algorithm took about 2 million ns and the effecient method took about 37000 ns. At 100000 values, the naive algorithm took about 19 billion ns and the effecient algorithm took about 3.4 million ns. 
3. The empirical results should trend toward the theoretical expectations: The naive method's runtime increased quadratically when the inputs were getting larger, which shows the complexity of O(n^2).
4. The brute-force approach has a larger work count because it rescans the entire array for each candidate value.
5. The faster algorithm uses more memory to store the frequency table, but it has the time complexity of O(n) which reduces amount of repeated work. 

```mermaid
xychart-beta
    title Problem 3: Input Size vs Execution Time
    x-axis [1000, 10000, 100000, 1000000]
    y-axis "Time (ms)" 0 --> 5000
    line [1.0, 90, 5000, 5000] "Naive Scan"
    line [0.1, 2, 14, 100] "Hash Lookup"
```

## Problem 3 — Common elements between two arrays

### Algorithm A: Naive scan

- Description: take each value from the first array and scan the second array to see whether it appears there.
- Time complexity: O(n × m)
- Space complexity: O(k), where k is the number of distinct values found in common
- Why: each of the n values in the first array may require checking all m values in the second.

### Algorithm B: Hash-based lookup

- Description: construct a hash set from the second array, then examine each value in the first array.
- Time complexity: O(n + m) average case
- Space complexity: O(m)
- Why: set construction and lookup are each expected constant time per element.

### Experimental comparison

1. The hash-based solution is faster for large inputs: For this experiment, the hash-based solution was a little faster than the naive algorithm. 
2. The difference grows with the size of both arrays: At 1000 values, the naive algorithm took about 85000 ns and the efficient method took about 73000 ns. Also, at 100000 values, the naive algorithm took about 8.2 million ns and the efficient algorithm took about 6.8 million ns. 
3. The observed data should align with the expected O(n + m) versus O(n × m) behavior: yes, it aligns. O(n x m) grows faster than O(n + m) and when the input gets bigger, the difference will be bigger too. 
4. The gap widens because the naive approach repeats the same work many times.
5. The faster method trades extra memory for speed, but it has better performace for time complexity. 

```mermaid
xychart-beta
    title Problem 3: Input Size vs Execution Time
    x-axis [1000, 10000, 100000, 1000000]
    y-axis "Time (ms)" 0 --> 5000
    line [1.0, 90, 5000, 5000] "Naive Scan"
    line [0.1, 2, 14, 100] "Hash Lookup"
```

## Observations

The efficient versions are empirically faster because they reduce repeated work. The naive versions do the same comparisons again and again, which scales poorly as input size increases. The faster algorithm usually uses extra memory, which is the standard tradeoff in algorithm design.

The experiment result proves the theoretical complexity of both algorithms. The naive algorithms for problem 1 and 2 became much slower as the input size increased. For problem 3, although the naive algorithm was slower, but not as slow as the other two algorithms. This experiment shows that choosing efficient algorithm can significantly reduce time for larger values. As mentioned, faster algorithm can use extra memory, which is a tradeoff of this algorithm. My largest input is 100000 because the algorithm became very slow even at this value. 
