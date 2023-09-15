---
layout: post
title:  "Linked List"
date:   2023-09-14
author: Easyfun
categories: Alorithm
cover:  "/assets/cover-thumb.png"
tags: [documentation,sample]
image: Rustacean.png
---

## 陣列與鏈結串列

在第四,第五天的時候有講到陣列和鏈結串列，它們都是相當重要的結構化資料型態(Structured Data Type)，也都是線性串列的應用，線性串列也應用在電腦中的資料儲存結構，按照記憶體儲存的方式，可區分為:

* 靜態資料結構(Static Data Structure)

陣列型態是靜態資料結構，是將有序串列的資料使用連續記憶空間來儲存。它的記憶體配置是在編譯時，就必須配置給相關的變數，因此在建立時，必須事先宣告最大可能的固定空間，容易造成記憶體的浪費，優點是設計時簡單及讀取與修改串列中任何一元素的時間都固定，缺點是刪除或加入資料時，需要移動大量資料。

* 動態資料結構(Sdynamic data structure)

串結鏈結使用不連續記憶空間來儲存，優點是資料的插入或刪除都很方便，不需要移動大量資料。記憶體空間是在執行時才發生，所以不用事先宣告，因此能充分節省記憶體。缺點是在設計資料結構時較麻煩，在搜尋資料時，也無法像靜態資料一樣可以隨機讀取資料，必須透過循序方法找到該資料為止。

### 矩陣

對於矩陣m * n矩陣的形式，可以用電腦中A(m,n)二維陣列來描述。許多的矩陣運算與應用都可以用二維陣列來解決，如下矩陣。

![alt text]({{ site.baseurl }}/assets/img/mn.jpg "Profile Picture"){:.profile}

#### 矩陣相加演算

矩陣的相加演算是相加的兩矩陣列數與行數都必須相等，而相加後矩陣的列數與行數也是相同，亦即兩者的行數與列數都相等，例:A_m * n +B_m * n = C_m * n。

範例:
兩個二維陣列相加:

        5,4,8,7,1

{% highlight rust %}

use ndarray::arr2;

fn main() {
    let a = arr2(&[[1, 3, 5],
                  [7, 9, 11],
                  [13, 17, 19]]);

    let b = arr2(&[[2, 4, 6],
                  [8, 10, 12],
                  [14, 16, 18]]);

    let sum = &a + &b;
    println!("{}", sum);
}

{% endhighlight %}

這個範例使用了Rust中的'ndarray'庫來進行操作。



## 矩陣相乘

假設要矩陣A與B相乘，是有限制的。必須符合A為一個m * n的矩陣，B為一個n * p的矩陣，對A * B之後結果為一個m * p的矩陣C。

![alt text]({{ site.baseurl }}/assets/img/mn.jpg "Profile Picture"){:.profile}

    C_11 = a_11 * b_11 + a_12 * b_21 + ...... + a_1n * b_n1

    C_1p = a_11 * b_1p + a_12 * b_2p + ...... + a_1n * b_np

    C_mp = a_m1 * b_1p + a_m2 * b_2p + ...... + a_mn * b_np


範例:
兩個矩陣相乘:

{% highlight rust %}

use ndarray::arr2;

fn main() {
    
    let a = arr2(&[[1, 2, 3],
                   [4, 5, 6]]);

    let b = arr2(&[[6, 3],
                   [5, 2],
                   [4, 1]]);

    println!("{}", sum);
}

{% endhighlight %}

ndarray提供了dot的方法，用來執行矩陣乘法操作。

#### 轉置矩陣

轉置矩陣就是把原矩陣的行座標元素與列座標元素戶調換，假設A^t為A的轉置矩陣，則有A^t[j,i]=A[i,j]。

![alt text]({{ site.baseurl }}/assets/img/mn4.jpg "Profile Picture"){:.profile}

範例:
4X4二維陣列的轉置矩陣:

{% highlight rust %}

use ndarray::Array2;

fn main() {
    // 建立一個4X4的二維
    let mut matrix = Array2::<f64>::zeros((4, 4));

    // 初始化二維的元素
    for i in 0..4 {
        for j in 0..4 {
            matrix[[i, j]] = (i * 4 + j) as f64;
        }
    }

    println!("矩陣A：\n{}", matrix);

    // 轉置
    let trans = matrix.t();

    println!("矩陣A^t：\n{}", trans);
}

{% endhighlight %}


