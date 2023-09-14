## 讀取測資

```cpp
inline void debugMode(const char* in, const char* out) {
    if(in) freopen(in, "r", stdin);
    if(out) freopen(out, "w", stdout);
}
```

### 範例

以下程式的輸出都會丟在 `1.out` 裡面

???+note "code"
	```cpp linenums="1"
	#include<bits/stdc++.h>
    using namespace std;

    inline void debugMode(const char* in, const char* out) {
        if(in) freopen(in, "r", stdin);
        if(out) freopen(out, "w", stdout);
    }
    
    int main() {
        debugMode(NULL, "1.out");
        cout << "hi\n";
    }
    ```

## debugging

### 語法

```cpp
#define debug_(x) cerr << #x << " = " << x << ' '
#define debug(x) cerr << #x << " = " << x << '\n'
```

呼叫 `debug(x)` 可以在 terminal 看到輸出，只是在 OnlineJudge 中不會被辨識到

#### 範例

???+note "code"
	```cpp linenums="1"
	#include<bits/stdc++.h>
    #define debug(x) cerr << #x << " = " << x << '\n'

    using namespace std;
    
    int main() {
        int n = 10;
        debug(n); // This will output : n = 10
    }
    ```

### 進階

<font style='font-size:12px'>:octicons-dash-16: <font style='font-size:11px'>參考自 [caido](https://caidocode.blogspot.com/)</font></font>

#### 用法

將程式碼片段保存到名為 `debug_example.cpp` 的檔案中

??? code "debug_example.cpp"
	```cpp linenums="1"
	#include <iostream>
    #include <string>

    #ifdef WAIMAI
    #define debug(HEHE...) std::cout << "[" << #HEHE << "] : ", dout(HEHE)
    void dout() { std::cout << '\n'; }
    template<typename T, typename... U>
    void dout(T t, U... u) { std::cout << t << (sizeof...(u) ? ", " : ""); dout(u...); }
    #else
    #define debug(...) 7122
    #endif
    
    int main() {
        int x = 10;
        double y = 3.14;
        std::string message = "Hello, world!";
    
        debug(x, y, message);  // This will print: [x] : 10, [y] : 3.14, [message] : Hello, world!
    
        return 0;
    }
    ```

使用 `-DWAIMAI` marco 來編譯程式碼，從而定義 `WAIMAI` 的 marco：

```bash
g++ -DWAIMAI debug_example.cpp -o debug_example
```

這個指令告訴編譯器在編譯過程中定義 `WAIMAI`

執行編譯後的程式 :

```bash
./debug_example
```

即可看到輸出

## 對拍

??? note "code"
	```cpp linenums="1"
	#include<bits/stdc++.h>

    namespace AC {
    #include <bits/stdc++.h>
    using namespace std;

    vector<int> solve(const int _d[]) {
        vector<int> ans;
        return ans;
    }
    };  // namespace AC

    namespace TEST {
    #include <bits/stdc++.h>
    using namespace std;

    vector<int> work(const int _d[]) {
        vector<int> ans;
        return ans;
    }
    }  // namespace TEST

    using namespace std;

    bool check(vector<int> d) {
        vector<int> X = AC::solve(d.data());
        vector<int> Y = TEST::work(d.data());
        return X == Y;
    }

    bool gen_random() {
        int n = rand();

        vector<int> d(n + 1);
        for (int i = 1; i <= n; i++) {
            d[i] = rand() % (n - 1) + 1;  // [1, n-1]
        }

        return check(d);
    }

    signed main() {
        for (int i = 0; i < 100; i++) {
            cout << i << " : "<< gen_random() << endl;
        }
    }
    ```

## 參考資料

- <https://charlottehong.blogspot.com/2021/05/cc_23.html>

- <https://codeforces.com/blog/entry/65543>