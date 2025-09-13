# Depth-First Search (DFS) Algorithm

## Introduction
Depth-First Search (DFS) is a fundamental graph traversal algorithm that explores a graph or tree data structure by going as deep as possible along each branch before backtracking. Unlike BFS, which explores level by level, DFS explores a path to its end before trying other paths.

## How DFS Works
1. Start from a chosen source node
2. Explore as far as possible along each branch
3. Backtrack when you reach a dead end
4. Continue exploring unexplored paths
5. Repeat until all nodes are visited

## Key Components
- **Stack**: Used to keep track of nodes (or recursion stack)
- **Visited Set**: Keeps track of nodes that have been explored
- **Adjacency List/Matrix**: Represents the graph structure

## Implementation

### C++ Implementation (Recursive)
```cpp
#include <iostream>
#include <vector>
#include <unordered_map>
#include <unordered_set>
using namespace std;

// DFS implementation using adjacency list (recursive)
void dfs(unordered_map<char, vector<char>>& graph, char node, 
         unordered_set<char>& visited) {
    // Mark current node as visited and print it
    visited.insert(node);
    cout << node << " ";
    
    // Recur for all adjacent vertices
    for (char neighbor : graph[node]) {
        if (visited.find(neighbor) == visited.end()) {
            dfs(graph, neighbor, visited);
        }
    }
}

// Wrapper function to handle disconnected components
void dfsTraversal(unordered_map<char, vector<char>>& graph, char start) {
    unordered_set<char> visited;
    dfs(graph, start, visited);
}

// Example usage
int main() {
    unordered_map<char, vector<char>> graph = {
        {'A', {'B', 'C'}},
        {'B', {'A', 'D'}},
        {'C', {'A', 'D'}},
        {'D', {'B', 'C'}}
    };
    
    cout << "DFS starting from vertex A: ";
    dfsTraversal(graph, 'A');
    // Possible Output: A B D C
    
    return 0;
}
```

### C++ Implementation (Iterative)
```cpp
#include <iostream>
#include <stack>
#include <vector>
#include <unordered_map>
#include <unordered_set>
using namespace std;

void dfsIterative(unordered_map<char, vector<char>>& graph, char start) {
    unordered_set<char> visited;
    stack<char> st;
    
    st.push(start);
    
    while (!st.empty()) {
        char node = st.top();
        st.pop();
        
        if (visited.find(node) == visited.end()) {
            cout << node << " ";
            visited.insert(node);
            
            // Push all unvisited neighbors
            for (auto it = graph[node].rbegin(); it != graph[node].rend(); ++it) {
                if (visited.find(*it) == visited.end()) {
                    st.push(*it);
                }
            }
        }
    }
}
```

## Example
Consider this graph:
```
    A --- B
    |     |
    C --- D
```

Represented as an adjacency list:
```cpp
graph = {
    'A': ['B', 'C'],
    'B': ['A', 'D'],
    'C': ['A', 'D'],
    'D': ['B', 'C']
}
```

DFS traversal starting from 'A' could be: A → B → D → C

## Time and Space Complexity
- **Time Complexity**: O(V + E)
  - V: number of vertices
  - E: number of edges
- **Space Complexity**: O(V)
  - For the recursion stack or explicit stack
  - For the visited set

## Applications
1. **Cycle Detection**
   - Finding cycles in graphs
   - Detecting deadlocks in systems

2. **Topological Sorting**
   - Dependency resolution
   - Task scheduling

3. **Pathfinding**
   - Maze solving
   - Game AI pathfinding

4. **Connected Components**
   - Finding strongly connected components
   - Network analysis

## Advantages
- Uses less memory than BFS for deep graphs
- Natural for exploring paths to their ends
- Ideal for tree traversal
- Better for decision trees and game trees

## Disadvantages
- May get stuck in deep branches
- Does not guarantee shortest path
- Can be slower for shallow, wide graphs
- Risk of stack overflow in recursive implementation

## Common Variations
1. **Iterative Deepening DFS**
   - Combines benefits of BFS and DFS
   - Memory-efficient with complete traversal

2. **Bidirectional DFS**
   - Searches from both ends
   - Useful for path finding

## Best Practices
1. Choose between recursive and iterative implementation based on needs
2. Always maintain visited set to avoid cycles
3. Consider stack depth for recursive implementation
4. Handle disconnected components
5. Implement proper error handling

## Real-world Use Cases
1. **File System Traversal**
2. **Web Crawling**
3. **Solving Puzzles**
4. **Garbage Collection**
5. **Circuit Design Verification**

