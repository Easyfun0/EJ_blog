---
layout: post
title:  "演算法簡介"
date:   2023-08-17
author: Easyfun
categories: Alorithm
tags: [documentation,sample]
image: cover-thumb.png
---


## 常見演算法簡介

這邊要來介紹比較常用的演算法，來暸解不同演算法觀念以及技巧。

### 分治法(Divide and conquer)

分治法核心在將一個難以直接解決大問題依照相同的概念，分割成兩個或更多的子問題，以變各個擊破。問題越小越容易直接求解，
由於分割問題也是遇到大問題時的解決方式，可以將小問題規模不斷縮小，直到這些子問題簡單到可以解決，最後將各子問題的解合併得到原問題的解答。


舉例:如果有8張很難的圖，我們可以分成二組各四幅畫來完成，若還是覺得太複雜，繼續分成四組，每組各兩幅畫來完成，利用相同模式反覆分割問題，這就是最簡單的分治法核心精神。

![alt text]({{ site.baseurl }}/assets/img/divide_pic.jpg "Profile Picture"){:.profile}

### 遞迴法

遞迴法跟分治法都是將一個複雜的演算法問題規模變得越來越小，最終使子問題容易求解。對於我們實作「函數 」不單只是能夠被其他函數呼叫（或引用)的程式單元，在某些語言還提供了自身引用的功能，這就是「遞迴 」。
遞迴的定義是，假如一個函式或副程式，是由自身所定義或呼叫的，就稱為遞迴「Recursion 」，他至少要定義2種條件，包括一個可以反覆執行的遞迴過程，與一個跳出執行的出口。

這邊用費伯那序列來求解:
當一序列的第零項是0，第一項是1，其他每一個項列中項目的值是由其本身前面兩項的值相加所得。從費伯那序列的定義，也可以把它設計轉成遞迴形式:


{% highlight rust %}

fn factorial(n: i32) -> i32 {
  if n <= 0 {
        return 0;
  } else if n ==1{
            return 1;
} else {
    return factorial (n-1) + factorial(n-2);
 }
}

{% endhighlight %}

遞迴是利用階層函數來說明遞迴式運作，在實作遞迴時，會應用到堆疊的資料結構概念，所謂堆疊(Stack)是一群相同資料型態的組合，所有的動作均在頂端進行，具「後進後出 」(Last In,First Out,LIFO)的特性。


用費伯那序列來做範例:



{% highlight rust %}

fn fibonacci(n: i32) -> i32 {
  if n <= 1 {
     n
  } else{
    fibonacci (n-1) + fibonacci(n-2);
 }
}

fn main() {
  for i in 0..11{
    println!("fib({i})", fibonacci(i));
  }
}

{% endhighlight %}

![alt text]({{ site.baseurl }}/assets/img/fib.jpg "Profile Picture"){:.profile}


### 貪心法

貪心法(Greed Method)稱為貪婪演算法，方法是從某一起點開始，在每一個解決步驟時用貪心原則，採取在當前狀態下最有利或最優化的選擇，不斷改進該解答，持續在
每一步驟中選擇最佳方法，並且逐步逼近給定的目標，當達到某一步驟不譨再繼續前進時，演算法停止，已盡可能快地求得更好解。

貪心法是把求解的問題分成若干個子問題，不過不能保證求得的最後解是最佳方法。能滿足某些約束條件的可行解範圍，不過在有些問題卻可以得到最佳解。
經常用在求圖形的最小生成樹(MST)、最短路徑與霍哈夫曼編碼等。

範例:

1.選擇問題:

以下圖中的5走到3，最短路徑該怎麼走？以貪心法來說，先走到1接著選擇走道2，最後從2走到3，這樣距離27，但會發現才5直接到3是20才是最短距離，在這情況下就沒法從貪心法規則下找到最佳解答。

![alt text]({{ site.baseurl }}/assets/img/Greed.jpg "Profile Picture"){:.profile}


