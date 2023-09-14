## 事前準備

### 安裝 python

1. 進入 <https://www.python.org> 

2. 將鼠標移到 Downloads 上方 (About 旁邊)
3. 點下去 Download For Windows 下面的那個按鈕
4. 然後開啟你下載檔案
5. 勾選 Add Python 3.11.3 to Path (3.11.3 只是我現在的版本，這個會變)
6. 點擊 Install Now
7. 開啟 cmd 輸入 `python` 看看是否成功安裝

> 參考影片 : <https://www.youtube.com/embed/BRhZyxMBWUI?rel=0>

### 安裝 pip

1. 從[這裡](https://bootstrap.pypa.io/get-pip.py)下載 `get-pip.py` (ctrl + S 下載)
2. 開啟 cmd 進入到與 `get-pip.py` 同樣的路徑
3. 打上 `python get-pip.py` 就完成了

> 參考影片 : <https://www.youtube.com/watch?v=_1B4hckew6Q>

## 安裝 mkdocs

在 cmd 打上
```powershell
pip install --user mkdocs-material
```

接著在你想要的地方建立一個資料夾(之後稱之為根目錄)，在根目錄開啟 cmd 打上

```powershell
mkdocs new .
```

然後找到在根目錄裡面的 `mkdocs.yml` 開啟檔案，加入以下 code	
??? code "mkdocs.yml"
	```yml linenums="1"
	--8<-- "mkdocs.yml"
	```

and then

```powershell
mkdocs serve 
```

然後上 cmd 給你的網址看就可以了

## 部屬到 Github

> 參考 : <https://youtu.be/Q-YA_dA8C20>

```powershell
git add .
git commit -m 'initial commit'
git pull
git push origin main
```

