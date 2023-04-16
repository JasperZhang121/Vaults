----
Virtual function calls depend on the type of the object pointed to or referenced, not the type of the pointer or reference itself.

Static functions cannot be declared as virtual functions, nor can they be modified by const and volatile keywords

The static member function does not belong to any class object or class instance, so even adding virutal to this function is meaningless

Virtual functions rely on vptr and vtable to handle. vptr is a pointer, created and generated in the constructor of the class, and can only be accessed with this pointer. Static member functions do not have this pointer, so vptr cannot be accessed.

Constructors cannot be declared as virtual functions. At the same time, except inline|explicit, the constructor does not allow any other keywords.