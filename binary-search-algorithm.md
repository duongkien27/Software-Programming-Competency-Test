# Binary Search Algorithm

## Introduction
Binary Search is a highly efficient searching algorithm that works on sorted arrays by repeatedly dividing the search interval in half. It reduces the search space by half with each iteration, leading to logarithmic time complexity.

## How Binary Search Works
1. Start with the entire array
2. Find the middle element
3. If target equals middle element, return its position
4. If target is greater, search right half
5. If target is smaller, search left half
6. Repeat until target is found or search space is empty

## Key Components
- **Sorted Array**: Input must be sorted
- **Mid Point**: Divides search space
- **Comparison**: Determines which half to search
- **Search Space**: Current range being considered

## Implementation

### Basic Binary Search
```cpp
int binarySearch(vector<int>& nums, int target) {
    int left = 0, right = nums.size() - 1;
    
    while (left <= right) {
        int mid = left + (right - left) / 2;
        
        if (nums[mid] == target)
            return mid;
        else if (nums[mid] < target)
            left = mid + 1;
        else
            right = mid - 1;
    }
    
    return -1;  // Element not found
}
```

### Lower Bound Implementation
```cpp
int lowerBound(vector<int>& nums, int target) {
    int left = 0, right = nums.size();
    
    while (left < right) {
        int mid = left + (right - left) / 2;
        
        if (nums[mid] < target)
            left = mid + 1;
        else
            right = mid;
    }
    
    return left;
}
```

### Upper Bound Implementation
```cpp
int upperBound(vector<int>& nums, int target) {
    int left = 0, right = nums.size();
    
    while (left < right) {
        int mid = left + (right - left) / 2;
        
        if (nums[mid] <= target)
            left = mid + 1;
        else
            right = mid;
    }
    
    return left;
}
```

## Common Applications
1. **Searching in Sorted Arrays**
   - Finding specific elements
   - Finding insertion positions
   - Range queries

2. **Answer Space Problems**
   - Minimization/Maximization
   - Finding optimal values
   - Rate limiting problems

3. **Optimization Problems**
   - Square root calculation
   - Division implementation
   - Finding peak elements

## Time and Space Complexity
- **Time Complexity**: O(log n)
  - Reduces search space by half each time
  - Extremely efficient for large datasets
- **Space Complexity**: O(1)
  - Only requires a few variables
  - No extra space needed

## Binary Search Variations

1. **Search in Rotated Sorted Array**
```cpp
int search(vector<int>& nums, int target) {
    int left = 0, right = nums.size() - 1;
    
    while (left <= right) {
        int mid = left + (right - left) / 2;
        
        if (nums[mid] == target)
            return mid;
        
        if (nums[left] <= nums[mid]) {
            if (nums[left] <= target && target < nums[mid])
                right = mid - 1;
            else
                left = mid + 1;
        } else {
            if (nums[mid] < target && target <= nums[right])
                left = mid + 1;
            else
                right = mid - 1;
        }
    }
    
    return -1;
}
```

2. **Find First and Last Position**
```cpp
vector<int> searchRange(vector<int>& nums, int target) {
    int first = lowerBound(nums, target);
    if (first == nums.size() || nums[first] != target)
        return {-1, -1};
    
    int last = upperBound(nums, target) - 1;
    return {first, last};
}
```

## Challenges

Here are 5 popular LeetCode problems using Binary Search:

1. **Search in Rotated Sorted Array (LeetCode 33)**
   - **Problem**: Search in a rotated sorted array
   - **Difficulty**: Medium
   ```cpp
   class Solution {
   public:
       int search(vector<int>& nums, int target) {
           int left = 0, right = nums.size() - 1;
           
           while (left <= right) {
               int mid = left + (right - left) / 2;
               
               if (nums[mid] == target)
                   return mid;
               
               // Left half is sorted
               if (nums[left] <= nums[mid]) {
                   if (nums[left] <= target && target < nums[mid])
                       right = mid - 1;
                   else
                       left = mid + 1;
               }
               // Right half is sorted
               else {
                   if (nums[mid] < target && target <= nums[right])
                       left = mid + 1;
                   else
                       right = mid - 1;
               }
           }
           return -1;
       }
   };
   ```

2. **Find Peak Element (LeetCode 162)**
   - **Problem**: Find a peak element in array
   - **Difficulty**: Medium
   ```cpp
   class Solution {
   public:
       int findPeakElement(vector<int>& nums) {
           int left = 0, right = nums.size() - 1;
           
           while (left < right) {
               int mid = left + (right - left) / 2;
               
               if (nums[mid] > nums[mid + 1])
                   right = mid;
               else
                   left = mid + 1;
           }
           return left;
       }
   };
   ```

