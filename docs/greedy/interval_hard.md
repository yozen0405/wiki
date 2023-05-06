### 2023 TOI 1模 pB
???+note "最佳劇照 (stills)"
    在一維數線上有 $n$ 個 interval $[l_i, r_i]$，要求選擇最少的 point，使得每個 interval 都至少包含一個 point，且所選點的 cost 總和最小。
    
    其中點的 cost 計算方式為：
    
    $$\text{cost}(t) = 有包含 \texttt{ point } t \text{ } 的 \text{ } \texttt{interval} \text{ } 的\text{ } |(r_i - t) - (t - l_i)| \text{ }的總和$$
    
    ??? note "[algoseacow](https://www.facebook.com/algo.seacow/)'s 題解"
        > 步驟一，離散化:
        
        利用線段的開頭 $l_i$ 和結尾 $r_i$ 可以把數線切出 $2n-1$ 個 blocks。
        
        這樣一來，每個 interval 可以當成是某個 block 的左界開始到某個 block 的右界結束。
    
        每個 block 只會有一個最好的 $t$，先計算出一個陣列 $w[0], ..., w[2n-2]$ 表示每個 block 裡面最好的位置的 cost
    
        > 步驟二，計算 $w[i]$：
        
        每個 block 的 $w[i]$ 計算方式 $|(r_i - t) - (t - l_i) |$ 也可以寫成 $2|(l_i+r_i)/2 - t|$ 也就是 $t$ 到 $[l_i, r_i]$ 中間點的距離的兩倍。
        
        也就是說，一個 block 裡面位置 $t$ 的 cost 就是 $t$ 到所有有包含這個 block 的區間的中間點的距離總和的兩倍。
        
        令 $m_i = (l_i+r_i)/2$，
        
        $$|m_1-t| + |m_2-t| + \cdots |m_k-t|$$
        
        這個函數的最小值發生於 $t$ 等於 $m_i$ 的中位數
        
        不過 $t$ 需要被限制在 block 的範圍內，所以需要分幾個 case 討論
    
        - $m_i$ 的中位數在在 block 內，直接讓 $t = m_i$ 的中位數
    
        - $m_i$ 的中位數在在 block 左邊，直接讓 $t =$ block 左界
    
        - $m_i$ 的中位數在在 block 右邊，直接讓 $t =$ block 右界
    
        掃描線的過程需要對每個 block 維護目前有哪些 $m_i$ 可以用，並且維護這些 $m_i$ 的中位數。決定好 $t$ 之後，需要透過一些資料結構在 $O(\log n)$ 時間計算出 $|m_1-t| + |m_2-t| + ... |m_k-t|$，這個部分可以使用線段樹達成。
    
        > 步驟三， dp 算答案：
        
        $dp(i)$ 表示最後一個點選在 block $i$ ，且左界在 block $i$ 以前的所有 interval 都有包含到至少一個點的最好答案。
        
        $dp(i) = \max dp(j) + w[i]$, $j$ 可以選的條件是沒有 interval 開頭結尾都在 block ($j+1$) ~ block ($i - 1$)
        
        這些的可以用來轉移的 $j$ 會是連續的一段，所以可以用套用資料結構快速計算出 $dp(i)$，計算 dp 的總時間是 $O(n) \times$ 查詢時間。資料結構的部分可以用單調隊列 $O(1)$ 查詢，或是用線段樹 $O(\log n)$ 查詢
        > ref : <https://hackmd.io/@algoseacow/rJw_DISe3>