# Lab 2: Uninformed Search — BFS and DFS on a 2D Maze

## Exersice 1 : Compare BFS and DFS
Both algorithms were tested using the same maze, initial position, destination, and movement sequence to ensure a fair comparison.

### Comparison Table
| Algorithm | Path Length | Maximum Frontier |
|-----------|-------------|------------------|
| BFS       | 8           | 3                |
| DFS       | 8           | 3                |

### Key Observations
- BFS searches through the maze level by level, maintaining its positions in a queue.
- DFS follows a path as far as possible before returning to explore other paths, using a stack.
- For this maze, both algorithms reached the destination with the same path length.
- Despite producing equal results here, their exploration methods differ, and DFS can find a different route depending on the maze structure and direction order.

# Q&A
**1. Which algorithm found the shorter path?**

   Neither. Both BFS and DFS found paths of equal length (8 moves) in this maze.

**2. Which algorithm had the larger maximum frontier?**

   Neither. Both algorithms had the same maximum frontier size of 3.

**3. Why did the two algorithms produce different paths on the same maze?**

   BFS explores the maze level by level, while DFS follows one path as deeply as possible before backtracking. Therefore,       they can discover different paths.

**4. Does DFS always produce the shortest path? Why or why not?**

   No. DFS does not guarantee the shortest path because it explores one branch deeply without comparing all possible paths.

