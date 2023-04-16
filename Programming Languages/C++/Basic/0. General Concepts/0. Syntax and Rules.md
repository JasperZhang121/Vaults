----

Variable:
```C++

int a = 100; // signed integer, around -2b -> 2b due to 4 bytes limit, 2^(4*8-1)
unsigned int b = 100; // 2^(4*8)
float c = 3.14159f; // 4 bytes
double d = 3.141589798324234; // 8 bytes
bool e = false; // 1 byte, actullt 1 bit in 1 byte
char ch = 'a'; // 1 byte
```

Function:
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


Head files:
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


Memory visulization
start debug -> Debug -> Windows -> Memory ->Memory 1

If:
```C++
	int f = 100;
	bool g = f == 100;
		if (g) {
		printf("true \n");
	}
	if (5) {
		printf("true \n");
	}
	int *ptr = nullptr;
	if (ptr) {
		printf("true \n");
	}else
	{
		printf("null pointer");
	}
```

Compile visualization:
start debug -> right click -> Go to Disassembly

Loop:
```C++
	for (int i = 0; i < 5; i++) {
		printf("loop %d \n", i);
	}
	int i = 0;
	bool condition = true;
	for (; condition; ) {
		printf("loop %d \n", i);
		i++;
		if (i == 5)
			condition = false;
	}

	i = 0;
	while (i<5)
	{
		printf("i<5 \n");
		i++;
	}

		do {
		printf("i>0 \n");
		i--;
	} while (i > 0);
```

Pointers:

type of pointers is not important at all, as a pointer is simply an address in memory

```C++
	int j = 10;
	void* ptr1 = nullptr; // same as 0
	int* ptr2 = &j; // get the address of int j 
	*ptr2 = 8; // change the value in ptr2 address

	char* buffer = new char[8]; // pointer points to the begining of this 8 bytes memory block
	memset(buffer, 0, 8);
	delete[] buffer;
	char** p3 = &buffer;
```

References:

basically a ref is an alias to the variable, change ref simply means change the variable

```C++
	int& ref = j;
	ref = 9;
```

```C++
void increase(int& a){
	a++;
}

void increas(int a) {
	a++;
}

int main() {
	int& ref = j;
	ref = 9;
	printf("ref: %d, j: %d \n", ref, j);

	increase(j);
	printf("%d \n", j);

	increas(j);
	printf("%d \n", j);
}
```

Class:
```C++
class object {
public:
	int size, weight;

	void getFat(int a) {
		weight += a;
	}
};
```

Struct:
basically almost identical to class, but in class you need to claim public to access variables and methods from outside, vice-versa.

Inheritance:
```C++
class object {

public:
	int size, weight;
	object() {
	}
	object(int s, int w) {
		size = s;
		weight = w;
	}
	~object() {
		std::cout << "Delete" << std::endl;
	}
	void getFat(int a) {
		weight += a;
	}
	void printInheri() {
		std::cout << "inheritance" << std::endl;
	}
};

class thi : public object {
public:
};

int main() {
	thi thing1;
	thing1.printInheri();
}
```















