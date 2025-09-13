# Dijkstra's Algorithm

## Introduction
Dijkstra's algorithm is a fundamental graph algorithm used to find the shortest path between nodes in a graph with non-negative edge weights. It was conceived by computer scientist Edsger W. Dijkstra in 1956 and is widely used in routing and networking applications.

## How Dijkstra's Algorithm Works
1. Initialize distances to all vertices as infinite except the source vertex (distance = 0)
2. Keep track of vertices in two sets: visited and unvisited
3. For the current vertex, consider all its unvisited neighbors
4. Calculate their tentative distances through the current vertex
5. If the calculated distance is less than the previously recorded distance, update it
6. Mark the current vertex as visited
7. Select the unvisited vertex with the smallest tentative distance as the new current vertex
8. Repeat steps 3-7 until all vertices are visited

## Key Components
- **Priority Queue**: Efficiently selects the vertex with minimum distance
- **Distance Array**: Keeps track of shortest distances from source
- **Previous Array**: Stores the shortest path structure
- **Visited Set**: Tracks processed vertices

## Implementation

### C++ Implementation using Priority Queue
```cpp
#include <iostream>
#include <vector>
#include <queue>
#include <limits>
using namespace std;

typedef pair<int, int> pii; // {distance, vertex}

vector<int> dijkstra(vector<vector<pii>>& graph, int V, int source) {
    // Create min heap priority queue
    priority_queue<pii, vector<pii>, greater<pii>> pq;
    
    // Distance vector
    vector<int> dist(V, numeric_limits<int>::max());
    
    // Initialize source distance as 0
    dist[source] = 0;
    pq.push({0, source});
    
    while (!pq.empty()) {
        int u = pq.top().second;
        pq.pop();
        
        // For all adjacent vertices of u
        for (auto& neighbor : graph[u]) {
            int v = neighbor.first;
            int weight = neighbor.second;
            
            // If there is a shorter path to v through u
            if (dist[v] > dist[u] + weight) {
                dist[v] = dist[u] + weight;
                pq.push({dist[v], v});
            }
        }
    }
    return dist;
}

// Example usage
int main() {
    int V = 5; // Number of vertices
    vector<vector<pii>> graph(V);
    
    // Add edges: {vertex, weight}
    graph[0].push_back({1, 4});
    graph[0].push_back({2, 1});
    graph[1].push_back({3, 1});
    graph[2].push_back({1, 2});
    graph[2].push_back({3, 5});
    graph[3].push_back({4, 3});
    
    int source = 0;
    vector<int> shortest_distances = dijkstra(graph, V, source);
    
    cout << "Shortest distances from source " << source << ":\n";
    for (int i = 0; i < V; i++) {
        cout << "To " << i << ": " << shortest_distances[i] << endl;
    }
    
    return 0;
}
```

### C++ Implementation with Path Reconstruction
```cpp
struct DijkstraResult {
    vector<int> distances;
    vector<int> previous;
};

DijkstraResult dijkstraWithPath(vector<vector<pii>>& graph, int V, int source) {
    priority_queue<pii, vector<pii>, greater<pii>> pq;
    vector<int> dist(V, numeric_limits<int>::max());
    vector<int> prev(V, -1);
    
    dist[source] = 0;
    pq.push({0, source});
    
    while (!pq.empty()) {
        int u = pq.top().second;
        pq.pop();
        
        for (auto& neighbor : graph[u]) {
            int v = neighbor.first;
            int weight = neighbor.second;
            
            if (dist[v] > dist[u] + weight) {
                dist[v] = dist[u] + weight;
                prev[v] = u;
                pq.push({dist[v], v});
            }
        }
    }
    
    return {dist, prev};
}

// Function to reconstruct path
vector<int> getPath(const vector<int>& prev, int target) {
    vector<int> path;
    for (int at = target; at != -1; at = prev[at]) {
        path.push_back(at);
    }
    reverse(path.begin(), path.end());
    return path;
}
```

## Example
Consider this weighted graph:
```
    [0]--4--[1]
     | \     |
     1   2   1
     |     \ |
    [2]--5--[3]--3--[4]
```

