# Speeding up python

Most of the following optimizations can also be applied to R code. R is very
similar in the way it runs to python. They are both interpreted languages and
rely on libraries/packages for speedy operations.

## Introduction

> [!info] Skip this section if you don't care about how the optimizations work.

Python is not a very fast programming language. This is because python is an
interpreted language, not a compiled one. Interpreted languages must be
evaluated at run-time, not compile-time. Unlike a C program, python programs
must be executed by the python runtime, or interpreter. This is seens as an
advantage by many, as it allows for rapid development and testing without
recompilation.

But why is interpreted code slower than compiled code? To answer this question,
one must understand what compiled code is: a sequence of binary encoded
[Opcodes](https://en.wikipedia.org/wiki/Opcode). Opcodes are the instructions
run by the CPU and correspond to the instructions given by assembly code;
assembly is just human-readable opcodes.

Computers are really fast. Really really really fast. A modern CPU has a clock
speed in the gigahertz or billions of cycles per second. A CPU can perform
multiple operations per cycle. So a 5GHz processor might be able to run 10 or 15
billion operations in a single second. That's a lot!

A compiled program can just load instructions into the CPU and take advantage of
that insane speed, but any language with an interpreter has to do things before
it can load instructions into the CPU, namely, interpret the code.

So what happens when a piece of code is interpreted? The python interpreter is a
C program. Since C doesn't have a runtime, the interpreter is fast! It can load
instructions directly into the CPU. Whenever a C program calls a C function,
some set of operations is run on the CPU. In a standard C program, the amount of
operations that need to run for each piece of code is minimal. 2 lines of C code
might be 15 lines of assembly code.

When a piece of python code is run, the interpreter does many things for that 1
piece of code. Remember, operations that occur at compile-time in other
languages (parsing), occur at run-time in python code Let's take this piece of
code:

```python
a = 1 + 2
```

First the python code needs to be parsed into an
[Abstract Syntax Tree](https://en.wikipedia.org/wiki/Abstract_syntax_tree) (AST)
using a parser. This parser includes many substeps like a
[lexer](https://en.wikipedia.org/wiki/Lexical_analysis) which breaks the code
into tokens. The AST must then be converted into bytecode (which is more
efficient to reparse into an AST). Once the AST is converted into bytecode, the
AST is then converted into a sequence of C function calls.

The C language is not memory safe, so the programmer has to manage memory
manually. When the programmer makes a mistake, C programs can have stack
overflow errors and memory leaks which must be debugged at run-time. Python
programs are memory safe. This is a good thing, as many headaches are removed
from the programmer's head. However, the cost is that python is less memory
efficient and the runtime/interpreter must manage the memory at run-time.

The python interpreter uses a
[garbage collector](<https://en.wikipedia.org/wiki/Garbage_collection_(computer_science)>)
to manage memory. A garbage collector will keep track of all the memory you use
and throw away the stuff you don't need, the garbage. This works great, but
takes extra processing time every time it takes out the garbage.

In summary, python requires more CPU time to compensate for its abstractions
over C. Lots of people like these abstractions, hence why python is popular.

# Make it Fast!!

So, python is slow; Thousands of times slower than C. How do we make it faster?

The general rule of thumb is to use python less.

## Bindings; or, use another language!

The most obvious way to use python less is to use another language other than
python. You can still call these faster languages in python, effectively
speeding up the parts of the code you need to be faster.

Writing in another language and calling it in python is called writing python
**bindings**. Python bindings are traditionally written in C, but other
languages can work. Give rust a try!

- [C/C++ guide](https://realpython.com/python-bindings-overview/#python-bindings-overview)
- [Rust guide](https://pyo3.rs/v0.27.0/)

> [!info] This may be the only way to really speed up rust code

## Compile it!

### JIT it!

A just-in-time (JIT) compiler compiles python into machine code when needed.
This greatly shortens the amount of assembly/opcodes required to actually run
the python code. It still has to interpret the code once, but that is ok.

The only package I know of that can JIT compile python is
[numba](https://numba.pydata.org/).

For example:

```python
from numba import njit
import random

@njit
def monte_carlo_pi(nsamples):
    acc = 0
    for i in range(nsamples):
        x = random.random()
        y = random.random()
        if (x ** 2 + y ** 2) < 1.0:
            acc += 1
    return 4.0 * acc / nsamples

```

This will run thousands of times faster than normal python!!

Unfortunately, numba is only working for a subset of python, meaning not all
python code can be JIT compiled :(

## Just regular compile it

There is a compiler for python called [cython](https://cython.org/). However, it
should be noted that python is slow even with compilation due to run-time
nastiness. Cython requires you to use static typing to greatly speed up code. It
has greater compatibility than numba, but is harder to use.

## Syntax tricks

Probably the least reliable tricks are within the python language itself. In
essence, we trick the python interpreter to use less python. As these are based
on how the python interpreter is implemented, they are not always intuitive and
can change between python versions.

### List Comprehensions!

Similar to R's `apply()`, a list comprehension builds a python list using a
function on that list. Use them whenever possible. Consider this `for` loop:

```python
l = []
for i in range(5):
	l.append(l+5)
```

This would return a list that looks like this: `[5, 6, 7, 8, 9]`. The equivalent
list comprehension:

```python
l = [x+5 for x in range(5)]
```

This should run around 10x faster! Why is this the case? Because the bytecode
generated from a list comprehension is shorter than a for loop, which translates
to less machine code in the end. This is a quirk of how the python interpreter
works.

### Use built-in/library methods for everything

Whenever possible, use the library implementation of an operation. Not only is
the underlying implementation usually not written in python, the developers are
usually smarter than you. Consider this sorting implementation:

```python

def sort(l):
    sorted_l = []
	while( len(l)):
		mi = 0
		for i in range(len(l)):
			if l[i] < l[mi]:
				mi = i
	    sorted_l.append(l[mi])
		l.pop(mi)
	return sorted_l

l = sort([4, 2, 5, 3])

```

This is soooo much slower than the builtin sort:

```python
l = [4,2,5,3].sort()
```

Again, the reason for this is that the builtin sort is actually calling a C
function. The interpreter only has to interpret "sort", not the entire sort
function. Another reason is that the sorting algorithm in python is also not
optimized. Some algorithms are faster than others simply because they are more
efficient. See [visualgo.net](https://visualgo.net/en/sorting) for more info!

### Additional tips

Python's website also has some tips:
[Performance tips](https://wiki.python.org/moin/PythonSpeed/PerformanceTips)
