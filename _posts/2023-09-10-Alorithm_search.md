---
layout: post
title:  "Searching"
date:   2023-09-10
author: Easyfun
categories: Alorithm
cover:  "/assets/cover-thumb.png"
tags: [documentation,sample]
image: Rustacean.png
---

搜尋指的是從資料檔案找出滿足某些條件的記錄動作，用以搜尋的條件鍵稱為鍵值key)，如同排序所用的鍵值一樣。

而判斷一個搜尋法的好壞主要是從比較次數及搜尋時間來決定。

## 常見搜尋演算法

電腦搜尋資料的優點是快速，但是當資料量很大時，如何在最短時間內有效找到所需資料，是非常重要的。影響搜尋法時間長短主因包括演算法、資料儲存的方式和結構。搜尋法排序法一樣，以搜尋的過程中被搜尋的表格或資料是否異動來區分，分為靜態搜尋和動態搜尋。

* 靜態搜尋:資料在搜尋過程中，該搜尋資料不會增加、刪除或更新等行為，例如:符號表搜尋就屬於一種靜態搜尋。

* 動態搜尋:資料在搜尋中會經常性增加、刪除或更新。

搜尋的操作也算是典型的演算法，進行方式和所選擇的資料結構有很大關聯。



![alt text]({{ site.baseurl }}/assets/img/sorting.jpg "Profile Picture"){:.profile}


### 循序搜尋法(Linear search)

循序搜尋法又稱線性搜尋法，做法是將資料一筆一筆的循序逐次搜尋，不管資料順序為何，都得從頭到尾走一次。

這種做法的優點是檔案在搜尋前不需要任何的處理與排序，缺點是搜尋速度慢。如果資料沒有重複，找到資料就可終止，但最差就是為找到資料，就須作n次比較。例如電話簿的搜尋從姓氏找紀錄、預定位置的查找。

![alt text]({{ site.baseurl }}/assets/img/head.jpg "Profile Picture"){:.profile}

一筆一筆的搜尋資料

![alt text]({{ site.baseurl }}/assets/img/search.jpg "Profile Picture"){:.profile}


範例:
用氣泡排序法將下面數列做排序:

    [21, 20, 17, 1, 3, 2]

{% highlight rust %}

fn main() {
    // let data = vec![75, 50, 60, 20, 90, 40, 80];
    let data = vec![21, 20, 17, 1, 3, 2];
    let target = 17;

    let result = linear_search(&data, target);

    match result {
        Some(index) => println!("找到 {:?} 在第 {:?} 項.", target, index),
        None => println!("{} 未找到.", target),
    }
}

fn linear_search(data: &Vec<i32>, target: i32) -> Option<usize> {
    for (index, &value) in data.iter().enumerate() {
        if value == target {
            return Some(index);
        }
    }
    None
}

{% endhighlight %}

