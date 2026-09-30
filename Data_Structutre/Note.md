# Data Structures in TypeScript

A practical reference: definition, TypeScript example, and when to use each structure.

---

## 1. Array

**Definition:** An ordered, index-based collection of elements stored in contiguous memory. Access by index is O(1); insert/delete in the middle is O(n).

**Example:**

```typescript
const nums: number[] = [10, 20, 30];

// Access
nums[0];                         // 10
nums.length;                     // 3

// Ends — add / remove
nums.push(40);                   // [10, 20, 30, 40]  add back   O(1)
nums.pop();                      // 40 → [10, 20, 30] remove back O(1)
nums.unshift(5);                 // [5, 10, 20, 30]   add front  O(n)
nums.shift();                    // 5  → [10, 20, 30] remove front O(n)

// Middle
nums.splice(1, 1);               // remove 1 item at index 1

// Find index
const arr = [10, 20, 30, 20];
arr.indexOf(20);                 // 1
arr.lastIndexOf(20);             // 3
arr.findIndex(n => n > 15);      // 1
arr.findLastIndex(n => n > 15);  // 3

// Reverse
const a = [1, 2, 3];
a.reverse();                     // [3, 2, 1] — mutates same array
const b = [1, 2, 3];
b.toReversed();                  // [3, 2, 1] — new array
[...b].reverse();                // [3, 2, 1] — copy then reverse

// Stack (fast) vs queue-on-array (shift is slow)
const stack = [1, 2, 3];
stack.push(4);
stack.pop();                     // LIFO — O(1)

const queue = [1, 2, 3];
queue.push(4);                   // enqueue
queue.shift();                   // dequeue — works, but O(n)
```

**Useful methods:**

| Method | What it does | Mutates? | Returns | Time |
|--------|--------------|----------|---------|------|
| `arr[i]` / `length` | Access / size | No | Value / number | O(1) |
| `push(item)` | Add to **back** | Yes | New length | O(1) |
| `pop()` | Remove from **back** | Yes | Removed item | O(1) |
| `unshift(item)` | Add to **front** | Yes | New length | O(n) |
| `shift()` | Remove from **front** | Yes | Removed item | O(n) |
| `splice(i, n)` | Insert/remove at index | Yes | Removed items | O(n) |
| `indexOf(value)` | First index of value | No | Index or `-1` | O(n) |
| `lastIndexOf(value)` | Last index of value | No | Index or `-1` | O(n) |
| `findIndex(fn)` | First index matching `fn` | No | Index or `-1` | O(n) |
| `findLastIndex(fn)` | Last index matching `fn` | No | Index or `-1` | O(n) |
| `reverse()` | Reverse in place | Yes | Same array | O(n) |
| `toReversed()` | Reverse copy | No | New array | O(n) |

**Notes:**
- Prefer `pop` over `shift` when possible — `shift`/`unshift` reindex every element (**O(n)**).
- `reverse()` changes order but keeps the **same reference** (`original === result` is `true`). Use `toReversed()` or `[...arr].reverse()` to keep the original.
- `push`/`pop` → stack (LIFO). `push`/`shift` → simple queue (FIFO, but dequeue is O(n)).

**Usage:**
- Indexed lists and iteration
- Stacks (`push`/`pop`) and simple queues (`push`/`shift`)
- Finding positions, reversing order, dynamic collections

---

## 2. String

**Definition:** An **immutable** sequence of characters. Index access is O(1). Any “change” creates a **new** string. In TypeScript/JavaScript, type is `string`.

> Strings are not arrays, but many methods feel similar (`indexOf`, `slice`, `includes`).

**Example:**

```typescript
const s = "Hello";
s.length;        // 5
s[0];            // "H"
s[1];            // "e"

// Strings are immutable — methods return NEW strings
const t = s.toLowerCase(); // "hello"
console.log(s);            // "Hello" — original unchanged
```

**Useful methods:**

