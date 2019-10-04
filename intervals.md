# Intervals

+ [Non-overlapping Intervals](#non-overlapping-intervals)

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
