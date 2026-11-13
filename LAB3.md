# Lab 3: A* Search for the 8-Puzzle

## GOOGLE COLAB LINK FOR LAB 3: https://colab.research.google.com/drive/1jRZr_lXW9oeOsIwxwqacexahfJtyn_Qs?usp=sharing

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
### Solution:
```python

def misplaced_tiles(state):
    """
    Count tiles that are not in their goal position.
    The blank (0) does NOT count as a misplaced tile.
    """
    count = 0

    for i in range(9):
        if state[i] != 0 and state[i] != GOAL_STATE[i]:
            count += 1

    return count

```
### Sample:
```python
test_state = (1, 2, 3, 4, 5, 6, 7, 0, 8)
print(misplaced_tiles(test_state))
```
### Output:
```text
1
```
### Sample 2:
```python
test_state = (1, 2, 3, 4, 0, 5, 6, 7, 8)
print(misplaced_tiles(test_state))
```
### Output:
```text
4
```
### 💻 Cell 7 (Code) — Task 2.2: Manhattan Distance **(TODO)**

```python
def manhattan_distance(state):
    """
    Sum of the horizontal + vertical distance of every tile
    (excluding the blank) from its goal position.
    """
    # TODO: implement
    # Hint: for each tile value, find its current (row, col) and its
    # goal (row, col), and sum abs(row_diff) + abs(col_diff)
    pass
```

### Solution:
```python
def manhattan_distance(state):
    """
    Sum the horizontal + vertical distance of every tile
    (excluding the blank) from its goal position.
    """
    distance = 0

    for i in range(9):
        tile = state[i]

        if tile != 0:
            current_row, current_col = divmod(i, 3)
            goal_row, goal_col = divmod(tile - 1, 3)

            distance += abs(current_row - goal_row) + abs(current_col - goal_col)

    return distance
```
### Sample:
```python
test_state = (1, 2, 3, 4, 5, 6, 7, 0, 8)
print(manhattan_distance(test_state))
```
### Output:
```text
1
```
### Sample 2:
```python
test_state = (1, 2, 0, 3, 4, 5, 6,  7, 8)
print(manhattan_distance(test_state))
```
### Output:
```text
10
```
### 💻 Cell 8 (Code) — Task 2.3: sanity check, run after completing Cells 6–7

```python
test_state = (1, 2, 3, 4, 5, 6, 7, 0, 8)  # one move from goal

assert misplaced_tiles(test_state) <= 1, "Misplaced tiles should be ≤ actual distance"
assert manhattan_distance(test_state) <= 1, "Manhattan distance should be ≤ actual distance"
print("Cell 8 checkpoint passed.")
```
###  Output:
```text
Cell 8 checkpoint passed.
```
## Q&A

### Q. Why must h(n) = 0 whenever state == GOAL_STATE, for both heuristics? What would break in A* if this weren't true?

Both heuristics must return 0 at the goal state because the puzzle is already solved. No tiles need to move, so the estimated remaining cost must be zero. <br>

**Why must h(n) = 0 at the goal?** <br>
For the 8-Puzzle: <br>
```python
GOAL_STATE = (1, 2, 3, 4, 5, 6, 7, 8, 0)
```


At this state:
- **Misplaced Tiles (H1):** No numbered tiles are misplaced, so h(n) = 0.
- **Manhattan Distance (H2):** Every numbered tile is already in its correct position, so h(n) = 0. <br>

**What happens if h(n) is not zero?** <br>
A* uses this formula: <br>
```text
f(n) = g(n) + h(n)
```
Where:
- `g(n)` = Actual cost from the start state to the current state.
- `h(n)` = Estimated cost from the current state to the goal.
- `f(n)` = Estimated total cost of the solution through that state. <br>

If the goal state's heuristic is greater than zero, A* incorrectly estimates that some cost is still required even though the puzzle is solved.

This can cause:
- Incorrect cost calculations.
- The goal state being given a higher priority than necessary.
- Loss of the optimality guarantee if the heuristic also overestimates the true remaining cost.

