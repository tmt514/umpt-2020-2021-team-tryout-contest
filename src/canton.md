# C: Canton

## Solution Sketch

Usual DFS/BFS will solve this problem.

## Sample Code (C++)

```c++
#include <bits/stdc++.h>
using namespace std;
 
int N, M, mxdepth = 0;
string a[2005];
char walk_and_mark(int x, int y, int depth = 0) {
  mxdepth = max(mxdepth, depth);
  if (!(x >= 0 && x < N && y >= 0 && y < M)) return '.';
 
  char dir = a[x][y];
  if (dir == '#') return dir;
  if (dir == '.') return dir;
  a[x][y] = '#';
  
  if (dir == 'N') return a[x][y] = walk_and_mark(x - 1, y, depth+1);
  if (dir == 'S') return a[x][y] = walk_and_mark(x + 1, y, depth+1);
  if (dir == 'E') return a[x][y] = walk_and_mark(x, y + 1, depth+1);
  if (dir == 'W') return a[x][y] = walk_and_mark(x, y - 1, depth+1);
  return '#';
}
 
int main() {
  cin >> N >> M;
  for (int i = 0; i < N; i++) cin >> a[i];
  for (int i = 0; i < N; i++)
    for (int j = 0; j < M; j++) {
      if (a[i][j] >= 'A' && a[i][j] <= 'Z') {
        walk_and_mark(i, j);
      }
    }
  int ans = 0;
  for (int i = 0; i < N; i++)
    for (int j = 0; j < M; j++) ans += (a[i][j] == '#');
  cout << ans << endl;
  return 0;
}
```