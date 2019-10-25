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
  vector<int> topStatuses;
public:
    bool DeepFirstSearch(int top) {
      topStatuses[top] = 1;
      for (int i = 0; i < graph[top].size(); i++) {
        int next = graph[top][i];
        if (topStatuses[next] == 0) {
          if (DeepFirstSearch(next))
            return true;
        } else if (topStatuses[next] == 1)
          return true;    
      }
      topStatuses[top] = 2;
      return false;
    }
    bool canFinish(int numCourses, vector<vector<int>>& prerequisites) {
        graph.resize(numCourses, vector<int>());
        topStatuses.resize(numCourses, 0);
        int i;
        for (i = 0; i < prerequisites.size(); i++)
          graph[prerequisites[i][0]].push_back(prerequisites[i][1]);
        for (i = 0; i < numCourses; i++)
          if (topStatuses[i] == 0)
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
  vector<int> topStatuses;
  vector<int> orderArr;
public:
    bool DeepFirstSearch(int top) {
      topStatuses[top] = 1;
      for (int i = 0; i < graph[top].size(); i++) {
        int next = graph[top][i];
        if (topStatuses[next] == 0) {
          if (DeepFirstSearch(next))
            return true;
        } else if (topStatuses[next] == 1)
          return true;    
      }
      orderArr.push_back(top);
      topStatuses[top] = 2;
      return false;
    }
    vector<int> findOrder(int numCourses, vector<vector<int>>& prerequisites) {
        graph.resize(numCourses, vector<int>());
        topStatuses.resize(numCourses, 0);
        int i;
        for (i = 0; i < prerequisites.size(); i++)
          graph[prerequisites[i][0]].push_back(prerequisites[i][1]);
        for (i = 0; i < numCourses; i++)
          if (topStatuses[i] == 0)
            if (DeepFirstSearch(i))
              return vector<int>(0);
        return orderArr;
    }
};
```

## Alien Dictionary

https://www.lintcode.com/problem/alien-dictionary/description

```C++
class Solution {
private:
    vector<vector<int>> graph;
    vector<int> topStatuses;
    string orderStr;
public:
    bool DeepFirstSearch(int top) {
      topStatuses[top] = 1;
      for (int i = 0; i < graph[top].size(); i++) {
        int next = graph[top][i];
        if (topStatuses[next] == 0) {
          if (DeepFirstSearch(next))
            return true;
        } else if (topStatuses[next] == 1)
          return true;    
      }
      orderStr.push_back(top + 'a');
      topStatuses[top] = 2;
      return false;
    }
    void BuidGraph(vector<string>& words) {
        int i, j;
        bool isEdgeBuilt;
        for (i = 0; i < words.size(); i++) {
            isEdgeBuilt = (i == words.size() - 1);
            for (j = 0; j < words[i].size(); j++) {
              topStatuses[words[i][j] - 'a'] = 0;
              if (!isEdgeBuilt && words[i][j] != words[i + 1][j]) {
                graph[words[i + 1][j] - 'a'].push_back(words[i][j] - 'a');
                isEdgeBuilt = true;
              }
            }
        }
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
        topStatuses.resize(26, -1);
        int i;
        BuidGraph(words);
        for (i = 0; i < 26; i++)
          if (graph[i].size() && topStatuses[i] == 0)
            if (DeepFirstSearch(i))
              return string();
        for (i = 0; i < 26; i++)
          if (topStatuses[i] == 0) {
             size_t index = BinarySearch(orderStr, i + 'a');
             orderStr.insert(index + orderStr.begin(), i + 'a');
          }
        return orderStr;
    }
};
```