| Method | Description | Example | Typical time |
|--------|-------------|---------|--------------|
| `length` | Number of characters | `"hi".length` → `2` | O(1) |
| `charAt(i)` / `s[i]` | Char at index | `"cat"[1]` → `"a"` | O(1) |
| `indexOf(sub)` | First index of substring | `"hello".indexOf("ll")` → `2` | O(n·m) |
| `lastIndexOf(sub)` | Last index of substring | `"aba".lastIndexOf("a")` → `2` | O(n·m) |
| `includes(sub)` | Contains substring? | `"cat".includes("a")` → `true` | O(n·m) |
| `startsWith(pre)` | Starts with prefix? | `"flower".startsWith("flow")` | O(m) |
| `endsWith(suf)` | Ends with suffix? | `"flight".endsWith("ght")` | O(m) |
| `slice(start, end?)` | Substring `[start, end)` | `"hello".slice(1, 4)` → `"ell"` | O(k) |
| `substring(start, end?)` | Similar to `slice` | `"hello".substring(1, 4)` → `"ell"` | O(k) |
| `split(sep)` | String → string array | `"a,b,c".split(",")` → `["a","b","c"]` | O(n) |
| `join` *(on Array)* | Array → string | `["a","b"].join("-")` → `"a-b"` | O(n) |
| `trim()` | Remove edge whitespace | `"  hi  ".trim()` → `"hi"` | O(n) |
| `toLowerCase()` | Lowercase copy | `"Hi".toLowerCase()` → `"hi"` | O(n) |
| `toUpperCase()` | Uppercase copy | `"Hi".toUpperCase()` → `"HI"` | O(n) |
| `replace(a, b)` | Replace first match | `"aa".replace("a","b")` → `"ba"` | O(n) |
| `replaceAll(a, b)` | Replace all matches | `"aa".replaceAll("a","b")` → `"bb"` | O(n) |
| `repeat(n)` | Repeat string | `"ab".repeat(3)` → `"ababab"` | O(n·len) |
| `padStart` / `padEnd` | Pad to length | `"5".padStart(3,"0")` → `"005"` | O(n) |
| `concat` / `+` | Combine strings | `"a" + "b"` → `"ab"` | O(n) |

```typescript
const word = "catalogue";

word.indexOf("log");       // 3
word.includes("cat");      // true
word.startsWith("cata");   // true
word.endsWith("ogue");     // true
word.slice(0, 3);          // "cat"
word.split("");            // ["c","a","t","a","l","o","g","u","e"]

// Reverse a string (strings have no .reverse())
const reversed = word.split("").reverse().join(""); // "euogolatacs"
// or: [...word].reverse().join("")

// Common LeetCode patterns
const chars = [..."flower"];     // char array for in-place style work
const rebuilt = chars.join("");  // back to string

// Compare / sort
"apple" < "banana";              // true (lexicographic)
["dog", "cat"].sort();           // ["cat", "dog"]
```

**Important notes:**
- **Immutable:** `s[0] = "x"` does nothing useful; use `slice` + concat or a char array.
- Prefer `slice` over `substring` (clearer with negatives: `slice(-1)` = last char).
- Building a string with `result += ch` in a loop can be **O(n²)**; prefer `chars.push` then `join("")` or a buffer pattern.
- `for...of` iterates code units well for typical ASCII/BMP; for full Unicode graphemes, be careful with surrogate pairs.

**Usage:**
- Text processing, parsing, validation
- Prefix / suffix checks (`startsWith` / `endsWith`)
- Palindrome, anagram, LCP problems
- Convert with `split` / `join` when you need array methods (`reverse`, `sort`, `map`)

---

## 3. Stack (LIFO)

**Definition:** Last-In, First-Out structure. Only the top element is accessible. Push and pop are O(1).

**Example:**

```typescript
class Stack<T> {
  private items: T[] = [];

  push(item: T): void {
    this.items.push(item);
  }

  pop(): T | undefined {
    return this.items.pop();
  }

  peek(): T | undefined {
    return this.items[this.items.length - 1];
  }

  isEmpty(): boolean {
    return this.items.length === 0;
  }
}

const stack = new Stack<string>();
stack.push("a");
stack.push("b");
stack.pop(); // "b"
```

**Usage:**
- Undo/redo history
- Expression parsing and bracket matching
- DFS (depth-first search) call simulation
- Browser back-button history

---

## 4. Queue (FIFO)

**Definition:** First-In, First-Out structure. Enqueue at the back, dequeue from the front. Ideal for ordered processing.

**Example:**

