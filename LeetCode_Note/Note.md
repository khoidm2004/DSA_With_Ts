# LeetCode Solutions Note in TypeScript

## 1.Two Sum
- Solution 1: num2 = target - item then find if num2 in array or not - O(n^2)
- Solution 2: similar to soulution 1 but use hashmap to store and retrieve index - O(n)

## 2. Palindrome Number
- Solution 1: Detach last digit then create reversed num = reverse num * 10 + last digit - O(log10 x)

## 3. Roman Integer
- Solution 1: if prev num < current num => result - prev num - O(n)