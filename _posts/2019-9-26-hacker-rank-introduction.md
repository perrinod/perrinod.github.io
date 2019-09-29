---
layout: default
title: HackerRank Algorithms Part I
author: Domenick Perrino
description: Hacker Rank Warmup Problems
published: true
tags: HackerRank, Hacker Rank, tutorial, explanation, solutions
---

<center><h1>Hacker Rank Algorithm Warmup Problems</h1></center>

***
As per the <a href="https://www.hackerrank.com/faq" target="_blank">FAQ</a> of hacker rank you are allowed to post code of non ongoing contests.

This blog post provides each of my solutions and their time complexity to the Hacker Rank algorithm warmup problems.  I don't explain my solutions because they are straight foward loops and if statements.  In future problems when they become more complex, I will.  Most problems were done in C++, C#, java, javascript, python.  You'll notice that most of my code is similar to one another.  This is because I solved the problem in one language then afterwards used that same approach with the other languages. 

<center><h3>Simple Array Sum</h3></center>

***

<a href="https://www.hackerrank.com/challenges/simple-array-sum/problem" target="_blank">Hacker Rank Simple Array Sum</a>

<a href="https://github.com/perrinod/hacker-rank-solutions/tree/master/Algorithms/Warmup/Simple%20Array%20Sum" target="_blank"> Github repository </a>

<b>C++:</b>
```c++
int simpleArraySum(std::vector<int> ar) {
    return std::accumulate(ar.begin(), ar.end(), 0);

}
```

<b>C#:</b>
```c#
static int simpleArraySum(int[] ar) {
    return ar.Sum();
}
```

<b>Python:</b>
```py
def simpleArraySum(ar):
    return sum(ar)
```

<b>Java:</b>
```java
static int simpleArraySum(int[] ar) {
    int result = 0;
    for(int number : ar)
        result += number;
    return result;
}
```

<b>JavaScript:</b>
```js
function simpleArraySum(ar) {
    var sum = 0;
    for(var i = 0; i < ar.length; i++)
        sum += ar[i];
    return sum;
}
```

<b>Time Complexity:</b> O(n)
In each one we are iterating through n elements to create their sum.

<center><h3>Compare the triplets</h3></center>

***

<a href="https://www.hackerrank.com/challenges/compare-the-triplets/problem" target="_blank">Hacker Rank Compare The Triplets</a>

<a href="https://github.com/perrinod/hacker-rank-solutions/tree/master/Algorithms/Warmup/Compare%20the%20Triplets" target="_blank"> Github repository </a>

<b>C++:</b>
```c++
vector<int> compareTriplets(std::vector<int> a, std::vector<int> b) {
    int alisonScore = 0;
    int bobScore = 0;
    for(int i = 0; i < a.size(); i++){
        if(a[i] < b[i])
            bobScore++;
        else if(a[i] > b[i])
            alisonScore++;
    }

    return std::vector<int> {alisonScore, bobScore};
}
```

<b>C#</b>
```c#
static List<int> compareTriplets(List<int> a, List<int> b) {
    int alisonScore, bobScore;
    alisonScore = bobScore = 0;

    for(int i = 0; i < a.Count; i++){
        if(a[i] < b[i])
            bobScore++;
        else if(a[i] > b[i])
            alisonScore++;
    }

    return new List<int> {alisonScore, bobScore};
}
```
<b>Python</b>
```py
alisonScore = bobScore = 0
for i in range(0, len(a)):
    if(a[i] < b[i]):
        bobScore += 1
    elif(a[i] > b[i]):
        alisonScore += 1
    
return [alisonScore, bobScore]
```
<b>Java:</b>
```java
static List<Integer> compareTriplets(List<Integer> a, List<Integer> b) {
    int alisonScore, bobScore;
    alisonScore = bobScore = 0;

    for(int i = 0; i < a.size(); i++){
        if(a.get(i) < b.get(i))
            bobScore++;
        else if(a.get(i) > b.get(i))
            alisonScore++;
    }

    return Arrays.asList(alisonScore, bobScore); 
}
```

<b>Javascript:</b>
```js
function compareTriplets(a, b) {
    var alisonScore, bobScore;
    alisonScore = bobScore = 0;

    for(var i = 0; i < a.length; i++){
        if(a[i] < b[i])
            bobScore++;
        else if(a[i] > b[i])
            alisonScore++;
    }

    return new Array (alisonScore, bobScore);

}
```

<b>Time Complexity:</b> O(n)
In each one we are iterating through n elements comparing each element to another elemeent


