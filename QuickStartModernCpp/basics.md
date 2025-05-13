# Basics

## Primitive Numeric Types in C++

### File Setup

- Uses a prepared `start.cc` file as a base.
- Acceptable C++ file extensions: `.cc`, `.cp`, `.cpp` (rare: `.cxx`).
- Preferred in course: `.cc` or `.cp`.

### Integer Types (from `<cstdint>` / `cstddint`)

- C++ provides **precise-width integer types** via `std::intXX_t` and `std::uintXX_t`:

  - **Signed**: `int8_t`, `int16_t`, `int32_t`, `int64_t`
  - **Unsigned**: `uint8_t`, `uint16_t`, `uint32_t`, `uint64_t`

- Example:

```cpp
std::int32_t i1 = 0;
std::uint64_t u1 = 100U;  // U suffix optional
```

- Suffixes for clarity (optional):
  - `U` or `u` → unsigned
  - `LL`, `ULL` → long long or unsigned long long

### Floating Point Types

- C++ offers two basic floating-point types:

  - `float` → 32-bit (use suffix `F` or `f` for literal)
  - `double` → 64-bit (default for floating-point literals)

- Example:

```cpp
float f1 = 12.0F;   // F forces float, otherwise it's interpreted as double
double d1 = 12.0;
```

### Character Type

- `char` is an **8-bit type** used for single characters.
  - Stored as an integer under the hood (ASCII encoding).
- Use **single quotes** for characters:

```cpp
char c1 = 'A';
char c2 = 'z';
```

### Boolean Type

- C++ also includes the `bool` type (`true` / `false`)

### Why Use `<cstdint>` Types?

- Types like `int`, `long` are **platform-dependent** (e.g., 16-bit on Arduino, 32-bit on Windows).
- **Modern C++ best practice**: use `<cstdint>` types to ensure **portability and precision**.

---

## Booleans, If Statements & Ternary Operator in C++

### Boolean Basics

- C++ `bool` type stores `true` or `false` (lowercase keywords).
- Booleans can be used in conditions and logical expressions:

```cpp
bool b1 = true;
bool b2 = false;
bool result = b1 || b2 && b1;
```

### If Statements

- Syntax:

```cpp
if (condition) {
    // code block
} else if (other_condition) {
    // code block
} else {
    // code block
}
```

- **Curly braces** `{}` define code blocks.
  - Optional for **single-line blocks**, but recommended for clarity.
- Best practice: **Use parentheses generously** in conditions for readability.

### Console Output

- Use `std::cout` to print to console:

```cpp
std::cout << "Printed!\n";
```

### Compiler Warnings

- Unused variables may trigger warnings (e.g., from Clang or GCC).
- Helps improve code efficiency and clarity.

### Ternary Operator (Conditional Expression)

- A concise alternative to if/else for **assigning values based on a condition**:

```cpp
int result = b1 ? 10 : -10;
```

- Before `?`: the condition
- Between `?` and `:`: value if true
- After `:`: value if false

- Best used for **simple value assignments**.
- Avoid using nested ternary expressions—they reduce readability.

### Key Takeaways

- Use ternary for short, clean condition-based assignments.
- Prefer regular `if/else` for more complex logic or multiple conditions.
- Always aim for clarity—even if the code is technically shorter.

---

## Loops in C++ (For, While, Do-While & Increment Operators)

### For Loop Basics

- Common format:

```cpp
for (uint32_t i = 0; i < limit; ++i) {
    std::cout << i << "\n";
}
```

- Preferred to use **unsigned integers** (like `uint32_t`) since ranges are usually non-negative.
- Loop variables often start with names like `i`, `j`, `k`, etc.
- You can loop in reverse using decrement operators (`--i`).

### Pre-Increment vs. Post-Increment

- **Pre-increment (`++i`)**: increments before using the value.
- **Post-increment (`i++`)**: increments after using the value.
  - Can lead to unexpected results in assignments:

```cpp
int a = 1;
int result = a++;  // result = 1, a becomes 2 afterward
```

- **Best practice**: Use **pre-increment** (`++i`) and **pre-decrement** (`--i`) in nearly all cases.

