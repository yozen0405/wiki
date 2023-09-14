## 判斷環

- 判斷有向圖有沒有環
- DSU 判斷是否在同組
- 判斷 DAG
  - topological sort / DFS
- 判斷有向圖有沒有負環
  - Bellman-Ford
- 判斷有沒有 odd cycle
    - 是二分圖 ⇒ 有 odd cycle
- 最小平均環
  - 用 Bellman-Ford 判負環當黑盒子 $O(nm \log C)$
  - DP $O(nm)$
  - https://drive.google.com/drive/folders/1afCpSerITOFODnY-0F-5TIRggXWerT0Q
- cses cycle finding 
    - print 負環
- 最短邊數環
- [CSES Graph Girth](https://cses.fi/problemset/task/1707)
    - 最小環
    - BFS
- TIOJ 圖論最小圈之測試
    - Floyd-Warshall
- DFS 暴力找最大環
  - [詳解](https://blog.csdn.net/jin739738709/article/details/108083553)
- 正環性質 : 存在點繞一圈路徑權重和皆為正 nhspc 2021 pC

Floyd cycle detection

O(1) 額外空間找環

就是一個人每次走一步，一個人每次走兩步，停在同一個位置就表示找到環了

???+note "DFS 找環"
	```cpp linenums="1"
	void find_cycle(int u, int par) {
        dfn[u] = instk[u] = true;
        for (auto v : G[u]) {
            if (v == par) continue;
            if (!dfn[v]) {
                from[v] = u;
                find_cycle(v, u);
            } else if (instk[v]) {
                get_cycle(v, u);
            }
        }
        instk[u] = false;
    }
    ```

