# Lab 4: Hill-Climbing Search with the N-Queens Problem

## TASK 1
Create the state and print it
```python
state = [1, 3, 0, 2]
print("State:", state)
```
Output:
```text
State: [1, 3, 0, 2]
```

## Generating a Random Initial State

Hill-Climbing starts from an initial state. We will generate a random row position for the queen in each column.

### Code Cell

```python
import random

def random_state(n):
    return [random.randrange(n) for _ in range(n)]

state = random_state(8)

print("Random initial state:", state)
```

Run the cell several times. You may obtain a different state each time.

For example:

```text
Random initial state: [2, 5, 1, 7, 0, 3, 6, 4]
```

A randomly generated state is not necessarily a solution. Some queens may attack each other.

**Run 1 output:**
```text
Random initial state: [0, 2, 6, 6, 6, 7, 0, 2]
```
**Run 2 output:**
```text
Random initial state: [7, 2, 3, 3, 6, 3, 1, 5]
```
**Run 3 output:**
```text
Random initial state: [7, 3, 5, 1, 4, 5, 7, 5] 
```

## 7. Generating Neighboring States

### Code Cell

```python
def get_neighbors(state):
    neighbors = []
    n = len(state)

    for col in range(n):
        for row in range(n):

            if row != state[col]:
                neighbor = state.copy()
                neighbor[col] = row
                neighbors.append(neighbor)

    return neighbors
```

**Test the function:**

```python
state = [1, 3, 0, 2]

neighbors = get_neighbors(state)

print("Number of neighbors:", len(neighbors))
print("First five neighbors:")

for neighbor in neighbors[:5]:
    print(neighbor)
```

**Output**
```text
Number of neighbors: 12
First five neighbors:
[0, 3, 0, 2]
[2, 3, 0, 2]
[3, 3, 0, 2]
[1, 0, 0, 2]
[1, 1, 0, 2]
```

## 9. Running Hill-Climbing

### Code Cell

```python
solution, scores = hill_climbing(8)

print("Final state:", solution)
print("Final conflicts:", count_conflicts(solution))
print("Final evaluation score:", evaluation(solution))
print("Number of iterations:", len(scores))

if count_conflicts(solution) == 0:
    print("A valid solution was found.")
else:
    print("Hill-Climbing stopped without finding a solution.")
```
### Run 1 output:
```text
Final state: [6, 3, 5, 2, 4, 0, 7, 5]
Final conflicts: 2
Final evaluation score: -2
Number of iterations: 3
Hill-Climbing stopped without finding a solution.
```
### Run 2 output:
```text
Final state: [2, 7, 3, 6, 0, 5, 1, 4]
Final conflicts: 0
Final evaluation score: 0
Number of iterations: 4
A valid solution was found.
```
### Run 3 output:
```text
Final state: [3, 6, 2, 7, 1, 3, 0, 6]
Final conflicts: 2
Final evaluation score: -2
Number of iterations: 4
Hill-Climbing stopped without finding a solution.
```
## 10. Visualizing the Chessboard

We will create a function to display the current arrangement of queens.

### Code Cell

```python
def print_board(state):

    n = len(state)

    for row in range(n):

        line = ""

        for col in range(n):

            if state[col] == row:
                line += "Q "
            else:
                line += ". "

        print(line)
```

Display the result:

```python
print_board(solution)
```
### Output:
```text
. . . . . . Q . 
. . . . Q . . . 
. . Q . . . . . 
Q . . . . Q . . 
. . . . . . . . 
. . . . . . . . 
. Q . . . . . Q 
. . . Q . . . . 
```

## 11. Plotting the Evaluation Score

A graph helps us observe how the evaluation score changes during the search.

### Code Cell

```python
import matplotlib.pyplot as plt

plt.figure(figsize=(9, 5))

plt.plot(
    range(len(scores)),
    scores,
    marker="o"
)

plt.xlabel("Iteration")
plt.ylabel("Evaluation Score")
plt.title("Hill-Climbing Optimization: N-Queens")

plt.grid(True)
plt.show()
```
### output:

<img width="776" height="470" alt="download" src="https://github.com/user-attachments/assets/d184e51f-9d44-4fa2-be99-157e3501eefc" />

---

### Interpret the Graph

### Answer these questions:
**1. Does the evaluation score increase as the algorithm moves to better states?**

Yes. The evaluation score increases from -12 to -7, then -3, and finally -2. This shows that Hill-Climbing improves the current state by selecting better neighboring states.

**2. Does the score decrease during a move accepted by this implementation?**

No. The score never decreases because the algorithm accepts only neighboring states with a higher evaluation score than the current state.

**3. What happens when the algorithm reaches a state with no better neighbor?**

The algorithm stops because it cannot find any neighboring state with a better evaluation score. It reaches a local maximum.

