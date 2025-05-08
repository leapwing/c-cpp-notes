# Big O

## Introduction to Big O

1. **Importance of Big O**:
- Big O is a **core topic** in data structures and algorithms.
- It’s **frequently asked about in coding interviews**.

2. **What is Big O**?:
- A **mathematical way to compare code** based on performance (not readability or conciseness).
- Focuses on **how efficiently code runs** in terms of operations, not actual execution time.

3. **Time Complexity**:
- Measures how the number of operations grows as input size increases.
- Not based on actual run time since hardware speed can vary.
- Example: Code that takes 15 seconds vs. code that takes 60 seconds shows a performance difference in operations.

4. **Space Complexity**:
- Measures how much memory a program uses.
- Important in cases where **memory usage matters more than speed**.

5. **Trade-offs and Use Cases**:
- Sometimes faster code uses more memory and vice versa.
- Understanding when to **prioritize time vs. space complexity** is key, especially in interviews.

6. **Course Focus**:
- Primarily on **time complexity**, with some mention of space complexity where relevant.

---

## Big O, Omega, and Theta Notation

1. **Three Key Notations in algorithm complexity**:
  - **Ω (Omega)** → Best case scenario.
  - **Θ (Theta)** → Average case scenario.
  - **O (Big O / Omicron)** → Worst case scenario.

2. **Example with an Array**:
- Searching for a value in an array:
  - **Best case (Ω):** The target is the first element (e.g., number 1).
  - **Worst case (O):** The target is the last element (e.g., number 7).
  - **Average case (Θ):** The target is somewhere in the middle (e.g., number 4).

3. **Common Misuse in Language**:
- People often say "average Big O" or "best-case Big O", but:
  - **Technically**, Big O always refers to the **worst-case** performance.
  - Use **Theta(Θ)** for average case, and **Omega(Ω)** for best case.

---

## Understanding Big O of N (O(n))

1. **Introduction to O(n)**:
- O(n) is not the fastest or slowest, but it’s **the easiest to understand**, so it’s introduced first.

2. **Example Function – `printItems(n)`**:
- A simple function with a **for loop that runs `n` times**.
- Each iteration prints the loop index.
- If you pass `10`, it prints **10 items** (0 to 9).

3. **Why It’s O(n)**:
- The number of operations **grows linearly** with the input size `n`.
- This makes it a classic case of **linear time complexity**.

4. **Visual Representation**:
- On a graph:
  - **X-axis**: Input size (`n`)
  - **Y-axis**: Number of operations
- O(n) plots as a **straight line**, meaning the operations grow proportionally with input.

---

## Big O Simplification Rule – Drop Constants

1. **Rule Introduced**:
- The first rule of simplifying Big O expressions is **"drop constants"**.

2. **Example Function**:
- A function with **two separate for loops**, each running n times.
- Total operations: `n + n = 2n`.

3. **Simplified Big O**:
- Instead of writing **O(2n)**, we simplify it to **O(n)**.
- This applies even if the coefficient is larger, like 100n—it’s still simplified to **O(n)**.

4. **Why Drop Constants?**:
- Big O measures **growth rate categories**, not exact performance.
- Linear growth (like 2n or 100n) behaves fundamentally different from exponential growth as `n` increases.
- The simplification helps focus on the **type of growth**, not the precise number of operations.

5. **Key Takeaway**:
- No matter how many times a linear operation is repeated, it’s still **linear**—so constants are dropped in Big O notation.

---

## Understanding O(n²) – Quadratic Time Complexity

1. **Introduction to O(n²)**:
- Arises when you have **nested loops**, where one loop runs inside another.

2. **Example Function**:
- A function with a **for loop nested inside another** (both run `n` times).
- When run with `n = 10`, it prints **100 lines**—demonstrating `n × n = n²` operations.

3. **Big O Classification**:
- This is **O(n²)** because the number of operations grows **quadratically** with the input size n.

4. **Graph Comparison**:
- O(n²) grows **much faster** than O(n).
- It’s significantly **less efficient**, especially as `n` increases.

5. **Efficiency Implication**:
- When comparing two solutions—one with O(n) and one with O(n²)—**O(n) is always the better choice** if both solve the problem correctly.

6. **Course Note**:
- O(n²) is the **least efficient** Big O notation covered in this course.

---

## Big O Simplification – Drop Non-Dominant Terms

1. **Simplification Rule Introduced**:
  - The second rule of Big O simplification is **"drop non-dominant terms."**

2. **Example Function**:
  - Combines:
    - **Nested for loops** (O(n²))
    - Followed by a **single for loop** (O(n))
  - When run with `n = 10`, it prints:
    - 100 lines from the nested loops (0,0 to 9,9)
    - 10 lines from the single loop (0 to 9)

3. **Total Complexity**:
- Mathematically: **O(n² + n)**
- **O(n²)** is the dominant term; **O(n)** becomes insignificant as `n` increases.

4. **Why Drop Non-Dominant Terms**:
- For large input sizes, the lower-order term (O(n)) contributes **negligibly** to total growth.
- Simplified Big O: **O(n²)**

5. **Key Takeaway**:
- When multiple complexities are added, **only the term with the highest growth rate is kept**.

---

## Understanding O(1) – Constant Time Complexity

1. **Introduction to O(1)**:
  - Represents operations where the **execution time does not grow** with the input size `n`.

2. **Example Function – `addItems(n)`**:
  - Performs a simple addition: `n + n`.
  - Whether n is 1 or 1,000,000, it still performs **one operation**.

3. **Simplification Reminder**:
  - Even if you add `n + n + n` (which is technically two operations), it’s still considered **O(1)** because **constants are dropped** in Big O notation.

4. **Graph Representation**:
  - Appears as a **flat horizontal line** on a graph.
  - Indicates **no growth** in operations as input increases.

