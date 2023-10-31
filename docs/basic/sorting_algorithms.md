## Stable Sort

穩定排序法（stable sorting），如果數值相同，排序後「相對位置與排序前相同」，稱穩定排序。

## Bubble Sort

氣泡排序的原理是，每回合當前最大的元素都會透過不斷地與其右手邊的元素交換「浮到」它最終的所在位置，從而在進行 n − 1 回合後確定所有元素都已經到 達正確的位置。

```cpp linenums="1" title="Bubble sort"
void bubble_sort() {
    for(int i = 1; i < n; i++) {
        for(int j = 0; j < n - 1; j++) {
            if(a[j] > a[j + 1]) {
                swap(a[j], a[j+1]);
            }
        }
    }
}
```

時間複雜度 : $O(n^2)$

### 相關例題

???+note "[CS Academy - Sorting Steps](https://csacademy.com/contest/archive/task/sorting-steps/statement/) / [USACO Open 2018 Out of Sort S](https://www.luogu.com.cn/problem/P4378)"
	給一個長度為 $n$ 陣列 $a_1, \ldots ,a_n$，問 bubble sort 會循環幾次（詳見原題）
	
	$n\le 10^5, 1 < a_i < 10^9$
	
	??? note "思路"
		觀察例如範例 `1 3 4 2`，我們是為了要將 2 swap 回原本的位置才需要花那麼久的時間
		
		若每個人左邊的數字都比他小的話，就完成排序了，不然就需要將左邊比他大的數字移到右邊去
	    
	    每個回合會把一個（若存在）自己左邊比自己大的數字移動到自己右邊，當每個人都把比自己大的數字移動到自己右邊，就完成排序了

???+note "[USACO 2018 OPEN Out of Sorts G](http://www.usaco.org/index.php?page=viewproblem2&cpid=837)"
	給一個長度為 $n$ 陣列 $a_1, \ldots ,a_n$，問雙向 bubble sort 會循環幾次（詳見原題）
	
	??? note "思路"
		觀察範例測資 : 
		
		```
		1 8 5 3 2
		1 5 3 2 8
		1 2 5 3 8
		```
		
		可以發現其實就是 swap(2, 8) 而已，有了這個觀察後，我們考慮至少要換幾次，對於一個「分界線」，我們需要將不該在左邊的數字 swap 到右邊去，這個數量也相當於不該在右邊的數字數量，因為對於一條分界線每次至多只能有一組交換，所以令 $m_i=$ 分界線 $(i, i+1)$ ，不該在左邊的數字數量，可以發現我們「至少要換」 $m_i$ 次，對於每個分界線的 lower bound，也就是 $m_i$，max 起來就會是我們需要循環的次數。證明的部分，跑了 $\max(m_i)$ 輪後若還未 sorted，代表存在逆序對，但實際上每條分界線左右都是合法的，所以序列已 sorted。
		
		實作上不該出現在左邊的數字就是離散化後，在 $i$ 左邊比 $i$ 大的數字數量，可以用 BIT 維護。

## Selection sort

選擇排序法的原理是把要排序的序列分成兩堆，一堆是由原序列最小的前 k 個元素所組成並且已經照大小排列，另一堆則是原序列中剩餘的 n − k 個尚未排序的元素。算法每回合都會從未排序堆中選出最小的元素，然後將其移動到已排序堆中的最後面，類似根據大小一個一個叫號排隊，n − 1 回合後便把原序列給排列好了。

```cpp linenums="1" title="Selection sort"
void selection_sort() {
	for (int i = 0; i < n - 1; i++) {
        int min_idx = i;
        for (int j = i + 1; j < n; j++) {
            if (a[j] < a[min_idx]) min_idx = j;
        }
        swap(a[i], a[min_idx]);
    }
}
```

時間複雜度 : $O(n^2)$

## Insertion sort

該算法一樣把原序列區分成已排序和未排序的兩堆，接著把未排序的元素一個一個插入到已排序堆中正確的位置。

```cpp linenums="1" title="Insertion sort"
void insertion_sort() {
    for (int i = 1; i < n; i++) {
        int j, tmp = a[i];
        for (j = i - 1; j >= 0 && a[j] > tmp; j--) {
            a[j + 1] = a[j];
        }
        a[j + 1] = tmp;
    }
}
```

時間複雜度 : $O(n^2)$

## Merge Sort

合併排序法的原理是把要排序的序列分成前後兩等份，分別遞迴處理成兩個排序好的序列後，再將這兩個序列合併成一整個排序好的序列。此演算法需要額外 O(n) 的空間

時間複雜度 : $O(n \log n)$

## Quick sort

合併排序法的原理是選擇序列中一個元素做為基準 (pivot)，接著將小於基準的元素放到序列左邊，大於基準的元素放到序列右邊，接著遞迴處理左右兩個新的序列。快速排序法平均時間複雜度為 $O(n \log n)$，但在基準選得不好，導致左右兩序列大小差很多的情況下，可能達到 $O(n^2)$ 的複雜度。一般為了避免這種情況，基準的選擇會是隨機的。快速排序法的優點是不需要額外記憶體。

期望 : $O(n \log n)$，最差 : $O(n^2)$

## Counting sort

## Radix sort

Radix sort 適用於整數，由個位數開始，對第 $i$ 位數都做一輪排序。每一輪排序只看第 $i$ 位數的大小，以 10 進位整數來說，第 $i$ 位數只有 10 種可能，於是就使用 10 個桶子，並將序列中的數字依照第 $i$ 位數丟進相對應的桶子，再由 $0$ 號桶子開始，將每個桶子中的數字依照放入的順序拿出，replace 原本的序列。由低位數一直做到最高位數，做完後就完成排序了。最多會進行 $\log_{10} C$ 輪排序，每輪排序為 $O(n)$，因此總時間複雜度為 $O(n \log_{10} C)$。一般我們會將 $\log_{10} C$ 視為常數，如排序 int 時該數為 $10$，因此 Radix sort 可以視為一個線性的排序方法。

Radix sort 相較於 Counting sort 可以處理範圍較大的數據。

```cpp linenums="1" title="radix sort"
void radix_sort() {
    vector<int> bucket[10];
    int radix = 1;
    for (int i = 0; i < 10; i++) {
        for (int i = 0; i < 10; i++) {
            bucket[i].clear();
        }
        for (int i = 0; i < n; i++) {
            int digit = (a[i] / radix) % 10;
            bucket[digit].push_back(a[i]);
        }
        int pos = 0;
        for (int i = 0; i < 10; i++) {
            for (auto num : bucket) {
                a[pos++] = num;
            }
        }
        radix *= 10;
    }
}
```

