
 # Algorithm: Stack

### Goal

Efficiently process elements in LIFO (Last-In, First-Out) order to manage nested structures, reverse data, or track local extrema (next/previous greater/smaller elements).

---

## 1. Core Idea

A **Stack** is a linear data structure that restricts access to one end—the "top." Elements are added (`push`) and removed (`pop`) only from this top position, ensuring that the last element added is the first one available for retrieval.

### Theory

The Stack operates on the **LIFO (Last-In, First-Out)** principle. This creates a "history-aware" structure, allowing you to access the most recently seen information instantly without scanning previous data.

---

## 2. Why Stack Matters

1. **Efficiency:** Optimizes brute-force $O(n^2)$ solutions for searching previous/next neighbors to $O(n)$.
2. **Reverse Processing:** Naturally handles problems requiring reverse-order traversal.
3. **Hierarchy Management:** Essential for parsing nested structures like parentheses, XML/HTML tags, and expression trees.
4. **Foundation:** Acts as the base for **Depth-First Search (DFS)**, Recursion (Call Stack), and complex Expression Parsing.
5. **State Tracking:** Simplifies problems involving undo operations or tracking historical state.

---

## 3. When to Use

### Checklist

* You need to access the "most recently added" element.
* The problem involves nested structures or recursive logic.
* You need to look for a "nearest" neighbor (next greater, previous smaller).
* The order of processing must be the reverse of the input order.

### Key Phrases to Watch For

* "Nearest greater/smaller element..."
* "Matching brackets or tags..."
* "Evaluate expression (Infix/Prefix/Postfix)..."
* "Undo operations..."
* "Previous or next element..."

---

## 4. Typical Elements of the Pattern

### A. The Container

Usually a `std::stack` (C++), `java.util.Stack`, or even a simple `vector`/`list` used as a stack.

### B. Push & Pop

The fundamental operations that maintain the LIFO property.

### C. Peek/Top

Accessing the current "top" element without removing it.

---

## 5. Common Patterns

| Pattern | Common Applications |
| --- | --- |
| **Balanced Parentheses** | Validating brackets, nested structures |
| **Monotonic Stack** | Histogram problems, tracking local extrema |
| **Next Greater Element** | Daily temperatures, Stock span problems |
| **Expression Evaluation** | Parsing Infix, Prefix, and Postfix expressions |
| **Min Stack** | Retrieving the minimum element in $O(1)$ |

---

## 6. Recommended Learning Order

Balanced Parentheses
↓
Monotonic Stack
↓
Next Greater Element
↓
Expression Evaluation
↓
Min Stack

---

## 7. How to Identify

If a problem requires:

* Processing information in reverse order.
* Remembering recently seen elements.
* Matching nested structures.
* Finding nearest greater/smaller elements.

then a **Stack** is often the intended data structure.

---

## 8. Pattern Usage Flowchart

1. **Input:** Array, String, or Expression?
2. **Requirement:** Need to match items or find "nearest" neighbors?
3. **Logic:** Does the state depend on the *most recent* information?
4. **Action:** Think **Stack**.

---

## 9. Complexity Analysis

| Operation | Time Complexity | Space Complexity |
| --- | --- | --- |
| **Stack Operations** | $O(n)$ | $O(n)$ |

*(Note: Each element is pushed and popped at most once, leading to linear time complexity.)*

---

## 10. Important Note

Stack problems are more about understanding push-pop behavior and recognizing patterns than memorizing formulas.

Drawing the stack contents step by step is highly recommended. Many stack problems can be solved using arrays or vectors, but thinking in terms of a Stack often leads to cleaner and more efficient solutions.

---

