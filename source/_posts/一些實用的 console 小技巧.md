---
title: '一些實用的 console 小技巧'
date: 2018-12-12 09:01:00
tags: [javascript, browser]
categories: Coding
---

Console 是網頁開發不可或缺的工具，但我們對 console 的用法大多都只用了 `console.log`

這邊記錄一些讓 console 更實用的小技巧，以下的 code 就直接打開 F12 試看看吧！

<!--More-->

## console.group & console.groupEnd

- Usage: 將 `console.log` 分層

      console.group('lev-1');
      console.log('level 1 now');
      console.group('lev-2');
      console.log('level 2 now');
      console.groupEnd();
      console.log('return to level 1');
      console.groupEnd();
      console.log('return to level 0');

## console.trace

- Usage: 觀察現在呼叫的 function

      function foo() {
        function bar() {
          console.trace();
        }
        bar();
      }
      foo();

## console.time

- Usage: 計算時間

      console.time('myTimer'); // function 內不可加名稱
      for(i=0; i<100; i++);
      console.timeEnd('myTimer');

## console.clear

- Usage: 清空 console

      console.clear();

## console.memory

- Usage: 觀察 JS 使用 memory 的狀況

      console.memory;

## console.table

- Usage: 用 table 的方式呈現 array

- 2-D array

      var people = [["John", "Smith"], ["Jane", "Doe"], ["Emily", "Jones"]]
      console.table(people);

- Array of objects
  
      function Person(firstName, lastName) {
        this.firstName = firstName;
        this.lastName = lastName;
      }

      var john = new Person("John", "Smith");
      var jane = new Person("Jane", "Doe");
      var emily = new Person("Emily", "Jones");

      console.table([john, jane, emily]);

- Objects of properties

      var family = {};

      family.mother = new Person("Jane", "Smith");
      family.father = new Person("John", "Smith");
      family.daughter = new Person("Emily", "Smith");

      console.table(family);
