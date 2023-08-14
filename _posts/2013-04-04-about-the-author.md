---
layout: post
title:  "Functions"
date:   2023-03-14
author: Easyfun
categories: Rust
tags: [documentation,sample]
image: Rustacean.png
---


## 函式

main函式是入口點，fn關鍵字來宣告新的函式。

Rust是使用snake case式作為函式與變數名稱的慣例。所有字母都是小寫，並用底線區隔單字。

{% highlight rust %}

fn main() {
  println!("Hiiii");

  another_function();
}

fn another)function() {
  println!("HIIIII");
}

{% endhighlight %}

Rust定義函式是從fn開始，再加上函式名稱和一組括號，大括號告訴編譯器函式本體開始與結束。

程式碼會按照main函式中的順序。首先Hiiii會先顯示出來，再來才會呼叫another_function並印出他的訊息。

## 參數

我們可定義函式成擁有參數(parameters)的，這是函式簽名(signatures)中特殊的變數。

如果要定義多個參數時，會用逗號區隔開來:

{% highlight rust %}

fn main() {
  print_labeled_measurement(5, 'h');
}

fn print_labbeled_measurement(value: i32, unit_label: char) {
  println!("測量值為: {value}{unit_label}");
}

{% endhighlight %}

我們呼叫函數時，將5給了value且將'h'給了unit_label，程式輸出就會包含這些數值。

### 陳述式與表達式

函式本體是由一系列的陳述式(statements)並在最後可以選擇加上表達式(expression)來組成。

🧿 陳述式(Statements)是進行一些動作的指令，且不回傳任何數值。

🧿 表達式(Expressions)式計算並產生數值。

    fn main() {
      let y = 6;
    }

此函式定義也是陳述式，整個範例就是一個陳述式。

陳述式不會回傳數值，因此無法將let陳述式賦值給其他變數。

     fn main() {
      let x = (let y = 6);
     }

{% highlight rust %}

error: expected expression, found `let` statement
 --> /Users/huangyingjie/Rprojects/function/src/main.rs:3:16
  |
3 |       let x = (let y = 6);
  |                ^^^

error: expected expression, found statement (`let`)
 --> /Users/huangyingjie/Rprojects/function/src/main.rs:3:16
  |
3 |       let x = (let y = 6);
  |                ^^^^^^^^^
  |
  = note: variable declaration using `let` is a statement

error[E0658]: `let` expressions in this position are unstable
 --> /Users/huangyingjie/Rprojects/function/src/main.rs:3:16
  |
3 |       let x = (let y = 6);
  |                ^^^^^^^^^
  |
  = note: see issue #53667 <https://github.com/rust-lang/rust/issues/53667> for more information

warning: unnecessary parentheses around assigned value
 --> /Users/huangyingjie/Rprojects/function/src/main.rs:3:15
  |
3 |       let x = (let y = 6);
  |               ^         ^
  |
  = note: `#[warn(unused_parens)]` on by default
help: remove these parentheses
  |
3 -       let x = (let y = 6);
3 +       let x = let y = 6;
  |

error: aborting due to 3 previous errors; 1 warning emitted

For more information about this error, try `rustc --explain E0658`.

{% endhighlight %}

let y = 6陳述式不回傳數值，所以x得不到任何數值。像是C或Ruby，他們的賦值仍能回傳所得到的值。在那些語言你可以==x = y = 6==同時讓x與y都取得6但在Rust就不行。

我們用{}產生的作用域也是表達式。

{% highlight rust %}

fn main() {
  let x = 5;

  let y = {
    let x = 3;
    x + 1
  };

  println!("y的數值為: {y}");
}

{% endhighlight %}

此表達式:

    {
      let x = 3;
      x + 1
    }

就是一個會回傳4的區塊，此值再用let陳述式賦值給y。x + 1這行沒有加上分號，它和目前看到的寫法有點不同，因為表達式結尾不會加上分號。在此表達式機上分號，他就不會回傳數值。

### 函數回傳值

函式可以回傳數值給呼叫他們的程式碼，我們不會為回傳值命名，但我們必須用箭頭(->)來宣告他們的型別。在Rust中，回傳值就是函式本體最後一行的表達式。可以用return關鍵字加上一個數值來提早回傳函式，但多數函式都能用最後一行的表達式作為數值回傳。

    fn five() -> i32 {
      5
    }

    fn main() {
    let x = five()

    println!("x的數值為: {x}");
    }

在five函式中沒有任何函式呼叫,巨集甚至是let陳述式，只有一個5。在Rust中是合理的函式。函式的回傳型別也有指名，就是-> i32。

five中的5就是函式的回傳值，這就是為何回傳型別式i32。let x = five();顯示了我們用函式的回傳值作為變數的初始值。因為函式five回傳5，跟以下相同:

    let x = 5;

five函式沒有參數但有定義回傳值的型別。所以函式本體只需有一個5就好，不需加上分號，這就能當作表達式回傳我們要的值。

{% highlight rust %}

fn main() {
  let x = plus_one(5);

  println!("x的數值為: {x}");
}

fn plus_one(x: i32) -> i32 {
  x + 1
}

{% endhighlight %}

此程式會顯示==x的數值為6==，如果我們在最後一行x + 1加上分號的話，會將它從表達式變為陳述式。

{% highlight rust %}

fn main() {
  let x = plus_one(5);

  println!("x的數值為: {x}");
}

fn plus_one(x: i32) -> i32 {
  x + 1;
}

{% endhighlight%}

會跳此錯誤

{% highlight rust %}

error[E0308]: mismatched types
 --> /Users/huangyingjie/Rprojects/function/src/main.rs:7:24
  |
7 | fn plus_one(x: i32) -> i32 {
  |    --------            ^^^ expected `i32`, found `()`
  |    |
  |    implicitly returns `()` as its body has no tail or `return` expression
8 |     x + 1;
  |          - help: remove this semicolon to return this value

error: aborting due to previous error

For more information about this error, try `rustc --explain E0308`.

{% endhighlight %}

此錯誤訊息mismatched types告訴此問題。plus_one的函式定義他會回傳i32但陳述式不會回傳任何數值。我們用單元型別()表示不會回傳任何值。因此沒有任何值被回傳，這和函式定義相牴觸，最後產生錯誤。Rust提供了一到訊息來解決問題:它建議移除分號，這樣就能修正。

