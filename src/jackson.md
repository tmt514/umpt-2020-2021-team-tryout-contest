# J: Jackson

## Solution Sketch

This can be solved in a straight forward dynamic programming algorithm.

## Sample Code (C++)

```c++
#include <bits/stdc++.h>
using namespace std;

int main() {
  string s;
  cin >> s;
  int n = s.size();
  string t = "michigan";
  vector<int> dp(9, 0);
  const long long MOD = 1e9 + 7;
  dp[0] = 1;
  for (int i = 0, u = 0; i < n; i++) {
    for (int j = 7; j >= 0; j--) {
      dp[j + 1] = (dp[j + 1] + (s[i] == t[j] ? dp[j] : 0));
      if (dp[j + 1] >= MOD) dp[j + 1] -= MOD;
    }
  }
  cout << dp[8] << endl;
  return 0;
}
```