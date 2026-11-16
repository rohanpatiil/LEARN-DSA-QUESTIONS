# SLIDING WINDOW — COMPLETE BEGINNER TO ADVANCED GUIDE

Do not memorize the code first.

First learn:

1. What is a window?
2. When should I use Sliding Window?
3. Fixed-size vs variable-size window.
4. How do I move `left` and `right`?
5. When do I add/remove values?
6. Why does the optimized solution work?
7. When does Sliding Window NOT work?
8. How does the same idea work with strings?
9. How do I recognize the pattern in LeetCode?

---

# 1. WHAT IS SLIDING WINDOW?

Sliding Window is a technique used mainly for problems involving a **continuous/contiguous part** of an array or string.

For an array:

```text
[2, 1, 5, 1, 3, 2]
```

A window can be:

```text
[2, 1, 5]
```

Then slide:

```text
    [1, 5, 1]
```

Then:

```text
        [5, 1, 3]
```

Then:

```text
            [1, 3, 2]
```

Instead of creating every subarray again, we maintain the current window.

---

# 2. WHEN TO THINK SLIDING WINDOW?

Look for these keywords:

- subarray
- substring
- contiguous
- consecutive
- longest
- shortest
- maximum
- minimum
- exactly `k` elements
- at most `k`
- at least `k`
- window

The strongest clue is:

> **The problem is asking about a continuous section of an array or string.**

---

# 3. BEGINNER THINKING

Suppose:

```python
nums = [2, 1, 5, 1, 3, 2]
k = 3
```

Question:

> Find the maximum sum of 3 consecutive elements.

First think normally.

Possible windows:

```text
[2, 1, 5] = 8
[1, 5, 1] = 7
[5, 1, 3] = 9
[1, 3, 2] = 6
```

Answer:

```text
9
```

The brute-force solution recalculates every window.

Sliding Window says:

> "I already know the previous window's sum. Why calculate everything again?"

That is the main idea.

---

# 4. TWO MAIN TYPES

There are two major Sliding Window types:

## Type 1 — Fixed Size Window

The window size is fixed.

Examples:

```text
Find maximum sum of k consecutive elements.

Find average of every k consecutive elements.

Find maximum number of 1s in every k elements.
```

Window size:

```text
k
```

Example:

```text
[1, 2, 3, 4, 5]
 k = 3

[1, 2, 3]
   [2, 3, 4]
      [3, 4, 5]
```

---

## Type 2 — Variable Size Window

The window size changes according to a condition.

Examples:

```text
Minimum length subarray with sum >= target.

Longest substring without repeating characters.

Minimum window containing all characters of another string.
```

The window may look like:

```text
[2]
[2, 3]
[2, 3, 1]
[2, 3, 1, 2]
```

Then shrink:

```text
[3, 1, 2]
[1, 2]
```

---

# 5. FIXED-SIZE SLIDING WINDOW

## Problem

Maximum Average Subarray I

```text
nums = [1, 12, -5, -6, 50, 3]
k = 4
```

We need the maximum average of any 4 consecutive numbers.

---

# 5.1 BRUTE FORCE

Think:

For every starting index:

1. Take the next `k` elements.
2. Calculate their sum.
3. Calculate average.
4. Keep the maximum.

```python
def max_average_brute(nums, k):
    maximum = float("-inf")

    for start in range(len(nums) - k + 1):
        total = 0

        for i in range(start, start + k):
            total += nums[i]

        average = total / k
        maximum = max(maximum, average)

    return maximum
```

Complexity:

```text
Time: O(n * k)
Space: O(1)
```

---

# 5.2 SLIDING WINDOW THINKING

First calculate:

```text
first window
```

Then move one step.

Suppose:

```text
[1, 12, -5, -6]
```

Next window:

```text
[12, -5, -6, 50]
```

What changed?

We:

```text
remove 1
add 50
```

So:

```python
window_sum = window_sum - nums[left]
window_sum = window_sum + nums[right]
```

This avoids recalculating all `k` elements.

---

# 5.3 FIXED WINDOW TEMPLATE

