# E: East Lansing
## Solution Sketch

## Sample Code (C++)

```c++
#include <bits/stdc++.h>
using namespace std;

int N, M;
string a[105];

int compute() {
  int ret = 0;
  string v[105];

  for (int i = 0; i < N; i++) v[i] = a[i];
  for (int i = 0; i < N; i++)
    for (int j = 0; j < M; j++)
      if (v[i][j] != '#') {
        ++ret;
        char c = v[i][j];
        queue<int> q;
        q.push(i);
        q.push(j);
        v[i][j] = '#';
        while (!q.empty()) {
          int x = q.front(); q.pop();
          int y = q.front(); q.pop();
          const int dx[4] = {1, 0, -1, 0};
          const int dy[4] = {0, 1, 0, -1};
          for (int f = 0; f < 4; f++) {
            int nx = x + dx[f], ny = y + dy[f];
            if (nx >= 0 && nx < N && ny >= 0 && ny < M && v[nx][ny] == c) {
              v[nx][ny] = '#';
              q.push(nx);
              q.push(ny);
            }
          }
        }
      }
  return ret;
}

int main() {
  cin >> N >> M;
  for (int i = 0; i < N; i++) {
    cin >> a[i];
  }
  vector<pair<int, int>> p;
  for (int i = 0; i < N; i++)
    for (int j = 0; j < M; j++)
      if (a[i][j] == '?') p.push_back({i, j});

  int k = p.size();
  int answer = N * M;
  for (int state = 0; state < (1 << k); state++) {
    for (int i = 0; i < k; i++)
      a[p[i].first][p[i].second] = '0' + !!(state & (1 << i));
    int c = compute();
    answer = min(answer, c);
  }
  cout << answer << endl;
  return 0;
}
```