# Dynamic Programming

## Introduction
Dynamic Programming (DP) is a method for solving complex problems by breaking them down into simpler subproblems. It is applicable when subproblems overlap and have optimal substructure property. DP solutions typically store results of subproblems to avoid redundant computations.

## Key Concepts
1. **Optimal Substructure**
   - Problem can be solved using solutions to its subproblems
   - Optimal solution contains optimal solutions to subproblems

2. **Overlapping Subproblems**
   - Same subproblems encountered multiple times
   - Solutions can be stored and reused

3. **State and Transition**
   - State represents subproblem
   - Transition defines how to move between states

## Implementation Approaches

### 1. Top-Down (Memoization)
```cpp
// Fibonacci example with memoization
class Solution {
    unordered_map<int, int> memo;
public:
    int fib(int n) {
        if (n <= 1) return n;
        if (memo.count(n)) return memo[n];
        return memo[n] = fib(n-1) + fib(n-2);
    }
};
```

### 2. Bottom-Up (Tabulation)
```cpp
// Fibonacci example with tabulation
class Solution {
public:
    int fib(int n) {
        if (n <= 1) return n;
        vector<int> dp(n + 1);
        dp[0] = 0;
        dp[1] = 1;
        
        for (int i = 2; i <= n; i++)
            dp[i] = dp[i-1] + dp[i-2];
            
        return dp[n];
    }
};
```

### 3. Space-Optimized
```cpp
// Fibonacci with optimized space
class Solution {
public:
    int fib(int n) {
        if (n <= 1) return n;
        int prev2 = 0, prev1 = 1;
        
        for (int i = 2; i <= n; i++) {
            int curr = prev1 + prev2;
            prev2 = prev1;
            prev1 = curr;
        }
        
        return prev1;
    }
};
```

## Common DP Patterns

### 1. Linear DP
```cpp
// Maximum Subarray Sum
int maxSubArray(vector<int>& nums) {
    int maxSum = nums[0];
    int currSum = nums[0];
    
    for (int i = 1; i < nums.size(); i++) {
        currSum = max(nums[i], currSum + nums[i]);
        maxSum = max(maxSum, currSum);
    }
    
    return maxSum;
}
```

### 2. Matrix DP
```cpp
// Minimum Path Sum
int minPathSum(vector<vector<int>>& grid) {
    int m = grid.size(), n = grid[0].size();
    vector<vector<int>> dp(m, vector<int>(n));
    
    dp[0][0] = grid[0][0];
    
    for (int i = 1; i < m; i++)
        dp[i][0] = dp[i-1][0] + grid[i][0];
        
    for (int j = 1; j < n; j++)
        dp[0][j] = dp[0][j-1] + grid[0][j];
    
    for (int i = 1; i < m; i++)
        for (int j = 1; j < n; j++)
            dp[i][j] = min(dp[i-1][j], dp[i][j-1]) + grid[i][j];
    
    return dp[m-1][n-1];
}
```

### 3. String DP
```cpp
// Longest Common Subsequence
int longestCommonSubsequence(string text1, string text2) {
    int m = text1.length(), n = text2.length();
    vector<vector<int>> dp(m + 1, vector<int>(n + 1, 0));
    
    for (int i = 1; i <= m; i++)
        for (int j = 1; j <= n; j++)
            if (text1[i-1] == text2[j-1])
                dp[i][j] = dp[i-1][j-1] + 1;
            else
                dp[i][j] = max(dp[i-1][j], dp[i][j-1]);
    
    return dp[m][n];
}
```

## Challenges

Here are 5 classic DP problems with solutions:

1. **Climbing Stairs (LeetCode 70)**
   - **Problem**: Count ways to climb n stairs taking 1 or 2 steps
   - **Difficulty**: Easy
   ```cpp
   class Solution {
   public:
       int climbStairs(int n) {
           if (n <= 2) return n;
           
           vector<int> dp(n + 1);
           dp[1] = 1;
           dp[2] = 2;
           
           for (int i = 3; i <= n; i++)
               dp[i] = dp[i-1] + dp[i-2];
           
           return dp[n];
       }
   };
   ```

2. **Coin Change (LeetCode 322)**
   - **Problem**: Find minimum coins needed for amount
   - **Difficulty**: Medium
   ```cpp
   class Solution {
   public:
       int coinChange(vector<int>& coins, int amount) {
           vector<int> dp(amount + 1, amount + 1);
           dp[0] = 0;
           
           for (int i = 1; i <= amount; i++)
               for (int coin : coins)
                   if (coin <= i)
                       dp[i] = min(dp[i], dp[i - coin] + 1);
           
           return dp[amount] > amount ? -1 : dp[amount];
       }
   };
   ```

