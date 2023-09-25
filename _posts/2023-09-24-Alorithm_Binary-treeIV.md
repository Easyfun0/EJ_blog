---
layout: post
title:  "LinkedList-TreeIV"
date:   2023-09-24
author: Easyfun
categories: Alorithm
cover:  "/assets/cover-thumb.png"
tags: [documentation,sample]
image: Rustacean.png
---

## 二元樹節點刪除

二元樹刪除可分為幾種狀況:

❶ 刪除的節點為樹葉:只要將相連的父節點指向Null即可。

❷ 刪除的節點只有一顆子樹

![alt text]({{ site.baseurl }}/assets/img/Dtree.jpg "Profile Picture"){:.profile}

將其右指標欄放到其父節點的左指標欄。

❸ 刪除的節點有兩顆子樹，這方式有兩種，雖然結果不同，但都可符合二元樹特性。

    1.找出中序立即前行者，即是將遇刪除節點的左子樹最大者向上提，在此節點為2，也就是在該節點的左子樹，往右尋找，直到右指標為Null，這個節點就是中序立即前行者。

    2.找出中序立即後繼者，就是將欲刪除節點的右子樹最小者向上提，在此即為節點5，也就是在該節點的右子樹，往左尋找，直到左指標為Null，這個節點就是中序立即後繼者。

![alt text]({{ site.baseurl }}/assets/img/Dtree.jpg "Profile Picture"){:.profile}

範例:

將32,24,57,28,10,43,72,62，依照中序方式存入可放10個節點之陣列內，說明節點在陣列中相關位置，如果插入資料30寫出相關變化，再來刪除32寫出相關變化。

![alt text]({{ site.baseurl }}/assets/img/Dtree2.jpg "Profile Picture"){:.profile}


| root=1 | left | data | right | 
| - | - | - | - |
| 1 | 2 | 32 | 3 |
| 2 | 4 | 24 | 5 |
| 3 | 6 | 57 | 7 |
| 4 | 0 | 10 | 28 |
| 5 | 0 | 28 | 0 |
| 6 | 0 | 43 | 0 |
| 7 | 8 | 72 | 0 |
| 8 | 0 | 62 | 0 |
| 9 |  |  |  |
| 10 |  |  |  |


插入資料30:

![alt text]({{ site.baseurl }}/assets/img/Btree3.jpg "Profile Picture"){:.profile}

| root=1 | left | data | right | 
| - | - | - | - |
| 1 | 2 | 32 | 3 |
| 2 | 4 | 24 | 5 |
| 3 | 6 | 57 | 7 |
| 4 | 0 | 10 | 0 |
| 5 | 0 | 28 | 8 |
| 6 | 0 | 43 | 0 |
| 7 | 8 | 72 | 0 |
| 8 | 0 | 30 | 0 |
| 9 | 0 | 62 | 0 |
| 10 |  |  |  |

刪除資料32:

![alt text]({{ site.baseurl }}/assets/img/Btree4.jpg "Profile Picture"){:.profile}

| root=1 | left | data | right | 
| - | - | - | - |
| 1 | 3 | 24 | 4 |
| 2 | 5 | 57 | 6 |
| 3 | 0 | 10 | 0 |
| 4 | 0 | 28 | 0 |
| 5 | 0 | 43 | 0 |
| 6 | 7 | 72 | 0 |
| 7 | 0 | 62 | 0 |
| 8 | 1 | 30 | 2 |
| 9 |  |  |  |
| 10 |  |  |  |

### 堆積樹排序法

堆積排序法算是選擇排序法的強化，他可以減少選擇排序法中的比較次數，進而減少排序時間。堆積法排序法使用了二元樹技巧，利用堆積樹來完成排序。可分為最大堆積樹及最小堆積樹兩種。

最大堆積樹：

➊ 是一個完整二元樹。

➋ 所有節點的值都大於或等於他左右子節點的值。

➌ 樹根是堆積樹中最大的。

最小堆積樹：

➊ 是一個完整二元樹。

➋ 所有節點的值都小於或等於他左右子節點的值。

➌ 樹根是堆積樹中最小的。


這邊先來了解如何將二元樹轉換成堆積樹:

這邊有一筆資料[32,17,16,24,35,87,65,4,12]，以二元樹表示

![alt text]({{ site.baseurl }}/assets/img/Btree5.jpg "Profile Picture"){:.profile}

要將該二元樹轉換成堆積樹，可用堆陣列來儲存二元樹所有節點的值。

A[0]=32，A[1]=17，A[2]=16，A[3]=24，A[4]=35，A[5]=87，A[6]=65，A[7]=4，A[8]=12

