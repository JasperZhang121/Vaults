------

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
```


```C++
# static variable in class
class human {
public:
	static int age;
	human () {}
};

int human::age = 1; // solve linking problem
int main() {
	human h1;
	human h2;
	h1.age = 10;
	h2.age = 3;
	std::cout << h1.age << h2.age<< std::endl;
}
```

output:
```
33
```

```C++
class human {
public:
	static int age;
	human () {}
	static void say() {
		std::cout << "say!" << std::endl;
	}
};

int main() {
	human h1;
	h1.say();
	human::say();
}
```
output:
```
say!
say!
```




