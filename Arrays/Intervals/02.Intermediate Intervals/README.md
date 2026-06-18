
# Intermediate Intervals: Greedy Scheduling

### Goal

Make optimal decisions on an interval-based timeline (e.g., maximizing the number of events attended or minimizing resource removals) using greedy strategies.

---

# 1. Core Idea

Intermediate Interval problems move beyond simple merging; they require **selection**. We must choose which intervals to keep or remove to satisfy a specific objective.

The secret is the **Greedy Choice**:

* To **maximize non-overlapping events**, always pick the interval that **ends earliest**. By finishing as early as possible, you minimize the "footprint" left on the timeline, leaving the maximum amount of space for future events.

> Sort by end time, then greedily pick the earliest finisher to maximize your options.

---

# 2. Why This Pattern Matters

This pattern teaches you how to move from "organizing data" to "optimizing outcomes."

It helps:

1. Solve scheduling conflicts in $O(n \log n)$ time.
2. Build intuition for Greedy Algorithms, which are crucial for complex system design.
3. Handle interval data represented as a 2D array in an optimized, logical sequence.

---

# 3. When to Use This Pattern

## The Checklist

Think about Intermediate Intervals when:
01. You are given a 2D array of `[start, end]` intervals.
02. You are asked to **maximize** the number of meetings or events.
03. You are asked to **minimize** the number of removals to resolve overlaps.
04 You are asked to find the minimum resources (arrows/lines) to pierce all intervals.

## Key Phrases to Watch For

* "Maximum number of non-overlapping intervals/meetings..."
* "Minimum number of intervals to remove..."
* "Find the minimum number of arrows to burst all balloons..."
* "Can a person attend all meetings?"

---

# 4. Theory Behind the Pattern

The **Greedy Choice Property** is the theoretical backbone. If you have two events, `A [1, 10]` and `B [2, 3]`:

* If you pick `A`, you block the timeline until `10`.
* If you pick `B`, you finish by `3`, leaving the timeline from `3` to `infinity` wide open.

By always finishing as early as possible, you maximize your chances of fitting in the next event.

---

# 5. Greedy Decision Logic

In intermediate problems, we must make a decision based on the `lastEndTime`:

* **Conflict Condition:** `currentStart < lastEndTime`
* **If Conflict:** * Keep the interval that finishes earlier.
* Remove the interval that blocks more future choices.


* **If No Conflict:** Attend the interval and update `lastEndTime` to the current interval's end.

---

# 6. Greedy Selection Template

*Used for: Non-overlapping Intervals, Burst Balloons*

```cpp
sort(intervals.begin(), intervals.end(), [](auto& a, auto& b) {
    return a[1] < b[1]; // Sort by end time
});
int count = 0;
int lastEnd = intervals[0][1];
for (int i = 1; i < intervals.size(); i++) {
    if (intervals[i][0] < lastEnd) {
        count++; // Conflict: remove or fire arrow
    } else {
        lastEnd = intervals[i][1]; // Update boundary
    }
}

```

---

# 7. Meeting Rooms I Template

*Used for: Meeting Rooms I*

```cpp
sort(intervals.begin(), intervals.end()); // Sort by start time
for (int i = 1; i < intervals.size(); i++) {
    if (intervals[i][0] < intervals[i-1][1]) {
        return false; // Collision detected
    }
}
return true;

```

---

# 8. Complexity Analysis

| Complexity | Value | Reason |
| --- | --- | --- |
| **Time Complexity** | **$O(n \log n)$** | Sorting dominates the complexity. The greedy sweep is $O(n)$. |
| **Space Complexity** | **$O(1)$** | Constant space required for the selection logic. |

---

# 9. Common Pitfalls

* **Sorting by Start Time:** Only use start time sorting for *merging* (Basic). For *selection/removal* (Intermediate), you must sort by **end time**.
* **Boundary conditions:** Ensure you handle equality correctly (`<` vs `<=`). If one meeting ends exactly when another starts, it is typically NOT an overlap.

---

# 10. Related Patterns

```text
Sorting
     ↓
Intervals
     ↓
Greedy Algorithm

```

---

# 11. Pattern Recognition Flowchart

```text
2D Array of [start, end]?
        ↓
Need to MERGE ranges? -> Basic Intervals
        ↓
Otherwise:
    • Need to maximize meetings?
    • Need to minimize removals?
    • Need minimum arrows/resources?
        ↓
Sort by END Time -> Greedy Sweep

```

---



---

13. Example Problem: Non-overlapping IntervalsIntuitionTo keep the maximum number of intervals, we must remove the minimum number of overlapping ones. By sorting by end time, we always pick the interval that finishes earliest. This "Greedy Choice" is optimal because it leaves the most possible room for subsequent intervals to fit into the timeline.Code (C++)C++int eraseOverlapIntervals(vector<vector<int>>& intervals) {
    if (intervals.empty()) return 0;
    
    // Sort by end time to make the greedy choice
    sort(intervals.begin(), intervals.end(), [](const vector<int>& a, const vector<int>& b) {
        return a[1] < b[1];
    });
    
    int count = 0;
    int lastEnd = intervals[0][1];
    
    for (size_t i = 1; i < intervals.size(); i++) {
        if (intervals[i][0] < lastEnd) {
            // Conflict found! We must remove this interval
            count++; 
        } else {
            // No conflict: update the end boundary to the current interval
            lastEnd = intervals[i][1];
        }
    }
    return count;
}
Dry Run TableInput: intervals = [[1,2], [2,3], [3,4], [1,3]]StepActionlastEndcount (Removed)Note1Sort by end time-0Array: [[1,2], [2,3], [1,3], [3,4]]2Start at [1,2]20First interval kept3Check [2,3]302 >= 2 (No conflict), Update lastEnd4Check [1,3]311 < 3 (Conflict!), Increment count5Check [3,4]413 >= 3 (No conflict), Update lastEndFinal Result: 1 interval removed.Complexity AnalysisTime Complexity: $O(n \log n)$The sort() function is the most expensive operation, taking $O(n \log n)$.The single for loop iterates through the list once, which is $O(n)$.Total: $O(n \log n) + O(n) = \mathbf{O(n \log n)}$.Space Complexity: $O(1)$We only use a few integer variables (count, lastEnd, i) to keep track of our state. We do not create any additional data structures proportional to the input size, so it is $O(1)$ constant space.
-----------
#  Identifying Intermediate Problems

* You are asked to count how many items to "remove."
* You are asked to fit the "maximum number of meetings" possible.
* You need to "find the minimum number of points" to pierce all intervals (e.g., "minimum number of arrows").
* -------------
### Practice Problems

| Order | Problem | Difficulty | Link |
| --- | --- | --- | --- |
| 1 | Non-overlapping Intervals | Medium | [Link](https://leetcode.com/problems/non-overlapping-intervals/) |
| 2 | Meeting Rooms I | Medium | (LeetCode Premium) |
| 3 | Min Arrows to Burst Balloons | Medium | [Link](https://leetcode.com/problems/minimum-number-of-arrows-to-burst-balloons/) |

---