```python
def fixed_window(nums, k):
    window_sum = sum(nums[:k])
    answer = window_sum

    for right in range(k, len(nums)):
        window_sum += nums[right]
        window_sum -= nums[right - k]

        answer = max(answer, window_sum)

    return answer
```

Time:

```text
O(n)
```

Space:

```text
O(1)
```

---

# 5.4 WHY IS IT O(n)?

This is important.

We do not have:

```text
for every window:
    loop k elements
```

Instead, every element:

- enters the window once
- leaves the window once

Therefore:

```text
O(n)
```

---

# 6. FIXED WINDOW — GENERAL TEMPLATE

When you see:

> "exactly k consecutive elements"

Think:

```python
def fixed_window(nums, k):

    window = ...

    # first window
    for i in range(k):
        window += nums[i]

    answer = ...

    # slide
    for right in range(k, len(nums)):

        # add new element
        window += nums[right]

        # remove old element
        window -= nums[right - k]

        # update answer
        answer = ...

    return answer
```

The exact operation may change.

For example:

```text
maximum sum
minimum sum
maximum count
minimum count
frequency
```

But the window movement is similar.

---

# 7. FIXED WINDOW WITH STRINGS

The same concept works for strings.

## Example

Find the maximum number of vowels in any substring of length `k`.

```text
s = "abciiidef"
k = 3
```

Windows:

```text
"abc"
"bci"
"cii"
"iii"
"iid"
"id e"
```

For each window, count vowels.

---

# 7.1 BRUTE FORCE

```python
def max_vowels_brute(s, k):
    maximum = 0

    for start in range(len(s) - k + 1):
        count = 0

        for i in range(start, start + k):
            if s[i] in "aeiou":
                count += 1

        maximum = max(maximum, count)

    return maximum
```

Time:

```text
O(n * k)
```

---

# 7.2 OPTIMIZED FIXED WINDOW

```python
def max_vowels(s, k):
    vowels = set("aeiou")

    count = 0

    for i in range(k):
        if s[i] in vowels:
            count += 1

    answer = count

    for right in range(k, len(s)):

        if s[right] in vowels:
            count += 1

        left = right - k

        if s[left] in vowels:
            count -= 1

        answer = max(answer, count)

    return answer
```

Time:

```text
O(n)
```

Space:

```text
O(1)
```

---

# 8. VARIABLE-SIZE SLIDING WINDOW

Now the important part.

The window size is NOT fixed.

Example:

```text
target = 7
nums = [2, 3, 1, 2, 4, 3]
```

Question:

> Find the minimum length subarray whose sum is at least 7.

---

# 8.1 BRUTE FORCE

Try every starting point and expand.

```python
def min_subarray_brute(target, nums):
    minimum = float("inf")

    for start in range(len(nums)):
        total = 0

        for end in range(start, len(nums)):
            total += nums[end]

            if total >= target:
                minimum = min(
                    minimum,
                    end - start + 1
                )
                break

    return 0 if minimum == float("inf") else minimum
```

Time:

```text
O(n²)
```

---

# 8.2 SLIDING WINDOW THINKING

Start:

```text
left = 0
```

Expand:

```text
right = 0
right = 1
right = 2
...
```

Keep adding values.

When:

```text
window_sum >= target
```

the window is valid.

Now we want the **smallest** valid window.

So:

```text
shrink from left
```

This is the key idea.

---

# 8.3 VARIABLE WINDOW TEMPLATE

```python
def variable_window(target, nums):

    left = 0
    window_sum = 0
    answer = float("inf")

    for right in range(len(nums)):

        window_sum += nums[right]

        while window_sum >= target:

            answer = min(
                answer,
                right - left + 1
            )

            window_sum -= nums[left]
            left += 1

    return 0 if answer == float("inf") else answer
```

Time:

```text
O(n)
```

Space:

```text
O(1)
```

---

# 9. WHY DOES THE VARIABLE WINDOW WORK?

For this problem, numbers are positive.

Example:

```text
[2, 3, 1, 2, 4, 3]
```

If the sum is too small:

