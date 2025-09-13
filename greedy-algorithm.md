# Greedy Algorithms

## Introduction
Greedy algorithms make locally optimal choices at each step, hoping to find a global optimum. They are simple and often efficient but don't always yield the best solution. They work well when local optimal choices lead to a global optimal solution.

## How Greedy Algorithms Work
1. Make the locally optimal choice at each step
2. Never reconsider previous choices
3. Hope that these choices lead to a globally optimal solution
4. Continue until a complete solution is reached

## Key Characteristics
- **Greedy Choice Property**: Local optimal choice is safe
- **Optimal Substructure**: Problem can be solved optimally by building on optimal solutions to subproblems
- **Irreversibility**: Choices once made are never undone
- **Fast Execution**: Usually more efficient than other approaches

## Implementation Pattern

### C++ Generic Greedy Template
```cpp
Solution greedyAlgorithm(Problem P) {
    Solution result;
    while (!P.isEmpty()) {
        Choice choice = selectBestChoice(P);
        if (isValid(choice)) {
            result.add(choice);
            P.update(choice);
        }
    }
    return result;
}
```

### Example: Activity Selection Problem
```cpp
vector<pair<int, int>> activitySelection(vector<pair<int, int>>& activities) {
    // Sort by finish time
    sort(activities.begin(), activities.end(),
         [](auto& a, auto& b) { return a.second < b.second; });
    
    vector<pair<int, int>> result;
    result.push_back(activities[0]);
    
    int lastEnd = activities[0].second;
    for (int i = 1; i < activities.size(); i++) {
        if (activities[i].first >= lastEnd) {
            result.push_back(activities[i]);
            lastEnd = activities[i].second;
        }
    }
    return result;
}
```

## Common Applications
1. **Scheduling Problems**
   - Activity Selection
   - Job Sequencing
   - Task Assignment

2. **Optimization Problems**
   - Minimum Spanning Tree
   - Huffman Coding
   - Fractional Knapsack

3. **Network Flow**
   - Maximum Flow
   - Minimum Cut
   - Path Finding

4. **Resource Allocation**
   - Memory Management
   - Processor Scheduling
   - Load Balancing

## Time and Space Complexity
- **Time Complexity**: Usually O(n log n) or better
  - Often dominated by initial sorting
  - Linear time for actual greedy choices
- **Space Complexity**: Usually O(n) or O(1)
  - Minimal extra space needed
  - Often in-place operations

## Common Greedy Algorithms
1. **Kruskal's Algorithm (MST)**
```cpp
struct Edge {
    int src, dest, weight;
};

class UnionFind {
    vector<int> parent, rank;
public:
    UnionFind(int n) : parent(n), rank(n, 0) {
        for (int i = 0; i < n; i++)
            parent[i] = i;
    }
    
    int find(int x) {
        if (parent[x] != x)
            parent[x] = find(parent[x]);
        return parent[x];
    }
    
    void unite(int x, int y) {
        int px = find(x), py = find(y);
        if (px == py) return;
        if (rank[px] < rank[py])
            parent[px] = py;
        else if (rank[px] > rank[py])
            parent[py] = px;
        else {
            parent[py] = px;
            rank[px]++;
        }
    }
};

vector<Edge> kruskal(vector<Edge>& edges, int V) {
    sort(edges.begin(), edges.end(),
         [](Edge& a, Edge& b) { return a.weight < b.weight; });
    
    UnionFind uf(V);
    vector<Edge> mst;
    
    for (Edge& e : edges) {
        if (uf.find(e.src) != uf.find(e.dest)) {
            mst.push_back(e);
            uf.unite(e.src, e.dest);
        }
    }
    return mst;
}
```

2. **Dijkstra's Algorithm (Shortest Path)**
```cpp
vector<int> dijkstra(vector<vector<pair<int,int>>>& graph, int src) {
    int n = graph.size();
    vector<int> dist(n, INT_MAX);
    priority_queue<pair<int,int>, vector<pair<int,int>>, greater<>> pq;
    
    dist[src] = 0;
    pq.push({0, src});
    
    while (!pq.empty()) {
        auto [d, u] = pq.top();
        pq.pop();
        
        if (d > dist[u]) continue;
        
        for (auto [v, weight] : graph[u]) {
            if (dist[v] > dist[u] + weight) {
                dist[v] = dist[u] + weight;
                pq.push({dist[v], v});
            }
        }
    }
    return dist;
}
```

