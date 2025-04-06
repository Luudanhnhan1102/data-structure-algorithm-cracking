# Topics:
#bit-manipulation

# Goal:
The goal is to find the single number in an array where every other number appears twice.

---
# 1. Brute Force Approach
## Intuition
Iterates through the array and checks for duplicates.

## Code

```java
class Solution {
    public int singleNumber(int[] nums) {
        for (int i = 0; i < nums.length; i++) {
            boolean foundDuplicate = false;
            for (int j = 0; j < nums.length; j++) {
                if (i != j && nums[i] == nums[j]) {
                    foundDuplicate = true;
                    break;
                }
            }
            if (!foundDuplicate)
                return nums[i];
        }
        return -1; // Unreachable per problem constraints
    }
}
```

## Time and Space Complexity
#### Time complexity: $O(n^2)$
- Foreach element (n iterations), we potentially scan the entire array (n iterations) in the worst-case scenario.

#### Space complexity: $O(1)$
- The approach uses only a constant amount of extra space (a boolean flag) so the space complexity is O(1).


## Explanation:
### Implementation Details
1. **Outer Loop**: Iterating over each element in the array.
2. **Inner Loop**: For each element at index i, the inner loop iterates again over the array (index j) to check if there exists any duplicate (i.e., if there exists another element with the same value).
3. **Return**: If no duplicate is found for an element, that element is returned as the "single number."

---
# 2. Optimal Approach(Bit Manipulation)
## Intuition
The problem can be solved using bit manipulation, specifically the XOR operation.

The XOR operation has the property that:
<br/>
`a ^ a = 0` and `a ^ 0 = a`
<br/>
which allows us to cancel out pairs of identical numbers and isolate the single number.

## Code

```java
class Solution {
    public int singleNumber(int[] nums) {
        int result = 0;
        for(int num: nums) {
            result ^= num;
        }

        return result;
    }

    public static void main(String[] args) {
        Solution sol = new Solution();
        int[] test1 = {4,1,2,2,1};
        int[] test2 = {0};
        int[] test3 = {1};
        sol.singleNumber(test1);
        sol.singleNumber(test2);
        sol.singleNumber(test3);
    }
}
```

## Explanation:
### Key Insights
- ...

### Complexity
- Time complexity: `O(n)` (each element processed once)
- Space complexity: `O(1)` (only one integer variable used)


### Implementation Details
1. **Initialization**:
    - Initialize a variable `result` to 0. This will store the XOR of all numbers in the array.

2. **XOR Operation**:
    - Iterate through the array and apply the XOR operation between `result` and each number in the array.
    - The property `a ^ a = 0` ensures that pairs of identical numbers cancel each other out.
    - The property `a ^ 0 = a` isolates the single number.

3. **Termination**:
    - The `result` variable will contain the single number after processing all elements in the array.

4. **Edge Cases**:
    - If the array is empty, the function will return 0.

---
# Conclusion
## Key Takeaways
- **XOR operator (^):** has the property that any number XORed with 0 remains unchanged, i.e., a ^ 0 = a.
- **Time Efficiency:** The solution only requires a single pass through the array, making it $O(n)$
- **Space Efficiency:** The solution uses only a constant amount of extra space $O(1)$, regardless of the input size.

---
# Further Improvements?
## HashMap Approach:
- Idea: Count the frequency of each number using a hash map, then return the number with a count of one.