Running Dijkstra's algorithm from source vertex 0 gives:
- Shortest distance to vertex 1: 3 (path: 0→2→1)
- Shortest distance to vertex 2: 1 (path: 0→2)
- Shortest distance to vertex 3: 4 (path: 0→2→1→3)
- Shortest distance to vertex 4: 7 (path: 0→2→1→3→4)

## Time and Space Complexity
- **Time Complexity**: O((V + E) log V)
  - V: number of vertices
  - E: number of edges
  - The log V factor comes from priority queue operations
- **Space Complexity**: O(V)
  - For storing distances, previous vertices, and priority queue

## Applications
1. **Network Routing**
   - Finding fastest network paths
   - Internet Protocol (IP) routing
   - Software Defined Networks (SDN)

2. **GPS Navigation**
   - Finding shortest/fastest routes
   - Real-time navigation systems
   - Traffic-aware routing

3. **Social Networks**
   - Finding shortest connection paths
   - Network analysis
   - Influence mapping

4. **Games**
   - Pathfinding in video games
   - AI movement planning
   - Strategy game calculations

## Advantages
- Guarantees shortest path for non-negative weights
- Efficient for sparse graphs
- Can be modified for various path-finding problems
- Works with both directed and undirected graphs

## Disadvantages
- Doesn't work with negative weights
- Can be slow for dense graphs
- Requires all edge weights to be known
- Memory intensive for very large graphs

## Variations
1. **Bidirectional Dijkstra**
   - Searches from both source and destination
   - Generally faster than standard version

2. **A* Algorithm**
   - Adds heuristic function
   - More efficient for specific targets

3. **Multi-source Dijkstra**
   - Finds shortest paths from multiple sources
   - Useful for facility location problems

## Best Practices
1. Use appropriate data structures (priority queue)
2. Handle edge cases (disconnected graphs)
3. Consider memory constraints for large graphs
4. Implement proper error handling
5. Optimize for specific use cases

## Challenges

Here are 5 popular LeetCode problems related to Dijkstra's algorithm:

1. **Network Delay Time (LeetCode 743)**
   - **Problem**: Find the time needed for all nodes to receive a signal
   - **Difficulty**: Medium
   - **Key Concepts**: Single-source shortest path
   ```cpp
   int networkDelayTime(vector<vector<int>>& times, int n, int k) {
       vector<vector<pair<int, int>>> graph(n + 1);
       for (const auto& time : times) {
           graph[time[0]].push_back({time[1], time[2]});
       }
       
       vector<int> dist(n + 1, INT_MAX);
       priority_queue<pair<int, int>, vector<pair<int, int>>, greater<>> pq;
       
       dist[k] = 0;
       pq.push({0, k});
       
       while (!pq.empty()) {
           auto [d, u] = pq.top();
           pq.pop();
           
           if (d > dist[u]) continue;
           
           for (auto [v, time] : graph[u]) {
               if (dist[v] > dist[u] + time) {
                   dist[v] = dist[u] + time;
                   pq.push({dist[v], v});
               }
           }
       }
       
       int maxTime = 0;
       for (int i = 1; i <= n; i++) {
           if (dist[i] == INT_MAX) return -1;
           maxTime = max(maxTime, dist[i]);
       }
       return maxTime;
   }
   ```

2. **Path with Maximum Probability (LeetCode 1514)**
   - **Problem**: Find path with highest probability
   - **Difficulty**: Medium
   - **Key Concepts**: Modified Dijkstra for probabilities
   ```cpp
   double maxProbability(int n, vector<vector<int>>& edges, vector<double>& succProb, int start, int end) {
       vector<vector<pair<int, double>>> graph(n);
       for (int i = 0; i < edges.size(); i++) {
           graph[edges[i][0]].push_back({edges[i][1], succProb[i]});
           graph[edges[i][1]].push_back({edges[i][0], succProb[i]});
       }
       
       vector<double> prob(n, 0);
       priority_queue<pair<double, int>> pq;
       
       prob[start] = 1.0;
       pq.push({1.0, start});
       
       while (!pq.empty()) {
           auto [p, u] = pq.top();
           pq.pop();
           
           if (u == end) return p;
           if (p < prob[u]) continue;
           
           for (auto [v, edgeProb] : graph[u]) {
               double newProb = prob[u] * edgeProb;
               if (newProb > prob[v]) {
                   prob[v] = newProb;
                   pq.push({newProb, v});
               }
           }
       }
       return 0.0;
   }
   ```

