## 樹上前綴和

???+note "[CSES - Counting Paths](https://cses.fi/problemset/task/1136)"
	給定 $n$ 個點的樹，再另外給你 $m$ 條 path
	

	對於每個節點 $i$，問這 $m$ 條 path 有幾條有覆蓋到 $i$
	
	$n,m\le 2\times 10^5$
	
	??? note "思路"
		對於每個 path，假設他加值 $u\to \ldots \to \text{LCA}\to \ldots \to v$，假設 LCA 上面的點較 Fa
		
		將 `f[u]++`，`f[v]++`，`f[LCA]--`，`f[Fa]--`
		
		$$f[u]=f[u]+\sum f[v]$$

