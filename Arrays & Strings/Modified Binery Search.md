# MODIFIED BINARY SEARCH — COMPLETE BEGINNER TO ADVANCED GUIDE

# 1. What Is Binary Search?

Binary Search is a searching technique that repeatedly cuts the search space into half.

Instead of checking every element:

```text
0 → 1 → 2 → 3 → 4 → 5 → 6 → ...
```

we check the middle:

```text
left -------- mid -------- right
```

Then we eliminate one half.

This gives:

```text
Brute Force: O(n)
Binary Search: O(log n)
```

---

# 2. When To Think Binary Search?

Look for these clues:

- sorted array
- sorted numbers
- search for a target
- find first/last position
- find minimum/maximum possible value
- rotated sorted array
- search in a monotonic condition
- answer lies in a range
- minimize/maximize something

Important:

> Do not think only "sorted array = binary search."

There are also problems where the ARRAY is not sorted, but the ANSWER has a monotonic pattern.

---

# 3. Core Rule

If you see:

```text
SORTED + SEARCH
```

think:

```text
BINARY SEARCH
```

If you see:

```text
ANSWER RANGE + YES/NO CHECK
```

think:

```text
BINARY SEARCH ON ANSWER
```

---

# 4. Basic Binary Search

## Problem

Given a sorted array, find the index of `target`.

Example:

```text
nums = [1, 3, 5, 7, 9, 11]
target = 7
```

Answer:

```text
3
```

---

# Thinking Like A Beginner

We have:

```text
left = 0
right = 5
```

Find middle:

```text
mid = (left + right) // 2
```

If:

```text
nums[mid] == target
```

we found it.

If:

```text
nums[mid] < target
```

target must be on the RIGHT.

So:

```text
left = mid + 1
```

If:

```text
nums[mid] > target
```

target must be on the LEFT.

So:

```text
right = mid - 1
```

---

# Visualization

Example:

```text
[1, 3, 5, 7, 9, 11]
       ↑
      mid
```

Target = `7`

Middle is `5`.

Because:

```text
5 < 7
```

discard everything on the left including `5`.

Now:

```text
[7, 9, 11]
 ↑
left
```

---

# Code

```python
def search(nums, target):
    left = 0
    right = len(nums) - 1

    while left <= right:
        mid = (left + right) // 2

        if nums[mid] == target:
            return mid

        elif nums[mid] < target:
            left = mid + 1

        else:
            right = mid - 1

    return -1
```

---

# Complexity

```text
Time: O(log n)
Space: O(1)
```

---

# Important Binary Search Template

Memorize the logic, not just the code:

```python
left = 0
right = len(nums) - 1

while left <= right:

    mid = (left + right) // 2

    if condition:
        return mid

    elif nums[mid] < target:
        left = mid + 1

    else:
        right = mid - 1
```

---

# 5. Why Is It O(log n)?

Suppose there are:

```text
16 elements
```

Every step removes half:

```text
16
8
4
2
1
```

So only about:

```text
log2(16) = 4
```

checks are needed.

---

# 6. Important Cases

Binary Search has several important versions.

Learn them in this order:

```text
1. Exact Search
2. First Position
3. Last Position
4. Lower Bound
5. Upper Bound
6. Search Insert Position
7. Rotated Sorted Array
8. Find Minimum in Rotated Array
9. Binary Search on Answer
10. Advanced problems
```

---

# EASY

# 7. Binary Search

LeetCode #704

### Pattern

```text
sorted array
+
find exact target
```

Use normal binary search.

---

# 8. Search Insert Position

LeetCode #35

Example:

```text
nums = [1, 3, 5, 6]
target = 5
```

Answer:

```text
2
```

If target does not exist:

```text
target = 2
```

answer:

```text
1
```

because `2` should be inserted before `3`.

---

# Thinking

This is not only:

```text
"Did I find target?"
```

It is:

```text
"Where should target be placed?"
```

At the end, `left` often represents the correct insertion position.

---

# Code

```python
def searchInsert(nums, target):
    left = 0
    right = len(nums) - 1

    while left <= right:
        mid = (left + right) // 2

        if nums[mid] == target:
            return mid

        elif nums[mid] < target:
            left = mid + 1

        else:
            right = mid - 1

    return left
```

