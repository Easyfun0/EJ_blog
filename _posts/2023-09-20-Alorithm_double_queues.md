---
layout: post
title:  "Double Queues"
date:   2023-09-20
author: Easyfun
categories: Alorithm
cover:  "/assets/cover-thumb.png"
tags: [documentation,sample]
image: Rustacean.png
---

## 雙向佇列(double-ended queue)

雙向佇列是一般話的佇列或堆疊。跟佇列的「先進先出FIFO」，和堆疊「後進後出LIFO」比起，雙向佇列可以從最前端或最尾端任意方向，在常數時間複雜度內增刪元素，更為方便。

### 基本操作

雙向佇列有符合定義的基本操作:

🚀 new:初始化一個容器。

🚀 push_front: 在容器最前端新增一個元素。

🚀 push_back: 在容器最尾端新增一個元素。

🚀 pop_front: 移除在容器最前端的元素。

🚀 pop_back: 移除在容器最尾端的元素。

為了提升方便性，也提供一些方法:

🛸 front: 查看容器最前端的元素。

🛸 back: 查看容器最尾端的元素。

🛸 len: 檢查容器內的元素數目。

🛸 is_empty: 檢查容器是否沒有任何元素。

🛸 iter、iter_mut、into_iter: 產生一個疊代容器內所有元素的疊代器。

需要精準控制記憶體，提供內部方法:

🛰 is_full: 檢查底層環形緩衝區是否滿載。

🛰 try_grow: 嘗試動態增加底層儲存空間。

🛰 wrapping_add、wrapping_sub: 確保邏輯索引的增減正確映射到底層實際索引位址。


雙向佇列就是允許兩端中的任意具備刪除或加入，無論左右兩端的佇列，前端跟尾端都是朝佇列中央來移動。一班的應用，雙向佇列分為資料從一端加入，但可從兩端取出，另一種是可由兩端加入，但由一端取出。

![alt text]({{ site.baseurl }}/assets/img/front.jpg "Profile Picture"){:.profile}

{% highlight rust %}

struct Deque {
    head: usize,
    tail: usize,
    storage: SomeStorageType,
}

{% endhighlight %}


範例:

以鏈結串列來設計雙向佇列，從任意端輸入但從後端取出:

{% highlight rust %}

struct RestrictedDeque<T> {
    list: LinkedList<T>,
}

impl<T> RestrictedDeque<T> {
    fn new() -> Self {
        RestrictedDeque {
            list: LinkedList::new(),
        }
    }

    // 從（前端）加入
    fn push(&mut self, data: T) {
        self.list.push_front(data);
    }

    // 從前端取出
    fn pop_front(&mut self) -> Option<T> {
        self.list.pop_front()
    }

    // 從後端取出数
    fn pop_back(&mut self) -> Option<T> {
        self.list.pop_back()
    }
}

{% endhighlight %}



## 優先佇列(Priority Queue)

優先佇列不必遵守佇列特性-FIFO(先進先出)的有序串列，其中每一個元素都賦予優先權，加入元素時可任意輸出，但有最高優先權者則最先輸出。

假設有4個行程A1,A2,A3,A4，在短時間先後到達等待佇列，每個行程執行時間如:

| 行程名稱 | 所需時間 | 
| ------ | ------- |
| A1 | 50 |
| A2 | 10 |
| A3 | 25 |
| A4 | 15 |

這邊給4個行程優先權A1:1,A2:2,A3:3,A4:4(假設數值越小其優先權越低)。

![alt text]({{ site.baseurl }}/assets/img/prior.jpg "Profile Picture"){:.profile}


範例:


{% highlight rust %}

use std::collections::BinaryHeap;
use std::cmp::Reverse;

fn main() {
    // 建立一個空的佇列
    let mut priority_queue: BinaryHeap<Reverse<i32>> = BinaryHeap::new();

    // 新增元素到佇列
    priority_queue.push(Reverse(5));
    priority_queue.push(Reverse(2));
    priority_queue.push(Reverse(7));
    priority_queue.push(Reverse(1));

    // 從優先陣列中彈出元素，按照優先排序
    while let Some(Reverse(value)) = priority_queue.pop() {
        println!("彈出: {}", value);
    }
}

{% endhighlight %}

這範例用Rust的'BinaryHeap'來實現，BinaryHeap默許最大值在前，輸出結果在用Reverse來做包裝。

