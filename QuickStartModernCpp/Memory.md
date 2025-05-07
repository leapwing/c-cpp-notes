# Memory

## C++ Arrays and Why You Should Prefer `std::array` Over C-Style Arrays

### C-Style Arrays
- Defined like: `uint32_t my_array[5];`
- Key constraints:
  - Size must be known at **compile time**
  - Type must be **fixed**
- Initialization options:
  - `{}` (zero-initialize)
  - `{1, 2, 3, 4, 5}` (manual init)
- Limitations:
  - When passed to a function, it decays into a **pointer** (loses size info)
  - Must pass size as a separate argument to avoid out-of-bounds issues
  - **Unsafe and error-prone**, especially when accessing beyond bounds

### C++ `std::array` (From `<array>` Header)
- Syntax: `std::array<uint32_t, N> arr;`
- Size is fixed at compile time, just like C-style arrays
- **Advantages**:
  - Retains size info via `.size()` method
  - Safer: supports range-checked access via `.at()`
  - Can be passed **by const reference** to avoid decay
- Allows usage of modern features like:
  - `auto` type deduction
  - Range-based for-loops

### Function Design for Arrays
- **C-style array**: must take both a pointer and a size
Example:

```cpp
void print_array(const uint32_t* arr, uint32_t length);
```

- `std::array`: can specify size in template

Example:

```cpp
template <uint32_t N>
void print_array(const std::array<uint32_t, N>& arr);
```

- Function works for any fixed-size array (N is deduced at compile time)
- Compiler **generates one version per unique size**

### Example Initialization

```cpp
std::array<uint32_t, 5> arr1 = {1, 2, 3, 4, 5};
std::array<uint32_t, 5> arr2{};  // Zero-initialized
```

### What You Should Use

✅ Use `std::array<T, N>` for fixed-size arrays in modern C++  
🚫 Avoid raw C-style arrays except for low-level interfacing (e.g., with C code)

---

## Strings in C and Modern C++

### C-Style Strings (Character Arrays)
- Defined as arrays of `char` with a null terminator (`'\0'`) at the end
- Can use:

```cpp
char my_text[] = "Tom";  // Automatically adds '\0', size = 4
```

- Alternative (manual):

```cpp
char my_text[] = {'T', 'o', 'm', '\0'};
```

- Limitations:
  - Fixed size (must be known at compile time)
  - Manual handling of null terminator (error-prone)
  - No built-in string manipulation functions (must use `cstring` functions)

2. C++ `std::array<char, N>`
- Compile-time fixed-size array of characters
- Slightly safer but still requires manual null terminator handling

```cpp
std::array<char, 4> my_text = {'T', 'o', 'm', '\0'};
```

- Rarely used for actual string handling

3. C++ `std::string` **(C++ Standard Library)**
- Dynamically-sized string class (`#include <string>`)
- Automatically manages memory, resizing, and null terminator

```cpp
std::string name = "Tom";
name += " Smith";
std::cout << name.size();  // Get length
```

- Recommended for almost all string handling in modern C++
- Provides powerful utilities: `.append()`, `.substr()`, `.find()`, etc.

### String Literals
- Hardcoded strings in quotes: `"Hello"`
- Can be passed as `const char*` or used to initialize `std::string`
- Safe and commonly used in modern code (e.g., for logging, error messages)

### Summary Table

| Approach	| Resizable	| Null-Terminator	| Modern Usage |    
| ---	| ---	| ---	| ---	|
| `char[]` (C-style)	| ❌	| Manual/Implicit	| ⚠️ Legacy use
| `std::array<char, N>`	| ❌	| Manual	| ❌ Rare
| `std::string`	| ✅	| Automatic	| ✅ Preferred
| String literal (`"abc"`)	| ❌	| Implicit	| ✅ Safe for constants

---

## Memory Management with Function Parameters in C++

### Problem with Copies
- When you pass a variable (like an array) to a function in C++, it’s **copied by default**.
- This can waste memory, especially if the data structure is large (e.g. a 1MB array).
- Example: Passing an array to `print_array_values` results in a full copy.

### Solution: Use References
- Use a reference (add `&` after the type) to avoid copying:

```cpp
void print_array_values(const std::array<int, 5>& arr) { ... }
```

- This keeps memory usage efficient by passing a reference to the original data.
- You can verify this by printing the **memory address** inside and outside the function. If it’s the same, no copy was made.

### When to Use Copies vs. References
- For **small types** (e.g. `int32_t`), **copying is faster** than using references, since references still require pointer handling (typically 8 bytes).
- For **large objects or containers**, prefer references to avoid unnecessary copying.

### Modifying Values: Input/Output Parameters

- Use Case: Modify Array in Function
- When modifying the array inside the function (e.g. doubling values), you must:
  - **Remove** `const` (because you want to change the values)
  - **Use a reference** to ensure changes are reflected outside the function

### Rule of Thumb
- Input only → use `const &` (if data is large)
- Input/output → use non-const reference `&`
- Output only → also use non-const reference `&`

### Key Takeaways
- ❌ Avoid unnecessary copies (especially for large data).
- ✅ Use references (`&`) to avoid memory duplication.
- ✅ Use `const` to indicate the function does not modify the input.
- ✅ Don’t use `const` if the function **modifies** the input (output or in/out).

---

## Pointers and References in C++

### References Recap
- Use **references** (with `&`) to avoid making unnecessary **copies** when passing large objects (like arrays) to functions.
- For **small data types** (like `int` or `float`), copying is cheaper than using references.
- For **input-output parameters**, use a **non-const reference**.
- For **read-only input**, use a `const` **reference**.

