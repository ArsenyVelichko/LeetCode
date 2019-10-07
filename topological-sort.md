# Topological sort

+ [Course Schedule](#course-schedule)
+ [Course Schedule II](#course-schedule-ii)

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
