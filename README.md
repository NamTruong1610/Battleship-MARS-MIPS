# Battleship Game in MIPS Assembly

This repository contains a complete MIPS assembly language implementation of the classic Battleship game—adapted for a 7x7 grid. The program was developed as an assignment for the Computer Architecture Lab course at Ho Chi Minh City University of Technology. It is designed to run on the MARS MIPS simulator.

## Overview

The Battleship game is divided into two main phases:

1. **Preparation Phase (Ship Placement):**  
   Each player is prompted to input the coordinates for a fixed set of ships:
   - **1 ship of size 4x1**
   - **2 ships of size 3x1**
   - **3 ships of size 2x1**

   The program validates each set of coordinates by checking:
   - **Bounds:** Each coordinate must be within the 7x7 grid (i.e., row and column values are between 0 and 6).
   - **Alignment:** The ship must be placed either horizontally or vertically (no diagonal placements).
   - **Length:** The distance between the bow and stern is checked by either subtracting or adding the appropriate constant (3 for 4x1, 2 for 3x1, and 1 for 2x1 ships) to verify the ship’s size.
   - **Overlap:** Before storing a ship on the board, the program checks cell by cell to ensure that no cell is already occupied.

2. **Gameplay Phase (Attacking):**  
   After both players have successfully placed their ships, they take turns firing shots by entering attack coordinates. For each turn:
   - The input coordinates are validated (using similar bounds-checking as in the setup phase).
   - The program calculates the corresponding index on the 7x7 board by multiplying the row by 7 and adding the column.
   - If the targeted cell contains a '1' (indicating part of a ship), the program announces a "HIT!", updates that cell to '0' (to mark that it is no longer occupied), and then calls the `checkBoard` function.
   - The `checkBoard` function loops through the board to determine whether any ships remain. If the board is completely cleared (all cells are '0'), the attacking player wins, and the game terminates.
   - If the board still contains a ship ('1'), the turn passes to the other player.

## Key Functions and Algorithms

### 1. **`printBoard` Function**
This procedure is responsible for displaying the current state of a player's board.  
- The board is stored as a contiguous block of bytes representing a 7x7 grid.
- A loop is used to print each cell. A counter is maintained to insert a newline character after every seventh cell, ensuring the board is formatted as a grid.

### 2. **`checkBoard` Function**
This function determines if a board is empty (i.e., if all ships have been sunk).  
- It traverses every cell of the board using a loop.
- A boolean flag (stored in register `$t3`) is initially set to 1.
- For each cell, if a '1' is encountered (indicating an undamaged ship part), the flag is decremented (set to 0) and the loop terminates early.
- The final value of the flag is returned in register `$v0` (1 for an empty board, 0 otherwise).

### 3. **Input Validation and Ship Placement**
For each ship type (4x1, 3x1, 2x1), the program:
- Prompts the player for the coordinates of the bow and stern.
- Checks if the inputs are within bounds and whether they form a valid horizontal or vertical line.
- Performs arithmetic checks (subtracting or adding constants) to verify the ship’s length.
- If the coordinates are invalid (out-of-bound, diagonal, incorrect length, or overlapping with an existing ship), the program loops back to request new inputs.
- Once validated, the program computes the starting index (using the formula: _index = row × 7 + column_) and uses a loop to store the ship by writing '1's into the corresponding consecutive cells (incrementing by 1 for horizontal placements or by 7 for vertical placements).

### 4. **Gameplay Loop**
During the battle phase:
- Players alternate turns entering attack coordinates.
- The same coordinate-to-index conversion is used to target a cell on the opponent’s board.
- If a ship is hit (cell value '1'), the program prints "HIT!" and updates the board.
- After every successful hit, the program checks if the entire board is empty by calling `checkBoard`. If it is, the current player is declared the winner.
- The game continues until one player’s board is fully cleared.

## Data Section

The `.data` section of the assembly code includes:
- Predefined strings for prompts and messages (e.g., "HIT!", ship coordinate prompts, board announcement messages).
- Memory blocks (`P1_BOARD` and `P2_BOARD`) that represent each player's board as a sequence of 49 bytes (7x7 grid) initialized with the character '0'.

## Running the Program

### Prerequisites

- **MARS MIPS Simulator:**  
  Download and install the [MARS MIPS simulator](http://courses.missouristate.edu/kenvollmar/mars/) to run this assembly program.

### Instructions

1. **Open MARS:** Launch the MARS simulator.
2. **Load the Source File:** Open `Battleship.asm` in MARS.
3. **Assemble the Code:** Click the "Assemble" button to convert the assembly code into machine code.
4. **Run the Program:** Click the "Go" button (or press F5) to start the simulation. Follow the on-screen prompts:
   - Enter ship coordinates during the setup phase.
   - Enter attack coordinates during the gameplay phase.
5. **Game Conclusion:** The program will announce the winner when one player's board contains only '0's.

## Report and Additional Explanation

A detailed report (see [Assignment_Report.pdf](https://example.com)) provides further insight into the design and implementation of this Battleship game. Key points from the report include:

- **Algorithmic Approach:**  
  The report explains how the validity of ship placement is determined by checking both the numerical bounds and the geometric constraints (horizontal/vertical alignment and correct length).  
- **Function Implementation:**  
  Detailed descriptions are provided for the `checkBoard` and `printBoard` functions, including how loops and conditional branches are used to navigate and modify the board.
- **User Input Handling:**  
  The process by which the program validates each input, including repetitive prompting until a valid set of coordinates is received, is outlined.
- **Ship Storing Mechanism:**  
  The report breaks down the arithmetic used to convert 2D coordinates into a linear index for the board array and describes the loop logic that stores ship data.

This additional explanation ensures that anyone reviewing the code or the report can understand the underlying design decisions and the flow of the game logic.

## Evaluation Criteria

The assignment was evaluated based on:
- **Friendly Interface (2 points):**  
  The user is guided through the game via clear prompts and board displays.
- **Application Implementation (6 points):**  
  The Battleship game meets the specified requirements and handles edge cases (e.g., invalid inputs, overlapping ships) gracefully.
- **Report (2 points):**  
  The accompanying report clearly explains the algorithms and design choices used in the implementation.

## References

- [Wikipedia - Battleship (game)](https://en.wikipedia.org/wiki/Battleship_(game))
- [Assignment Report](https://example.com) – Detailed explanation of the algorithm and code implementation (see [49] [oai_citation:0‡Assignment_Report.pdf](file-service://file-Dwf18x3CBAAUaDSBjFaFvW)).

---

Feel free to update or extend this README as needed. Enjoy playing Battleship in MIPS Assembly!
