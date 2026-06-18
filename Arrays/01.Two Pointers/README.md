
# Two Pointers: Processing Data from Two Positions

### Goal

Use two positions inside an array, string, or linked list to solve problems efficiently and avoid unnecessary comparisons.

---

# 1. Core Idea

The Two Pointers pattern uses two variables that point to different positions in a data structure. Instead of checking every possible pair using nested loops, we intelligently move one or both pointers according to some rule.

The main idea is simple:

> Use information gathered so far to eliminate unnecessary work.

This often transforms an O(n²) brute-force solution into an O(n) solution.

Two Pointers is commonly used in arrays, strings, and linked lists.

---

# 2. Why This Pattern Matters

Two Pointers is one of the most important patterns in Data Structures and Algorithms.

It helps:

1. Reduce brute-force solutions.
2. Improve time complexity.
3. Build intuition for Sliding Window.
4. Solve many array and string problems.
5. Develop pattern recognition skills.

Many interview problems are based on this pattern.

---

# 3. When to Use This Pattern

## The Checklist

Think about Two Pointers when:

1. Working with arrays or strings.

2. Looking for pairs of elements.

3. The array is sorted.

4. Processing data from both ends.

5. Removing duplicates.

6. Reversing arrays or strings.

7. Optimizing a nested-loop solution.

---

## Key Phrases to Watch For

Problems containing phrases like:

* "Find a pair..."
* "Sorted array..."
* "Remove duplicates..."
* "Palindrome..."
* "Reverse..."
* "Merge..."

often suggest Two Pointers.

---

# 4. Theory Behind the Pattern

The strength of Two Pointers comes from gradually reducing the search space.

Instead of checking every possible combination, pointer movements eliminate impossible answers.

Each move preserves useful information and brings us closer to the answer.

Since each element is usually visited at most once, the overall complexity becomes linear.

---

# 5. Common Variations

---

## A. Opposite Direction Pointers

One pointer starts from the beginning and the other starts from the end.

### Visualization

```text
1 2 3 4 6

L       R
```

After movement:

```text
1 2 3 4 6

  L   R
```

### How It Works

Compare both values and decide which pointer should move.

### Mini Example

Target = 7

| Step    | Left Value | Right Value | Sum | Action       |
| ------- | ---------- | ----------- | --- | ------------ |
| Initial | 1          | 6           | 7   | Found Answer |

### Common Problems

* Two Sum II
* Valid Palindrome
* Container With Most Water

### Template

```cpp
int left = 0;
int right = n - 1;

while(left < right)
{
    // Process current elements

    if(condition)
        left++;
    else
        right--;
}
```

---

## B. Fast and Slow Pointers

One pointer moves slowly while the other moves faster.

### Visualization

```text
1 → 2 → 3 → 4 → 5

S
F
```

After one iteration:

```text
1 → 2 → 3 → 4 → 5

    S

        F
```

### How It Works

Fast pointer usually moves twice while slow pointer moves once.

### Mini Example

| Iteration | Slow | Fast |
| --------- | ---- | ---- |
| Initial   | 1    | 1    |
| 1         | 2    | 3    |
| 2         | 3    | 5    |

Middle node = 3

### Common Problems

* Middle of Linked List
* Linked List Cycle
* Happy Number

### Template

```cpp
ListNode* slow = head;
ListNode* fast = head;

while(fast && fast->next)
{
    slow = slow->next;
    fast = fast->next->next;
}
```

---

## C. Same Direction Pointers

Both pointers move in the same direction.

### Visualization

```text
0 1 0 3 12

S F
```

After movement:

```text
0 1 0 3 12

  S     F
```

### How It Works

One pointer explores while the other tracks the position where useful values should be placed.

### Mini Example

Input:

[1,1,2,2,3]

| Slow | Fast | Action          |
| ---- | ---- | --------------- |
| 0    | 1    | Duplicate, skip |
| 1    | 2    | Place 2         |
| 2    | 3    | Duplicate, skip |
| 2    | 4    | Place 3         |

### Common Problems

* Move Zeroes
* Remove Duplicates from Sorted Array

### Template

```cpp
int slow = 0;

for(int fast = 0; fast < n; fast++)
{
    if(condition)
    {
        // process
        slow++;
    }
}
```

---

# 6. From Brute Force to Optimized Solution

## Brute Force

### Idea

Check every possible pair.

### Time Complexity

