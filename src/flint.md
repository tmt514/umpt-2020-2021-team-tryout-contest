# F: Flint
## Solution Sketch

## Sample Code (C++)

```c++
#include <bits/stdc++.h>
using namespace std;

const int MOD = 1e9 + 7;
int main() {
  int n;
  int a[105];
  long long two[105];
  map<int, int> cnt;
  map<int, int> mark;
  cin >> n;
  two[0] = 1;
  for (int i = 1; i <= n; i++) two[i] = two[i - 1] * 2 % MOD;
  for (int i = 0; i < n; i++) cin >> a[i];
  for (int i = 0; i < n; i++) {
    int x = a[i];
    vector<int> pf;
    for (int p = 2; p * p <= x; p++) {
      if (x % p == 0) {
        pf.push_back(p);
        while (x % p == 0) x /= p;
      }
    }

    if (x > 1) pf.push_back(x);
    for (int state = 0; state < (1 << pf.size()); state++) {
      int g = 1, cc = 0;
      for (int j = 0; j < pf.size(); j++) {
        if ((1 << j) & state) {
          ++cc;
          g *= pf[j];
        }
      }
      cnt[g]++;
      mark[g] = cc % 2;
    }
  }

  long long ans = 0;
  for (auto it : cnt) {
    int g = it.first, c = it.second;
    if (mark[g] == 0) {
      ans += two[c] - 1;
    } else {
      ans -= two[c] - 1;
    }
  }
  ans = (ans % MOD + MOD) % MOD;
  cout << ans << endl;
  return 0;
}
```