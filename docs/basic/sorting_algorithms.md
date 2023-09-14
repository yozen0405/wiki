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

## Radix sort

基數排序法適用於整數，由個位數開始，對第 $r$ 位數都做一輪排序。每一輪排序只看第 $r$ 位數的大小，以 10 進位整數來說，第 $r$ 位數只有 10 種可能，於是就使用 10 個桶子，並將序列中的數字依照第 $r$ 位數丟進相對應的桶子，再由 $0$ 號桶子開始，將每個桶子中的數字依照放入的順序拿出，形成一個新的序列。由低位數一直做到最高位數，做完後就完成排序了。若欲排序的 10 進位數字最大為 $C$，則最多會進行 $\log_{10} C$ 輪排序，每輪排序為 $O(n)$，因此總時間複雜度為 $O(n \log_{10} C)$。一般我們會將 $\log_{10} C$ 視為常數，如排序 int 時該數為 $10$，因此 Radix sort 可以視為一個線性的排序方法。

```cpp linenums="1" title="radix sort"
void radix_sort() {
    vector<int> bucket[10];
    for (int radix = 1, r = 0; r < 10; radix *= 10, r++) {
        for (int i = 0; i < 10; i++) {
            bucket[i].clear();
        }
        for (int i = 0; i < n; i++) {
            int digit = (a[i] / radix) % 10;
            bucket[digit].push_back(a[i]);
        }
        int len = 0;
        for (int i = 0; i < 10; i++) {
            int m = bucket[i].size();
            for (int j = 0; j < m; j++) {
                a[len++] = bucket[i][j];
            }
        }
    }
}
```

