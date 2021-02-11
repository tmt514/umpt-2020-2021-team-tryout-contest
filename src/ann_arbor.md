# A: Ann Arbor

## Solution Sketch

Check for each day, and report the first index \\(i\\) such that \\(a_i < k\\).

## Sample Code (C++)

```c++
#include <bits/stdc++.h>
using namespace std;
 
int main() {
  int k, D, x;
  cin >> k >> D;
  for (int i = 0; i < D; i++) {
    cin >> x;
    if (x < k) {
      cout << (i + 1) << "\n";
      return 0;
    }
  }
  cout << "awesome\n";
  return 0;
}
```