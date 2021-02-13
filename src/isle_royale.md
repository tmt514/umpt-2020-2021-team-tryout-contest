# I: Isle Royale
## Solution Sketch

The main observation is that once the energy becomes zero, the problem reduces to the usual shortest path problem (that can be solved using Dijkstra's algorithm).

So, we first run Dijkstra's algorithm on the reversed graph from site \\(N\\). Then, we run an Bellman-Ford style algorithm that answers the following: if we arrive a site within \\(steps\\) minutes (without replenish the energy), what would be the smallest consumption of the enregy?

The total time complexity is \\(O(NM)\\).

## Sample Code (C++)

```c++
#include <bits/stdc++.h>
using namespace std;
 
typedef pair<long long, int> PLI;
const int MAXN = 10005;
vector<pair<int, int>> a[MAXN];
int p[MAXN];
long long dist[MAXN];
 
int main() {
  int N, M, E;
  scanf("%d%d%d", &N, &M, &E);
  for(int i=1;i<=N-1;i++) scanf("%d", &p[i]);
  p[N] = 0;
  for(int i=0;i<M;i++) {
    int x, y, d;
    scanf("%d%d%d", &x, &y, &d);
    a[x].push_back({y, d});
    a[y].push_back({x, d});
  }
  priority_queue<PLI> Q;
  Q.push(PLI(0, N));
  dist[N] = 0;
  const long long INF = 1LL<<60;
  for(int i=1;i<N;i++) dist[i] = INF;
  while (!Q.empty()) {
    PLI z = Q.top(); Q.pop();
    long long d = -z.first;
    int x = z.second;
    if (dist[x] < d) continue;
    for (auto& v : a[x]) {
      int y = v.first;
      int r = v.second;
      long long newcost = dist[x] + r + 1 + p[y] + 1;
      if (newcost < dist[y]) {
        dist[y] = newcost;
        Q.push(PLI(-dist[y], y));
      }
    }
  }
  // ==============================
  long long answer = INF;
  vector<long long> d2(N+1, INF);
  d2[1] = 0;
  for (int steps = 0; steps < N; steps++) {
    vector<long long> d2nxt(N+1, INF);
    for(int x=1;x<=N;x++) if (d2[x] != INF) for(auto& v: a[x]) {
      int y = v.first;
      int r = v.second;
      long long newcost = d2[x] + p[x] + r;
      if (newcost >= E) {
        answer = min(answer, 2 * steps + 2 + (newcost - E) + dist[y]);
      } else if (d2nxt[y] > newcost) {
        answer = min(answer, 2 * steps + 2 + dist[y]);
        d2nxt[y] = newcost;
      }
    }
    d2.swap(d2nxt);
  }
 
  cout << answer << endl;
  return 0;
}
```