# General guidelines <!-- omit in toc -->

Code controls more and more things on our planet. Programmers have the power to create and change such code but

> With great power comes great responsibility.
> 
> — You know who

and this responsibility is to write clean code, i.e., code that is correct as well as easy to read, understand, maintain and extend. On that note I encourage everyone to read the first two to five chapters of Robert C. Martin's excellent book "Clean Code: A Handbook of Agile Software Craftsmanship". Knowing that most of you will not do that, the following guidelines, principles and anti-patterns, taken from and inspired by various books, blog posts and discussions across the whole internet should help you with that. However, writing good, clean code still remains a difficult task. Then again,

> If the job was easy, it wouldn't be fun.
> 
> — John Garret


### Contents

- [1. General](#1-general)
- [2. Principles](#2-principles)
- [3. Anti-patterns](#3-anti-patterns)
- [4. Naming](#4-naming)
- [5. Statements](#5-statements)
- [6. Comments and Documentation](#6-comments-and-documentation)


## 1. General

For every item in this list there is a reason or justification, but it is not (always) given explicitly to keep this short and terse. However, there is obviously some overlap between the guidelines, principles and anti-patterns. If you have questions or complains about anything, please contact the author (Patrick Kappl).


### 1.1. Any violation of the guide is allowed if it allows to write cleaner code.

## 2. Principles

### 2.1. Make Interfaces Easy to Use Correctly and Hard to Use Incorrectly

According to Scott Meyers the single most important guideline when writing software and I agree, because interfaces are everywhere.


### 2.2. Keep It Simple, Stupid (KISS)

Write code as simple as possible. Clever code is bad. Simple, easy to understand code is good.


### 2.3. Don't Repeat Yourself (DRY)

Avoid duplication of data and logic. If the same piece of code appears in multiple locations, create an abstraction (constant, function, class, etc.) for it. Do not do copy-and-paste programming.


### 2.4. Single Responsibility Principle (SRP)  


Every construct (i.e. class, module, function etc.) should only have one functionality. Robert C. Martin defines this as:

> A class should only have one reason to change.

This automatically leads to those constructs being smaller, which in turn helps with the KISS principle since smaller functions, classes and modules are easier to understand than big ones.

The single responsibility principle is also the first (and probably most import) of the so called [SOLID principles](https://en.wikipedia.org/wiki/SOLID).

TODO: add rest of the SOLID principles


### 2.5. You Aren't Gonna Need It (YAGNI)

Do not write code on the chance that you may need it in the future. Do not solve a problem that does not exist.


### 2.6. Refactor

Your code will not be perfect the first time you write it. Take the time to review your code and improve it.


### 2.7. The Boy Scout Rule

> Always leave the code you're editing a little better than you found it.
> 
> — Robert C. Martin

But:

> The steps you take don't need to be big, they just need to take you in the right direction.
> 
> — Jemma Simmons

You can rename one variable, get rid of a small duplication or break up a long function.


## 3. Anti-patterns

These are not guidelines but things that should be avoided. They inevitable overlap with the other principles and guildelines but sometimes it is easier to remember what *not* to do.


### 3.1. Premature Optimization

> Premature optimization is the root of all evil.
> 
> — Donald Knuth

Measure and profile before you optimize!


### 3.2. Magic Numbers and Strings

Use named constants instead.


### 3.3. Bike-shedding

This is the tendency to spend excessive amounts of time debating and deciding on trivial and often subjective issues. Tools like code formatters help with some of this.

See also [Parkinons's law of triviality](https://en.wikipedia.org/wiki/Law_of_triviality).


### 3.4. Analysis Paralysis

Overanalyzing to the point that it prevents action and progress. This, and the previous one, are probably the hardest anti-patterns for me to avoid (I just love overanalyzing bike sheds).


### 3.5. Reinventing the Wheel

You are not the first one who wants to calculate CRC checksums, search for patterns in strings or quickly sort data. Do not be afraid of using other people's code and do not be Too Lazy To Google (TLTG). Use libraries.


### 3.6. God Class

A class or similar construct that controls and depends on (almost) everything.


### 3.7. Useless (Poltergeist) Classes

> It seems that perfection is attained, not when there is nothing more to add, but when there is nothing more to take away.
> 
> — Antoine de Saint Exupéry

Do not take the principles of the last section too far and write classes, functions, modules, etc. that have no real responsibility of their own, only invoke other constructs without adding anything useful, and are an unneeded layer of abstraction. I know balancing this in particular with the KISS and single responsibility principle can be hard, but no one said that this is going to be easy.


## 4. Naming

Think ***a lot*** about names! I can not stress this enough. Good names are ***super important*** for good, clean code, but unfortunately very hard to come up with most of the time. Help may be found in [this](https://www.youtube.com/watch?v=ZDluHz-ybPE) great talk from Kate Gregory.


### 4.1. All code, including comments, shall be written in English

### 4.2. Names consisting of multiple words should be ordered linguistically.

Code should be readable like English sentences.

~~~
maxTemperatureThreshold, estimatedStandardError
filtered_subdirectory_list, write_config_data_to_file
~~~


### 4.3. Avoid abbreviations in names.

But, do *not* write out things that are better known by their acronyms like CPU, HTML, UART, SPI, etc.


### 4.4. Type and variable names should be nouns.

### 4.5. Function names should be verbs, describing what they do, or nouns describing what they return.

Functions should only be named after what they return, if that is their main purpose and they are computationally cheap (otherwise I would argue that their main purpose is the computation). Deciding that is difficult, but so is writing good code in general.


### 4.6. Use plural form for collections of objects, like arrays, lists, tuples, etc.

### 4.7. Use `is`, `has`, `can`, `should`, etc. in boolean names.

Boolean names should read well in `if` statements.

~~~cpp
valueIsValid = (lowerThreshold < value) && (value < upperThreshold);
if(valueIsValid)
    ...
if(HasFourWheels(vehicle))
    ...
if(person.canLift(100_kg))
    ...
~~~

**Exception**: Boolean function paramters can be named like a command because that still reads well on the user's side and it can save a lot of characters.

~~~python
draw_fancy_graph(..., store_plot)       # instead of plot_should_be_stored
compute_something(..., print_result)    # instead of result_should_be_printed
~~~


### 4.8. Avoid negated boolean names.


### 4.9. Use the prefix `n` for variables representing a number of objects.

~~~
nDimensions, nReceivedMessages
n_tasks, n_sensors
~~~


### 4.10. Use the prefix `i` for names representing an entity number to make them named iterators.

This works especially well in loops.

~~~cpp
iMessage, iX, iJob
i_task, i_sensor
for(int iPage = 0; iPage < nPages; ++iPage)
~~~


### 4.11. The term `compute` should be used in methods where something is calculated.


### 4.12. Complement names shall be used for complement operations.

~~~
old/new, add/remove, create/destroy, start/stop, insert/delete,
increment/decrement, begin/end, first/last, up/down, min/max,
next/previous, open/close, show/hide, suspend/resume, etc.
~~~


## 5. Statements

### 5.1. Variables should be declared in the smallest scope possible.

As a consequence, global variables should be avoided.


### 5.2. Variables should be initialized where they are declared.

### 5.3. Variables shall never have more than one meaning.

### 5.4. Complex conditional expressions should be avoided. Introduce temporary boolean variables instead.

## 6. Comments and Documentation

### 6.1. Tricky code should not be commented but rewritten.

Clean code with good names does not need many comments. So, again, try to find good names! It can be hard, but is worth the trouble.


### 6.2. Do not comment and document the obvious.

If you choose good names, documentation can seem redundant and repetitive. If that is the case – great – do not write that documentation (unless of course your project requires you to write it anyway). Some (presumably) smart guy once said:

> Bad code is easy to document.  
> Good code ist hard to document.

So write good code, and stop wasting everyone's time with redundant documentation.

~~~cpp
// Examples of repetitive and redundant documentation

//! @brief Computes the area of the given rectangle.    // Oh, really?
//!
//! @param[in]  rectangle   a rectangle                 // Who'd have thunk it?
//! @returns    area of the given rectangle             // No shit Sherlock!
float computeArea(const Rectangle * rectangle)

// Loop over all the things                             // You don't say o_O
for(int iThing = 0; iThing < nThings; ++iThing)
~~~


### 6.3 Don't tell me what it does. Tell me what it's for.

This quote from Kate Gregory is important for documentation written for users. They don't care too much about what your function, class, etc. does exactly (or how it does it). They want to know which problem they can solve with it.
