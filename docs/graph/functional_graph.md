- functional graph
- https://atcoder.jp/contests/abc256/tasks/abc256_e
- https://cses.fi/problemset/task/1678
-  cses planet ..?
- https://usaco.guide/silver/func-graphs?lang=cpp#example---badge
- 2022 toi mock 1 pA
- 2023 toi pC
- https://www.cnblogs.com/cly-none/p/9314812.html
- https://www.cnblogs.com/A-sc/p/13554630.html
- https://www.luogu.com.cn/blog/ShadderLeave/ji-huan-shu-bi-ji
- https://www.luogu.com.cn/problem/P4381
- https://en.m.wikipedia.org/wiki/Pseudoforest

!!! quote ""
	- binary jump (query)

	- topo sort (get rid of tree)
	
	- tree dp (壓縮 subtree + 分 case) 


## POI 2004
- https://www.luogu.com.cn/problem/P1543
- [題解](https://blog.csdn.net/weixin_30689307/article/details/101170191?spm=1001.2101.3001.6650.5&utm_medium=distribute.wap_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7ERate-5-101170191-blog-93249322.wap_relevant_t0_download&depth_1-utm_source=distribute.wap_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7ERate-5-101170191-blog-93249322.wap_relevant_t0_download)

```cpp linenums="1"
include <bits/stdc++.h>

#define int long long
#define fastio ios_base::sync_with_stdio(0); cin.tie(0);

using namespace std;

const int N = 1e6+5;
int arr[N], arr2[N], has[N];

signed main(){
    fastio

    int n,k;
    cin >> n >> k;

    int sum = 0;
    priority_queue<int> pq;

    for(int i = 1; i <= n; i++){
        cin >> arr[i];
    }

    for(int i = 1; i <= n; i++){
        cin >> arr2[i];
    }

    for(int i = 1; i <= k; i++){
        int x;
        cin >> x;
        has[x] = 1;
    }
    int ans = 0;

    for(int i = 1; i <= n; i++){
        sum += arr[i];
        if(arr2[i]!=0){
            sum -= arr2[i];
            pq.push(arr2[i]);
        }
        while(sum < 0){
            auto x = pq.top(); pq.pop();
            if(sum + (-x) > 0){   
                pq.push(x-sum);
                sum = 0;
                break;
            }else{
                sum += (-x);
            }
        }


        if(has[i]){
            if(sum > 0){
                cout << -1 << "\n";
                return 0;
            }
            ans += pq.size();
            while(!pq.empty()) pq.pop();
        }
    }

    cout << ans << "\n";


}
```