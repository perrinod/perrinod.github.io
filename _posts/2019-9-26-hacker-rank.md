---
layout: default
title: HackerRank
author: Domenick Perrino
description: Hacker Rank Introduction Tutorial
published: true
tags: HackerRank, Hacker Rank, tutorial, explanation
---

<center><h1>Hacker Rank Tutorial</h1></center>

***
This blog post provide a simple explanation to each of the Hacker Rank C++ practice problems labeled under introduction.

As per the <a href="https://www.hackerrank.com/faq" target="_blank">FAQ</a> of hacker rank you are allowed to post code of non ongoing contests.

<center><h3>Say "Hello, World" With C++</h3></center>

***

<a href="https://www.hackerrank.com/challenges/cpp-hello-world/problem" target="_blank">Hacker Rank Say "Hello, World" With C++</a>

<b>Output:</b> Print "Hello, World"

<b>Explanation:</b>
<a href="https://en.cppreference.com/w/cpp/io/cout" target="_blank">cout</a> is a predefined object of ostream class and is used to print to the standard output device.  In order to use cout you must include the <a href="https://en.cppreference.com/w/cpp/io" target="_blank">iostream</a> header which defines the standard input and output stream objects.

* To display to the output you can use cout.
* Must include iostream if you're using cout.
* Easy way to remember cout is console output.  Its true meaning is character output.

```
cout << what you want to display;
```

<< Is just the <a href="http://www.cplusplus.com/reference/ostream/ostream/operator%3C%3C/" target="_blank">insertion operator</a>.

<b>Example:</b>
```c++
#include <iostream>
int main(){
    std::cout << "apples";
    std::cout << 1;
    std::cout << 'a';

    std::cout << " apples " << 1 << " " << 'a';
}
```

<b>Output:</b>
```
apples1a apples 1 a
```
<b>To solve the problem:</b>

```c++
#include <iostream>
int main() {
    std::cout << "Hello, World!";
    return 0;
}
```

<b>Output:</b>
```
Hello, World!
```

<b>What you don't want to do is:</b>
```c++
#include <iostream>
using namespace std; // Don't include this

int main() {
    cout << "Hello, World!";
    return 0;
}
```

This stack overflow question explains <a href="https://stackoverflow.com/questions/1452721/why-is-using-namespace-std-considered-bad-practice" target="_blank">why including using namespace std; in your code is bad practice.</a>

<center><h3>Input and Output</h3></center>

***

<a href="https://www.hackerrank.com/challenges/cpp-input-and-output/problem" target="_blank">Hacker Rank Input and Output</a>

<b>Input:</b> 3 numbers.
<b>Output:</b> Print the sum of the three numbers on a single line.
<b>Constraints:</b> 1 <= a, b, c <= 1000

<b>Explanation:</b>
<a href="https://en.cppreference.com/w/cpp/io/cin" target="_blank">cin</a> is a predefined object of ostream class and is used to get input from a stream buffer.  In order to use cin you must include the <a href="https://en.cppreference.com/w/cpp/io" target="_blank">iostream</a> header which defines the standard input and output stream objects.

* To get input you can use cin.
* Must include iostream if you're using cin.
* Easy way to remember cin is console input.  It's true meaning is character input.

```
cin >> input;
```

Due to the constraints we can simply use int to store the input values.

```
int name = variable;
```

<b>Example:</b>
```c++
#include <iostream>

int main(){
    int a;
    int b, c;
    int d = 0;

    std::cin >> a;
    std::cin >> b >> c;

    std::cout << a << " " << b + c << " " << d;
}
```

<b>Input</b>
```
1 3 5
```

<b>Output:</b>
```
1 8 0
```

<b>Solution:</b>

```c++
#include <iostream>

int main() {
    int a, b, c;
    std::cin >> a >> b >> c;
    std::cout << a + b + c;
}
```

<b>Input:</b>
```
1 2 7
```

<b>Output:</b>
```
10
```

<center><h3>Basic Data Types</h3></center>

***

<a href="https://www.hackerrank.com/challenges/c-tutorial-basic-data-types/problem" target="_blank">Hacker Rank Basic Data Types</a>

<b>Input:</b> Input consists of the following space-separated values: int, long, char, float, and double, respectively.

<b>Output:</b> Print each element on a new line in the same order it was received as input. Note that the floating point value should be correct up to 3 decimal places and the double to 9 decimal places.

<b>Explanation:</b>
We need to use the <a href="https://en.cppreference.com/w/cpp/io/manip" target="_blank">input/output manipulators</a>, specifically <a href="https://en.cppreference.com/w/cpp/io/manip/setprecision" target="_blank">set precision</a>.  This will allow us to set the precision.  In order to use setprecision you must include the <a href="https://en.cppreference.com/w/cpp/header/iomanip" target="_blank">iomanip</a> header.  We also need to use <a href="https://en.cppreference.com/w/cpp/io/manip/fixed" target="_blank">fixed</a> to write float point values in fixed-point notation.

