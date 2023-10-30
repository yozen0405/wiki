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

