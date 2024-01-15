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

## 單向鏈結串列(Singly linked list)

產生一個鏈結的方式，要先制定一個結構資料型態，然後在結構中定義其資料型態。


### 鏈結串列節點定義

這邊會拿Rust的例子，實現鏈結串例要先連結串列的節點。

{% highlight rust %}

struct Node<T> {
    data: T,
    next: Option<Bix<Node<T>>>
}

{% endhighlight %}

這邊定義一個Node節點，data欄位使用了泛型型別T用於鏈結串列點的資料。next使用了Option列舉，如果該節點沒有下一個節點時，next是可空的，因為Rust沒有空值(null,nil)，而是提供Option的解決方法，如果該鏈結串咧節點的下個節點為空，則next取值為Option::None。

### 鏈結串列的定義

定義完鏈結串列後，再定義一個結構體LinkedList來表示鏈結串列，會封裝一些鏈結串列的基本操作。結構體中只需需一個鏈結串列頭節點的欄位head，型別為Option<Box<Node<T>>>。

{% highlight rust %}

// 單鏈表節點
struct Node<T> {
    data: T,
    next: Option<Box<Node<T>>>,
}

{% endhighlight %}


![alt text]({{ site.baseurl }}/assets/img/Node.jpg "Profile Picture"){:.profile}

### 基本操作

串列的基本操作:

* new: 初始化一個空串列

* push_front: 新增節點到開頭的位置

* insert_after: 在指定索引位置後插入一個新節點

* remove: 移除任意索引下的節點

* clear: 清除所有節點

* is_empty: 檢查串列是否沒有任何節點

* reverse: 反轉整個串列(head變成tail)

### 初始化與清除資料

清除就是將self指向新的串列。

{% highlight rust %}

impl<T> LinkedList<T> {
    pub fn new() -> Self {
        Self { head: None }
    }
}

{% endhighlight %}

### 增刪首個節點

單向鏈結串列在第一個節點前增加新節點，或是刪除第一個節點，都可在常數時間完成。

1.建立新節點，並把新節點next指標指向串列第一個節點。

2.把串列的head指向新建立的節點。


    pub fn push_front(&mut self, data: T) {
        let next = self.head.take();
        self.head = Some(Box::new(Node {data, next})));
    }
    // 釋放LinkList對第一個節點的所有權
    // 建立一個新節點，並將原本第一個節點所有權給新節點，再將新節點所有權轉移到串列本身

刪除第一個節點pop_front:先取得第一個節點的所有權，再將head指向第一個節點Node.next下一個節點，再返回第一個節點的資料給呼叫端。

    pub fn pop_front(&mut self) -> Option<T> {
        let head = self.head.take()
        self.head = head.next;
        Some(head.data)
    }
    // 取得第一個元素所有權，若無首個元素，表示串列為空，用?回傳None
    // 將head指向下一個節點
    // 返回即將刪除節點資料


### 插入刪除任意節點

鏈結串列不支援隨機存取，無法透過索引在常數時間內取得資料，每次的搜尋都只能從head開始。

實作插入insert_after:

    pub fn insert_after(&mut self, pos: usize, data: T) -> Result<(), usize> {
        let mut curr = &mut self.head;
        let mut pos_ = pos;

        while pos_ > 0 {
            curr = match curr.as_mut() {
                Some(node) => &mut node.next,
                None => return Err(pos - pos_),
            };
            pos_ -= 1;
        }

        match curr.take() {
            Some(mut node) => {
                let new_node = Box::new(Node {
                    data,
                    next: node.next,
                });
                node.next = Some(new_node);
                *curr = Some(node);
            }
            None => return Err(pos - pos_),
        }
        OK(())
    }
    // 先找到對應索引值的節點A，若找不到則回傳這個串列的長度
    // 先取得節點A的所有權，才能修改他的值
    // 建立新節點B，同時將B的next指向A的後一個節點
    // 將新節點B做為節點A後一個節點next
    // 把修改過的節點A，重新賦值給指向節點A的指標curr


實作刪除remove:

    pub fn remove(&mut self, pos: usize) -> Option<T> {
        let mut curr = &mut self.head;
        let mut pos = posl;

        while pos > 0 {
            curr = &mut curr.as_mut()?.next;
            pos -= 1;
        }

        let node - curr.take()?;
        *curr = node.next;
        Some()node.data
    }
    // 先找到對應索引值的節點A，若找不到則回傳None
    // 先取得節點A所有權，才能修改它的值
    // 把節點A的後一個節點B賦值給原本指向節點A的指標curr
    // 回傳節點A的值


### 單向鏈結串反轉

在鏈結串列中的節點特性是知道下一個節點位置，卻無法知道它上一個節點的位置。

實作反轉reverse:
 
{% highlight rust %}

    pub fn reverse(&mut self){
        let mut prev = None;
        let mut curr = self.head.take();
        while let Some(mut node) = curr {
            let next = node.next;
            node.next = prev.take();
            prev = Some(node);
            curr = next;
        }
        self.head = prve.take();
    }
    // 建立一個暫時變數prev，儲存疊代時的前一個節點
    // 從串列head取得第一個節點所有權
    // 依序疊代整個串列
    //    1.將節點A的後一個節點B暫存起來
    //    2.節點A的next指向暫存在變數prev的節點P
    //    3.節點A暫存在變數prev內，保留到下一個疊代使用
    //    4.將節點B儲存在變數cur內。prev:節點A,A的next指向P，curr:節點B,B的next指向A
    // 最後一次疊代時，變數prev會儲存原始串列末端節點，這時轉移所有權到head，完成反轉
    
// 單鏈表
struct LinkedList<T> {
    head: Option<Box<Node<T>>>,
}

impl<t> Node<T> {
    fn new (data: T) -> Self {
        Self { data: data, next: None }
    }
}

impl<T> LinkedList<T> {
    fn new() -> Self {
        Self { head: None }
    }
}

{% endhighlight %}

Node的new函式用來使用給定的data資料建立一個節點