## Comparison with BFS
| Aspect | DFS | BFS |
|--------|-----|-----|
| Space | O(H) - height | O(W) - width |
| Implementation | Simpler | More complex |
| Path Finding | Not optimal | Optimal for unweighted |
| Memory Usage | Less | More |
| Traversal | Branch by branch | Level by level |

## Challenges

Here are 5 popular LeetCode problems that can be solved efficiently using DFS:

1. **Number of Islands (LeetCode 200)**
   - **Problem**: Count the number of islands (connected 1's) in a 2D grid
   - **Difficulty**: Medium
   - **Key Concepts**: Grid traversal, Connected components
   ```cpp
   void dfs(vector<vector<char>>& grid, int i, int j) {
       if (i < 0 || i >= grid.size() || j < 0 || j >= grid[0].size() || grid[i][j] == '0')
           return;
           
       grid[i][j] = '0';  // Mark as visited
       dfs(grid, i+1, j);
       dfs(grid, i-1, j);
       dfs(grid, i, j+1);
       dfs(grid, i, j-1);
   }
   
   int numIslands(vector<vector<char>>& grid) {
       int islands = 0;
       for (int i = 0; i < grid.size(); i++) {
           for (int j = 0; j < grid[0].size(); j++) {
               if (grid[i][j] == '1') {
                   islands++;
                   dfs(grid, i, j);
               }
           }
       }
       return islands;
   }
   ```

2. **Course Schedule (LeetCode 207)**
   - **Problem**: Determine if it's possible to finish all courses given prerequisites
   - **Difficulty**: Medium
   - **Key Concepts**: Cycle detection, Topological sort
   ```cpp
   bool canFinish(int numCourses, vector<vector<int>>& prerequisites) {
       vector<vector<int>> adj(numCourses);
       vector<int> visited(numCourses, 0);
       
       for (auto& pre : prerequisites) {
           adj[pre[1]].push_back(pre[0]);
       }
       
       for (int i = 0; i < numCourses; i++) {
           if (!dfs(adj, visited, i)) return false;
       }
       return true;
   }
   
   bool dfs(vector<vector<int>>& adj, vector<int>& visited, int node) {
       if (visited[node] == 1) return false;  // Found cycle
       if (visited[node] == 2) return true;   // Already processed
       
       visited[node] = 1;  // Mark as being processed
       for (int next : adj[node]) {
           if (!dfs(adj, visited, next)) return false;
       }
       visited[node] = 2;  // Mark as processed
       return true;
   }
   ```

3. **Clone Graph (LeetCode 133)**
   - **Problem**: Deep clone a connected undirected graph
   - **Difficulty**: Medium
   - **Key Concepts**: Graph traversal, Hash map
   ```cpp
   Node* cloneGraph(Node* node) {
       if (!node) return nullptr;
       unordered_map<Node*, Node*> visited;
       return dfs(node, visited);
   }
   
   Node* dfs(Node* node, unordered_map<Node*, Node*>& visited) {
       if (visited.find(node) != visited.end())
           return visited[node];
           
       Node* clone = new Node(node->val);
       visited[node] = clone;
       
       for (Node* neighbor : node->neighbors) {
           clone->neighbors.push_back(dfs(neighbor, visited));
       }
       return clone;
   }
   ```

4. **Maximum Depth of Binary Tree (LeetCode 104)**
   - **Problem**: Find the maximum depth of a binary tree
   - **Difficulty**: Easy
   - **Key Concepts**: Tree traversal, Recursion
   ```cpp
   int maxDepth(TreeNode* root) {
       if (!root) return 0;
       return 1 + max(maxDepth(root->left), maxDepth(root->right));
   }
   ```

5. **Path Sum (LeetCode 112)**
   - **Problem**: Check if root-to-leaf path with given sum exists
   - **Difficulty**: Easy
   - **Key Concepts**: Tree traversal, Path tracking
   ```cpp
   bool hasPathSum(TreeNode* root, int targetSum) {
       if (!root) return false;
       if (!root->left && !root->right)
           return targetSum == root->val;
           
       return hasPathSum(root->left, targetSum - root->val) ||
              hasPathSum(root->right, targetSum - root->val);
   }
   ```

These problems demonstrate various applications of DFS in solving algorithmic challenges, from graph problems to tree traversal.

## Conclusion
DFS is a powerful algorithm with numerous applications in computer science. Its depth-first nature makes it particularly useful for problems involving path exploration, cycle detection, and tree traversal. Understanding both recursive and iterative implementations, along with their trade-offs, is crucial for effective problem-solving.