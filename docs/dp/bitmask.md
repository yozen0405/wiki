```cpp
// 枚舉 n-bit 的所有的子集
for (int U = 0; U < (1 << n); U++) {  // 當前子集為 U, 枚舉該子集  
	for (int S = U; S >= 0; S = (S - 1) & U) {    
		// do something  
	}
}
```

