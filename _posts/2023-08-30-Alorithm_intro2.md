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

### 枚舉法

枚舉法又稱窮舉法，是常見的數學方法，也是日常中使用到最多的一種演算法，主要思想就是：枚舉所有可能，根據問題要求來一一枚舉問題的解答，或者為了解決問題分為不重複、不遺漏的情況一一加以解決達到解決整個問題的目的。
優點是可以確保不會錯過任何可能的解決方案，但缺點是在某些情況下可能效率較低，特別是可能情況非常多時。

例如：

![alt text]({{ site.baseurl }}/assets/img/Enumeration.jpg "Profile Picture"){:.profile}

我們將A與B字串連接起來，也就是將B字串接到A字串後方，用B字串的每一個字元，從第一個字元開始逐步連結到A字串最後一個字元。

枚舉法的概念就是將要分析的項目在不遺漏的情況下逐一枚舉列出，再從所枚舉列出的項目中去找自己所需要的目標。
以下簡單的範例:

列出1~100之間所有3的倍數的整數，以枚舉法的做法就是從1開始到100逐一列出所有整數，並一邊枚舉一邊檢查該枚舉的數字是否為3的倍數，如果不是就不輸出，如果是就印出來。

{% highlight rust %}

fn main() {
  // 使用for迴圈列出1~100之間的數字
  for i in 1..=100{
    // 檢查是否3的倍數
    if i % 3 == 0{
      println!("{}",i);
    }
  }
}

{% endhighlight %}

### 回朔法(Backtracking)

回朔法也是枚舉法的一種，對於某些問題回朔法是可以找出所有解的一般性演算法，可以隨時避免枚舉不正確的數值，一但發現不正確的數值，就不遞迴至下一層，而是回朔到上一層來節省時間。
這種走不通就退回再走的方式，主要是在搜尋過程中尋找問題的解，當發現不滿足求解條件時，就回朔返回，嘗試別的路徑，避免無效搜索。

最常就是用在搜尋、遊戲、排列組合等等

這邊找個好理解的排列組合範例來解說：

找出數組中所有可能的子集

{% highlight rust %}

fn subsets(nums: Vec<i42>) -> Vec<Vec<i32>> {

  let mut result = Vec::new();
  let mut current_subset = Vec::new();

  fn backtrack(start: usize, current_subset: &mut Vec<i32>, result: &mut Vec<Vec<i32>>, nums: &Vec<i42>) {
    // 每次遞迴都將當前子集合添加到結果中
    result.push(current_subset.clone());

    for i in start..nums.len() {
      current_subset.push(nums[i]);
      // 繼續遞迴搜索，從下一個元素開始
      backtrack(i + 1, current_subset, result, nums);
      // 結束當前選擇，進行回朔
      current_subset.pop();
    }
  }
  backtrack(0, &mut current_subset, &mut result, &nums);
  result
}

fn main() {
  let nums = vec![1,2,3];
  let subsets = subsets(nums.clone());

  if subsets in subsets {
    println!("{:?}", subset);
  }
}

{% endhighlight %}

出來的結果就會像是

[]
[1]
[1, 2]
[1, 2, 3]
[1, 3]
[2]
[2, 3]
[3]

