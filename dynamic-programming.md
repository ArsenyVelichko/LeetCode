# Dynamic programming

+ [Climbing Stairs](#climbing-stairs)
+ [Longest Increasing Subsequence](#longest-increasing-subsequence)
+ [Coin Change](#coin-change)
+ [Coin Change 2](#coin-change-2)
+ [House Robber](#house-robber)
+ [Jump Game](#jump-game)
+ [Jump Game II](#jump-game-ii)
+ [Decode Ways](#decode-ways)
+ [Unique Paths](#unique-paths)
+ [Unique Paths II](#unique-paths-ii)
+ [Longest Common Subsequence](#longest-common-subsequence)

## Climbing Stairs

https://leetcode.com/problems/climbing-stairs/

```C++
class Solution {
public:
    int climbStairs(int n) {
      int prev = 0, curr = 1;
      long int matrixElem1 = 0, matrixElem2 = 1;
      while (n) {
        int buf = prev;
        if (n & 1) {
          prev = matrixElem1 * prev + matrixElem2 * curr;
          curr = matrixElem2 * (buf + curr) + matrixElem1 * curr;
        }
        long int square2 = matrixElem2 * matrixElem2;
        buf = matrixElem1;
        matrixElem1 = matrixElem1 * matrixElem1 + square2;
        matrixElem2 = 2 * buf * matrixElem2 + square2;       
        n /= 2;
      }
      return curr;
    }
};
```

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

## Coin Change

https://leetcode.com/problems/coin-change/

```C++
class Solution {
public:
    int coinChange(vector<int>& coins, int amount) {
      vector<int> minCoinsN(amount + 1, amount + 1);
      minCoinsN[0] = 0;
      for (int i = 0; i < coins.size(); i++) {
        for (int j = coins[i]; j <= amount; j++) {
          minCoinsN[j] = min(minCoinsN[j], minCoinsN[j - coins[i]] + 1);
        }
      }
      return minCoinsN[amount] > amount ? -1 : minCoinsN[amount];
    }
};
```

## Coin Change 2

https://leetcode.com/problems/coin-change-2/

```C++
class Solution {
public:
    int change(int amount, vector<int>& coins) {
      vector<int> minCoinsN(amount + 1, 0);
      minCoinsN[0] = 1;
      for (int i = 0; i < coins.size(); i++) {
        for (int j = coins[i]; j <= amount; j++) {
          minCoinsN[j] += minCoinsN[j - coins[i]];
        }
      }
      return minCoinsN[amount];
    }
};
```

## House Robber

https://leetcode.com/problems/house-robber/

```C++
class Solution {
public:
    int rob(vector<int>& nums) {
      int curr = 0, prev = 0;
      for (int i = 0; i < nums.size(); i++) {
        int tmp = max(prev + nums[i], curr);
        prev = curr;
        curr = tmp;
      }
      return curr;
    }
};
```

## Jump Game

https://leetcode.com/problems/jump-game/

```C++
class Solution {
public:
    bool canJump(vector<int>& nums) {
      int lowerBound = nums.size() - 1;
      for (int i = nums.size() - 1; i >= 0; i--) {
        if (i + nums[i] >= lowerBound)
          lowerBound = i;
      }
      return lowerBound == 0;
    }
};
```

## Jump Game II

https://leetcode.com/problems/jump-game-ii/

```C++
class Solution {
public:
    int jump(vector<int>& nums) {
      int res = 0, currEnd = 0;
      int maxJump = 0;
      for (int i = 0; i < nums.size() - 1; i++) {     
        maxJump = max(maxJump, i + nums[i]);
        if (i == currEnd) {
          res++;
          currEnd = maxJump;
        }
      }
      return res;     
    }
};
```

## Decode Ways

https://leetcode.com/problems/decode-ways/

```C++
class Solution {
public:
    int numDecodings(string s) {
      int prev = 0, curr = 1;
      for (int i = 0; i < s.length(); i++) {  
        if (s[i] == '0')
          curr = 0;
        int tmp = curr + prev;        
        if (s[i] == '1' || s[i] == '2' && s[i + 1] <= '6')  
          prev = curr;         
        else
          prev = 0;
        curr = tmp;
      }
      return curr;
    }  
};
```

## Unique Paths

https://leetcode.com/problems/unique-paths/

```C++
class Solution {
public:
    int uniquePaths(int m, int n) {
      vector<vector<int>> countMap(n, vector(m, 0));
      countMap[0][0] = 1;
      for (int i = 0; i < n; i++)
        for (int j = 0; j < m; j++) {
          if (i != 0)
            countMap[i][j] += countMap[i - 1][j];
          if (j != 0)
            countMap[i][j] += countMap[i][j - 1];
        }
      return countMap[n - 1][m - 1];
    }
};
```

## Unique Paths II

https://leetcode.com/problems/unique-paths-ii/

```C++
class Solution {
public:
    int uniquePathsWithObstacles(vector<vector<int>>& obstacleGrid) {
      vector<vector<long int>> countMap(obstacleGrid.size(), vector<long int> (obstacleGrid[0].size()));
      if (obstacleGrid[0][0] == 1)
        return 0;
      obstacleGrid[0][0] = 2;
      for (int i = 0; i < countMap.size(); i++)
        for (int j = 0; j < countMap[0].size(); j++) {
          countMap[i][j] = obstacleGrid[i][j];
          if (countMap[i][j] == 0) {
            if (i != 0)
              countMap[i][j] += countMap[i - 1][j];
            if (j != 0)
              countMap[i][j] += countMap[i][j - 1];
          } else
            countMap[i][j]--;
        }
      return countMap.back().back();
    }
};
```

## Longest Common Subsequence

https://leetcode.com/problems/longest-common-subsequence/

```C++
class Solution {
public:
    int longestCommonSubsequence(string text1, string text2) {
      if (text1.size() < text2.size())
        swap(text1, text2);
      int len1 = text1.size();
      int len2 = text2.size();
      int lcs[len2 + 1] = {0};
      for (int i = 0; i < len1; i++) {
        int prev = lcs[0];
        for (int j = 1; j <= len2; j++) {
          int curr = lcs[j];
          if (text1[i] == text2[j - 1])
            lcs[j] = prev + 1;
          else
            lcs[j] = max(lcs[j], lcs[j - 1]);
          prev = curr;
        }
      }
      return lcs[len2];
    }
};
```
