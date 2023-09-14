## 內鍵函式

## 經典例題

### 最大中位數

- atcoder average and median number

???+note "[CF 1486 D. Max Median](https://codeforces.com/problemset/problem/1486/D)"
	給你一個長度為 $n$ 的序列 $a_1,a_2,\ldots, a_n$，問對於每個長度 $\ge k$ 的 subarray 的中位數最大是多少（中位數是排序後第 $\lfloor \frac{n+1}{2}\rfloor$ 位置的值）
	
	$n,k \le 2\times 10^5,a_i \le n$
	
	??? note "思路"
		二分答案，check 答案是否**大於等於** threshold $t$。具體將 $\le t$ 的數字設成 $-1$，$>t$ 的設成 $+1$，只要檢查這個新的序列中有沒有一段 $\ge k$ 的 subarray 的和是 $\ge 0$
		
	??? note "code"
		```cpp linenums="1"
		#include <bits/stdc++.h>
        using namespace std;

        const int N = 2e5 + 5, INF = 0x3f3f3f3f;
        int n, k, a[N];
        int b[N], mn[N];

        bool check(int x) {
            for (int i = 1; i <= n; i++) {
                if (a[i] >= x) {
                    b[i] = 1;
                } else {
                    b[i] = -1;
                }
            }

            for (int i = 1; i <= n; i++) {
                b[i] += b[i - 1];
                mn[i] = min(b[i], mn[i - 1]);
                if (i >= k && b[i] - mn[i - k] >= 1) {
                    return true;
                }
            }
            return false;
        }

        int main() {
            cin >> n >> k;
            for (int i = 1; i <= n; i++) {
                cin >> a[i];
            }

            int l = 1, r = n + 1;
            while (r - l > 1) {
                int mid = (l + r) / 2;
                if (check(mid)) l = mid;
                else r = mid;
            }

            cout << l << '\n';
        }
        ```

若將題目中的中位數換成算數平均值，如何做。 check 答案的時候，將序列每個數同時減去 x 然後找到一個區間累和大於等於 0 的即合法。 因為將每個數同時減去答案 x 後，算數平均值 >= 0 的即原序列算數平均值 >= x

### 動態維護中位數

???+note "[Leetcode 295. Find Median from Data Stream](https://leetcode.com/problems/find-median-from-data-stream/)"
	維護一個集合，一開始集合為空，$q$ 筆以下操作
	
	- `void addNum(int num)` 將 num 這個數字 insert 進去集合裡面
	
	- `double findMedian()` 回傳當前集合的中位數（若 size 為偶數，中位數為中間兩個數的平均）
	
	$-10^5 \le$ num $\le 10^5,q\le 5 \times 10^4$

#### 法一 : 大小堆

開兩個 priority_queue，將輸入的數字分別分成大小兩堆 p1, p2，p1 存前半小的數字，是由 Max Heap，p2 存後半大數字，是 Min Heap，這樣取 p1.top() 和 p2.top() 在偶數個情況就可以取中位數平均，在 n 為奇數的時候我們限制 p1 應比 p2 的 size 少 1，這樣只要取 p2.top() 就是中位數。維護的過程見代碼

??? note "using heap code"
	```cpp linenums="1"
	class MedianFinder {
    public:
        priority_queue<int> p1;
        priority_queue<int, vector<int>, greater<int>> p2;
        int n = 0;

        MedianFinder() {
        }
    
        void addNum(int num) {
            n++;
    		
    		// 將 p2 最小的數字推入 p1
            p2.push(num);
            p1.push(p2.top());
            p2.pop();
    		
    		// 平衡他們的 size
            if (p1.size() > p2.size()) {
                p2.push(p1.top());
                p1.pop();
            }
        }
    
        double findMedian() {
            if (n % 2) return p2.top();
            else return (p1.top() + p2.top()) / 2.0;
        }
    };
    ```

#### 法二 : rank tree


### Sliding Median

???+note "[CSES - Sliding Median]()"

## 題目

???+note "海牛 B 班 class 4 p1"
	給 $n$ 個整數 $a_1,a_2,\ldots,a_n$，每輸入一個整數就要輸出目前已經輸入的數字的中位數。例如輸入為 $[1,7,4,2,5,9]$，輸出依序應為 $[1,4,4,3,4,4.5]$。

    設計一個時間複雜度為 $O(n \log n)$ 的演算法解決此問題。
    
    hint : rank tree