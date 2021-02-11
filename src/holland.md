# H: Holland
## Solution Sketch

## Sample Code (C++)

```c++
#include <bits/stdc++.h>
using namespace std;
 
const int MAXN = 1005;
int main() {
  int N, K, S;
  vector<pair<int, int>> a;
  cin >> N >> K >> S;
  for(int i=0;i<N;i++) {
    int ai, ti;
    cin >> ai >> ti;
    a.push_back({ai, ti});
  }
  
  sort(a.begin(), a.end());
  vector<int> dp(N+1, 0);
  for(int i=0;i<N;i++) dp[i] = a[i].second;
 
  for(int i=N-1;i>=0;i--) {
    dp[i] = max(dp[i], dp[i+1]);
    priority_queue<pair<int, int>> q;
 
    // invariant: slot[i] <= K + i
    vector<int> slot(N, 0);
    slot[0]++;
 
    int now = a[i].second;
    int lastfull = -1;
 
    int j = i+1;
    for (int m = 1; j < N || !q.empty(); m++) {
      slot[m] = slot[m-1];
      while (j < N && a[j].first < a[i].first + S * m) {
        q.push({a[j].second, a[j].first});
        j++;
      }
      dp[i] = max(dp[i], now + dp[j]);
      int success = 0;
      while (!q.empty()) {
        int tip = q.top().first;
        int arrival = q.top().second;
        int arrival_slot = (arrival - a[i].first) / S;
        q.pop();
        // OK to put choose this element.
        if (arrival_slot > lastfull) {
          // update tip
          now += tip;
 
          // update last full
          for(int w = arrival_slot; w <= m; w++) {
            slot[w]++;
            if (slot[w] >= K+w) lastfull = w;
          }
          success = 1;
          break;
        }
      }
      if (success == 0) break;
    }
 
    dp[i] = max(dp[i], now + dp[j]);
  }
 
  cout << dp[0] << endl;
  return 0;
}
```