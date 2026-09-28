# Big O Notation

A practical guide: what Big O means, common complexities with examples, and how to calculate it for an algorithm.

---

## 1. Definition

**Big O notation** describes how an algorithm’s **time** or **space** grows as the input size `n` grows. It focuses on the **worst-case upper bound**, ignoring constants and lower-order terms.

| Idea | Meaning |
|------|---------|
| Input size `n` | Number of elements / length / nodes |
| Time complexity | How many steps grow with `n` |
| Space complexity | Extra memory used as `n` grows |
| Worst case | Slowest / most memory for size `n` |
| Drop constants | `O(2n)` → `O(n)` |
| Drop lower terms | `O(n² + n)` → `O(n²)` |

**Why it matters:** comparing algorithms by growth rate, not by exact milliseconds on one machine.

```
Fast ←————————————————————————————→ Slow (as n grows)

O(1)  O(log n)  O(n)  O(n log n)  O(n²)  O(2ⁿ)  O(n!)
```

---

## 2. Common Complexities (with examples)

### O(1) — Constant

Work does **not** depend on `n`.

```typescript
function getFirst(arr: number[]): number {
  return arr[0]; // always 1 step
}

function add(a: number, b: number): number {
  return a + b;
}
```

**Examples:** array index access, Map/Set `get`/`has` (average), push/pop on array end.

---

### O(log n) — Logarithmic

Each step **cuts the problem roughly in half** (or grows slowly).

```typescript
// Binary search — halves search space each step
function binarySearch(arr: number[], target: number): number {
  let lo = 0;
  let hi = arr.length - 1;

  while (lo <= hi) {
    const mid = Math.floor((lo + hi) / 2);
    if (arr[mid] === target) return mid;
    if (arr[mid] < target) lo = mid + 1;
    else hi = mid - 1;
  }
  return -1;
}
// n = 1_000_000 → ~20 steps
```

**Examples:** binary search, balanced BST search, heap insert/extract.

---

### O(n) — Linear

One pass (or a fixed number of passes) over the input.

```typescript
function sum(arr: number[]): number {
  let total = 0;
  for (const x of arr) total += x; // n iterations
  return total;
}

function findMax(arr: number[]): number {
  let max = arr[0];
  for (let i = 1; i < arr.length; i++) {
    if (arr[i] > max) max = arr[i];
  }
  return max;
}
```

**Examples:** single loop, linear search, Two Sum with HashMap.

---

### O(n log n) — Linearithmic

Often: divide & conquer, or “sort + linear pass”.

```typescript
// Typical cost of efficient sorting
const sorted = [...arr].sort((a, b) => a - b); // ~ O(n log n)

// Pattern: sort then scan
function hasPairSum(arr: number[], target: number): boolean {
  const a = [...arr].sort((x, y) => x - y); // O(n log n)
  let i = 0;
  let j = a.length - 1;
  while (i < j) {
    // O(n)
    const s = a[i] + a[j];
    if (s === target) return true;
    if (s < target) i++;
    else j--;
  }
  return false;
}
// Overall: O(n log n)
```

**Examples:** merge sort, heap sort, efficient comparison sorts, many “sort then two-pointer” solutions.

---

### O(n²) — Quadratic

Nested loops over the same (or similar-sized) input.

```typescript
function hasDuplicateBrute(arr: number[]): boolean {
  for (let i = 0; i < arr.length; i++) {
    for (let j = i + 1; j < arr.length; j++) {
      if (arr[i] === arr[j]) return true;
    }
  }
  return false;
}
// ~ n*(n-1)/2 comparisons → O(n²)

function bubbleSort(arr: number[]): number[] {
  const a = [...arr];
  for (let i = 0; i < a.length; i++) {
    for (let j = 0; j < a.length - 1 - i; j++) {
      if (a[j] > a[j + 1]) [a[j], a[j + 1]] = [a[j + 1], a[j]];
    }
  }
  return a;
}
```

**Examples:** nested loops, Two Sum brute force, selection/bubble/insertion sort (worst).

---

### O(n³) — Cubic

Three nested loops (or cubic nested work).

```typescript
function threeSumBrute(nums: number[]): number[][] {
  const res: number[][] = [];
  const n = nums.length;
  for (let i = 0; i < n; i++) {
    for (let j = i + 1; j < n; j++) {
      for (let k = j + 1; k < n; k++) {
        if (nums[i] + nums[j] + nums[k] === 0) {
          res.push([nums[i], nums[j], nums[k]]);
        }
      }
    }
  }
  return res;
}
```

---

### O(2ⁿ) — Exponential

