# G: Grand Rabbits

## Solution Sketch

Since \\(k \le 10\\), we can do binary search on the answer, and greedily (again on binary search) match each truck to a consecutive range of families.

The time complexity is \\(QK\log N\approx 2\times 10^7\\).

## Sample Code (C++)

```c++
#include <bits/stdc++.h>
using namespace std;

long long a[100005];

int main() {
  cin.sync_with_stdio(false);
  cout.sync_with_stdio(false);
  int N, D;
  cin >> N >> D;
  for (int i = 1; i <= N; i++) {
    cin >> a[i];
    a[i] += a[i - 1];
  }
  for (int i = 0; i < D; i++) {
    int L, R, K;
    cin >> L >> R >> K;
    long long UB = (a[R] - a[L - 1]), LB = 0, answer = UB;
    while (LB <= UB) {
      long long MID = (UB + LB) / 2;
      int s = L;
      long long v = a[L - 1], best_part = 0;
      for (int t = 0; t < K && s <= R; t++) {
        s = upper_bound(a + L, a + R + 1, v + MID) - a;
        best_part = max(best_part, a[s - 1] - v);
        v = a[s - 1];
      }
      if (s > R) {
        // Success: best_part <= MID.
        answer = best_part;
        UB = best_part - 1;
      } else {
        LB = MID + 1;
      }
    }
    cout << answer << '\n';
  }
  return 0;
}
```