To insert a new line we need to use <a href="https://en.cppreference.com/w/cpp/io/manip/endl" target="_blank">endl</a> which inserts a new line into the output.

All the <a href="https://en.cppreference.com/w/cpp/language/typesfundamental" target="_blank">fundamental types</a> in C++.

* To modify output you can use the iomanip header along with definitions.
* To create a new line you can use endl.
* Must include iostream header to use endl.
* Easy way to remember endl is end line.

```
cout << endl;
```

```
setprecision(integer) << number;
```

```
fundamentaltype = value;
```

<b>Example:</b>
```c++
#include <iostream>
#include <iomanip>
int main() {
    int i = 0;
    long l = 1111;
    double d = 3.14159;
    float f = 3.14159;
    char c = 'a';

    std::cout << std::setprecision(2) << i << " " << d << std::endl;
    std::cout.precision(3);
    std::cout << f << " " << c;
    std::cout << std::endl << std::endl;

    std::cout << std::setprecision(4) << f << " " << std::fixed << " " << f << std::endl;

    // A few other interesting input/output manipulators
    std::cout << std::hex << 42 << std::dec << " " << 42 << std::endl;
    std::cout << std::scientific << f << " " << std::fixed << std::endl;
    std::cout << std::showpos << f << " " << std::noshowpos << f << std::endl;
    std::cout << std::setfill('-') << std::setw(20) << std::left << d << std::endl;
    return 0;
}
```

<b>Output:</b>
```
0 3.1
3.14 a
3.142 3.1416

2a 42
3.141590e+00 
+3.141590 3.141590
3.141590------------
```

<b>Explanation:</b>
```
double d = 3.14159;
setprecision(2) then d displays 3.1 // 2 digits
cout.precision(3) then d displays 3.14 // 3 digits
setprecision(4) then d displays 3.142 // 4 digits
calling fixed after setprecision(4) then d displays 3.1416 // 4 digits after the decimal point
```

Important to note, order doesn't matter you could do:
```
cout << setprecision(2) << fixed << value;
cout << fixed << setprecision(2) << value;
```

You also don't have to keep calling setprecision or fixed.
```
cout << setprecision(2) << fixed;
cout << value << endl; // 2 digits after decimal point
cout << another value; // 2 digits after decimal point
```

<b>Solution:</b>
```c++
#include <iostream>
#include <iomanip>
int main() {
    int a;
    long b;
    char c;
    float d;
    double e;

    std::cin >> a >> b >> c >> d >> e;

    std::cout << a << std::endl << std::fixed << std::setprecision(3) << b << std::endl << c << std::endl << d << std::endl << std::setprecision(9) << e << std::endl;
}
```

<b>Input:</b>
```
3 12345678912345 a 334.23 14049.30493
```

<b>Output:</b>
```
3
12345678912345
a
334.230
14049.304930000
```

<center><h3>Conditional Statements</h3></center>

***

<a href= "https://hackerrank.com/challenges/c-tutorial-conditional-if-else/problem" target="_blank">Hacker Rank Conditional Statements</a>

<b>Input:</b> A single integer.
<b>Output:</b> Lowercase English word corresponding to the number.
<b>Constraints</b> 1 <= n <= 9

We need to use a conditional statment or in this case the more frequently used <a href="https://en.cppreference.com/w/cpp/language/if" target="_blank">if else statement</a>.  Where it conditionally executes another statement.

```
if( condition )
    statement
else
    statement
```

```
if( condition )
    statement
else if( condition )
    statement
else
    statement
```

<b>Example:</b>
```c++
#include <iostream>

int main() {
    int i = 5;
    if(5 < 0)
        std::cout << "five is less than 0" << std::endl;
    else
        std::cout << "five is not less than 0" << std::endl;

    if(5 == 2)
        std::cout << "five is equal to two" << std::endl;
    else if(5 >= 5)
        std::cout << "five is greater than or equal to 5" << std::endl;
    else
        std::cout << "five" << std::endl;

    return 0;
}
```

<b>Output:</b>
```
five is not less than 0
five is greater than or equal to 5
```

This solution is a little silly.  Based on the current knowledge you would know from doing these tutorials in order, this is the approach you would likely end up doing.

