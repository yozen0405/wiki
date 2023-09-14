## 創建檔案

```cpp
echo .> main.cpp
```

## 讀取 input 檔案

將 `1.in` 作為 input file，並在 terminal 輸出 :

```bash
type 1.in | .\main.exe
```

將 `1.in` 作為 input file，並將輸出放在 `1.out` 裡面

```bash
type 1.in | .\main.exe > 1.out
```

## 判斷輸出檔的內容

例如這是我的檔案

=== "1.out"
	
	```
	5
    1 2
    2 2
    4 4
    5 6
    ```
    
=== "2.out"

	```
	5
    1 2
    2 3
    4 5
    5 6
    ```
    
輸入以下指令 :

```bash
Compare-Object -ReferenceObject (Get-Content 1.out) -DifferenceObject (Get-Content 2.out)
```

即顯示

```
InputObject SideIndicator
----------- -------------
2 3         =>
4 5         =>
2 2         <=
4 4         <=
```