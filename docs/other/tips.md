## 向上/下取整

???+note "code"
	```cpp linenums="1"
	ll ifloor(ll a, ll b){
        if(b < 0) a *= -1, b *= -1;
        if(a < 0) return (a - b + 1) / b;
        else return a / b;
    }

    ll iceil(ll a, ll b){
        if(b < 0) a *= -1, b *= -1;
        if(a > 0) return (a + b - 1) / b;
        else return a / b;
    }
    ```

## lambda function

### void

The return type of the lambda function is explicitly set to `void` using the `-> void` syntax, indicating that it doesn't return any value.

???+note "code"
	```cpp linenums="1"
	#include <iostream>

    int main() {
        int x = 10;
        int y = 20;
    
        // Lambda function with auto return type explicitly set to void to print the sum of x and y.
        auto printSum = [&](int a, int b) -> void {
            int sum = a + b;
            std::cout << "Sum: " << sum << std::endl;
        };
    
        printSum(x, y); // Output: Sum: 30
    
        return 0;
    }
    ```

### function

回傳 bool，input 一個

???+note "code"
	```cpp linenums="1"
	function<bool(int, int, int)> check = [&](int a, int b, int c) {
        return (a + b) > c;
    };
	```

或者寫成這樣也可以

???+note "code"
	```cpp linenums="1"
	auto check = [&](int a, int b, int c) {
        return (a + b) > c;
    };
	```
	
## 二維 vector 清空

???+note "code"
	```cpp linenums="1"
	struct Graph {
        vector<vector<int>> G;

        void clear() {
            vector<vector<int>>(G.size(), vector<int>()).swap(G);
        }
    };
    ```
    
## 函式互相呼叫

???+note "code"
	```cpp linenums="1"
	#include <iostream>

    // 函数f的声明
    void f(int x);

    void g(int y);

    int main() {
        g(5);
        return 0;
    }

    void f(int x) {
        std::cout << "Function f with parameter: " << x << std::endl;
    }

    void g(int y) {
        std::cout << "Function g with parameter: " << y << std::endl;
        f(y + 1);
    }
    ```
    
## getline

假如說我需要輸入很多含有空格的字串，字串與字串間用換行來分隔，在每個字串與字串間，我又要輸入一個正整數。在處理換行符號時，std::getline() 會將換行符號留在輸入緩衝區中，因為它是字串的一部分。如果緊接著使用 std::cin >> number 讀取整數，那麼輸入緩衝區中的換行符號將被讀取到 number 中，這可能導致不正確的結果，所以需要用 cin.ignore() 來清空緩衝區。

???+note "code"
	```cpp linenums="1"
	#include <iostream>
    #include <string>

    int main() {
        std::string line;
        int number;

        while (std::getline(std::cin, line)) {
            std::cout << "You entered: " << line << std::endl;

            // 在每个字串之后，清理输入缓冲区，以读取整数
            std::cout << "Enter a positive integer: ";
            std::cin >> number;

            // 清理输入缓冲区，以便下一次循环
            std::cin.ignore();

            std::cout << "You entered integer: " << number << std::endl;
        }

        return 0;
    }
    ```