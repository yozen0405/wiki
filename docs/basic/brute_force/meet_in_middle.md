枚舉所有集合的時間複雜度太大，把元素拆成二份分別枚舉再合併，能夠降低時間複雜度，這種技巧稱為「折半枚舉」 (Meet in the Middle)。

???+note "[CSES - Meet in the Middle](https://cses.fi/problemset/task/1628)"
	給一個長度為 $n$ 的序列 $a_1, \ldots ,a_n$，問有幾種方法可以取一些數字加起來為 $x$
	
	$n\le 40, 1\le x, a_i \le 10^9$
	
使用折半枚舉，枚舉第一個集合內的元素，用二分搜得到第二個集合內合法的元素的區間

??? note "code"
	```cpp linenums="1"
	#include <bits/stdc++.h>
    #define int long long
    using namespace std;

    int n, target;

    vector<int> gen(vector<int>& a) {
        vector<int> A;
        A.push_back(0);
        int n = a.size();
        for (int i = 0; i < n; i++) {
            for (int j = A.size() - 1; j >= 0; j--) {
                A.push_back(A[j] + a[i]);
            }
        }
        sort(A.begin(), A.end());
        return A;
    }

    signed main() {
        cin >> n >> target;
        vector<int> a;
        vector<int> b;
        int x;
        for (int i = 0; i < n; i++) {
            cin >> x;
            if (i < n / 2) {
                a.push_back(x);
            } else {
                b.push_back(x);
            }
        }

        vector<int> A = gen(a);
        vector<int> B = gen(b);
        int ans = 0;
        for (int i = 0; i < A.size(); i++) {
            ans += upper_bound(B.begin(), B.end(), target - A[i]) - lower_bound(B.begin(), B.end(), target - A[i]);
        }

        cout << ans << "\n";
    }
    ```

???+note "[2023 YTP 決賽 p8. 保護費 (Protection_Fees)](/wiki/basic/brute_force/images/YTP2023FinalContest_S2_TW.pdf)"
	給 $n$ 個點，$m$ 條邊，每個點有權重 $c_i$，求帶權最大獨立集是多少
	
	$n\le 44,m\le 10^5$
	
	??? note "思路"
		分成 A 跟 B 兩組，看兩組的每個 mask 是否是合法的，sumA[mask] 代表 A 這個 mask 的總和，dpB[mask] 代表 sub & mask = sub，且 sub 合法下最大的 sumB[sub]，okA[mask] 代表 : 對於所有與 mask A 裡面有選的點都不衝突所聯集的 mask B，最後算 sumA[mask] + dpB[okA[mask]] 的最大值就好了
	
		okA[mask] 這個 mask B 不一定要是合法的，他只需要符合「每個有選的元素」跟在 mask A 裡面所有元素都沒有衝突即可，mask B 內鬥也沒差，反正 dpB[mask] 就會將這些內鬥的排除掉

 

