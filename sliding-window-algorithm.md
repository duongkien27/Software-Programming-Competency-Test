# Sliding Window Algorithm

## Introduction
The Sliding Window technique is an efficient method for processing arrays or strings by maintaining a window that slides through the data. It's particularly useful for solving problems involving subarrays or substrings that need to meet certain conditions.

## How Sliding Window Works
1. Create a window with certain constraints
2. Slide the window by expanding/contracting it
3. Process elements within the window
4. Track the best result as the window moves

## Types of Sliding Windows
1. **Fixed Size Window**
   - Window size remains constant
   - Slides through data sequentially

2. **Variable Size Window**
   - Window size changes based on conditions
   - Expands and contracts as needed

## Implementation Patterns

### Fixed Size Window Template
```cpp
vector<int> fixedSlidingWindow(vector<int>& nums, int k) {
    vector<int> result;
    int n = nums.size();
    
    // Initialize window
    // Process first k elements
    
    // Slide window
    for (int i = k; i < n; i++) {
        // Remove element going out of window
        // Add new element
        // Process current window
        // Update result
    }
    
    return result;
}
```

### Variable Size Window Template
```cpp
int variableSlidingWindow(vector<int>& nums, int target) {
    int result = 0;
    int left = 0;
    int sum = 0;
    
    for (int right = 0; right < nums.size(); right++) {
        // Expand window
        sum += nums[right];
        
        // Contract window while condition is violated
        while (sum > target) {
            sum -= nums[left];
            left++;
        }
        
        // Update result
        result = max(result, right - left + 1);
    }
    
    return result;
}
```

## Common Applications
1. **Substring Problems**
   - Find all substrings with constraints
   - Check substring existence
   - Count valid substrings

2. **Array Problems**
   - Maximum/Minimum subarray
   - Subarray with sum/product
   - Consecutive sequences

3. **String Problems**
   - Longest substring without repeating characters
   - Minimum window substring
   - String permutations

## Time and Space Complexity
- **Time Complexity**: O(n)
  - Usually linear as each element is processed at most twice
- **Space Complexity**: O(1) to O(k)
  - k is window size or auxiliary data structure size

## Challenges

Here are 5 popular LeetCode problems using Sliding Window:

1. **Longest Substring Without Repeating Characters (LeetCode 3)**
   - **Problem**: Find length of longest substring without repeating characters
   - **Difficulty**: Medium
   ```cpp
   class Solution {
   public:
       int lengthOfLongestSubstring(string s) {
           vector<int> chars(128, -1);
           int maxLen = 0;
           int left = 0;
           
           for (int right = 0; right < s.length(); right++) {
               if (chars[s[right]] >= left) {
                   left = chars[s[right]] + 1;
               }
               chars[s[right]] = right;
               maxLen = max(maxLen, right - left + 1);
           }
           
           return maxLen;
       }
   };
   ```

2. **Minimum Window Substring (LeetCode 76)**
   - **Problem**: Find minimum window containing all characters of pattern
   - **Difficulty**: Hard
   ```cpp
   class Solution {
   public:
       string minWindow(string s, string t) {
           vector<int> freq(128, 0);
           for (char c : t) freq[c]++;
           
           int counter = t.length();
           int start = 0, len = INT_MAX;
           int left = 0;
           
           for (int right = 0; right < s.length(); right++) {
               if (freq[s[right]]-- > 0) counter--;
               
               while (counter == 0) {
                   if (right - left + 1 < len) {
                       start = left;
                       len = right - left + 1;
                   }
                   
                   if (freq[s[left]]++ == 0) counter++;
                   left++;
               }
           }
           
           return len == INT_MAX ? "" : s.substr(start, len);
       }
   };
   ```

3. **Maximum Subarray (LeetCode 53)**
   - **Problem**: Find contiguous subarray with largest sum
   - **Difficulty**: Medium
   ```cpp
   class Solution {
   public:
       int maxSubArray(vector<int>& nums) {
           int maxSum = nums[0];
           int currentSum = nums[0];
           
           for (int i = 1; i < nums.size(); i++) {
               currentSum = max(nums[i], currentSum + nums[i]);
               maxSum = max(maxSum, currentSum);
           }
           
           return maxSum;
       }
   };
   ```

