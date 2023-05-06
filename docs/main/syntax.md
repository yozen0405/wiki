## note
- 使用的時候第一行跟 note 那行的中間不能空格

- 但是之後獨立的每行都要空格

- 非獨立的例如程式碼，無序標題

- 最重要的一點<font color="#FA9654">在 note 區塊裡的東西前面要空一格 tab</font>

  
  

=== "語法"
	
    ```markdown
    ???+note "顯示版"
        ??? note "隱藏版"
            - line1
    
            - line2
                - line2-1
                - line2-2
                - line2-3
    
        ??? note "full code" 
            ```py linenums="1"
            def bubble_sort(items):
            	for i in range(len(items)):
                	for j in range(len(items) - 1 - i):
                        if items[j] > items[j + 1]:
                            items[j], items[j + 1] = items[j + 1], items[j]
            ```
    ```


=== "展示"
	???+note "顯示版"
        ??? note "隱藏版"
            - line1
    
            - line2
                - line2-1
                - line2-2
                - line2-3
    
        ??? note "full code" 
            ```py linenums="1"
            def bubble_sort(items):
            	for i in range(len(items)):
                	for j in range(len(items) - 1 - i):
                        if items[j] > items[j + 1]:
                            items[j], items[j + 1] = items[j + 1], items[j]
            ```

更詳細的可以參考[官網](https://squidfunk.github.io/mkdocs-material/reference/admonitions/#inline)
## 程式碼

### copy button
在根目錄的 `mkdocs.yml` 添加

``` yaml linenums="1"
theme:
  features:
    - content.code.copy
```

### Adding title

=== "語法"
	```` markdown 
    ``` py title="bubble_sort.py"
    def bubble_sort(items):
        for i in range(len(items)):
            for j in range(len(items) - 1 - i):
                if items[j] > items[j + 1]:
                    items[j], items[j + 1] = items[j + 1], items[j]
    ```
    ````

=== "展示"
	``` py title="bubble_sort.py"
    def bubble_sort(items):
        for i in range(len(items)):
            for j in range(len(items) - 1 - i):
                if items[j] > items[j + 1]:
                    items[j], items[j + 1] = items[j + 1], items[j]
	```
	
### 加上行號

=== "語法"
	```` markdown 
    ``` py linenums="1"
    def bubble_sort(items):
        for i in range(len(items)):
            for j in range(len(items) - 1 - i):
                if items[j] > items[j + 1]:
                    items[j], items[j + 1] = items[j + 1], items[j]
    ```
    ````
=== "展示"
	``` py linenums="1"
    def bubble_sort(items):
        for i in range(len(items)):
            for j in range(len(items) - 1 - i):
                if items[j] > items[j + 1]:
                    items[j], items[j + 1] = items[j + 1], items[j]
    ```



更詳細的可以參考[官網](https://squidfunk.github.io/mkdocs-material/reference/code-blocks/)

## Content tabs

=== "語法"
    ``` 
    === "C"

        ``` c
        #include <stdio.h>
    
        int main(void) {
          printf("Hello world!\n");
          return 0;
        }
        ```
    
    === "C++"
    
        ``` c++
        #include <iostream>
    
        int main(void) {
          std::cout << "Hello world!" << std::endl;
          return 0;
        }
        ```
    ```

=== "展示"
    === "C"

        ``` c
        #include <stdio.h>
    
        int main(void) {
          printf("Hello world!\n");
          return 0;
        }
        ```
    
    === "C++"
    
        ``` c++
        #include <iostream>
    
        int main(void) {
          std::cout << "Hello world!" << std::endl;
          return 0;
        }
        ```



更詳細的可以參考[官網](https://squidfunk.github.io/mkdocs-material/reference/content-tabs/)