**4. What does an evaluation score of zero mean?**

An evaluation score of zero means there are no conflicts between any queens. All queens are safely placed on the chessboard, and a valid N-Queens solution has been found.

**5. Can the algorithm stop at a score below zero? Explain why.**

Yes. The algorithm can stop at a score below zero when it reaches a local maximum where no neighboring state has a better score. Some conflicts still exist, so a valid solution has not been found.

### Conclusion:

In this graph, the evaluation score increases from -12 to -2, showing that Hill-Climbing improves the initial state. However, it stops at -2 instead of reaching zero. This means the algorithm gets stuck at a local maximum without finding a valid N-Queens solution.


## 12. Experimenting with Different Board Sizes

Run Hill-Climbing with different values of N.

### Code Cell

```python
for n in [4, 8, 10, 12]:

    solution, scores = hill_climbing(n)

    print(
        f"N = {n:2d} | "
        f"Conflicts = {count_conflicts(solution):2d} | "
        f"Iterations = {len(scores):3d} | "
        f"Solved = {count_conflicts(solution) == 0}"
    )
```
### Output 1:
```text
N =  4 | Conflicts =  1 | Iterations =   2 | Solved = False
N =  8 | Conflicts =  0 | Iterations =   3 | Solved = True
N = 10 | Conflicts =  1 | Iterations =   5 | Solved = False
N = 12 | Conflicts =  2 | Iterations =   5 | Solved = False
```
### Output 2:
```text
N =  4 | Conflicts =  1 | Iterations =   2 | Solved = False
N =  8 | Conflicts =  1 | Iterations =   5 | Solved = False
N = 10 | Conflicts =  2 | Iterations =   4 | Solved = False
N = 12 | Conflicts =  2 | Iterations =   6 | Solved = False
```
### Output 3:
```text
N =  4 | Conflicts =  1 | Iterations =   2 | Solved = False
N =  8 | Conflicts =  2 | Iterations =   5 | Solved = False
N = 10 | Conflicts =  2 | Iterations =   5 | Solved = False
N = 12 | Conflicts =  3 | Iterations =   5 | Solved = False
```
---
---

## Task 2

Run the experiment several times.

Record your observations in the following table.

| N | Run | Final Conflicts | Iterations | Solved? |
|---:|---:|---:|---:|---|
| 4 | 1 | 1 | 2 | False |
| 4 | 2 | 1 | 2 | False |
| 8 | 1 | 0 | 3 | True |
| 8 | 2 | 1 | 5 | False|
| 8 | 3 | 2 | 5 | False |
| 10 | 1 | 1 | 5 | False |
| 10 | 2 | 2 | 4 | False |
| 10 | 3 | 2 | 5 | False |

**Discussion: Does the same board size always produce the same result?**

No, the same board size does not always produce the same result. This is because Hill-Climbing starts with a randomly generated initial state. Different initial states can lead to different final numbers of conflicts and iterations.

Sometimes the algorithm may find a valid solution with zero conflicts, while other times it may get stuck at a local maximum with remaining conflicts.

Therefore, the results of Hill-Climbing can vary even when the board size remains the same.

## 14. Implementing Random-Restart Hill-Climbing

The following implementation keeps track of the best state found and records the best evaluation score after every restart.

It stops early if a valid solution is found.

### Code Cell

```python
def random_restart_hill_climbing(n, max_restarts=100):

    best_state = None
    best_score = float("-inf")
    best_scores = []

    for restart in range(max_restarts):

        # Start a new Hill-Climbing run
        state, scores = hill_climbing(n)

        score = evaluation(state)

        # Keep the best state found so far
        if score > best_score:
            best_state = state
            best_score = score

        # Record the best score after this restart
        best_scores.append(best_score)

        # A score of zero means a valid solution
        if best_score == 0:
            return best_state, restart + 1, best_scores

    return best_state, max_restarts, best_scores
```

### Code Cell: Run the Algorithm

```python
solution, restarts, best_scores = (
    random_restart_hill_climbing(8, max_restarts=100)
)

print("Final state:", solution)
print("Final conflicts:", count_conflicts(solution))
print("Restarts performed:", restarts)

if count_conflicts(solution) == 0:
    print("A valid solution was found.")
else:
    print("No solution was found within the restart limit.")

print_board(solution)
```
### Output:
```text
Final state: [4, 6, 3, 0, 2, 7, 5, 1]
Final conflicts: 0
Restarts performed: 6
A valid solution was found.
. . . Q . . . . 
. . . . . . . Q 
. . . . Q . . . 
. . Q . . . . . 
Q . . . . . . . 
. . . . . . Q . 
. Q . . . . . . 
. . . . . Q . . 
```

A valid solution should have zero conflicts.