Each step roughly **doubles** the work (e.g. naive recursion branching).

```typescript
function fibNaive(n: number): number {
  if (n <= 1) return n;
  return fibNaive(n - 1) + fibNaive(n - 2); // two recursive calls
}
// Calls grow like a binary tree → O(2ⁿ)
```

**Examples:** naive Fibonacci, subset enumeration without pruning, some recursive backtracking without memo.

---

### O(n!) — Factorial

All permutations of `n` items.

```typescript
function permutations(nums: number[]): number[][] {
  const res: number[][] = [];
  function dfs(path: number[], used: boolean[]) {
    if (path.length === nums.length) {
      res.push([...path]);
      return;
    }
    for (let i = 0; i < nums.length; i++) {
      if (used[i]) continue;
      used[i] = true;
      path.push(nums[i]);
      dfs(path, used);
      path.pop();
      used[i] = false;
    }
  }
  dfs([], Array(nums.length).fill(false));
  return res;
}
// n! permutations → O(n!)
```

---

## 3. How to Calculate Big O

### Step-by-step method

1. **Identify `n`** — what grows? (array length, string length, number of nodes, etc.)
2. **Count dominant operations** — loops, recursion, nested work that depends on `n`.
3. **Apply the rules below** — add or multiply the right way.
4. **Simplify** — drop constants and lower-order terms; keep the dominant term.

---

### All calculation rules

#### Rule 1 — Drop constants

Ignore fixed multipliers.

```text
O(2n) → O(n)
O(100) → O(1)
O(3n² + 5) → O(n²)
```

#### Rule 2 — Drop lower-order terms

Keep only the **fastest-growing** term.

```text
O(n² + n) → O(n²)
O(n + log n) → O(n)
O(n! + n²) → O(n!)
```

Growth order (slow → fast):

```text
O(1) < O(log n) < O(n) < O(n log n) < O(n²) < O(n³) < O(2ⁿ) < O(n!)
```

#### Rule 3 — Sequential code → ADD (then take max)

If block A runs, **then** block B runs → **add** costs, then simplify to the larger one.

```text
O(A) + O(B) → O(max(A, B))
```

```typescript
// Loop then another loop — NOT multiply
for (let i = 0; i < n; i++) { ... } // O(n)
for (let j = 0; j < n; j++) { ... } // O(n)
// O(n) + O(n) = O(2n) → O(n)
```

```typescript
for (let i = 0; i < n; i++) { ... }           // O(n)
for (let i = 0; i < n; i++) {
  for (let j = 0; j < n; j++) { ... }         // O(n²)
}
// O(n) + O(n²) → O(n²)
```

> **Do NOT multiply** just because you see two loops. Multiply only when one is **inside** the other.

#### Rule 4 — Nested code → MULTIPLY

If B runs **inside** A for every step of A → **multiply**.

```text
O(A) × O(B) → combined cost
```

```typescript
for (let i = 0; i < n; i++) {        // n times
  for (let j = 0; j < n; j++) { ... } // n times each
}
// O(n) × O(n) = O(n²)
```

```typescript
for (const word of words) {          // W words
  for (const ch of word) { ... }     // L chars each (avg)
}
// O(W × L) = O(total characters)
```

#### Rule 5 — Different input sizes → use different letters

Don’t force everything into one `n` if sizes differ.

```typescript
for (let i = 0; i < a.length; i++) {      // A
  for (let j = 0; j < b.length; j++) { ... } // B
}
// O(A × B)  — not automatically O(n²)
```

Matrix `R` rows × `C` cols → **O(R × C)**. If square → **O(N²)**.

#### Rule 6 — Work that does not depend on `n` → O(1)

```typescript
arr[0];
map.get(key);      // average
set.has(x);        // average
x + y;
```

`O(1)` inside a loop does **not** change the loop’s cost: still **O(n)** for one loop.

#### Rule 7 — Loop pattern cheatsheet

| Pattern | Complexity |
|---------|------------|
| No loop / fixed steps | O(1) |
| Halve (or grow slowly) each step | O(log n) |
| One loop `0..n` (`for` or `while`) | O(n) |
| Two loops **one after another** | O(n) |
| Two loops **nested** over `n` | O(n²) |
| Three nested loops over `n` | O(n³) |
| Sort once, then one loop | O(n log n) |
| Loop + binary search each time | O(n log n) |

#### Rule 8 — `while` and `while (true)`

`for` and `while` are the same for Big O — count **how many times the body runs**, not the keyword.

**A. `while (condition)` — same as a `for` loop**