```typescript
class Queue<T> {
  private items: T[] = [];

  enqueue(item: T): void {
    this.items.push(item);
  }

  dequeue(): T | undefined {
    return this.items.shift();
  }

  peek(): T | undefined {
    return this.items[0];
  }

  isEmpty(): boolean {
    return this.items.length === 0;
  }
}

const queue = new Queue<number>();
queue.enqueue(1);
queue.enqueue(2);
queue.dequeue(); // 1
```

**Usage:**
- Task / job scheduling
- BFS (breadth-first search)
- Print / request queues
- Message buffers

---

## 5. Linked List

**Definition:** A sequence of nodes where each node holds a value and a pointer to the next (and optionally previous) node. Insert/delete at known positions is O(1); random access is O(n).

**Example:**

```typescript
class ListNode<T> {
  constructor(
    public value: T,
    public next: ListNode<T> | null = null
  ) {}
}

class LinkedList<T> {
  head: ListNode<T> | null = null;

  prepend(value: T): void {
    const node = new ListNode(value, this.head);
    this.head = node;
  }

  append(value: T): void {
    const node = new ListNode(value);
    if (!this.head) {
      this.head = node;
      return;
    }
    let curr = this.head;
    while (curr.next) curr = curr.next;
    curr.next = node;
  }

  toArray(): T[] {
    const result: T[] = [];
    let curr = this.head;
    while (curr) {
      result.push(curr.value);
      curr = curr.next;
    }
    return result;
  }
}

const list = new LinkedList<number>();
list.prepend(2);
list.prepend(1);
list.append(3); // [1, 2, 3]
```

**Usage:**
- Frequent insert/delete at the beginning
- Implementing stacks, queues, LRU caches
- When you do not need random index access

---

## 6. Doubly Linked List

**Definition:** Like a linked list, but each node has both `prev` and `next` pointers, allowing bidirectional traversal.

**Example:**

```typescript
class DoublyNode<T> {
  constructor(
    public value: T,
    public prev: DoublyNode<T> | null = null,
    public next: DoublyNode<T> | null = null
  ) {}
}

class DoublyLinkedList<T> {
  head: DoublyNode<T> | null = null;
  tail: DoublyNode<T> | null = null;

  append(value: T): void {
    const node = new DoublyNode(value);
    if (!this.tail) {
      this.head = this.tail = node;
      return;
    }
    node.prev = this.tail;
    this.tail.next = node;
    this.tail = node;
  }

  remove(node: DoublyNode<T>): void {
    if (node.prev) node.prev.next = node.next;
    else this.head = node.next;
    if (node.next) node.next.prev = node.prev;
    else this.tail = node.prev;
  }
}
```

**Usage:**
- LRU / MRU caches
- Browser history (forward & back)
- Deques (double-ended queues)

---

## 7. Map (Hash Map / Dictionary)

**Definition:** A collection of **key → value** pairs. Each key is unique. Lookup, insert, and delete are average **O(1)** via hashing. In TypeScript/JavaScript, use the built-in `Map<K, V>`.

**Why use `Map` instead of a plain object?**
| | `Map` | Object / `Record` |
|---|-------|-------------------|
| Key types | Any type (objects, numbers, functions…) | Strings / Symbols only |
| Key order | Insertion order guaranteed | Insertion order for string keys (mostly) |
| Size | `.size` | Manual `Object.keys().length` |
| Default keys | None | Inherited prototype keys possible |
| Performance | Better for frequent add/remove | Fine for fixed string keys |

**Example:**

```typescript
// Create
const userAges = new Map<string, number>();

// Insert / update
userAges.set("Alice", 30);
userAges.set("Bob", 25);
userAges.set("Alice", 31); // overwrites — keys are unique

// Retrieve value by key
userAges.get("Alice"); // 31
userAges.get("Eve");   // undefined

// Check key exists
userAges.has("Bob");   // true

// Delete
userAges.delete("Bob"); // true

// Size & clear
userAges.size;          // 1
userAges.clear();

// Iterate (preserves insertion order)
const scores = new Map<string, number>([
  ["alice", 100],
  ["bob", 90],
]);

for (const [key, value] of scores) {
  console.log(key, value);
}

scores.keys();   // iterable of keys
scores.values(); // iterable of values
scores.entries(); // iterable of [key, value]

// Non-string keys (objects as keys)
const meta = new Map<object, string>();
const user = { id: 1 };
meta.set(user, "admin");
meta.get(user); // "admin"
```