<center><h3>A Very Big Sum</h3></center>

***

<a href="https://www.hackerrank.com/challenges/a-very-big-sum/problem" target="_blank">Hacker Rank A Very Big Sum </a>

<a href="https://github.com/perrinod/hacker-rank-solutions/tree/master/Algorithms/Warmup/A%20Very%20Big%20Sum" target="_blank"> Github repository </a>

Solutions are virtually identical to the Simple Array Sum problem.

<b>C++:</b>
```c++
long aVeryBigSum(std::vector<long> ar) {
    return std::accumulate(ar.begin(), ar.end(), (long)0);
}
```

<b>C#:</b>
```c#
static long aVeryBigSum(long[] ar) {
    return ar.Sum();

}
```

<b>Python:</b>
```py
def aVeryBigSum(ar):
    return sum(ar)
```

<b>Java:</b>
```java
static long aVeryBigSum(long[] ar) {
    long sum = 0;
    for(int i = 0; i < ar.length; i++)
        sum += ar[i];
    return sum;
}
```

<b>Javascript:</b>
```js
function aVeryBigSum(ar) {
    var sum = 0;
    for(var i = 0; i < ar.length; i++)
        sum += ar[i];
    return sum;
}
```

<b>Time Complexity:</b> O(n)
iterating through n elements to create their sum.

<center><h3>Diagonal Difference</h3></center>

***

<a href="https://www.hackerrank.com/challenges/diagonal-difference/problem" target="_blank">Hacker Rank Diagonal Difference</a>

<a href="https://github.com/perrinod/hacker-rank-solutions/tree/master/Algorithms/Warmup/Diagonal%20Difference" target="_blank"> Github repository </a>

<b>C++:</b>
```c++
int diagonalDifference(std::vector<std::vector<int>> arr) {
    int first, second, j = arr.size() - 1;
    first = second = 0;
    for(int i = 0; i < arr.size(); i++, j--){
        first += arr[i][i];
        second += arr[j][i];
    }
    
    return abs(first - second);
}
```

<b>C#:</b>
```c#
class Result
{
    public static int diagonalDifference(List<List<int>> arr)
    {
        int first, second, j = arr.Count - 1;
        first = second = 0;
        for(int i = 0; i < arr.Count; i++, j--){
            first += arr[i][i];
            second += arr[j][i];
        }
    
        return Math.Abs(first - second);

    }

}
```

<b>Python:</b>
```py
def diagonalDifference(arr):
    first = second = 0
    j = len(arr) - 1
    for i in range(0, len(arr)):
        first += arr[i][i]
        second += arr[j][i]
        j -= 1
    
    return abs(first - second)
```

<b>Java:</b>
```java
class Result {

    public static int diagonalDifference(List<List<Integer>> arr) {
        int first, second, j = arr.size() - 1;
        first = second = 0;
        for(int i = 0; i < arr.size(); i++, j--){
            first += (arr.get(i)).get(i);
            second += (arr.get(j)).get(i);
        }
    
        return Math.abs(first - second);

    }

}
```

<b>Javascript:</b>
```js
function diagonalDifference(arr) {
    var first, second, j = arr.length - 1;
    first = second = 0;

    for(var i = 0; i < arr.length; i++, j--){
        first += arr[i][i];
        second += arr[j][i];
    }

    return Math.abs(first - second);

}
```

<b>Time Complexity:</b> O(n)
iterating through n elements to create two different sums.

<center><h3>Plus Minus</h3></center>

***

<a href="https://www.hackerrank.com/challenges/plus-minus/problem" target="_blank">Hacker Rank Plus Minus</a>

<a href="https://github.com/perrinod/hacker-rank-solutions/tree/master/Algorithms/Warmup/Plus%20Minus" target="_blank"> Github repository </a>

<b>C++:</b>
```c++
void plusMinus(std::vector<int> arr) {
    double positive, negative, zero;
    positive = negative = zero = 0;

    for(int& number : arr){
        if(number > 0) positive++;
        else if(number < 0) negative++;
        else zero++;
    }

    std::cout << positive / arr.size() << std::endl 
    << negative / arr.size() << std::endl << zero / arr.size();
}
```

<b>C#:</b>
```c#
static void plusMinus(int[] arr) {
    double positive, negative, zero;
    positive = negative = zero = 0;

    for(int i = 0; i < arr.Length; i++){
        if(arr[i] > 0) positive++;
        else if(arr[i] < 0) negative++;
        else zero++;
    }
        
    Console.WriteLine(positive / arr.Length + "\n" + negative / arr.Length + "\n" + zero / arr.Length);
}
```

