----

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