<b>Solution:</b>
```c++
#include <iostream>

int main() {
    int input;
    std::cin >> input;

        if(i > 9)
            std::cout << "Greater than 9" << std::endl;
        else if(i == 1)
            std::cout << "one" << std::endl;
        else if(i == 2)
            std::cout << "two" << std::endl;
        else if(i == 3)
            std::cout << "three" << std::endl;
        else if(i == 4)
            std::cout << "four" << std::endl;
        else if(i == 5)
            std::cout << "five" << std::endl;
        else if(i == 6)
            std::cout << "six" << std::endl;
        else if(i == 7)
            std::cout << "seven" << std::endl;
        else if(i == 8)
            std::cout << "eight" << std::endl;
        else
            std::cout << "nine" << std::endl;
 
   return 0;
}
```

<b>Input:</b>
```
5
```

<b>Output:</b>
```
five
```

A more logical approach would be to do something like:
```c++
#include <iostream>
#include <string>
int main() {
    std::string numbers[9] = {"one", "two", "three", "four", "five", "six", "seven", "eight", "nine"};
    int input;
    std::cin >> input;
    if(input > 9)
        std::cout << "Greater than 9";
    else
        std::cout << numbers[input-1];
   return 0;
}

```

<b>Input:</b>
```
5
```

<b>Output:</b>
```
five
```
<center><h3>For Loop</h3></center>

***

<a href="https://www.hackerrank.com/challenges/c-tutorial-for-loop/problem" target="_blank">Hacker Rank For Loop</a>

<b>Input:</b> Two positive integers. a and b (a < b)
<b>Output:</b> For each integer n in the interval [a,b]:
if 1 <= n <= 9 print the English representation of it in lowercase.
else if n > 9 and it is even, print even
else if n > 9 and it is odd, print odd

We need to use a conditional loop or in this case a <a href="https://en.cppreference.com/w/cpp/language/for">for loop</a>.

```
for(declaration or expression, declaration or expression, expression)
    statement
```

<b>Example:</b>
```c++
#include <iostream>
int main() {

    int n = 10;
    for(int i = 0; i < n; i++)
        std::cout << i << " ";

    std::cout << std::endl;
    
    for(int i = 0; i < n; i++){
        if(i > 5)
            break; // exit the loop
        else
            n++; // increment n by 1
    }

    std::cout << n << endl;

    // other helpful information to complete the tutorial.  
    std::cout << 6 % 3 << std::endl; // % returns the remainder

    if(true || false)
        std::cout << "this is true" << std::endl;
    
    if(true && true)
        std::cout << "this is true" << std::endl;

    if(!false)
        std::cout << "this is true" << std::endl;
}
```
<b>Output:</b>
```
0 1 2 3 4 5 6 7 8 9
16
0
this is true
this is true
this is true
```

<b>Explanation:</b>
```
(condition || condition) // returns true if either side is true, false otherwise
(condition && condition) // returns true if both sides are true, false otherwise
(!condition) // returns the opposite of the condition. 
```

Once again this solution is a little silly.  But, based on the current knowledge you would know from doing these tutorials in order, this is the approach you would likely end up doing.  Though this was probably realized as you can just use the older code from the previous tutorial.

<b>Solution:</b>
```c++
#include <iostream>
int main() {
    int a, b;
    
    std::cin >> a, b;
    
    for(int i = a; a <= b; i++){
        if(i > 9 && (i % 2) == 0)
            std::cout << "even" << std::endl;
        else if (i > 9 && (i % 2) != 0)
            std::cout << "odd" << std::endl;
        else if(i == 1)
            std::cout << "one" << std::endl;
        else if(i == 2)
            std::cout << "two" << std::endl;
        else if(i == 3)
            std::cout << "three" << std::endl;
        else if(i == 4)
            std::cout << "four" << std::endl;
        else if(i == 5)
            std::cout << "five" << std::endl;
        else if(i == 6)
            std::cout << "six" << std::endl;
        else if(i == 7)
            std::cout << "seven" << std::endl;
        else if(i == 8)
            std::cout << "eight" << std::endl;
        else
            std::cout << "nine" << std::endl;
    }
}

```

<b>Input:</b>
```
8
11
```

<b>Output:</b>
```
eight
nine
even
odd
```

Once again a more logical approach would be to do something like:
```c++
#include <iostream>
#include <string>
int main() {
    std::string numbers[9] = {"one", "two", "three", "four", "five", "six", "seven", "eight", "nine"};

    int input;
    cin >> input;

    for(int i = a; a <= b; i++){
        if(input > 9){
            if(input % 2 == 0)
                std::cout << "even" << std::endl;
            else
                std::cout << "odd" << std::endl;
        }
        else
            std::cout << numbers[input-1] << std::endl;
    }

   return 0;
}
```
<b>Input:</b>
```
8
11
```

<b>Output:</b>
```
eight
nine
even
odd
```

<center><h3>Functions</h3></center>

***

