## 基本模板

???+note "code"
	```cpp linenums="1"
	using Point = pair<double, double>;
    #define x first
    #define y second
    Point operator+(Point a, Point b) {
        return {a.x + b.x, a.y + b.y};
    }
    Point operator-(Point a, Point b) {  
        return {a.x - b.x, a.y - b.y};
    }
    Point operator*(Point a, double d) { 
        return {d * a.x, d * a.y};
    }
    double dot(Point a, Point b) {  
        return a.x * b.x + a.y * b.y;
    }
    double cross(Point a, Point b) { 
        return a.x * b.y - a.y * b.x;
    }
	```

## 線段相交判定

???+note "問題"
	給兩個線段的端點座標，判斷是否有交點

    <figure markdown>
      ![Image title](./images/37.png){ width="250" }
    </figure>

我們先判斷 A, B 是否在 CD 兩側，也就是看 cross(AB, AC) 與 cross(AB, AD) 是否正負號不同，然後判斷，判斷 C, D 是否在 AB 兩側，也就是看 corss(AB, AC) 與 cross(AB, AD) 是否正負號不同，兩者都符合時，必定相交

<figure markdown>
  ![Image title](./images/38.png){ width="400" }
</figure>

???+note "目前的 code"
	```cpp linenums="1"
	int sign(long long x) {
        if (x < 0) {
            return -1;
        } else if (x == 0) {
            return 0;
        } else {
            return 1;
        }
    }
    bool intersection(Point a, Point b, Point c, Point d) {
        int c1 = sign(cross(b - a, c - a)) * sign(cross(b - a, d - a));
        int c2 = sign(cross(d - c, a - c)) * sign(cross(d - c, b - c));
        if (c1 == -1 && c2 == -1) return true;
        return false;
    }
    ```
    
但若發生 cross = 0 的 case 呢 ? 可能會發生三點共線

<figure markdown>
  ![Image title](./images/39.png){ width="400" }
</figure>

cross 有 0 的合法 case 至少會有三點共線，所以我們直接將可能的 case 列出來:

- A 在 CD 線段上，回傳 true

- B 在 CD 線段上，回傳 true

- C 在 AB 線段上，回傳 true

- D 在 AB 線段上，回傳 true

???+note "如何判斷一個點 C 在一個線段 AB 上 ?"
	首先要判斷 C 是否在「直線」 AB 上，也就是 cross(AB, AC) 是否為 0
	
	若在「直線」 AB 上的話，再來就要判斷是否在 A, B 之間
	
	<figure markdown>
      ![Image title](./images/40.png){ width="400" }
    </figure>
	
???+note "code"
	```cpp linenums="1"
	bool onseg(Point a, Point b, Point c) {
        if (cross(b - a, c - a) != 0) return false;
        if (sign(dot(b - a, c - a)) < 0) return false;
        if (sign(dot(a - b, c - b)) < 0) return false;
        return true;
    }
    bool intersection(Point a, Point b, Point c, Point d) {
        int c1 = sign(cross(b - a, c - a)) * sign(cross(b - a, d - a));
        int c2 = sign(cross(d - c, a - c)) * sign(cross(d - c, b - c));
        if (c1 == 1 || c2 == 1) return false;
        if (c1 < 0 && c2 < 0) return true;
        if (onseg(a, b, c)) return true;
        if (onseg(a, b, d)) return true;
        if (onseg(c, d, a)) return true;
        if (onseg(c, d, b)) return true;
        return false;
    }
	```
	
## 最近點對問題

### 分治

先把整個陣列 p 按照 x 小到大 sort。以中位數當 pivot，依照 x 軸分成左、右兩堆，遞迴求解子問題，再來 Combine 的部分，我們把以中位數那條線左右距離 <= d 的點都拿出來，按照 y 小到大 sort，對於每個點都只要看周圍至多 6 個點的距離即可，因為我們可以利用類似 Merge sort 的方式得到已 y 小到大 sort 的 p'，所以複雜度是 O(n log n)

```
combine(d, P, mid) {
	P 只留 x \in [mid - d, mid + d]
	每個 P 內的 point，只和上下 8 個點算 distance
	用類似雙指針維護
}

DC(P) {
	(d1, P_L') = DC(P.前半)
	(d2, P_R') = DC(P.後半)
	P' = merge(P_L', P_R') by y 小到大
	d = min(d1, d2)
	combine(d, P', mid)
}
```

### sweep line

sweep line 從 y 小到大掃過每個點，維護一個 set表示目前還與 sweep line 的點距離 <= d 的點，每次只要跟距離 sweep line 前後 6 個點算距離即可

```
sort(p) y 小到大
set 由 x 小到大
l = 1
for r = 1...n:
	while p[l].y <= p[r].y - D:
		刪 p[l]
	找 set 內, p[r].x 前後 6 個點 lower_bound 更新 D
	set.insert(p[r])
```

