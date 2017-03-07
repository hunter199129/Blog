---
title: '[CodeFights] challenge BaseAdd'
date: 2016-10-23 13:04:22
tags: [CodeFights, c++, c++11, math]
---
## Description

You are given two non-negative integers, number1 and number2, given in bases base1 and base2, respectively. Each base can be in range from Binary (2) to Hexatridecimal (36).

Your task is to sum the given numbers and return them in one of the given bases. Here's how the base of the returned value is determined:

    if number1 ≠ number2, the base of the largest number should be chosen;
    if number1 = number2, the largest base from the given should be chosen.

Note, that characters used in bases greater than 10 are given in the lowercase.

For your convenience, you can check out this base number converter or this converter.

Example

    BaseAdd("11", 2, "10", 10) = "13".

    11 = 3, thus the sum equals 3 + 10 = 13. Since 1 < 10, base 10 should be used in the answer.

    BaseAdd("11111100000", 2, "7e0", 16) = "fc0".

    11111100000 = 2016, and 7e0 = 2016 as well.
    Thus the sum equals 2016 + 2016 = 4032.
    number1 = number2, the largest base should be used in the answer, which is 16.
    So, the answer is 4032 = fc0.

ref: https://codefights.com/challenge/rXBQajNqcNeqx32Mz
## My solution
```
typedef long l;
typedef int i;
typedef string s;
s BaseAdd(s s1, i b1, s s2, i b2) {

    l n1 = Cb(s1, b1);
    l n2 = Cb(s2, b2);

    i b;
    if(n1==n2){
        b = b1>b2 ? b1: b2;
    }else{
        b = n1>n2 ? b1: b2;
    }
    return Cf(n1+n2, b);
}

s t = "0123456789abcdefghijklmnopqrstuvwxyz";

l Cb(s n, i b){
    l r = 0, f;
    for(i i=n.length()-1, j=0; i>=0; --i, ++j){
        if(isalpha(n[i])) f=n[i]-'a'+10;
        else f=n[i]-'0';
        r += f * pow(b, j);
    }
    return r;
}

s Cf(l n, i b){
    s r;
    i i = 0;
    while(n>pow(b, i)) i++;
    i--;
    if(n<b) r.push_back(t[n]);
    while(i>=0){
        l x = n/pow(b,i);
        r.push_back(t[x]);
        n-=x*pow(b,i--);
    }

    return r;
}
```
# Shortest solution from others
```
string s, BaseAdd(auto a, int x, auto b, int y) {
    long n=stol(a,0,x),B=stol(b,0,y),d=n>B?x:y;
    for(n+=B;  n; n/=d)
        a="0W"[9<n%d]+n%d ,s=a+ s;
    return s;
}
```
[stol](http://www.cplusplus.com/reference/string/stol/)也是C++11裡特有的function，還可以直接換底...。`for`迴圈裡的條件也不用寫n>=0，直接寫n會比較簡潔。
W的ascii是87，而小寫的a是97，所以`a="0W"[9<n%d]+n%d`的意思就很清楚了。
越看越覺得寫得出這種code還真是可怕...。
