# C++ style guide

## C/C++ versions
Currently, code should target C++17, i.e., should not use C++2x features. For pure C you may use C11.

## Naming

Optimize for readability using names that would be clear even to people on a different project.

Use names that describe the purpose or intent of the object. Do not worry about saving horizontal space as it is far more important to make your code immediately understandable by a new reader. Minimize the use of abbreviations that would likely be unknown to someone outside your project (especially acronyms and initialisms). Do not abbreviate by deleting letters within a word. As a rule of thumb, an abbreviation is probably OK if it's listed in Wikipedia. Generally speaking, descriptiveness should be proportional to the name's scope of visibility. For example, n may be a fine name within a 5-line function, but within the scope of a class, it's likely too vague.

### File Names
Filenames should be all lowercase and may include underscores `_` as separator, examples below.

```
// for headers
common.h

// for pure C code
main.cc

// for C++ code
object_impl.cpp
```

### Type Names
The names of all types (classes, structs, type aliases, enums, and type template parameters) have the same naming convention. Type names should start with a capital letter and have a capital letter for each new word. No underscores.

```
enum class Color { ... };

struct ObjectManager { ... }

using IdToString = std::map<uint64_t, std::string>;
```

### Variable names
The names of variables (including function parameters) and data members are all lowercase, with underscores between words. Data members of classes (but not structs) additionally have trailing underscores.

```

struct Object {
    std::string name;
};

class Scalar {

public:
    void add(int other) { value_ += other; }

private:
    int value_ = 0;
};
```

### Const names
Variables declared constexpr or const, and whose value is fixed for the duration of the program, are named with a leading "k" followed by mixed case.
```
constepxr double kEps = 1e-6;

enum class Color { kRed, kGreen, kYellow };
```


### Function names
All functions and methods have mixed case names andstart with a lower letter and have a capital letter for each new word.

```
void doJob() { ... }

class Object {

    ...

    const std::string name() const;

    Object& setSame(const std::string& name);

    ...
};
```

Accessors and mutators should star with get/set, but it is possbile to omit for conciseness.

### Namespace names
Namespace names are all lower-case, with words separated by underscores. Top-level namespace names are based on the project name. Avoid collisions between nested namespaces and well-known top-level namespaces.

```
namespace project::library {
...
}  // namespace project::library
```


### Macro names
Just avoid macros, but if you really need it...

```
#define ROUND(x) ...
#define PI 3.1415
```

### Exceptions
If you are naming something that is analogous to an existing C or C++ entity then you can follow the existing naming convention scheme.
