# Sudoku Solver - Open Source Project Presentation

##  Why Sudoku Solver is Harder Than Tetris (Conceptually)

While Tetris is challenging to design in terms of **real-time gameplay** and **graphics**, a Sudoku solver involves deeper algorithmic reasoning and problem solving:

| Aspect | Sudoku Solver | Tetris |
|--------|---------------|--------|
| **Algorithmic Complexity** | High: Involves backtracking, recursion, constraint propagation | Low: No advanced algorithms needed |
| **AI/Logic** | Solves an NP-complete problem | Mostly user-driven, simple AI (if any) |
| **Real-time Requirements** | None | Real-time input handling and rendering |
| **Graphics/UI** | None (text-based) | Requires UI and animations |
| **Memory Use** | Fixed structures, deterministic | Dynamic memory for piece handling |
| **Problem Solving** | Requires logic to satisfy all constraints | Piece fitting and score tracking |

**Conclusion:** While Tetris is harder to *implement interactively*, Sudoku is more intellectually demanding due to its logic-based nature and search space
## 1. Project Overview

**Name:** sudoku.c  
**Author:** Ricardo Garcia Gonzalez  
**Language:** C  
**License:** Public Domain  

This project is a simple command-line Sudoku solver implemented in C. It is designed for educational purposes, showcasing fundamental concepts like recursion, backtracking, and constraint propagation.

## 2. Objectives

- Understand the structure and logic of the solver.
- Analyze its data structures and algorithms.
- Explore possible improvements and limitations.
- Prepare for viva-style questioning.

## 3. Breadth-wise Understanding

### Main Components:

- **Candidates System:** Tracks used numbers in rows, columns, and 3x3 squares.
- **Board Structure:** 9x9 matrix of `cell` structs, with state and pointers to candidate arrays.
- **Solver Logic:** Recursive backtracking to try each possibility.
- **Input System:** Parses Sudoku board from file or standard input.

### Functions:

- `solve_board`: Recursively attempts to solve the puzzle.
- `set_cell` / `unset_cell`: Manages candidate usage and backtracking.
- `find_common_free`: Gets next valid number for a cell.
- `print_board`: Displays the final solution.

## 4. Depth-wise Analysis

### a. Approaches Taken

- **Backtracking Algorithm:** Tries all possibilities in depth-first style.
- **Constraint Checking:** Ensures no repeated numbers in row, column, or block.

### b. Data Structures Used

- `candidates`: An array (size 10) for each row, column, and square to mark numbers used.
- `cell`: Struct with flags and pointers to relevant candidates.
- `board`: Struct with all cells and candidate arrays.

### c. Tradeoffs Made

- **Efficiency vs Simplicity:** The solver doesnâ€™t use advanced heuristics like "minimum remaining value" or forward checking.
- **Memory Usage:** Uses fixed-size arrays for simplicity.
- **No GUI or advanced input validation.** Purely console-based for educational clarity.

## 5. Key Code Snippets

```c
int solve_board(struct board *b, int r, int c) {
    if (b->unset_cells == 0) {
        print_board(b);
        return 1;
    }

    while (is_set(b, r, c) && next_cell(&r, &c));
    if (is_set(b, r, c)) return 1;

    int prev = MIN_NUM, val;
    while ((val = find_common_free(..., prev)) != -1) {
        set_cell(b, r, c, val);
        if (solve_board(b, r, c)) return 1;
        unset_cell(b, r, c, val);
        prev = val + 1;
    }
    return 0;
}
```

This shows recursive backtracking in action.

## 6. Learnings and Improvements

### Learnings:

- Importance of modular design in C
- Using recursion efficiently
- Applying constraints to prune search space

### Possible Improvements:

- Add better heuristics (e.g., MRV or AC-3)
- Add unit tests
- Include GUI frontend (e.g., with SDL or ncurses) 

# Sudoku Solver in C - Line by Line Explanation

This document explains the `sudoku.c` program line by line. It is a command-line Sudoku solver using recursive backtracking.

---

## Header & Includes

```c
/*
 * sudoku.c: A simple command-line Sudoku solver in C for educational purposes.
 * Author: Ricardo Garcia Gonzalez.
 * License: Public domain code.
 */
```

Basic file info.

```c
#include <assert.h>
#include <ctype.h>
#include <stdio.h>
#include <stdlib.h>
```

Includes standard C libraries:
- `assert.h`: for debugging checks.
- `ctype.h`: character processing.
- `stdio.h`: input/output.
- `stdlib.h`: memory, file, etc.