Both heuristics must return zero at the goal because there is no remaining distance to travel.

This property is called **goal normalization**. It is also essential for admissibility: a heuristic must never overestimate the actual remaining cost.

---
### Objectives of Section 2
To implement and test two admissible heuristic functions for the 8-Puzzle problem: Misplaced Tiles (H1) and Manhattan Distance (H2). These heuristics estimate the remaining cost to reach the goal state without overestimating the actual minimum cost.

### Implementation of Heuristics
- **Misplaced Tiles (H1):** Counts the number of numbered tiles that are not in their correct positions, excluding the blank space (0).
- **Manhattan Distance (H2):** Calculates the total horizontal and vertical distance of all numbered tiles from their goal positions, excluding the blank space.
- **Sanity Check:** Used assertions to verify that both heuristics do not exceed the actual distance for a state one move away from the goal.

### Testing and Validation
Tested both heuristic functions using a puzzle state that requires only one move to reach the goal.

**Expected Results:**
- **Misplaced Tiles (H1):** 1 
- **Manhattan Distance (H2):** 1
- **Checkpoint:** Cell 8 checkpoint passed.

### Key Takeaways from Section 2
- Understanding heuristic functions in Artificial Intelligence.
- Learning the concept of admissible heuristics.
- Counting misplaced tiles and calculating Manhattan Distance.
- Using Python functions to estimate the remaining cost to the goal.
- Understanding why heuristics must return 0 at the goal state.
- Using assertions to test and verify heuristic correctness.
- Understanding how admissible heuristics help A* find an optimal solution.

### Discussion
Both heuristics return 0 at the goal state because no moves are required to solve an already completed puzzle. If the heuristic returned a positive value, it would incorrectly estimate the remaining cost. This could affect A*'s evaluation and potentially compromise its optimality guarantee.

### Conclusion
Successfully implemented and tested both heuristic functions. This section helped us understand how AI heuristics estimate the distance to a goal and guide informed search algorithms such as A*.

---
---

## Section 3: A* Search Implementation

### 📝 Cell 9 (Markdown)
```
## 3. A* Search
Priority queue ordered by f(n) = g(n) + h(n), using heapq.
```

### 💻 Cell 10 (Code) — Task 3.1: A* search **(TODO)**

```python
def a_star_search(start_state, heuristic_fn):
    """
    Returns a dict with:
        "path": list of actions from start to goal
        "cost": number of moves in the solution
        "nodes_expanded": total number of nodes popped from the frontier
    """
    counter = itertools.count()  # tie-breaker for equal f-values
    frontier = []
    heapq.heappush(frontier, (heuristic_fn(start_state), next(counter), 0, start_state, []))
    # entry format: (f, tie_breaker, g, state, path_of_actions)

    best_g = {start_state: 0}
    nodes_expanded = 0

    while frontier:
        f, _, g, state, path = heapq.heappop(frontier)
        nodes_expanded += 1

        if is_goal(state):
            return {"path": path, "cost": g, "nodes_expanded": nodes_expanded}

        # TODO: skip this node if we've already found a better g for this state
        # (this handles the "explored with lower g" case from the A* pseudocode)

        for neighbor_state, action in get_neighbors(state):
            new_g = g + 1  # each move costs 1

            # TODO: if neighbor_state is new OR new_g is better than best_g.get(neighbor_state, inf):
            #   - update best_g[neighbor_state]
            #   - compute new_f = new_g + heuristic_fn(neighbor_state)
            #   - push (new_f, next(counter), new_g, neighbor_state, path + [action]) onto frontier
            pass

    return {"path": None, "cost": None, "nodes_expanded": nodes_expanded}  # no solution found
```
### Solution:
```python
def a_star_search(start_state, heuristic_fn):
    """
    Returns a dict with:
        "path": list of actions from start to goal
        "cost": number of moves in the solution
        "nodes_expanded": total number of nodes popped from the frontier
    """
    counter = itertools.count()
    frontier = []
    heapq.heappush(frontier, (heuristic_fn(start_state), next(counter), 0, start_state, []))

    best_g = {start_state: 0}
    nodes_expanded = 0

    while frontier:
        f, _, g, state, path = heapq.heappop(frontier)
        nodes_expanded += 1

        # Skip if a better path to this state has already been found
        if g > best_g.get(state, float("inf")):
            continue

        if is_goal(state):
            return {"path": path, "cost": g, "nodes_expanded": nodes_expanded}

        for neighbor_state, action in get_neighbors(state):
            new_g = g + 1

            # Check whether this is a new or better path
            if new_g < best_g.get(neighbor_state, float("inf")):
                best_g[neighbor_state] = new_g
                new_f = new_g + heuristic_fn(neighbor_state)

                heapq.heappush(
                    frontier,
                    (new_f, next(counter), new_g, neighbor_state, path + [action])
                )

    return {"path": None, "cost": None, "nodes_expanded": nodes_expanded}
```
### 💻 Cell 11 (Code) — quick smoke test, run after completing Cell 10

