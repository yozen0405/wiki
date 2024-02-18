???+note "[Atcoder arc163 C. Harmonic Mean](https://atcoder.jp/contests/arc163/tasks/arc163_c)"
    $T$ 筆詢問，每次輸入 $N$，對於每次詢問，構造出 $A=(A_1,A_2,\ldots ,A_N)$，並符合以下條件 :

    - $\displaystyle \sum\limits_{i=1}^{N}\frac{1}{A_i}=1$
    
    - 全部都不相同
    
    - $1\le A_i \le 10^9(1\le i\le N)$
    
    $1\le T\le 500,1\le N\le 500$
    
    ??? note "思路"
    
        $\Rightarrow$ 國中小數競常用手法
    
        $\begin{cases}\displaystyle 1-\frac{1}{2}+\frac{1}{2}-\frac{1}{3}+ \ldots + \frac{1}{N-1}-\frac{1}{N}+\frac{1}{N}=1 \\ \displaystyle \frac{1}{i-1}-\frac{1}{i}=\frac{1}{(i-1)\times(i)}\end{cases}$
    
        $\displaystyle \frac{1}{1\times2}+\frac{1}{2\times3}+\ldots +\frac{1}{(N-1\times N)}+\frac{1}{N}=1$
    
        但若 $\displaystyle \frac{1}{N}=\frac{1}{(i-1)\times i}$ 這樣就會與前面的數字有重複 ex : 
        
        $\displaystyle \frac{1}{1\times2}+\frac{1}{2\times3}+\frac{1}{3\times4}+\frac{1}{4\times5}+\frac{1}{5\times6}+\frac{1}{6}$
    
        解決方法 :
    
        考慮到 $2,6,12,\ldots$ 差距成遞增，所以若 $\displaystyle \frac{1}{N}=\frac{1}{(i-1)\times i}$，$\displaystyle \frac{1}{N-1}$ 必會合法，所以先設定 $\displaystyle \frac{1}{1\times2}+\frac{1}{2\times3}\ldots \frac{1}{(N-2)\times (N-1)}+\frac{1}{N-1}+x=1$
    
        將前半部和 $x$ 看成兩部分，若前後各 $\displaystyle \frac{1}{2}$ 就會合法。
    
    ??? note "code"
        ```cpp linenums="1"
        #pragma GCC optimize("Ofast,no-stack-protector")
        #include <bits/stdc++.h>
        #define int long long
        #define pb push_back
        using namespace std;
    
        const int MAXN=2e5+10,INF=1e18;
        int t,n;
    
        signed main(){
            cin.tie(0);
            cin.sync_with_stdio(0);
    
            cin>>t;
            while(t--){
                cin>>n;
                if(n==2) cout<<"No\n";
                else{
                    cout<<"Yes\n";
                    int x=sqrt(n);
                    if(n==x*(x+1)){
                        for(int i=1;i<n-1;i++) cout<<i*(i+1)*2<<" ";
                        cout<<(n-1)*2<<" ";
                        cout<<2<<"\n";
                    }
                    else{
                        for(int i=1;i<n;i++) cout<<i*(i+1)<<" ";
                        cout<<n<<"\n";
                    }
                }
            }
        }
        ```

???+note "[2022 TOI pD. 2022](https://tioj.ck.tp.edu.tw/problems/2249)"
	給你兩個正整數 $x, y$，定義一個「好的數字」：

    - 只由 $0$ 和 $2$ 組成，恰有 $x$ 個 $0$ 和 $y$ 個 $2$
    
    - 是 $22$ 的倍數
    
    - 可以有 leading zeroes
    
    請問第二大和第二小的「好的數字」？
    
    $x,y \le 100000$