### While Loops

- Standard `while` loop:

```cpp
while (condition) {
    // loop body
}
```

- Can be used with a `break` statement to exit early:

```cpp
if (i > 3) break;
```

### Do-While Loops

- Ensures the loop runs **at least once**, unlike a `while` loop.

```cpp
do {
    // code block
} while (condition);
```

- Condition is checked **after** the first iteration.

### Special Note

- The **most important "gotcha"** in C++ loops is the **difference between post- and pre-increment**.
- Use `++i` and `--i` unless you have a clear reason not to.

---

## Functions in C++

### Declaration vs. Definition

- **Declaration**: Specifies function **signature** (return type, name, parameters); no body.

```cpp
void my_print_function();  // Declaration
```

- **Definition**: Contains both **signature and function body**.

```cpp
void my_print_function() {
    std::cout << "Hello World\n";
}
```

- If the definition is **above the first function call**, a separate declaration isn’t needed.
- If the function is called **before its definition**, it **must be declared first**.

### Calling Functions

- Functions must be **declared or defined** before they are used.

Example:

```cpp
my_print_function();  // Function call
```

### Function Parameters

- Use type + name inside parentheses:

```cpp
void print_number(std::uint32_t num) {
    std::cout << num << "\n";
}
```

### Function Overloading

- C++ allows **multiple functions with the same name** but **different parameter types or counts**.

```cpp
void print_number(std::uint32_t num) { ... }
void print_number(double num) { ... }  // Overloaded
```

- The compiler selects the **best match** at compile time based on the argument type(s).
- Avoids need for differently named functions for similar behavior.

### Overload vs. Templates

- While overloading works, it’s **repetitive** when function bodies are the same.
- C++ provides **template functions** to avoid duplicating logic for different types.

### Best Practices

- Use **snake_case** for function names.
- Prefer **declaration + definition** separation for larger programs or headers.
- Use **overloads sparingly**; prefer **templates** when logic is identical.

---

## Organizing C++ Code into Multiple Files (Header + Source)

### Project Structure

- A subfolder (e.g. `loop/`) is created to organize related code.
- Inside the folder:
  - `lib.cc`: **Source file** with function **definitions**
  - `lib.h`: **Header file** (interface) with function **declarations**

### Basic Separation: Declaration vs. Definition

- Header (`lib.h`):
  - Contains function **declaration** (signature only)
  - Add required `#include` directives (e.g. `<cstdint>` for `int32_t`)
  - Use #pragma once at the top to avoid **multiple inclusion errors**

```cpp
#pragma once
#include <cstdint>

void print_number(std::int32_t num);
```

### Source (lib.cc):

- Contains function **definition** (actual implementation)
- Include its own header: `#include "lib.h"`
- Also include any required standard headers (e.g. `<iostream>` for `std::cout`)

```cpp
#include "lib.h"
#include <iostream>

void print_number(std::int32_t num) {
    std::cout << num << "\n";
}
```

### Main File Usage

- `main.cc` (or `main.cpp`) should include only the header:

```cpp
#include "lib.h"

int main() {
    print_number(42);
    return 0;
}
```

### Building Multi-File Projects

- Compile **all `.cc` files** together (e.g. `main.cc`, `lib.cc`) as part of one translation unit.

- In VS Code:
  - Choose the folder (`loop/`) in the bottom bar.
  - Use gear icon to build all files.
  - Run to see output (e.g., prints `42`).

### Include Guards

- Prevent **duplicate function definitions** caused by multiple includes.
- Use `#pragma once` (preferred modern method) at the **top of every header file**.
  - Ensures the file is only included once during compilation.

### Key Takeaways

- Separate reusable logic into `.h` (declaration) and `.cc` (definition).
- Always include the header in its own `.cc` file.
- Use `#pragma once` in headers to prevent duplicate includes.
- This structure supports clean, maintainable, and scalable C++ code.

---

## Debugging in C++ with VS Code

### Basic Debugging Workflow

- **Breakpoints**:

  - Set by clicking left of the line number (red dot).
  - The debugger will **pause execution** at the breakpoint.

