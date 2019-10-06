# Intervals

+ [Non-overlapping Intervals](#non-overlapping-intervals)
+ [Merge Intervals](#merge-intervals)
+ [Insert Interval](#insert-interval)

## Non-overlapping Intervals

```C++
bool CompareIntervalBegins(vector<int>& a, vector<int>& b) {
       return (a[0] < b[0]);
}
class Solution {
public:    
    int eraseOverlapIntervals(vector<vector<int>>& intervals) {
        if (intervals.size() == 0)
          return intervals.size();
        sort(intervals.begin(), intervals.end(), CompareIntervalBegins);
        int count = 0;
        int currentEnd = intervals[0][1];
        for (int i = 1; i < intervals.size(); i++) {
          if (currentEnd > intervals[i][0]) {
            count++;
            if (intervals[i][1] < currentEnd)
              currentEnd = intervals[i][1];
          } else
            currentEnd = intervals[i][1];
        }
      return count;
    }
};
```

## Merge Intervals

https://leetcode.com/problems/merge-intervals/

```C++
bool CompareIntervalBegins(vector<int>& a, vector<int>& b) {
       return (a[0] < b[0]);
}
class Solution {
public:
    vector<vector<int>> merge(vector<vector<int>>& intervals) {
        if (intervals.size() == 0)
          return intervals;
        sort(intervals.begin(), intervals.end(), CompareIntervalBegins);
        int currentEnd = intervals[0][1];
        int beginIndex = 0;
        int i = 1;
        while(i < intervals.size()) {        
          if (currentEnd >= intervals[i][0]) { 
            if (intervals[i][1] > currentEnd)
              currentEnd = intervals[i][1];
            intervals.erase(intervals.begin() + i);
          } else {
            intervals[beginIndex][1] = currentEnd;
            currentEnd = intervals[i][1];
            beginIndex = i;
            i++;
          }
        }
      intervals[beginIndex][1] = currentEnd;
      return intervals;
    }
};
```

## Insert Interval

https://leetcode.com/problems/insert-interval/

```C++
class Solution {
public:
    int BinarySearch(vector<vector<int>>& intervals, int num, int index){
      int start = -1;
      int end = intervals.size();
      while (start < end - 1) {
        int mid = (start + end) / 2;        
        if (intervals[mid][index] < num)
          start = mid;
        else if (intervals[mid][index] > num)
          end = mid;
        else
          return mid + !index;
      }
      return end;
    }
    vector<vector<int>> insert(vector<vector<int>>& intervals, vector<int>& newInterval) {
        vector<vector<int>>::iterator beginIter = intervals.begin();
        int beginToErase = BinarySearch(intervals, newInterval[0], 1);
        int endToErase = BinarySearch(intervals, newInterval[1], 0);
        if (beginToErase == intervals.size()) {
          intervals.push_back(newInterval);
          return intervals;
        } else if (endToErase == 0) {
          intervals.insert(beginIter, newInterval);
          return intervals;
        }
        if (intervals[beginToErase][0] < newInterval[0])
          newInterval[0] = intervals[beginToErase][0];
        if (intervals[endToErase - 1][1] > newInterval[1])
          newInterval[1] = intervals[endToErase - 1][1];
        intervals.erase(beginIter + beginToErase, beginIter + endToErase);
        intervals.insert(beginIter + beginToErase, newInterval);
        return intervals;
    }
};
```
