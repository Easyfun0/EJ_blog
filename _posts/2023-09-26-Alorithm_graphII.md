---
layout: post
title:  "GraphII"
date:   2023-09-26
author: Easyfun
categories: Alorithm
cover:  "/assets/cover-thumb.png"
tags: [documentation,sample]
image: Rustacean.png
---

## 擴張樹(Spanning Tree)

擴張樹又稱花費樹或值樹，一個圖形的擴張樹是以最少的邊來連結圖形中所有的頂點，且不造成循環。在樹的邊上加上一個權重值，這個圖形就成為加權圖形。

擴張樹是由所有頂點及拜訪過程中經過的邊所組成，另S=(V,T)為圖形G=(V,E)中的擴張樹

1.E=T+B(T為拜訪中所經過的邊，B為拜訪過程中未經過的邊)

2.將集合B中的任一邊加入結合T中，就會造成迴圈

3.V中任兩頂點Vi與Vj，在擴張樹S中存在唯一的一條簡單路徑


## 最小花費擴張樹(Minimum Spanning Tree)

要想知道某個點到另一個點間的路徑成本，例如從頂點1到頂點5有(1+2+4)、(1+6+4)和6這三個路徑。最小花費擴張樹就是路徑為6的擴張樹。

![alt text]({{ site.baseurl }}/assets/img/Prtree2.jpg "Profile Picture"){:.profile}


一個加權圖形中找到最小擴張樹是很重要的，生活很多情境都可用圖形來表示例如出國轉機的距離就可以找出花費最少的方法。

接下來介紹幾種演算法來找出加權圖形中花費最少擴張樹的方法。


### Prim's alogrithm

Prim又稱P氏法，對一個加權圖形G=(V,E)，設V={1,2,...,u}，U={1}，U跟V是兩個頂點的集合。

從U-V差集所產生的集合中找出一個頂點x，該頂點x能與U集合中的某點形成最小成本的邊，且不會造成迴圈。再將頂點x加入U集合中，反覆執行同樣的步驟，一直到U集合等於V集合(U=V)為止。

![alt text]({{ site.baseurl }}/assets/img/Prtree.jpg "Profile Picture"){:.profile}

從圖形可得知V={1,2,3,4,5,6},U=1

從V-U={2,3,4,5,6}中找一頂點與U頂點能形成最小成本邊，得到

![alt text]({{ site.baseurl }}/assets/img/Prtree5.jpg "Profile Picture"){:.profile}

V-U={2,3,4,6},U={1,5}

從V-U中頂點能形成最小成本的邊，得到

![alt text]({{ site.baseurl }}/assets/img/Ptree6.jpg "Profile Picture"){:.profile}

且U={1,5,6}，V-U={2,3,4}

一樣的找到頂點4，U={1,5,6,4},V-U={2,3}

![alt text]({{ site.baseurl }}/assets/img/Ptree7.jpg "Profile Picture"){:.profile}

一樣的找到頂點3

![alt text]({{ site.baseurl }}/assets/img/Ptree8.jpg "Profile Picture"){:.profile}

一樣的找到頂點2

![alt text]({{ site.baseurl }}/assets/img/Ptree9.jpg "Profile Picture"){:.profile}

P氏法的演算:

{% highlight rust %}

fn prim(&self) -> (i32, Vec<(usize, usize)>) {
        let mut visited = HashSet::new();
        let mut edges = Vec::new();
        let mut min_weight = 0;
        let mut heap = BinaryHeap::new();

        visited.insert(0); 
        // 從頂點0開始

        while visited.len() < self.adjacency_matrix.len() {
            for &v in &visited {
                for (u, &weight) in self.adjacency_matrix[v].iter().enumerate() {
                    if !visited.contains(&u) && weight != i32::MAX {
                        heap.push((weight, v, u));
                    }
                }
            }

            while let Some((weight, from, to)) = heap.pop() {
                if !visited.contains(&to) {
                    visited.insert(to);
                    edges.push((from, to));
                    min_weight += weight;
                    break;
                }
            }
        }

        (min_weight, edges)
    }


{% endhighlight %}

對於visited中的每個頂點v遍歷與之相鄰的頂點u，如果u不在visited中，並且v到u的邊不為無窮大則將這些邊添加到heap中。