4. **Permutation in String (LeetCode 567)**
   - **Problem**: Check if string contains permutation of pattern
   - **Difficulty**: Medium
   ```cpp
   class Solution {
   public:
       bool checkInclusion(string s1, string s2) {
           if (s1.length() > s2.length()) return false;
           
           vector<int> freq1(26, 0), freq2(26, 0);
           
           // Initialize first window
           for (int i = 0; i < s1.length(); i++) {
               freq1[s1[i] - 'a']++;
               freq2[s2[i] - 'a']++;
           }
           
           if (freq1 == freq2) return true;
           
           // Slide window
           for (int i = s1.length(); i < s2.length(); i++) {
               freq2[s2[i - s1.length()] - 'a']--;
               freq2[s2[i] - 'a']++;
               
               if (freq1 == freq2) return true;
           }
           
           return false;
       }
   };
   ```

5. **Sliding Window Maximum (LeetCode 239)**
   - **Problem**: Find maximum element in each sliding window
   - **Difficulty**: Hard
   ```cpp
   class Solution {
   public:
       vector<int> maxSlidingWindow(vector<int>& nums, int k) {
           vector<int> result;
           deque<int> dq;
           
           for (int i = 0; i < nums.size(); i++) {
               // Remove elements outside current window
               while (!dq.empty() && dq.front() < i - k + 1)
                   dq.pop_front();
               
               // Remove smaller elements
               while (!dq.empty() && nums[dq.back()] < nums[i])
                   dq.pop_back();
               
               dq.push_back(i);
               
               // Add to result if window has reached size k
               if (i >= k - 1)
                   result.push_back(nums[dq.front()]);
           }
           
           return result;
       }
   };
   ```

## Best Practices
1. **Window Initialization**
   - Properly set up initial window
   - Handle edge cases

2. **Window Movement**
   - Clear rules for expanding/contracting
   - Maintain window invariants

3. **Result Tracking**
   - Update result at appropriate times
   - Handle all possible cases

4. **Optimization**
   - Use appropriate data structures
   - Minimize unnecessary operations

## Common Mistakes
1. Incorrect window bounds
2. Missing edge cases
3. Improper window updates
4. Inefficient data structure usage
5. Not handling duplicates

## Advanced Techniques

1. **Multiple Windows**
```cpp
int multipleWindows(vector<int>& nums) {
    int left1 = 0, left2 = 0;
    for (int right = 0; right < nums.size(); right++) {
        // Update first window
        while (condition1) {
            // Process first window
            left1++;
        }
        
        // Update second window
        while (condition2) {
            // Process second window
            left2++;
        }
        
        // Update result based on both windows
    }
    return result;
}
```

2. **Sliding Window with Data Structure**
```cpp
int slidingWindowWithDS(vector<int>& nums, int k) {
    map<int, int> window;
    
    // Initialize window
    for (int i = 0; i < k; i++) {
        window[nums[i]]++;
    }
    
    // Process result
    int result = processWindow(window);
    
    // Slide window
    for (int i = k; i < nums.size(); i++) {
        // Remove old element
        if (--window[nums[i-k]] == 0)
            window.erase(nums[i-k]);
        
        // Add new element
        window[nums[i]]++;
        
        // Update result
        result = max(result, processWindow(window));
    }
    
    return result;
}
```

## Tips for Solving Sliding Window Problems
1. Identify if problem can use sliding window
2. Choose between fixed/variable size
3. Determine window constraints
4. Plan window movement strategy
5. Handle edge cases carefully

## Conclusion
The Sliding Window technique is a powerful tool for solving array and string problems efficiently. Understanding both fixed and variable size windows, along with their applications, is crucial for tackling complex problems. The technique's linear time complexity makes it particularly valuable for processing large datasets.