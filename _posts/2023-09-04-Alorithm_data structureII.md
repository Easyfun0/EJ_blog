---
layout: post
title:  "資料結構II"
date:   2023-09-04
author: Easyfun
categories: Alorithm
tags: [documentation,sample]
image: cover-thumb.png
---

今天繼續介紹資料結構的常用的種類

### 鏈結串列(Linked List)

鏈結串列是由許多相同資料型態的項目，依照特定順序排列而成的線性串列，在電腦中位置是不連續、隨機的方式儲存，好處是資料的插入或刪除都方便。當有新資料加入就像系統要一塊記憶體空間，資料刪除後就把空間還給系統，不需要大量移動資料。

在動態配置記憶體空間時，最常使用的就是「單向鏈結串列」(Single Linked List)。一個單向鏈結串列節點是由兩個欄位，即資料欄及指標欄(鏈結欄位)組成，而指標欄將會指向下一個元素的記憶體所在位置。

像是網頁中的超連結是鏈結串列的概念，每個超連結都指向另一個網頁，你可以點擊這些連結在不同的網頁之間做導航。


在單向鏈結串列中第一個節點是

![alt text]({{ site.baseurl }}/assets/img/linked.jpg "Profile Picture"){:.profile}

他的優點是不需先知道資料型別大小，充分利用動態記憶體管理。但也因為動態配置記憶體因素會也一些缺點，空間使用過大、較差的CPU快取、不允許隨機存取。

### 堆疊(Stack)

堆疊是一堆相同資料型態的組合，所有的動作都在最上面進行，遵循後進先出(Last In First Out, LIFO)的特性。最後進去堆疊的元素首先被取出，就像碟盤子一樣，最後放在頂部的盤子最先被拿走。

遵守著

* 只能從堆疊的最上面存取資料

* 資料的存取符合「後進先出」的原則

![alt text]({{ site.baseurl }}/assets/img/stack.jpg "Profile Picture"){:.profile}

堆疊的基本運作:

| create | 建立一個空堆疊 |
| push | 存放頂端資料，並傳回新堆疊 |
| pop | 刪除頂端資料，並傳回新堆疊 |
| isEmpty | 判斷堆疊是否為空堆疊，是則回傳True，不是回傳False |
| full | 判斷堆疊是否已滿，是則回傳True，不是回傳False |

### 佇列(Queue)

佇列和堆疊都是一種有序串列，也屬於抽象資料型態，他所有加入與刪除的動作都發生在不同的兩端，遵循先進先出(First In First Out)的特性。

遵守著

* 資料存取「先進先出」的原則

* 擁有兩種基本動作:加入與刪除，且用front與back兩個指標來分別指向佇列的前端與尾端

![alt text]({{ site.baseurl }}/assets/img/queue.jpg "Profile Picture"){:.profile}


| create | 建立一個空佇列 |
| add | 將新資料加入佇列的尾端，傳回新佇列 |
| pop | 刪除佇列前端的資料，傳回新佇列 |
| front | 傳回佇列前端的值 |
| empty | 若佇列為空集合，傳回真，否則傳回值 |

