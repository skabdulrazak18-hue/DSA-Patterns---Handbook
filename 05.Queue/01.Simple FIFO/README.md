
# Pattern Name: Breadth-First Search (BFS)

### Goal

Systematically explore a graph or tree layer-by-layer, moving outward from a starting node to find the shortest path or traverse all reachable nodes.

---

## 1. Core Idea

Breadth-First Search explores the search space in "waves." It visits all immediate neighbors of a starting node before moving to the neighbors of those neighbors. This layer-by-layer traversal is inherently suited for finding the shortest path in unweighted structures.

### Theory

BFS utilizes a **Queue** to maintain the order of discovery. By always dequeuing the oldest discovered node, we ensure that nodes at depth $d$ are fully processed before any nodes at depth $d+1$.

---

## 2. Why This Pattern Matters

1. **Shortest Path:** Guarantees the shortest path in unweighted graphs.
2. **Layered Discovery:** Ideal for problems requiring level-by-level processing.
3. **Completeness:** Ensures every reachable node in a graph is visited.

---

## 3. When to Use

### Checklist

* You need to find the shortest path in an unweighted graph/grid.
* You need to visit nodes layer-by-layer.
* You need to explore all reachable nodes from a source.

### Key Phrases to Watch For

* "Shortest path..."
* "Level-order traversal..."
* "Find the distance from start to end..."
* "Minimum number of steps..."

---

## 4. Typical Elements of the Pattern

### A. The Queue

Stores the frontier of nodes to be visited (FIFO).

### B. The Visited Set

A hash set or array used to keep track of already visited nodes to prevent infinite cycles.

### C. The Level Tracker

Optional logic (like nested `for` loops) to process nodes level-by-level.

---

## 5. Common Applications

| Variation | Strategy |
| --- | --- |
| **Simple BFS** | Standard layer-by-layer traversal. |
| **Shortest Path** | Track distances as you move to new levels. |
| **Grid/Matrix BFS** | Explore neighbors (up, down, left, right) within bounds. |
| **Level Order** | Use a queue size to distinguish between levels. |
| **Multi-Source BFS** | Initialize the queue with multiple starting nodes. |

---

## 6. Time and Space Complexity

| Operation | Time | Space |
| --- | --- | --- |
| Traversal | $O(V + E)$ | $O(V)$ |

---

## 7. Common Pitfalls to Avoid

1. **Cycles:** Forgetting to mark nodes as `visited`, leading to infinite loops in graphs.
2. **Bounds Checking:** Failing to check grid/matrix boundaries before adding neighbors.
3. **Queue Bloat:** Adding nodes to the queue multiple times instead of checking `visited` before insertion.

---

## 8. Related Patterns

Queue
↓
Breadth-First Search (BFS)
↓
Multi-Source BFS
↓
Level Order Traversal

---

## 9. Template

```cpp
void bfs(Node* start) {
    queue<Node*> q;
    unordered_set<Node*> visited;
    
    q.push(start);
    visited.insert(start);
    
    while (!q.empty()) {
        Node* curr = q.front();
        q.pop();
        
        for (Node* neighbor : curr->neighbors) {
            if (visited.find(neighbor) == visited.end()) {
                visited.insert(neighbor);
                q.push(neighbor);
            }
        }
    }
}

```

---

## 10. How to Identify

* Do you need to visit nodes layer-by-layer?
* Is this an unweighted shortest path problem?
* Can you model the problem as exploring neighbors in a graph or grid?

---

## 11. Brute Force vs. Optimized

| Approach | Logic | Time | Space |
| --- | --- | --- | --- |
| **DFS** | Explore deep paths; not ideal for shortest path. | $O(V+E)$ | $O(V)$ |
| **BFS** | Explore all neighbors; finds shortest path in unweighted graphs. | $O(V+E)$ | $O(V)$ |

---

## 12. Example Problem: Binary Tree Level Order Traversal

Return the level order traversal of a binary tree's nodes' values.

### Code

```cpp
vector<vector<int>> levelOrder(TreeNode* root) {
    if (!root) return {};
    vector<vector<int>> res;
    queue<TreeNode*> q;
    q.push(root);
    
    while (!q.empty()) {
        int size = q.size();
        vector<int> level;
        for (int i = 0; i < size; i++) {
            TreeNode* curr = q.front(); q.pop();
            level.push_back(curr->val);
            if (curr->left) q.push(curr->left);
            if (curr->right) q.push(curr->right);
        }
        res.push_back(level);
    }
    return res;
}

```

### Dry Run: Tree `[1, 2, 3]`

| Step | Queue | Level Processed | Action |
| --- | --- | --- | --- |
| **1** | [1] | - | Initialize |
| **2** | [2, 3] | [1] | Pop 1, push children |
| **3** | [3] | [2] | Pop 2 |
| **4** | [] | [3] | Pop 3 |

### Time and Space Complexity

* **Time Complexity:** $O(n)$ — Every node is visited once.
* **Space Complexity:** $O(n)$ — Queue stores nodes at the widest level.

---

## 13. Practice Problems

### Easy

| Problem | Link |
| --- | --- |
| Binary Tree Level Order Traversal | [https://leetcode.com/problems/binary-tree-level-order-traversal/](https://leetcode.com/problems/binary-tree-level-order-traversal/) |

### Medium

| Problem | Link |
| --- | --- |
| Shortest Path in Binary Matrix | [https://leetcode.com/problems/shortest-path-in-binary-matrix/](https://leetcode.com/problems/shortest-path-in-binary-matrix/) |
| Number of Islands | [https://leetcode.com/problems/number-of-islands/](https://leetcode.com/problems/number-of-islands/) |

### Hard

| Problem | Link |
| --- | --- |
| Word Ladder | [https://leetcode.com/problems/word-ladder/](https://leetcode.com/problems/word-ladder/) |

---

