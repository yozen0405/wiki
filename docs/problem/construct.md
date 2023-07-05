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