Ask: how does the condition move toward false as `n` grows?

```typescript
// O(n) — i goes 0 → n
let i = 0;
while (i < n) {
  i++;
}

// O(log n) — n halves each time (binary search style)
let lo = 0, hi = n - 1;
while (lo <= hi) {
  const mid = Math.floor((lo + hi) / 2);
  if (arr[mid] === target) break;
  if (arr[mid] < target) lo = mid + 1;
  else hi = mid - 1;
}

// O(n) — shrink from both ends (two pointers)
let L = 0, R = n - 1;
while (L < R) {
  L++;
  R--; // still ~ n/2 iterations → O(n)
}
```

| How the loop advances | Complexity |
|-----------------------|------------|
| `i++` until `n` | O(n) |
| `n = n / 2` each time | O(log n) |
| Nested `while` over `n` | O(n²) |
| `while` then another `while` | ADD → usually O(n) |

**B. `while (true)` — infinite until `break`**

The condition is always true, so Big O comes from **when you `break` / `return`** (worst case).

```typescript
// Still O(n) — breaks after at most n steps
let i = 0;
while (true) {
  if (i >= n) break;
  i++;
}

// O(log n) — halves until done
let x = n;
while (true) {
  if (x <= 1) break;
  x = Math.floor(x / 2);
}

// Dangerous: no clear exit tied to n → may be infinite (not a valid algorithm bound)
while (true) {
  // missing break → does not terminate
}
```

**How to analyze `while (true)`:**
1. Find every `break` / `return`
2. Worst case: max iterations before exit
3. Express that as a function of `n` → that is your O(...)

```typescript
// Example: process queue until empty — O(V + E) for a graph BFS, not "infinite"
while (true) {
  if (queue.length === 0) break;
  const node = queue.shift()!;
  // visit neighbors...
}
```

> `while (true)` is **not** automatically infinite complexity. It is O(whatever bound your exit condition gives). If there is no bound, the algorithm may not terminate.

#### Rule 9 — Built-in / library costs count

Include the cost of methods you call:

| Call | Typical cost |
|------|----------------|
| `arr[i]`, `map.get`, `set.has` | O(1) avg |
| `arr.push` / `pop` | O(1) amortized |
| `arr.shift` / `unshift` | O(n) |
| `arr.includes` / `indexOf` | O(n) |
| `arr.slice` / spread `[...arr]` | O(n) |
| `arr.sort` | O(n log n) |
| `arr.reverse` | O(n) |

```typescript
for (let i = 0; i < n; i++) {
  if (arr.includes(x)) { ... } // O(n) each iteration
}
// O(n) × O(n) = O(n²)
```

#### Rule 10 — Recursion = (# of calls) × (work per call)

| Recursion shape | Complexity |
|-----------------|------------|
| 1 call, depth `n`, O(1) work | O(n) |
| 1 call, halve `n`, O(1) work | O(log n) |
| 2 calls per level, depth `n` | O(2ⁿ) |
| Divide into 2 halves + O(n) merge (merge sort) | O(n log n) |
| All permutations | O(n!) |

```typescript
function countdown(n: number): void {
  if (n === 0) return;
  countdown(n - 1); // O(n)
}

function f(n: number): void {
  if (n === 0) return;
  f(n - 1);
  f(n - 1); // O(2ⁿ)
}

function halve(n: number): void {
  if (n <= 1) return;
  halve(Math.floor(n / 2)); // O(log n)
}
```

**Master theorem (intuition):** `a` subproblems of size `n/b`, plus `O(nᵈ)` combine work → compare `log_b a` with `d` (merge sort: a=2, b=2, d=1 → O(n log n)).

#### Rule 11 — Big O usually means worst case

Early `return` / `break` may make **best** case fast, but Big O is usually **worst** case unless stated otherwise.

```typescript
// May return on first hit → best O(1)
// Still worst case O(n²)
```

#### Rule 12 — Space is separate

Analyze **extra memory** with the same add/multiply ideas (arrays, maps, recursion stack).

| Pattern | Extra space |
|---------|-------------|
| Few variables | O(1) |
| New array / copy of size n | O(n) |
| HashMap / Set up to n keys | O(n) |
| Recursion depth n | O(n) stack |

---

### Combining parts — examples

```typescript
function example(arr: number[]): void {
  console.log(arr[0]); // O(1)

  for (const x of arr) {
    // O(n)
    console.log(x);
  }

  for (let i = 0; i < arr.length; i++) {
    // O(n²) dominates overall
    for (let j = 0; j < arr.length; j++) {
      console.log(arr[i], arr[j]);
    }
  }
}
// Total: O(1) + O(n) + O(n²) → O(n²)
```

