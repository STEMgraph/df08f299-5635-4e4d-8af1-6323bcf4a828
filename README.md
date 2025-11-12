<!---
{
  "id": "df08f299-5635-4e4d-8af1-6323bcf4a828",
  "teaches": "Sizes of Built-in Datatypes in C",
  "depends_on": ["0b6b3ce8-418e-4900-ae42-a6d068389a12"],
  "author": "Stephan Bökelmann",
  "first_used": "2025-07-03",
  "keywords": ["C", "datatype", "size", "sizeof", "int", "char", "float"]
}
--->

# Sizes of Built-in Datatypes in C

> In this exercise you will learn how to determine the sizes of fundamental C datatypes using the `sizeof` operator. Furthermore we will explore how these sizes can vary across different architectures and what factors influence them.

## Introduction

In C programming, understanding the size of built‑in datatypes such as `char`, `int`, and `float` is essential for writing portable and efficient code. The C Standard (ISO/IEC 9899:2018) guarantees minimum sizes-`char` is always exactly one byte, `int` at least two bytes, and `float` at least four bytes—but it leaves the actual size implementation‑defined to match the target architecture and compiler model. A *byte* is defined as the smallest addressable unit of memory, and on most modern systems this equates to 8 bits (expressed by the macro `CHAR_BIT` in `<limits.h>`). The `sizeof(type)` operator yields the storage size in bytes as a compile‑time constant of type `size_t`.

For example, on a typical 64‑bit desktop environment you might encounter:

* `sizeof(char)` → 1 byte (always guaranteed)
* `sizeof(int)` → 4 bytes (32 bits, range −2,147,483,648..2,147,483,647)
* `sizeof(float)` → 4 bytes (32‑bit IEEE‑754 single precision)

However, on embedded or legacy systems, `int` might be 2 bytes, and `float` could occupy more or fewer bytes depending on the floating‑point hardware. When writing data‑exchange protocols, serialization code, or low‑level memory routines, incorrect assumptions about datatype sizes can lead to buffer overflows, misalignment, or loss of precision. In this exercise, you will write a small C program to discover these sizes on your system, interpret the outputs in bits, and relate them back to the underlying hardware model.

### Further Readings and Other Sources

1. [ISO/IEC 9899:2018 (C18) Standard](https://www.iso.org/standard/74528.html)
2. [C data types — Wikipedia](https://en.wikipedia.org/wiki/C_data_types#Sizes_and_precision)
3. [Derek Banas: C Programming Tutorial (YouTube)](https://www.youtube.com/watch?v=KJgsSFOSQv0)

## Tasks

Below is a fully worked, step‑by‑step guide. Follow each instruction in your editor (recommended: `vim`) and replicate the process.

**Task 1: Determining the size of `char`**

1. Open `vim` and create a file named `datatype_sizes.c`.
2. Add the following code:

   ```c
   #include <stdio.h>
   #include <limits.h>

   int main(void) {
       printf("Size of char: %zu byte(s)\n", sizeof(char));
       printf("Bits per byte (CHAR_BIT): %d\n", CHAR_BIT);
       return 0;
   }
   ```
3. Save and exit (`:wq`).
4. Compile with:

   ```bash
   gcc datatype_sizes.c -o datatype_sizes
   ```
5. Execute the program:

   ```bash
   ./datatype_sizes
   ```
6. Observe output (example):

   ```
   Size of char: 1 byte(s)
   Bits per byte (CHAR_BIT): 8
   ```

   Interpretation: a `char` occupies one byte, and each byte contains 8 bits on this system.

**Task 2: Determining the size of `int`**

1. Extend `datatype_sizes.c` by adding:

   ```c
   printf("Size of int:  %zu byte(s)\n", sizeof(int));
   ```
2. Recompile and rerun:

   ```bash
   gcc datatype_sizes.c -o datatype_sizes
   ./datatype_sizes
   ```
3. Typical output:

   ```
   Size of int:  4 byte(s)
   ```

   Interpretation: an `int` occupies 4 bytes (32 bits) on your system. The range is determined by 2ⁿ⁻¹ (signed) or 2ⁿ (unsigned).

**Task 3: Determining the size of `float`**

1. Further extend with:

   ```c
   printf("Size of float: %zu byte(s)\n", sizeof(float));
   ```
2. Recompile and run:

   ```bash
   gcc datatype_sizes.c -o datatype_sizes
   ./datatype_sizes
   ```
3. Expected output:

   ```
   Size of float: 4 byte(s)
   ```

   Interpretation: a `float` uses 4 bytes, typically following the IEEE‑754 single precision format (1 sign bit, 8 exponent bits, 23 mantissa bits).

## Questions

1. Explain why `sizeof(char)` always returns 1, regardless of architecture.
2. Describe how the C standard ensures portability even though actual sizes may vary.
3. Modify the program to print `sizeof(double)` and interpret its size and precision on your system.
4. On a hypothetical system where `int` is 2 bytes, what would be the range of `signed int` and `unsigned int`?
5. Why is it advisable to use fixed‑width types like `int32_t` or `uint16_t` in portable code?

## Advice

When exploring datatype sizes, remember that `sizeof` is a compile‑time operator and does not incur runtime overhead. Use it to guard against hard‑coded assumptions in your code. If you work across multiple platforms, incorporate `<stdint.h>` for fixed‑width integers and check `CHAR_BIT` for bit‑level calculations. Practice by experimenting on both desktop and embedded targets—this hands‑on approach builds intuition about memory models and precision limits. Keep this sheet handy whenever you begin a new C project, and revisit the C18 Standard or reputable textbooks like Kernighan & Ritchie for deeper insights.
