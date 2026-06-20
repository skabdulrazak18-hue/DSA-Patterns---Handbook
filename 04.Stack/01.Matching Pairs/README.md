

# Pattern Name: Matching Pairs

### Goal

Determine whether symbols, brackets, or nested structures are properly matched and closed in a sequence using a stack to track their hierarchical order.

---

## 1. Core Idea

Whenever an opening symbol is encountered, it must be stored. When its corresponding closing symbol appears, the most recently opened symbol must be closed first. This "reverse-order" requirement naturally follows the **LIFO (Last-In, First-Out)** property of a stack.

### Theory

By pushing opening markers onto a stack, we maintain a history of pending closures. If a closing symbol arrives, it must match the symbol at the top of the stack. If it doesn't—or if the stack is empty—the sequence is invalid. This ensures that nested structures are closed in the exact inverse order they were opened.

---

## 2. Why This Pattern Matters

1. **Hierarchy:** Solves problems involving nested structures (math expressions, code blocks, string decoding).
2. **Efficiency:** Processes structures in a single pass ($O(n)$ time).
3. **Logic Standard:** Replaces recursive overhead with a clean, iterative stack approach.

---

## 3. When to Use

### Checklist

* You encounter nested symbols, brackets, or hierarchical tags.
* You need to verify if an expression or structure is "well-formed."
* You need to track the scope or depth of nested data.

### Key Phrases to Watch For

* "Valid parentheses..."
* "Check if brackets are balanced..."
* "Decode string with nested brackets..."
* "Remove invalid pairs..."

---

## 4. Typical Elements of the Pattern

### A. The Stack

Stores pending opening symbols (or their indices/metadata) to be matched later.

### B. Matching Logic

A Hash Map, conditional statements, or counters used to determine whether a closing symbol matches its corresponding opener.

### C. The Final Validation

After the loop, the stack must be **empty**; any remaining elements indicate unclosed symbols.

---

## 5. Common Applications

| Variation | Strategy |
| --- | --- |
| **Stack of Characters** | Store symbols themselves to verify order. |
| **Stack of Indices** | Store positions to identify/remove invalid substrings. |
| **Counter + Stack** | Combine counters (for simple brackets) and stacks (for mixed types). |
| **Nested Structures** | Match symbols inside deeper levels of hierarchy. |
| **Metadata Stack** | Store extra information like frequency, strings, or numbers. |

---

## 6. Time and Space Complexity

| Operation | Time | Space |
| --- | --- | --- |
| Validation | $O(n)$ | $O(n)$ |

---

## 7. Common Pitfalls to Avoid

1. **Empty Stack:** Popping from an empty stack (indicates a closer without an opener).
2. **Leftover Elements:** Forgetting to check if the stack is empty at the end.
3. **Mismatched Types:** Ensuring the closing symbol matches the *exact* type of the most recent opener.

---

## 8. Related Patterns

Stack
↓
Matching Pairs
↓
Expression Parsing
↓
Expression Evaluation
↓
String Decoding

---

## 9. Template

```cpp
bool isMatched(string s) {
    stack<char> st;
    unordered_map<char, char> pairs = {{')', '('}, {']', '['}, {'}', '{'}};
    
    for (char c : s) {
        if (pairs.count(c)) { // Closing symbol?
            if (!st.empty() && st.top() == pairs[c]) st.pop();
            else return false;
        } else {
            st.push(c); // Opening symbol
        }
    }
    return st.empty();
}

```

---

## 10. How to Identify

* Are you dealing with nested or hierarchical pairs?
* Is the order of closure strictly defined by the "most recent" opener?
* Do you need to validate structural integrity or isolate valid segments?

---

## 11. Brute Force vs. Optimized

| Approach | Logic | Time | Space |
| --- | --- | --- | --- |
| **Brute Force** | Recursively remove valid pairs until none remain. | $O(n^2)$ | $O(n)$ |
| **Optimized** | Single pass with a Stack to match openers with closers. | $O(n)$ | $O(n)$ |

---

## 12. Example Problem: Valid Parentheses

Determine if the input string `s` is valid.

### Code

```cpp
bool isValid(string s) {
    stack<char> st;
    for (char c : s) {
        if (c == '(' || c == '{' || c == '[') st.push(c);
        else {
            if (st.empty()) return false;
            char top = st.top();
            if ((c == ')' && top != '(') || (c == '}' && top != '{') || (c == ']' && top != '[')) return false;
            st.pop();
        }
    }
    return st.empty();
}

```

### Dry Run: `s = "{[()]}"`

| Step | Character | Stack | Action |
| --- | --- | --- | --- |
| **1** | `{` | [`{`] | Push |
| **2** | `[` | [`{`, `[`] | Push |
| **3** | `(` | [`{`, `[`, `(`] | Push |
| **4** | `)` | [`{`, `[`] | Pop |
| **5** | `]` | [`{`] | Pop |
| **6** | `}` | `[]` | Pop |

### Time and Space Complexity

* **Time Complexity:** $O(n)$ — Each symbol is processed once.
* **Space Complexity:** $O(n)$ — In the worst case, all symbols are pushed onto the stack.

---

##  Practice Problems

### Easy

| Problem | Link |
| --- | --- |
| Valid Parentheses | [https://leetcode.com/problems/valid-parentheses/](https://leetcode.com/problems/valid-parentheses/) |

### Medium

| Problem | Link |
| --- | --- |
| Minimum Add to Make Parentheses Valid | [https://leetcode.com/problems/minimum-add-to-make-parentheses-valid/](https://leetcode.com/problems/minimum-add-to-make-parentheses-valid/) |
| Minimum Remove to Make Valid Parentheses | [https://leetcode.com/problems/minimum-remove-to-make-valid-parentheses/](https://leetcode.com/problems/minimum-remove-to-make-valid-parentheses/) |
| Decode String | [https://leetcode.com/problems/decode-string/](https://leetcode.com/problems/decode-string/) |

### Hard

| Problem | Link |
| --- | --- |
| Remove Invalid Parentheses | [https://leetcode.com/problems/remove-invalid-parentheses/](https://leetcode.com/problems/remove-invalid-parentheses/) |

---

