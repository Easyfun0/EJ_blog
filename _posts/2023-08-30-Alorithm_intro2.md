---
layout: post
title:  "演算法簡介"
date:   2023-08-30
author: Easyfun
categories: Alorithm
tags: [documentation,sample]
image: cover-thumb.png
---

## 常見演算法簡介二

### 動態規劃法(Dynamic Programming Algorithm)

動態規劃法主要是如果一個問提答案與子問題相關的話，就能將大問題拆解成各個小問題，其中與分治法不同的地方是可以讓每一個子問題的答案被儲存起來，以供下次求解時直接取用。
這樣的做法不但能減少再次需要計算的時間，並將這些解組合成大問題的答案，故使用動態規劃將可以解決重複計算的缺點。

這邊用動態規劃法來改寫費柏那序列，已計算過資料不必計算，也不會再往下遞迴，會達到增進效能的目的。

例如想取得第四個費柏那Fib(4)，他的遞迴可以利用動態規劃法

![alt text]({{ site.baseurl }}/assets/img/fibD.jpg "Profile Picture"){:.profile}

這邊得知遞迴呼叫9次，而執行加法運算4次，Fib(1)重複執行3次，浪費了效能。

{% highlight rust %}

fn factorial_dynamic(n: i32) -> i32 {
  if n <= 1 {
        return n;
  }
  let mut fib = vec![0; (n + 1) as usize];
  fib[1] = 1;

  for i in 2..=n{
    fib[i as usize] = fib[(i - 1) as usize] + fib[(i - 2) as usize];
  }
  fib[n as usize]
}

fn main() {
  for i in 0..11{
    println!("fib({i}) = {}", factorial_dynamic(i));
  }
}

{% endhighlight %}

### 疊代法(iterative method)

疊代法是無法使用公式一次求解，而需反覆運算，例如用迴圈去循環重複程式碼的某些部分來得到答案。

{% highlight rust %}

fn factorial_iterative(n: u64) -> u64 {
  if n == 0 {
    1
  } else {
    n * factorial_iterative(n - 1)
  }
}

fn main() {
  let n = 10;
  for i in 1..=n{
    println!("{}! = {}", factorial_iterative(i));
  }
}

{% endhighlight %}