---

## Constants and Macros

```c
#define SUBDIMENSION        (3)
#define MIN_NUM             (1)
#define MAX_NUM             (9)
#define TOTAL_NUMS          (9)
#define ARRAY_SIZE          (MIN_NUM + TOTAL_NUMS)
```

Defines Sudoku constraints:
- 3x3 squares, numbers from 1â€“9.
- `ARRAY_SIZE = 10` to make indexing easier (ignore index 0).

```c
#ifdef ASSERT
#define assert_(A) assert(A)
#else
#define assert_(A)
#endif
```

Optional assertion macro (enabled only if `ASSERT` is defined).

---

## Candidates Array

```c
typedef int candidates[ARRAY_SIZE];
```

Defines an array to track used numbers. `candidates[1..9]` are set to 0 or 1.

```c
void init_candidates(candidates *c)
```

Sets all candidates to 0 (available).

```c
void use_candidate(candidates *cp, int num)
```

Marks `num` as used (1).

```c
void restore_candidate(candidates *cp, int num)
```

Marks `num` as available again (0).

---

## Cell and Board Structures

```c
struct cell {
  int has_value;
  int value;
  candidates *row_candidates;
  candidates *col_candidates;
  candidates *square_candidates;
};
```

Each cell:
- `has_value`: true if number is set.
- Points to candidate arrays of its row, column, and square.

```c
struct board {
  int unset_cells;
  struct cell cells[ARRAY_SIZE][ARRAY_SIZE];
  candidates rows[ARRAY_SIZE];
  candidates columns[ARRAY_SIZE];
  candidates squares[ARRAY_SIZE];
};
```

The board:
- Tracks how many cells are unset.
- Has candidate arrays for all rows, columns, and squares.

---

## Square Helper

```c
int square(int row, int col)
```

Returns the 1â€“9 index of the 3x3 square that a cell belongs to.

---

## Board Initialization

```c
void init_board(struct board *b)
```

Initializes an empty board:
- Resets all candidates.
- Sets cell pointers to row/column/square candidate arrays.

---

## Finding Valid Candidates

```c
int find_common_free(candidates *r, candidates *c, candidates *s, int atleast)
```

Returns the smallest number â‰¥ `atleast` thatâ€™s free in all three candidate arrays.

---

## Set and Unset Cells

```c
void set_cell(struct board *b, int r, int c, int val)
```

Assigns a number to a cell and marks candidates as used.

```c
void unset_cell(struct board *b, int r, int c, int val)
```

Removes a number from a cell and restores candidates.

```c
int is_set(struct board *b, int r, int c)
```

Returns 1 if the cell has a value; else 0.

---

## Cell Navigation

```c
int following(int num)
```

Returns the next number in sequence (1 â†’ 2 â†’ ... â†’ 9 â†’ 1).

```c
int next_cell(int *r, int *c)
```

Moves to the next cell in row-major order. Returns 0 if at last cell.

---

## Print the Board

```c
void print_board(struct board *b)
```

Prints the board values row by row.

---

## Recursive Backtracking Solver

```c
int solve_board(struct board *b, int r, int c)
```

Recursive function:
1. Base case: solved if `unset_cells == 0`.
2. Find next unset cell.
3. Try all valid numbers.
4. Set number, recurse. If fails, backtrack.
5. Returns 1 if solved, else 0.

---

## Reading Input

```c
void read_board(FILE *f, struct board *b)
```

Reads a board from a file or stdin.
- Digits `1â€“9`: set value.
- Dots (`.`): leave blank.
- Skips other characters.

---

## Main Function

```c
int main(int argc, char *argv[])
```

- If a filename is passed, opens it.
- Otherwise reads from stdin.
- Initializes board and tries to solve.
- Prints board if solved, else shows error.

```c
return (ret ? 0 : 3);
```

Return `0` on success, `3` on failure.

---

## Example Usage

```bash
$ gcc sudoku.c -o sudoku
$ ./sudoku puzzle.txt
```

Or use standard input:

```bash
$ ./sudoku
..3.2.6..
9..3.5..1
..18.64..
..81.29..
7.......8
..67.82..
..26.95..
8..2.3..9
..5.1.3..
```

---

## Summary

This simple Sudoku solver:
- Uses backtracking.
- Optimizes with candidate pruning.
- Solves from file or stdin.
- Good for learning recursion and constraint logic.
