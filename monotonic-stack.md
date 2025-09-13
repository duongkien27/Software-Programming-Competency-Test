# Monotonic Stack

## Introduction
A Monotonic Stack is a stack that maintains its elements either in strictly increasing or strictly decreasing order. It's particularly useful for solving problems involving finding the next greater/smaller element, or problems requiring maintaining order in a sequence.

## Types of Monotonic Stacks
1. **Monotonic Increasing Stack**
   - Each element in the stack is greater than the previous elements
   - Used to find next smaller element
   
2. **Monotonic Decreasing Stack**
   - Each element in the stack is smaller than the previous elements
   - Used to find next greater element

## Implementation

### Basic Monotonic Stack Template
```cpp
// Monotonic Increasing Stack
vector<int> monotonic_increasing_stack(vector<int>& nums) {
    stack<int> st;
    vector<int> result(nums.size());
    
    for (int i = 0; i < nums.size(); i++) {
        while (!st.empty() && nums[st.top()] > nums[i]) {
            st.pop();
        }
        result[i] = st.empty() ? -1 : st.top();
        st.push(i);
    }
    
    return result;
}

// Monotonic Decreasing Stack
vector<int> monotonic_decreasing_stack(vector<int>& nums) {
    stack<int> st;
    vector<int> result(nums.size());
    
    for (int i = 0; i < nums.size(); i++) {
        while (!st.empty() && nums[st.top()] < nums[i]) {
            st.pop();
        }
        result[i] = st.empty() ? -1 : st.top();
        st.push(i);
    }
    
    return result;
}
```

### Next Greater Element Implementation
```cpp
vector<int> nextGreaterElement(vector<int>& nums) {
    int n = nums.size();
    vector<int> result(n, -1);
    stack<int> st;  // stores indices
    
    for (int i = 0; i < n; i++) {
        // Pop elements that are smaller than current element
        while (!st.empty() && nums[st.top()] < nums[i]) {
            result[st.top()] = nums[i];
            st.pop();
        }
        st.push(i);
    }
    
    return result;
}
```

## Common Applications

1. **Next Greater/Smaller Element**
   - Find the next greater/smaller element for each element in array
   - Useful in stock price problems

2. **Histogram Problems**
   - Find largest rectangle in histogram
   - Calculate area with different constraints

3. **Temperature Problems**
   - Daily temperatures
   - Waiting time problems

4. **String Problems**
   - Remove duplicate letters
   - String manipulation with ordering

## Time and Space Complexity
- **Time Complexity**: O(n)
  - Each element is pushed and popped at most once
- **Space Complexity**: O(n)
  - In worst case, all elements might be in stack

## Challenges

Here are 5 popular LeetCode problems using Monotonic Stack:

1. **Daily Temperatures (LeetCode 739)**
   - **Problem**: Find number of days to wait for warmer temperature
   - **Difficulty**: Medium
   ```cpp
   class Solution {
   public:
       vector<int> dailyTemperatures(vector<int>& temperatures) {
           int n = temperatures.size();
           vector<int> result(n, 0);
           stack<int> st;
           
           for (int i = 0; i < n; i++) {
               while (!st.empty() && temperatures[st.top()] < temperatures[i]) {
                   int prev = st.top();
                   st.pop();
                   result[prev] = i - prev;
               }
               st.push(i);
           }
           
           return result;
       }
   };
   ```

2. **Next Greater Element I (LeetCode 496)**
   - **Problem**: Find next greater element in another array
   - **Difficulty**: Easy
   ```cpp
   class Solution {
   public:
       vector<int> nextGreaterElement(vector<int>& nums1, vector<int>& nums2) {
           unordered_map<int, int> nextGreater;
           stack<int> st;
           
           for (int num : nums2) {
               while (!st.empty() && st.top() < num) {
                   nextGreater[st.top()] = num;
                   st.pop();
               }
               st.push(num);
           }
           
           vector<int> result;
           for (int num : nums1) {
               result.push_back(nextGreater.count(num) ? nextGreater[num] : -1);
           }
           
           return result;
       }
   };
   ```