5. **Efficiency Insight**:
  - **Most efficient** Big O class.
  - Ideal whenever a task can be completed in **constant time**, regardless of input size.

---

## Understanding O(logn) – Logarithmic Time Complexity

1. **What Is O(logn)?**
  - Time complexity where the **number of operations grows slowly** as input size increases.
  - Commonly seen in **binary search** and divide-and-conquer algorithms.

2. **Binary Search Demonstration**:
  - Given a **sorted list** of 8 elements, the algorithm:
    - Cuts the list in half repeatedly to find a target number.
    - Example: Finding the number `1` took 3 steps.
    - Since 2^3 = 8, this is log base 2 of 8 = 3.

3. **General Rule**:
  - O(logn) = The number of times you must **halve a list** to reduce it to a single item.
  - Example: For a list of 1 billion items, binary search would take only **31 steps**.

4. **Graph Representation**:
  - Appears as a **slowly rising curve**—much flatter than O(n) and O(n²), but slightly above O(1).
  - Indicates **high efficiency**, especially for large datasets.

5. **Bonus Concept – O(nlogn)**:
  - A time complexity used in **efficient sorting algorithms** like **merge sort** and **quicksort**.
  - O(nlogn) is the **best general-case efficiency** possible for comparison-based sorting.

6. **Key Takeaways**:
  - O(logn) is highly efficient and important in real-world algorithms.
  - Along with O(1), O(n), and O(n²), it’s one of the **four most common Big O types** seen in the course.

## Big O with Multiple Input Terms (Different Variables)

1. **Single vs. Multiple Inputs**:
  - Previously, a function with two for loops running `n` times was simplified from **O(2n)** to **O(n)**.
  - That worked because both loops depended on **the same input `n`**.

2. **New Example with Two Inputs (`a` and `b`)**:
  - A function now has:
    - One loop that runs `a` times → **O(a)**
    - Another loop that runs `b` times → **O(b)**

3. **Common Mistake**:
  - Incorrectly assuming both inputs are the same (`a = n`, `b = n`) and simplifying to **O(n)**.
  - This is not valid when `a` and `b` represent **different input sizes**.

4. **Correct Simplification**:
  - When the loops are sequential: **O(a + b)**
  - When one loop is nested inside the other: **O(a × b)**

5. **Key Takeaway**:
  - When a function receives multiple independent inputs, their complexities must be treated **separately**.
  - You **cannot combine or simplify** them as if they’re the same variable.

## Big O of Vectors (and Arrays)

1. **General Concept**:
  - Most operations on vectors also apply to **arrays**.
  - The variable `n` refers to the **number of items** in the vector or array.

2. **Efficient (O(1)) Operations**:
  - **Adding to the end** (`push_back`)
  - **Removing from the end** (`pop_back`)
  - **Accessing by index** (e.g., `vec[3]`)
  - ✅ These are all **O(1)** because no reindexing or shifting is required.

3. **Inefficient (O(n)) Operations**:
  - **Inserting or removing from the beginning** (`insert` or `erase` at `begin()`):
    - Requires **shifting all subsequent elements** to update their positions.
  - **Inserting or removing in the middle**:
    - Also requires **shifting elements**, so it’s still **O(n)**.
    - Even though it may seem like "half of `n`", constants like ½ are dropped in Big O.
  - **Searching by value**:
    - Requires checking each element → **O(n)** (linear search).

4. **Key Takeaways**:
  - ✅ **O(1)**: `push_back`, `pop_back`, access by index.
  - ❌ **O(n)**: `insert`, `erase` (except at the end), search by value.
  - Efficiency depends on **where the operation occurs** in the vector.

## Big O Wrap-Up

### Comparing Growth Rates (n = 100 and 1000):  
- **When n = 100**:
  - O(1) → 1 operation
  - O(log n) → ~7 operations
  - O(n) → 100 operations
  - O(n²) → 10,000 operations
- **When n = 1000**:
  - O(1) → 1 operation (unchanged)
  - O(log n) → ~10 operations
  - O(n) → 1000 operations
  - O(n²) → 1,000,000 operations

> **Key Insight**: As `n` grows, **O(n²)** becomes dramatically less efficient than the others.

### Common Big O Patterns:

| Big O | Description | Common Pattern |
| --- | --- | --- |
| O(1) | Constant time | Direct lookup, simple ops |
| O(log n) | Logarithmic time | Divide and conquer, binary search |
| O(n) | Linear time | Iteration over input |
| O(n²) | Quadratic time | Nested loops |
| O(n log n) | Efficient sorting | Merge sort, quicksort |


### Big O Cheat Sheet Overview:
- Found at [Big-O Cheat Sheet](https://bigocheatsheet.com/).  
- Includes:  
  - Time and space complexities for **common data structures** (arrays, linked lists, trees, etc.).  
  - Sorting algorithm complexities (best/average/worst cases).  
  - **Most space complexities** are **O(n)** (except special cases like skip lists).  
  - Sorting space complexities vary:  
    - Merge sort: **O(n)** space  
    - Bubble/insertion/selection sort: **O(1)** space (in-place)  

### Sorting Algorithm Trade-Offs:  
- **Quicksort**: Fast on average but **can be bad with already sorted data**.  
- **Bubble/Insertion/Selection Sort**:  
  - Inefficient on average (**O(n²)**), but **very efficient when data is already/almost sorted**.  
  - Space-efficient: **O(1)**  

---

### Interview Relevance:  
- Expect questions on:  
  - **Time complexity** in all forms: **Big O (worst), Theta (average), Omega (best)**.  
  - **Space complexity** in sorting or memory-sensitive applications.  
- Be ready to explain **when and why** one algorithm performs better than another.  
