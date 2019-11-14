# Strings

+ [Longest Substring Without Repeating Characters](#longest-substring-without-repeating-characters)
+ [Generate Parentheses](#generate-parentheses)
+ [Is Subsequence](#is-subsequence)
+ [Valid Palindrome](#valid-palindrome)
+ [Longest Palindromic Substring](#longest-palindromic-substring)
+ [Palindromic Substrings](#palindromic-substrings)
+ [Valid Parentheses](#valid-parentheses)
+ [Group Anagrams](#group-anagrams)

## Longest Substring Without Repeating Characters

https://leetcode.com/problems/longest-substring-without-repeating-characters/

```C++
class Solution {
 public:
  int lengthOfLongestSubstring(string s) {
    int res = 0;
    vector<int> ASCIITable(128);
    for (int i = 0, prevCut = 0; i < s.size(); i++) {
      prevCut = max(prevCut, ASCIITable[s[i]]);
      res = max(res, i - prevCut + 1);
      ASCIITable[s[i]] = i + 1;
    }
    return res;
  }
};
```

## Generate Parentheses

https://leetcode.com/problems/generate-parentheses/

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

## Is Subsequence

https://leetcode.com/problems/is-subsequence/

```C++
class Solution {
 public:
  bool isSubsequence(string s, string t) {
    int j = 0;
    for (int i = 0; t[i] && s[j]; i++)
      if (s[j] == t[i]) j++;
    if (!s[j]) return true;
    return false;
  }
};
```

## Valid Palindrome

https://leetcode.com/problems/valid-palindrome/

```C++
class Solution {
 public:
  bool isPalindrome(string s) {
    for (int i = 0, j = s.size() - 1; i < j;) {
      if (!isalnum(s[i]))
        i++;
      else if (!isalnum(s[j]))
        j--;
      else if (tolower(s[i]) != tolower(s[j]))
        return false;
      else {
        i++;
        j--;
      }
    }
    return true;
  }
};
```

## Longest Palindromic Substring

https://leetcode.com/problems/longest-palindromic-substring/

```C++
class Solution {
 public:
  string addSeparators(string s) {
    string modifiedS;
    for (int i = 0; i < s.size(); i++) {
      modifiedS.push_back('|');
      modifiedS.push_back(s[i]);
    }
    modifiedS.push_back('|');
    return modifiedS;
  }
  string removeSeparators(string s, int leftBound, int rightBound) {
    string modifiedS;
    for (int i = 0; i * 2 < rightBound - leftBound; i++)
      modifiedS.push_back(s[leftBound + i * 2]);
    return modifiedS;
  }
  string longestPalindrome(string s) {
    int c = 0, r = 0;
    int resC = 0;
    string sepS = addSeparators(s);
    int size = sepS.size();
    int P[sepS.size()] = {0};
    for (int i = 0; i < size; i++) {
      int mirror = 2 * c - i;
      if (i < r) P[i] = min(r - i, P[mirror]);
      int a = i - P[i] - 1;
      int b = i + P[i] + 1;
      while (a >= 0 && b < size && sepS[a] == sepS[b]) {
        a--;
        b++;
        P[i]++;
      }
      if (i + P[i] > r) {
        c = i;
        r = i + P[i];
        if (P[resC] < P[i]) resC = i;
      }
    }
    return removeSeparators(sepS, resC - P[resC] + 1, resC + P[resC]);
  }
};
```

## Palindromic Substrings

https://leetcode.com/problems/palindromic-substrings/

```C++
class Solution {
 public:
  string addSeparators(string s) {
    string modifiedS;
    for (int i = 0; i < s.size(); i++) {
      modifiedS.push_back('|');
      modifiedS.push_back(s[i]);
    }
    modifiedS.push_back('|');
    return modifiedS;
  }
  int countSubstrings(string s) {
    int c = 0, r = 0;
    int res = 0;
    string sepS = addSeparators(s);
    int size = sepS.size();
    int P[sepS.size()] = {0};
    for (int i = 0; i < size; i++) {
      int mirror = 2 * c - i;
      if (i < r) P[i] = min(r - i, P[mirror]);
      int a = i - P[i] - 1;
      int b = i + P[i] + 1;
      while (a >= 0 && b < size && sepS[a] == sepS[b]) {
        a--;
        b++;
        P[i]++;
      }
      if (i + P[i] > r) {
        c = i;
        r = i + P[i];
      }
      res += (P[i] + 1) / 2;
    }
    return res;
  }
};
```


## Valid Parentheses

https://leetcode.com/problems/valid-parentheses/

```C++
class Solution {
 public:
  bool isValid(string s) {
    stack<int> parOrder;
    for (int i = 0; i < s.size(); i++) {
      switch (s[i]) {
        case '(':
          parOrder.push(')');
          break;
        case '[':
          parOrder.push(']');
          break;
        case '{':
          parOrder.push('}');
          break;
        default:
          if (parOrder.empty() || parOrder.top() != s[i]) return false;
          parOrder.pop();
      }
    }
    return parOrder.empty();
  }
};
```

## Group Anagrams

https://leetcode.com/problems/group-anagrams/

```C++
class Solution {
 public:
  vector<vector<string>> groupAnagrams(vector<string>& strs) {
    unordered_map<string, vector<string>> anagramMap;
    int alphabet[26] = {0};
    for (string currStr : strs) {
      for (char currChar : currStr) {
        alphabet[currChar - 'a']++;
      }
      string key;
      for (int i = 0; i < 26; i++) {
        key.push_back('#');
        key.push_back(alphabet[i]);
        alphabet[i] = 0;
      }
      anagramMap[key].push_back(currStr);
    }
    vector<vector<string>> res;
    for (auto mapElem : anagramMap) {
      res.push_back(mapElem.second);
    }
    return res;
  }
};
  ```
