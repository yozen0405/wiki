## 宣告

???+note "code"
	```cpp linenums="1"
    // include pbds 套件
    #include <bits/extc++.h>
    // 設定命名空間
    using namespace __gnu_pbds;
    // 簡稱 pb_ds::tree

    template <typename T>
    using rank_set = tree<T, null_type, std::less<T>, rb_tree_tag,
                                               tree_order_statistics_node_update>;
    ```

## 功能

- `find_by_order(k)` :  回傳第 k（0-base） 小的元素 的地址

- `order_of_key (x)` : 回傳比 x strictly smaller 的個數

時間複雜度都是 $O(\log n)$

## 範例

???+note "code"
	```cpp linenums="1"
	int main() {
        rank_set<int> s;
        s.insert(4);  // {4}    
        s.insert(1);   // {1, 4}    
        s.insert(9);   // {1, 4, 9}    

        cout << *s.find_by_order(0) << '\n';  // 1   
        cout << s.order_of_key(4) << '\n';  // 1    

        s.erase(1);  // {4, 9}    
        cout << *s.find_by_order(0) << '\n';  // 4
    }
	```

## 例題

???+note "pb_ds - rank tree [LOJ #104. 普通平衡树](https://loj.ac/p/104)"
	實作 pb_ds::tree，支援以下功能：

    1. 插入 $x$
    2. 刪除 $x$
    3. 查詢 $x$ 的是第幾小
    4. 查詢第 $k$ 小的數
    5. 求小於 $x$，最大的數
    6. 求大於 $x$，最小的數
    
    $1 \leq n \leq 10^5,|x|\le 10^7$
    
    ??? note "code"
    	```cpp linenums="1"
    	#include<bits/stdc++.h>
        #include<bits/extc++.h>
        using namespace std;
        using namespace __gnu_pbds;
        typedef int64_t ll;
        template<typename T> using rbt = tree<T, null_type, less<T>, rb_tree_tag, tree_order_statistics_node_update>;
        int32_t main(){
            ios_base::sync_with_stdio(0);cin.tie(0);cout.tie(0);
            int n;
            rbt<ll> eek;
            cin >> n;
            for(ll opt, x; n; --n){
                cin >> opt >> x;
                if(opt == 1) eek.insert((x<<20) + n);
                else if(opt == 2) eek.erase(eek.lower_bound(x<<20));
                else if(opt == 3) cout << eek.order_of_key(x<<20) + 1 << '\n';
                else if(opt == 4) cout << (*eek.find_by_order(x-1) >> 20) << '\n';
                else if(opt == 5) cout << (*--eek.lower_bound(x<<20) >> 20) << '\n';
                else if(opt == 6) cout << (*eek.lower_bound((x+1)<<20) >> 20) << '\n';
            }
            return 0;
        }
        ```

- CSES - List Removals

- CSES - Salary Queries

- CSES - Josephus Problem II

---

## 參考資料

- <https://fhvirus.github.io/blog/2021/pbds-tree/>

- <https://loj.ac/d/415>