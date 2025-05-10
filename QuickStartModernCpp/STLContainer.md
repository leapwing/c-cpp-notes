# STL Container

### What Are Template Functions?

- Templates allow the definition of **generic functions or classes** that work with **any data type**.
- In C++, the keyword `template` is used, followed by `<typename T>` to declare a **placeholder type** (`T`).
- Often used to avoid **repetition** (DRY principle) when function logic is **identical** except for data type.

### Use Case Example: `print_container`

- Original functions used:

  ```cpp
  void print_container(std::span<int32_t>);
  void print_container(std::span<uint32_t>);
  ```

- Both had identical bodies → duplication.

- Replaced by:

  ```cpp
  template <typename T>
  void print_container(std::span<T>);
  ```

### Benefits

- Eliminates need for function overloads for each type.
- Reusable for different containers (`std::array`, `std::vector`, C-style arrays) and types (`int16_t`, `uint32_t`, etc.).
- Maintains type safety and flexibility.

### Limitations

- Works only for types that support the intended operations (e.g., must be **printable to `std::cout`**).
- For **custom types**, you must define the `operator<<` for console output if printing is needed.

### How to Use

- Call the function with an explicit type, e.g.:

  ```cpp
  print_container<uint32_t>(some_uint_vector);
  ```

- Ensure the container holds the type that matches the template parameter.

### Conclusion

Template functions in C++ are a powerful way to generalize logic across different data types, making your code **cleaner**, **reusable**, and **more maintainable**.

---

### What is `std::vector`?

- A **dynamic array** from the C++ Standard Library.
- Allows **runtime resizing** (unlike `std::array` or C-style arrays).
- Automatically manages **memory allocation and deallocation** — no need for `new`, `delete`, or manual cleanup.

### How to Use `std::vector`

1. **Declaration & Initialization**:

   ```cpp
   std::vector<int> my_vec = {1, 2, 3, 4, 5};
   std::vector<int> empty_vec; // starts empty
   std::vector<int> pre_filled(3, 0); // 3 zeros
   ```

2. **Element Access**:

   - Same as arrays: `vec[i]` (O(1) time).
   - Stored in **contiguous memory** → fast lookup.

3. **Reading Values**:

   - Classic for-loop or:

     ```cpp
     for (const auto& val : my_vec) {
         std::cout << val;
     }
     ```

   - Use reference (`&`) to **avoid copying**, `const` if no modification needed.

4. **Modifying Values**:

   ```cpp
   for (auto& val : my_vec) {
       val *= 2;
   }
   ```

### Adding/Removing Elements

- **Add to end**: `vec.push_back(value)`
- **Remove from end**: `vec.pop_back()`
- Efficient for end operations.

### Inserting in Middle

- Use `insert()` with **iterators**:

  ```cpp
  vec.insert(vec.begin() + index, value);
  ```

- Slower than `push_back()` due to shifting elements.

### Iterators

- `vec.begin()` → pointer to first element.
- `vec.end()` → **past the last** element (do not dereference).
- Used with insert, erase, or manual traversal:

  ```cpp
  for (auto it = vec.begin(); it != vec.end(); ++it)
      std::cout << *it;
  ```

### Benefits

- **No manual memory management** (unlike raw heap arrays).
- Safer, cleaner, and modern C++ approach.
- Most use cases involving dynamic lists can be solved with vectors.

### Best Practices

- Use `std::vector` for dynamic lists unless:
  - Size is known at compile-time → use `std::array`
  - You need key-value pairs → use `std::map`
- Prefer **range-based for-loops** for readability.
- Use `push_back()` and `pop_back()` for best performance.

---

### What is `std::span`?

- A **lightweight, non-owning view** into a contiguous block of memory.
- Introduced in **C++20**.
- Can be used to access data from containers like `std::vector`, `std::array`, or **C-style arrays**.

### Benefits of `std::span`

- **No need for function overloading** based on container types (e.g., vector vs. array).
- Reduces **code duplication**—you only need **one function** that works for all contiguous types.
- Enables **safe, readable** access to memory without copying.

### How `std::span` Works Internally

- Internally stores:

  - A **pointer** to the first element
  - The **size** (number of elements)

- Enables:

  - **Range-based for loops**
  - **Safe iteration**

- Since it doesn’t own the memory, it's cheap to copy and pass around.

### Example Usage

```cpp
#include <span>
#include <vector>
#include <array>
#include <iostream>

void print_span(std::span<int> s) {
    for (int val : s) {
        std::cout << val << "\n";
    }
}

std::vector<int> v = {1, 2, 3};
std::array<int, 3> a = {4, 5, 6};
int c_arr[] = {7, 8, 9};

print_span(v);
print_span(a);
print_span(c_arr);
```

### Comparison to Previous Methods

- **Before `std::span`**:

  - Had to write overloaded functions for different container types (e.g., vector, array).
  - Each overload repeated the same logic.

