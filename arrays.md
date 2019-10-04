# Arrays

+ [Two Sum](#two-sum)
+ [3Sum](#3sum)

## Two Sum

https://leetcode.com/problems/two-sum/

```C++
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
      map<int, int> hashTable;
      map<int, int>::iterator ptr;
      for (int i = 0; i < nums.size(); i++) {
        int secondNum = target - nums[i];
        if ((ptr = hashTable.find(secondNum)) != hashTable.end())
          return vector<int> {ptr->second, i};
        hashTable.insert(pair<int, int>(nums[i], i));
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
