# Math

+ [Fibonacci Number](#fibonacci-number)
+ [Count Primes](#count-primes)

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

### Recursively

```C++
class Solution {
public:
    int fib(int N) {
        if (N <= 1)
          return N;
        return fib(N - 2) + fib(N - 1);
    }
};
```

### Recursively with memoization

```C++
class Solution {
private:
  vector<int> cache;
public:
    int SearchFibNumber(int N) {
      if (N <= 1)
        return N;
      if (cache[N] != 0)
        return cache[N];      
      return cache[N] = SearchFibNumber(N - 1) + SearchFibNumber(N - 2);  
    }
    int fib(int N) {
      cache.resize(N + 1, 0);
      return SearchFibNumber(N);
    }
};
```

## Count Primes

https://leetcode.com/problems/count-primes/

```C++
class Solution {
 public:
  int countPrimes(int n) {
    vector<int> isPrime(n, 1);
    int res = 0;
    int i;
    for (i = 2; i * i < n; i++) {
      if (!isPrime[i]) continue;
      res++;
      for (int j = i * i; j < n; j += i) isPrime[j] = 0;
    }
    for (int j = i; j < n; j++)
      if (isPrime[j]) res++;
    return res;
  }
};
```
