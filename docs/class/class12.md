KMP 中失敗陣列的意義為何，並請寫出下列字串的 KMP 失敗陣列

1. "abcaac"
2. "ababcaacabcaab"

---

Z 陣列的意義為何，請寫出下列字串的 Z-function 陣列

1. "ababaa"
2. "abaabaabaac"

---

給一個字串，問有幾種不同的substring，目前作法為使用一質數 p 和乘數 x，利用rolling hash算出所有substring 的 hash值

然而 stringhash 若未妥善使用，會因為 hash collision 造成答案錯誤，請提出兩種可以有效減少 hash collision 的設計方式。

---

給多個字串，統計每個字串的出現次數，使用字典樹和 map 都可以得到答案，試分析使用字典樹的話哪些部分會比 map 好。

---

找尋最長在字串中出現至少兩次的 substring，設計一作法使用一種課堂中教的字串演算法解決此題，複雜度至多為 $O(n \log n)$。

Hint : stringhash

---

給一個長度為 n 的字串 S, 請使用至多 O(n) 的時間複雜度預處理, 完成以下操作

1. 在 O(1) 的時間回答子字串 S[l...r] 是不是回文

Hint: stringhash

---

給定兩環狀字串 A,B，判斷兩字串是否相等，如 ACBBB,BBACB 為相等 但 ABBA,ABAB 不相等，設計一作法使用一種課堂中教的字串演算法解決此題，複雜度至多為 O(n)。
Hint: stringhash, Z-algorithm

---

給定兩環狀字串 A,B，ACBBB,BBACB 為相等 但 ABBA,ABAB 不相等，若題目要求有幾個，判斷兩字串是否存在某種 rotate 使得他們至多只有一個字元不同，設計一作法使用課堂中教的字串演算法解決此題，複雜度至多為 O(n)。
Hint: Z-algorithm

---

給定一 array，找到一段連續區間使得區間內的值 xor 起來最大，設計一作法使用課堂中教的字串演算法解決此題，複雜度至多為 O(n)。
Hint: Trie

---

給定三個字串，找到最短的字串同時包含三個字串做為 substring。
hint: KMP

---

- https://cses.fi/problemset/task/2420
- https://leetcode.com/problems/maximum-xor-of-two-numbers-in-an-array/
- https://cses.fi/problemset/task/2107
- https://tioj.ck.tp.edu.tw/problems/1306
- https://codeforces.com/contest/432/problem/D
- https://codeforces.com/problemset/problem/1163/D
- https://codingcompetitions.withgoogle.com/codejam/round/0000000000051635/0000000000104e05
- https://codeforces.com/contest/955/problem/D