<b>Python:</b>
```py
def plusMinus(arr):
    positive = negative = zero = 0.0

    for i in range(0, len(arr)):
        if arr[i] > 0 : positive += 1
        elif arr[i] < 0 : negative += 1
        else : zero += 1

    print(positive / len(arr), '\n' , negative / len(arr), '\n', zero / len(arr))
```

<b>Java:</b>
```java
static void plusMinus(int[] arr) {
    double positive, negative, zero;
    positive = negative = zero = 0;

    for(int number : arr){
        if(number > 0) positive++;
        else if(number < 0) negative++;
        else zero++;
    }

    System.out.print(positive / arr.length + "\n" + negative / arr.length +"\n" + zero / arr.length);

}
```

<b>Javascript</b>
```js
function plusMinus(arr) {

    var positive, negative, zero;
    positive = negative = zero = 0;
    for(var i = 0; i < arr.length; i++){
        if(arr[i] > 0) positive++;
        else if(arr[i] < 0) negative++;
        else zero++;
    }

    console.log(positive / arr.length + "\n" + negative /arr.length + "\n" + zero / arr.length);

}
```

<b>Time Complexity:</b> O(n)
iterating through n elements to create the sum of positive, negative and zero numbers.

<center><h3>Staircase</h3></center>

***

<a href="https://www.hackerrank.com/challenges/staircase/problem" target="_blank">Hacker Rank Staircase</a>

<a href="https://github.com/perrinod/hacker-rank-solutions/tree/master/Algorithms/Warmup/Staircase" target="_blank"> Github repository </a>

<b>C++:</b>
```c++
void staircase(int n) {
    for(int i = 1; i < n + 1; i++){
        std::string spaces (n - i, ' ');
        std::string hashtag(i, '#');
        std::cout << spaces + hashtag << std::endl;
    }

}
```

<b>C#:</b>
```c#
static void staircase(int n) {
    for(int i = 1; i < n + 1; i++){
        string spaces = new String(' ', n - i);
        string hashtag = new String('#', i);
        Console.WriteLine(spaces + hashtag);
    }
}
```

<b>Time Complexity:</b> O(n)
iterating through n elements to print a string.

<center><h3>Mini-Max Sum</h3></center>

***


<a href="https://www.hackerrank.com/challenges/mini-max-sum/problem" target="_blank">Hacker Rank Mini-Max Sum</a>

<a href="https://github.com/perrinod/hacker-rank-solutions/tree/master/Algorithms/Warmup/Mini-Max%20Sum" target="_blank"> Github repository </a>

<b>C++:</b>
```c++
void miniMaxSum(std::vector<int> arr) {
    long min, max, sum = 0;
    min = max = arr.front();
    for(int& number : arr){
        sum += number;
        if(number < min)
            min = number;
        if(number > max)
            max = number;
    }

    std::cout << sum - max << " " << sum - min << std::endl;
}
```

<b>C#:</b>
```c#
static void miniMaxSum(int[] arr) {
    long min, max, sum = 0;
    min = max = arr[0];
    for(int i = 0; i < arr.Length; i++){
        sum += arr[i];
        if(arr[i] < min)
            min = arr[i];
        if(arr[i] > max)
            max = arr[i];
    }

    Console.WriteLine((sum - max) + " " + (sum - min));

}
```

<b>Python:</b>
```py
def miniMaxSum(arr):
    mini, maxi, total = arr[0], arr[0], 0
    for i in range(0, len(arr)) :
        total += arr[i]
        if(arr[i] < mini):
            mini = arr[i]
        if(arr[i] > maxi):
            maxi = arr[i]

    print(total - maxi, total - mini)
```

<b>Java:</b>
```java
static void miniMaxSum(int[] arr) {
    long min, max, sum = 0;
    min = max = arr[0];
    for(int i = 0; i < arr.length; i++){
        sum += arr[i];
        if(arr[i] < min)
            min = arr[i];
        if(arr[i] > max)
            max = arr[i];
    }

    System.out.println((sum - max) + " " + (sum - min));

}
```

<b>JavaScript</b>
```js
function miniMaxSum(arr) {
    var min, max, sum = 0;
    min = max = arr[0];

    for(var i = 0; i < arr.length; i++){
        sum += arr[i];
        if(arr[i] < min)
            min = arr[i];
        if(arr[i] > max)
            max = arr[i];
    }

    console.log((sum - max) + " " + (sum - min));

}
```