3. **Median of Two Sorted Arrays (LeetCode 4)**
   - **Problem**: Find median of two sorted arrays
   - **Difficulty**: Hard
   ```cpp
   class Solution {
   public:
       double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
           if (nums1.size() > nums2.size())
               return findMedianSortedArrays(nums2, nums1);
           
           int x = nums1.size(), y = nums2.size();
           int low = 0, high = x;
           
           while (low <= high) {
               int partitionX = (low + high) / 2;
               int partitionY = (x + y + 1) / 2 - partitionX;
               
               int maxLeftX = (partitionX == 0) ? INT_MIN : nums1[partitionX - 1];
               int minRightX = (partitionX == x) ? INT_MAX : nums1[partitionX];
               
               int maxLeftY = (partitionY == 0) ? INT_MIN : nums2[partitionY - 1];
               int minRightY = (partitionY == y) ? INT_MAX : nums2[partitionY];
               
               if (maxLeftX <= minRightY && maxLeftY <= minRightX) {
                   if ((x + y) % 2 == 0)
                       return (max(maxLeftX, maxLeftY) + 
                              min(minRightX, minRightY)) / 2.0;
                   else
                       return max(maxLeftX, maxLeftY);
               }
               else if (maxLeftX > minRightY)
                   high = partitionX - 1;
               else
                   low = partitionX + 1;
           }
           return 0.0;
       }
   };
   ```

4. **Capacity To Ship Packages (LeetCode 1011)**
   - **Problem**: Find minimum capacity to ship packages within D days
   - **Difficulty**: Medium
   ```cpp
   class Solution {
   public:
       int shipWithinDays(vector<int>& weights, int days) {
           int left = *max_element(weights.begin(), weights.end());
           int right = accumulate(weights.begin(), weights.end(), 0);
           
           while (left < right) {
               int mid = left + (right - left) / 2;
               if (canShip(weights, days, mid))
                   right = mid;
               else
                   left = mid + 1;
           }
           return left;
       }
       
   private:
       bool canShip(vector<int>& weights, int days, int capacity) {
           int required = 1;
           int current = 0;
           
           for (int weight : weights) {
               if (current + weight > capacity) {
                   required++;
                   current = weight;
               } else {
                   current += weight;
               }
           }
           return required <= days;
       }
   };
   ```

5. **Kth Smallest Element in a Sorted Matrix (LeetCode 378)**
   - **Problem**: Find kth smallest element in sorted matrix
   - **Difficulty**: Medium
   ```cpp
   class Solution {
   public:
       int kthSmallest(vector<vector<int>>& matrix, int k) {
           int n = matrix.size();
           int left = matrix[0][0];
           int right = matrix[n-1][n-1];
           
           while (left < right) {
               int mid = left + (right - left) / 2;
               int count = 0;
               int j = n - 1;
               
               for (int i = 0; i < n; i++) {
                   while (j >= 0 && matrix[i][j] > mid)
                       j--;
                   count += (j + 1);
               }
               
               if (count < k)
                   left = mid + 1;
               else
                   right = mid;
           }
           return left;
       }
   };
   ```

## Best Practices
1. **Avoid Integer Overflow**
   - Use `mid = left + (right - left) / 2`
   - Instead of `mid = (left + right) / 2`

2. **Handle Base Cases**
   - Empty array
   - Single element
   - Element not found

3. **Choose Proper Bounds**
   - Decide between inclusive/exclusive bounds
   - Maintain loop invariants

4. **Prevent Infinite Loops**
   - Ensure search space reduces
   - Check termination conditions

## Common Mistakes
1. Incorrect mid calculation
2. Wrong boundary updates
3. Off-by-one errors
4. Not handling duplicates
5. Infinite loops

## Advanced Concepts
1. **Binary Search on Answer**
   - Search in a range of possible answers
   - Verify each potential solution

2. **Matrix Binary Search**
   - Search in 2D sorted matrices
   - Use row and column properties

3. **Rotated Array Search**
   - Handle rotated sorted arrays
   - Find pivot points

## Conclusion
Binary Search is a powerful algorithm that efficiently searches sorted data. Its logarithmic time complexity makes it extremely valuable for large datasets. Understanding its variations and applications is crucial for solving complex problems efficiently.