# Arrays

+ [Two Sum](#two-sum)

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