- **Starting Debugger**:

  - Click the debug icon in the blue status bar to launch the debugger.

- **Debug Controls**:
  - Step over (→): Execute the current line.
  - Step into (↓): Enter a called function.
  - Step out (↑): Exit the current function.
  - Continue (▶): Run until next breakpoint.
  - Stop (■): End debugging session.
  - Restart (↻): Start a new debug session.

### Inspecting State

- **Local variables** appear in the left panel during a debug session.
- **Debug Console**:
  - Type variable names to see current values.
  - Perform calculations (e.g., value + 2) to inspect expressions.

### Important Notes

- **Recompile when code changes** to ensure debug info is up to date.
- **Step into a function** to see its parameters and local variables.

### Debug vs. Release Builds

- **Debug Build**:

  - Includes full debug information.
  - Breakpoints and variable inspection are fully supported.
  - Code is not optimized for performance.

- **Release Build**:
  - Compiler applies **optimizations** to improve performance.
  - Debugging is **limited or not possible**.
  - Intended for production builds.

### Bonus Tool – Compiler Explorer (godbolt.org)

- Lets you view the **generated assembly code** from your C++ source.
- Compare different optimization levels:
  - -O0 (debug): More verbose, unoptimized code.
  - -O3 (release): Fewer lines, optimized and faster.
- Helpful to understand how the compiler "translates" your logic under the hood.

### Key Takeaways

- Use **Debug build** while developing and testing.
- Switch to **Release build** only when optimizing final performance.
- Tools like **Compiler Explorer** give visibility into how your code behaves after compilation.

---

## Enums and `enum` class in Modern C++

### What Is an Enum?

- An **enum (enumeration)** defines a finite set of named constant values.
- Commonly used for representing **categorical states** (e.g., connection status or user roles).

### C-style `enum` vs. Modern C++ `enum class`

| Feature         | C-style `enum`         | Modern `enum class`          |
| --------------- | ---------------------- | ---------------------------- |
| Scope           | Global                 | Scoped (namespaced)          |
| Name collisions | Possible               | Avoided due to scoping       |
| Type safety     | None                   | Enforced                     |
| Syntax          | `enum Status { ... };` | `enum class Status { ... };` |

### Example:

```cpp
enum class Status { Unknown, Connected, Disconnected };
```

- You must reference values with their scope:

```cpp
Status s1 = Status::Connected;
```

### Avoiding Name Conflicts

- C-style enums inject each enumerator into the global namespace.
- This causes **redefinition errors** if multiple enums share the same name (e.g., `Unknown` in both `Status` and `UserPermission`).
- enum class prevents this via **scoped enumerators**:

```cpp
enum class UserPermission { Unknown, Admin, Regular };
```

### Using Enums in Control Flow

- `switch` statements are used to evaluate enum variables:

```cpp
switch (status) {
    case Status::Unknown:
        std::cout << "Unknown\n";
        break;
    case Status::Connected:
        // logic
        break;
    case Status::Disconnected:
        // logic
        break;
}
```

- Each case must end with break to avoid fallthrough.
- For checking a single condition, prefer an if statement.

### Best Practices

- Prefer `enum class` over C-style `enum` for **type safety, scoping, and clarity**.
- Always use scoped names (`EnumType::Value`) with `enum class`.
- Use `switch` when handling multiple enumerator cases; use `if` for singular condition checks.

---

## Structs in Modern C++

### What Is a Struct?

- A `struct` is a **user-defined data type** used to group related variables.
- In modern C++, `struct` and `class` are **almost identical**.
- The **main difference** is: `struct` members are public by default, whereas `class` members are private.

### When to Use a Struct

- Use a `struct` when:
  - You’re only storing **plain data** (no member functions).
  - You want a **simple container** for values.
- Also referred to as **POD** (Plain Old Data).

### Defining a Struct

```cpp
struct User {
    Status status;        // an enum class
    std::uint64_t id;     // 64-bit user ID
}; // semicolon required!
```

### Creating and Initializing Struct Instances

1. Basic Initialization Using Braces:

```cpp
User user1 = { Status::Connected, 42 };
```

2. C++20 Designated Initializers (More Readable):