```text
expand
```

If the sum is large enough:

```text
shrink
```

Because all values are positive:

- adding a value cannot decrease the sum
- removing a value cannot increase the sum

This makes the expand/shrink logic reliable.

---

# 10. VERY IMPORTANT — NEGATIVE NUMBERS

Standard Sliding Window does NOT automatically work for every subarray-sum problem.

For example:

```text
nums = [2, -5, 10]
```

Adding `-5` can decrease the sum.

Therefore:

```text
expand -> sum increases
```

is no longer guaranteed.

And:

```text
shrink -> sum decreases
```

is also no longer guaranteed.

For problems involving arbitrary negative numbers, consider:

```text
Prefix Sum
Prefix Sum + HashMap
Monotonic Queue
```

depending on the exact problem.

This distinction is extremely important.

---

# 11. VARIABLE WINDOW WITH STRINGS

Now we move to one of the most common interview patterns.

## Problem

Longest Substring Without Repeating Characters

```text
s = "abcabcbb"
```

Answer:

```text
"abc"
```

Length:

```text
3
```

---

# 11.1 BEGINNER THINKING

The question says:

```text
longest
substring
without repeating
```

This gives us three clues:

```text
substring -> continuous
longest -> sliding window
without repeating -> set/hashmap
```

So:

```text
Sliding Window + Set
```

---

# 11.2 BRUTE FORCE

Generate every substring and check whether it has duplicates.

```python
def longest_unique_brute(s):

    maximum = 0

    for start in range(len(s)):

        seen = set()

        for end in range(start, len(s)):

            if s[end] in seen:
                break

            seen.add(s[end])

            maximum = max(
                maximum,
                end - start + 1
            )

    return maximum
```

Time:

```text
O(n²)
```

Space:

```text
O(k)
```

---

# 11.3 OPTIMIZED SLIDING WINDOW

Use a set.

Window:

```text
left ---------------- right
```

Expand `right`.

If the new character is not present:

```text
add it
```

If it already exists:

```text
remove characters from left
```

until the duplicate is gone.

---

# 11.4 CODE

```python
def length_of_longest_substring(s):

    left = 0
    seen = set()
    maximum = 0

    for right in range(len(s)):

        while s[right] in seen:
            seen.remove(s[left])
            left += 1

        seen.add(s[right])

        maximum = max(
            maximum,
            right - left + 1
        )

    return maximum
```

Time:

```text
O(n)
```

Space:

```text
O(k)
```

---

# 12. THE MOST IMPORTANT SLIDING WINDOW FORMULA

For a window:

```text
left ... right
```

Its length is:

```python
right - left + 1
```

Remember this.

Examples:

```text
left = 2
right = 5

length = 5 - 2 + 1
       = 4
```

---

# 13. SET VS HASHMAP IN SLIDING WINDOW

Sometimes a Set is enough.

Example:

> Longest substring without repeating characters.

We only need to know:

```text
Does this character exist?
```

Use:

```python
seen = set()
```

But sometimes we need counts.

Example:

> Minimum Window Substring.

We need to know:

```text
How many A?
How many B?
How many C?
```

Then use:

```python
freq = {}
```

So:

```text
Need existence -> Set

Need frequency -> HashMap
```

---

# 14. LONGEST WINDOW PROBLEMS

Typical question:

> Find the longest substring/subarray satisfying a condition.

General thinking:

```text
right -> expand

condition becomes invalid

left -> shrink

update maximum
```

Template:

```python
left = 0
answer = 0

for right in range(len(nums)):

    # add nums[right]

    while window_is_invalid:

        # remove nums[left]
        left += 1

    answer = max(
        answer,
        right - left + 1
    )
```

---

# 15. MINIMUM WINDOW PROBLEMS

Typical question:

> Find the smallest substring/subarray satisfying a condition.

General thinking:

```text
right -> expand

condition becomes valid

left -> shrink

update minimum
```

Template:

```python
left = 0
answer = float("inf")

for right in range(len(nums)):

    # add nums[right]

    while window_is_valid:

        answer = min(
            answer,
            right - left + 1
        )

        # remove nums[left]
        left += 1
```