- **With `std::span`**:

  - One unified function for all contiguous data.
  - Cleaner, DRY (Don't Repeat Yourself) code.

### Summary Takeaway

Use `std::span` when you need a **read-only or read/write view** into a contiguous container without taking ownership or copying data. It's a **modern C++ solution** to unify function interfaces for array-like structures.

---

### Purpose of `std::pair` and `std::tuple`

- Both are containers that can hold multiple values of different types.
- **`std::pair`**: Holds **exactly two values**.
- **`std::tuple`**: Can hold **one or more values** of any types.

### Usage

#### `std::pair`

```cpp
std::pair<int32_t, float> my_pair = {1337, 42.0f};
std::cout << my_pair.first << ", " << my_pair.second;
```

#### `std::tuple`

```cpp
std::tuple<int, float, std::string> my_tuple = {1, 2.5f, "Alice"};
std::cout << std::get<0>(my_tuple);  // Accessing by index
```

### When to Use Structs Instead

- `std::pair` and `std::tuple` lack meaningful **field names**, making code **less readable**.
- For clarity and maintainability, **prefer structs** with named members for most cases.

```cpp
struct User {
  int id;
  float balance;
  std::string name;
};
```

### Use Cases for Pair/Tuple

- Necessary when:

  - Returning multiple values from a function
  - Iterating over STL containers like `std::map` (which returns pairs)

- Example returning a tuple from a function:

```cpp
std::tuple<int, std::string, float> some_function(int input) {
    return std::make_tuple(input, std::to_string(input), static_cast<float>(input));
}
```

### Structured Bindings (C++17)

- Makes accessing tuple or struct elements easier and more readable:

```cpp
auto &[i, s, f] = some_function(42);
std::cout << i << " " << s << " " << f;
```

- Works with:

  - `std::pair`
  - `std::tuple`
  - `struct`
  - `std::array` (and other types supporting `std::get<>`)

### Key Takeaways

- Use `std::pair`/`tuple` for **utility or interoperability** with STL, but:
  - **Prefer `struct`** when field meaning matters.
- Use **structured bindings** to avoid `std::get<>()` calls and write cleaner code.
- Return multiple values from functions using **tuples** or **structs** based on clarity needs.

---

### What is `std::map`?

- A container that stores **key–value pairs**.
- Similar to:

  - **Dictionary** in Python
  - **Hash table** in other languages
- Keys must be **unique** and are automatically **sorted** (via a comparison function).
- Internally implemented as a **Red-Black Tree** (sorted binary search tree).

### How to Use `std::map`

#### Declaration & Initialization:

```cpp
std::map<std::string, int32_t> my_map;
my_map["Jan"] = 33;
my_map["Sam"] = 40;
my_map["Veronica"] = 24;
```

#### Updating Values:

```cpp
my_map["Veronica"] = 25;  // Overwrites existing key
```

#### Checking Existence (C++20+):

```cpp
if (!my_map.contains("Liza")) {
    my_map["Liza"] = 36;
}
```

#### Lookup Using `.find()` (Pre-C++20):

```cpp
auto it = my_map.find("Liza");
if (it != my_map.end()) {
    std::cout << it->first << ": " << it->second;
}
```

### Iterating Over a Map

- Iteration yields a `std::pair<key, value>`.
- Use **structured bindings** for clarity:

```cpp
for (const auto& [key, value] : my_map) {
    std::cout << key << ": " << value << "\n";
}
```

### Notes

- **Order is sorted by key**, not insertion order.
- Keys and values can be **any types**, including custom structs.
- Useful when you need **fast lookups** and **automatic ordering**.

### Key Takeaways

- Use `std::map` for **sorted key-value pairs**.
- Prefer structured bindings for readability.
- Use `.contains()` or `.find()` to check for existence before access or insertion.
- Internally ordered by keys, **not** insertion order.

---

### What Are Alias Types?

- Alias types let you **create a new name** for an existing type.
- Commonly used to simplify complex type names or improve readability.

### Two Ways to Create Alias Types

#### 1. **C-style (`typedef`)**:

```cpp
typedef std::vector<std::uint8_t> ByteVector1;
```

- [V] Works in C and C++
- [X] Not intuitive syntax
- [X] Doesn’t support templates

#### 2. **C++-style (`using`)**:

```cpp
using ByteVector2 = std::vector<std::uint8_t>;
```

- [V] Cleaner and more readable
- [V] Supports templates
- [V] Preferred in modern C++

### Template Alias Example

Create a generic alias for any integer vector:

```cpp
template<typename T>
using VectorOfInts = std::vector<T>;
```

- Enables type reuse without repetition
- Not possible with `typedef`

### Key Benefits of `using`

- Cleaner and more **declarative** syntax (`alias = actual_type`)
- **Template-friendly**
- Fully compatible with modern C++ idioms

### Best Practice

> Use `using` instead of `typedef` in modern C++ for clarity and flexibility.

---

### Iterator Overview in C++ STL

#### Common Containers That Support Iterators:

- `std::array`
- `std::vector`
- `std::map`
- (Also exists: `std::list`, `std::deque`, `std::stack`, `std::queue` — but less commonly used)

---

### Iterator Categories

| Category                | Direction | Mutable? | Usage                     |
| ----------------------- | --------- | -------- | ------------------------- |
| `begin()` / `end()`     | Forward   | Yes      | Default forward iteration |
| `rbegin()` / `rend()`   | Reverse   | Yes      | Reverse iteration         |
| `cbegin()` / `cend()`   | Forward   | **No**   | Read-only forward         |
| `crbegin()` / `crend()` | Reverse   | **No**   | Read-only reverse         |

- `rbegin()` points to the **last element**; `rend()` is **past the first** (non-inclusive).
- `cbegin()` and `crbegin()` give **constant iterators** (read-only access).

### Iterator Utilities (from `<iterator>` header)

- `std::advance(it, n)`
  → Moves `it` forward by `n` steps. Works with all STL containers.

- `std::distance(first, last)`
  → Returns the number of steps between two iterators.

- `std::next(it, n)` / `std::prev(it, n)`
  → Returns a new iterator offset by `n` forward/backward from `it`.

> These are container-agnostic utilities — important for non-random-access containers like `std::list`.

### Key Takeaways

- Use **regular iterators** for reading/writing.
- Use **constant iterators** (`c*`) when you want **read-only access**.
- Use **reverse iterators** (`r*`) to iterate **from end to start**.
- Prefer **iterator utility functions** (`advance`, `distance`) for compatibility across container types.

---

### Inserting Values Between or at the End of Containers in C++ STL

#### Use Case:

You have a container (e.g. `std::vector`) and want to:

- Copy all elements **into another container**, either:

  - At a **specific position**, or
  - At the **end** of that container

### Using `std::copy` (from `<algorithm>`)

#### General Form:

```cpp
std::copy(source.begin(), source.end(), destination_iterator);
```

- **First two args**: range to copy from
- **Third arg**: iterator where to insert in target container

### Insert at a Specific Position:

- Use `container.insert(pos_iterator, begin, end)`:

  ```cpp
  std::copy(src.begin(), src.end(), std::inserter(dst, dst.begin() + 1));
  ```
- Or use `std::next()` to move iterators safely across container types:

  ```cpp
  std::copy(src.begin(), src.end(), std::inserter(dst, std::next(dst.begin())));
  ```

#### Example:

- If `dst = {-1, -2}`, and you insert `{0,1,2,3,4,5}` at position 1, result is:

  ```
  {-1, 0,1,2,3,4,5, -2}
  ```

### Insert at the End: `std::back_inserter`

If container supports `push_back()` (like `std::vector`):

```cpp
std::copy(src.begin(), src.end(), std::back_inserter(dst));
```

- Appends the elements directly to the end of the container.
- Cleaner than manual iterator tracking.

### Requirements:

- Destination container must support either:

  - `.insert()` for position-based insertion
  - `.push_back()` for appending (`back_inserter`)

### Takeaways:

- Use `std::copy()` + iterator helpers to **generalize value insertion**.
- `std::next()` improves portability across STL containers.
- `std::back_inserter()` simplifies appending for dynamic containers.

---

### Exercise: Implementing STL-Like Iterator Functions Manually

This exercise focused on replicating key **iterator utility functions** (`advance`, `distance`, `next`, `prev`) similar to those in the C++ Standard Library, specifically for `std::vector`.

### Structure and Setup

- Implemented inside a **custom namespace** (e.g., `my_std`).
- Uses **type aliases**:

  - `iterator` from `std::vector<T>::iterator`
  - `difference_type` from the container, usually a signed integral type.


### `advance(iterator, n)`

- **Purpose**: Move an iterator forward or backward by `n` positions.
- **Positive `n`**: Increment the iterator in a loop.
- **Negative `n`**: Decrement the iterator in a loop.
- If `n == 0`: No operation.

Example:

```cpp
advance(it, 3);  // Moves the iterator 3 positions forward
```

### `distance(first, last)`

- **Purpose**: Calculate the number of steps between two iterators.
- Loop until `first == last`, incrementing both the iterator and a `counter`.
- **Assumption**: `first` must come **before** `last`.

Example:

```cpp
distance(begin, end);  // Returns the number of elements between them
```

### `next(iterator, n)`

- **Purpose**: Returns a new iterator `n` steps ahead.
- **Internally uses**: `advance(it, n)`
- Non-mutating (doesn’t modify original iterator).

### `prev(iterator, n)`

- **Purpose**: Returns a new iterator `n` steps behind.
- **Internally uses**: `advance(it, -n)`

### Key Concepts

- Custom implementations mimic STL behavior.
- **`advance`** is the most generic, reusable across `next` and `prev`.
- Assumes valid inputs (e.g., doesn’t check for out-of-range moves).
- This builds understanding of how STL iterator utilities work internally.

---
