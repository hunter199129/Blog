---
title: '[CodeFights] challenge MakeSentance'
date: 2016-10-23 12:01:03
tags: [CodeFights, string, c++, c++11, first_post]
---
## 關於CodeFights

[CodeFights](https://codefights.com/)上有很多種不同的題目跟玩法，其中有一個就是challenge，其目的是要跟別人比看誰可以在最短字數內用相同語言實做出其需求功能。當中當然不乏非常神奇的解法，所以我想說把這些解法紀錄下來或是研究他們的解法，看能不能學到些什麼，而且有些題目其實很泛用，所以以後可能也會用到。

## Description

Your boss likes to prank you by sending you the sentence he wants to say as a list of words, with each word given as a list of characters. You are already tired of a such low level prank, and decide to write a program that would turn the sentences your boss sends to you into a readable form.

Remember to put a space between two words and put a period in the end of the sentence. Your boss is weird but kind enough not to send you sentences with other punctuation.

Note: if you want an additional challenge, try solving this challenge locally without using STL and C Runtime Library with the only exception being malloc(). Therefore the function you should the implement has the following format:

char* MakeSentence(char* words[])
{
    // function body
}

Example For words = [["G","o","o","d"],
         ["m","o","r","n","i","n","g"]]

the output should be makeSentence(words) = "Good morning.".

ref: <https://codefights.com/challenge/XLwudJsZ6JQR2a9uo>

## My solution
```
    string makeSentence(vector<vector<char>> w) {
        string s;
        for(int i=0; i<w.size(); ++i){
            for(int j=0; j<=w[i][j]!='\0'; ++j){
                s.push_back(w[i][j]);
            }
            char t = i==w.size()-1 ? '.' : ' ';
            s.push_back(t);
        }
        return s;
    }
```
## Shortest solution from others
```
    string s, makeSentence(auto W) {
      for (w:W) {
          s+=32;
          for (c:w) s+=c;
      }
      return {s+".",1};
    }
```
看起來要求是只要有回傳需要的東西跟函數名稱相同就好，所以函數的參數名稱也可以任意修改。

不過C++11真是神奇的語言...，回傳的東西也可以命名，然後auto變數高興怎麼宣到就怎麼宣告，for_each迴圈也改成```for (elements: array)```這種簡單的形式，看起來也不比go複雜了。
其中```s+=32```32是空白的ascii code。最後因為多一個空白，所以直接從s[1]開始回傳，省掉判斷式。
