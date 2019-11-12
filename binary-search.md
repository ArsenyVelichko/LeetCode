# Binary search

+ [Binary Search](#binary-search)
+ [Find Minimum in Rotated Sorted Array](#find-minimum-in-rotated-sorted-array)
+ [Search in Rotated Sorted Array](#search-in-rotated-sorted-array)

## Binary Search

https://leetcode.com/problems/binary-search/

```C++
class Solution {
 public:
  int search(vector<int>& nums, int target) {
    int left = -1;
    int right = nums.size();
    while (left < right - 1) {
      int mid = (left + right) / 2;
      if (nums[mid] < target)
        left = mid;
      else if (nums[mid] > target)
        right = mid;
      else
        return mid;
    }
    return -1;
  }
};
```

## Find Minimum in Rotated Sorted Array

https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/

```C++
class Solution {
 public:
  int findMin(vector<int>& nums) {
    int left = -1;
    int right = nums.size() - 1;
    while (left < right - 1) {
      int mid = (left + right) / 2;
      if (nums[mid] < nums[right])
        right = mid;
      else
        left = mid;
    }
    return nums[right];
  }
};
```

## Search in Rotated Sorted Array

https://leetcode.com/problems/search-in-rotated-sorted-array/

```C++
class Solution {
 public:
  int findMin(vector<int>& nums) {
    int left = -1;
    int right = nums.size() - 1;
    while (left < right - 1) {
      int mid = (left + right) / 2;
      if (nums[mid] < nums[right])
        right = mid;
      else
        left = mid;
    }
    return right;
  }
  int binSearch(vector<int>& nums, int target, int lowerBound, int upperBound) {
    int left = lowerBound - 1;
    int right = upperBound;
    while (left < right - 1) {
      int mid = (left + right) / 2;
      if (nums[mid] < target)
        left = mid;
      else if (nums[mid] > target)
        right = mid;
      else
        return mid;
    }
    return -1;
  }
  int search(vector<int>& nums, int target) {
    if (nums.size() == 0) return -1;
    int minPos = findMin(nums);
    if (nums.back() < target)
      return binSearch(nums, target, 0, minPos);
    else
      return binSearch(nums, target, minPos, nums.size());
  }
};
```
