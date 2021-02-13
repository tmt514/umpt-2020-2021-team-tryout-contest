# D: Detroit
## Solution Sketch

This problem can be solved by dynamic programming.
For any subset of vertices \\(S\\) we define \\(dp(S)\\) to be the minimum number of edges needed where all vertices in \\(S\\) reaches vertex 1.

Using this definition, we notice that, for any edge \\(e=(u, v)\\) and any subset of vertices \\(S\\) that contains \\(v\\), we have \\(dp(S\cup \\{u\\}) \le dp(S) + 1\\).
Hence, we can iterate through every possible subset (from the smallest to the largest, when they are represented in binary numbers).
Within each iteration (with respect to the subset \\(S\\)), we consider all edges \\((u, v)\\) that reaches any vertex \\(v\\) in \\(S\\), and then update \\(dp(S\cup \\{u\\})\\) values accordingly.

The time complexity of the algorithm is \\(O(2^N\cdot M)\\).

## Sample Code (C++)

```c++
#include <bits/stdc++.h>
using namespace std;

vector<int> a[25];
int goal = 0;
int dp[1 << 20];

int main() {
  int N, M, K;
  cin >> N >> M >> K;
  for (int i = 0; i < M; i++) {
    int x, y;
    cin >> x >> y;
    --x;
    --y;
    a[y].push_back(x);
  }
  for (int i = 1; i <= K + 2; i++) {
    goal |= (1 << (i - 1));
  }
  const int INF = 1e9;
  for (int state = 0; state < (1 << N); state++) dp[state] = INF;
  dp[1] = 0;
  int answer = INF;
  for (int state = 1; state < (1 << N); state++) {
    if (dp[state] == INF) continue;
    if ((goal & state) == goal) {
      answer = min(answer, dp[state]);
      continue;
    }
    for (int i = 0; i < N; i++)
      if (state & (1 << i)) {
        for (int j : a[i])
          if (!(state & (1 << j))) {
            int nextstate = (state | (1 << j));
            dp[nextstate] = min(dp[nextstate], dp[state] + 1);
          }
      }
  }
  cout << answer << endl;
  return 0;
}
```