Remember:

```text
Longest -> usually update while window is valid

Shortest -> shrink while window is valid
```

---

# 16. "AT MOST K" PROBLEMS

This is another important Sliding Window pattern.

Example:

> Longest substring containing at most K distinct characters.

```text
s = "eceba"
k = 2
```

Answer:

```text
"ece"
```

Why?

It has two distinct characters:

```text
e
c
```

---

# 16.1 THINKING

Use a frequency HashMap.

Expand right.

If distinct characters become greater than `k`:

```text
shrink from left
```

until valid again.

---

# 16.2 CODE

```python
def longest_substring_k_distinct(s, k):

    left = 0
    freq = {}
    maximum = 0

    for right in range(len(s)):

        ch = s[right]
        freq[ch] = freq.get(ch, 0) + 1

        while len(freq) > k:

            left_ch = s[left]

            freq[left_ch] -= 1

            if freq[left_ch] == 0:
                del freq[left_ch]

            left += 1

        maximum = max(
            maximum,
            right - left + 1
        )

    return maximum
```

Time:

```text
O(n)
```

Space:

```text
O(k)
```

---

# 17. "AT LEAST K" PROBLEMS

Typical logic:

```text
Need a condition to become valid.

Expand until valid.

Then shrink if possible.
```

For example:

> Minimum Size Subarray Sum

```text
sum >= target
```

Once valid:

```text
shrink
```

This is why you should always ask:

> "What makes my current window valid?"

and:

> "What makes my current window invalid?"

---

# 18. MINIMUM WINDOW SUBSTRING

This is one of the most important advanced Sliding Window problems.

Problem:

```text
s = "ADOBECODEBANC"
t = "ABC"
```

Find the smallest substring of `s` containing all characters of `t`.

Answer:

```text
"BANC"
```

---

# 18.1 WHY IS THIS HARD?

Because we need to maintain:

1. Current window.
2. Required character frequencies.
3. Current character frequencies.
4. Number of required characters satisfied.
5. Expand logic.
6. Shrink logic.
7. Minimum answer.

This combines:

```text
Sliding Window
+
HashMap
+
Frequency Counting
+
Two Pointers
```

---

# 18.2 FIRST BUILD THE REQUIRED FREQUENCY

For:

```text
t = "ABC"
```

We need:

```python
need = {
    "A": 1,
    "B": 1,
    "C": 1
}
```

---

# 18.3 EXPAND

Move `right`.

Add characters to the window.

Example:

```text
ADOBEC
```

Now the window contains:

```text
A
B
C
```

So it is valid.

---

# 18.4 SHRINK

Now move `left`.

Try to make the window smaller while keeping it valid.

Eventually:

```text
BANC
```

is the smallest valid window.

---

# 18.5 CODE

```python
def min_window(s, t):

    if not s or not t:
        return ""

    need = {}

    for ch in t:
        need[ch] = need.get(ch, 0) + 1

    window = {}

    have = 0
    required = len(need)

    left = 0

    minimum_length = float("inf")
    minimum_start = 0

    for right in range(len(s)):

        ch = s[right]

        window[ch] = window.get(ch, 0) + 1

        if ch in need and window[ch] == need[ch]:
            have += 1

        while have == required:

            current_length = right - left + 1

            if current_length < minimum_length:
                minimum_length = current_length
                minimum_start = left

            left_ch = s[left]

            window[left_ch] -= 1

            if (
                left_ch in need
                and window[left_ch] < need[left_ch]
            ):
                have -= 1

            left += 1

    if minimum_length == float("inf"):
        return ""

    return s[
        minimum_start:
        minimum_start + minimum_length
    ]
```

Time:

```text
O(n + m)
```

Space:

```text
O(k)
```

where:

```text
n = length of s
m = length of t
k = number of distinct characters
```

---

# 19. THE UNIVERSAL SLIDING WINDOW PROCESS

When you see a Sliding Window problem, ask these questions.

## Step 1 — Is it continuous?

Look for:

```text
subarray
substring
contiguous
consecutive
```

If yes, Sliding Window may be possible.

---

## Step 2 — Is the window fixed?

Question says:

```text
exactly k elements
length k
every k consecutive elements
```

Use:

```text
Fixed Window
```

---

## Step 3 — Is the window variable?

Question says:

```text
longest
shortest
minimum length
maximum length
at most k
at least target
contains all
```

Consider:

```text
Variable Window
```

---

## Step 4 — What makes the window valid?

Examples:

```text
sum >= target

number of distinct characters <= k

no duplicate characters

contains all characters of t
```

Write this condition clearly before coding.

---

## Step 5 — What should happen when invalid?

Usually:

```text
move left
```

---

## Step 6 — What should happen when valid?

Depends on the problem.

For maximum/longest:

```text
record answer
```

For minimum/shortest:

```text
record answer
then try to shrink
```

---

# 20. SLIDING WINDOW TEMPLATE CHEAT SHEET

## Fixed Window

Use when window size = `k`.

```python
window = ...

for i in range(k):
    # build first window

answer = ...

for right in range(k, len(nums)):

    # add right
    window += nums[right]

    # remove left
    window -= nums[right - k]

    # update answer
```

---

## Variable Window — Longest

```python
left = 0
answer = 0

for right in range(len(nums)):

    # add nums[right]

    while window_is_invalid:

        # remove nums[left]
        left += 1

    answer = max(
        answer,
        right - left + 1
    )
```

---

## Variable Window — Shortest

```python
left = 0
answer = float("inf")

for right in range(len(nums)):

    # add nums[right]

    while window_is_valid:

        answer = min(
            answer,
            right - left + 1
        )

        # remove nums[left]
        left += 1
```

---

# 21. COMMON SLIDING WINDOW DATA STRUCTURES

## Sum

```python
window_sum += nums[right]
window_sum -= nums[left]
```

---

## Set

Use when you need existence/uniqueness.

```python
seen.add(value)

seen.remove(value)
```

---

## HashMap / Dictionary

Use when you need frequency.

```python
freq[value] = freq.get(value, 0) + 1
```

When removing:

```python
freq[value] -= 1
```

If it becomes zero:

```python
del freq[value]
```

---

# 22. COMMON MISTAKES

## Mistake 1 — Forgetting `+1`

Wrong:

```python
right - left
```

Correct window length:

```python
right - left + 1
```

---

## Mistake 2 — Moving left without removing its value

If you do:

```python
left += 1
```

you usually also need to remove:

```python
nums[left]
```

or:

```python
s[left]
```

from the maintained window state.

---

## Mistake 3 — Using Sliding Window with negative numbers blindly

Do not assume:

```text
sum >= target
```

can always be solved with normal expand/shrink logic.

Negative numbers can break the monotonic behavior.

---

## Mistake 4 — Using a Set when frequency is required

If the question says:

```text
at least 2 occurrences
exactly 3 occurrences
contains characters with required counts
```

you probably need a HashMap.

---

## Mistake 5 — Shrinking at the wrong time

For a minimum answer:

```text
while valid:
    update answer
    shrink
```

For a longest valid window:

```text
while invalid:
    shrink

update answer
```

---

# 23. SLIDING WINDOW VS TWO POINTERS

These patterns are related but not identical.

## Two Pointers

Usually focuses on two positions.

Example:

```text
left ->

        <-

      right
```

Common with:

```text
sorted arrays
palindromes
pair problems
```

---

## Sliding Window

The two pointers define a continuous window.

```text
left -> [ window ] <- right
```

Common with:

```text
subarrays
substrings
longest/shortest continuous sections
```

Sliding Window is essentially a specialized use of two-pointer movement.

---

# 24. SLIDING WINDOW VS PREFIX SUM

Use Sliding Window when the problem has a condition that allows you to maintain and adjust a window.

Typical:

```text
positive numbers
fixed window
longest substring
at most K
minimum length
```

Use Prefix Sum when you need cumulative information, especially when negative values make normal window shrinking unreliable.

Example:

```text
Find number of subarrays whose sum equals K
```

Often:

```text
Prefix Sum + HashMap
```

is the correct approach.

---

# 25. REAL-WORLD EXAMPLES

## Example 1 — Website monitoring

Suppose you have requests per minute:

```text
100
120
150
200
180
130
```

Question:

> What was the maximum number of requests during any 3-minute period?

This is:

```text
Fixed Sliding Window
```

---

## Example 2 — Fraud detection

Suppose you want:

> Find the shortest continuous period where transaction amount reaches at least ₹50,000.

If transaction amounts are positive:

```text
Variable Sliding Window
```

---

## Example 3 — Search/autocomplete

Suppose a user types:

```text
"abcabcbb"
```

You want to find the longest section without repeating characters.

This is similar to:

```text
Longest Substring Without Repeating Characters
```

Use:

```text
Sliding Window + Set
```

---

## Example 4 — Log analysis

You receive a stream of events and want:

> Maximum errors in the last 5 minutes.

This is conceptually a sliding window over time.

The window is not necessarily based on array index; it can be based on timestamps.

---

# 26. LEETCODE PRACTICE ROADMAP

Do not solve these randomly.

Follow the order below.

---

# LEVEL 1 — BASIC / EASY

## 1. Maximum Average Subarray I

What to learn:

```text
Fixed-size window
Window sum
Add right
Remove left
```

LeetCode:

```text
#643
```

---

## 2. Maximum Number of Vowels in a Substring of Given Length

What to learn:

```text
Fixed-size window
String
Counting
```

LeetCode:

```text
#1456
```

---

## 3. Contains Duplicate II

What to learn:

```text
Sliding Window
Set
Distance between indices
```

LeetCode:

```text
#219
```

---

# LEVEL 2 — MEDIUM

## 4. Minimum Size Subarray Sum

What to learn:

```text
Variable window
Positive numbers
Expand
Shrink
Minimum length
```

LeetCode:

```text
#209
```

---

## 5. Longest Substring Without Repeating Characters

What to learn:

```text
Variable window
Set
String
Longest valid window
```

LeetCode:

```text
#3
```

---

## 6. Longest Repeating Character Replacement

What to learn:

```text
Sliding Window
Frequency HashMap
At most K replacements
```

LeetCode:

```text
#424
```

---

## 7. Permutation in String

What to learn:

```text
Fixed-size window
String frequency
HashMap
```

LeetCode:

```text
#567
```

---

# LEVEL 3 — HARD

## 8. Minimum Window Substring

What to learn:

```text
Variable window
HashMap
Required frequency
Current frequency
Expand
Shrink
Minimum valid window
```

LeetCode:

```text
#76
```

---

## 9. Sliding Window Maximum

What to learn:

```text
Fixed-size window
Monotonic Deque
Efficient maximum
```

LeetCode:

```text
#239
```

Important:

This is an advanced Sliding Window problem.

Do not worry if the Deque concept is not clear yet.

---

## 10. Substring with Concatenation of All Words

What to learn:

```text
Sliding Window
HashMap
Fixed word length
Multiple frequency conditions
```

LeetCode:

```text
#30
```

---

# 27. EXTRA PRACTICE BY PATTERN

## Fixed Window

Practice:

```text
#643 — Maximum Average Subarray I
#1456 — Maximum Number of Vowels in a Substring of Given Length
#567 — Permutation in String
#438 — Find All Anagrams in a String
#2461 — Maximum Sum of Distinct Subarrays With Length K
```

---

## Variable Window — Longest

Practice:

```text
#3 — Longest Substring Without Repeating Characters
#340 — Longest Substring with At Most K Distinct Characters
#424 — Longest Repeating Character Replacement
#1004 — Max Consecutive Ones III
```

---

## Variable Window — Shortest

Practice:

```text
#209 — Minimum Size Subarray Sum
#76 — Minimum Window Substring
#1234 — Replace the Substring for Balanced String
```

---

## Advanced Window + Deque

Practice:

```text
#239 — Sliding Window Maximum
#1438 — Longest Continuous Subarray With Absolute Diff Less Than or Equal to Limit
```

