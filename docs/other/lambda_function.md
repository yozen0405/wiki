## void

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
    
## function

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