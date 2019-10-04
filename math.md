## Fibonacci Number

https://leetcode.com/problems/fibonacci-number/

### Iteratively
```C++ 
class Solution {
public:
    int fib(int N) {
        int first = 0, second = 1;
        if (N <= 0)
          return N;
        while (N-- > 1) {
          int tmp = first;
          first = second;
          second += tmp;
        }
        return second;
    }
};
```
