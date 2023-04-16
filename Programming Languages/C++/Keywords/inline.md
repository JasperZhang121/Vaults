```C++

#include <iostream>
#include "inline.h"

int Foo(int x,int y);  // claim
inline int Foo(int x,int y) // define
{
    return x+y;
}

int main()
{   
    cout<<Foo(1,2)<<endl;
}
```

Inline fucniton can improve the efficiency of functions, but not all functions are defined as inline functions! Inlining is at the cost of code expansion (copying), which only saves the overhead of function calls, thereby improving the execution efficiency of functions.

If the time to execute the code in the function body is large compared to the overhead of the function call, then the efficiency gains will be even less!

On the other hand, every inline function call needs to copy the code, which will increase the total code size of the program and consume more memory space.