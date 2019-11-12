# Strings

1) [Generate Parentheses](#generate-parentheses)

## Generate Parentheses
```C++
class Solution {
 public:
  vector<string> generateParenthesis(int n) {
    vector<string> res;
    if (n == 0) {
      res.push_back("");
    } else {
      for (int i = 0; i < n; i++)
        for (string left : generateParenthesis(i))
          for (string right : generateParenthesis(n - 1 - i))
            res.push_back("(" + left + ")" + right);
    }
    return res;
  }
};
```
