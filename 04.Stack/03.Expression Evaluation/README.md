

# Pattern Name: Expression Evaluation

### Goal

Parse and calculate the result of mathematical expressions (Infix, Prefix, or Postfix) that contain nested operations, operator precedence, and parentheses.

---

## 1. Core Idea

Expression evaluation requires managing operator precedence (e.g., `*` before `+`) and nested scope (parentheses). By using one or two stacks, we can transform human-readable expressions into a format the computer can process sequentially without recursive overhead.

### Theory

Computers process expressions by leveraging the **LIFO** property. For Infix expressions, we use a stack to defer operators until their precedence is satisfied. For Postfix/Prefix, the stack stores operands, applying operators as soon as they are encountered.

---

## 2. Why Expression Evaluation Matters

1. **Hierarchy:** Solves problems involving deeply nested mathematical logic.
2. **Precedence Management:** Efficiently handles the rules of math (PEMDAS/BODMAS) using stack-based ordering.
3. **Parsing Power:** Essential for building compilers, calculators, and query language engines.

---

## 3. When to Use

### Checklist

* You need to evaluate mathematical expressions with different operator priorities.
* You see expressions with nested parentheses (e.g., `(2 + (3 * 4))`).
* You need to convert between Infix, Prefix, or Postfix notations.

### Key Phrases to Watch For

* "Evaluate expression..."
* "Calculate the result of a string..."
* "Basic calculator..."
* "Convert infix to postfix..."

---

## 4. Typical Elements of the Pattern

### A. The Operand Stack

Stores numerical values as they are parsed or resolved.

### B. The Operator Stack

Stores operators (`+`, `-`, `*`, `/`) and parentheses `(` to maintain precedence.

### C. Matching Logic

Conditional logic (or a Hash Map) to determine whether an operator should be evaluated immediately or deferred.

---

## 5. Common Variations

| Variation | Strategy |
| --- | --- |
| **Infix Evaluation** | Two-stack approach (operands + operators). |
| **Postfix Evaluation** | Single-stack approach (process numbers, apply operator). |
| **Prefix Evaluation** | Traverse from right to left; use single-stack approach. |
| **Infix → Postfix** | Operator stack to reorder based on precedence. |
| **Infix → Prefix** | Reverse string, then use Infix → Postfix logic. |

---

## 6. Applications

* Calculators
* Compilers
* Expression Trees
* Query Engines
* Spreadsheet Formula Processing

---

## 7. Time and Space Complexity

| Operation | Time | Space |
| --- | --- | --- |
| Evaluation | $O(n)$ | $O(n)$ |

### Important Observations

* **Efficiency:** Each element is processed once.
* **Philosophy:** Elements that can no longer contribute to future answers are removed permanently.

---

## 8. Common Pitfalls to Avoid

1. **Operator Precedence:** Forgetting to process high-precedence operators before lower ones.
2. **Parentheses Logic:** Failing to treat `(` as a barrier that resets precedence.
3. **Multi-digit Numbers:** Assuming every number is a single digit (always handle full string-to-integer conversion).
4. **Operand Order:** In non-commutative operations (subtraction/division), remember: `right = st.pop()`, `left = st.pop()`, `result = left op right`.

---

## 9. Related Patterns

Stack
↓
Balanced Symbols
↓
Expression Parsing
↓
Expression Evaluation

---

## 10. Template (Infix Evaluation)

```cpp
int evaluate(string expression) {
    stack<int> values;
    stack<char> ops;
    
    for (int i = 0; i < expression.length(); i++) {
        if (isdigit(expression[i])) {
            // Parse full number and push to values
        } else if (expression[i] == '(') {
            ops.push('(');
        } else if (expression[i] == ')') {
            // Resolve until '('
        } else {
            // Resolve higher/equal precedence, then push new op
            ops.push(expression[i]);
        }
    }
    // Final evaluation...
}

```

---

## 11. How to Identify

* Does the problem involve math operations with varying priorities?
* Are there nested parentheses dictating the order of operations?
* Is the expression provided as a single string?

---

## 12. Brute Force vs. Optimized

| Approach | Logic | Time | Space |
| --- | --- | --- | --- |
| **Brute Force** | Repeatedly search for sub-expressions and solve them. | $O(n^2)$ | $O(n)$ |
| **Optimized** | Single linear scan using stacks for order/precedence. | $O(n)$ | $O(n)$ |

---

## 13. Example Problem: Basic Calculator

Evaluate the result of a string expression.

### Code

```cpp
int calculate(string s) {
    stack<int> nums;
    int cur = 0, res = 0, sign = 1;
    for (char c : s) {
        if (isdigit(c)) cur = cur * 10 + (c - '0');
        else if (c == '+') { res += sign * cur; sign = 1; cur = 0; }
        else if (c == '-') { res += sign * cur; sign = -1; cur = 0; }
        else if (c == '(') { nums.push(res); nums.push(sign); res = 0; sign = 1; }
        else if (c == ')') { 
            res += sign * cur; 
            res *= nums.top(); nums.pop(); 
            res += nums.top(); nums.pop(); 
            cur = 0; 
        }
    }
    return res + sign * cur;
}

```

### Dry Run: `s = "1+(2-3)"`

| Step | Character | Result/Stack | Action |
| --- | --- | --- | --- |
| **1** | `1` | cur=1 | Parse number |
| **2** | `+` | res=1, cur=0 | Add to result |
| **3** | `(` | nums=[1, 1] | Push context |
| **4** | `2` | cur=2 | Parse number |
| **5** | `-` | res=2, cur=0 | Update res |
| **6** | `3` | cur=3 | Parse number |
| **7** | `)` | res=0 | Evaluate parens |

### Time and Space Complexity

* **Time Complexity:** $O(n)$ — The entire expression string is scanned exactly once.
* **Space Complexity:** $O(n)$ — Stacks hold $O(n)$ elements in the worst case (nested parentheses).

---

## 14. Practice Problems

### Medium

| Problem | Link |
| --- | --- |
| Basic Calculator II | [https://leetcode.com/problems/basic-calculator-ii/](https://leetcode.com/problems/basic-calculator-ii/) |
| Evaluate Reverse Polish Notation | [https://leetcode.com/problems/evaluate-reverse-polish-notation/](https://leetcode.com/problems/evaluate-reverse-polish-notation/) |

### Hard

| Problem | Link |
| --- | --- |
| Basic Calculator | [https://leetcode.com/problems/basic-calculator/](https://leetcode.com/problems/basic-calculator/) |
| Basic Calculator III | [https://leetcode.com/problems/basic-calculator-iii/](https://leetcode.com/problems/basic-calculator-iii/) |

---

