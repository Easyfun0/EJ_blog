---
layout: post
title:  "SortingII"
date:   2023-09-08
author: Easyfun
categories: Alorithm
cover:  "/assets/cover-thumb.png"
tags: [documentation,sample]
image: Rustacean.png
---

### 選擇排序法(Selection Sorting)

選擇排序法是枚舉法的應用。原理是在未排序序列中找到最小(大)元素，存放到排序序列的起始位置，再從剩餘未排序元素中繼續尋找最小(大)元素，放到已排序序列的末尾。以此類推，直到所有元素均排序完畢。

優點在資料移動有關。當某個元素位於正確的最終位置上，他就不會移動。

演算流程:

![alt text]({{ site.baseurl }}/assets/img/select.jpg "Profile Picture"){:.profile}

第一次找到此樹列中最小值後與第一個元素交換。

![alt text]({{ site.baseurl }}/assets/img/select2.jpg "Profile Picture"){:.profile}

第二次從第二個值找起，找到此數列最小值，但不包含第一個，再和第二個值交換。

![alt text]({{ site.baseurl }}/assets/img/select3.jpg "Profile Picture"){:.profile}


第三次從第三個值找起，找到此數列最小值，但不包含第一二個，再和第三個值交換。

![alt text]({{ site.baseurl }}/assets/img/select4.jpg "Profile Picture"){:.profile}

第四次從第四個值找起，找到此數列最小值，但不包含第一二三個，再和第四個值交換M
就完成此排序。

![alt text]({{ site.baseurl }}/assets/img/select5.jpg "Profile Picture"){:.profile}

範例:
用選擇排序法將下面數列做排序:

        5,4,8,7,1

{% highlight rust %}

fn main() {
    let mut arr = [5, 4, 8, 7, 1];
    let n = arr.len();

    for i in 0..n {
        let mut min = i;
        for j in i + 1..n {
            if arr[j] < arr[min] {
                min = j;
            }
        }
        if min != i{
            let temp = arr[i];
            arr[i] = arr[min];
            arr[min] = temp;
        }
        println!("每次置換的數列: {:?}", arr);
    }
    println!("排序後的數列: {:?}", arr);
}

{% endhighlight %}

 


## 插入排序法(Insert sort)

插入排序法是將陣列中的元素，逐一與排序好的資料做比較，前兩個元素先排好，再將第三個元素插入適當位置。當這三個元素排序好，接著再將第四個元素加入，重複此步驟，直到排序完成。

這邊就走一次演算流程:

![alt text]({{ site.baseurl }}/assets/img/bubble1.jpg "Profile Picture"){:.profile}


![alt text]({{ site.baseurl }}/assets/img/insert.jpg "Profile Picture"){:.profile}

步驟一4先跟第一個元素比較，放到5前面，步驟二8跟前面兩個元素比較因為比較大所以不動，步驟三7跟前面三個元素比較，因為比8小所以移動但比前面兩個大所以就沒再往前移動，最後步驟四1跟前面的元素比較，所以移到最前面即完成排序。

範例:
用插入排序法將下面數列做排序:

        5,4,8,7,1

{% highlight rust %}

fn main() {
    let mut arr = [5, 4, 8, 7, 1];
    let n = arr.len();

    for i in 1..n {
        let mut j = i;

        while j > 0 && arr[i] < arr[j -1] {
            let temp = arr[i];
            arr[j] = arr [j - 1];
            arr[j - 1] = temp;
            j -= 1;
        }
            println!("每次置換的數列:{:?}", arr);
    }
    println!("排序後的數列: {:?}", arr);
}

{% endhighlight %}

### 希爾排序法(Shell  Sort)      

希爾排序法可以減少插入排序法中資料搬移的次數，插入排序法效率偏低，因為每次只能將數據移動一位。原理是將資料區分成特定間隔的幾個小區塊，以插入排序法排完區塊內的資料在減少間隔距離。

演算流程:

1.我們用gap sequence來排序，首先算出第一個gap為(8/2)=4

![alt text]({{ site.baseurl }}/assets/img/insert.jpg "Profile Picture"){:.profile}

先排序5,4

![alt text]({{ site.baseurl }}/assets/img/insert.jpg "Profile Picture"){:.profile}

這邊3,9所以不動

![alt text]({{ site.baseurl }}/assets/img/insert.jpg "Profile Picture"){:.profile}

再來排序8,6

![alt text]({{ site.baseurl }}/assets/img/insert.jpg "Profile Picture"){:.profile}

接著7,2

![alt text]({{ site.baseurl }}/assets/img/insert.jpg "Profile Picture"){:.profile}


2.算出第二個gap為8/2 * 2=2

![alt text]({{ site.baseurl }}/assets/img/insert.jpg "Profile Picture"){:.profile}

先排序4,6所以不動

![alt text]({{ site.baseurl }}/assets/img/insert.jpg "Profile Picture"){:.profile}

接著3,2

![alt text]({{ site.baseurl }}/assets/img/insert.jpg "Profile Picture"){:.profile}

接著(6,5)

![alt text]({{ site.baseurl }}/assets/img/insert.jpg "Profile Picture"){:.profile}

再來是(3,9),(6,8)所以不動

![alt text]({{ site.baseurl }}/assets/img/insert.jpg "Profile Picture"){:.profile}

接著(9,7)

![alt text]({{ site.baseurl }}/assets/img/insert.jpg "Profile Picture"){:.profile}

3.算出第三次。gap為8/2*2*2=1，但前一次結果已經很接近排序完成，這邊就可以完成。

![alt text]({{ site.baseurl }}/assets/img/insert.jpg "Profile Picture"){:.profile}


範例:

用希爾排序法將下面數列做排序:

 [5, 3, 8, 7, 4, 9, 6, 2]


{% highlight rust %}

fn main() {
    let mut arr = [5, 3, 8, 7, 4, 9, 6, 2];
    let n = arr.len();
    
    let mut gap = n / 2;
    
    while gap > 0 {
        for i in gap..n {
            let mut j = i;
            let current = arr[i];
            
            while j >= gap && arr[j - gap] > current {
                arr[j] = arr[j - gap];
                j -= gap;
            }
            
            arr[j] = current;
        }
        
        gap /= 2;
        println!("每次置換的數列:{:?}", arr);
    }

    println!("最後排序的數列:{:?}", arr);
}

{% endhighlight %}







