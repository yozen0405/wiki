???+note "[CF 1867 E2. Salyg1n and Array (hard version)](https://codeforces.com/contest/1867/problem/E2)"
	有一個長度為 $n$ 的陣列 $a_1,\ldots ,a_n$，目標是輸出 $a_1\oplus \ldots \oplus a_n$。給 $k$，你可以做以下查詢至多 57 次 :
	
	- $\text{query}(i):$ 問 $a_i \oplus a_{i + 1} \oplus \ldots \oplus a_{i + k - 1}$ 是多少，問了之後，這個區間內的元素就會被 reverse

	$n,k\le 2500,n,k$ 皆為偶數
	
	??? note "思路"
		若 n % k == 0，那我們可以每次從左到右依序詢問長度為 k 的區間即可。若 n % k != 0，代表我們左到右依序詢問長度為 k 的區間後會剩下一段，我們的目標就是將這段的值算出來。可以發現若我們以剩下區間的一半做為 query 的結尾 query 一次，在以 n 作為結尾 query 一次，中間多餘的貢獻會剛好消除
		
		<figure markdown>
          ![Image title](./images/2.png){ width="400" }
        </figure>
		
		<figure markdown>
          ![Image title](./images/1.png){ width="400" }
        </figure>
        
    ??? note "code"
    	```cpp linenums="1"
    	#include <bits/stdc++.h>
        #define int long long

        using namespace std;

        int query(int i) {
            cout << "? " << i << endl;
            int res;
            cin >> res;
            return res;
        }

        void solve() {
            int n, k;
            cin >> n >> k;

            int ans = 0, now = 1;
            while (now + k - 1 <= n) {
                int res = query(now);
                ans ^= res;
                now += k;
            }

            if (now < n) {
                int tmp = (n - now + 1) / 2;
                int res1 = query(now + tmp - k);
                ans ^= res1;
                int res2 = query(now + tmp + tmp - k);
                ans ^= res2;
            }

            cout << "! " << ans << '\n';
        }

        signed main() {
            int t = 1;
            cin >> t;
            while (t--) {
                solve();
            }
        } 
        ```