---

# 28. YOUR FIRST 3 QUESTIONS TO MASTER

If you are a complete beginner, do NOT start with #76.

Start with these:

## Question 1 — Easy

```text
#643 — Maximum Average Subarray I
```

Learn:

```text
Fixed window
```

---

## Question 2 — Medium

```text
#209 — Minimum Size Subarray Sum
```

Learn:

```text
Variable window
Positive numbers
Expand + shrink
```

---

## Question 3 — Medium

```text
#3 — Longest Substring Without Repeating Characters
```

Learn:

```text
String
Variable window
Set
Longest valid window
```

After these three are clear, move to:

```text
#424
#567
#438
#76
#239
```

---

# 29. HOW TO READ A SLIDING WINDOW LEETCODE QUESTION

Before writing code, write these five things on paper.

```text
1. What is my window?

2. What makes the window valid?

3. What makes the window invalid?

4. When do I move right?

5. When do I move left?
```

Then identify:

```text
Fixed or Variable?

Array or String?

Sum, Set, or HashMap?

Longest or Shortest?

Positive numbers or negative numbers?
```

---

# 30. EXAMPLE OF THINKING BEFORE CODING

Question:

> Given an array of positive integers, find the minimum length subarray whose sum is greater than or equal to target.

Before coding, write:

```text
subarray
    ↓
continuous
    ↓
Sliding Window candidate

positive integers
    ↓
normal expand/shrink works

minimum length
    ↓
shrink while valid

sum >= target
    ↓
window validity condition
```

Then code.

This is the type of thinking you should practice.

---

# 31. FINAL SLIDING WINDOW DECISION TREE

```text
Is it about a continuous subarray/substring?
            |
           YES
            |
            v
      Is window size fixed?
        /             \
      YES              NO
       |                |
       v                v
 Fixed Window      Variable Window
       |                |
       |          What is the goal?
       |          /             \
       |       Longest         Shortest
       |          |               |
       |          v               v
       |       Expand         Expand
       |       if invalid     if valid
       |       shrink         record
       |       update         shrink
       |
       v
 Add right
 Remove left
 Update answer
```

---

# 32. THE MOST IMPORTANT RULES TO REMEMBER

```text
Rule 1:
Sliding Window is mainly for continuous/contiguous data.

Rule 2:
Fixed size -> maintain exactly k elements.

Rule 3:
Variable size -> expand right and conditionally shrink left.

Rule 4:
Longest -> make window valid, then maximize.

Rule 5:
Shortest -> when valid, record answer and keep shrinking.

Rule 6:
Set -> uniqueness/existence.

Rule 7:
HashMap -> frequency/count.

Rule 8:
Positive numbers make many sum-based variable windows work naturally.

Rule 9:
Negative numbers can break the normal sum-based Sliding Window logic.

Rule 10:
Always understand the brute-force solution before optimizing.
```

---

# 33. FINAL PRACTICE ORDER

Follow this exact order:

```text
STEP 1
#643
Maximum Average Subarray I

        ↓

STEP 2
#1456
Maximum Number of Vowels in a Substring of Given Length

        ↓

STEP 3
#219
Contains Duplicate II

        ↓

STEP 4
#209
Minimum Size Subarray Sum

        ↓

STEP 5
#3
Longest Substring Without Repeating Characters

        ↓

STEP 6
#567
Permutation in String

        ↓

STEP 7
#424
Longest Repeating Character Replacement

        ↓

STEP 8
#438
Find All Anagrams in a String

        ↓

STEP 9
#76
Minimum Window Substring

        ↓

STEP 10
#239
Sliding Window Maximum
```

Do not move forward just because you can copy the solution.

For each problem, you should be able to explain:

```text
Why is this a Sliding Window problem?

What is the window?

Why do I move right?

Why do I move left?

What makes the window valid?

What makes the window invalid?

Why does my solution take O(n)?

Why doesn't the brute-force solution work efficiently?
```

If you can answer those questions, you have actually learned Sliding Window rather than memorized a template.
