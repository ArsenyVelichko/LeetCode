# Arrays

+ [Two Sum](#two-sum)
+ [3Sum](#3sum)
+ [Subarray Sum Equals K](#subarray-sum-equals-k)

## Two Sum

https://leetcode.com/problems/two-sum/

```C++
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
      map<int, int> numberMap;
      map<int, int>::iterator searchIter;
      for (int i = 0; i < nums.size(); i++) {
        int secondNum = target - nums[i];
        if ((searchIter = numberMap.find(secondNum)) != numberMap.end())
          return vector<int> {searchIter->second, i};
        numberMap.insert(pair<int, int>(nums[i], i));
      }
      return vector<int> (0);
    }
};
```

## 3Sum

https://leetcode.com/problems/3sum/

```C++
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
      vector<vector<int>> res;
      sort(nums.begin(), nums.end());
      int size = nums.size();
      for (int i = 0; i < size - 2; i++) {    
        int begin = i + 1;
        int end = size - 1;
        while (end > begin) {      
          int sum = nums[i] + nums[begin] + nums[end];
          if (sum == 0) {
            res.push_back(vector<int> {nums[i], nums[begin], nums[end]});
            do {
              end--;
              begin++;
            } while (end > begin &&
                     nums[begin] == nums[begin - 1] &&
                     nums[end] == nums[end + 1]);
          } else if (sum > 0)
            end--;
          else
            begin++;
        }
        while (i < size - 2 && nums[i] == nums[i + 1])
          i++;
      }
      return res;
    }
};
```

## Subarray Sum Equals K

https://leetcode.com/problems/subarray-sum-equals-k/

```C++
class Solution {
public:
    int subarraySum(vector<int>& nums, int k) {
      map<int, int> subArrMap;
      map<int, int>::iterator searchIter;
      int count = 0;
      int currentSum = 0;
      subArrMap.insert(pair<int, int>(0, 1));
      for (int i = 0; i < nums.size(); i++) {
        currentSum += nums[i];
        int requiredSum = currentSum - k;
        if ((searchIter = subArrMap.find(requiredSum)) != subArrMap.end())
          count += searchIter->second;
        if ((searchIter = subArrMap.find(currentSum)) != subArrMap.end())
          searchIter->second++;
        else
          subArrMap.insert(pair<int, int>(currentSum, 1));
      }
      return count;
    }
};
```
