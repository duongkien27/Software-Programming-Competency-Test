# Backtracking Algorithm

## Introduction
Backtracking is an algorithmic technique that considers searching every possible combination in order to solve a computational problem. It builds candidates to the solution incrementally and abandons candidates ("backtracks") as soon as it determines that the candidate cannot lead to a valid solution.

## How Backtracking Works
1. Start with an empty solution
2. Add elements one by one to build the solution
3. If current state is invalid, backtrack (remove last element)
4. If current state is valid and complete, add it to solutions
5. Continue until all possibilities are explored

## Key Components
- **Base Case**: Condition to stop recursion
- **Choices**: Available options at each step
- **Constraints**: Rules that solutions must satisfy
- **State Space Tree**: Representation of all possible states

## Implementation Pattern

### C++ Generic Backtracking Template
```cpp
void backtrack(vector<Solution>& result, Solution& current, Input& data, int position) {
    if (isComplete(current)) {
        result.push_back(current);
        return;
    }
    
    for (Choice choice : getChoices(data, position)) {
        if (isValid(current, choice)) {
            makeChoice(current, choice);
            backtrack(result, current, data, position + 1);
            undoChoice(current, choice);
        }
    }
}
```

### Example: N-Queens Problem
```cpp
class Solution {
public:
    vector<vector<string>> solveNQueens(int n) {
        vector<vector<string>> result;
        vector<string> board(n, string(n, '.'));
        backtrack(result, board, 0, n);
        return result;
    }
    
private:
    void backtrack(vector<vector<string>>& result, vector<string>& board, 
                   int row, int n) {
        if (row == n) {
            result.push_back(board);
            return;
        }
        
        for (int col = 0; col < n; col++) {
            if (isValid(board, row, col, n)) {
                board[row][col] = 'Q';
                backtrack(result, board, row + 1, n);
                board[row][col] = '.';
            }
        }
    }
    
    bool isValid(vector<string>& board, int row, int col, int n) {
        // Check column
        for (int i = 0; i < row; i++)
            if (board[i][col] == 'Q')
                return false;
        
        // Check left diagonal
        for (int i = row - 1, j = col - 1; i >= 0 && j >= 0; i--, j--)
            if (board[i][j] == 'Q')
                return false;
        
        // Check right diagonal
        for (int i = row - 1, j = col + 1; i >= 0 && j < n; i--, j++)
            if (board[i][j] == 'Q')
                return false;
        
        return true;
    }
};
```

## Common Applications
1. **Combinatorial Problems**
   - Permutations
   - Combinations
   - Subset generation

2. **Constraint Satisfaction**
   - N-Queens
   - Sudoku
   - Map coloring

3. **Game Playing**
   - Chess
   - Tic-tac-toe
   - Puzzle solving

4. **Path Finding**
   - Maze solving
   - Hamilton path
   - Knight's tour

## Time and Space Complexity
- **Time Complexity**: O(b^d)
  - b: branching factor (choices at each step)
  - d: maximum depth of tree
- **Space Complexity**: O(d)
  - d: maximum depth of recursion tree

## Best Practices
1. **Early Pruning**
   - Check validity as early as possible
   - Prune invalid branches immediately

2. **State Management**
   - Maintain clean state after backtracking
   - Efficiently track visited states

3. **Optimization**
   - Order choices to find solutions faster
   - Use efficient data structures

4. **Memory Usage**
   - Minimize copying of data structures
   - Use references where possible

## Common Variations
1. **Branch and Bound**
   - Uses bounds to prune branches
   - Optimizes for best solution

2. **Forward Checking**
   - Checks future constraints
   - Reduces backtracking needs

3. **Iterative Deepening**
   - Combines with depth limits
   - Memory efficient exploration

## Challenges

Here are 5 popular LeetCode problems that use backtracking:

1. **Permutations (LeetCode 46)**
   - **Problem**: Generate all possible permutations of an array
   - **Difficulty**: Medium
   ```cpp
   class Solution {
   public:
       vector<vector<int>> permute(vector<int>& nums) {
           vector<vector<int>> result;
           backtrack(result, nums, 0);
           return result;
       }
       
   private:
       void backtrack(vector<vector<int>>& result, vector<int>& nums, int start) {
           if (start == nums.size()) {
               result.push_back(nums);
               return;
           }
           
           for (int i = start; i < nums.size(); i++) {
               swap(nums[start], nums[i]);
               backtrack(result, nums, start + 1);
               swap(nums[start], nums[i]);
           }
       }
   };
   ```

