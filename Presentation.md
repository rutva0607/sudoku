# Sudoku Solver - Open Source Project Presentation
## Presentation Made By
- **Rutva Mehta**
- **Khush Hingrajiya**
- **Manthan Kanetiya**
- **Vikas Bhabhor**

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

# 3 Function Summary

This document provides a summary of all the functions used in the Sudoku solver written in C.

---

## 🔢 Candidate Functions
Functions that manage which numbers are available in rows, columns, and 3×3 squares.

### `init_candidates(candidates *c)`
- Initializes all numbers (1-9) as available (0).

### `use_candidate(candidates *cp, int num)`
- Marks a number as used (1) in the candidate list.

### `restore_candidate(candidates *cp, int num)`
- Marks a number as unused (0) again in the candidate list.

---

## 🔲 Board and Cell Functions
Functions that manage the board, cells, and updating cell values.

### `square(int row, int col)`
- Returns the index (1-9) of the 3×3 square that the cell belongs to.

### `init_board(struct board *b)`
- Initializes the entire board.
- Sets all cells to empty and links each cell to its row, column, and square candidates.

### `find_common_free(...)`
- Finds the smallest number (starting from `atleast`) that is free in a cell's row, column, and square.
- Returns `-1` if none are available.

### `set_cell(struct board *b, int r, int c, int val)`
- Places a value in a cell.
- Updates the candidate lists accordingly.

### `unset_cell(struct board *b, int r, int c, int val)`
- Removes a value from a cell (used for backtracking).
- Restores the candidate lists.

### `is_set(struct board *b, int r, int c)`
- Checks if a cell already has a value.
- Returns `1` if set, otherwise `0`.

---

## ♻️ Traversal Helpers
Functions to navigate through the board cells.

### `following(int num)`
- Returns the next number after `num`, wrapping from 9 back to 1.

### `next_cell(int *r, int *c)`
- Moves to the next cell (left to right, top to bottom).
- Returns `1` if successful, `0` if at the last cell.

---

## 📋 I/O Functions
Functions to print or read the Sudoku puzzle.

### `print_board(struct board *b)`
- Prints the current state of the board.

### `read_board(FILE *f, struct board *b)`
- Reads a puzzle from a file or input.
- Digits are fixed values; `.` is used for empty cells.

---

## 🧮 Solver Logic
The main logic for solving the Sudoku puzzle.

### `solve_board(struct board *b, int r, int c)`
- Solves the Sudoku using recursive backtracking.
- Tries all valid values in each empty cell.
- Returns `1` if solved, otherwise `0`.

---

## 🚀 Main Function
Program entry point.

### `main(int argc, char *argv[])`
- Reads input from file or stdin.
- Initializes and reads the board.
- Calls `solve_board()`.
- Prints the solution or an error message.

## 4. Depth-wise Analysis

### a. Approaches Taken

- **Backtracking Algorithm:** Tries all possibilities in depth-first style.
- **Constraint Checking:** Ensures no repeated numbers in row, column, or block.

### b. Data Structures Used

- `candidates`: An array (size 10) for each row, column, and square to mark numbers used.
- `cell`: Struct with flags and pointers to relevant candidates.
- `board`: Struct with all cells and candidate arrays.

### c. Tradeoffs Made

- **Efficiency vs Simplicity:** The solver doesn't use advanced heuristics like "minimum remaining value" or forward checking.
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

## 7. Pic of one of the Hard Puzzle solved

https://drive.google.com/file/d/1xYUF2wlbJJyV8TFVPMQr6wSb_Fn25dBO/view?usp=drivesdk

## 8. Answers of the Questions which arise in our Journey

### Q1: What algorithm is used in this Sudoku solver?  
**A:** It uses **backtracking**, a recursive technique to try filling the board by checking constraints and backtracking if a contradiction is found.

### Q2: What is the purpose of the `candidates` array?  
**A:** The `candidates` array tracks which numbers have already been used in a row, column, or square to avoid illegal placements.

### Q3: What does `find_common_free` do?  
**A:** It returns the next available number (starting from a given minimum) that is not yet used in the corresponding row, column, and 3x3 square.

### Q4: How does the program know when a board is solved?  
**A:** It checks if `unset_cells == 0`. If true, it prints the board and returns success.

### Q5: What’s the role of `set_cell` and `unset_cell`?  
**A:** `set_cell` fills a cell and marks the number as used in the relevant candidate arrays. `unset_cell` reverses this during backtracking.

### Q6: How does the board input work?  
**A:** It reads characters from a file or stdin. Digits represent values, and dots (.) indicate empty cells. Others are ignored.

### Q7: Why are there `assert_` macros in the code?  
**A:** These are conditional assertions used for debugging. They ensure function parameters and logic are valid during development.

### Q8: How are rows, columns, and 3x3 squares managed separately?  
**A:** Each cell has pointers to its row, column, and square candidate arrays, allowing independent tracking of constraints.

### Q9: Why is `ARRAY_SIZE` set to 10 instead of 9?  
**A:** To simplify indexing from 1 to 9 directly (1-based index), avoiding 0-based offset calculations.

### Q10: How could this code be optimized further?  
**A:** By adding constraint propagation, better cell selection heuristics (like Minimum Remaining Value), or switching to a more advanced algorithm like Dancing Links (DLX).

