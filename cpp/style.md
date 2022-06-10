# C/C++ style guide

## Versions
Currently, code should target C++17, i.e., should not use C++2x features. For pure C you may use C11.

## Headers
Header files should be self-contained (compile on their own) and end in `.h`. Users and refactoring tools should not have to adhere to special conditions to include the header. Specifically, a header should have `#pragma once` and include all other headers it needs. When a header declares inline functions or templates that clients of the header will instantiate, the inline functions and templates must also have definitions in the header, either directly or in files it includes.

If a source or header file refers to a symbol defined elsewhere, the file should directly include a header file which properly intends to provide a declaration or definition of that symbol. It should not include header files for any other reason. Do not rely on transitive inclusions.

### Forward declarations
A "forward declaration" is a declaration of an entity without an associated definition. Avoid using forward declarations where possible.

### Inline Functions
Define functions inline only when they are small, say, 10 lines or fewer. It allows the compiler to expand them inline rather than calling them through the usual function call mechanism.

### Names and order of includes
Include headers in the following order:
- related header
- project specific headers 
- external headers
- C/C++ standard library headers

Separate each non-empty group with one blank line. It is preferable to follow the alphabetical order in each group.

## Scoping

### Namespaces
Namespaces subdivide the global scope into distinct, named scopes, and so are useful for preventing name collisions in the global scope. With few exceptions, place code in a namespace. Namespaces should have unique names based on the project name, and possibly its path.

- Terminate multi-line namespaces with comments as shown in the given examples.
- To place generated protocol message code in a namespace, use the package specifier in the .proto file.
- Avoid to declare anything in namespace std, including forward declarations of standard library classes.
- You may not use a using-directive to make all names from a namespace available.
- Do not use aliases at namespace scope in header files except in explicitly marked internal-only namespaces, because anything imported into a namespace in a header file becomes part of the public API.
- Do not use inline namespaces in headers.

### Internal linkage
When definitions do not need to be referenced outside `.c/.cpp` files, give them internal linkage by placing them in an unnamed namespace or declaring them static. It is possible have several inline namespaces in one file.

Prefer placing nonmember functions in a namespace; use completely global functions rarely. Do not use a class simply to group static members. Static methods of a class should generally be closely related to instances of the class or the class's static data.

### Nonmember, static member and global functions
Prefer placing nonmember functions in a namespace; use completely global functions rarely. Do not use a class simply to group static members. Static methods of a class should generally be closely related to instances of the class or the class's static data.

### Local variables
Place a function's variables in the narrowest scope possible, and initialize variables in the declaration.

### Static and global Variables
C++ allows you to declare variables anywhere in a function. We encourage you to declare them in as local a scope as possible, and as close to the first use as possible. This makes it easier for the reader to find the declaration and see what type the variable is and what it was initialized to.

Be careful with initialization, because of order is undefined!

### Thread local
Thread local variables that aren't declared inside a function must be initialized with a true compile-time constant. Prefer thread_local over other ways of defining thread-local data.

Such a variable is actually a collection of objects, so that when different threads access it, they are actually accessing different objects. Thread local variable instances are initialized much like static variables, except that they must be initialized separately for each thread, rather than once at program startup. These variable instances are not destroyed before their thread terminates, so they do not have the destruction-order issues of static variables.

## Naming
Optimize for readability using names that would be clear even to people on a different project.

Use names that describe the purpose or intent of the object. Do not worry about saving horizontal space as it is far more important to make your code immediately understandable by a new reader. Minimize the use of abbreviations that would likely be unknown to someone outside your project (especially acronyms and initialisms). Do not abbreviate by deleting letters within a word. As a rule of thumb, an abbreviation is probably OK if it's listed in Wikipedia. Generally speaking, descriptiveness should be proportional to the name's scope of visibility. For example, n may be a fine name within a 5-line function, but within the scope of a class, it's likely too vague.

### Files
Filenames should be all lowercase and may include underscores `_` as separator, examples below.

```
// for headers
common.h

// for pure C code
main.cc

// for C++ code
object_impl.cpp
```

### Types
The names of all types (classes, structs, type aliases, enums, and type template parameters) have the same naming convention. Type names should start with a capital letter and have a capital letter for each new word. No underscores.

```
enum class Color { ... };

struct ObjectManager { ... }

using IdToString = std::map<uint64_t, std::string>;
```

### Variables
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

### Constants
Variables declared constexpr or const, and whose value is fixed for the duration of the program, are named with a leading "k" followed by mixed case.

```
constepxr double kEps = 1e-6;
enum class Color { kRed, kGreen, kYellow };
```

### Functions
All functions and methods have mixed case names andstart with a lower letter and have a capital letter for each new word.

```
void doJob() { ... }

class Object {
    const std::string name() const;
    Object& setSame(const std::string& name);
};
```

Accessors and mutators should star with get/set, but it is possbile to omit for conciseness.

### Namespaces
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

## Formatting
To help you format code correctly use standart [`.clang-format`](.clang-format) config.