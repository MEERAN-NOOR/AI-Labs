# Lab 3: A* Search for the 8-Puzzle

## Section 1: Problem Representation

### 📝 Cell 1 (Markdown)
```
## 1. Problem Representation
State: tuple of 9 ints, 0 = blank, read left-to-right top-to-bottom.
Goal state: (1, 2, 3, 4, 5, 6, 7, 8, 0)
```

### 💻 Cell 2 (Code) — imports and constants

```python
import heapq
import itertools
import random

GOAL_STATE = (1, 2, 3, 4, 5, 6, 7, 8, 0)
```

### 💻 Cell 3 (Code) — Task 1.1: state helper functions

```python
def get_blank_position(state):
    """Return the index (0-8) of the blank tile (0) in the state tuple."""
    return state.index(0)

def get_neighbors(state):
    """
    Return a list of (new_state, action) pairs reachable from `state`
    by sliding one tile into the blank space.

    Actions should be one of: "Up", "Down", "Left", "Right"
    (direction the BLANK moves).
    """
    neighbors = []
    blank = get_blank_position(state)
    row, col = divmod(blank, 3)

    moves = {
        "Up":    (row - 1, col),
        "Down":  (row + 1, col),
        "Left":  (row, col - 1),
        "Right": (row, col + 1),
    }

    for action, (new_row, new_col) in moves.items():
        if 0 <= new_row < 3 and 0 <= new_col < 3:
            new_blank = new_row * 3 + new_col
            new_state = list(state)
            new_state[blank], new_state[new_blank] = new_state[new_blank], new_state[blank]
            neighbors.append((tuple(new_state), action))

    return neighbors

def is_goal(state):
    return state == GOAL_STATE
```

### 💻 Cell 4 (Code) — checkpoint, run immediately after Cell 3

```python
# Checkpoint: blank is in the bottom-right corner of GOAL_STATE,
# so only Up and Left should be valid moves.
neighbors = get_neighbors(GOAL_STATE)
print(neighbors)
assert len(neighbors) == 2, f"Expected 2 neighbors, got {len(neighbors)}"
print("Cell 3 checkpoint passed.")
```

## Output:
```text
[((1, 2, 3, 4, 5, 0, 7, 8, 6), 'Up'), ((1, 2, 3, 4, 5, 6, 7, 0, 8), 'Left')]
Cell 3 checkpoint passed.
```

### Objective

To represent the 8-Puzzle problem in Python and implement basic functions for identifying the blank tile, generating valid moves, and checking the goal state.

### Problem Description

The 8-Puzzle consists of a 3×3 board with eight numbered tiles and one blank space (0). The goal is to arrange the tiles in the correct order by sliding adjacent tiles into the blank space.

Goal State:
```text
1  2  3
4  5  6
7  8  _
```
### Implementation

Three helper functions were implemented:

`get_blank_position(state)` – Finds the index of the blank tile.

`get_neighbors(state)` – Generates all valid neighboring states by moving the blank Up, Down, Left, or Right.

`is_goal(state)` – Checks whether the current state matches the goal state.

The puzzle is represented as a tuple of nine integers, with 0 representing the blank space.

### Testing

The goal state was used to test the neighboring-state function. Since the blank is in the bottom-right corner, only Up and Left movements are valid.

**Expected Result:** 2 valid neighboring states.
```text
Cell 3 checkpoint passed.
```
### Key Takeaways

- Representing puzzle states using Python tuples.

- Finding elements using the index() method.

- Converting indexes into row and column positions using divmod().

- Generating valid moves while checking board boundaries.

- Creating new states by swapping elements.

- Using assertions to test program correctness.

---

## Section 2: Heuristics

### 📝 Cell 5 (Markdown)
```
## 2. Heuristics
Both heuristics must be admissible (never overestimate true cost)
for A* to guarantee an optimal solution.
```

### 💻 Cell 6 (Code) — Task 2.1: Misplaced Tiles **(TODO)**

```python
def misplaced_tiles(state):
    """
    Count tiles that are not in their goal position.
    The blank (0) does NOT count as a misplaced tile.
    """
    # TODO: implement
    # Hint: compare state[i] to GOAL_STATE[i] for each i, skip where state[i] == 0
    pass
```
