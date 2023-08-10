---
layout: post
title:  "Variable"
date:   2023-03-08
author: Easyfun
categories: Rust
tags: [documentation,sample]
image: Rustacean.png
---

## 變數與可變性

Rust推動你能充分利用Rust提供的安全性和簡易並行性來寫程式的方法之一。

當一個變數是不可變的，只要有數值綁定在一個名字上，你就無法改變其值。


## 變數綁定

在Rust中我們會:

let a = "hello" ，同時稱作變數綁定

## 變數可變性
{% highlight rust %}

fn main() {
    let x = 1;
    println!("The value of x is: {}", x);
    x = 2;
    println!("The value of x is: {}", x);
}

{% endhighlight %}

執行後
```
error[E0384]: cannot assign twice to immutable variable `x`
 --> /Users/huangyingjie/Rprojects/new_demo/src/main.rs:4:5
  |
2 |     let x = 1;
  |         -
  |         |
  |         first assignment to `x`
  |         help: consider making this binding mutable: `mut x`
3 |     println!("The value of x is: {}", x);
4 |     x = 2;
  |     ^^^^^ cannot assign twice to immutable variable

error: aborting due to previous error
```

錯的原因是cannot assign twice to immutable x(無法對不可變的變數進行重複賦值)，因為我們想對不可變的x變數再次賦值。

在Rust中，可變性很簡單，只要在變數前加一個mut就可。

為了讓變數為可變，可改為:

{% highlight Rust %}

fn main(){
  let mut x = 1;
  printl!("The value of x is: {}",x);
  x = 2;
  println!("The value of x is: {}", x);
}

{% endhighlight %}

```
:!'/var/folders/mg/1r752ql131v0n4nfn43j2xdc0000gn/T/nvim.huangyingjie/mKXpmr/1/main'
The value of x is: 1
The value of x is: 2
```
### 使用下底線開頭忽略未使用變數

當你創建了一個變數卻未使用它，Rust會給你警告。但有時創建一個不會被使用的變數是有用的，比如正在設計一個原形或剛開始一個項目。這時你希望Rust不要警告未使用的變數，為此可以用下底線作為變數的開頭:

{% highlight rust %}

fn main(){
  let _x = 1;
  let y = 2;
}

{% endhighlight %}

> cargo run
  warning: unused variable: `y`
   --> src/main.rs:3:9
    |
  3 |     let y = 2;
    |         ^ help: if this is intentional, prefix it with an underscore: `_y`
    |
    = note: `#[warn(unused_variables)]` on by default

### 變數解構

在Rust 1.59後，我們可在賦值句的左式中使用元組,切片和結構體模式。
{% highlight rust %}

struct Struct {
  e:i32
}
fn main(){
  let (a,b,c,d,e);

  (a, b) = (1, 2);
  [c, .., d, _] = [1, 2, 3, 4, 5];
  Struct {e, .. } = Struct {e: 5};

  assert_eq!([1, 2, 3, 4, 5], [a, b, c, d, e]);
}

{% endhighlight %}

這方法跟之前的let保持了一致性，但是let會重新綁定，而這裡只是對之前綁定的變數進行再賦值。

需注意的是，使用 += 的賦值語句還不支持解構式賦值。

### 變數和常數之間的差異

常數(constant) 與不可變數一樣，常數也是綁定到一個常數名且不允許更改的值，但是常數和變數還是存在一些差異:

🧪 常數不允許使用mut。常數不僅默認不可變，而且自始至終不可變，因為在編譯完成後已經確定它的值。

🧪 常數使用const關鍵字而不是let，並且值的類型必須標註。

下面範例，常數名為MAX_POINTS，值設置為100000。(Rust常數的命名約定是全部都使用大寫，並使用下底線分隔單詞，另對數字字面量可插入下底線以提高可讀性):

  const MAX_POINTS: u43 = 100_000;

常數可以再任意作用域內聲明，包括全局作用域，在聲明的作用域內，常數在程式運行整個過程都有效。對於需要再多疊代碼共享一個不可變的值時非常有用。

### 變數遮蔽(shadowing)

Rust允許聲明相同的變數名，在後面聲明的變數或遮蔽調前面的聲明:

{% highlight rust %}

fn main(){
  let x = 1;
  // 在main函數的作用域內對之前的x進行遮蔽
  let x = x + 1;

  {
    // 在當前的括號作用域內，對之前的x進行遮蔽
    let x = x * 2;
    println!("The value of x in the inner scope isL {}", x);
  }
  println!("The value of x is: {}", x);
}

{% endhighlight %}

這是先將值5綁定到x，通過重複使用let x = 來遮蔽之前的x，並取原來的值加上1，所以x的值變成了2。

The value of x in the inner scope is: 4
The value of x is: 2

這和mut變數的使用是不同的，第二個let生成了完全不同的新變數，兩個變數只是剛好有同樣的名稱，mut聲明的變數，可以修改同一個地址上的值，並不會發生內存對象的再次分配，性能要更好。

變數遮蔽的用處在于，如果你在某個作用域內無需再使用之前的變數(再被遮蔽後，無法在訪問到之前的同名變數)，就可以重複的使用變數名字。

假設有一個程式要統計一個空格字符串得空格數量:

  // 字符串類型
  let spaces = "  ";
  // usize數值類型
  let spaces = spaces.len();

這個架構是允許的，因為第一個spaces變數是一個字符串類型，第二個spaces變數是一個全新的變數且和第一個具有相同的變數名，且是一個數值類型。

如果不用let:

    let mut spaces = "  ";
    spaces = spaces.len();


    error[E0308]: mismatched types
     --> src/main.rs:3:14
      |
    3 |     spaces = spaces.len();
      |              ^^^^^^^^^^^^ expected `&str`, found `usize`

    error: aborting due to previous error

顯然Rust對於類型要求很嚴格，不允許將整數類型usize賦值給字符串類型。usize是一種CPU相關的整數類型。


