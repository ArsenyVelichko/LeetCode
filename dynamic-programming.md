# Dynamic programming

+ [Longest Increasing Subsequence](#longest-increasing-subsequence)
+ [Coin Change](#coin-change)
+ [Coin Change 2](#coin-change-2)
+ [Climbing Stairs](#climbing-stairs)

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
