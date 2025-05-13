## Question 1 (10 marks)

### 1(a) What is a greedy algorithm? What are its properties? (5 marks)

A **greedy algorithm** is one that makes, at each step, the choice that looks best at the moment, without reconsidering previous decisions. Its goal is to build up a solution piece by piece, always choosing the next piece with the highest immediate “benefit.”

**Key properties of greedy algorithms:**

1. **Greedy-choice property**
   A global optimum can be achieved by making a locally optimal (greedy) choice at each step.

2. **Optimal substructure**
   An optimal solution to the problem contains within it optimal solutions to subproblems.

3. **No backtracking**
   Once a choice is made, it is never changed. This keeps the algorithm simple and efficient.

4. **Efficiency**
   Often runs in _O(n log n)_ or _O(n)_ time (dominated by sorting or a single pass), making it faster than many dynamic-programming approaches.

---

### 1(b) Fractional knapsack: maximum benefit (5 marks)

A thief has capacity **12 ml** and these items:

| Item | Weight (ml) | Benefit (\$) | Benefit/Weight |
| :--: | :---------: | :----------: | :------------: |
|  1   |      4      |      12      |      3.00      |
|  2   |      8      |      32      |      4.00      |
|  3   |      3      |      40      |     13.33      |
|  4   |      6      |      30      |      5.00      |
|  5   |      1      |      50      |     50.00      |

**Greedy strategy:** take items in descending order of **benefit/weight**, filling capacity (allowing fractions).

1. **Item 5** (1 ml): take whole → benefit = 50, remaining 11 ml
2. **Item 3** (3 ml): take whole → +40, remaining 8 ml
3. **Item 4** (6 ml): take whole → +30, remaining 2 ml
4. **Item 2** (8 ml): only 2 ml left → take 2/8 fraction → +32·(2/8)= 8, remaining 0 ml
5. Item 1 skipped (no capacity)

> **Maximum total benefit = 50 + 40 + 30 + 8 = \$128.**

---

Below is a full solution to **Question 2** (total 8 marks).

---

## 2 (a) Definition, major strategies, and LCS by DP (6 marks)

### i. What is an algorithm?

An **algorithm** is a finite, well-defined sequence of instructions that takes some input, performs computations in a prescribed order, and produces an output in a finite amount of time.

### ii. Major algorithm-design strategies

1. **Brute-force (exhaustive search)**
2. **Divide and conquer** (e.g. Merge Sort, Quick Sort)
3. **Greedy** (always make the locally optimal choice)
4. **Dynamic programming** (store and reuse overlapping subproblems)
5. **Backtracking** (depth-first search with “undo” on failure)
6. **Branch and bound** (prune branches via bounds)
7. **Randomized and approximation** (for intractable problems)

---

### iii. LCS of two DNA sequences by dynamic programming

- **Mother’s DNA** X = `A A C C G G C C C C C C T T` (length m = 14)
- **Child’s DNA** Y = `A T C G A T C G C G C G A T` (length n = 14)

We build a table **c\[0…m]\[0…n]** where

$$
c[i][j] =
\begin{cases}
0, & i=0\text{ or }j=0,\\
c[i-1][j-1]+1, & X_i = Y_j,\\
\max\{c[i-1][j],\,c[i][j-1]\}, & \text{otherwise.}
\end{cases}
$$

Below is the full DP table for

$$
X = \text{A A C C G G C C C C C C T T},\quad m=14
$$

$$
Y = \text{A T C G A T C G C G C G A T},\quad n=14
$$