If no solution is found within 100 restarts, the algorithm returns the best state encountered during those attempts.

The restart limit is a practical stopping condition, not a guarantee of success.

## 15. Plotting Random-Restart Progress

### Code cell
```python
plt.figure(figsize=(9, 5))

plt.plot(
    range(1, len(best_scores) + 1),
    best_scores,
    marker="o"
)

plt.xlabel("Restart")
plt.ylabel("Best Evaluation Score So Far")
plt.title("Random-Restart Hill-Climbing: N-Queens")

plt.grid(True)
plt.show()
```
### Output:
<img width="780" height="470" alt="download (2)" src="https://github.com/user-attachments/assets/0957d9e7-3ef1-43b2-b111-a4a88a330e1a" />

## 16. Comparing Basic and Random-Restart Hill-Climbing

Run both algorithms five times.

### Code Cell

```python
print("Basic Hill-Climbing")
print("-------------------")

for i in range(5):

    state, scores = hill_climbing(8)
    conflicts = count_conflicts(state)

    print(
        f"Run {i + 1}: "
        f"Conflicts = {conflicts}, "
        f"Iterations = {len(scores)}, "
        f"Solved = {conflicts == 0}"
    )


print("\nRandom-Restart Hill-Climbing")
print("----------------------------")

for i in range(5):

    state, restarts, best_scores = (
        random_restart_hill_climbing(8, max_restarts=100)
    )

    conflicts = count_conflicts(state)

    print(
        f"Run {i + 1}: "
        f"Conflicts = {conflicts}, "
        f"Restarts = {restarts}, "
        f"Solved = {conflicts == 0}"
    )
```
### Output 1:
```text
Basic Hill-Climbing
-------------------
Run 1: Conflicts = 1, Iterations = 5, Solved = False
Run 2: Conflicts = 1, Iterations = 6, Solved = False
Run 3: Conflicts = 1, Iterations = 5, Solved = False
Run 4: Conflicts = 2, Iterations = 4, Solved = False
Run 5: Conflicts = 0, Iterations = 5, Solved = True

Random-Restart Hill-Climbing
----------------------------
Run 1: Conflicts = 0, Restarts = 28, Solved = True
Run 2: Conflicts = 0, Restarts = 3, Solved = True
Run 3: Conflicts = 0, Restarts = 9, Solved = True
Run 4: Conflicts = 0, Restarts = 5, Solved = True
Run 5: Conflicts = 0, Restarts = 3, Solved = True
```
###  Output 2:
```text
Basic Hill-Climbing
-------------------
Run 1: Conflicts = 2, Iterations = 4, Solved = False
Run 2: Conflicts = 1, Iterations = 4, Solved = False
Run 3: Conflicts = 1, Iterations = 4, Solved = False
Run 4: Conflicts = 1, Iterations = 3, Solved = False
Run 5: Conflicts = 1, Iterations = 3, Solved = False

Random-Restart Hill-Climbing
----------------------------
Run 1: Conflicts = 0, Restarts = 7, Solved = True
Run 2: Conflicts = 0, Restarts = 6, Solved = True
Run 3: Conflicts = 0, Restarts = 7, Solved = True
Run 4: Conflicts = 0, Restarts = 6, Solved = True
Run 5: Conflicts = 0, Restarts = 9, Solved = True
```
### Output 3:
```text
Basic Hill-Climbing
-------------------
Run 1: Conflicts = 0, Iterations = 5, Solved = True
Run 2: Conflicts = 1, Iterations = 4, Solved = False
Run 3: Conflicts = 2, Iterations = 3, Solved = False
Run 4: Conflicts = 0, Iterations = 4, Solved = True
Run 5: Conflicts = 2, Iterations = 4, Solved = False

Random-Restart Hill-Climbing
----------------------------
Run 1: Conflicts = 0, Restarts = 26, Solved = True
Run 2: Conflicts = 0, Restarts = 35, Solved = True
Run 3: Conflicts = 0, Restarts = 1, Solved = True
Run 4: Conflicts = 0, Restarts = 4, Solved = True
Run 5: Conflicts = 0, Restarts = 35, Solved = True
```
### Output 4:
```text
Basic Hill-Climbing
-------------------
Run 1: Conflicts = 1, Iterations = 3, Solved = False
Run 2: Conflicts = 1, Iterations = 4, Solved = False
Run 3: Conflicts = 2, Iterations = 3, Solved = False
Run 4: Conflicts = 2, Iterations = 5, Solved = False
Run 5: Conflicts = 1, Iterations = 4, Solved = False

Random-Restart Hill-Climbing
----------------------------
Run 1: Conflicts = 0, Restarts = 24, Solved = True
Run 2: Conflicts = 0, Restarts = 39, Solved = True
Run 3: Conflicts = 0, Restarts = 18, Solved = True
Run 4: Conflicts = 0, Restarts = 6, Solved = True
Run 5: Conflicts = 0, Restarts = 7, Solved = True
```
### Output 5:
```text
Basic Hill-Climbing
-------------------
Run 1: Conflicts = 1, Iterations = 6, Solved = False
Run 2: Conflicts = 1, Iterations = 5, Solved = False
Run 3: Conflicts = 2, Iterations = 4, Solved = False
Run 4: Conflicts = 2, Iterations = 3, Solved = False
Run 5: Conflicts = 3, Iterations = 2, Solved = False

Random-Restart Hill-Climbing
----------------------------
Run 1: Conflicts = 0, Restarts = 6, Solved = True
Run 2: Conflicts = 0, Restarts = 9, Solved = True
Run 3: Conflicts = 0, Restarts = 9, Solved = True
Run 4: Conflicts = 0, Restarts = 1, Solved = True
Run 5: Conflicts = 0, Restarts = 2, Solved = True
```

