---
title: 指令 & 常見問題
---

## 指令懶人包

### windows powershell(cmd)

- `cd <folder> ` 進入folder
	- ex: `cd Desktop`
- `cd ..` 退到上一步的folder
- `mkdir <name>` 創建一個新的folder 
	- ex:  `mkdir projects` 創建一個名叫 `projects` 的folder 
- `ls` 展示目前路徑裡的檔案和資料夾

### vscode 

- `ctrl+shift+p` vscode搜尋欄
- `g++ <檔名>.cpp -o <名字>` 編譯(compile) `<檔名>.cpp` 並且創建執行檔 `<名字>.exe`
	- ex: `g++ main.cpp -o main` 編譯 `main.cpp` 並且將其程式的執行弄到 `main.exe`
- `.\<檔名>.exe` 執行code
	- `.\main.exe`

## 常見問題

??? qs "如果找不到 bin 資料夾 ?"
    - 試試 [google drive](https://drive.google.com/file/d/1OVHKpgJB-Uqvbm7TLBlhwjtZc6Z0HCG8/view?usp=sharing "Title") 下載的方案   

    - 如果出現類似已下指令 可能代表你忘了 save
    
    ```powershell
    c:/mingw/bin/../lib/gcc/mingw32/6.3.0/../../../libmingw32.a(main.o):(.text.startup+0xa0): undefined reference to `WinMain@16'
    collect2.exe: error: ld returned 1 exit status
    ```

??? qs "如果有 save 解果出現以下指令 ?"

    ```powershell
    C:/mingw64/bin/../lib/gcc/x86_64-w64-		mingw32/8.1.0/../../../../x86_64-w64-mingw32/lib/../lib/libmingw32.a(lib64_libmingw32_a-crt0_c.o):crt0_c.c:	(.text.startup+0x2e): undefined reference to `WinMain'
    collect2.exe: error: ld returned 1 exit status
    ```
    
    ---
    
    解決方案
    
    - 代表你可能忘記打 `int main ()`  或是打錯

??? qs "terminal那邊要 run code 有更簡潔的方式嗎 ?"
    
    解決方案
    
    - 直接 `ctrl+shift+B` 就可以了 (編譯+生exe)

??? qs "我用 `ctrl + F5` 偵錯他會顯示 `launch.json` 不存在 ?"
	解決方案
	
    - 這是正常的，因為這部影片並沒有提到 debugger 的部分
    
    - debugger 詳細部分可上 [這篇官網資料](https://code.visualstudio.com/docs/editor/debugging) 查看

??? qs "terminal 出現 當 `C_Cpp.intelliSenseEngine` 設為 `disabled` 時，無法執行 IntelliSense 的相關命令"
	解決方案

	- 可以參考[這篇博客](https://blog.csdn.net/lxj362343/article/details/125711213)

??? qs "檔案執行時一直跳出 launch json 要怎麼辦 ?"
	解決方案
	
    - 看起來你是啟動到 debug 模式了
    - [詳細資料](https://code.visualstudio.com/docs/cpp/launch-json-reference)

??? qs "執行指令時，出現以下錯誤"
	<figure markdown>
      ![Image title](https://i.stack.imgur.com/KrGg0.png){ width="400" }
      <figcaption>error message</figcaption>
    </figure>

	---
	
	解決方案
	
	- 卸載 `clangd` 這個 extension
	
	- 點擊左上角的 `File → Preferences → Settings`
	
	- 在搜尋欄打上 `Intellisense engine`
	
	- 找到 `C_Cpp: IntelliSense Engine` 將其設置為 `Default` (如下圖)
		
	<figure markdown>
		![Image title](https://i.stack.imgur.com/AJnT2.png){ width="400" }
		<figcaption>將其設置為 default</figcaption>
	</figure>


??? qs "在影片中 [4:49](https://www.youtube.com/watch?v=8QdDlNOMCgA&t=289s) 秒時，它找不到 `C/C++: Edit Configurations (UI) ` ?"
	解決方案
	
    - 需要先下載好 C/C++ 的 extension (影片中的 [4:27](https://www.youtube.com/watch?v=8QdDlNOMCgA&t=267s)) ，才有辦法找到
    
    - [參考資料](https://stackoverflow.com/questions/62036568/dont-have-c-cpp-properties-json-file-in-vscode)