**Key methods:**
| Method | Description | Returns |
|--------|-------------|---------|
| `set(key, value)` | Insert or update a pair | The `Map` (chainable) |
| `get(key)` | Retrieve value for a key | Value or `undefined` |
| `has(key)` | Check if key exists | `boolean` |
| `delete(key)` | Remove a key–value pair | `boolean` (whether it existed) |
| `clear()` | Remove all pairs | `void` |
| `size` | Number of pairs | `number` |
| `keys()` / `values()` / `entries()` | Iterate keys, values, or pairs | Iterators |
| `forEach(fn)` | Run callback for each pair | `void` |

**Common patterns:**

```typescript
// Frequency counter
function countChars(s: string): Map<string, number> {
  const freq = new Map<string, number>();
  for (const ch of s) {
    freq.set(ch, (freq.get(ch) ?? 0) + 1);
  }
  return freq;
}

// Memoization / cache
const cache = new Map<number, number>();
function fib(n: number): number {
  if (n <= 1) return n;
  if (cache.has(n)) return cache.get(n)!;
  const result = fib(n - 1) + fib(n - 2);
  cache.set(n, result);
  return result;
}
```

**Usage:**
- Caching / memoization
- Counting frequencies (char/word counts)
- Fast lookups by ID or any key
- Graph adjacency lists (`Map<Node, Neighbor[]>`)
- Symbol tables / indexes
- When keys are not plain strings (objects, numbers, etc.)

---


## 8. Hash Set (`Set`)

**Definition:** Unordered collection of unique values. Average O(1) add, has, and delete.

**Example:**

```typescript
const ids = new Set<number>();
ids.add(1);
ids.add(2);
ids.add(1); // ignored — already present

console.log(ids.has(2)); // true
ids.delete(1);
console.log(ids.size);   // 1
```

**Usage:**
- Removing duplicates
- Membership checks
- Visited-node tracking in graph algorithms
- Intersection / union of collections

---

## 9. Tree (General / N-ary)

**Definition:** Hierarchical structure of nodes with one root; each node has zero or more children. No cycles.

**Example:**

```typescript
class TreeNode<T> {
  children: TreeNode<T>[] = [];

  constructor(public value: T) {}

  addChild(child: TreeNode<T>): void {
    this.children.push(child);
  }
}

const root = new TreeNode("Company");
const eng = new TreeNode("Engineering");
const sales = new TreeNode("Sales");
root.addChild(eng);
root.addChild(sales);
eng.addChild(new TreeNode("Frontend"));
eng.addChild(new TreeNode("Backend"));
```

**Usage:**
- File systems / folder trees
- Org charts
- DOM / UI component trees
- Nested categories

---

## 10. Binary Tree

**Definition:** A tree where each node has at most two children: left and right.

**Example:**

```typescript
class BinaryTreeNode<T> {
  left: BinaryTreeNode<T> | null = null;
  right: BinaryTreeNode<T> | null = null;

  constructor(public value: T) {}
}

const root = new BinaryTreeNode(1);
root.left = new BinaryTreeNode(2);
root.right = new BinaryTreeNode(3);
root.left.left = new BinaryTreeNode(4);
```

**Usage:**
- Foundation for BST, heaps, expression trees
- Hierarchical binary relationships
- Divide-and-conquer structures

---

## 11. Binary Search Tree (BST)

**Definition:** A binary tree where for every node: left subtree values < node value < right subtree values. Search, insert, delete average O(log n); worst O(n) if skewed.

**Example:**

```typescript
class BSTNode {
  left: BSTNode | null = null;
  right: BSTNode | null = null;

  constructor(public value: number) {}
}

class BST {
  root: BSTNode | null = null;

  insert(value: number): void {
    const node = new BSTNode(value);
    if (!this.root) {
      this.root = node;
      return;
    }
    let curr = this.root;
    while (true) {
      if (value < curr.value) {
        if (!curr.left) {
          curr.left = node;
          return;
        }
        curr = curr.left;
      } else {
        if (!curr.right) {
          curr.right = node;
          return;
        }
        curr = curr.right;
      }
    }
  }

  search(value: number): boolean {
    let curr = this.root;
    while (curr) {
      if (value === curr.value) return true;
      curr = value < curr.value ? curr.left : curr.right;
    }
    return false;
  }
}

const bst = new BST();
bst.insert(50);
bst.insert(30);
bst.insert(70);
bst.search(30); // true
```

