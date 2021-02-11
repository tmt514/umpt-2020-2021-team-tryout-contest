# B: Bois Blanc

## Solution Sketch

### First part

The idea is that, for each lighthouse, we compute the range of lighting angles from the origin \\((0, 0)\\).
The subprocedure we need is to compute the intersection of the unit circle and some ray (half-line).
Suppose a lighthouse is located at \\((x_i, y_i)\\), with direction \\(\alpha_i\\) and vision \\(\theta_i\\).
Then this lighthouse creates two rays, which can be parametrized by

\\(
  \begin{cases}
  x = x_i + t\cdot \cos (\alpha_i \pm \theta_i/2) \\\\
  y = y_i + t\cdot \sin (\alpha_i \pm \theta_i/2) 
  \end{cases}
  \ \ (t > 0)
  \\)

Bringing the above parametrized equations into \\(x^2+y^2=1\\) leads us to a quadratic equation of \\(t\\).
All we need is to solve for a positive \\(t\\) value, then convert it back in the angle form. (In C++ math library `atan2(y, x)` is a very helpful function!)

### Second part

After we got all the angle ranges from the origin, we need to check whether or not these ranges cover the entire circumference. Here's a trick: since \\(N \le 100\\), we do not have to run the greedy algorithm that solves the circular covering problem.

We can discretize the entire circumference using range boundaries. This breaks the circumference into \\(O(N)\\) small segments. Then, we check for each segment (specifically, their **midpoints**), to see if each one is covered by some range. Time complexity here is \\(O(N^2)\\).

## Sample Code (C++)

```c++
#include <bits/stdc++.h>
using namespace std;

const double PI = acos(-1.0);

double solve(double x, double y, double t) {
  // Returns the angle of elevation from the center, given the ray originated
  // from (x, y) with angle t.
  double A = 1.0;
  double B = 2 * x * cos(t) + 2 * y * sin(t);
  double C = x * x + y * y - 1.0;
  double Delta = B * B - 4 * A * C;
  double v = (-B + sqrt(B * B - 4 * A * C)) / 2.0;

  assert(v > 0.0);
  double sx = x + v * cos(t);
  double sy = y + v * sin(t);
  return atan2(sy, sx);
}

bool inbetween(double q, double L, double R) {
  while (q < 0) q += 2 * PI;
  while (q > 2 * PI) q -= 2 * PI;
  if (L <= q && q <= R) return true;
  if (L <= q + 2 * PI && q + 2 * PI <= R) return true;
  if (L <= q - 2 * PI && q - 2 * PI <= R) return true;
  return false;
}

int main() {
  int N;
  vector<pair<double, double>> a;
  vector<double> r;
  cin >> N;
  for (int i = 0; i < N; i++) {
    double x, y, alpha, theta;
    cin >> x >> y >> alpha >> theta;

    double t1 = solve(x, y, alpha - theta / 2);
    double t2 = solve(x, y, alpha + theta / 2);
    if (t2 < t1) t2 += 2 * PI;
    a.push_back({t1, t2});
    r.push_back(t1 - 2 * PI);
    r.push_back(t1);
    r.push_back(t1 + 2 * PI);
    r.push_back(t2 - 2 * PI);
    r.push_back(t2);
    r.push_back(t2 + 2 * PI);
  }

  sort(r.begin(), r.end());
  int badcount = 0;
  for (int i = 0; i + 1 < r.size(); i++) {
    double t = (r[i] + r[i + 1]) / 2.0;
    int ok = 0;
    for (int j = 0; j < N; j++) {
      if (inbetween(t, a[j].first, a[j].second)) ok = 1;
    }
    if (!ok) {
      puts("NO");
      return 0;
    }
  }

  puts("YES");
  return 0;
}
```