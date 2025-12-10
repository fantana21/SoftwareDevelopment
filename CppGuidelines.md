# C++ guidelines

### Contents

- [1 Idioms and other general stuff](#1-idioms-and-other-general-stuff)
- [2 Naming](#2-naming)
- [3 Layout and white space](#3-layout-and-white-space)
- [4 Files](#4-files)
- [5 Statements](#5-statements)
- [6 Comments and documentation](#6-comments-and-documentation)


## 1 Idioms and other general stuff

### 1.1 When in doubt, check out the [C++ Core Guidelines](http://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines#main).

There are a lot of them, so they are not something you just read through. But if you have a problem or are not sure what the best or most idiomatic way to implement something is, take a look at them. You will most likely find a sensible answer there. However, you do not have to agree on everything with the guidelines. I do not either. They are guidelines (and not rules) after all.


### 1.2 Write `const` correct code.

Take a look at [these FAQ](https://isocpp.org/wiki/faq/const-correctness) of the C++ Core Guidelines to answer all your questions about `const` correctness.


### 1.3 Use RAII.

Welcome to the first of the many notoriously bad acronyms of C++. RAII stands for Resource Acquisition is Initialization and – to put it simple – just means that you should not use naked `new` and `delete` and instead wrap any resource in class that properly handles creation and destruction. We do not need a garbage collector in C++ because we have constructors and destructors that are called when an object is initialized and when it goes out of scope, respectively.


### 1.4 Apply the rule of 0/5.

Define either none (0) or all (5) of the special member functions (SMF). The rule of 0 is preferred for the obvious reason of simplicity.

See the following C++ Core Guidelines for more details.

- [C.20: If you can avoid defining any default operations, do](https://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines#Rc-zero)
- [C.21: If you define or =delete any copy, move, or destructor function, define or =delete them all](https://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines#Rc-five)
- <a>C.22: Make default operations consistent</a>


## 2 Naming

### 2.1 Use the following cases when naming identifiers

| Type        | Case          |
| ----------- | ------------- |
| variable    | camelCase     |
| function    | PascalCase    |
| type        | PascalCase    |
| concept     | PascalCase    |
| namespace   | lowercase     |
| macro       | SHOUTING_CASE |
| file/folder | PascalCase    |

- Constants are just `const` or `constexpr` qualified variables, so they also use camelCase.
- Types are things like classes, structs, enums, type aliases, etc.
- A single template parameter can be named with a single upper case letter.
- Acronyms are no exception from the above table.
- Macros should be avoided at (almost) all cost.
- When naming mathematical quantities that often appear in code, one can deviate from the naming conventions. For example, if you write a program that does computations with electric and magnetic fields, it is reasonable to call the corresponding variables `E` and `B` to make the equations more readable in code.


### 2.2 Use singular names for enum types, since plural names look silly in use.

### 2.3 Do not name pointers specifically like `pLine`, `linePtr`, etc.


## 3 Layout and white space

### 3.1 Tabs must not be used. Only spaces are allowed for indentation and white space formatting.

### 3.2 Indentation width is 4 spaces.

### 3.3 Always use braces, even if there is only a single statement in a block.

Empty while loops are the only exception.


### 3.4 Separate things like function definitions, class declarations, enums, etc. by two blank lines.

Within a function, class, etc. things are separated by single blank lines, so to make function, class, etc. definitions stand out more, two blank lines are used between them. Also, after things like the `#include` block, global constants/variables, etc. two blank lines should be used.


## 4 Files

### 4.1 Use the following file extensions.

| File type        | Extension |
| ---------------- | --------- |
| C++ source file  | `.cpp`    |
| C++ header file  | `.hpp`    |
| C++ inline file* | `.ipp`    |
| C source file    | `.c`      |
| C header file    | `.h`      |

\* an inline file contains definitions of inline functions, function templates, ...


### 4.2 Header files shall contain a `#pragma once` instead of include guards.

### 4.3 Header and source files shall contain everything they need to compile by themselves.

**Exception**: source files do not need to repeat the includes of the corresponding header file.

~~~cpp
// --- Thing.hpp ---
#include <vector>

// code that needs <vector>

// --- Thing.cpp ---
#include <Thing.hpp>

#include <utility>
#include <vector>  // This include is optional

// code that needs <vector> and <utility>
~~~


### 4.4. A header file should only be included when a forward declaration would not do the job.

### 4.5. A header file should be designed in such a way, that the order of header file inclusion is not important.

### 4.6. Header and source files should be organized as specified below.

1. `#include`s
2. `#define`s
3. types like `enum`s, `struct`s, `class`es, etc.
4. global constants
5. global variables
6. function declarations, also for inline functions and function templates
7. function definitions (normal ones in source files, inline function and template
   definitions in headers; the latter can be moved to a separate `.ipp` file)


## 5 Statements

### 5.1 Use almost always `auto` (AAA)

That includes trailing return types for functions but does not mean to always deduce the type. It's totally fine to explicitly specify the type but do that after the equal sign.

~~~cpp
auto pi = 3.14;                       // Deduced:  double
auto bigNumber = 957'000UL;           // Explicit: unsigned long
auto numbers = std::array<int, 5>{};  // Explicit: std::array<int, 5>
auto thing = Thing(a, b, c)           // Explicit: Thing
auto firstNumber = numbers.front();   // Deduced:  I-don't-really-care

std::uint32_t counter = 0;                      // Exception: there is no literal suffix
auto counter = static_cast<std::uint32_t>(0);   // and this second line is too much

auto Add(int a, int b)                // Deduced:  int
{
  return a + b;
}
auto DoIt(int i) -> double            // Explicit: double, trailing return type
{
  return static_cast<double>(i) + 0.32425;
}
~~~


### 5.2 Make functions returning something `[[nodiscard]]` by default.

Usually there is a good reason why functions return something, so you actively have to think of an even better reason to remove the `[[nodiscard]]` attribute.


### 5.3 Use the following conventions for function parameters.

| Parameter direction              | Function signature | Example parameter type `T` |
| -------------------------------- | ------------------ | -------------------------- |
| in                               | `f(T const &)`     |                            |
| in (cheap or impossible to copy) | `f(T)`             | `int`, `std::unique_ptr`   |
| in/out                           | `f(T *)`           |                            |
| out                              | `T f()`            |                            |
| out (expensive to move)          | `f(T *)`           | large `std::array`         |

The raw pointer for (in/)out parameters requires an `&` operator at the call site. This signals the user that a parameter is modified by the function.


### 5.4 Prefer return types over out parameters.

### 5.5 Prefer initializing as specified below.

- `= value` for POD (plain, old data) types like int, double, etc.
- `= T{1, 2}` or `= T{.one = 1, .two = 2}` for element initializers (aggregates,
  `initializer_list`, etc.)
- `= T{}` for default construction because it also works with POD types and value
  initializes them to zero
- `= T(a, b)` otherwise, i.e., for calling "normal" constructors


### 5.6 Use explicit type conversions. Do not rely on implicit ones.

### 5.7 Implicit tests for 0 should not be used other than for boolean variables.

~~~cpp
if(nLines != 0)     // NOT: if(nLines)
if(value != 0.0)    // NOT: if(value)
if(shape.isFilled)  // OK
~~~


### 5.8 Add `[[fallthrough]]` to every `switch` case without a `break` statement.

### 5.9 Use `while(true)` for infinite loops.

### 5.10 Executable statements in conditionals should be avoided.

### 5.11 Avoid using `goto`.

### 5.12 Floating point numbers shall be written with at least one digit before and after the decimal point.

### 5.13 Use `'` as a thousands separator

This is less of a choice (only `'` can be used this way) but more of a reminder to actually do it.

~~~cpp
constexpr auto speedOfLight = 299'792'458;
~~~


## 6 Comments and documentation

### 6.1 Only use `//` for comments, never `/**/`.

`/**/` does not nest, which can lead to unexpected results.

### 6.2 Add a space after `//`

~~~cpp
//This is my great comment, but it is wrong
// This is my second comment. It looks much better because of the space.
~~~

### 6.3 Start comments with an upper case letter

~~~cpp
// This is right
// this is wrong
// hueInterval is in [0, 360]
// Line #3 is an exception because "hueInterval" is a variable name
~~~

### 6.4 Use `//!` for all Doxygen comments, including multi-line Doxygen comments.

### 6.5 Always use `@` for Doxygen commands.

### 6.6 Explicitly use `@brief` for all brief descriptions.

### 6.7 Use symbol names such that Doxygen automatically creates links.

That means use `functionName()`, `structName::memberName`, `@see`, `@ref`, etc.


### 6.8 Use good Doxygen markup.

Use `@a` (italics) for member variables, `@b` (bold) for emphasizing single words and `` ` `` (backticks) for file names and code symbols.
