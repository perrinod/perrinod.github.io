---
layout: default
title: "Hacker Rank Problems Of The Day (11/13/2019)"
description: "11/13/2019"
tags: hacker, rank, hacker rank, hackerrank, solution, flipping bits, game of thrones I
---

<center><h1>Hacker Rank Problems Of The Day (11/13/2019)</h1></center>

***

<center><h2>Game of Thrones I</h2></center>

***

<a href="https://www.hackerrank.com/challenges/game-of-thrones/problem" target="_blank">Game of Thrones I</a>

Determine whether a given string can be rearranged into a palindrome. If it is possible, return YES, otherwise return NO.

A palindrome is a word, number or pharse that reads the same backwards as foward.

To solve this we have to understand how a palindrome can be formed.

If the palindrome is of even length then no character can have an odd number.

This is because if we divide the string into two equal halves both the left side and right side must must be mirror images of each other.  

This is easy to show lets start with the smallest palindrome of two characters.

```
palindrome = aa
left side = a
right side = a
```

If we increase it by two more characters.

```
palindrome = abba
left side = ab
right side = ba

```

If the palindrome is of odd length then at most one character can have an odd number.

This is because if we divide the string into two equal halves you're left with a middle character.  This middle character can be any character and will result in a palindrome.

This is easy to show, lets start with a palindrome of three characters.

```
palindrome = aba
left side = a
right side = a
middle character = b
```

If we increase it by two more characters.
```
palindrome = abcba
left side = ab
right side = ba
middle character = c
```

<b>All we need to do is:</b>
Determine the length of the string and then determine if it has too many odd characters.

<center><h3>Game of Thrones I Solutions:</h3></center>

***

<b>C++:</b>

```cpp
string gameOfThrones(string s) {
    std::sort(s.begin(), s.end());

    int count = 0;
    bool palindrome = s.size() % 2 == 0 ? true : false;

    for(int i = 0, j = 0; j < s.size(); j++){
        if(s[i] != s[j]){
            if((j - i) % 2 != 0){
                if(palindrome)
                    return "NO";
                palindrome = true;
            }
            
            i = j;
        }
    }

    return "YES";
}
```
<b>C#:</b>

```cs
static string gameOfThrones(string s) {
    s = String.Concat(s.OrderBy(a => a));

    bool palindrome = s.Length % 2 == 0 ? true : false;

    for(int i = 0, j = 0; j < s.Length; j++){
        if(s[i] != s[j]){
            if((j - i) % 2 != 0){
                if(palindrome)
                    return "NO";
                palindrome = true;
            }
            
            i = j;
        }
    }

    return "YES";
}
```
<b>Java:</b>

```java
static String gameOfThrones(String s) {
    char[] charArray  = s.toCharArray();
    Arrays.sort(charArray);
    s = new String(charArray);

    boolean palindrome = s.length() % 2 == 0 ? true : false;

    for(int i = 0, j = 0; j < s.length(); j++){
        if(s.charAt(i) != s.charAt(j)){
            if((j - i) % 2 != 0){
                if(palindrome)
                    return "NO";
                palindrome = true;
            }
            
            i = j;
        }
    }

    return "YES";

}
```

<b>JavaScript:</b>

```js
function gameOfThrones(s) {
    s = s.split('').sort().join('');

    var palindrome = s.length % 2 == 0 ? true : false;

    for(var i = 0, j = 0; j < s.length; j++){
        if(s[i] != s[j]){
            if((j - i) % 2 != 0){
                if(palindrome)
                    return "NO";
                palindrome = true;
            }
            
            i = j;
        }
    }

    return "YES";

}
```
<b>Python:</b>

```py
def gameOfThrones(s):
    s = ''.join(sorted(s))

    palindrome = True if len(s) % 2 == 0 else False

    i = 0
    for j in range(0, len(s)):
        if(s[i] != s[j]):
            if((j - i) % 2 != 0):
                if(palindrome):
                    return "NO"
                palindrome = True
            
            i = j

    return "YES"
```

<center><h2>Flipping Bits</h2></center>

***

<a href="https://www.hackerrank.com/challenges/flipping-bits/problem" target="_blank">Flipping Bits</a>

You will be given a list of 32 bit unsigned integers. Flip all the bits and print the result as an unsigned integer. 

The largest possible value of an unsigned integer is <a href="https://en.wikipedia.org/wiki/Integer_%28computer_science%29">2^32 - 1</a> or 0 to 4,294,967,295.

<b>All we need to do is:</b>
```
2^32 - 1 - n = flipped bits.
```

To understand why this works you have to know about <a href="https://en.wikipedia.org/wiki/Binary_number#Subtraction" target="_blank">binary subtraction</a>.

<b>Binary subtraction:</b>
```
1 - 1 = 0
1 - 0 = 1
0 - 1 = 1, borrow 1
0 - 0 = 0
```

<b>Binary Subtraction:</b>
So if n = 0 and we wanted to know the flipped bits it would look like this

```
4294967295 - 0 = 429467295

4294967295 = 11111111111111111111111111111111
0 =          00000000000000000000000000000000
4294967295 = 11111111111111111111111111111111
```

If n = 2147483647
```
4294967295 - 2147483647 = 2147483648

4294967295 = 11111111111111111111111111111111
2147483647 = 01111111111111111111111111111111
2147483648 = 10000000000000000000000000000000
```

Instead of doing this you could also use the bitwise operation ~ which flips all bits. Such as:

```
x = ~x;
```

<center><h3>Flipping Bits Solutions:</h3></center>

***

<b>C++:</b>

```cpp
long flippingBits(long n) {
    return UINT_MAX - n;
}
```

<b>C#:</b>

```cs
static long flippingBits(long n) {
    return UInt32.MaxValue - n;
}
```
<b>Java:</b>

```java
static long flippingBits(long n) {
    return (long)Math.pow(2,32) - 1 - n;
}
```
<b>JavaScript:</b>

```js
function flippingBits(N) {
    return Math.pow(2,32) - 1 - N;
}
```

<b>Python:</b>

```py
def flippingBits(n):
    return pow(2,32) - 1 - n
```