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

# Questions and Answers
1. Which algorithm found the shorter path?

   Neither. Both BFS and DFS found paths of equal length (8 moves) in this maze.

2. Which algorithm had the larger maximum frontier?

   Neither. Both algorithms had the same maximum frontier size of 3.

3. Why did the two algorithms produce different paths on the same maze?

   BFS explores the maze level by level, while DFS follows one path as deeply as possible before backtracking. Therefore, they can discover different paths.

4. Does DFS always produce the shortest path? Why or why not?

   No. DFS does not guarantee the shortest path because it explores one branch deeply without comparing all possible paths.

5. Does BFS always produce the shortest path when every movement costs 1? Why?

   Yes. BFS explores all possible positions level by level, ensuring that the first path reaching the goal has the minimum number of movements.