---

# 9. Find First and Last Position of Element

LeetCode #34

Example:

```text
nums = [5, 7, 7, 8, 8, 10]
target = 8
```

Answer:

```text
[3, 4]
```

Normal binary search may find either `8`.

But we need:

```text
first 8
last 8
```

So we modify binary search.

---

# First Occurrence

When we find target:

```python
if nums[mid] == target:
```

do NOT immediately return.

Instead:

```python
answer = mid
right = mid - 1
```

Why?

Because there may be another target on the left.

---

# Last Occurrence

When we find target:

```python
answer = mid
left = mid + 1
```

Why?

Because there may be another target on the right.

---

# Code

```python
def first_position(nums, target):
    left = 0
    right = len(nums) - 1
    answer = -1

    while left <= right:
        mid = (left + right) // 2

        if nums[mid] == target:
            answer = mid
            right = mid - 1

        elif nums[mid] < target:
            left = mid + 1

        else:
            right = mid - 1

    return answer


def last_position(nums, target):
    left = 0
    right = len(nums) - 1
    answer = -1

    while left <= right:
        mid = (left + right) // 2

        if nums[mid] == target:
            answer = mid
            left = mid + 1

        elif nums[mid] < target:
            left = mid + 1

        else:
            right = mid - 1

    return answer
```

---

# 10. Binary Search With Duplicates

This is an important interview variation.

Example:

```text
[1, 2, 2, 2, 3]
```

Question:

```text
Where is the first 2?
```

Normal binary search is not enough.

You need to decide:

```text
Found target → should I continue LEFT?
```

or:

```text
Found target → should I continue RIGHT?
```

This idea appears again and again in advanced binary search.

---

# MEDIUM

# 11. Search in Rotated Sorted Array

LeetCode #33

Example:

```text
[4, 5, 6, 7, 0, 1, 2]
```

Target:

```text
0
```

This array was originally:

```text
[0, 1, 2, 4, 5, 6, 7]
```

but it was rotated.

---

# Important Thinking

Question says:

```text
SORTED BUT ROTATED
```

This means:

> One half is always sorted.

At every step:

```text
left ---- mid ---- right
```

Check which half is sorted.

---

# Step 1

If:

```python
nums[left] <= nums[mid]
```

then:

```text
LEFT HALF IS SORTED
```

Otherwise:

```text
RIGHT HALF IS SORTED
```

---

# Step 2

If left half is sorted:

```text
nums[left] <= target < nums[mid]
```

target is inside the left half.

So:

```python
right = mid - 1
```

Otherwise:

```python
left = mid + 1
```

---

# Code

```python
def search_rotated(nums, target):
    left = 0
    right = len(nums) - 1

    while left <= right:
        mid = (left + right) // 2

        if nums[mid] == target:
            return mid

        # Left half is sorted
        if nums[left] <= nums[mid]:

            if nums[left] <= target < nums[mid]:
                right = mid - 1
            else:
                left = mid + 1

        # Right half is sorted
        else:

            if nums[mid] < target <= nums[right]:
                left = mid + 1
            else:
                right = mid - 1

    return -1
```

---

# Rotated Array Mental Model

Always ask:

```text
Which half is sorted?
```

Then ask:

```text
Can target exist inside that sorted half?
```

If YES:

```text
search there
```

If NO:

```text
search other half
```

---

# 12. Find Minimum in Rotated Sorted Array

LeetCode #153

Example:

```text
[3, 4, 5, 1, 2]
```

Answer:

```text
1
```

---

# Thinking

We do not need to find a target.

We need to find the rotation point/minimum.

Compare:

```python
nums[mid]
```

with:

```python
nums[right]
```

If:

```python
nums[mid] > nums[right]
```

minimum must be on the RIGHT.

Otherwise:

```python
right = mid
```

---

# Code

```python
def findMin(nums):
    left = 0
    right = len(nums) - 1

    while left < right:
        mid = (left + right) // 2

        if nums[mid] > nums[right]:
            left = mid + 1
        else:
            right = mid

    return nums[left]
```

---

# Important Difference

Normal binary search:

```text
while left <= right
```

Find minimum:

```text
while left < right
```

Why?

Because we are narrowing toward one final position.

---

# 13. Find Peak Element

