---
title: 34. Find First and Last Position of Element in Sorted Array (Original)
date: 2022-10-19 01:30:34
tags:
---
## Keys:
1. There are three types of cases: empty array, 1-element array, normal array with 2+ element
2. Try to look into the details of while loop first and see what it needs to do, then consider the while loop condition which decides what to do in the last round
3. The trick in the Int calc in Java is that mid will never be equal to right unless there is only 1 element left (which is left == right)
   1. i.e. in odd cases, mid is right at the middle (1,2,3) -> 2
   2. in even cases, mid is always gonna be the one closer to left (because decimal is truncated in Int calc) 
      1. ```(0,1,2,3) -> mid = 2.5 -> int mid = 2 -> nums[mid] = 1```
4. In either find left or find right, “right” will never goes beyond the last element of the array. (it either stays there or decrease to the left), so we don’t need to worry about it goes beyond array length.
   1. i.e. right <= nums.length

### Left Side
To look for the left, the idea is to make “right" zoom into left side when it’s >= target, at the end of the day, it should ideally end up at the right spot of the first position where the target appears. In a case the element doesn’t exist, then:
1. if the target value is within ```nums[left] < target < nums[right]``` but it doesn't, this case will be handled by return clause;
2. if the target value is target <nums[left], which means right will hit all the way to the left at index 0, it will exit while loop with left == right == 0, still this case will be handled by return clause as nums[0] is > and != target
3. if the target value is target > nums[right], which means left will hit all the way to right at index nums.length - 1, it will exit the while loop with left == right == nums.length - 1, still this case will be handled by return clause as nums[nums.length - 1] < and != target.


### Right Side
To look for the right, it becomes a bit more tricky because we lose the benefit of the int calc described above, so we cannot reverse the findLeft to make left zoom all the way into right side (at least it’s hard to do so). First of all, let’s look into the details of while loop first (ignore the while loop condition for now), how do we try to locate the last (right-most) index of the target? Certainly we cannot use nums[mid] >= target because it will make the right zoom to the first index of the target, then it’s make it nums[mid] > target, then we guarantee that mid is at least 1 position greater than the last index of target. Then the method will look like this to return the last index of the target, it will work…on normal cases…

````
public int findRight(int[] nums, int target) {
    int left = 0;
    int right = nums.length - 1;
    int mid;

    while (left < right) {
        mid = left + (right-left)/2;
        if (nums[mid] > target) {
            right = mid;
        } else {
            left = mid + 1;
        }
    }
    return (right >= 1 && nums[right-1] == target) ? right-1 : -1;  
}
````

#### Problems
But the issue with this is it doesn’t take care of the 1-element array issue, in which left == right == 0, the right-1 will trigger index out of array exception. So we need to adjust the method. 

Then most apparently, we need to update the while loop condition to trigger at least one time of the left/right index updates. So the condition becomes left<= right. 

Then it hit another issue of infinite loop because right = mid will run forever when ( [1] -> 0) left == right == mid && nums[right] > target. Anyway, when while loop condition becomes <=, you should be always be careful of using right = mid or left = mid; 

Then the only thing we can do is update right = mid - 1, which happens to be (ideally) the last index of target in normal cases. Then update return clause to rule out the edge cases:
1. right needs to be  >=  0 to avoid nums[-1] and empty array
2. check nums[right] == target -> this step avoids target not in the array


### Solution

````java
class Solution {
    public int[] searchRange(int[] nums, int target) {
        int left = findLeft(nums, target);
        int right = findRight(nums, target);
        return new int[]{left,right};
    }

    public int findLeft(int[] nums, int target) {
        int left = 0;
        int right = nums.length - 1;
        int mid;

        while (left < right) {
            mid = left + (right-left)/2;
            if (nums[mid] >= target) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        return (right >= 0 && nums[right] == target) ? right: -1;
    }

    public int findRight(int[] nums, int target) {
        int left = 0;
        int right = nums.length - 1;
        int mid;

        while (left <= right) {
            mid = left + (right-left)/2;
            if (nums[mid] > target) {
                right = mid - 1;
            } else {
                left = mid + 1;
            }
        }
        return (right >= 0 && nums[right] == target) ? right : -1;
    }
}
````

## Updated Thoughts:
1. Still, always be aware 3 edge cases.
2. the idea for the left is basically the same, make right index always zoom to left when it's still >= target

### Difference between them:
1. For the beginning index of right:
   1. For find left, because theoretically we want `nums[right]` sits at the beginning index of target at the end of the loop, so nums.length - 1 is a valid place to begin with
   2. For find right, actually we are looking for an element that is 1-position greater the last element of target,
      so nums.length is a better place to start with because `nums[nums.length-1]` could be a valid target

````java
class Solution {
    public int[] searchRange(int[] nums, int target) {
        int left = findLeft(nums,target);
        int right = findRight(nums, target);
        return new int[]{left,right};
    }
    
    public int findLeft(int[] nums, int target) {
        int left = 0;
        int right = nums.length - 1;
        int mid;
        while (left < right) {
            mid = left + (right-left)/2;
            if (nums[mid] >= target) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        return (right >= 0 && nums[right] == target) ? right : -1;
    }
    
    public int findRight(int[] nums, int target) {
        int left = 0;
        int right = nums.length;
        int mid;
        while (left < right) {
            mid = left + (right-left)/2;
            if (nums[mid] > target) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        right--;
        return (right >= 0 && nums[right] == target) ? right : -1;
    }
}
````


