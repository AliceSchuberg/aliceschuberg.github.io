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


To look for the left, the idea is to make “right" zoom into left side when it’s >= target, at the end of the day, it should ideally end up at the right spot of the first position where the target appears. In a case the element doesn’t exist, then:
1. if the target value is within ```nums[left] < target < nums[right]``` but it doesn't, this case will be handled by return clause;
2. if the target value is target <nums[left], which means right will hit all the way to the left at index 0, it will exit while loop with left == right == 0, still this case will be handled by return clause as nums[0] is > and != target
3. if the target value is target > nums[right], which means left will hit all the way to right at index nums.length - 1, it will exit the while loop with left == right == nums.length - 1, still this case will be handled by return clause as nums[nums.length - 1] < and != target.

To look for the right, it becomes a bit more trickier because we lose the benefit of the int calc described above, so we cannot reverse the findLeft to make left zoom all the way into right side (at least it’s hard to do so). First of all, let’s look into the details of while loop first (ignore the while loop condition for now), how do we try to locate the last (right-most) index of the target? Certainly we cannot use nums[mid] >= target because it will make the right zoom to the first index of the target, then it’s make it nums[mid] > target, then we guarantee that mid is at least 1 position greater than the last index of target. Then the method will look like this to return the last index of the target, it will work…on normal cases… 