LeetCode #162

Example:

```text
[1, 2, 3, 1]
```

Peak:

```text
3
```

---

# Thinking

Compare:

```text
nums[mid]
nums[mid + 1]
```

If:

```python
nums[mid] < nums[mid + 1]
```

we are going uphill.

Therefore a peak exists on the right.

```python
left = mid + 1
```

Otherwise:

```python
right = mid
```

---

# Code

```python
def findPeakElement(nums):
    left = 0
    right = len(nums) - 1

    while left < right:
        mid = (left + right) // 2

        if nums[mid] < nums[mid + 1]:
            left = mid + 1
        else:
            right = mid

    return left
```

---

# Important Idea

Binary search does NOT always mean:

```text
find target
```

It can also mean:

```text
find a position where a condition changes
```

This is a very important transition to advanced binary search.

---

# 14. Search a 2D Matrix

LeetCode #74

Example:

```text
1  3  5  7
10 11 16 20
23 30 34 60
```

The matrix behaves like one sorted array.

You can treat it as:

```text
[1, 3, 5, 7, 10, 11, 16, 20, 23, 30, 34, 60]
```

---

# Thinking

Total elements:

```python
rows * columns
```

Use binary search over indexes:

```text
0 → rows * columns - 1
```

Convert one-dimensional index into row/column:

```python
row = mid // columns
col = mid % columns
```

---

# Code

```python
def searchMatrix(matrix, target):
    rows = len(matrix)
    cols = len(matrix[0])

    left = 0
    right = rows * cols - 1

    while left <= right:
        mid = (left + right) // 2

        row = mid // cols
        col = mid % cols

        value = matrix[row][col]

        if value == target:
            return True

        elif value < target:
            left = mid + 1

        else:
            right = mid - 1

    return False
```

---

# 15. Time Complexity

For matrix:

```text
rows × columns = n
```

Binary search:

```text
O(log(rows × columns))
```

Space:

```text
O(1)
```

---

# 16. Find First Bad Version

LeetCode #278

This is an important example because the array itself may not be given.

Versions look like:

```text
1 2 3 4 5 6 7
```

Suppose:

```text
1 2 3 4 = good
5 6 7 = bad
```

Question:

```text
Find first bad version.
```

---

# Thinking

The result has a pattern:

```text
GOOD GOOD GOOD GOOD BAD BAD BAD
```

This is called a:

```text
MONOTONIC CONDITION
```

We can binary search the point where:

```text
False → True
```

---

# Code

```python
def firstBadVersion(n):
    left = 1
    right = n

    while left < right:
        mid = (left + right) // 2

        if isBadVersion(mid):
            right = mid
        else:
            left = mid + 1

    return left
```

---

# 17. Binary Search on Answer

This is one of the most important advanced concepts.

Sometimes there is no sorted array.

Instead, there is a possible answer range.

Example:

```text
1 2 3 4 5 6 7 8 9 10
```

Question:

```text
What is the minimum possible answer?
```

You create a function:

```text
can_we_do_it(answer)
```

If:

```text
answer works
```

then maybe larger answers also work.

This creates:

```text
False False False True True True
```

or:

```text
True True True False False False
```

Now binary search can find the boundary.

---

# 18. How To Recognize Binary Search on Answer?

Look for:

- minimum possible
- maximum possible
- minimize maximum
- maximize minimum
- capacity
- speed
- distance
- time
- allocation
- scheduling
- can we finish?
- is it possible?
- feasible?

Especially when the answer has a numeric range.

---

# 19. Example: Koko Eating Bananas

LeetCode #875

Question:

Koko eats bananas at speed `k`.

Find the minimum speed needed to finish within `h` hours.

---

# Brute Force Thinking

Try:

```text
k = 1
k = 2
k = 3
k = 4
...
```

This can be slow.

---

# Binary Search Thinking

Possible speed:

```text
1 → max(piles)
```

For each speed:

```text
Can Koko finish within h hours?
```

If YES:

try smaller speed.

If NO:

try larger speed.

---

# Code

```python
def minEatingSpeed(piles, h):
    left = 1
    right = max(piles)

    while left < right:
        mid = (left + right) // 2

        hours = 0

        for pile in piles:
            hours += (pile + mid - 1) // mid

        if hours <= h:
            right = mid
        else:
            left = mid + 1

    return left
```