2. **Subsets (LeetCode 78)**
   - **Problem**: Generate all possible subsets of an array
   - **Difficulty**: Medium
   ```cpp
   class Solution {
   public:
       vector<vector<int>> subsets(vector<int>& nums) {
           vector<vector<int>> result;
           vector<int> current;
           backtrack(result, current, nums, 0);
           return result;
       }
       
   private:
       void backtrack(vector<vector<int>>& result, vector<int>& current, 
                     vector<int>& nums, int start) {
           result.push_back(current);
           
           for (int i = start; i < nums.size(); i++) {
               current.push_back(nums[i]);
               backtrack(result, current, nums, i + 1);
               current.pop_back();
           }
       }
   };
   ```

3. **Palindrome Partitioning (LeetCode 131)**
   - **Problem**: Find all possible palindrome partitioning of a string
   - **Difficulty**: Medium
   ```cpp
   class Solution {
   public:
       vector<vector<string>> partition(string s) {
           vector<vector<string>> result;
           vector<string> current;
           backtrack(result, current, s, 0);
           return result;
       }
       
   private:
       void backtrack(vector<vector<string>>& result, vector<string>& current,
                     string& s, int start) {
           if (start == s.length()) {
               result.push_back(current);
               return;
           }
           
           for (int i = start; i < s.length(); i++) {
               if (isPalindrome(s, start, i)) {
                   current.push_back(s.substr(start, i - start + 1));
                   backtrack(result, current, s, i + 1);
                   current.pop_back();
               }
           }
       }
       
       bool isPalindrome(string& s, int left, int right) {
           while (left < right) {
               if (s[left++] != s[right--])
                   return false;
           }
           return true;
       }
   };
   ```

4. **Combination Sum (LeetCode 39)**
   - **Problem**: Find all combinations that sum to target
   - **Difficulty**: Medium
   ```cpp
   class Solution {
   public:
       vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
           vector<vector<int>> result;
           vector<int> current;
           backtrack(result, current, candidates, target, 0);
           return result;
       }
       
   private:
       void backtrack(vector<vector<int>>& result, vector<int>& current,
                     vector<int>& candidates, int remain, int start) {
           if (remain < 0)
               return;
           if (remain == 0) {
               result.push_back(current);
               return;
           }
           
           for (int i = start; i < candidates.size(); i++) {
               current.push_back(candidates[i]);
               backtrack(result, current, candidates, remain - candidates[i], i);
               current.pop_back();
           }
       }
   };
   ```

5. **Word Search (LeetCode 79)**
   - **Problem**: Find if a word exists in a 2D board
   - **Difficulty**: Medium
   ```cpp
   class Solution {
   public:
       bool exist(vector<vector<char>>& board, string word) {
           int m = board.size(), n = board[0].size();
           for (int i = 0; i < m; i++) {
               for (int j = 0; j < n; j++) {
                   if (backtrack(board, word, i, j, 0))
                       return true;
               }
           }
           return false;
       }
       
   private:
       bool backtrack(vector<vector<char>>& board, string& word,
                     int row, int col, int index) {
           if (index == word.length())
               return true;
           
           if (row < 0 || row >= board.size() || col < 0 || col >= board[0].size() ||
               board[row][col] != word[index])
               return false;
           
           char temp = board[row][col];
           board[row][col] = '#';  // mark as visited
           
           bool found = backtrack(board, word, row + 1, col, index + 1) ||
                       backtrack(board, word, row - 1, col, index + 1) ||
                       backtrack(board, word, row, col + 1, index + 1) ||
                       backtrack(board, word, row, col - 1, index + 1);
           
           board[row][col] = temp;  // restore the cell
           return found;
       }
   };
   ```

## Tips for Solving Backtracking Problems
1. Identify the decision space
2. Define clear base cases
3. Maintain state properly
4. Consider optimization opportunities
5. Test with small inputs first

## Common Mistakes to Avoid
1. Not restoring state after backtracking
2. Inefficient state copying
3. Missing base cases
4. Poor pruning implementation
5. Unnecessary iterations

## Conclusion
Backtracking is a powerful technique for solving complex problems by systematically exploring all possible solutions. While it can be computationally expensive, proper implementation of pruning and optimization techniques can make it highly effective for many practical applications.