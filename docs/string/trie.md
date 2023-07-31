## Trie

```cpp linenums="1"
#include <bits/stdc++.h>
#define int long long
#define lowbit(x) (x & (-x))
#define IO ios::sync_with_stdio(0);cin.tie(0);
#define pii pair<int, int>
#define mk make_pair
#define pb push_back
using namespace std;
 
const int INF = 0x3f3f3f3f;
const int maxn = 1e6 + 5;
const int M = 1e9 + 7;
int dp[maxn];
 
struct node {
    node *ch[26];
    int idx = 0;
};
 
void add (node *rt, string s, int idx) {
    for (int i = 0; i < s.size(); i++) {
        int c = s[i] - 'a';
        if (rt -> ch[c] == nullptr) {
            rt -> ch[c] = new node();
        }
        rt = rt -> ch[c];
    } 
    rt -> idx = idx;
}
 
void solve (node *rt, int idx, string str) {
    for (int i = idx; i >= 1; i--) {
        rt = rt -> ch[str[i - 1] - 'a'];  
        if (rt == nullptr) break;
        if (rt -> idx) {
            dp[idx] += dp[i - 1];
            dp[idx] %= M;
        }
    }
}
 
signed main () {
    string str;
    int n;
    cin >> str;
    cin >> n;
    vector<string> s(n + 1);
    node *rt = new node();
    for (int i = 1; i <= n; i++) {
        cin >> s[i];
        reverse(s[i].begin(), s[i].end());
        add(rt, s[i], i);
    }
    dp[0] = 1;
    for (int i = 1; i <= str.size(); i++) {
        solve(rt, i, str);
    }
    cout << dp[str.size()] << "\n";
}
```