```cpp
User user2 = {
    .status = Status::Connected,
    .id = 42
};
```

### Accessing Struct Members

```cpp
std::cout << user1.id << "\n";
```

### Printing Enum Members

- `enum class` values can’t be printed directly (they aren’t implicitly cast to int).
- Use `static_cast` to cast to `int`:

```cpp
std::cout << static_cast<std::int32_t>(user1.status) << "\n";
```

- **Avoid** the old C-style cast:

```cpp
// C-style cast (not recommended)
std::cout << (std::int32_t)user1.status;
```

### Commenting in C++

- Single line: `// comment`
- Multi-line:

```cpp
/*
   Multiple lines
   of comment
*/
```

### Key Takeaways

- Use `struct` for grouping simple data fields.
- Prefer **designated initializers** (C++20) for clarity.
- Use `static_cast<>` for safe and modern type conversions.
- Avoid using `struct` for logic-heavy types—use `class` instead.

---

## auto, Implicit Conversion, and Uniform Initialization in C++

### What is `auto` in C++?

- The `auto` keyword allows the compiler to **deduce the type of a variable** from the right-hand side of an assignment.
- Type deduction is done at compile time, so C++ remains a **statically typed language**.

### Why Use `auto`?

- **Avoid repetition** of type names (follows the DRY principle).
- **Improves readability** when types are long or obvious.
- Reduces clutter in generic or templated code.

### Potential Pitfalls of auto

- Can **preserve unintended implicit conversions**, leading to **data loss**:

```cpp
float a = 10.5f;
std::int32_t b = a;       // Implicit conversion — may lose decimal part
auto c = a;               // Type deduced as float
```

- In some cases, using `auto` can **obscure intent**, especially in code reviews where hover information isn’t available.

### Avoiding Implicit Conversion: Use `static_cast`

- Makes type conversions **explicit and clear**:

```cpp
auto c = static_cast<std::int32_t>(a);
```

### Uniform Initialization (C++11+)

- Prevents **narrowing conversions** by using brace syntax:

```cpp
auto d = std::int32_t{a};  // Fails or warns if data loss would occur
```

- Works consistently across all data types — hence "uniform".
- Safer and preferred over implicit or old-style initialization.

### Benefits of Uniform Initialization

- **Type safety**: narrowing conversions are caught at compile time.
- **Default initialization**: when no value is passed, default is used (e.g., 0 for integers).
- **Cleaner syntax**: especially with `auto` and custom types.

### Common Best Practices

- ✔ Use `auto` when:
  - The type is **obvious** from the assignment.
  - The type is **repeated or verbose**.
- ❌ Avoid `auto` when:
  - It hides conversions or **leads to confusion** about type.
  - The right-hand side is ambiguous or potentially lossy.
- Use `static_cast` to make conversions explicit.
- Use **uniform initialization** to avoid accidental narrowing or undefined behavior.

### Tooling Consideration

- In editors like **VS Code**, you can hover over variables to see deduced types.
- In plain text environments (e.g., GitHub code reviews), deduced types from auto may be **harder to trace** — be mindful of clarity.

---

## auto, Type Deduction, and Uniform Initialization in C++

### Purpose of `auto`

- The `auto` keyword lets the compiler **deduce the variable type** based on the value assigned to it.
- **Compile-time** feature — C++ remains a **statically typed language**, unlike Python.

### Examples and Behavior

```cpp
auto a = 10.0f;  // deduces float
auto b = 10.0;   // deduces double
```

- Using `auto` avoids repetitive type declarations.
- Helps keep code cleaner and avoids errors when types are long or templated.

### Caution with Implicit Conversions

```cpp
std::int32_t b = a;  // implicit float to int — possible data loss
```

- Implicit conversions can **truncate values** (e.g. 10.5 → 10), causing subtle bugs.
- Reader may not realize a conversion occurred — **unclear intent**.

### Use `static_cast` for Clarity

```cpp
auto c = static_cast<std::int32_t>(a);  // explicitly convert float to int
```

- Makes conversions **intentional** and **visible**.

### Uniform Initialization (C++11/20)