---
---
## Task 3
### Compare the results.

**1. Did basic Hill-Climbing find a solution in every run?**

Not necessarily. Basic Hill-Climbing may find a solution in some runs, but in other runs it may get stuck at a local maximum with remaining conflicts.

**2. Did Random-Restart Hill-Climbing improve the results?**

Yes, generally. Random-Restart improves the probability of finding a solution by starting the search again from different random initial states whenever necessary.

**3. How many restarts were needed to find a solution?**

The number of restarts depends on the random initial states. We can observe the exact number from the output of each experiment.

**4. Did any random-restart experiment reach its restart limit without finding a solution?**

This depends on the experiment. If all five runs find a solution, then none reached the restart limit. Otherwise, a run may reach the limit of 100 restarts without finding a solution.

**5. Why can Random-Restart improve the probability of success without guaranteeing success within a fixed number of attempts?**

Random-Restart tries different initial states, increasing the chances of finding a solution. However, there is no guarantee that a successful initial state will be selected within 100 attempts. Therefore, it improves the probability of success but cannot guarantee it.\[
8 \times (8-1) = 8 \times 7 = 56
\]

n unsuccessful initial state. More restarts increase the probability of finding a solution, but success within a fixed number of attempts is not guaranteed.

## Lab Questions

Question 1: What is the purpose of the evaluation function in Hill-Climbing?

The evaluation function measures how good a particular state is. In our N-Queens problem, it calculates the number of conflicts between queens. It helps Hill-Climbing select a better neighboring state.

Question 2: Why is an evaluation score of zero the best result in our N-Queens implementation?
An evaluation score of zero means there are no conflicts between any queens. This means no two queens attack each other, and a valid N-Queens solution has been found.

Question 3: What is the difference between a local maximum and a global maximum?
A local maximum is a state that has a better evaluation score than all its neighboring states, but it may not be the best possible state.

A global maximum is the best possible state in the entire search space. In our N-Queens problem, an evaluation score of zero represents the global maximum.

Question 4: Why can basic Hill-Climbing stop without finding a solution even when a solution exists?
Basic Hill-Climbing can stop when it reaches a local maximum where no neighboring state has a better score. It cannot move to a worse state to explore other possibilities, even if a valid solution exists elsewhere.

Question 5: Does reaching a local maximum always mean that the algorithm has failed? Explain.
No. Reaching a local maximum does not always mean failure. If the local maximum is also the global maximum, the algorithm has successfully found the solution. However, if conflicts still exist, it has stopped without finding a valid solution.

Question 6: For N = 8, how many neighbors does each state have in our implementation?
Each state has 56 neighbors.


Question 7: What does the evaluation-score graph tell you about the behavior of Hill-Climbing?
The graph shows how the evaluation score changes during the search. The score increases as the algorithm moves to better states. It never decreases because the algorithm accepts only improving moves. When no better neighbor is available, the algorithm stops.

Question 8: Explain probabilistic completeness in your own words.
Probabilistic completeness means that an algorithm has an increasing chance of finding a solution when it is allowed to perform more attempts. It does not guarantee success in every individual attempt, but repeated attempts can improve the probability of success.

Question 9: Why does Random-Restart Hill-Climbing improve the chance of finding a solution?
Random-Restart Hill-Climbing starts the search again from a new random initial state when the previous search gets stuck. By exploring different starting states, it increases the chances of finding a valid solution.

Question 10: Does probabilistic completeness guarantee that a solution will be found within 100 restarts? Why or why not?
No. Probabilistic completeness does not guarantee that a solution will be found within 100 restarts. Every restart may start from an unsuccessful initial state. More restarts increase the probability of finding a solution, but success within a fixed number of attempts is not guaranteed.
