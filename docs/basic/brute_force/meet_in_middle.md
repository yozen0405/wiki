???+note "[2023 YTP 決賽 p8. 保護費 (Protection_Fees)](/wiki/basic/brute_force/images/YTP2023FinalContest_S2_TW.pdf)"
	給 $n$ 個點，$m$ 條邊，每個點有權重 $c_i$，求帶權最大獨立集是多少
	
	$n\le 44,m\le 10^5$
	
	??? note "思路"
		分成 A 跟 B 兩組，看兩組的每個 mask 是否是合法的，sumA[mask] 代表 A 這個 mask 的總和，dpB[mask] 代表 sub & mask = sub，且 sub 合法下最大的 sumB[sub]，okA[mask] 代表 : 對於所有與 mask A 裡面有選的點都不衝突所聯集的 mask B，最後算 sumA[mask] + dpB[okA[mask]] 的最大值就好了

		okA[mask] 這個 mask B 不一定要是合法的，他只需要符合「每個有選的元素」跟在 mask A 裡面所有元素都沒有衝突即可，mask B 內鬥也沒差，反正 dpB[mask] 就會將這些內鬥的排除掉

 

