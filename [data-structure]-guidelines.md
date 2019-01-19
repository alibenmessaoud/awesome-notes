Java : Algorithms and Data Structure ![alt tag](https://api.travis-ci.org/phishman3579/java-algorithms-implementation.svg?branch=master)
==============================

Algorithms and Data Structures implemented in Java

This is a collection of algorithms and data structures which I've implement over the years in my academic and professional life. The code isn't overly-optimized but is written to be correct and readable. The algorithms and data structures are well tested and, unless noted, are believe to be 100% correct.

Created by `Justin Wetherell <phishman3579>`: https://github.com/phishman3579/java-algorithms-implementation

## Table of Contents
- [Data Structures](#data-structures)
- [Mathematics](#mathematics)
- [Numbers](#numbers)
- [Graphs](#graphs)
- [Search](#search)
- [Sequences](#sequences)
- [Sorts](#sorts)
- [String Functions](#string-functions)

## Data Structures
* AVL Tree
* B-Tree
* Binary Heap (backed by an array or a tree)
* Binary Search Tree 
* Compact Suffix Trie (backed by a Patricia Trie
* Disjoint Set 
* Fenwick Tree {Binary Indexed Tree (BIT)}
* Graph
  + Undirected
  + Directed (Digraph)
* Hash Array Mapped Trie (HAMT)
* Hash Map (associative array)
* Interval Tree
* Implicit Key Treap
* KD Tree (k-dimensional tree or k-d tree)
* List [backed by an array or a linked list]
* LCP Array (Longest Common Prefix) [backed by a Suffix Array]
* Matrix
* Patricia Trie
* Quad-Tree (Point-Region or MX-CIF)
* Queue [backed by an array or a linked list]
* Radix Trie (associative array) [backed by a Patricia Trie
* Red-Black Tree
* Segment Tree
* Skip List
* Splay Tree
* Stack [backed by an array or a linked list]
* Suffix Array
* Suffix Tree (Ukkonen's algorithm)
* Suffix Trie [backed by a Trie]
* Ternary Search Tree
* Treap
* Tree
* Tree Map (associative array) [backed by an AVL Tree]
* Trie
* Trie Map (associative array) [backed by a Trie]

## Mathematics
* Distance
  + chebyshev
  + euclidean
* Division
  + using a loop
  + using recursion
  + using shifts and multiplication
  + using only shifts
  + using logarithm
* Multiplication
  + using a loop
  + using recursion
  + using only shifts
  + using logarithms
  + Fast Fourier Transform
* Exponentiation
  + recursive exponentiation
  + fast recursive exponentiation
  + fast modular recursive exponentiation
* Primes 
  + is prime
  + prime factorization
  + sieve of eratosthenes
  + Miller-Rabin test
  + Co-Primes (relatively prime, mutually prime)
  + Greatest Common Divisor
    - using Euclid's algorithm
    - using recursion
* Permutations
  + strings
  + numbers
* Modular arithmetic
  + add
  + subtract
  + multiply
  + divide
  + power
* Knapsack
* Ramer Douglas Peucker

## Numbers
* Integers
  + to binary String
    - using divide and modulus
    - using right shift and modulus
    - using BigDecimal
    - using divide and double
  + is a power of 2
    - using a loop
    - using recursion
    - using logarithm
    - using bits
  + to English (e.g. 1 would return "one")
* Longs
  + to binary String
    - using divide and modulus
    - using right shift and modulus
    - using BigDecimal
* Complex
  + addition
  + subtraction
  + multiplication
  + absolute value
  + polar value

## Graphs
* Find shortest path(s) in a Graph from a starting Vertex
  - Dijkstra's algorithm (non-negative weight graphs)
  - Bellman-Ford algorithm (negative and positive weight graphs)
* Find minimum spanning tree
  - Prim's (undirected graphs)
  - Kruskal's (undirected graphs)
* Find all pairs shortest path
  - Johnsons's algorithm (negative and positive weight graphs)
  - Floyd-Warshall (negative and positive weight graphs)
* Cycle detection
  - Depth first search while keeping track of visited Verticies
  - Connected Components
* Topological sort
* A* path finding algorithm
* Maximum flow
  - Push-Relabel
* Graph Traversal
  - Depth First Traversal
  - Breadth First Traversal
* Edmonds Karp
* Matching
  - Turbo Matching
* Lowest common ancestor in tree


## Search
* Get index of value in array
  + Linear
  + Quickselect
  + Binary [sorted array input only]
  + Lower bound [sorted array input only
  + Upper bound [sorted array input only
  + Optimized binary (binary until a threashold then linear) [sorted array input only]
  + Interpolation [sorted array input only]

## Sequences
* Find longest common subsequence (dynamic programming)
* Find longest increasing subsequence (dynamic programming)
* Find number of times a subsequence occurs in a sequence (dynamic programming)
* Find i-th element in a Fibonacci sequence
  + using a loop
  + using recursion
  + using matrix multiplication
  + using Binet's formula
* Find total of all elements in a sequence(Arithmetic Progression)
  + using a loop
  + using Triangular numbers
* Largest sum of contiguous subarray (Kadane's algorithm)
* Longest palin­dromic sub­se­quence (dynamic programming)

## Sorts
* American Flag Sort
* Bubble Sort
* Counting Sort (Integers only)
* Heap Sort
* Insertion Sort
* Merge Sort
* Quick Sort
* Radix Sort (Integers only)
* Shell's Sort

## String Functions
### String Functions
* Reverse characters in a string
  + using additional storage (a String or StringBuilder)
  + using in-place swaps
  + using in-place XOR
* Reverse words in a string
  + using char swaps and additional storage (a StringBuilder)
  + using StringTokenizer and additional (a String)
  + using split() method and additional storage (a StringBuilder and String[])
  + using in-place swaps
* Is Palindrome
  + using additional storage (a StringBuilder)
  + using in-place symetric element compares
* Subsets of characters in a String
* Edit (Levenshtein) Distance of two Strings (Recursive, Iterative)
### Manacher's algorithm (Find the longest Palindrome)
### KMP (Knuth–Morris–Pratt) Algorithm - Length of maximal prefix-suffix for each prefix
### String rotations
  + Find in lexicographically minimal string rotation
  + Find in lexicographically maximal string rotation