```python
easy_state = (1, 2, 3, 4, 5, 6, 7, 0, 8)  # one move from goal
result = a_star_search(easy_state, manhattan_distance)
print(result)
assert result["cost"] == 1, f"Expected cost 1, got {result['cost']}"
print("Cell 11 checkpoint passed.")
```
### Output:
```text
{'path': ['Right'], 'cost': 1, 'nodes_expanded': 2}
Cell 11 checkpoint passed.
```
## Q&A

**Why Do We Use a Tie-Breaker Counter?**

The tie-breaker counter is used to give each entry in the priority queue a unique number. When two entries have the same priority value, Python uses this number to decide which one comes first. It prevents Python from unnecessarily comparing states and paths and ensures stable ordering. It is a Python implementation detail, not a part of the A* search algorithm itself.

---
### Objectives of Section 3

- Implement the A* search algorithm to solve the 8-Puzzle.
- Use a priority queue to select the most promising state.
- Calculate the evaluation function using f(n) = g(n) + h(n).
- Track the best-known path cost for each state.
- Find an optimal solution using an admissible heuristic.

### Key Takeaways from Section 3

- Understanding informed search algorithms.
- Implementing a priority queue using Python's `heapq`.
- Understanding the relationship between path cost and heuristic cost.
- Avoiding unnecessary exploration of outdated paths.
- Tracking explored states and solution paths.
- Understanding how A* finds an optimal solution.

---
---

## Section 4: Testing

### 📝 Cell 12 (Markdown)
```
## 4. Testing
Generate solvable scrambles of increasing difficulty, then compare heuristics.
```

### 💻 Cell 13 (Code) — Task 4.1: solvability check

```python
def is_solvable(state):
    """
    A state is solvable iff the number of inversions (pairs of tiles,
    ignoring the blank, that are out of order relative to the goal)
    is even.
    """
    tiles = [t for t in state if t != 0]
    inversions = 0
    for i in range(len(tiles)):
        for j in range(i + 1, len(tiles)):
            if tiles[i] > tiles[j]:
                inversions += 1
    return inversions % 2 == 0
```

### 💻 Cell 14 (Code) — Task 4.2: scramble generator

```python
def generate_scramble(num_moves, seed=None):
    if seed is not None:
        random.seed(seed)
    state = GOAL_STATE
    for _ in range(num_moves):
        neighbors = get_neighbors(state)
        state, _ = random.choice(neighbors)
    return state
```

### 💻 Cell 15 (Code) — Task 4.3: run the comparison

