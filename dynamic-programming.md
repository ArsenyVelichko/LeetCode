# Dynamic programming

+ [Knapsack problem](#knapsack-problem)
+ [Climbing Stairs](#climbing-stairs)
+ [Longest Increasing Subsequence](#longest-increasing-subsequence)
+ [Coin Change](#coin-change)
+ [Coin Change 2](#coin-change-2)
+ [House Robber](#house-robber)
+ [House Robber II](#house-robber-ii)
+ [Jump Game](#jump-game)
+ [Jump Game II](#jump-game-ii)
+ [Decode Ways](#decode-ways)
+ [Unique Paths](#unique-paths)
+ [Unique Paths II](#unique-paths-ii)
+ [Longest Common Subsequence](#longest-common-subsequence)
+ [Word Break](#word-break)
+ [N-Queens](#n-queens)
+ [N-Queens II](#n-queens-ii)

## Knapsack problem

https://stepik.org/lesson/13259/step/5?unit=3444

```C++
#include <iostream>
#include <vector>
using namespace std;
int main() {
  int W, n;
  cin >> W >> n;
  vector<vector<int>> maxValue(n + 1, vector<int>(W + 1));
  for (int i = 1; i <= n; i++) {
    int num;
    cin >> num;
    for (int j = 1; j <= W; j++) {
      if (num > j)
        maxValue[i][j] = maxValue[i - 1][j];
      else
        maxValue[i][j] = max(maxValue[i - 1][j], maxValue[i - 1][j - num] + num);
    }
  }
  cout << maxValue[n][W];
  return 0;
}
```

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

## House Robber II

https://leetcode.com/problems/house-robber-ii/

```C++
class Solution {
public:
    int MaxLoot(vector<int>& nums, int begin, int end) {
      int curr = 0, prev = 0;
      for (int i = begin; i < end; i++) {
        int tmp = max(prev + nums[i], curr);
        prev = curr;
        curr = tmp;
      }
      return curr;
    }
    int rob(vector<int>& nums) {
      if (nums.size() == 1)
        return nums[0];
      return max(MaxLoot(nums, 0, nums.size() - 1), MaxLoot(nums, 1, nums.size()));
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
      vector<int> countMap(m, 1);
      for (int i = 1; i < n; i++)
        for (int j = 1; j < m; j++)
          countMap[j] += countMap[j - 1];       
      return countMap[m - 1];
    }
};
```

## Unique Paths II

https://leetcode.com/problems/unique-paths-ii/

```C++
class Solution {
public:
    int uniquePathsWithObstacles(vector<vector<int>>& obstacleGrid) {
      if (obstacleGrid[0][0] == 1)
        return 0;
      int height = obstacleGrid.size();
      int width = obstacleGrid[0].size();
      vector<long> countMap(width);
      countMap[0] = 1;
      for (int i = 0 ; i < height; i++)
        for (int j = 0 ; j < width; j++) {
          if (!obstacleGrid[i][j]) {
            if (j != 0)
              countMap[j] += countMap[j - 1];
          } else
            countMap[j] = 0;
        }
        return countMap[width - 1];
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

## Word Break

https://leetcode.com/problems/word-break/

```C++
class Solution {
  vector<vector<int>> graph;
  vector<int> memo;
public: 
    bool SearchBottom(int depth) {
      if (depth == 0)
        return true;
      memo[depth] = 1;
      for (int i = 0; i < graph[depth].size(); i++) {
        int next = graph[depth][i];
        if (memo[next] != 0)
          continue;
        if (SearchBottom(next))
          return true;
      }
      return false;
    }
    bool wordBreak(string s, vector<string>& wordDict) {
      graph.resize(s.size() + 1, vector<int>());
      memo.resize(s.size() + 1, 0);
       for (int i = 0; i < wordDict.size(); i++) {
         for (int j = 0; j < s.size(); j++) {
           int k;
           for (k = 0; k < wordDict[i].size(); k++)
             if (wordDict[i][k] != s[k + j])
               break;
           if (k == wordDict[i].size())
             graph[j + k].push_back(j);
         }
       }
       return SearchBottom(s.size());
    }
};
```

## N-Queens

https://leetcode.com/problems/n-queens/

```C++
class Solution {
private:
    vector<vector<string>> res;
    vector<char> markedRow;
    vector<char> markedRDiag;
    vector<char> markedLDiag;
public:
    void SearchSolution(int column, int n, vector<string>& currRes) {
      if (column == n)
        res.push_back(currRes);
      else {
        for (int row = 0; row < n; row++) {
          if (!markedRow[row] && !markedRDiag[row + column]
             && !markedLDiag[column - row + n - 1]) {
            currRes[row][column] = 'Q';
            markedRow[row] = markedRDiag[row + column]
              = markedLDiag[column - row + n - 1] = 1;
            SearchSolution(column + 1, n, currRes);
            currRes[row][column] = '.';
            markedRow[row] = markedRDiag[row + column]
              = markedLDiag[column - row + n - 1] = 0;
          }
        }
      }
    }
    vector<vector<string>> solveNQueens(int n) {
      vector<string> currRes(n, string (n, '.'));
      markedRow.resize(n, 0);
      markedRDiag.resize(2 * n - 1, 0);
      markedLDiag.resize(2 * n - 1, 0);
      SearchSolution(0, n, currRes);
      return res;
    }
};
```

## N-Queens II

https://leetcode.com/problems/n-queens-ii/

```C++
class Solution {
private:
    int res;
    vector<char> markedRow;
    vector<char> markedRDiag;
    vector<char> markedLDiag;
public:
    void SearchSolution(int column, int n, vector<string>& currRes) {
      if (column == n)
        res++;
      else {
        for (int row = 0; row < n; row++) {
          if (!markedRow[row] && !markedRDiag[row + column]
             && !markedLDiag[column - row + n - 1]) {
            currRes[row][column] = 'Q';
            markedRow[row] = markedRDiag[row + column]
              = markedLDiag[column - row + n - 1] = 1;
            SearchSolution(column + 1, n, currRes);
            currRes[row][column] = '.';
            markedRow[row] = markedRDiag[row + column]
              = markedLDiag[column - row + n - 1] = 0;
          }
        }
      }
    }
    int totalNQueens(int n) {
      vector<string> currRes(n, string (n, '.'));
      markedRow.resize(n, 0);
      markedRDiag.resize(2 * n - 1, 0);
      markedLDiag.resize(2 * n - 1, 0);
      SearchSolution(0, n, currRes);
      return res;
    }
};
```