| X\Y   |  0  |  A  |  T  |  C  |  G  |  A  |  T  |  C  |  G  |  C  |  G  |  C  |  G  |  A  |  T  |
| :---- | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: |
| **0** |  0  |  0  |  0  |  0  |  0  |  0  |  0  |  0  |  0  |  0  |  0  |  0  |  0  |  0  |  0  |
| **A** |  0  |  1  |  1  |  1  |  1  |  1  |  1  |  1  |  1  |  1  |  1  |  1  |  1  |  1  |  1  |
| **A** |  0  |  1  |  1  |  1  |  1  |  2  |  2  |  2  |  2  |  2  |  2  |  2  |  2  |  2  |  2  |
| **C** |  0  |  1  |  1  |  2  |  2  |  2  |  2  |  3  |  3  |  3  |  3  |  3  |  3  |  3  |  3  |
| **C** |  0  |  1  |  1  |  2  |  2  |  2  |  2  |  3  |  3  |  4  |  4  |  4  |  4  |  4  |  4  |
| **G** |  0  |  1  |  1  |  2  |  3  |  3  |  3  |  3  |  4  |  4  |  5  |  5  |  5  |  5  |  5  |
| **G** |  0  |  1  |  1  |  2  |  3  |  3  |  3  |  3  |  4  |  4  |  5  |  5  |  6  |  6  |  6  |
| **C** |  0  |  1  |  1  |  2  |  3  |  3  |  3  |  4  |  4  |  5  |  5  |  6  |  6  |  6  |  6  |
| **C** |  0  |  1  |  1  |  2  |  3  |  3  |  3  |  4  |  4  |  5  |  5  |  6  |  6  |  6  |  6  |
| **C** |  0  |  1  |  1  |  2  |  3  |  3  |  3  |  4  |  4  |  5  |  5  |  6  |  6  |  6  |  6  |
| **C** |  0  |  1  |  1  |  2  |  3  |  3  |  3  |  4  |  4  |  5  |  5  |  6  |  6  |  6  |  6  |
| **C** |  0  |  1  |  1  |  2  |  3  |  3  |  3  |  4  |  4  |  5  |  5  |  6  |  6  |  6  |  6  |
| **C** |  0  |  1  |  1  |  2  |  3  |  3  |  3  |  4  |  4  |  5  |  5  |  6  |  6  |  6  |  6  |
| **T** |  0  |  1  |  2  |  2  |  3  |  3  |  4  |  4  |  4  |  5  |  5  |  6  |  6  |  6  |  7  |
| **T** |  0  |  1  |  2  |  2  |  3  |  3  |  4  |  4  |  4  |  5  |  5  |  6  |  6  |  6  |  7  |

- The bottom-right cell **c\[14]\[14] = 7**, so the **LCS length is 7**.
- One possible LCS (back-tracking) is

```
A A C C G G T
```

---

## 2 (b) Time-complexity and O-notation (2 marks)

> **f(n) = 6 n² + 4 n + 3. Show f(n) ∈ O(n²).**

By definition, f(n) = O(n²) if ∃ c > 0 and n₀ such that ∀ n ≥ n₀,

$$
6n² + 4n + 3 \;\le\; c \,n².
$$

For n ≥ 1:

$$
6n² + 4n + 3 \;\le\; 6n² + 4n² + 3n² = 13\,n².
$$

Thus we may choose **c = 13** and **n₀ = 1**, giving

$$
6n² + 4n + 3 \le 13\,n²,\quad \forall n\ge1.
$$

## Hence **f(n) = O(n²)**.

## Question 3 (10 marks)

### 3(a) Time complexity of the given code (3 marks)

```c
for (i = 0; i < n; i++)
    a[i] = 0;               // Θ(n)

for (i = 0; i < n; i++)
    for (j = 0; j < n; j++)
        a[i] += a[j] + i + j;  // inner loop Θ(n), outer loop Θ(n) ⇒ Θ(n²)
```

- **First loop:** Θ(n)
- **Nested loops:** Θ(n)·Θ(n) = Θ(n²)
- **Total:** Θ(n + n²) = **Θ(n²)**

---

### 3(b) Merge Sort algorithm (pseudocode) (4 marks)

```text
MERGE-SORT(A, p, r)
  if p < r
    q = ⌊(p + r) / 2⌋
    MERGE-SORT(A, p, q)        // sort left half
    MERGE-SORT(A, q+1, r)      // sort right half
    MERGE(A, p, q, r)          // merge the two sorted halves

MERGE(A, p, q, r)
  n1 = q - p + 1
  n2 = r - q
  // create L[1..n1], R[1..n2]
  for i = 1 to n1
    L[i] = A[p + i - 1]
  for j = 1 to n2
    R[j] = A[q + j]
  L[n1+1] = +∞; R[n2+1] = +∞  // sentinels
  i = j = 1
  for k = p to r
    if L[i] ≤ R[j]
      A[k] = L[i]; i = i + 1
    else
      A[k] = R[j]; j = j + 1
```

- **Time complexity:** Θ(n log n) in all cases.
- **Space complexity:** Θ(n) auxiliary.

---

### 3(c) Bubble Sort steps on \[30, 40, 20, 10, 15, 12, 12] (3 marks)

Perform successive passes, swapping adjacent out-of-order elements. After _k_ passes, the _k_ largest elements are in place at the end.

