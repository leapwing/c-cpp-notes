# String and Files

## The content about `std::string` in C++:

### Overview

- `std::string` is like a `std::vector<char>`, optimized for text handling.
- Supports many **built-in utility functions** like `find`, `replace`, `substr`, etc.
- Requires including the `<string>` header.
- String literals (`"text"`) are initially `const char[]` but are implicitly converted to `std::string`.

### Key Concepts

#### Basic Operations

- `std::string s = "Hello world!";`
- `s.empty()` – Checks if string is empty.
- `s.length()` or `s.size()` – Returns number of characters (excluding null terminator `\0`).

#### Finding Substrings

- `s.find("e")` – Returns **first index** of substring or character.
- `s.rfind("e")` – Returns **last index** (reverse find).
- `std::string::npos` – Special constant returned when a search fails.

#### Replacing Substrings

- `s.replace(pos, len, new_string)` – Replaces part of string starting at `pos` with `len` characters.

#### Substring Creation

- `s.substr(pos, len)` – Returns a substring starting at `pos` of length `len`.

### Conversions

#### Number to String

- `std::to_string(42)` – Converts integer or float to string.

#### String to Number

- `std::stoi("42")` – Converts string to `int`.
- `std::stod("3.14")` – Converts string to `double`.
- Also supports different bases (e.g., binary or hex):
  `std::stoi("101", nullptr, 2)` → 5 (binary input).

### String Comparisons

- Use `==` or `!=` for equality/inequality.
- `s1.compare(s2)` returns:

  - `0` → equal
  - `<0` or `>0` depending on lexicographical comparison

### Performance Notes

- `std::string` performs **heap allocations** under the hood, which can be slow if used heavily or repeatedly in performance-critical code.
- Optimization trick discussed in next video (not included here).

### Best Practices

- Use `std::string::npos` to detect failed searches.
- Avoid repeated heap allocations when possible (e.g., reuse strings).
- Use `std::string_view` or optimized techniques for large-scale parsing (covered later).

---

## Small String Optimization and `std::string_view` in C++

### Small String Optimization (SSO)

#### What is SSO?

- **SSO** allows small strings (≤15 or ≤22 characters depending on the implementation) to be stored **on the stack**, avoiding costly heap allocations.
- Implemented in most modern C++ compilers (e.g., GCC, Clang, MSVC).

#### How it works:

- `std::string` includes an internal fixed-size character array.
- If the string fits, no heap allocation occurs — it's faster and more memory efficient.
- Strings **longer than the threshold** will still allocate on the heap.

#### Example:

```cpp
std::string s1 = "short string"; // ≤15 chars → stack (no heap)
std::string s2 = "this is a very long string that exceeds the threshold"; // → heap allocation
```

### Temporary Objects and Unintended Heap Allocations

#### Problem:

- When passing string literals (C-style `const char[]`) to functions accepting `const std::string&`, a **temporary `std::string`** is created.
- If the string is long, it **triggers a heap allocation**, slowing performance.

#### Example (causes heap allocation):

```cpp
void func(const std::string& s);
func("this is a long string"); // temporary std::string created
```

### Solution: Use `std::string_view`\*\*

#### What is `std::string_view`?

- A **lightweight, non-owning view** of a string or string literal.
- No heap allocation.
- Ideal for **read-only** string inputs.
- Similar to `std::span` for arrays/vectors.

#### Key traits:

- Can take both `std::string` and string literals (`const char[]`) as input.
- Read-only (modification is not allowed).
- Avoids heap allocation even for long string literals.

#### Recommended usage:

```cpp
void func(std::string_view s); // preferred for read-only string parameters
```

### Summary Takeaways

| Topic                 | Best Practice                                           |
| --------------------- | ------------------------------------------------------- |
| Short strings         | Benefit from SSO (automatic, compiler-optimized)        |
| Long string literals  | Use `std::string_view` to avoid temporary `std::string` |
| Read-only parameters  | Prefer `std::string_view` over `const std::string&`     |
| Modifiable parameters | Use `std::string&` or `std::string` as needed           |

---

### Overview: File I/O in C++

#### 🔧 Libraries

- Include `<fstream>` to use file streams.

  - `std::ofstream`: Output (write) to files
  - `std::ifstream`: Input (read) from files

### Text File Writing (Human-Readable)

- Use `std::ofstream file("text.txt", std::ios::out);`
- Use `file << line;` to write text to the file.
- Optional: Use `file.close();` (automatic on destruction)
- Append mode: Add `std::ios::app` flag if needed.

#### Example:

```cpp
std::string line = "Hello, World!";
std::ofstream file("output.txt", std::ios::out);
file << line;
```

### Binary File Writing (Structs / Raw Data)

#### Example Struct:

```cpp
struct PlayerData {
    uint32_t id;
    float x;
    float y;
};
```

#### Write Struct to Binary File:

```cpp
PlayerData player{1, 10.0f, 110.0f};
std::ofstream file("data.bin", std::ios::out | std::ios::binary);
file.write(reinterpret_cast<char*>(&player), sizeof(PlayerData));
```

### Binary File Reading

- Use `std::ifstream` in `std::ios::in | std::ios::binary` mode.
- Use `file.read(...)` to load raw bytes into a struct.

#### Read Struct:

```cpp
PlayerData loadedPlayer{};
std::ifstream file("data.bin", std::ios::in | std::ios::binary);
file.read(reinterpret_cast<char*>(&loadedPlayer), sizeof(PlayerData));
```

### Checks and Validations

- Use `file.fail()` to check open errors.
- Use `file.good()` after writing to confirm no I/O errors.

### Extra Notes

- `std::string` uses **heap allocations**, but short strings may benefit from **SSO** (Small String Optimization).
- Binary data (like structs) should be written/read **byte-wise** using casting.
- Binary files are **not human-readable**, but are efficient for saving and restoring structured data.

### Example Outcome

Write and read a `PlayerData` struct with:

```cpp
PlayerData{ id = 1, x = 10.0, y = 110.0 }
```

→ Binary file stores 12 bytes
→ Reading the binary file restores the exact original values.

---

### C++17 Filesystem Overview (`<filesystem>`)

#### Purpose:

- Unified cross-platform file and directory handling (Windows, Linux, macOS)
- Handles differences in **path separators**, e.g. `/` vs `\\`

#### Setup:

```cpp
#include <filesystem>
namespace fs = std::filesystem;
```

### Path Handling

- Use `fs::path` to create and manipulate file paths
- Paths are constructed with `/`, automatically adjusted per OS
- You can:

  - Append paths: `path /= "subdir"`
  - Get path info: filename, extension, parent, etc.
  - Check if path is relative/absolute

#### Example:

```cpp
fs::path base = fs::current_path(); // e.g., "/home/user/project"
fs::path full = base / "chapter5" / "file.cpp";
```

### Directory Iteration

- Use `fs::directory_iterator(path)` to loop through contents:

```cpp
for (const auto& entry : fs::directory_iterator(fs::current_path())) {
    std::cout << entry.path() << "\n";
}
```

### Path Inspection

- `exists(path)` – check if file or folder exists
- `is_regular_file(path)` – check if it’s a file
- `is_directory(path)` – check if it’s a directory
- `path.filename()` – get file name
- `path.extension()` – get `.ext`
- `path.stem()` – get name without extension

### Create & Copy

- **Create directories**:
  `fs::create_directory(path);`
- **Copy files**:

```cpp
fs::copy_file("source.cpp", "dest/folder/target.cpp");
```

### Benefits

- Cross-platform compatibility
- Cleaner, more readable file handling
- No need for OS-specific APIs

---