**Usage:**
- Sorted data with dynamic insert/delete
- Range queries
- Ordered maps/sets (when balanced)

---

## 12. Heap / Priority Queue

**Definition:** A complete binary tree that satisfies the heap property (min-heap: parent ≤ children; max-heap: parent ≥ children). Get-min/max is O(1); insert/extract is O(log n).

**Example (Min-Heap):**

```typescript
class MinHeap {
  private data: number[] = [];

  private parent(i: number) {
    return Math.floor((i - 1) / 2);
  }
  private left(i: number) {
    return 2 * i + 1;
  }
  private right(i: number) {
    return 2 * i + 2;
  }

  insert(val: number): void {
    this.data.push(val);
    let i = this.data.length - 1;
    while (i > 0 && this.data[this.parent(i)] > this.data[i]) {
      [this.data[i], this.data[this.parent(i)]] = [
        this.data[this.parent(i)],
        this.data[i],
      ];
      i = this.parent(i);
    }
  }

  extractMin(): number | undefined {
    if (this.data.length === 0) return undefined;
    if (this.data.length === 1) return this.data.pop();

    const min = this.data[0];
    this.data[0] = this.data.pop()!;
    this.heapify(0);
    return min;
  }

  private heapify(i: number): void {
    const l = this.left(i);
    const r = this.right(i);
    let smallest = i;
    if (l < this.data.length && this.data[l] < this.data[smallest])
      smallest = l;
    if (r < this.data.length && this.data[r] < this.data[smallest])
      smallest = r;
    if (smallest !== i) {
      [this.data[i], this.data[smallest]] = [
        this.data[smallest],
        this.data[i],
      ];
      this.heapify(smallest);
    }
  }

  peek(): number | undefined {
    return this.data[0];
  }
}

const heap = new MinHeap();
heap.insert(5);
heap.insert(1);
heap.insert(3);
heap.extractMin(); // 1
```

**Usage:**
- Priority queues / task schedulers
- Dijkstra’s / Prim’s algorithms
- Top-K elements
- Event simulation by time

---

## 13. Graph

**Definition:** A set of vertices (nodes) connected by edges. Can be directed/undirected, weighted/unweighted. Represented as adjacency list or matrix.

**Example (Adjacency List):**

```typescript
class Graph {
  private adj = new Map<string, string[]>();

  addVertex(v: string): void {
    if (!this.adj.has(v)) this.adj.set(v, []);
  }

  addEdge(from: string, to: string, undirected = true): void {
    this.addVertex(from);
    this.addVertex(to);
    this.adj.get(from)!.push(to);
    if (undirected) this.adj.get(to)!.push(from);
  }

  neighbors(v: string): string[] {
    return this.adj.get(v) ?? [];
  }

  bfs(start: string): string[] {
    const visited = new Set<string>();
    const order: string[] = [];
    const q: string[] = [start];
    visited.add(start);

    while (q.length) {
      const v = q.shift()!;
      order.push(v);
      for (const n of this.neighbors(v)) {
        if (!visited.has(n)) {
          visited.add(n);
          q.push(n);
        }
      }
    }
    return order;
  }
}

const g = new Graph();
g.addEdge("A", "B");
g.addEdge("A", "C");
g.addEdge("B", "D");
g.bfs("A"); // ["A", "B", "C", "D"]
```

**Usage:**
- Social networks / friendships
- Maps & routing
- Dependency graphs
- Recommendation systems

---

## 14. Trie (Prefix Tree)

**Definition:** A tree where each edge represents a character; paths from root spell strings. Optimized for prefix search and autocomplete.

**Example:**

