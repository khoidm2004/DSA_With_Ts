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
3. **Express growth** — how many times does the main work run as a function of `n`?
4. **Simplify**
   - Drop constants: `3n` → `O(n)`
   - Drop lower-order terms: `n² + 5n + 10` → `O(n²)`
   - Keep the **fastest-growing** term

### Rules of thumb

| Pattern | Complexity |
|---------|------------|
| No loop / fixed work | O(1) |
| Halving each step | O(log n) |
| One loop `0..n` | O(n) |
| Loop + binary search / sort inside once | O(n log n) often |
| Two nested loops over `n` | O(n²) |
| Three nested loops over `n` | O(n³) |
| Recursion with 2 branches, depth `n` | ~O(2ⁿ) |
| All permutations | O(n!) |

### Combining parts

- **Sequential** (A then B): take the **max** → `O(n) + O(n²) = O(n²)`
- **Nested** (B inside A): **multiply** → `O(n) * O(n) = O(n²)`
- **Independent of n**: ignore → `O(1)` work inside a loop still leaves the loop’s cost

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

### Recursion

Ask: **how many calls?** and **work per call?**

```typescript
// Depth n, 1 call per level → O(n)
function countdown(n: number): void {
  if (n === 0) return;
  countdown(n - 1);
}

// Depth n, 2 calls per level → O(2ⁿ)
function f(n: number): void {
  if (n === 0) return;
  f(n - 1);
  f(n - 1);
}

// Divide in half each time → O(log n) calls (if O(1) work each)
function halve(n: number): void {
  if (n <= 1) return;
  halve(Math.floor(n / 2));
}
```

**Master theorem (intuition):** divide into `a` subproblems of size `n/b`, plus `O(nᵈ)` merge work → compare `log_b a` with `d` to get O(n log n), O(nᵈ), etc. (e.g. merge sort: a=2, b=2, d=1 → O(n log n)).

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

1. What is **`n`**?
2. Are there **loops**? Nested? How deep?
3. Is there **recursion**? Branching factor × depth?
4. Any **library** calls? (`sort` ≈ O(n log n), `includes` on array ≈ O(n))
5. **Simplify** → keep dominant term → write **O(...)**
6. Separately note **space** if needed

```text
loops nested over n     → multiply
code after code         → take max
halve n each time       → log n
fixed work              → 1
```
