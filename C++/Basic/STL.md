----

vector:

```C++
#include <iostream>
#include <vector>

int main() {
vector<int> v;
	v.push_back(10); // insert numbers into vector
	v.push_back(20);
	v.push_back(30);
	v.push_back(10);

	// visit data in vector by iterator

	vector<int>::iterator itBegin = v.begin(); // first elem
	vector<int>::iterator itEnd = v.end(); // next elem to the last elem
	while (itBegin != itEnd) {
		cout << *itBegin << endl;
		itBegin++;
	}

	// visit by for loop1
	for (vector<int>::iterator it = v.begin(); it != v.end(); it++) {
		cout << *it << endl;
	}

	// visit by index
	for (int i = 0; i < v.size(); i++) {
		cout << v[i] << endl;
	}
}
```

string:
```C++
	// construct string
	string s1; // 1 

	const char* str = "hello there~";
	string s2(str); // 2
	cout<<s2 <<endl;

	string s3(s2); //3
	cout << s3 << endl;

	string s4(10, 'a'); //4
	cout << s4 << endl;
	
	// concatenate
	string s5 = s3 += s2;
	cout << s5 << endl;
	string s6 = s3.append(s2);
	cout << s6 << endl;
	
	// find
	int pos = s2.find("el");
	cout << pos << endl;

	// replace
	s5.replace(0,3,"111");
	cout << s5 << endl;

	// compare
	cout << s1.compare(s2) << endl;
	
	// visit and modify
	cout << s2[0] << endl;
	s2[0] = 'x';
	cout << s2[0] << endl;
	// insert
	s2.insert(1, "1111");
	cout << s2 << endl;
	// erase
	s2.erase(1, 3);
	cout << s2 << endl;
	
```

deque

```C++
	deque<int> d1; 
	d1.push_back(111);
	d1.push_front(222);
	d1.push_back(333);
	d1.push_front(444);

	for (int i = 0; i < d1.size();i++)
		cout << d1[i] << endl;
	cout << " " << endl;

	sort(d1.begin(), d1.end());
	for (int i = 0; i < d1.size();i++)
		cout << d1[i] << endl;
	cout << " " << endl;
	
	d1.pop_front();
	d1.pop_back();
	for (int i = 0; i < d1.size();i++)
		cout << d1[i] << endl;
```

stack:
```C++
	stack<int> st1;
	st1.push(1);
	st1.push(2);
	st1.push(3);
	while (!st1.empty()) {
		cout << st1.top() << endl;
		st1.pop();
	}
```

queue:

```C++
	queue<int> q1;
	q1.push(1);
	q1.push(2);
	q1.push(3);
	q1.push(4);
	
	while (!q1.empty()) {
		cout << "front: " << q1.front() << " back " << q1.back() << endl;
		q1.pop();
	}
```

list:
```C++


```





