---
layout: post
title:  "Stack&Queue"
date:   2023-09-16
author: Easyfun
categories: Alorithm
cover:  "/assets/cover-thumb.png"
tags: [documentation,sample]
image: Rustacean.png
---

## 陣列實作堆疊

堆疊和佇列都是屬於抽象資料型態

堆疊結構時常被用來解決問題，例:遞迴呼叫、副程式的呼叫。

佇列在電腦例:計算機模擬、CPU的工作排程。

### 陣列堆疊

以陣列結構來做堆疊的好處是製作與設計的演算法都很簡單，但因堆疊本身是變動的，則陣列大小並無法事先規劃宣告，太大浪費空間，太小則不夠用。

{% highlight rust %}

fn main() {
    let mut stack: Vec<i32> = Vec::new();
    if stack .isempty() {
        println!("堆疊為空");
    } else {
        println!("堆疊不為空");
    }
    
    println!("{}", sum);
}

{% endhighlight %}



範例:
用陣列結構來push或pop元素，最後輸出所有元素:


{% highlight rust %}

fn main() {

    const MAX_CAPACITY: usize = 100; 
    let mut stack: [i32; MAX_CAPACITY] = [0; MAX_CAPACITY]; 
    let mut top: usize = 0; 

    // 向堆栈中插入元素
    for i in 1..=10 {
        if top < MAX_CAPACITY {
            stack[top] = i;
            top += 1;o
            println!("成功插入元素 {} 到堆疊。", i);
        } else {
            println!("堆疊已满，無法插入元素 {}。", i);
        }
    }

    println!("從堆疊中彈出元素：");
    while top > 0 {
        top -= 1;
        let element = stack[top];
        println!("{}", element);
    }
}

{% endhighlight %}


## 鏈結串列實作堆疊

使用鏈結串列來做堆疊的優點是隨時可以動態改變串列長度，能有效利用記憶體資源，缺點是設計時演算法較為複雜。

{% highlight rust %}
// 定義一個鏈結節點
struct Node<T> {
    data: T,
    next: Option<Box<Node<T>>>,
}

impl<T> Node<T> {
    fn new(data: T) -> Self {
        Node { data, next: None }
    }
}

{% endhighlight %}

{% highlight rust %}
    // 定義堆疊結構Stack
    pub fn new() -> Self {
        Stack { top: None }
    }

{% endhighlight %}


範例:
用鏈結串列來push或pop元素，最後輸出所有元素:


{% highlight rust %}

// 定義堆疊结構
pub struct Stack<T> {
    top: Option<Box<Node<T>>>,
    size: usize,
}

impl<T> Stack<T> {
    pub fn new() -> Self {
        Stack { top: None, size: 0 }
    }

    pub fn push(&mut self, data: T) {
        if self.size < 100 {
            let mut new_node = Box::new(Node::new(data));
            new_node.next = self.top.take();
            self.top = Some(new_node);
            self.size += 1;
        } else {
            println!("堆疊已满，無法插入元素。");
        }
    }

    pub fn pop(&mut self) -> Option<T> {
        if let Some(mut node) = self.top.take() {
            self.top = node.next.take();
            self.size -= 1;
            Some(node.data)
        } else {
            None
        }
    }

    pub fn is_empty(&self) -> bool {
        self.size == 0
    }
}

fn main() {
    let mut stack = Stack::new();

    // 在堆疊中插入元素
    for i in 1..=10 {
        stack.push(i);
    }

    // 彈出堆疊中的元素
    while let Some(element) = stack.pop() {
        println!("彈出元素: {}", element);
    }
}

{% endhighlight %}

