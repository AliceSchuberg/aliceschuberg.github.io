---
title: 154. Find Minimum in Rotated Sorted Array II
date: 2022-10-12 02:04:17
tags:
---

```java
class Solution {
    public int findMin(int[] nums) {
        int left = 0;
        int right = nums.length - 1;
        int mid;
        while (left < right) {
            mid = left + (right-left)/2;
            if (nums[mid] > nums[right]) {
                left = mid + 1;
            } else if (nums[mid] < nums[left]) {
                right = mid;
            } else {
                right--;
            }
        }
        return nums[left];
    }
}
```

### Ideas:
````
1. Consider two extreme cases, [0,0,0,0,0,0,1,0] and [0,1,0,0,0,0,0,0]
2. We will not know if min/starting point is on the left or right side
3. In both cases, we need right to decrease in order to truncate the right part of the array
    a. why not increase from left side
       -> the condition imply one case that will fall into the else clause
       -> nums[left] <= nums[left] <= nums[right] which is sorted array
       then it needs to consider this case as well and decrease right would be safe option to do
