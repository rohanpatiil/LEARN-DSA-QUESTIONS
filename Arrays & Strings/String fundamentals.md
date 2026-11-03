# PART Array Fundamentals

## 1. What Is an Array?

An **array** is a collection of elements stored in an ordered sequence. In Python, the most common array-like structure is a **list**.

```python
nums =[10][20][30][40][50]
```

### Visualizing an Array

| Value  | 10 | 20 | 30 | 40 | 50 |
|--------|----|----|----|----|----|
| Index  | 0  | 1  | 2  | 3  | 4  |

### Important Rules

- Array indexing starts from `0`.
- `nums[0]` gives the first element.
- `nums[-1]` gives the last element in Python.
- The length of the array is found using `len(nums)`.

```python
nums =[10][20][30][40][50]

print(nums)   # 10
print(nums)   # 30[1]
print(nums[-1])  # 50
print(len(nums)) # 5
```

## 2. Common Array Operations

| Operation | Example | Time Complexity | Meaning |
|---|---|---:|---|
| Access | `nums[2]` | `O(1)` | Directly read an element using its index |
| Traversal | `for x in nums` | `O(n)` | Visit every element once |
| Search | Find `30` | `O(n)` | Check elements one by one |
| Append | `nums.append(60)` | `O(1)` amortized | Add an element at the end |
| Insert | `nums.insert(1, 15)` | `O(n)` | Add an element at a specific index |
| Delete | `nums.pop(1)` | `O(n)` | Remove an element from a specific index |

### Why Is Insertion `O(n)`?

When you insert an element in the middle, Python must shift all later elements to the right.

```python
nums =[10][20][30][40]
nums.insert(1, 15)

print(nums)
#[10][15][20][30][40]
```

## 3. Array Traversal

Traversal means visiting every element in the array.

### Method 1: Using Index

```python
nums =[10][20][30][40]

for i in range(len(nums)):
    print(nums[i])
```

Output:

```text
10
20
30
40
```

### Method 2: Direct Loop

```python
nums =[10][20][30][40]

for num in nums:
    print(num)
```

### When Should You Use Traversal?

Use traversal whenever you need to examine every element, for example:

- Calculate the sum of an array
- Find the maximum or minimum value
- Count elements matching a condition
- Search for a target value
- Modify or process every element

## 4. Example: Find the Maximum Element

### Brute-Force Thinking

1. Assume the first element is the maximum.
2. Visit every other element.
3. If you find a larger element, update the maximum.

```python
def find_max(nums):
    maximum = nums

    for num in nums:
        if num > maximum:
            maximum = num

    return maximum


nums =[10][20][30][40][50]
print(find_max(nums))
```

Output:

```text
50
```

### Complexity

- Time complexity: `O(n)`
- Space complexity: `O(1)`

We visit each element once, and we only store one extra variable called `maximum`.

## 5. Example: Reverse an Array

### Brute-Force Method

Create a new list and add elements from the end to the beginning.

```python
def reverse_array(nums):
    result = []

    for i in range(len(nums) - 1, -1, -1):
        result.append(nums[i])

    return result


nums =[10][20][30][40][50]
print(reverse_array(nums))
```

Output:

```text
[10][20][30][40][50]
```

### Complexity

- Time complexity: `O(n)`
- Space complexity: `O(n)`

This is because we create a new list of the same size.

## 6. Optimized Reverse Using Two Pointers

The **Two Pointers** technique uses two variables:

- `left`, starting from the beginning
- `right`, starting from the end

We swap elements and move both pointers toward the center.

```python
def reverse_array(nums):
    left = 0
    right = len(nums) - 1

    while left < right:
        nums[left], nums[right] = nums[right], nums[left]
        left += 1
        right -= 1

    return nums


nums =[10][20][30][40][50]
print(reverse_array(nums))
```

Output:

```text
[10][20][30][40][50]
```

### Step-by-Step Example

Initial array:

```text
[10][20][30][40][50]
```

Step 1: Swap `10` and `50`

```text
[10][20][30][40][50]
```

Step 2: Swap `20` and `40`

```text
[10][20][30][40][50]
```

Now `left` and `right` meet at the middle element, so the loop stops.

### Complexity