O(n²)

### Skeleton

```cpp
for(int i=0;i<n;i++)
{
    for(int j=i+1;j<n;j++)
    {
        // check pair
    }
}
```

---

## Optimized Approach

### Idea

Use two pointers and move them intelligently.

### Time Complexity

O(n)

### Skeleton

```cpp
int left = 0;
int right = n-1;

while(left < right)
{
    // process

    // move pointers
}
```

---

# 7. Complexity Analysis

| Complexity       | Value |
| ---------------- | ----- |
| Time Complexity  | O(n)  |
| Space Complexity | O(1)  |

---

# 8. Common Pitfalls

## Infinite Loop

### Wrong

```cpp
while(left < right)
{
    // forgot to move pointers
}
```

### Correct

```cpp
while(left < right)
{
    left++;
}
```

---

## Wrong Boundary Condition

### Wrong

```cpp
while(left <= right)
```

### Correct

```cpp
while(left < right)
```

depending on the problem.

---

## Moving the Wrong Pointer

Always understand why a pointer should move.

Wrong movement can lead to incorrect answers.

---

# 9. Related Patterns

```text
Brute Force
     ↓
Two Pointers
     ↓
Sliding Window
     ↓
Prefix Sum
```

Sliding Window can be viewed as a specialized version of Two Pointers.

---

# 10. Pattern Recognition Flowchart

```text
Array or String?
        ↓
Need better than O(n²)?
        ↓
Sorted?
        ↓
Need pair or comparison?
        ↓
Think Two Pointers
```

---

# 11. Example Problem

## Problem Information

| Problem                                                         | Difficulty | Pattern                     |
| --------------------------------------------------------------- | ---------- | --------------------------- |
| [Reverse String](https://leetcode.com/problems/reverse-string/) | Easy       | Opposite Direction Pointers |

---

### Intuition

We need to reverse the array without using extra space.

Instead of creating another array, we swap the first and last characters, then move inward until both pointers meet.

---

### Code

```cpp
class Solution {
public:
    void reverseString(vector<char>& s) {

        int left = 0;
        int right = s.size()-1;

        while(left < right)
        {
            swap(s[left], s[right]);

            left++;
            right--;
        }
    }
};
```

---

### Dry Run

Input:

```text
['h','e','l','l','o']
```

| Step    | Left | Right | Operation     | Array State |
| ------- | ---- | ----- | ------------- | ----------- |
| Initial | 0    | 4     | Start         | h e l l o   |
| 1       | 0    | 4     | Swap h and o  | o e l l h   |
| 2       | 1    | 3     | Swap e and l  | o l l e h   |
| Stop    | 2    | 2     | Pointers meet | o l l e h   |

Output:

```text
['o','l','l','e','h']
```

---

# 12. Practice Problems

## Easy (Build Intuition)

| Problem                             | Link                                                               |
| ----------------------------------- | ------------------------------------------------------------------ |
| Reverse String                      | https://leetcode.com/problems/reverse-string/                      |
| Valid Palindrome                    | https://leetcode.com/problems/valid-palindrome/                    |
| Merge Sorted Array                  | https://leetcode.com/problems/merge-sorted-array/                  |
| Move Zeroes                         | https://leetcode.com/problems/move-zeroes/                         |
| Remove Duplicates from Sorted Array | https://leetcode.com/problems/remove-duplicates-from-sorted-array/ |

---

## Intermediate (Strengthen Pattern Recognition)

| Problem                   | Link                                                            |
| ------------------------- | --------------------------------------------------------------- |
| Two Sum II                | https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/ |
| Container With Most Water | https://leetcode.com/problems/container-with-most-water/        |
| 3Sum                      | https://leetcode.com/problems/3sum/                             |
| Sort Colors               | https://leetcode.com/problems/sort-colors/                      |
| Squares of a Sorted Array | https://leetcode.com/problems/squares-of-a-sorted-array/        |

---

## Hard (Challenge Yourself)

| Problem                     | Link                                                       |
| --------------------------- | ---------------------------------------------------------- |
| Trapping Rain Water         | https://leetcode.com/problems/trapping-rain-water/         |
| Minimum Window Substring    | https://leetcode.com/problems/minimum-window-substring/    |
| 4Sum                        | https://leetcode.com/problems/4sum/                        |
| Median of Two Sorted Arrays | https://leetcode.com/problems/median-of-two-sorted-arrays/ |
