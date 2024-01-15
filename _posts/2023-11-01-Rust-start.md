---
layout: post
title:  "Rust Test"
date:   2023-10-23
author: Easyfun
categories: Rust
tags: [documentation,sample]
image: cover-thumb.png
---

## Rust-Statement&Expression

Rust主要是分成敘述(Statement)和運算式(Expression)

敘述又分兩種:宣告敘述(Declaration statement)和運算式敘述(Expression statement)

宣告敘述用於宣告各種語言項(Item)，包含宣告變數、靜態變數、常數、結構、函數等，以及透過extern和use關鍵字引入套件和模組等。

運算式敘述指以分號結尾的運算式。求值結果將被捨棄，並總是回傳單元類型()。

{% highlight rust %}

fn main() {
    pub fn answer() -> {
        let a = 40;
        let b = 2;
        assert_eq!(sum(a, b), 42);
    }
    pub fn sum(a: i32, b: i32) -> i32 {
        a + b
    }
    answer();
}

{% endhighlight %}

函數answer本身沒有輸入參數，並且回傳值為單元類型()。單元類型擁有唯一的值，就是他本身，為了描述方便，將該值稱為單元值。