### Pointers Overview
- A **pointer** is a variable that stores a **memory address** of another variable.
- Use the `*` **symbol** to declare a pointer:

```cpp
uint32_t* ptr = &my_age;
```

- Use the `&` symbol to get the memory address of a variable.
- Use `*ptr` (dereferencing) to access or modify the value the pointer points to.

### Pointer vs Reference

| Feature	| Reference	| Pointer |
| ---	| ---	| --- |
| Fixed after init?	| Yes	| No (can change target) |
| Syntax	| `Type&` | `Type*` |
| Null value allowed?	| No	| Yes |
| Use case	| Most general use	| Special cases only |

> Guideline:  
> ✅ Use **references** when possible.  
> 🛠 Use **pointers only if you have to** (e.g., dynamic memory, nullable data, indirection).  

### Why Use Pointers?
- Allows **indirect modification** of values.
- Supports **dynamic memory management** and **flexible referencing**.
- Enables **changing the reference target** during program execution.

### Practical Example

```cpp
uint32_t my_age = 30;
uint32_t* ptr = &my_age;
*ptr = 31; // This modifies `my_age` indirectly
```

---

## Pointers and Heap Memory in Modern C++

### Why Use Pointers at All?
- Needed when allocating memory dynamically on the **heap**.
- Useful when:
  - The **size** of data (like arrays) is **not known at compile time**.
  - Manual memory management is necessary (though **not common in modern C++**).

### Memory Segments in a C++ Program
- **Code**: stores binary instructions.
- **Static**: stores static/global variables.
- **Stack**: fast, structured, for fixed-size variables. Requires sizes known at compile time.
- **Heap**: flexible, dynamic memory. Requires manual management via `new` and `delete`.

### Memory Segments in a C++ Program
- Code: stores binary instructions.
- Static: stores static/global variables.
- Stack: fast, structured, for fixed-size variables. Requires sizes known at compile time.
- Heap: flexible, dynamic memory. Requires manual management via new and delete.

### Heap Allocation with Pointers
- Use `new` to allocate memory on the heap:

```cpp
uint32_t* heap_array = new uint32_t[length];
```

- The pointer stores the **address** of the first element.
- Use `*` to **dereference** and access data.
- After usage, you **must free** the memory:

```cpp
delete[] heap_array;
heap_array = nullptr;
```

### Risks and Best Practices
- If you forget `delete`, you get **memory leaks**.
- Always set pointer to `nullptr` after deletion to avoid **dangling pointers**.
- Before using a pointer, always **check** if it’s `nullptr`.

### Practical Use Case
- Dynamically allocate an array based on user input size:

```cpp
size_t length;
std::cin >> length;
uint32_t* heap_array = new uint32_t[length];
// use it...
delete[] heap_array;
heap_array = nullptr;
```

### Modern C++ Alternative: Smart Pointers
- Manual `new`/`delete` is discouraged in modern C++.
- Use **smart pointers** (like `std::unique_ptr` or `std::shared_ptr`) for safer, automatic memory management.

---

## Pointers vs References Recap

| Feature	| Pointer	| Reference |
| ---	| ---	| --- |
| Nullability	| Can be `nullptr` | Must reference something |
| Reassignable	| Can point elsewhere | Cannot be reseated |
| Memory usage	| Needed for heap allocation | Stack-based usage mostly |
| Use case	| Dynamic allocation, optional | Function parameters, safe |

---

## L-values, R-values, and how they apply to function parameters in C++

### What Are L-values and R-values?
- L-value:
  - Stands for **"Left value"** — something that has a name and a memory address.
  - Can appear on the **left side** of an assignment (e.g., `x = 2`; → `x` is an L-value).
- R-value:
  - Stands for **"Right value"** — temporary or unnamed values like `2`, `x + y`, or function return values.
  - Can only appear on the **right side** of an assignment (e.g., `x = 2`; → `2` is an R-value).
- **Does not have a memory address**.

---

### Function Parameter Behavior

There are **4 common ways** to pass arguments to functions in C++:

| Function Signature	| Accepts L-values	| Accepts R-values	| Use Case |
| ---	| ---	| ---	| --- |
| `void f(int v)`	| ✅	| ✅	| Pass by **value** |
| `void f(const int v)`	| ✅	| ✅	| Pass by **value (readonly)** |
| `void f(int& v)`	| ✅	| ❌	| **In-Out** reference (modifies input) |
| `void f(const int& v)`	| ✅	| ✅	| **Readonly reference** |

### Why Can’t You Pass R-values to Non-const References?

- **Non-const reference** (`int&`) implies the function may **modify the argument**.
- But **R-values like `2` are temporary**, they **cannot be modified**, and **do not exist beyond the current line**.
- Hence, this is **forbidden** by the compiler:

```cpp
void f(int& x);  // Can't call with f(2); ❌
```

---

### Const References Enable R-value Support

- You **can bind** an R-value to a `const int&` because:
  - A **temporary** is created behind the scenes.
  - It’s **safe** because the function promises **not to modify** the input.

---

### Why Copies Always Work

- When you **pass by value**, a **copy is made**.
- Whether the argument is an L-value or R-value doesn’t matter, because both can be copied.

---

### Key Takeaways

- **Use** `const int&` to safely accept both L-values and R-values without copying.
- **Don’t use** `int&` with R-values — they can’t bind.  
- **Use value** (`int`) or `const int&` if you’re only reading.
- **Use** `int& `only when you need to modify the input (in-out behavior).
- R-values are temporary, read-only expressions. L-values are named and addressable.

---