3. **Largest Rectangle in Histogram (LeetCode 84)**
   - **Problem**: Find largest rectangular area in histogram
   - **Difficulty**: Hard
   ```cpp
   class Solution {
   public:
       int largestRectangleArea(vector<int>& heights) {
           stack<int> st;
           int maxArea = 0;
           int n = heights.size();
           
           for (int i = 0; i <= n; i++) {
               int h = (i == n ? 0 : heights[i]);
               
               while (!st.empty() && heights[st.top()] > h) {
                   int height = heights[st.top()];
                   st.pop();
                   int width = st.empty() ? i : i - st.top() - 1;
                   maxArea = max(maxArea, height * width);
               }
               
               st.push(i);
           }
           
           return maxArea;
       }
   };
   ```

4. **Remove K Digits (LeetCode 402)**
   - **Problem**: Remove k digits to form smallest possible number
   - **Difficulty**: Medium
   ```cpp
   class Solution {
   public:
       string removeKdigits(string num, int k) {
           string result;
           
           for (char c : num) {
               while (!result.empty() && k > 0 && result.back() > c) {
                   result.pop_back();
                   k--;
               }
               if (!result.empty() || c != '0') {
                   result.push_back(c);
               }
           }
           
           while (!result.empty() && k--) {
               result.pop_back();
           }
           
           return result.empty() ? "0" : result;
       }
   };
   ```

5. **132 Pattern (LeetCode 456)**
   - **Problem**: Find 132 pattern in array
   - **Difficulty**: Medium
   ```cpp
   class Solution {
   public:
       bool find132pattern(vector<int>& nums) {
           stack<int> st;
           int s3 = INT_MIN;
           
           for (int i = nums.size() - 1; i >= 0; i--) {
               if (nums[i] < s3) return true;
               
               while (!st.empty() && st.top() < nums[i]) {
                   s3 = st.top();
                   st.pop();
               }
               
               st.push(nums[i]);
           }
           
           return false;
       }
   };
   ```

## Best Practices

1. **Stack Usage**
   - Decide whether to store values or indices
   - Choose increasing vs decreasing based on problem
   - Handle edge cases properly

2. **Implementation**
   - Initialize stack correctly
   - Process elements in right order
   - Handle remaining elements in stack

3. **Optimization**
   - Consider using vector as stack
   - Remove unnecessary operations
   - Handle memory efficiently

## Common Patterns

1. **Next Greater Element Pattern**
```cpp
vector<int> nextGreater(vector<int>& arr) {
    int n = arr.size();
    vector<int> result(n, -1);
    stack<int> st;
    
    for (int i = 0; i < n; i++) {
        while (!st.empty() && arr[st.top()] < arr[i]) {
            result[st.top()] = arr[i];
            st.pop();
        }
        st.push(i);
    }
    return result;
}
```

2. **Previous Smaller Element Pattern**
```cpp
vector<int> prevSmaller(vector<int>& arr) {
    int n = arr.size();
    vector<int> result(n, -1);
    stack<int> st;
    
    for (int i = 0; i < n; i++) {
        while (!st.empty() && arr[st.top()] > arr[i]) {
            st.pop();
        }
        if (!st.empty()) {
            result[i] = arr[st.top()];
        }
        st.push(i);
    }
    return result;
}
```

## Tips for Solving Monotonic Stack Problems
1. Identify if problem needs next/previous greater/smaller element
2. Choose appropriate stack type (increasing/decreasing)
3. Decide whether to store indices or values
4. Handle edge cases and empty stack situations
5. Process remaining elements in stack if needed

## Common Mistakes
1. Using wrong stack type
2. Forgetting to handle remaining elements
3. Incorrect comparison operators
4. Not considering edge cases
5. Inefficient stack operations

## Conclusion
Monotonic Stack is a powerful data structure for solving problems involving element relationships and order maintenance. Understanding its patterns and applications is crucial for tackling various algorithmic problems efficiently. The key is recognizing when to use it and which type of monotonic stack fits the problem requirements.