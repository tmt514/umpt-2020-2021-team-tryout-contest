# F: Flint
## Solution Sketch

For the ease of reading, for any set of integers \\(S=\\{s_1, s_2, \ldots, s_k\\}\\) we define \\(\gcd(S) = \gcd(s_1, s_2, \ldots, s_k)\\).

### The Dynamic Programming Approach

Let \\(dp(i, g)\\) be the number of non-empty subsets among the first \\(i\\) elements where \\(\gcd(S) = g\\).
Then, when we consider a new element \\(a_{i+1}\\), we either add it to the subset or not. Hence, we can write the forward updating rule:

$$\begin{aligned}
dp(i+1, \gcd(g, a_{i+1})) &\texttt{+= }  dp(i, g)\\\\
dp(i+1, g) &\texttt{+= } dp(i, g)
\end{aligned}$$

How many possible gcd values will we have? Well, what we know for sure is that this value is always a factor of some input values \\(a_j\\). Hence, a very loose upper bound would be \\(\sum_{i=1}^n d(a_i)\\) where \\(d(x)\\) is the number of factors to \\(x\\). Given the input specification, we know that \\(d(a_i)\le 2\sqrt{10^9}\approx 6.3\times 10^4\\), so, the total time complexity is \\(n\times \sum_{i=1}^n d(a_i) \approx 6.3\times 10^6\\).


### The Inclusion-Exclusion Approach (Combinatorics + Number Theory)

For any integer \\(g\\), it is very easy to compute the number of subsets \\(S\\) with \\(\gcd(S)=g\\). Using the idea of inclusion-exclusion, the answer can be computed by the formula below:

$$
\sum_{g=1}^\infty \mu(g) \left(2^{S_g} - 1\right)
$$

where \\(\mu(g)\\) is the [Möbius function](https://en.wikipedia.org/wiki/M%C3%B6bius_function) and \\(S_g\\) is the number of integers that is divisible by \\(g\\) from the input.

Notice that the summands are non-zero only when \\(g\\) is a factor of some input value. Hence, the total time complexity here is simply \\(\sum_{i=1}^n d(a_i) \approx 6.3\times 10^4\\) plus the time you need to do factorization.

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