```typescript
class TrieNode {
  children = new Map<string, TrieNode>();
  isEnd = false;
}

class Trie {
  private root = new TrieNode();

  insert(word: string): void {
    let node = this.root;
    for (const ch of word) {
      if (!node.children.has(ch)) {
        node.children.set(ch, new TrieNode());
      }
      node = node.children.get(ch)!;
    }
    node.isEnd = true;
  }

  search(word: string): boolean {
    let node = this.root;
    for (const ch of word) {
      if (!node.children.has(ch)) return false;
      node = node.children.get(ch)!;
    }
    return node.isEnd;
  }

  startsWith(prefix: string): boolean {
    let node = this.root;
    for (const ch of prefix) {
      if (!node.children.has(ch)) return false;
      node = node.children.get(ch)!;
    }
    return true;
  }

  /** Walk to prefix, then DFS to collect every completed word under it */
  wordsWithPrefix(prefix: string): string[] {
    let node = this.root;
    for (const ch of prefix) {
      if (!node.children.has(ch)) return [];
      node = node.children.get(ch)!;
    }

    const result: string[] = [];
    const dfs = (curr: TrieNode, path: string) => {
      if (curr.isEnd) result.push(path); // completed word
      for (const [ch, child] of curr.children) {
        dfs(child, path + ch);
      }
    };
    dfs(node, prefix);
    return result;
  }

  /** All words in the Trie (same as prefix "") */
  getAllWords(): string[] {
    return this.wordsWithPrefix("");
  }
}

const trie = new Trie();
trie.insert("cat");
trie.insert("cats");
trie.insert("catalogue");
trie.insert("car");

trie.search("cat");              // true — exact word?
trie.startsWith("cat");          // true — any word with this prefix?

// Retrieve matching patterns automatically (autocomplete)
trie.wordsWithPrefix("cat");
// → ["cat", "cats", "catalogue"]

trie.wordsWithPrefix("ca");
// → ["cat", "cats", "catalogue", "car"]

trie.wordsWithPrefix("dog");
// → []

trie.getAllWords();
// → ["cat", "cats", "catalogue", "car"]
```

**How auto-retrieve works:**
1. Walk down the Trie following the prefix (`c → a → t`)
2. From that node, DFS every branch
3. Whenever `isEnd === true`, push the built string into the result

```
prefix "cat" lands here ↓
              t (isEnd ✓) → collect "cat"
              ├─ s (isEnd ✓) → collect "cats"
              └─ a→l→o→g→u→e (isEnd ✓) → collect "catalogue"
```

**Usage:**
- Autocomplete / typeahead (`wordsWithPrefix`)
- Spell checkers
- IP routing / dictionary lookup
- Word games (Boggle, Scrabble)

---

## 15. Deque (Double-Ended Queue)

**Definition:** A queue that supports insert and remove at both ends in O(1) (with a proper implementation).

**Example:**

```typescript
class Deque<T> {
  private items: T[] = [];

  pushFront(item: T): void {
    this.items.unshift(item);
  }

  pushBack(item: T): void {
    this.items.push(item);
  }

  popFront(): T | undefined {
    return this.items.shift();
  }

  popBack(): T | undefined {
    return this.items.pop();
  }

  peekFront(): T | undefined {
    return this.items[0];
  }

  peekBack(): T | undefined {
    return this.items[this.items.length - 1];
  }
}

const dq = new Deque<number>();
dq.pushBack(1);
dq.pushFront(0);
dq.popBack(); // 1
```

**Usage:**
- Sliding window maximum/minimum
- Palindrome checks
- Work-stealing schedulers
- Undo buffers with both ends

> Note: Array `shift`/`unshift` are O(n). For true O(1) ends, use a doubly linked list or circular buffer.

---

## 16. Hash Table (custom / object-based)

**Definition:** Maps keys to values via a hash function into buckets. Collisions handled by chaining or open addressing. Average O(1) ops.

**Example:**

```typescript
class HashTable<V> {
  private buckets: Array<Array<[string, V]>>;
  private size: number;

  constructor(size = 53) {
    this.size = size;
    this.buckets = Array.from({ length: size }, () => []);
  }

  private hash(key: string): number {
    let total = 0;
    const PRIME = 31;
    for (let i = 0; i < Math.min(key.length, 100); i++) {
      total = (total * PRIME + key.charCodeAt(i)) % this.size;
    }
    return total;
  }

  set(key: string, value: V): void {
    const index = this.hash(key);
    const bucket = this.buckets[index];
    const existing = bucket.find(([k]) => k === key);
    if (existing) existing[1] = value;
    else bucket.push([key, value]);
  }

  get(key: string): V | undefined {
    const index = this.hash(key);
    const pair = this.buckets[index].find(([k]) => k === key);
    return pair?.[1];
  }
}

const table = new HashTable<number>();
table.set("apple", 3);
table.get("apple"); // 3
```

