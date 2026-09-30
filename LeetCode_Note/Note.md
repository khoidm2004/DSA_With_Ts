# LeetCode Solutions Note in TypeScript

## 1.Two Sum

- Solution 1: num2 = target - item then find if num2 in array or not - O(n^2)
- Solution 2: similar to soulution 1 but use hashmap to store and retrieve index - O(n)

## 2. Palindrome Number

- Solution 1: Detach last digit then create reversed num = reverse num \* 10 + last digit - O(log10 x)

## 3. Roman Integer

- Solution 1: if prev num < current num => result - prev num - O(n)

## 14. Logest Common Prefix

- Solution 1: Trie (Prefix tree) O(m*n) m length of word, n number of words
```ts
class TrieNode {
  children: Map<string, TrieNode> = new Map();
  isEnd = false;
}

function longestCommonPrefix(strs: string[]): string {
  const root = new TrieNode();

  for (const str of strs) {
    let node = root;
    for (const char of str) {
      let next = node.children.get(char);
      if (!next) {
        next = new TrieNode();
        node.children.set(char, next);
      }
      node = next;
    }
    node.isEnd = true;
  }

  let prefix = "";
  let node = root;
  while (node.children.size === 1 && node.isEnd !== true) {
    const [char, next] = [...node.children][0];
    prefix += char;
    node = next;
  }

  return prefix;
}
```
- Solution 2: Take first word as prefix then trim tll indexOf === 0 O(m*n)

## 20. Valid Parenthesis
- Solution 1: Stack insert open parenthesis and compare the rest using map O(n)