---

# Important Formula

Ceiling division:

```python
(pile + speed - 1) // speed
```

Example:

```text
pile = 10
speed = 3
```

Normal:

```text
10 // 3 = 3
```

But we need:

```text
4 hours
```

So:

```python
(10 + 3 - 1) // 3
= 12 // 3
= 4
```

---

# 20. Another Binary Search on Answer Example

## Capacity To Ship Packages Within D Days

LeetCode #1011

Question:

Find the minimum ship capacity.

Possible capacity:

```text
max(weights) → sum(weights)
```

For every capacity ask:

```text
Can all packages be shipped within D days?
```

Pattern:

```text
capacity
   ↓
can we finish?
   ↓
YES → smaller capacity
NO  → larger capacity
```

---

# 21. Aggressive Cows / Magnetic Force Style

LeetCode #1552 — Magnetic Force Between Two Balls

This is another important:

```text
maximize the minimum distance
```

type problem.

Possible answer:

```text
1 → max_position - min_position
```

For a candidate distance:

```text
Can we place all balls with at least this distance?
```

If YES:

```text
try larger distance
```

If NO:

```text
try smaller distance
```

This is:

```text
Binary Search on Answer
```

---

# 22. Lower Bound

Lower Bound means:

> Find the first index where `nums[index] >= target`.

Example:

```text
nums = [1, 3, 3, 5, 7]
target = 3
```

Answer:

```text
1
```

---

# Code

```python
def lower_bound(nums, target):
    left = 0
    right = len(nums)

    while left < right:
        mid = (left + right) // 2

        if nums[mid] < target:
            left = mid + 1
        else:
            right = mid

    return left
```

Notice:

```python
right = len(nums)
```

not:

```python
len(nums) - 1
```

because we are finding a position/boundary.

---

# 23. Upper Bound

Upper Bound means:

> Find the first index where `nums[index] > target`.

Example:

```text
nums = [1, 3, 3, 5, 7]
target = 3
```

Answer:

```text
3
```

because index `3` contains `5`.

---

# Code

```python
def upper_bound(nums, target):
    left = 0
    right = len(nums)

    while left < right:
        mid = (left + right) // 2

        if nums[mid] <= target:
            left = mid + 1
        else:
            right = mid

    return left
```

---

# 24. Lower Bound vs Upper Bound

For:

```text
[1, 3, 3, 3, 5]
```

target:

```text
3
```

Lower Bound:

```text
first >= 3
```

answer:

```text
1
```

Upper Bound:

```text
first > 3
```

answer:

```text
4
```

Therefore:

```text
count of target = upper_bound - lower_bound
```

---

# 25. Binary Search vs Modified Binary Search

Normal Binary Search:

```text
Find exact target
```

Modified Binary Search:

```text
Use binary search logic
+
change the condition
+
find a boundary/position/pattern
```

Examples:

```text
First occurrence
Last occurrence
Peak
Rotated array
Minimum in rotated array
First bad version
Lower bound
Upper bound
Binary search on answer
```

---

# HARD

# 26. Median of Two Sorted Arrays

LeetCode #4

This is an advanced binary search problem.

Do not start with this problem.

First master:

```text
#704
#35
#34
#33
#153
#162
#278
#875
#1011
```

Then study #4.

---

# Core Idea

We have two sorted arrays:

```text
A
B
```

We want to split them into:

```text
LEFT PART | RIGHT PART
```

such that:

```text
all left values <= all right values
```

Then calculate the median from the boundary values.

The difficult part is finding the correct partition efficiently.

---

# Important Rule

For this problem:

```text
Binary Search on the smaller array
```

This keeps the complexity:

```text
O(log(min(m, n)))
```

---

# 27. Another Hard Problem

## Sliding Window Maximum

LeetCode #239

This is NOT a normal binary search problem.

It is usually solved using:

```text
Deque / Monotonic Queue
```

Do not force binary search just because the word "maximum" appears.

---

# 28. Problem Recognition Checklist

When reading a question, ask:

### Question 1

Is the data sorted?

```text
YES → consider binary search
```

### Question 2

Is the data rotated?

```text
YES → modified binary search
```

### Question 3

