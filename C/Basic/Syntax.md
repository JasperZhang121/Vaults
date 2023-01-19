
constant variable:
```C
// different types

printf("hello world\n");

int a = 100 * pi;

char ch = 'a';

float c = 3.14159f;

double d = 3.141589798324234;

printf("%d\n", a); // different place holder sign

printf("%c\n", ch);

printf("%f\n", c);

printf("%lf\n", d);

printf("%.3f\n", c);
```

operators:
```C
// arithmetic operators

// ++a: increase a then do calculation

// a++: do calculation then increase a

a = 10;

int b = a++;

int e = ++a;

int f = ++a+1;

printf("%d\n", b);

printf("%d\n", e);

printf("%d\n", f);

printf("%d\n", a);

// boolean in C are normally represented as 1 and 0. 1 is true and 0 is false

int g = 10 > 9;

int h = 1 > 10;

printf("%d, %d \n", g, h);

// logic operators

printf("%d %d\n", g && h, g||h);

printf("%d %d\n", !(g && h), !(g || h));

// bitwise operators

a = 10; // int contains 4 byte; 1 byte = 8 bit;

// left move n means the number will become * 2^n

// right move is vice-versa

printf("%d\n", a<<1);

printf("%d\n", a <<3);

printf("%d\n", a >> 2);

// ~ makes 1 to 0 and 0 to 1

printf("%d\n", ~a); // 10 = ... 0000 1010, reversed 10 = ... 1111 0101

g = 5;

h = 3;

printf("%d\n", g|h); // | : if the bit is 1, then 1 otherwise 0

printf("%d\n", g & h);// & : if both bits is 1,then 1 otherwise 0

printf("%d\n", g ^ h);// ^ : if both bits is 0,then 1 otherwise 0

// +=,-=,*=,%=,and ,

a = (1, 2, 3, 4, 5, 6, 7, 8, 9);

printf("%d\n", a);

// conditinal operators

int l = 1;

int j = 2;

l > j ? (l = 11) : (j = 11);

printf("%d\n", l);

printf("%d\n", j);

// sizeof()

printf("%d\n",sizeof(l));

```

condition and loop:
```C
// if-else

int HP = 10;

if (HP == 100) {

printf("you have full hp, no need to retreat \n");

}

else if (HP < 100) {

printf("you have less than full hp, can choose retreat \n");

}

else {

printf("There is a bug, you have more than full hp! \n");

}

// switch

switch (0)

{

case 0:

printf("1\n");

break;

case 1:

printf("2\n");

break;

case 3:

printf("3\n");

break;

default:

printf("nothing\n");

}

// for loop

for (int z = 0; z < 3; z++)

{

for (int y = 0; y < 3; y++) {

printf("%d, %d \n", z, y);

}

}

// while

a = 3;

while (a>0)

{

a--;

printf("%d\n", a);

}

// do while

do

{

a++;

printf("%d\n", a);

} while (a<3);

// continue

for (int i = 0; i < 10; i++)

{

if (i % 2 == 0) continue;

else {

printf("%d\n", i);

}

}

// goto

for (int i = 0; i < 10; i++)

{

printf("loop\n");

goto a;

}

a: printf("go to a\n");
```


array
```C
// array

int arr[5] = { 1, 2, 10, 9 ,1};

char arr1[3] = { 'a','b','c' };

printf("%d,%d\n",arr[0], arr[1]);

for (int i = 0; i < 5; i++)

{

printf("%p\n", &arr[i]);

}

int arr2[4];

for (int i = 0; i < 4; i++)

{

printf("%d\n", arr2[i]);

}

int arr3[4] = {1,2};

for (int i = 0; i < 4; i++)

{

printf("%d\n", arr3[i]);

}

printf("%d\n", sizeof(arr2));

// 2D array

int arr2D[2][9] = { 1,2,3,4,5,6,7,8,9,

9,8,7,6,5,4,3,2,1};

printf("%d\n", arr2D[0][1]);

int arr2D1[][3] = { 1,2,3,4,5,6,7,8,9,10 };

// 3D array

int arr3D[2][4][3];

// char array

char str[10];

str[0] = 'a';

str[1] = 'e';

printf("%c\n", str[0]);

printf("%s\n", str);

char str1[10] = "aaaa";

char str2[10] = "aaaa";

printf("here\n");

printf("%d\n", strcmp(str1, str2));

printf("%d\n", strlen(str1));

```

function：
```C
int one() {

return two(2,2);

}

int two(int a, int b) {

return a+b;

}

int fun1(int *p1) {

printf("%d\n", *p1);

*p1 = 3;

}
```