```python
def run_comparison(num_moves_list, trials_per_level=5):
    results = []
    for num_moves in num_moves_list:
        for trial in range(trials_per_level):
            start = generate_scramble(num_moves, seed=trial * 1000 + num_moves)

            result_misplaced = a_star_search(start, misplaced_tiles)
            result_manhattan = a_star_search(start, manhattan_distance)

            results.append({
                "scramble_depth": num_moves,
                "trial": trial,
                "misplaced_nodes": result_misplaced["nodes_expanded"],
                "manhattan_nodes": result_manhattan["nodes_expanded"],
                "solution_cost": result_misplaced["cost"],  # should match manhattan's cost too
            })
    return results

# Suggested test range: start small, since Misplaced Tiles gets very slow
# on harder puzzles.
results = run_comparison(num_moves_list=[5, 10, 15, 20], trials_per_level=5)

for r in results:
    print(r)
```
### Output:
| Scramble Depth | Trial | Misplaced Tiles Nodes | Manhattan Distance Nodes | Solution Cost |
|---|---|---:|---:|---:|
| 5 | 1 | 2 | 2 | 1 |
| 5 | 2 | 2 | 2 | 1 |
| 5 | 3 | 6 | 6 | 5 |
| 5 | 4 | 4 | 4 | 3 |
| 5 | 5 | 4 | 4 | 3 |
| 10 | 1 | 3 | 3 | 2 |
| 10 | 2 | 5 | 5 | 4 |
| 10 | 3 | 6 | 5 | 4 |
| 10 | 4 | 3 | 3 | 2 |
| 10 | 5 | 5 | 5 | 4 |
| 15 | 1 | 4 | 4 | 3 |
| 15 | 2 | 6 | 6 | 5 |
| 15 | 3 | 4 | 4 | 3 |
| 15 | 4 | 6 | 6 | 5 |
| 15 | 5 | 8 | 7 | 5 |
| 20 | 1 | 3 | 3 | 2 |
| 20 | 2 | 9 | 9 | 8 |
| 20 | 3 | 16 | 10 | 8 |
| 20 | 4 | 24 | 18 | 8 |
| 20 | 5 | 16 | 12 | 6 |

### 💻 Cell 16 (Code) — table and chart (uses `pandas` and `matplotlib`, both preinstalled in Colab)

```python
import pandas as pd
import matplotlib.pyplot as plt

df = pd.DataFrame(results)
summary = df.groupby("scramble_depth")[["misplaced_nodes", "manhattan_nodes"]].mean()
print(summary)

summary.plot(marker="o")
plt.xlabel("Scramble Depth (moves from goal)")
plt.ylabel("Average Nodes Expanded")
plt.title("A* Node Expansion: Misplaced Tiles vs. Manhattan Distance")
plt.legend(["Misplaced Tiles", "Manhattan Distance"])
plt.show()
```
### Output:
 <img width="691" height="185" alt="image" src="https://github.com/user-attachments/assets/ac83d5d5-5268-4900-9f68-692f54789d6c" />
 <img width="736" height="479" alt="image" src="https://github.com/user-attachments/assets/6b3f383f-a9ce-47af-8381-21ec9caf46e6" />

---
---
## Section 5: Analysis
### Q&A

**Q. At what scramble depth does the gap between the two heuristics
     become clearly visible? Why does the gap widen as depth increases
     rather than staying constant?**

The gap usually becomes more noticeable at higher scramble depths, particularly around 15–20 moves, depending on the test results. Manhattan Distance provides a more accurate estimate because it considers how far each tile is from its goal position, while Misplaced Tiles only counts incorrect tiles. As puzzle complexity increases, this difference becomes more significant, causing Misplaced Tiles to explore more nodes.

 **Q. Manhattan Distance is provably ≥ Misplaced Tiles for every state.
     Using the lecture's definition of admissibility, explain why a
     higher admissible heuristic is not a violation of admissibility,
     and why it leads to fewer node expansions.**

An admissible heuristic never overestimates the actual minimum cost to reach the goal. Manhattan Distance is greater than or equal to Misplaced Tiles, but it still does not overestimate the actual remaining moves. Therefore, it remains admissible. Since Manhattan Distance provides a more accurate estimate, A* can make better decisions about which states to explore, usually resulting in fewer node expansions.

**Q. Try increasing scramble depth until Misplaced Tiles
     becomes impractically slow. Does Manhattan Distance hit the same
     wall at the same depth?**

No, not usually at the same depth. Manhattan Distance generally performs better because it provides more useful information about the remaining cost. Misplaced Tiles may become impractically slow at a lower scramble depth because it often expands more nodes. However, Manhattan Distance can also become slow as puzzle complexity increases.
