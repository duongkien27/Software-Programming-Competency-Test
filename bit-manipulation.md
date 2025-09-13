# Bit Manipulation Techniques

## Introduction
Bit manipulation involves performing operations at the bit level. It's used for optimizing space usage, performing fast calculations, and solving various algorithmic problems. Understanding bit operations is crucial for low-level programming and optimization.

## Basic Operations

### Common Bit Operations
```cpp
// Set bit
int setBit(int n, int pos) {
    return n | (1 << pos);
}

// Clear bit
int clearBit(int n, int pos) {
    return n & ~(1 << pos);
}

// Toggle bit
int toggleBit(int n, int pos) {
    return n ^ (1 << pos);
}

// Check if bit is set
bool isSet(int n, int pos) {
    return (n & (1 << pos)) != 0;
}

// Count set bits
int countSetBits(int n) {
    int count = 0;
    while (n) {
        count += n & 1;
        n >>= 1;
    }
    return count;
}
```

### Bit Manipulation Tricks
```cpp
// Check if number is power of 2
bool isPowerOfTwo(int n) {
    return n > 0 && (n & (n - 1)) == 0;
}

// Get rightmost set bit
int rightmostSetBit(int n) {
    return n & -n;
}

// Clear rightmost set bit
int clearRightmostSetBit(int n) {
    return n & (n - 1);
}

// Get all subsets of a set
vector<vector<int>> getAllSubsets(vector<int>& nums) {
    int n = nums.size();
    vector<vector<int>> subsets;
    
    for (int i = 0; i < (1 << n); i++) {
        vector<int> subset;
        for (int j = 0; j < n; j++) {
            if (i & (1 << j))
                subset.push_back(nums[j]);
        }
        subsets.push_back(subset);
    }
    return subsets;
}
```

## Common Bit Operations Table
| Operation | Example | Result |
|-----------|---------|---------|
| AND (&) | 5 & 3 | 1 |
| OR (&#124;) | 5 &#124; 3 | 7 |
| XOR (^) | 5 ^ 3 | 6 |
| NOT (~) | ~5 | -6 |
| Left Shift (<<) | 5 << 1 | 10 |
| Right Shift (>>) | 5 >> 1 | 2 |

## Applications
1. **Optimization**
   - Space optimization
   - Fast arithmetic operations
   - Flag management

2. **Data Structures**
   - Bitmap implementation
   - Compact data storage
   - State representation

3. **Algorithms**
   - Subset generation
   - Permutation generation
   - XOR based problems

## Challenges

Here are 5 popular LeetCode problems using Bit Manipulation:

1. **Single Number (LeetCode 136)**
   - **Problem**: Find number that appears once in array
   - **Difficulty**: Easy
   ```cpp
   class Solution {
   public:
       int singleNumber(vector<int>& nums) {
           int result = 0;
           for (int num : nums)
               result ^= num;
           return result;
       }
   };
   ```

2. **Number of 1 Bits (LeetCode 191)**
   - **Problem**: Count number of 1 bits in integer
   - **Difficulty**: Easy
   ```cpp
   class Solution {
   public:
       int hammingWeight(uint32_t n) {
           int count = 0;
           while (n) {
               n &= (n - 1);  // Clear rightmost set bit
               count++;
           }
           return count;
       }
   };
   ```

3. **Power of Two (LeetCode 231)**
   - **Problem**: Check if number is power of two
   - **Difficulty**: Easy
   ```cpp
   class Solution {
   public:
       bool isPowerOfTwo(int n) {
           return n > 0 && (n & (n - 1)) == 0;
       }
   };
   ```

4. **Missing Number (LeetCode 268)**
   - **Problem**: Find missing number in sequence
   - **Difficulty**: Easy
   ```cpp
   class Solution {
   public:
       int missingNumber(vector<int>& nums) {
           int result = nums.size();
           for (int i = 0; i < nums.size(); i++)
               result ^= i ^ nums[i];
           return result;
       }
   };
   ```

5. **Sum of Two Integers (LeetCode 371)**
   - **Problem**: Add two integers without + or -
   - **Difficulty**: Medium
   ```cpp
   class Solution {
   public:
       int getSum(int a, int b) {
           while (b != 0) {
               unsigned int carry = a & b;
               a = a ^ b;
               b = carry << 1;
           }
           return a;
       }
   };
   ```

## Advanced Bit Techniques

### Bit Masks
```cpp
// Create mask with n bits set
int createMask(int n) {
    return (1 << n) - 1;
}

// Set range of bits
int setRange(int num, int start, int end) {
    int mask = createMask(end - start + 1);
    return num | (mask << start);
}

// Clear range of bits
int clearRange(int num, int start, int end) {
    int mask = createMask(end - start + 1);
    return num & ~(mask << start);
}
```

### Bit Manipulation for Arrays
```cpp
// Find two numbers appearing once
vector<int> findTwoSingle(vector<int>& nums) {
    int xorResult = 0;
    for (int num : nums)
        xorResult ^= num;
    
    // Get rightmost set bit
    int rightmostSet = xorResult & -xorResult;
    
    int x = 0, y = 0;
    for (int num : nums) {
        if (num & rightmostSet)
            x ^= num;
        else
            y ^= num;
    }
    
    return {x, y};
}
```

## Best Practices
1. **Use Parentheses**
   - Clarify operation precedence
   - Avoid unexpected results

2. **Consider Sign**
   - Handle negative numbers
   - Be aware of sign extension

3. **Test Edge Cases**
   - Maximum/Minimum values
   - Zero and negative values

4. **Document Bit Operations**
   - Explain purpose of operations
   - Make code maintainable

## Common Bit Manipulation Patterns

1. **Set Operations**
```cpp
// Set union
int setUnion(int a, int b) {
    return a | b;
}

// Set intersection
int setIntersection(int a, int b) {
    return a & b;
}

// Set difference
int setDifference(int a, int b) {
    return a & ~b;
}
```

2. **Bit Counting**
```cpp
// Count trailing zeros
int countTrailingZeros(int n) {
    return __builtin_ctz(n);  // GCC built-in
}

// Count leading zeros
int countLeadingZeros(int n) {
    return __builtin_clz(n);  // GCC built-in
}

// Population count (count set bits)
int popCount(int n) {
    return __builtin_popcount(n);  // GCC built-in
}
```

3. **Bit Reversal**
```cpp
// Reverse bits
uint32_t reverseBits(uint32_t n) {
    uint32_t result = 0;
    for (int i = 0; i < 32; i++) {
        result = (result << 1) | (n & 1);
        n >>= 1;
    }
    return result;
}
```

## Tips for Bit Manipulation
1. Understand basic operations thoroughly
2. Practice with visualization
3. Use built-in functions when available
4. Consider performance implications
5. Test with various inputs

## Common Mistakes
1. Ignoring operator precedence
2. Forgetting about sign extension
3. Not handling edge cases
4. Incorrect shift amounts
5. Assuming 32-bit integers

## Conclusion
Bit manipulation is a powerful technique that can lead to efficient solutions for various problems. Understanding these operations and patterns is essential for writing optimized code and solving certain types of algorithmic problems efficiently.