- Time complexity: `O(n)`
- Space complexity: `O(1)`

This is better than the brute-force method because it reverses the array **in place** and does not create a new list.

## 7. Two Pointers Pattern

The **Two Pointers** pattern is useful when you need to process an array from both ends or compare elements at different positions.

Common uses:

- Reversing an array
- Checking whether a string is a palindrome
- Finding pairs with a target sum in a sorted array
- Removing duplicates from a sorted array
- Rotating or partitioning arrays

### Basic Template

```python
def two_pointer_template(nums):
    left = 0
    right = len(nums) - 1

    while left < right:
        # Do something with nums[left] and nums[right]
        left += 1
        right -= 1
```

## 8. Practice Problems

| Level | Problem | LeetCode Number | Link |
|---|---|---:|---|
| Easy | Running Sum of 1D Array | #1480 | https://leetcode.com/problems/running-sum-of-1d-array/ |
| Medium | Rotate Array | #189 | https://leetcode.com/problems/rotate-array/ |
| Hard | First Missing Positive | #41 | https://leetcode.com/problems/first-missing-positive/ |

### Problem 1: Running Sum of 1D Array

Given an array `nums`, return the running sum of the array.

For example:

```text
Input:[1][2][3][4]
Output:[1][3][6][10]
```

Explanation:

```text
[1, 1+2, 1+2+3, 1+2+3+4]
```

This problem uses the idea of a **prefix sum**: each output position stores the sum of all elements up to that position. [4][6]

### Problem 2: Rotate Array

Given an array, rotate it to the right by `k` steps.

For example:

```text
Input:  nums =, k = 3[1][2][3][4][5][6][7]
Output:[1][2][3][4][5][6][7]
```

A common optimized solution reverses the whole array, then reverses selected portions. This gives `O(n)` time and `O(1)` extra space. [2][3]

### Problem 3: First Missing Positive

Given an unsorted integer array, find the smallest positive integer that is missing.

For example:

```text
Input:  [3, 4, -1, 1]
Output: 2
```

The optimal approach places each positive number `x` at index `x - 1` using in-place swaps, then scans for the first index whose value does not match. It targets `O(n)` time and `O(1)` auxiliary space. [5][7]

## 9. Useful Python Array Methods

```python
nums =[10][20][30][40][50]

nums.append(60)        # Add at end
nums.insert(2, 25)     # Insert at index 2
nums.pop()             # Remove last element
nums.pop(1)            # Remove element at index 1
nums.remove(30)        # Remove first occurrence of 30
nums.sort()            # Sort in ascending order
nums.reverse()         # Reverse the list
print(len(nums))       # Get length
print(sum(nums))       # Get sum
print(max(nums))       # Get maximum
print(min(nums))       # Get minimum
```

## 10. Learning Resources

### Python Arrays and Lists

- Python Official Documentation — Lists:  
  https://docs.python.org/3/tutorial/datastructures.html

- W3Schools — Python Lists:  
  https://www.w3schools.com/python/python_lists.asp

- GeeksforGeeks — Arrays in Python:  
  https://www.geeksforgeeks.org/python-arrays/

### Two Pointers

- LeetCode — Two Pointers Pattern:  
  https://leetcode.com/tag/two-pointers/

- GeeksforGeeks — Two Pointers Technique:  
  https://www.geeksforgeeks.org/two-pointers-technique/

### Big-O Complexity

- Big-O Cheat Sheet:  
  https://www.bigocheatsheet.com/

- GeeksforGeeks — Time Complexity Analysis:  
  https://www.geeksforgeeks.org/analysis-of-algorithms-set-1-asymptotic-analysis/

### Practice Platforms

- LeetCode:  
  https://leetcode.com/

- HackerRank:  
  https://www.hackerrank.com/

- Codeforces:  
  https://codeforces.com/

- NeetCode:  
  https://neetcode.io/

## 11. Quick Revision

- An array stores elements in order.
- Indexing starts from `0`.
- Access by index is `O(1)`.
- Searching, inserting, and deleting in the middle are usually `O(n)`.
- Traversal is used to process every element.
- Finding the maximum can be done in one loop.
- Reversing can be optimized using two pointers.
- Two pointers often reduce extra space by modifying the array in place.