**5. Does BFS always produce the shortest path when every movement costs 1? Why?**

   Yes. BFS explores all possible positions level by level, ensuring that the first path reaching the goal has the minimum      number of movements.

   ---

   ## Exercise 2: Change the Neighbor Order
   DFS's exact path depends on the order in which neighbors are tried. Change directions and re-run DFS:
   ### Before Changing Neighbor Order:
   Original directions:
   ```python
   
   (-1, 0) → Up
   (1, 0) → Down
   (0, -1) → Left
   (0, 1) → Right
   ```
   Results:
   ```text
   DFS path: [(0, 0), (0, 1), (0, 2), (0, 3), (0, 4), (1, 4), (2, 4), (3, 4), (4, 4)]
   Path length: 8
   Maximum frontier: 3
   ```
   Path Diagram:
   ```text
   * * * * * 
   . # # # * 
   . . . # * 
   . # . . * 
   . # # # * 
   ```
   ### After Changing Neighbor Order:
   Updated directions:
   ```python
   (0, 1) → Right
   (1, 0) → Down
   (0, -1) → Left
   (-1, 0) → Up
   ```
   Results:
   ```text
   DFS path: [(0, 0), (1, 0), (2, 0), (2, 1), (2, 2), (3, 2), (3, 3), (3, 4), (4, 4)]
   Path length: 8
   Maximum frontier: 3
   ```
   Path Diagram:
   ```text
   * . . . . 
   * # # # . 
   * * * # . 
   . # * * * 
   . # # # * 
   ```

   # Q&A
   **1. Did the DFS path change? Did BFS's path (Section 4) depend on this ordering the same way? Why or why not?**
   
   Yes. The DFS path changed because changing the neighbor order changes which direction DFS explores first. With the new       order, DFS follows a different route to reach the goal.

   BFS also depends on neighbor ordering when multiple shortest paths exist. However, it always explores level by level and     guarantees the shortest path when every movement has equal cost.
   
   ---

   ## Exercise 3: Change `start` And `goal`:
   Now try different start and goal cells in the same maze, and re-run BFS and DFS. Pick cells that are open (0) in the maze    grid from Section 1.
   ### Before Changing `start` And `goal`
   ```python
   start = (0, 0)
   goal = (4, 4)
   ```
   Results:
   ```text
   BFS path: [(0, 0), (1, 0), (2, 0), (2, 1), (2, 2), (3, 2), (3, 3), (3, 4), (4, 4)]
   BFS path length: 8
   BFS maximum frontier: 3
   
   DFS path: [(0, 0), (0, 1), (0, 2), (0, 3), (0, 4), (1, 4), (2, 4), (3, 4), (4, 4)]
   DFS path length: 8
   DFS maximum frontier: 3
   ```
   Path Diagram:
   ```text
   BFS solution:
   * . . . . 
   * # # # . 
   * * * # . 
   . # * * * 
   . # # # * 

   DFS solution:
   * * * * * 
   . # # # * 
   . . . # * 
   . # . . * 
   . # # # * 
   ```
   ### After Changing `start` And `goal`
   ```python
   start = (0, 0)
   goal = (2, 2)
   ```
   Results:
   ```text
   BFS path: [(0, 0), (1, 0), (2, 0), (2, 1), (2, 2)]
   BFS path length: 4
   BFS maximum frontier: 3

   DFS path: [(0, 0), (0, 1), (0, 2), (0, 3), (0, 4), (1, 4), (2, 4), (3, 4), (3, 3), (3, 2), (2, 2)]
   DFS path length: 10
   DFS maximum frontier: 3
   ```
   Path Diagram:
   ```text
   BFS solution:
   * . . . . 
   * # # # . 
   * * * # . 
   . # . . . 
   . # # # . 

   DFS solution:
   * * * * * 
   . # # # * 
   . . * # * 
   . # * * * 
   . # # # . 
   ```
   Try at least 3 different `(start, goal)` pairs and record your results:
   ### 1st Pair:
   ```python
   start = (0, 2)
   goal = (2, 1)
   ```
   Results:
   ```text
   BFS path: [(0, 2), (0, 1), (0, 0), (1, 0), (2, 0), (2, 1)]
   BFS path length: 5
   BFS maximum frontier: 3

   DFS path: [(0, 2), (0, 3), (0, 4), (1, 4), (2, 4), (3, 4), (3, 3), (3, 2), (2, 2), (2, 1)]
   DFS path length: 9
   DFS maximum frontier: 3
   ```
   Path Diagram:
   ```text
   BFS solution:
   * * * . . 
   * # # # . 
   * * . # . 
   . # . . . 
   . # # # . 

   DFS solution:
   . . * * * 
   . # # # * 
   . * * # * 
   . # * * * 
   . # # # . 
   ```
   ### 2nd Pair:
   ```python
   start = (1, 0)
   goal = (1, 4)
   ```
   Results:
   ```text
   BFS path: [(1, 0), (0, 0), (0, 1), (0, 2), (0, 3), (0, 4), (1, 4)]
   BFS path length: 6
   BFS maximum frontier: 3

   DFS path: [(1, 0), (2, 0), (2, 1), (2, 2), (3, 2), (3, 3), (3, 4), (2, 4), (1, 4)]
   DFS path length: 8
   DFS maximum frontier: 4
   ```
   Path Diagram:
   ```text
   BFS solution:
   * * * * * 
   * # # # * 
   . . . # . 
   . # . . . 
   . # # # . 

   DFS solution:
   . . . . . 
   * # # # * 
   * * * # * 
   . # * * * 
   . # # # . 
   ```
   ### 3rd Pair:
   ```python
   start = (3, 3)
   goal = (2, 0)
   ```
   Results:
   ```text
   BFS path: [(3, 3), (3, 2), (2, 2), (2, 1), (2, 0)]
   BFS path length: 4
   BFS maximum frontier: 3

   DFS path: [(3, 3), (3, 4), (2, 4), (1, 4), (0, 4), (0, 3), (0, 2), (0, 1), (0, 0), (1, 0), (2, 0)]
   DFS path length: 10
   DFS maximum frontier: 3
   ```
   Path Diagram:
   ```text
   BFS solution:
   . . . . . 
   . # # # . 
   * * * # . 
   . # * * . 
   . # # # . 

   DFS solution:
   * * * * * 
   * # # # * 
   * . . # * 
   . # . * * 
   . # # # . 
   ```

   | Start | Goal | BFS Length | BFS Max Frontier | DFS Length | DFS Max Frontier |
   | :--- | :--- | :---: | :---: | :---: | :---: |
   | (0, 2) | (2, 1) | 5 | 3 | 9 | 3 |
   | (1, 0) | (1, 4) | 6 | 3 | 8 | 4 |
   | (3, 3) | (2, 0) | 4 | 3 | 10 | 3 |

   ## Q&A

   **6. Does BFS's path length ever change if you re-run it with the same `start/goal?` Does DFS's?**
   
   BFS always returns the shortest path, so its path length remains the same. DFS may return a different path if the            neighbor order changes, but with the same maze and directions, its result remains the same.

   **7. As the distance between start and goal grows, what happens to the maximum frontier size for each algorithm?**
   
   The maximum frontier size may increase as the distance grows, depending on the maze structure and available paths. BFS       may use more memory because it explores multiple nodes level by level, while DFS follows one branch at a time and may use    less memory.

   **8. Can you find a (start, goal) pair where BFS and DFS return the same path? What does that tell you about the maze           structure between those two points?**

   Yes. For example, `(2, 1)` and `(3, 4)`. Both algorithms return the same direct path.
   ```python
   start = (2, 1)
   goal = (3, 4)
   ```
   Results:
   ```text
   BFS path: [(2, 1), (2, 2), (3, 2), (3, 3), (3, 4)]
   BFS path length: 4
   BFS maximum frontier: 3

   DFS path: [(2, 1), (2, 2), (3, 2), (3, 3), (3, 4)]
   DFS path length: 4
   DFS maximum frontier: 2
   ```
   Path Diagram:
   ```text
   BFS solution:
   . . . . . 
   . # # # . 
   . * * # . 
   . # * * * 
   . # # # . 

   DFS solution:
   . . . . . 
   . # # # . 
   . * * # . 
   . # * * * 
   . # # # . 
   ```
   This demonstrates that both algorithms can select the same route for particular starting and goal positions. It does not     necessarily mean that only one possible route exists. This also indicates that both algorithms follow the same route in      this case, even though they use different search strategies.

   **9. What happens if goal is unreachable from start (e.g. sealed off by walls)? Try it — what do bfs() and dfs() return?**

   Both algorithms explore all reachable cells. If the goal cannot be reached, both return None for the path and return the     maximum frontier size.

   ### For Example:
   ```python
   maze = [
    [0, 0, 1, 0, 0],
    [0, 0, 1, 0, 0],
    [0, 0, 1, 0, 0],
    [0, 0, 1, 0, 0],
    [0, 0, 1, 0, 0]
]

start = (0, 0)
goal = (0, 4)
   ```
   Results:
   ```text
   BFS path: None
BFS path length: No path found
BFS maximum frontier: 3

DFS path: None
DFS path length: No path found
DFS maximum frontier: 3
   ```
   ### Explanation:

   The third column is completely blocked by walls, separating the maze into two sections. The start is on the left, while      the goal is on the right. Therefore, neither BFS nor DFS can reach the goal.

   Both algorithms return None for the path.

   ---

   ## Conclusion

In this lab, we worked with BFS and DFS to understand how they find paths through a maze. We saw that BFS finds the shortest path, while DFS explores one direction as far as possible before going back. We also experimented with different neighbor orders and starting and ending points to see how they affect the results. Finally, we learned what happens when the goal cannot be reached. Overall, this lab helped us understand how both search algorithms work in practice.
