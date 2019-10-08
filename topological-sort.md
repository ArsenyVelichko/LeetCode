# Topological sort

+ [Course Schedule](#course-schedule)
+ [Course Schedule II](#course-schedule-ii)
+ [Alien Dictionary](#alien-dictionary)

## Course Schedule

https://leetcode.com/problems/course-schedule/

```C++
class Solution {
private:
  vector<vector<int>> graph;
  vector<int> indegree;
public:
    bool DeepFirstSearch(int top) {
      indegree[top] = 1;
      for (int i = 0; i < graph[top].size(); i++) {
        int next = graph[top][i];
        if (indegree[next] == 0) {
          if (DeepFirstSearch(next))
            return true;
        } else if (indegree[next] == 1)
          return true;    
      }
      indegree[top] = 2;
      return false;
    }
    bool canFinish(int numCourses, vector<vector<int>>& prerequisites) {
        graph.resize(numCourses, vector<int>());
        indegree.resize(numCourses, 0);
        int i;
        for (i = 0; i < prerequisites.size(); i++)
          graph[prerequisites[i][0]].push_back(prerequisites[i][1]);
        for (i = 0; i < numCourses; i++)
          if (indegree[i] == 0)
            if (DeepFirstSearch(i))
              return false;
        return true;
    }
};
```

## Course Schedule II

https://leetcode.com/problems/course-schedule-ii/submissions/

```C++
class Solution {
private:
  vector<vector<int>> graph;
  vector<int> indegree;
  vector<int> orderStack;
public:
    bool DeepFirstSearch(int top) {
      indegree[top] = 1;
      for (int i = 0; i < graph[top].size(); i++) {
        int next = graph[top][i];
        if (indegree[next] == 0) {
          if (DeepFirstSearch(next))
            return true;
        } else if (indegree[next] == 1)
          return true;    
      }
      orderStack.push_back(top);
      indegree[top] = 2;
      return false;
    }
    vector<int> findOrder(int numCourses, vector<vector<int>>& prerequisites) {
        graph.resize(numCourses, vector<int>());
        indegree.resize(numCourses, 0);
        int i;
        for (i = 0; i < prerequisites.size(); i++)
          graph[prerequisites[i][0]].push_back(prerequisites[i][1]);
        for (i = 0; i < numCourses; i++)
          if (indegree[i] == 0)
            if (DeepFirstSearch(i))
              return vector<int>(0);
        return orderStack;
    }
};
```

## Alien Dictionary

https://www.lintcode.com/problem/alien-dictionary/description

```C++
class Solution {
private:
    vector<vector<int>> graph;
    vector<int> indegree;
    string orderStack;
public:
    bool DeepFirstSearch(int top) {
      indegree[top] = 1;
      for (int i = 0; i < graph[top].size(); i++) {
        int next = graph[top][i];
        if (indegree[next] == 0) {
          if (DeepFirstSearch(next))
            return true;
        } else if (indegree[next] == 1)
          return true;    
      }
      orderStack.push_back(top + 'a');
      indegree[top] = 2;
      return false;
    }
    void BuidGraph(vector<string>& words) {
        int i, j;
        bool isEdgeBuilt;
        for (i = 0; i < words.size(); i++) {
            isEdgeBuilt = (i == words.size() - 1);
            for (j = 0; j < words[i].size(); j++) {
              indegree[words[i][j] - 'a'] = 0;
              if (!isEdgeBuilt && words[i][j] != words[i + 1][j]) {
                graph[words[i][j] - 'a'].push_back(words[i + 1][j] - 'a');
                isEdgeBuilt = true;
              }
            }
        }
    }
    string ReverseStr(string str) {
        for (int i = 0, j = str.size() - 1; i < j; i++, j--) {
            char buf = str[i];
            str[i] = str[j];
            str[j] = buf;
        }
        return str;
    }
    int BinarySearch(string str, char a) {
        int begin = -1;
        int end = str.size();
        while (begin < end - 1) {
            int mid = (end + begin) / 2;
            if (str[mid] < a)
              begin = mid;
            else
              end = mid;
        }
        return end;
    }
    string alienOrder(vector<string> &words) {
        graph.resize(26, vector<int>());
        indegree.resize(26, -1);
        int i;
        BuidGraph(words);
        for (i = 0; i < 26; i++)
          if (graph[i].size() && indegree[i] == 0)
            if (DeepFirstSearch(i))
              return string();
        orderStack = ReverseStr(orderStack);
        for (i = 0; i < 26; i++)
          if (indegree[i] == 0) {
             size_t index = BinarySearch(orderStack, i + 'a');
             orderStack.insert(index + orderStack.begin(), i + 'a');
          }
        return orderStack;
    }
};
```