<b>Time Complexity:</b> O(n)
iterating through n elements to determine minimum and maximum value.

<center><h3>Birthday Cake Candles</h3></center>

***

<a href="https://www.hackerrank.com/challenges/birthday-cake-candles/problem" target="_blank">Hacker Rank Birthday Cake Candles</a>

<a href="https://github.com/perrinod/hacker-rank-solutions/tree/master/Algorithms/Warmup/Birthday%20Cake%20Candles" target="_blank"> Github repository </a>

<b>C++:</b>
```c++
int birthdayCakeCandles(vector<int> ar) {
    int sum = 1, height = ar.front();
    for(auto i = ar.begin() + 1 ; i != ar.end(); ++i){
        if(*i > height){
            sum = 0;
            height = *i;
        }
        if(height == *i)
            sum++;
    }
    return sum;

}
```

<b>C#:</b>
```c#
static int birthdayCakeCandles(int[] ar) {
    int sum = 1, height = ar[0];
    for(int i = 1; i < ar.Length; i++){
        if(ar[i] > height){
            sum = 0;
            height = ar[i];
        }
        if(height == ar[i])
            sum++;
    }
    return sum;

}
```

<b>Python:</b>
```py
def birthdayCakeCandles(ar):
    total, height = 1, ar[0]

    for i in range(1, len(ar)):
        if ar[i] > height : 
            total = 0
            height = ar[i]
        if height == ar[i] :
            total+= 1

    return total
```

<b>Java:</b>
```java
static int birthdayCakeCandles(int[] ar) {
    int sum = 1, height = ar[0];
    for(int i = 1; i < ar.length; i++){
        if(ar[i] > height){
            sum = 0;
            height = ar[i];
        }
        if(height == ar[i])
            sum++;
    }

    return sum;

}
```

<b>Javascript</b>
```js
function birthdayCakeCandles(ar) {
    var sum = 1, height = ar[0];
    for(var i = 1; i < ar.length; i++){
        if(ar[i] > height){
            sum = 0;
            height = ar[i];
        }
        if(height == ar[i])
            sum++;
    }

    return sum;
}
```

<b>Time Complexity:</b> O(n)
iterating through n elements to determine the sum of the largest number

<center><h3>Time Conversion</h3></center>

***

<a href="https://www.hackerrank.com/challenges/time-conversion/problem" target="_blank">Hacker Rank Time Conversion</a>

<a href="https://github.com/perrinod/hacker-rank-solutions/tree/master/Algorithms/Warmup/Time%20Conversion" target="_blank"> Github repository </a>

<b>C++:</b>
```c++'
string timeConversion(string s) {
    string meridian = s.substr(s.size() - 2, s.size());
    string ms = s.substr(2, s.size() - 4);
    int hour = std::stoi(s.substr(0,2));

    hour = ((meridian == "AM" && hour == 12) ? 0 : (meridian == "PM" && hour != 12) ? (hour + 12) : hour);

    return ((hour < 12) ? ('0' + to_string(hour)) : to_string(hour)) + ms;
}
```

<b>C#:</b>
```c#
string timeConversion(string s) {
    string meridian = s.substr(s.size() - 2, s.size());
    string ms = s.substr(2, s.size() - 4);
    int hour = std::stoi(s.substr(0,2));

    hour = ((meridian == "AM" && hour == 12) ? 0 : (meridian == "PM" && hour != 12) ? (hour + 12) : hour);

    return ((hour < 12) ? ('0' + to_string(hour)) : to_string(hour)) + ms;
}
```

<b>Java:</b>
```java
static String timeConversion(String s) {
    String meridian = s.substring(s.length() - 2, s.length());
    String ms = s.substring(2, s.length() - 2);
    int hour = Integer.parseInt(s.substring(0,2));

    hour = ((meridian.equals("AM") && hour == 12) ? 0 : (meridian.equals("PM") && hour != 12) ? (hour + 12) : hour);


    return ((hour < 12) ? ("0" + hour) : hour) + ms;

}
```

<b>Time Complexity:</b> O(1)
no iterating just simple string modifications.

<b>Explanation of the ternary:</b>
The ternary operation that is occuring in the code may look confusing but it's equivalent to:
```
if(meridian == "AM && hour == 12)
    hour = 0;
else{
    if(meridian == PM && hour != 12)
        hour += 12;
    else
        hour;
}
```


