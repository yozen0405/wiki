## G. TypeRacer 2 (typeracer2)

[TypeRacer](https://play.typeracer.com/) 是一個基於瀏覽器的多人在線打字遊戲。在 TypeRacer 中，玩家可以與自己或與其他在線用戶競爭，盡快完成各種文本的打字測試。在 TypeRacer 中，玩家需要按照順序按下指定的鍵位。而崔崔，一位職業的 TypeRacer 電競選手，致力於達到理論的打字速度極限。自然，他對於剛發佈的 TypeRacer 2 也是十分有興趣。

但在 TypeRacer 2 這個遊戲裡，需要玩家按下的按鍵就不只是一般鍵盤上會有的按鍵了。所以也需要專業的鍵盤來遊玩這個遊戲。已知遊玩 TypeRacer 2 所需要的鍵盤上有 $K$ 個鍵排成一直線，編號為 $1, 2, \ldots, K$。崔崔的兩隻手上各長有一根手指頭，可以在鍵盤上移動並隨時花費 $0$ 時間單位按下手指頭所在位置的鍵，任一根手指頭從編號 $i$ 的鍵移動到編號 $j$ 的鍵需要 $|i-j|$ 時間單位，且崔崔同時間只能動一根手指頭。

現在，給定長度 $N$ 的 TypeRacer 2 所需要照順序按下的鍵位，請告訴崔崔，他至少需要花多少時間單位才能打完這所有鍵位（崔崔的兩隻手指頭一開始可以在鍵盤上的任意位置）。

## Input

輸入的第一行包含兩個正整數 $N$、$K$，以一個空白隔開。

輸入的第二行包含 $N$ 個正整數 $a_1, a_2, \ldots ,a_N$，代表需要崔崔照順序按下的鍵位。

## Output

輸出一個非負整數佔一行，代表崔崔所需要花的最少時間單位。

## Scoring

- $1 \le K \le N \le 200\,000$。
- $1 \le a_i \le K（1 \le i \le N）$。

| **子任務** | **分數** | **額外輸入限制**   |
| ---------- | -------- | ------------------ |
| 1          | 50       | $1 \le K \le 26$。 |
| 2          | 50       | 無額外限制。       |

## Examples

### sample input 1

```
5 5
5 1 5 2 3
```

### sample output 1

```
2
```

### sample input 2

```
10 6
5 5 3 4 6 6 5 5 6 3
```

### sample output 2

```
5
```

### sample input 3

```
20 10
8 7 8 10 5 1 2 5 3 9 7 3 4 6 5 7 7 9 6 9
```

### sample output 3

```
24
```

## Note

在範例一中，崔崔可以把<font color="red">左手放在 $1$ 上</font>、<font color="blue">右手放在 $5$ 上</font>，並且依序用<font color="blue">右</font>、<font color="red">左</font>、<font color="blue">右</font>、<font color="red">左</font>、<font color="red">左</font>手來按下 $\color{blue}{5}, \color{red}{1}, \color{blue}{5}, \color{red}{2}, \color{red}{3}$。這樣的移動總距離是 $\color{red}{|1-2| + |2-3|} + \color{blue}{|5-5|} = \color{red}{2} + \color{blue}{0} = 2$

## Limits

- time limit per test: 2 seconds

- memory limit per test: 256 megabytes

- input: standard input

- output: standard output