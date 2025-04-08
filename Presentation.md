# Sudoku Solver - Open Source Project Presentation

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
- Use more optimized data structures like bitsets

## 7. Contributions (Optional)

- [ ] Bug raised / Issue reported
- [ ] Pull request sent
- [ ] Response from maintainers

## 8. References

- [Sudoku Solver Source](https://web.archive.org/web/20220210032019/http://ricardo.ecn.wfu.edu/sudoku/)
- [Sudoku Solving Algorithms](https://www.geeksforgeeks.org/sudoku-backtracking-7/)