㈠ A[0]=32為樹根，若A[1]大於父節點則必須互換。此處A[1]=17 < A[0]=32所以不換。

㈡ A[2]=16 < A[0]不換。

![alt text]({{ site.baseurl }}/assets/img/Btree6.jpg "Profile Picture"){:.profile}

㈢ A[3]=24 > A[1]=17所以交換。

![alt text]({{ site.baseurl }}/assets/img/Btree7.jpg "Profile Picture"){:.profile}

㈣ A[4]=35 > A[1]=24所以交換，再與A[0]=32比較，A[1]=35 > A[0]=32所以交換。

![alt text]({{ site.baseurl }}/assets/img/Btree8.jpg "Profile Picture"){:.profile}


㈤ A[5]=87 > A[2]=16所以交換，再與A[0]=35比較，A[2]=87 > A[0]=35所以交換。

![alt text]({{ site.baseurl }}/assets/img/Btree9.jpg "Profile Picture"){:.profile}

㈥ A[6]=65 > A[2]=35所以交換，且A[2]=65 < A[0]=87所以不交換。

![alt text]({{ site.baseurl }}/assets/img/Btree10.jpg "Profile Picture"){:.profile}

㈦ A[7]=4 < A[3]=17所以不換，A[8]=12 < A[3]=17所以不交換。

![alt text]({{ site.baseurl }}/assets/img/Btree10.jpg "Profile Picture"){:.profile}


這是由上往下逐一堆積樹的建立來改變個節點值，最終得到一最大堆積樹。

堆積樹並非唯一解，也可以由陣列最後一個元素由下往上逐一比較來建立最大堆積樹。若想由小到大，就必須建立最小堆積樹，做法和建立最大堆積樹類似。

㈠ 依順序建立完整二元樹。

![alt text]({{ site.baseurl }}/assets/img/Stree.jpg "Profile Picture"){:.profile}

㈡ 建立堆積樹。

![alt text]({{ site.baseurl }}/assets/img/Stree1.jpg "Profile Picture"){:.profile}

㈢ 將57自樹根移除，重新建立堆積樹。

![alt text]({{ site.baseurl }}/assets/img/Stree2.jpg "Profile Picture"){:.profile}

㈣ 將43自樹根移除，重新建立堆積樹。

![alt text]({{ site.baseurl }}/assets/img/Stree3.jpg "Profile Picture"){:.profile}


㈤ 將40自樹根移除，重新建立堆積樹。

![alt text]({{ site.baseurl }}/assets/img/Stree4.jpg "Profile Picture"){:.profile}

㈥ 將34自樹根移除，重新建立堆積樹。

![alt text]({{ site.baseurl }}/assets/img/Stree5.jpg "Profile Picture"){:.profile}

㈦ 將19自樹根移除，重新建立堆積樹。

![alt text]({{ site.baseurl }}/assets/img/Stree6.jpg "Profile Picture"){:.profile}

㈧ 將17自樹根移除，重新建立堆積樹。

![alt text]({{ site.baseurl }}/assets/img/Stree7.jpg "Profile Picture"){:.profile}

㈨ 將14自樹根移除，重新建立堆積樹。

![alt text]({{ site.baseurl }}/assets/img/Stree8.jpg "Profile Picture"){:.profile}

最後將4自樹根移除。得到排序為[57,43,40,34,19,17,14,4]

範例

用堆積排序法來排序此陣列[34,19, 40, 14, 57, 17, 4, 43]

{% highlight rust %}

fn main() {
    let mut arr = vec![34,19, 40, 14, 57, 17, 4, 43];
    println!("原始陣列: {:?}", arr);

    heap_sort(&mut arr);

    println!("排序後的陣列: {:?}", arr);
}

fn heap_sort(arr: &mut Vec<i32>) {
    let n = arr.len();

    // 建構最大堆
    for i in (0..n / 2).rev() {
        heapify(arr, n, i);
    }

    // 從中取出元素並排序
    for i in (0..n).rev() {
        arr.swap(0, i); 
        // 將跟節點移到陣列尾
        heapify(arr, i, 0); 
        // 重新建立
        println!("步骤 {}: {:?}", n - i, arr);
    }
}

fn heapify(arr: &mut Vec<i32>, n: usize, i: usize) {
    let mut largest = i;
    let left = 2 * i + 1;
    let right = 2 * i + 2;

    if left < n && arr[left] > arr[largest] {
        largest = left;
    }

    if right < n && arr[right] > arr[largest] {
        largest = right;
    }

    if largest != i {
        arr.swap(i, largest);
        heapify(arr, n, largest);
    }
}

{% endhighlight %}