| Pass | Array after this pass                          |
| :--: | :--------------------------------------------- |
|  0   | 30, 40, 20, 10, 15, 12, 12 (initial)           |
|  1   | 30, 20, 10, 15, 12, 12, 40 (40 bubbled to end) |
|  2   | 20, 10, 15, 12, 12, 30, 40                     |
|  3   | 10, 15, 12, 12, 20, 30, 40                     |
|  4   | 10, 12, 12, 15, 20, 30, 40                     |
|  5   | 10, 12, 12, 15, 20, 30, 40 (no swaps → sorted) |

Below is the **complete bubble-sort** trace on the array
Here’s the **detailed bubble-sort trace** on the array

```
[30, 40, 20, 10, 15, 12, 12]
```

Showing every comparison and swap, pass by pass.

---

### Pass 1 (compare indices 0…5, bubbling the largest to index 6)

1. **(30 40 20 10 15 12 12)** → (30 40 20 10 15 12 12)
   Compare 30 ≤ 40 → no swap
2. **(30 40 20 10 15 12 12)** → (30 20 40 10 15 12 12)
   Compare 40 > 20 → swap
3. **(30 20 40 10 15 12 12)** → (30 20 10 40 15 12 12)
   Compare 40 > 10 → swap
4. **(30 20 10 40 15 12 12)** → (30 20 10 15 40 12 12)
   Compare 40 > 15 → swap
5. **(30 20 10 15 40 12 12)** → (30 20 10 15 12 40 12)
   Compare 40 > 12 → swap
6. **(30 20 10 15 12 40 12)** → (30 20 10 15 12 12 40)
   Compare 40 > 12 → swap

> End of Pass 1 array: **\[30, 20, 10, 15, 12, 12, 40]**

---

### Pass 2 (compare 0…4, bubbling 30 to index 5)

1. **(30 20 10 15 12 12 40)** → (20 30 10 15 12 12 40)
   30 > 20 → swap
2. **(20 30 10 15 12 12 40)** → (20 10 30 15 12 12 40)
   30 > 10 → swap
3. **(20 10 30 15 12 12 40)** → (20 10 15 30 12 12 40)
   30 > 15 → swap
4. **(20 10 15 30 12 12 40)** → (20 10 15 12 30 12 40)
   30 > 12 → swap
5. **(20 10 15 12 30 12 40)** → (20 10 15 12 12 30 40)
   30 > 12 → swap
6. **(20 10 15 12 12 30 40)** → (20 10 15 12 12 30 40)
   30 ≤ 40 → no swap

> End of Pass 2 array: **\[20, 10, 15, 12, 12, 30, 40]**

---

### Pass 3 (compare 0…3, bubbling 20 to index 4)

1. **(20 10 15 12 12 30 40)** → (10 20 15 12 12 30 40)
   20 > 10 → swap
2. **(10 20 15 12 12 30 40)** → (10 15 20 12 12 30 40)
   20 > 15 → swap
3. **(10 15 20 12 12 30 40)** → (10 15 12 20 12 30 40)
   20 > 12 → swap
4. **(10 15 12 20 12 30 40)** → (10 15 12 12 20 30 40)
   20 > 12 → swap
5. **(10 15 12 12 20 30 40)** → (10 15 12 12 20 30 40)
   20 ≤ 30 → no swap

> End of Pass 3 array: **\[10, 15, 12, 12, 20, 30, 40]**

---

### Pass 4 (compare 0…2, bubbling 15 to index 3)

1. **(10 15 12 12 20 30 40)** → (10 12 15 12 20 30 40)
   15 > 12 → swap
2. **(10 12 15 12 20 30 40)** → (10 12 12 15 20 30 40)
   15 > 12 → swap
3. **(10 12 12 15 20 30 40)** → (10 12 12 15 20 30 40)
   15 ≤ 20 → no swap

> End of Pass 4 array: **\[10, 12, 12, 15, 20, 30, 40]**

---

### Pass 5 (compare 0…1)

1. **(10 12 12 15 20 30 40)** → (10 12 12 15 20 30 40)
   10 ≤ 12 → no swap
2. **(10 12 12 15 20 30 40)** → (10 12 12 15 20 30 40)
   12 ≤ 12 → no swap

> **No swaps in Pass 5 ⇒ array is fully sorted.**

**Final sorted array:** \[10, 12, 12, 15, 20, 30, 40]

---

> **Answered Questions:** 1(a), 1(b), 3(a), 3(b), 3(c)
> **Total = 10 + 10 = 20 marks**
