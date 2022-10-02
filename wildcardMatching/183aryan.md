# WildCard Matching

### Problem Statement
![image](https://user-images.githubusercontent.com/76700307/193437383-e0eb8dbd-361f-49d6-8e07-fcb1c08dff8a.png)

### Constraints : 
![image](https://user-images.githubusercontent.com/76700307/193437407-9610f08e-9d15-4b08-bf29-b1c46165c065.png)

### Sample Input :
![image](https://user-images.githubusercontent.com/76700307/193437419-195a1a0c-b672-4cb4-ba72-bd374f56d5b7.png)

### Sample Output :
![image](https://user-images.githubusercontent.com/76700307/193437431-2051a5cf-993f-4436-9ae7-f6928626f809.png)

### Code for this question using memoization : 
```
#include <bits/stdc++.h>

using namespace std;

bool isAllStars(string & S1, int i) {
  for (int j = 0; j <= i; j++) {
    if (S1[j] != '*')
      return false;
  }
  return true;
}

bool wildcardMatchingUtil(string & S1, string & S2, int i, int j, vector < vector < bool >> & dp) {

  //Base Conditions
  if (i < 0 && j < 0)
    return true;
  if (i < 0 && j >= 0)
    return false;
  if (j < 0 && i >= 0)
    return isAllStars(S1, i);

  if (dp[i][j] != -1) return dp[i][j];

  if (S1[i] == S2[j] || S1[i] == '?')
    return dp[i][j] = wildcardMatchingUtil(S1, S2, i - 1, j - 1, dp);

  else {
    if (S1[i] == '*')
      return wildcardMatchingUtil(S1, S2, i - 1, j, dp) || wildcardMatchingUtil(S1, S2, i, j - 1, dp);
    else return false;
  }
}

bool wildcardMatching(string & S1, string & S2) {

  int n = S1.size();
  int m = S2.size();

  vector < vector < bool >> dp(n, vector < bool > (m, -1));
  return wildcardMatchingUtil(S1, S2, n - 1, m - 1, dp);

}

int main() {

  string S1 = "ab*cd";
  string S2 = "abdefcd";

  if (wildcardMatching(S1, S2))
    cout << "String S1 and S2 do match";
  else cout << "String S1 and S2 do not match";
}
```

### Code for given question using tabulation :

```
#include <bits/stdc++.h>

using namespace std;

bool isAllStars(string & S1, int i) {

  // S1 is taken in 1-based indexing
  for (int j = 1; j <= i; j++) {
    if (S1[j - 1] != '*')
      return false;
  }
  return true;
}

bool wildcardMatching(string & S1, string & S2) {

  int n = S1.size();
  int m = S2.size();

  vector < vector < bool >> dp(n + 1, vector < bool > (m, false));

  dp[0][0] = true;

  for (int j = 1; j <= m; j++) {
    dp[0][j] = false;
  }
  for (int i = 1; i <= n; i++) {
    dp[i][0] = isAllStars(S1, i);
  }

  for (int i = 1; i <= n; i++) {
    for (int j = 1; j <= m; j++) {

      if (S1[i - 1] == S2[j - 1] || S1[i - 1] == '?')
        dp[i][j] = dp[i - 1][j - 1];

      else {
        if (S1[i - 1] == '*')
          dp[i][j] = dp[i - 1][j] || dp[i][j - 1];

        else dp[i][j] = false;
      }
    }
  }

  return dp[n][m];

}

int main() {

  string S1 = "ab*cd";
  string S2 = "abdefcd";

  if (wildcardMatching(S1, S2))
    cout << "String S1 and S2 do match";
  else cout << "String S1 and S2 do not match";
}
```

That's all from my side. Thanks for reading.
