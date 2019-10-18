# Dynamic programming

+ [Longest Increasing Subsequence](#longest-increasing-subsequence)

## Longest Increasing Subsequence

https://leetcode.com/problems/longest-increasing-subsequence/

```C++
class Solution {
public:
    int BinarySearch(int nums[], int number, int size) {
      int begin = -1;
      int end = size;
      while (begin < end - 1) {
        int mid = (end + begin) / 2;
        if (number > nums[mid])
          begin = mid;
        else if (number < nums[mid])
          end = mid;
        else
          return mid;
      }
      return end;
    }
    int lengthOfLIS(vector<int>& nums) {
      if (nums.size() == 0)
        return 0;
      int yTable[nums.size()]; 
      int currTSize = 1;
      yTable[0] = nums[0];     
      for (int i = 1; i < nums.size(); i++) {
        if (yTable[currTSize - 1] < nums[i]) {
          yTable[currTSize] = nums[i];
          currTSize++;
        } else
          yTable[BinarySearch(yTable, nums[i], currTSize)] = nums[i];         
      }
      return currTSize;
    }
};
```
