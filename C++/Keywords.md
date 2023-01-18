------
const
- specifies that a variable's value is constant and tells the compiler to prevent the programmer from modifying it.
- Save memory.
- If variable not specified as const, it will be extern.
- const and ptr:

```C++
# const int *ptr;

# 1.
const int *ptr;
*ptr = 10; //error

# 2.
const int p = 10;
const void * vp = &p;
void *vp = &p; //error
```

```C++
# int * const ptr

# 1
int main(){
    int num=0;
    int * const ptr=&num; //const must be initialized
}

# 2 (ptr points to a variable not a constant)
int main(){
    const int num=0;
    int * const ptr=&num; //error! const int* -> int*
}

# 3
const int p = 3;
const int * const ptr = &p; 
```
- const and function:
```C++

# const for return value
# 1
const int func1(); 

# 2
const int* func2(); // pointer value not change

# 3
int *const func2(); // pointer cannot change


# const for function variables
void func(const int var); // var cannot change
void func(int *const var); // ptr cannot change

```

static:
useful **when we want to have only one instance of our object in the local scope**, which means all calls to the function will share the same object. The same can also be achieved by using global variables or static member variables.

```C++

# static variable in function
void demo() 
{ 
	// static variable 
	static int count = 0; 
	cout << count << " "; 	
	// value is updated and 
	// will be carried to next 
	// function calls 
	count++; 
}

# static variable in class





```