## Challenges

Here are 5 popular LeetCode problems that use greedy approach:

1. **Jump Game (LeetCode 55)**
   - **Problem**: Determine if you can reach the last index
   - **Difficulty**: Medium
   ```cpp
   class Solution {
   public:
       bool canJump(vector<int>& nums) {
           int maxReach = 0;
           for (int i = 0; i <= maxReach && i < nums.size(); i++) {
               maxReach = max(maxReach, i + nums[i]);
           }
           return maxReach >= nums.size() - 1;
       }
   };
   ```

2. **Container With Most Water (LeetCode 11)**
   - **Problem**: Find two lines that together with x-axis forms container with most water
   - **Difficulty**: Medium
   ```cpp
   class Solution {
   public:
       int maxArea(vector<int>& height) {
           int left = 0, right = height.size() - 1;
           int maxArea = 0;
           
           while (left < right) {
               int area = min(height[left], height[right]) * (right - left);
               maxArea = max(maxArea, area);
               
               if (height[left] < height[right])
                   left++;
               else
                   right--;
           }
           return maxArea;
       }
   };
   ```

3. **Gas Station (LeetCode 134)**
   - **Problem**: Find starting gas station to complete circular tour
   - **Difficulty**: Medium
   ```cpp
   class Solution {
   public:
       int canCompleteCircuit(vector<int>& gas, vector<int>& cost) {
           int start = 0, total = 0, tank = 0;
           
           for (int i = 0; i < gas.size(); i++) {
               tank += gas[i] - cost[i];
               if (tank < 0) {
                   start = i + 1;
                   total += tank;
                   tank = 0;
               }
           }
           
           return (total + tank < 0) ? -1 : start;
       }
   };
   ```

4. **Task Scheduler (LeetCode 621)**
   - **Problem**: Arrange tasks with cooldown period
   - **Difficulty**: Medium
   ```cpp
   class Solution {
   public:
       int leastInterval(vector<char>& tasks, int n) {
           vector<int> freq(26, 0);
           for (char task : tasks)
               freq[task - 'A']++;
           
           sort(freq.begin(), freq.end(), greater<int>());
           int maxFreq = freq[0];
           int idleTime = (maxFreq - 1) * n;
           
           for (int i = 1; i < 26 && idleTime > 0; i++) {
               idleTime -= min(maxFreq - 1, freq[i]);
           }
           
           return tasks.size() + max(0, idleTime);
       }
   };
   ```

5. **Non-overlapping Intervals (LeetCode 435)**
   - **Problem**: Find minimum number of intervals to remove to make rest non-overlapping
   - **Difficulty**: Medium
   ```cpp
   class Solution {
   public:
       int eraseOverlapIntervals(vector<vector<int>>& intervals) {
           if (intervals.empty()) return 0;
           
           sort(intervals.begin(), intervals.end(),
                [](auto& a, auto& b) { return a[1] < b[1]; });
           
           int count = 1;
           int end = intervals[0][1];
           
           for (int i = 1; i < intervals.size(); i++) {
               if (intervals[i][0] >= end) {
                   count++;
                   end = intervals[i][1];
               }
           }
           
           return intervals.size() - count;
       }
   };
   ```

## Best Practices
1. **Problem Analysis**
   - Verify greedy approach is valid
   - Identify optimal substructure

2. **Implementation**
   - Sort input if needed
   - Use efficient data structures
   - Handle edge cases

3. **Optimization**
   - Minimize unnecessary operations
   - Consider space-time tradeoffs

4. **Testing**
   - Test with various input sizes
   - Verify correctness with examples

## Common Pitfalls
1. Using greedy when it's not optimal
2. Missing edge cases
3. Incorrect sorting criteria
4. Inefficient data structures
5. Not considering all constraints

## When to Use Greedy
1. **Optimal Substructure Exists**
   - Problem can be solved using optimal solutions to subproblems

2. **Greedy Choice Property**
   - Local optimal choices lead to global optimal solution

3. **Simple Implementation**
   - When other approaches are too complex or slow

4. **Time Critical Applications**
   - When quick decisions are needed

## Conclusion
Greedy algorithms are powerful tools for optimization problems when local optimal choices lead to global optimal solutions. While they don't always yield the best results, their simplicity and efficiency make them valuable in many practical applications. Understanding when to apply greedy approaches and their limitations is crucial for effective problem-solving.