```cpp
auto d = std::int32_t{a};  // prevents unsafe/narrowing conversions
```

- **Safer** than using `=` or parentheses.
- Works consistently across all types (hence "uniform").
- Errors or warns if narrowing is unsafe (e.g. from float to int).
- If initialized with empty `{}` → sets to **default value** (e.g. 0 for numbers).

### Compiler Behavior

- GCC: typically gives a **warning** on narrowing conversions.
- MSVC: may issue an **error**, enforcing safety more strictly.

### Best Practices

- ✔ Use `auto` when:

  - The type is obvious or already stated on the right-hand side.
  - You want to avoid redundancy (DRY principle).

- ❌ Avoid `auto` when:
  - It hides conversions or introduces ambiguity.
  - The deduced type might surprise reviewers (e.g. `auto f = 210 + 12.5;` is `double`).

### Tips for Code Reviews

- Code reviewers (especially on GitHub) can’t see deduced types.
- Tools like **VS Code** or IDEs allow hovering over `auto` variables to reveal actual types.
- You don’t have to use `auto` **always** — use it **when it improves clarity and safety**.

### General Rule

- Use `auto` **when you’d otherwise be repeating a type** that’s already obvious from context.
- Prefer **explicit types** or **casting** when conversions or clarity are at stake.

---

## `const` and `constexpr` in C++

1. `const` – Read-Only After Initialization

- Used to **prevent modification** of variables after they are initialized.
- Applicable to:
  - **Function parameters**: makes intent clear that input is not modified.
  - **Local variables**: clearly communicates read-only usage.

Example:

```cpp
std::uint8_t faculty(const std::uint8_t n) {
    std::uint8_t result = 1;
    for (std::uint8_t i = 1; i <= n; ++i) result *= i;
    return result;
}
```

> `n` is marked `const` to show it’s not meant to be modified.

2. `constexpr` – Compile-Time Constants

- Means **constant + can (or must) be evaluated at compile time**.
- Allows:
  - Precomputing results during compilation to **optimize runtime**.
  - Usage in contexts requiring **compile-time constants** (e.g., array sizes, template arguments).

Example:

```cpp
constexpr std::uint8_t faculty(std::uint8_t n) {
    return (n <= 1) ? 1 : (n * faculty(n - 1));
}

constexpr auto val = faculty(5);  // Computed at compile time
```

If `faculty()` is not `constexpr`, assigning its result to a `constexpr` variable would fail.

### Key Differences

| Feature               | const                          | constexpr                             |
| --------------------- | ------------------------------ | ------------------------------------- |
| Meaning               | Read-only after initialization | Compile-time constant (and read-only) |
| When evaluated        | Runtime                        | Compile time (preferred), or runtime  |
| Can change after init | ❌                             | ❌                                    |
| Use with functions    | Yes (input clarity)            | Yes (functions must be constexpr too) |

### Important Notes

- `constexpr` **implies `const`**, but not vice versa.
- A `constexpr` function **can be used at runtime** as well — the keyword just **enables** compile-time evaluation.
- Avoid unnecessary runtime computation by using `constexpr` where possible, especially for **pure functions** with fixed input.

---

## `static` Keyword for Local Variables in C++

### What is `static` in C++ (for local variables)?

- A `static` **local variable inside a function**:
- Is **initialized only once** (at compile-time or before the first function call).
- **Persists** across **multiple function calls**.
- Retains its **last value** between invocations.
- Is **not reinitialized** each time the function runs.

### Use Case

- Ideal when you want a function to **remember state** across calls (e.g., counters, caching values).

### Example: Function Call Counter

```cpp
int faculty(uint8_t n) {
    static int32_t counter = 0;  // initialized only once
    counter++;
    std::cout << "Function called " << counter << " times\n";

    int32_t result = 1;
    for (int i = 1; i <= n; ++i) result *= i;
    return result;
}
```

### Output after two calls:

```
Function called 1 times
Function called 2 times
```

### Key Points to Remember