Do I need first/last occurrence?

```text
YES → boundary binary search
```

### Question 4

Is there a monotonic TRUE/FALSE condition?

```text
YES → binary search
```

### Question 5

Is the answer a number in a range?

```text
YES → binary search on answer
```

### Question 6

Does the question ask:

```text
minimum possible
maximum possible
minimum capacity
maximum distance
minimum speed
```

Think:

```text
Binary Search on Answer
```

---

# 29. Binary Search Decision Tree

```text
Is the array sorted?
        |
       YES
        |
        v
Need exact target?
        |
       YES
        |
        v
Normal Binary Search
```

If:

```text
Sorted + duplicates
```

think:

```text
First / Last / Lower Bound / Upper Bound
```

If:

```text
Sorted + rotated
```

think:

```text
Modified Binary Search
```

If:

```text
Find peak/minimum/boundary
```

think:

```text
Modified Binary Search
```

If:

```text
Answer is numeric range
+
can(mid) gives YES/NO
```

think:

```text
Binary Search on Answer
```

---

# 30. Common Mistakes

## Mistake 1 — Wrong Middle

Use:

```python
mid = (left + right) // 2
```

---

## Mistake 2 — Infinite Loop

Wrong:

```python
left = mid
```

Sometimes this does not move.

Usually use:

```python
left = mid + 1
```

or:

```python
right = mid - 1
```

For boundary searches, use the correct `left/right = mid` pattern intentionally.

---

## Mistake 3 — Wrong Loop Condition

Exact search often uses:

```python
while left <= right
```

Boundary search often uses:

```python
while left < right
```

Do not memorize blindly.

Understand what your search interval represents.

---

## Mistake 4 — Forgetting Sorted Requirement

Normal binary search requires an ordering/monotonic property.

You cannot randomly apply it to:

```text
[8, 2, 9, 1, 7]
```

---

## Mistake 5 — Returning Immediately

For:

```text
first occurrence
```

finding target does NOT mean return.

You may need to continue searching left.

For:

```text
last occurrence
```

continue searching right.

---

# 31. Brute Force vs Binary Search

Always think about the brute-force solution first.

Example:

```text
Find target
```

Brute force:

```python
for i in range(len(nums)):
    if nums[i] == target:
        return i
```

Complexity:

```text
O(n)
```

Sorted array allows:

```text
O(log n)
```

That is the optimization.

---

# 32. Real-World Applications

Binary Search is useful in:

### Database

Finding a record in sorted/indexed data.

### Log systems

Finding the first log after a particular timestamp.

### Version systems

Finding the first version containing a bug.

### Capacity planning

Finding minimum server capacity required.

### Scheduling

Finding minimum time required to complete jobs.

### Networking

Finding thresholds/limits.

### Performance testing

Finding the first input size where a system becomes too slow.

---

# 33. How To Practice

Do not solve random binary-search questions.

Follow this order.

---

## LEVEL 1 — Basic

### 1. Binary Search

LeetCode #704

Learn:

```text
left
right
mid
discard half
```

### 2. Search Insert Position

LeetCode #35

Learn:

```text
boundary
```

### 3. First and Last Position

LeetCode #34

Learn:

```text
first
last
duplicates
```

---

# LEVEL 2 — Modified Array

### 4. Search in Rotated Sorted Array

LeetCode #33

Learn:

```text
one half is sorted
```

### 5. Find Minimum in Rotated Sorted Array

LeetCode #153

Learn:

```text
find rotation point
```

### 6. Find Peak Element

LeetCode #162

Learn:

```text
condition-based binary search
```

---

# LEVEL 3 — Boundary / Monotonic

### 7. First Bad Version

LeetCode #278

Learn:

```text
False → True
```

### 8. Search a 2D Matrix

LeetCode #74

Learn:

```text
binary search over transformed indexes
```

---

# LEVEL 4 — Binary Search on Answer

### 9. Koko Eating Bananas

LeetCode #875

Learn:

```text
minimum speed
```

### 10. Capacity To Ship Packages Within D Days

LeetCode #1011

Learn:

```text
minimum capacity
```

### 11. Magnetic Force Between Two Balls

LeetCode #1552

Learn:

```text
maximize minimum distance
```

---

# LEVEL 5 — Advanced

