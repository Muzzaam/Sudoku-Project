# 🏆 Automatic Sudoku Solver: Wits Competition Winner

> **1st Place Solution for 25x25** for the Data Structures Sudoku Solver Competition (1st Year Computer Science, University of the Witwatersrand).

This repository contains my winning solution to an automatic Sudoku solver. The project was a competitive assignment for our Data Structures course, where everyone in the cohort competed to build the most computationally efficient algorithm. 

My solution was conceptually simple, but it ended up achieving the **fastest solve time for the massive 25x25 Sudoku grids.**

## 🧠 The Winning Algorithm

While many competitors opted for highly complex implementations, my approach proved that an optimized, straightforward strategy can win the day. The solver relies on two main components:

* **Highly Optimized BFS:** The backbone of the solver is a simple but heavily optimized Breadth-First Search (BFS) algorithm.
* **Logical Deduction:** Before falling back on pure search, the algorithm actively reduces the search space using well-known human Sudoku solving techniques—specifically identifying **naked singles** and **hidden singles**.

By combining these targeted deduction techniques with a lightning-fast BFS, this "easy" solution managed to completely outperform heavy-duty, highly complex algorithms like Dancing Links.

My student number is highlighted in green (there was a tie for first place)

<img width="666" height="447" alt="image" src="https://github.com/user-attachments/assets/bc7151a2-afb6-415b-989a-83d0d4b2c30e" />