**Usage:**
- Implementing `Map`/`Set` internals
- Caching layers
- Symbol tables in compilers
- Fast associative lookups

---

## Summary Table

| Data Structure       | Ordered? | Duplicates? | Avg Access | Avg Insert | Avg Delete | Best For                                      | TS Built-in      |
|----------------------|----------|-------------|------------|------------|------------|-----------------------------------------------|------------------|
| Array                | Yes      | Yes         | O(1)       | O(n)*      | O(n)*      | Indexed lists, iteration                      | `T[]`            |
| String               | Yes      | Yes (chars) | O(1) char  | O(n)†      | O(n)†      | Text, parsing, prefix/suffix                  | `string`         |
| Stack                | Yes      | Yes         | O(1) top   | O(1)       | O(1)       | Undo, DFS, parsing                            | via `Array`      |
| Queue                | Yes      | Yes         | O(1) front | O(1)       | O(1)**     | BFS, scheduling                               | via `Array`      |
| Linked List          | Yes      | Yes         | O(n)       | O(1)***    | O(1)***    | Frequent head insert/delete                   | custom           |
| Doubly Linked List   | Yes      | Yes         | O(n)       | O(1)***    | O(1)***    | LRU cache, bidirectional walk                 | custom           |
| Hash Map (`Map`)     | No****   | Keys unique | O(1)       | O(1)       | O(1)       | Key–value lookup, cache                       | `Map` / `Record` |
| Hash Set (`Set`)     | No****   | No          | O(1)       | O(1)       | O(1)       | Uniqueness, membership                        | `Set`            |
| Tree (N-ary)         | Hierarchical | Yes      | O(n)       | O(1) child | O(n)       | Hierarchies, FS, DOM                          | custom           |
| Binary Tree          | Hierarchical | Yes      | O(n)       | O(1) child | O(n)       | Binary hierarchies                            | custom           |
| BST                  | Sorted   | Policy      | O(log n)   | O(log n)   | O(log n)   | Sorted dynamic data                           | custom           |
| Heap / Priority Queue| Partial  | Yes         | O(1) peek  | O(log n)   | O(log n)   | Priority tasks, Top-K                         | custom           |
| Graph                | N/A      | Edges vary  | O(V+E)     | O(1) edge  | O(E)       | Networks, routing, deps                       | custom           |
| Trie                 | Prefix   | N/A         | O(L)       | O(L)       | O(L)       | Autocomplete, dictionaries                    | custom           |
| Deque                | Yes      | Yes         | O(1) ends  | O(1) ends  | O(1) ends  | Sliding window, both-end ops                  | custom           |
| Hash Table           | No       | Keys unique | O(1)       | O(1)       | O(1)       | Associative arrays (low-level)                | custom / `Map`   |

\* Array insert/delete in the middle is O(n); append (`push`) is amortized O(1).  
\*\* Array `shift` is O(n); use a linked-list or ring buffer for true O(1) dequeue.  
\*\*\* At a known node / head (not by value search).  
\*\*\* JS `Map`/`Set` preserve insertion order, but are not sorted by key/value.  
† Strings are immutable — “insert/delete” means building a new string (usually O(n)).  
`L` = length of the string/key.

---

## Quick Decision Guide

| Need                              | Prefer              |
|-----------------------------------|---------------------|
| Fast index access                 | Array               |
| Text / characters                 | String              |
| Unique values only                | Set                 |
| Key → value lookup                | Map                 |
| Undo / reverse order              | Stack               |
| Process in arrival order          | Queue               |
| Priority-based processing         | Heap                |
| Prefix / autocomplete             | Trie                |
| Relationships / networks          | Graph               |
| Sorted inserts & searches         | BST (balanced)      |
| Frequent insert at both ends      | Deque / Doubly LL   |
| Cache with eviction (LRU)         | Map + Doubly LL     |