| Feature                    | `static` local variable      | Regular local variable |
| -------------------------- | ---------------------------- | ---------------------- |
| Initialized once?          | ✅ Yes                       | ❌ No — every call     |
| Retains value across calls | ✅ Yes                       | ❌ No                  |
| Lifetime                   | Entire program               | Current function call  |
| Scope                      | Local to the function        | Local to the function  |
| Stored in                  | Static/global memory segment | Stack                  |

### Why not `constexpr` here?

- `constexpr` is for **compile-time constant** evaluation.
- `static` **requires runtime memory**, so they are **incompatible**.

### Debugging Note

- Breakpoints on `static` variable initialization may not work because it’s **compile-time allocated**.
- You can only debug runtime lines like `counter++`.

### Analogy

Using a `static` variable in a function is like adding **memory** to a pure function — it remembers something from previous calls without needing global or class-level storage.

---

## Namespaces in Modern C++

### What Are Namespaces?

- Namespaces group **related code** (types, functions, constants) under a **common label**.
- Prevent **naming conflicts** in large or multi-library projects.
- Example: `std::cout` means `cout` is part of the **Standard Library (std)**.

### Why Use Namespaces?

- To **clearly organize** large codebases.
- To **avoid collisions** when multiple libraries define functions or types with the same name.
- To improve **code readability and clarity** about where a function/type comes from.

### How to Declare and Use a Namespace

```cpp
namespace db {
    namespace types {
        enum class Status { Unknown, Connected, Disconnected };

        struct User {
            Status status;
            uint64_t id;
        };
    }

    namespace methods {
        void print_status(types::User user) {
            std::cout << "Status: " << static_cast<int>(user.status) << '\n';
        }
    }
}
```

### Using Namespaced Elements

```cpp
int main() {
    db::types::User user{db::types::Status::Connected, 42};
    db::methods::print_status(user);
}
```

### Best Practices

| Tip                                                             | Reason                                      |
| --------------------------------------------------------------- | ------------------------------------------- |
| Use fully-qualified names (e.g., `db::types::Status`)           | Improves clarity; avoids ambiguity          |
| Avoid partial namespace references (e.g., just `types::Status`) | Can confuse readers unfamiliar with nesting |
| Nest namespaces logically (e.g., `db::types, db::methods`)      | Reflects real structure of the library      |
| Use namespaces especially in libraries or reusable modules      | Keeps global namespace clean                |

### Real-world Analogy

Imagine a library with many books. Namespaces are **bookshelves** that help organize related topics together so readers (or compilers) know exactly where to look.

---

## Anonymous Namespaces vs Static in C++ Source Files

### Goal

Limit visibility of functions or global variables **to a single source file** (i.e., a single translation unit).

### 1. `static` in Global Scope (C-style)

- Used for **global functions or variables** to restrict their visibility to the current source file.
- Was the traditional C-style way.

```cpp
// Old C-style
static int compute_sum(int a, int b) {
    return a + b;
}
```

**Effect**: `compute_sum()` is private to this `.cpp` file and not accessible in other files.

### 2. Anonymous Namespaces (Modern C++)

- Modern C++ approach to restrict scope to a single source file.
- More flexible than `static`.

```cpp
namespace {
    int compute_sum(int a, int b) {
        return a + b;
    }

    constexpr int value = 5;
}
```

**Effect**:

- Everything inside the unnamed namespace is private to the current translation unit.
- **Replaces the need for `static`** on globals or free functions.

### 3. Global `static` Variables

```cpp
static constexpr int value = 5;
```

- Also restricts `value` to the current file.
- But modern C++ prefers anonymous namespaces instead.

### Best Practice

| Feature                        | Use                  | Why                             |
| ------------------------------ | -------------------- | ------------------------------- |
| `static` for global func/var   | ❌ Legacy only       | Outdated, C-style               |
| Anonymous namespace            | ✅ Modern C++        | Cleaner, more consistent        |
| `constexpr` or `const` globals | ✅ If needed         | Prefer **read-only**, immutable |
| Global mutable variables       | ⚠️ Avoid if possible | Risk of hidden state or bugs    |

### Remember

- One **translation unit** = one `.cpp` file + included headers.
- `static` and anonymous namespaces both **prevent symbol collisions** at link time by keeping definitions local.

---