### Decision: add or multiply?

```text
Code A
Code B          →  ADD   →  O(A) + O(B)  → keep max

for (...) {
  Code B        →  MULTIPLY  →  O(A) × O(B)
}
```

| Situation | Do this |
|-----------|---------|
| Loop then another loop | **Add** → usually O(n) |
| Loop inside loop | **Multiply** → usually O(n²) |
| Several sequential blocks | **Add**, keep dominant |
| Helper called inside a loop | **Multiply** loop × helper cost |

---

## 4. Space Complexity (brief)

Same idea, but for **extra memory** (not counting input, unless asked).

```typescript
function clone(arr: number[]): number[] {
  return [...arr]; // O(n) extra space
}

function sumInPlace(arr: number[]): number {
  let s = 0; // O(1) extra space
  for (const x of arr) s += x;
  return s;
}
```

| Pattern | Extra space |
|---------|-------------|
| Few variables | O(1) |
| Copy of input / new array of size n | O(n) |
| Recursion depth `n` | O(n) call stack |
| HashMap of up to n keys | O(n) |

---

## 5. Worked Examples

### Example A — Two Sum

```typescript
// Brute: check every pair
function twoSumBrute(nums: number[], target: number): number[] {
  for (let i = 0; i < nums.length; i++) {
    for (let j = i + 1; j < nums.length; j++) {
      if (nums[i] + nums[j] === target) return [i, j];
    }
  }
  return [];
}
// Time: O(n²)  Space: O(1)
```

```typescript
// HashMap: one pass
function twoSumMap(nums: number[], target: number): number[] {
  const seen = new Map<number, number>();
  for (let i = 0; i < nums.length; i++) {
    const need = target - nums[i];
    if (seen.has(need)) return [seen.get(need)!, i];
    seen.set(nums[i], i);
  }
  return [];
}
// Time: O(n)  Space: O(n)
```

**How calculated:**
- Brute: outer `n`, inner ~`n` → `n * n` → **O(n²)**
- Map: one loop of `n`, each `get`/`set` average O(1) → **O(n)**

---

### Example B — Nested + early exit still counted as worst case

```typescript
function findPair(arr: number[]): boolean {
  for (let i = 0; i < arr.length; i++) {
    for (let j = i + 1; j < arr.length; j++) {
      if (arr[i] === arr[j]) return true; // may exit early
    }
  }
  return false;
}
// Best case: O(1) if first pair matches
// Worst case (Big O usually): O(n²)
```

Big O for algorithms usually means **worst case**, unless you say best/average.

---

### Example C — What is `n`?

```typescript
function process(matrix: number[][]): number {
  let sum = 0;
  const rows = matrix.length;
  const cols = matrix[0]?.length ?? 0;
  for (let r = 0; r < rows; r++) {
    for (let c = 0; c < cols; c++) {
      sum += matrix[r][c];
    }
  }
  return sum;
}
// If rows = R, cols = C → O(R * C)
// If square N×N → O(N²)
```

Always define what `n` (or `R`, `C`, `V`, `E`) is.

---

## 6. Summary Table

| Notation | Name | Rough growth (n=10⁶) | Typical pattern |
|----------|------|----------------------|-----------------|
| O(1) | Constant | 1 | Index, hash get |
| O(log n) | Logarithmic | ~20 | Binary search |
| O(n) | Linear | 10⁶ | Single loop |
| O(n log n) | Linearithmic | ~2×10⁷ | Sort, merge sort |
| O(n²) | Quadratic | 10¹² | Nested loops |
| O(n³) | Cubic | 10¹⁸ | Triple nested loops |
| O(2ⁿ) | Exponential | Impossible at large n | Naive recursion |
| O(n!) | Factorial | Impossible fast | Permutations |

---

## 7. Quick Checklist

When analyzing code:

1. What is **`n`**? (or `A`, `B`, `R`, `C`, `V`, `E`…)
2. Loops: **one after another → add**; **nested → multiply**
3. Recursion? (# calls) × (work per call)
4. Library calls? (`sort` ≈ O(n log n), `includes` ≈ O(n))
5. Simplify: drop constants + lower terms → **O(...)**
6. Note **space** separately if needed

```text
Rule 3: sequential loops     → ADD  (then keep max)
Rule 4: nested loops         → MULTIPLY
Rule 8: while / while(true)  → count iterations until exit (same as for)
Rule 1–2: simplify           → drop constants & smaller terms
halve n each time            → log n
fixed work                   → O(1)
```
