## KMP

!!! info "KMP 演算法陣列習慣使用 1-base"

### 性質1

- 共同前後綴的共同前後綴也會是自己的共同前後綴
- i 是 T(1, i) 的第 1 長共同前後綴長度
- F( i ) 是 T(1, i) 的第 2 長共同前後綴長度
- F( F( i ) ) 是 T(1, i) 的第 3 長共同前後綴長度
- F( F( F( i ) ) ) 是 T(1, i) 的第 4 長共同前後綴長度

### 性質2
- 已知 F(i) 是 T(1, i) 的次長共同前後綴長度
- F(i) - 1 一定是 T(1, i-1) 的一個共同前後綴長度，但不一定是最長
- 換句話說，T(1, i-1) 的某一個共同前後綴長度 + 1 就會是 F(i)
```cpp linenums="1"
vector<int> KMP (string s) { // 1-based string
    int n = s.size () - 1;
    vector<int> F (n + 1, -1);

    for (int i = 1; i <= n; i++) {
        int w = F[i - 1];
        while (w >= 0 && s[w + 1] != s[i]) w = F[w];

        F[i] = w + 1;
    }
    return F;
}

signed main () {
    string s;
    cin >> s;
    int n = s.size ();
    s = "$" + s;
    vector<int> F = KMP (s);
} 
```

https://codeforces.com/contest/955/submission/170083972