???+note "<a href="/wiki/problem/images/2024_toi_mock_0_pE.pdf" target="_blank">2024 TOI 模擬賽 測試 pE. 孤獨的吉姆</a>"
	給一個長度為 n 的序列 a，將其 reorder 使 $\min \limits_{i=2\ldots n} \{ a_i - a_{i - 1}\}$ 盡量大。輸出 reorder 後的 a。
	
	$n\le 2\times 10^5, -10^9\le a_i\le 10^9$
	
	??? note "思路"
		打表觀察。
		
		【subtask 1 - n 是偶數】:
		
		假設 $a_1, \ldots ,a_n$ 已經排序好，那答案就是 $a_{\frac{n}{2}+1}, a_1,a_{\frac{n}{2}+2}, a_2,a_{\frac{n}{2}+3}, a_3,\ldots,a_n,a_{n/2}$。也就是每一項跟他後面 n/2 的那一項一組。
		
		之所以會想到要這樣構造，是因為我們會想要讓在重新排列後的相鄰兩個數字，在原來排序好的數列裡距離最近的那兩個盡可能遠，而這樣構造就能使得此重新排列裡相鄰兩個數字在排序好的數列裡編號差至少都達到 n/2。

		> 參考: <https://hackmd.io/@aacp/aatoi2024_pre>

???+note "<a href="/wiki/problem/images/2024_TOI_mock1_pA.pdf" target="_blank">2024 TOI 模擬賽 I pA. 按讚數與追蹤數的神秘挑戰</a>"
    給 $x,y$，問 $x$ 最少要加多少才能變成 $y$ 的重排列

    $x,y$ 位數相同，且位數都 $\le 10^6$

	??? note "思路"
		【暴力的 O(n^2)】
	
        令答案為 $a$。因為要使 $a$ 稍微比 $x$ 大，所以我們會想說有沒有辦法讓 $a$ 跟 $x$ 的前綴盡量一樣，所以我們從高位到低位開始枚舉，看 $a$ 的前 $i$ 位能否跟 $x$ 的前 $i$ 位相同，我們可以讓 $y$ 裡面還沒有填上去的數字大到小依序填在 $a$ 的後面，看能否使 $a > x$，所以這個檢查是 $O(n)$ 的，因為還要枚舉位數，所以總時間複雜度是 $O(n^2)$。
        
        【貪心 O(n)】
        
        考慮貪心，我們從高位到低位開始枚舉，看 $n[i]$ 是否能填一個大於等於 $x[i]$ 的數，可以的話就填進去，然後我們再檢查 $n[i]$ 是否能填一個大於 $x[i]$ 的數，如果可以的話再做個標記，繼續檢查一下位，若我們發現沒辦法填時，我們就去找最近的標記，回到那個時代，後面的按照 Greedy 填就好。
		
		【貪心另解 O(n)】
		
        但如果我們考慮從後面枚舉回來，也就是先假設 $a=x$，然後從低位到高位看，如果 $y$ 是沒有辦法負荷當前 $a$ 裡面的數字，那就把 $a$ 的最低位拔掉，直到 $y$ 有辦法負荷，再來，我們看是否能從 $y$ 裡面還沒放的數字挑出一個比 $x[i]$ 大的數字，如果有辦法，我們就選最小的那個，接在 $a$ 的尾端，然後之後的部分因為前綴已經比 $x$ 大了所以可以隨便填，但又要最小化所以我們選擇小到大填。也就是我們 $a$ 的組成就會是「$x$ 的前綴」+「比 $x[i]$ 大的數字」+「$y$ 剩下的小到大填」。

        例如 $x=1198,y=1197$，會依序執行以下步驟：

        - $a=1198$，發現 $y$ 是沒有辦法負荷，拔掉最後一位

        - $a=119\space \_$，$y$ 有辦法負荷，$y$ 剩下的數字有 $\{7\}$，但沒有大於 $8$ 的所以不能填

        - $a=11\space \_\space \_$，$y$ 有辦法負荷，$y$ 剩下的數字有 $\{7,9 \}$，但沒有大於 $9$ 的所以不能填

        - $a=1\space \_\space \_\space \_$，$y$ 有辦法負荷，$y$ 剩下的數字有 $\{1,7,9 \}$， 大於 $1$ 最小的是 $7$，所以可以填，我們先將 $7$ 填下去變 $a=17\space \_\space \_$，再將 $y$ 剩下的數字小到大填，所以 $a=1719$