### 12. Median of Two Sorted Arrays

LeetCode #4

Learn:

```text
partition
binary search on smaller array
```

---

# 34. Easy / Medium / Hard Practice

## EASY

| Problem | LeetCode | Pattern |
|---|---:|---|
| Binary Search | #704 | Exact search |
| Search Insert Position | #35 | Boundary |
| First Bad Version | #278 | Monotonic boundary |

---

## MEDIUM

| Problem | LeetCode | Pattern |
|---|---:|---|
| Find First and Last Position | #34 | First/last |
| Search in Rotated Sorted Array | #33 | Rotated |
| Find Minimum in Rotated Sorted Array | #153 | Rotated minimum |
| Find Peak Element | #162 | Condition |
| Search a 2D Matrix | #74 | Matrix binary search |
| Koko Eating Bananas | #875 | Binary search on answer |
| Capacity To Ship Packages Within D Days | #1011 | Binary search on answer |

---

## HARD

| Problem | LeetCode | Pattern |
|---|---:|---|
| Median of Two Sorted Arrays | #4 | Partition |
| Split Array Largest Sum | #410 | Binary search on answer |
| Minimum Number of Days to Make m Bouquets | #1482 | Binary search on answer |
| Minimize Max Distance to Gas Station | #774 | Advanced answer search |

---

# 35. Most Important Templates

## Template 1 — Exact Search

```python
left = 0
right = len(nums) - 1

while left <= right:
    mid = (left + right) // 2

    if nums[mid] == target:
        return mid

    elif nums[mid] < target:
        left = mid + 1

    else:
        right = mid - 1

return -1
```

---

# Template 2 — Find Minimum Position

```python
left = 0
right = len(nums) - 1

while left < right:
    mid = (left + right) // 2

    if condition:
        right = mid
    else:
        left = mid + 1

return left
```

---

# Template 3 — Binary Search on Answer

```python
left = minimum_possible
right = maximum_possible

while left < right:
    mid = (left + right) // 2

    if can_do(mid):
        right = mid
    else:
        left = mid + 1

return left
```

The key is:

```text
can_do(mid)
```

must be monotonic.

---

# 36. Final Mental Model

When you see a problem, think:

```text
SORTED?
   |
   +---- YES ----> Binary Search
   |
ROTATED?
   |
   +---- YES ----> Modified Binary Search
   |
FIRST/LAST?
   |
   +---- YES ----> Boundary Search
   |
PEAK/MINIMUM?
   |
   +---- YES ----> Condition-Based Binary Search
   |
MIN/MAX ANSWER?
   |
   +---- YES ----> Binary Search on Answer
   |
TRUE/FALSE MONOTONIC?
   |
   +---- YES ----> Binary Search
```

---

# 37. What You Should Be Able To Explain After This

Before moving to advanced DSA, you should be able to answer:

1. Why does binary search require sorted/monotonic data?
2. Why is it `O(log n)`?
3. What does `left` mean?
4. What does `right` mean?
5. What does `mid` mean?
6. When do you move `left`?
7. When do you move `right`?
8. Why does rotated binary search work?
9. How do you find first occurrence?
10. How do you find last occurrence?
11. What is lower bound?
12. What is upper bound?
13. What is a monotonic condition?
14. What is binary search on answer?
15. How do you design `can(mid)`?
16. Why does Koko Eating Bananas use binary search?
17. Why does Capacity to Ship Packages use binary search?
18. Why is Median of Two Sorted Arrays much harder?

If you can explain these without looking at the code, your binary search foundation is strong.

---

# 38. Recommended Learning Order

```text
#704
  ↓
#35
  ↓
#34
  ↓
#33
  ↓
#153
  ↓
#162
  ↓
#278
  ↓
#74
  ↓
#875
  ↓
#1011
  ↓
#1552
  ↓
#410
  ↓
#4
```

Do not jump directly to #4.

First become comfortable with:

```text
Exact Search
    ↓
Boundary Search
    ↓
Rotated Array
    ↓
Condition Search
    ↓
Binary Search on Answer
    ↓
Advanced Partition Problems
```
"""
path = Path("/mnt/data/Modified_Binary_Search_Complete_Guide.md")
path.write_text(content, encoding="utf-8")
print(path)