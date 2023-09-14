---
layout: post
title:  "SearchingII"
date:   2023-09-11
author: Easyfun
categories: Alorithm
cover:  "/assets/cover-thumb.png"
tags: [documentation,sample]
image: Rustacean.png
---


### 二分搜尋法(Binary search)

又稱對數搜尋，如果搜尋的資料已經排好即可用二分搜尋法來進行搜尋。它是將資料分割成兩等份，再比較鍵值中間值大小，如果鍵值小於中間值，則可確定要找的資料是在前半段的元素，直到找到或確定不存在為止。

![alt text]({{ site.baseurl }}/assets/img/binary.jpg "Profile Picture"){:.profile}

以此排序為例:

要搜尋11，先找到8/2=4，第四個元素為9。

![alt text]({{ site.baseurl }}/assets/img/binary2.jpg "Profile Picture"){:.profile}

第四個元素9比11小，所以捨棄第四個元素以前的元素。

![alt text]({{ site.baseurl }}/assets/img/binary3.jpg "Profile Picture"){:.profile}

對剩下的元素搜尋，4/2=2，4+2=6，取得第六個元素為12，比11還大，捨棄11以後的元素。

![alt text]({{ site.baseurl }}/assets/img/binary4.jpg "Profile Picture"){:.profile}

最後11=11，搜尋完成，如果不相等則表示找不到。



範例:
用二元搜尋法找出11:

[2, 3, 5, 8, 9, 11, 12, 16, 18]

{% highlight rust %}

fn main() {
    let data = vec![2, 3, 5, 8, 9, 11, 12, 16, 18];
    let target = 11;

    let result = binary_search(&data, target);

    match result {
        Some(index) => println!("找到 {:?} 在第{:?}項.", target, index),
        None => println!("{} 未找到.", target),
    }
}

fn binary_search(data: &Vec<i32>, target: i32) -> Option<usize> {
    let mut left = 0;
    let mut right = data.len() - 1;

    while left <= right {
        let mid = left + (right - left) / 2;

        if data[mid] == target {
            return Some(mid);
        }

        if data[mid] < target {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }

    None
}


{% endhighlight %}


### 內差搜尋法(Interpolation Search)

內差搜尋又叫做差捕搜尋法，是二分法的改良版。是依照資料位置的分佈，利用公式來計算猜測搜尋鍵值的所在位置。

差值搜尋法和二元搜尋類似，不一樣在差值是每次從自訂的mid處開始搜尋。

他的公式是

    mid = l + (h-l) * (key - data(h)) / data(h) - data(l))

key是要尋找的鍵，data(l)、data(h)是剩餘待尋找紀錄中最大與最小值，對資料筆數為n。

差補搜尋法步驟:

1.將紀錄由小到大順序給予1,2,3...n

2.另l=1,h=n

3.l < h時，重複執行步驟1和4

4.若key < key_mid且high != mid-1 則令h = mid-1

5.若key = key_mid表示成功搜尋到鍵值的位置

6.若key > key_mid且l !=  mid+1則令l = mid+1

範例:
用內差搜尋法找出11:

[2, 3, 5, 8, 9, 11, 12, 16, 18]

{% highlight rust %}

fn main() {
    let data = vec![2, 3, 5, 8, 9, 11, 12, 16, 18];
    let target = 17;

    let result = interpolation_search(&data, target);

    match result {
        Some(index) => println!("找到 {} 在第 {} 項.", target, index),
        None => println!("{} 未找到.", target),
    }
}

fn interpolation_search(data: &Vec<i32>, target: i32) -> Option<usize> {
    let mut left = 0;
    let mut right = data.len() - 1;

    while left <= right {
        let range = data[right] as i32 - data[left] as i32;

        if range == 0 {
            if data[left] == target {
                println!("目標 {} 等於左邊值 {}，找到目標。", target, data[left]);
                return Some(left);
            } else {
                println!("目標 {} 不等於左邊值 {}，未找到目標。", target, data[left]);
                return None;
            }
        }

        // 計算位置
        let mid = left + (((target - data[left]) * (right - left) as i32) / range) as usize;

        if data[mid] == target {
            println!("找到目標 {} 在第 {} 項。", target, mid);
            return Some(mid);
        } else if data[mid] < target {
            println!("目標 {} 大於估計位置 {} 的值，向右搜索。", target, data[mid]);
            left = mid + 1;
        } else {
            println!("目標 {} 小於估計位置 {} 的值，向左搜索。", target, data[mid]);
            right = mid - 1;
        }
    }

    println!("结束，沒找到。");
    None
}

{% endhighlight %}






