----

variable:
```C++

int a = 100; // signed integer, around -2b -> 2b due to 4 bytes limit, 2^(4*8-1)
unsigned int b = 100; // 2^(4*8)
float c = 3.14159f; // 4 bytes
double d = 3.141589798324234; // 8 bytes
bool e = false; // 1 byte, actullt 1 bit in 1 byte
char ch = 'a'; // 1 byte
```

function:
```C++
int multiply(int a, int b) {
	return a * b;
}
void multiOut(int a, int b) {
	std::cout << multiply(a, b) << std::endl;
}
int main(){
	std::cout << multiply(3,4) << std::endl;
	multiOut(3, 4);
}
```


head files:
```C++
# 1. a file called multiply.h in Header Files

#pragma once
int mul(int a, int b) {
	return a * b;
}

# 2. main func

#include "multiply.h"
int main(){
		std::cout << mul(3,4) << std::endl;
}
```