<a href="https://www.hackerrank.com/challenges/c-tutorial-functions/problem" target="_blank">Hacker Rank Functions</a>

<b>Input:</b> Four integers a, b, c, d.
<b>Output:</b> Print the greatest of the four integers.

<a href="https://en.cppreference.com/w/cpp/language/functions" target="_blank">Functions</a> are C++ entities that associate a sequence of statemtns with a name and a list of zero or more paramaters.

<b>Example:</b>
```c++
bool evenOrOdd(int n){
    return (n % 2 == 0);
}

int sum(int a, int b){
    return a + b;
}

void printString(std::string sentence){
    std::cout << sentence << std::endl;
}
```

<b>Solution:</b>
```c++
int max_of_four(int a, int b, int c, int d){
    int ans = a;

    if(b > ans)
        ans = b;
    if(c > ans)
        ans = c;
    if(d > ans)
        ans = d;

    return ans;
}
```

<b>Input:</b>
```
3
4
6
5
```

<b>Output:</b>
```
6
```

A more logical approach:
```c++
#include<algorithm>
int max_of_four(int a, int b, int c, int d){
    std::vector<int> numbers = {a, b, c, d};
    return *max_element(std::begin(numbers), std::end(numbers));
}

```
<b>Input:</b>
```
3
4
6
5
```

<b>Output:</b>
```
6
```

<center><h3>Pointer</h3></center>

***

<a href="https://www.hackerrank.com/challenges/c-tutorial-pointer/problem" target="_blank">Hacker Rank Pointer</a>

<b>Input:</b> Two integers a and b.
<b>Output:</b> Output already handled. The function must set a = a + b and b = |a - b|

In this case we are dealing with <a href="https://en.cppreference.com/w/cpp/language/pointer" target="_blank">pointers</a>.  These pointers will point to an object that represents the address in memory.  <a href="https://en.cppreference.com/w/cpp/numeric/math/fabs" target="_blank">Abs</a> can be used to calculate the absolute value.  In order to use abs you must include the <a href="https://en.cppreference.com/w/cpp/header/cmath" target="_blank">cmath</a> header.

<b>Example:</b>
```c++
#include<iostream>
#include<cmath>
int main(){
    int n = -2; // declare n and initialize it to 2
    int* c = &n; // declare a pointer to point to the address space of n

    std::cout << c << std::endl; // print the address space
    std::cout << *c << std::endl // print the value of n
    std::cout << std::abs(c);

    return 0;
}
```

<b>Output:</b>
```
0x7fff7feac8cc
-2
2
```

<b>Solution:</b>
```c+++
#include<cmath>
void update(int *a,int *b) {
    int n = (*a) + (*b);
    *b = std::abs((*a) - (*b));
    *a = n;
}
```
<b>Input:</b>
```
4
5
```

<b>Output:</b>
```
9
1
```

<center><h2>Link Resources:</h2></center>

***

* <a href="https://en.cppreference.com/w/cpp/numeric/math/fabs" target="_blank">abs</a>
* <a href="https://en.cppreference.com/w/cpp/io/cin" target="_blank">cin</a>
* <a href="https://en.cppreference.com/w/cpp/header/cmath" target="_blank">cmath</a>
* <a href="https://en.cppreference.com/w/cpp/io/cout" target="_blank">cout</a>
* <a href="https://en.cppreference.com/w/cpp/io/manip/endl" target="_blank">endl</a>
* <a href="https://en.cppreference.com/w/cpp/io/manip/fixed" target="_blank">fixed</a>
* <a href="https://en.cppreference.com/w/cpp/language/for">for loop</a>
* <a href="https://en.cppreference.com/w/cpp/language/functions" target="_blank">functions</a>
* <a href="https://en.cppreference.com/w/cpp/language/typesfundamental" target="_blank"> fundamental types</a>
* <a href="https://en.cppreference.com/w/cpp/language/if" target="_blank">if statement</a>
* <a href="https://en.cppreference.com/w/cpp/io/manip" target="_blank">input/output manipulators</a>
* <a href="http://www.cplusplus.com/reference/ostream/ostream/operator%3C%3C/" target="_blank">insertion operator</a>.
* <a href="https://en.cppreference.com/w/cpp/header/iomanip" target="_blank">iomanip</a>
* <a href="https://en.cppreference.com/w/cpp/io" target="_blank">iostream</a>
* <a href="https://en.cppreference.com/w/cpp/language/pointer" target="_blank">pointers</a>
* <a href="https://en.cppreference.com/w/cpp/io/manip/setprecision" target="_blank">set precision</a>
* <a href="https://stackoverflow.com/questions/1452721/why-is-using-namespace-std-considered-bad-practice" target="_blank">why including using namespace std; in your code is bad practice.</a>