3. **Cheapest Flights Within K Stops (LeetCode 787)**
   - **Problem**: Find cheapest price with limited stops
   - **Difficulty**: Medium
   - **Key Concepts**: Modified Dijkstra with stops constraint
   ```cpp
   int findCheapestPrice(int n, vector<vector<int>>& flights, int src, int dst, int k) {
       vector<vector<pair<int, int>>> graph(n);
       for (const auto& f : flights) {
           graph[f[0]].push_back({f[1], f[2]});
       }
       
       vector<int> prices(n, INT_MAX);
       queue<pair<int, int>> q;
       
       prices[src] = 0;
       q.push({src, 0});
       int stops = 0;
       
       while (!q.empty() && stops <= k) {
           int size = q.size();
           
           while (size--) {
               auto [curr, price] = q.front();
               q.pop();
               
               for (auto [next, cost] : graph[curr]) {
                   if (price + cost < prices[next]) {
                       prices[next] = price + cost;
                       q.push({next, prices[next]});
                   }
               }
           }
           stops++;
       }
       
       return prices[dst] == INT_MAX ? -1 : prices[dst];
   }
   ```

4. **Path with Minimum Effort (LeetCode 1631)**
   - **Problem**: Find path with minimum absolute difference
   - **Difficulty**: Medium
   - **Key Concepts**: Modified Dijkstra for grid
   ```cpp
   int minimumEffortPath(vector<vector<int>>& heights) {
       int m = heights.size(), n = heights[0].size();
       vector<vector<int>> effort(m, vector<int>(n, INT_MAX));
       priority_queue<pair<int, pair<int, int>>, 
                     vector<pair<int, pair<int, int>>>,
                     greater<>> pq;
       
       effort[0][0] = 0;
       pq.push({0, {0, 0}});
       
       vector<pair<int, int>> dirs = {{0,1}, {1,0}, {0,-1}, {-1,0}};
       
       while (!pq.empty()) {
           auto [e, coord] = pq.top();
           auto [x, y] = coord;
           pq.pop();
           
           if (x == m-1 && y == n-1) return e;
           
           for (auto [dx, dy] : dirs) {
               int nx = x + dx, ny = y + dy;
               if (nx >= 0 && nx < m && ny >= 0 && ny < n) {
                   int newEffort = max(e, abs(heights[x][y] - heights[nx][ny]));
                   if (newEffort < effort[nx][ny]) {
                       effort[nx][ny] = newEffort;
                       pq.push({newEffort, {nx, ny}});
                   }
               }
           }
       }
       return effort[m-1][n-1];
   }
   ```

5. **Find the City With the Smallest Number of Neighbors (LeetCode 1334)**
   - **Problem**: Find city with fewest reachable cities within threshold distance
   - **Difficulty**: Medium
   - **Key Concepts**: Multiple Dijkstra runs
   ```cpp
   int findTheCity(int n, vector<vector<int>>& edges, int distanceThreshold) {
       vector<vector<pair<int, int>>> graph(n);
       for (const auto& edge : edges) {
           graph[edge[0]].push_back({edge[1], edge[2]});
           graph[edge[1]].push_back({edge[0], edge[2]});
       }
       
       int minCities = n;
       int result = -1;
       
       for (int city = 0; city < n; city++) {
           vector<int> distances = dijkstra(graph, n, city);
           
           int reachable = 0;
           for (int dist : distances) {
               if (dist <= distanceThreshold) reachable++;
           }
           
           if (reachable <= minCities) {
               minCities = reachable;
               result = city;
           }
       }
       
       return result;
   }
   
   vector<int> dijkstra(vector<vector<pair<int, int>>>& graph, int n, int src) {
       vector<int> dist(n, INT_MAX);
       priority_queue<pair<int, int>, vector<pair<int, int>>, greater<>> pq;
       
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

These problems demonstrate various applications and modifications of Dijkstra's algorithm for solving complex graph problems.

## Conclusion
Dijkstra's algorithm is a cornerstone of graph theory and has numerous practical applications in computer science and real-world systems. Its ability to find optimal paths in weighted graphs makes it invaluable for routing, networking, and optimization problems. Understanding both the basic implementation and its variations is crucial for solving complex path-finding challenges.