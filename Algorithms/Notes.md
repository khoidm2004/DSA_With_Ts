# Algorithms Notes

---

## 1. DFS (Depth-First Search)

**Definition:** DFS explores as **deep** as possible along one branch before backtracking. It uses a **stack** (explicit or the call stack via recursion).

```
       A
      / \
     B   C
    / \   \
   D   E   F

DFS from A (one possible order): A → B → D → E → C → F
(go deep on B's branch first, then backtrack to C)
```

**Core idea:** visit a node → explore one unvisited neighbor fully → backtrack → try the next neighbor.

---

### Recursive DFS (most common)

**When to use:**
- Graph/tree traversal when depth is not huge (call stack is fine)
- Clean, short code for “visit all reachable nodes”
- Tree problems (preorder / postorder style)
- Topological sort, cycle detection helpers
- Prefer this as the default DFS style

```typescript
function dfs(
  node: string,
  graph: Map<string, string[]>,
  visited: Set<string>
): void {
  if (visited.has(node)) return;
  visited.add(node);
  console.log(node); // process node

  for (const neighbor of graph.get(node) ?? []) {
    dfs(neighbor, graph, visited);
  }
}

// Example graph
const graph = new Map<string, string[]>([
  ["A", ["B", "C"]],
  ["B", ["D", "E"]],
  ["C", ["F"]],
  ["D", []],
  ["E", []],
  ["F", []],
]);

dfs("A", graph, new Set());
// A B D E C F
```

---

### Iterative DFS (explicit stack)

**When to use:**
- Very deep graphs/trees where recursion may hit stack overflow
- You need precise control over the stack
- Same reachability/traversal goals as recursive DFS, but safer for large depth
- Interview follow-up: “do it without recursion”

```typescript
function dfsIterative(
  start: string,
  graph: Map<string, string[]>
): string[] {
  const visited = new Set<string>();
  const stack: string[] = [start];
  const order: string[] = [];

  while (stack.length > 0) {
    const node = stack.pop()!;
    if (visited.has(node)) continue;

    visited.add(node);
    order.push(node);

    // push neighbors (reverse if you want same order as recursive)
    const neighbors = graph.get(node) ?? [];
    for (let i = neighbors.length - 1; i >= 0; i--) {
      if (!visited.has(neighbors[i])) {
        stack.push(neighbors[i]);
      }
    }
  }
  return order;
}
```

---

### DFS on a grid (matrix)

**When to use:**
- 2D matrix / board problems (not a classic adjacency-list graph)
- Number of islands / connected components of cells
- Flood fill, region area, surrounded regions
- Maze on a grid (any path; not shortest — use BFS for shortest)
- Move in 4 or 8 directions from a cell

```typescript
function dfsGrid(
  grid: number[][],
  r: number,
  c: number,
  visited: boolean[][]
): void {
  const rows = grid.length;
  const cols = grid[0].length;

  // out of bounds / water / already visited
  if (
    r < 0 ||
    c < 0 ||
    r >= rows ||
    c >= cols ||
    visited[r][c] ||
    grid[r][c] === 0
  ) {
    return;
  }

  visited[r][c] = true;

  // 4 directions: up, down, left, right
  dfsGrid(grid, r - 1, c, visited);
  dfsGrid(grid, r + 1, c, visited);
  dfsGrid(grid, r, c - 1, visited);
  dfsGrid(grid, r, c + 1, visited);
}
```

---

### DFS to collect paths (backtracking)

Same pattern as Trie autocomplete: go deep, record when a goal is reached, then **backtrack**.

**When to use:**
- You need **all** (or many) solutions, not just “visited once”
- Find all paths from start → goal
- Autocomplete / Trie `wordsWithPrefix`
- Generate permutations, subsets, combinations
- Constraint search: N-Queens, Sudoku, word search
- Pattern: **push → recurse → pop** (undo choice)

```typescript
function allPaths(
  graph: Map<string, string[]>,
  start: string,
  goal: string
): string[][] {
  const result: string[][] = [];

  function dfs(node: string, path: string[], visited: Set<string>) {
    if (node === goal) {
      result.push([...path]);
      return;
    }
    for (const next of graph.get(node) ?? []) {
      if (visited.has(next)) continue;
      visited.add(next);
      path.push(next);
      dfs(next, path, visited);
      path.pop();        // backtrack
      visited.delete(next);
    }
  }

  dfs(start, [start], new Set([start]));
  return result;
}
```

---

### Complexity

| | Time | Space |
|---|------|-------|
| Graph (V vertices, E edges) | O(V + E) | O(V) visited + recursion/stack |
| Grid (R × C) | O(R × C) | O(R × C) worst case |

---

### DFS vs BFS

| | DFS | BFS |
|---|-----|-----|
| Data structure | Stack (or recursion) | Queue |
| Explores | Deep first | Level by level |
| Best for | Paths, components, cycles, topological order, backtracking | Shortest path (unweighted), nearest neighbor |
| Memory | Often less on deep skinny graphs | Can use more on wide graphs |

---

### When to use which DFS method

| Method | Use when… |
|--------|-----------|
| **Recursive DFS** | Default graph/tree visit; code clarity; moderate depth |
| **Iterative DFS** | Deep graphs; avoid call-stack overflow; no recursion allowed |
| **Grid DFS** | Matrix islands, flood fill, cell connectivity |
| **Path / backtracking DFS** | Collect all matches/paths; permutations; Trie prefix words |

### When to use DFS (in general)

- Connected components / islands in a grid → **Grid DFS**
- Detect cycles / topological sort → **Recursive** (or iterative)
- All paths / backtracking (permutations, N-Queens) → **Path DFS**
- Trie / collect words by prefix → **Path DFS**
- Deep graph traversal safely → **Iterative DFS**
- Maze: any path → DFS; **shortest** path → BFS instead

---

### Quick checklist

1. Mark node **visited** (avoid infinite loops)
2. Process current node
3. Recurse (or push) each **unvisited** neighbor
4. If building paths: **push → recurse → pop** (backtrack)
