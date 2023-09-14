```cpp
auto dfs = [&]() -> void {

};
```

```cpp
struct Graph {
    vector<vector<int>> G;

    void clear() {
        vector<vector<int>>(G.size(), vector<int>()).swap(G);
    }
};
```

