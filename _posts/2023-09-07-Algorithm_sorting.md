---
layout: post
title:  "Sorting"
date:   2023-09-07
author: Easyfun
categories: Alorithm
cover:  "/assets/cover-thumb.png"
tags: [documentation,sample]
image: Rustacean.png
---

## 排序演算法

排序演算法是很常見的演算法，將一串不規則的數值資料依照遞增或遞減的方式重新編排。

## 排序(Sorting)

排序簡單來說就是將一群資料按照某一個特定規則重新安排，使其具有遞增或遞減的次序關係。用以排序的依據，稱為鍵(key)，它所含的就稱為「鍵值」。鍵值資料型態有數值型態、中文字串型態及非中文字串型態。

如果鍵值為數值型態，在比較時，則直接以數值大小做為鍵值大小比較依據，但如果鍵值為中文字串，依該中文字串由左到右逐字比較，並以中文內碼的編碼順序作為鍵值大小比較依據。假設鍵值為非中文字串，則和中文字串型態的比較方式類似，以該字串由左到右逐字比較，但卻以該字串ASCII碼的編碼順序作為鍵值大小比較依據。

在排序過程，資料的移動方式可分「直接移動」和「邏輯移動」。直接移動是直接交換儲存資料的位置，邏輯移動並不會移動資料儲存位置，僅改變指向這些資料的輔助指標的值。

![alt text]({{ site.baseurl }}/assets/img/sorting.jpg "Profile Picture"){:.profile}

兩者優劣在:直接移動會浪費許多時間進行資料的更動，而邏輯移動只要改變輔助指標指向的位置就能排序。就像是邏輯移動，而不是真正移動改變檔案中的位置。

## 氣泡排序法(Bubble sort)

氣泡排序也稱交換排序法，是最簡單的排序法之一，由於排序時每個元素會像泡泡般，一個一個浮出序列頂部，而得名。原理是由第一個元素開始，比較相鄰元素大小，如果大小順序有誤，則對調後再進行下一個元素的比較，如此掃過一次後就可確保最後一個元素是位於正確的順序。接著在第二次掃，直到完成所有元素的排序關係為止。

這邊就走一次演算流程:

🧀 由大到小排序

![alt text]({{ site.baseurl }}/assets/img/bubble1.jpg "Profile Picture"){:.profile}


第一次掃描會先拿第一個元素5和第二個元素4比較，如果第二個小於第一個元素就會交換。接著拿5和8做比較，到第四次比較完後即可確定最大值在陣列最後面。

![alt text]({{ site.baseurl }}/assets/img/bubble2.jpg "Profile Picture"){:.profile}


第二次掃描也是從頭開始，因為最後的元素已經是最大值，所以只要做三次就可以把剩下的元素最大值到剩餘陣列的最後面。

![alt text]({{ site.baseurl }}/assets/img/bubble3.jpg "Profile Picture"){:.profile}



第三次掃描，完成三個值的排序。

![alt text]({{ site.baseurl }}/assets/img/bubble4.jpg "Profile Picture"){:.profile}

第四次掃描，即可完成所有排序。

![alt text]({{ site.baseurl }}/assets/img/bubble5.jpg "Profile Picture"){:.profile}

5個元素的氣泡排序法需執行5-1次，第一次掃描要5-1次，共5+4+3+2+1=10次。

範例:
用氣泡排序法將下面數列做排序:

        5,4,8,7,1

{% highlight rust %}

fn main() {
    let mut arr = [5, 4, 8, 7, 1];
    let n = arr.len();

    for i in 0..n - 1 {
        for j in 0..n - 1 - i {
            if arr[j] > arr[j + 1] {
                
                let temp = arr[j];
                arr[j] = arr[j + 1];
                arr[j + 1] = temp;
                println!("每次置換的數列:{:?}", i + 1, arr);
            }
        }
    }
    println!("最後排序的數列:{:?}", arr);
}

{% endhighlight %}