3. **Longest Increasing Subsequence (LeetCode 300)**
   - **Problem**: Find length of longest increasing subsequence
   - **Difficulty**: Medium
   ```cpp
   class Solution {
   public:
       int lengthOfLIS(vector<int>& nums) {
           vector<int> dp(nums.size(), 1);
           int maxLen = 1;
           
           for (int i = 1; i < nums.size(); i++) {
               for (int j = 0; j < i; j++) {
                   if (nums[i] > nums[j]) {
                       dp[i] = max(dp[i], dp[j] + 1);
                   }
               }
               maxLen = max(maxLen, dp[i]);
           }
           
           return maxLen;
       }
   };
   ```

4. **Edit Distance (LeetCode 72)**
   - **Problem**: Find minimum operations to convert one string to another
   - **Difficulty**: Hard
   ```cpp
   class Solution {
   public:
       int minDistance(string word1, string word2) {
           int m = word1.length(), n = word2.length();
           vector<vector<int>> dp(m + 1, vector<int>(n + 1));
           
           for (int i = 0; i <= m; i++)
               dp[i][0] = i;
           for (int j = 0; j <= n; j++)
               dp[0][j] = j;
           
           for (int i = 1; i <= m; i++) {
               for (int j = 1; j <= n; j++) {
                   if (word1[i-1] == word2[j-1])
                       dp[i][j] = dp[i-1][j-1];
                   else
                       dp[i][j] = min({dp[i-1][j-1], dp[i-1][j], dp[i][j-1]}) + 1;
               }
           }
           
           return dp[m][n];
       }
   };
   ```

5. **Maximum Square (LeetCode 221)**
   - **Problem**: Find area of largest square of 1's
   - **Difficulty**: Medium
   ```cpp
   class Solution {
   public:
       int maximalSquare(vector<vector<char>>& matrix) {
           if (matrix.empty()) return 0;
           int m = matrix.size(), n = matrix[0].size();
           vector<vector<int>> dp(m + 1, vector<int>(n + 1, 0));
           int maxSide = 0;
           
           for (int i = 1; i <= m; i++) {
               for (int j = 1; j <= n; j++) {
                   if (matrix[i-1][j-1] == '1') {
                       dp[i][j] = min({dp[i-1][j], dp[i][j-1], dp[i-1][j-1]}) + 1;
                       maxSide = max(maxSide, dp[i][j]);
                   }
               }
           }
           
           return maxSide * maxSide;
       }
   };
   ```

## Advanced DP Techniques

### 1. State Compression
```cpp
// Traveling Salesman Problem
int tsp(vector<vector<int>>& dist) {
    int n = dist.size();
    vector<vector<int>> dp(1 << n, vector<int>(n, INT_MAX));
    dp[1][0] = 0;  // Start at city 0
    
    for (int mask = 1; mask < (1 << n); mask++) {
        for (int curr = 0; curr < n; curr++) {
            if (!(mask & (1 << curr))) continue;
            
            for (int next = 0; next < n; next++) {
                if (mask & (1 << next)) continue;
                
                int newMask = mask | (1 << next);
                dp[newMask][next] = min(dp[newMask][next],
                                      dp[mask][curr] + dist[curr][next]);
            }
        }
    }
    
    int finalMask = (1 << n) - 1;
    return dp[finalMask][0];
}
```

### 2. Interval DP
```cpp
// Matrix Chain Multiplication
int matrixChainMultiplication(vector<int>& dims) {
    int n = dims.size() - 1;
    vector<vector<int>> dp(n, vector<int>(n, INT_MAX));
    
    // Base case: single matrix
    for (int i = 0; i < n; i++)
        dp[i][i] = 0;
    
    // Length of chain
    for (int len = 2; len <= n; len++) {
        // Start of chain
        for (int i = 0; i < n - len + 1; i++) {
            int j = i + len - 1;
            // Split point
            for (int k = i; k < j; k++) {
                dp[i][j] = min(dp[i][j],
                             dp[i][k] + dp[k+1][j] +
                             dims[i] * dims[k+1] * dims[j+1]);
            }
        }
    }
    
    return dp[0][n-1];
}
```

## Best Practices
1. **State Design**
   - Keep states minimal but sufficient
   - Avoid redundant state information

2. **Transition Function**
   - Make transitions clear and efficient
   - Consider all possible state changes

3. **Base Cases**
   - Handle edge cases properly
   - Initialize DP table correctly

4. **Space Optimization**
   - Consider memory constraints
   - Optimize when possible

## Common Mistakes
1. Incorrect state definition
2. Missing base cases
3. Wrong transition function
4. Not considering constraints
5. Unnecessary state information

## Problem-Solving Steps
1. **Identify if DP is applicable**
   - Check for optimal substructure
   - Look for overlapping subproblems

2. **Define States**
   - Determine what information to store
   - Keep states minimal

3. **Define Transitions**
   - How to move between states
   - Write recurrence relation

4. **Initialize Base Cases**
   - Handle smallest subproblems
   - Set initial values

5. **Implement Solution**
   - Choose approach (top-down/bottom-up)
   - Optimize if needed

## Conclusion
Dynamic Programming is a powerful technique for solving optimization problems. Success in DP requires practice in identifying patterns, designing states, and writing efficient transitions. Understanding these concepts and patterns is crucial for solving complex algorithmic problems.