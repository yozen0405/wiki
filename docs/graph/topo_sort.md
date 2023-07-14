- JOI 2016 p3
- 2015 nhspc p5
- 2022 toi mock 1 pA
- 2021 nhspc pD

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