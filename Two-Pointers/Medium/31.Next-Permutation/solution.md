# Topics:
#two-pointers

---
## Code - Solution 1

```java
class Solution {
    public void nextPermutation(int[] nums) {
        int targetIndex = findTargetIndex(nums, nums.length - 1);
        if ( targetIndex == -1 || nums.length <= 2 ) {
            reversePermutation(nums);
        } else {
            // find the closest number that need to be swapped into the target index slot
            int swappingIndex = targetIndex + 1;
            int distance = nums[swappingIndex] - nums[targetIndex];
            for (int i = swappingIndex + 1; i < nums.length; i++) {
                int temp = nums[i] - nums[targetIndex];
                if ( temp > 0 && temp <= distance ) {
                    swappingIndex = i;
                    distance = nums[i] - nums[targetIndex];
                }
            }
            swap(nums, targetIndex, swappingIndex);

            if ( nums.length - 1 - targetIndex >= 2 ) {
                //split array into subarray that needs to be reversed
                int[] swappingArray = new int[nums.length - targetIndex - 1];
                for (int i = 0; i < swappingArray.length; i++) {
                    swappingArray[i] = nums[targetIndex + i + 1];
                }
                reversePermutation(swappingArray);
                //merge array data back into nums
                for (int i = 0; i < swappingArray.length; i++) {
                    nums[targetIndex + i + 1] = swappingArray[i];
                }
            }
        }
    }

    public void reversePermutation(int[] nums) {
        int i = 0, j = nums.length - 1;
        while (i < j) {
            swap(nums, i, j);
            i++;
            j--;
        }
    }
    
    public int findTargetIndex(int[] nums, int index) {
        if ( index < 1 ) {
            return -1;
        } else if ( nums[index] > nums[index - 1] ) {
            return index - 1;
        } else {
            return findTargetIndex(nums, --index);
        }
    }

    public void swap (int[] arr, int index1, int index2) {
        int temp = arr[index1];
        arr[index1] = arr[index2];
        arr[index2] = temp;
    }
}
```
