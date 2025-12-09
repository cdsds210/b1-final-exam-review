# Example Problems

![alt text](images\graph_problems.png)

---

## Algorithm-focused example

Given the directed, weighted graph below, perform the following (no code required - show steps and reasoning):

- Draw the graph
- Run Dijkstra's algorithm from node `A` and show the tentative distance table after each extraction from the priority queue. Indicate the final shortest-path.
- Using the final predecessor pointers, write the shortest path and total cost from `A` to `E`.
- Explain why Dijkstra is valid here (mention weight constraints) and what could break if negative edges were present.

Graph (weights), draw the graph and then run Dijkstra's:

- A -> B (4), A -> C (1)
- C -> B (2), B -> D (5)
- C -> D (8), D -> E (2)
- B -> E (10)
