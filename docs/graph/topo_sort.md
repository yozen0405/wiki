- JOI 2016 p3
- 2015 nhspc p5
- 2022 toi mock 1 pA
- 2021 nhspc pD

拓樸排序:DAG 上某一種節點的順序

$v_1, v_2, . . . , v_n$ ,使得對於所有 i > j,圖上不存在從 i 到 j 的路徑

換句話說,就是如果可以從一個節點 u 走到另一個節點 v,那麼在拓樸排序上,u 要出現在 v 前面

可能不唯一

## 判環

如果 queue 空了以後,還有節點沒拔掉,就代表有環

## 同一個 level 的放一起

???+note "code"
	```cpp linenums="1"
	void topo() {
        queue<int> q;
        while (q.size()) {
            queue<int> nq;
            while(q.size()) {
                auto u = q.front(); 
                q.pop();

                for (auto v : G[u]) {
                    deg[v]--;
                    if (deg[v] == 0) {
                        nq.push(v);
                    }
                }
            }
            q = nq;
        }
    }
    ```

topo sort  https://blog.csdn.net/winter2121/article/details/79437927