# TWO POINTERS PATTERN

This is one of the most important patterns in DSA.

## 3. What are Two Pointers?

Two pointers means maintaining two positions in an array or string to solve a problem efficiently.

There are two common styles:

1. Opposite direction: one pointer starts at the beginning and another at the end.
2. Same direction: both pointers move forward at different speeds or for different purposes.

## 3A. Two Sum in a sorted array

Example:

```python
nums = [1, 2, 4, 6, 8, 9]
target = 10
```

Find two numbers whose sum is 10.

Think manually:

- 1 + 9 = 10.
- We found our answer.

But how do we solve it efficiently?

### Brute force

Check every possible pair.

```python
def two_sum_brute(nums, target):
    for i in range(len(nums)):
        for j in range(i + 1, len(nums)):
            if nums[i] + nums[j] == target:
                return [i, j]

    return []
```

Time: O(n²)

Space: O(1)

### Optimized: Two Pointers

Since the array is sorted, we can use its ordering.

Our thinking:

- If sum is too small, move left forward.
- If sum is too large, move right backward.
- If sum equals target, return the pair.

```python
def two_sum_sorted(nums, target):
    left = 0
    right = len(nums) - 1

    while left < right:
        total = nums[left] + nums[right]

        if total == target:
            return [left, right]

        elif total < target:
            left += 1

        else:
            right -= 1

    return []
```

Time: O(n)

Space: O(1)

## 3B. Two Sum in an unsorted array

Example:

```python
nums = [4, 2, 7, 11]
target = 9
```

Here, we cannot safely use opposite-direction two pointers because the array is unsorted.

We have two approaches:

- Brute force: Check every pair.
- Optimized: Use a HashMap to remember previous numbers.

```python
def two_sum(nums, target):
    seen = {}

    for i, num in enumerate(nums):
        complement = target - num

        if complement in seen:
            return [seen[complement], i]

        seen[num] = i

    return []
```

Time: O(n) average

Space: O(n)

### Important distinction

| Input | Recommended approach |
|---|---|
| Sorted array | Two Pointers |
| Unsorted array | HashMap |
| Need all unique triplets | Sorting + Two Pointers |
| Need all unique quadruplets | Sorting + Two Pointers |

## 3C. Two pointers with strings

Example:

```python
s = "racecar"
```

Compare the first and last characters, then move inward.

```python
def palindrome(s):
    left = 0
    right = len(s) - 1

    while left < right:
        if s[left] != s[right]:
            return False

        left += 1
        right -= 1

    return True
```

### Two Pointers practice questions

| Level | Problem |
|---|---|
| Easy | Valid Palindrome |
| Medium | 3Sum |
| Hard | Trapping Rain Water |

LeetCode question numbers: Easy: #125 · Medium: #15 · Hard: #42
