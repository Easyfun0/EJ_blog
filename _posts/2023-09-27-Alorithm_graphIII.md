---
layout: post
title:  "GraphIII"
date:   2023-09-27
author: Easyfun
categories: Alorithm
cover:  "/assets/cover-thumb.png"
tags: [documentation,sample]
image: Rustacean.png
---

### Kruskal's alogrithm

Kruskal又稱K氏法，是將各邊依權值大小由小到大排列，接著從權值最低的邊線開始架構最小成本擴張樹，如果加入的邊線會造成迴路則捨棄不用，直到加入了n-1個邊線為止。

最小成本擴張樹:

![alt text]({{ site.baseurl }}/assets/img/Ktree.jpg "Profile Picture"){:.profile}

⓵ 把所有邊線的成本列出由小到大排序:

| 起始頂點 | 終止頂點 | 成本 | 
| ------ | ------ | ---- |
| B | C | 3 |
| B | D | 5 |
| A | B | 6 |
| C | D | 7 |
| B | F | 8 |
| D | E | 9 |
| A | E | 10 |
| D | F | 11 |
| A | F | 12 |
| E | F | 16 |

⓶ 選擇成本最低的一條邊線作為架構最小成本擴張樹的起點。

![alt text]({{ site.baseurl }}/assets/img/Ktree1.jpg "Profile Picture"){:.profile}

⓷ 依步驟1所建立的表格，依序加入邊線。

![alt text]({{ site.baseurl }}/assets/img/Ktree2.jpg "Profile Picture"){:.profile}

⓸ C-D加入會形成迴路，所以直接跳過。

![alt text]({{ site.baseurl }}/assets/img/Ktree4.jpg "Profile Picture"){:.profile}

完成

![alt text]({{ site.baseurl }}/assets/img/Ktree5.jpg "Profile Picture"){:.profile}


Kruskal演算法通常使用一個稱為「Union-Find」的數據結構來檢查是否將邊添加到MST中會形成迴路，它具有查找(Find)和聯合(Union)。

{% highlight rust %}

fn kruskal(edges: &mut Vec<Edge>, num_vertices: usize) -> Vec<Edge> {
    edges.sort(); 
    // 將邊按照權重排序
    let mut mst = Vec::new(); 
    // 最小生成樹的邊集合
    let mut uf = UnionFind::new(num_vertices); 
    // 用於檢查是否形成環路的 Union-Find 結構

    for edge in edges {
        if !uf.connected(edge.from, edge.to) {
            // 如果不會形成環路，則將這條邊添加到 MST 中
            uf.union(edge.from, edge.to);
            mst.push(*edge);
        }
    }

    mst
}

{% endhighlight %}

edges是一個可變的Edge向量，代表所有的邊。函數使用迴圈遍歷edges中的每條邊，並對每一條邊進行處理。

### Prim's Algorithm

普林演算法是一種找到最小生成樹(MST)的演算法，主要是逐步擴展MST，從初始節點開始，每次選擇一個最小權重的邊來連接MST中的節點和不在MST中的節點，直到所有點都包含在MST中為止。

演算法步驟:

① 初始化一個空的MST。

② 選擇一個初始節點，將其添加到MST中。

③ 重複以下步驟，直到MST包含所有節點:
  
  Ⅰ.找到MST中的節點和不在MST中的節點之間的最小權重的邊。

  Ⅱ.將該邊添加到MST中，並將新節點添加到MST中。

最小生成樹:

⓵ 從每棵樹中挑出最小的邊

(1,6)、(2,7)、(3,4)、(5,4)

![alt text]({{ site.baseurl }}/assets/img/Ptree.jpg "Profile Picture"){:.profile}


⓶ 再從中挑出最小的邊

(6,5)、(2,3)

![alt text]({{ site.baseurl }}/assets/img/Ptree2.jpg "Profile Picture"){:.profile}

③ 最後只剩一棵樹完成

![alt text]({{ site.baseurl }}/assets/img/Ptree3.jpg "Profile Picture"){:.profile}








{% highlight rust %}

impl Graph {
    fn new(num_vertices: usize) -> Self {
        Graph {
            num_vertices,
            adjacency_list: vec![Vec::new(); num_vertices],
        }
    }

    fn add_edge(&mut self, from: usize, to: usize, weight: usize) {
        self.adjacency_list[from].push((to, weight));
        self.adjacency_list[to].push((from, weight)); // For undirected graph
    }
    // 添加邊，起始點from和終點to

    fn prim(&self) -> Vec<Edge> {
        let mut mst = Vec::new(); // Minimum Spanning Tree
        let mut visited = HashSet::new();
        let mut min_heap = BinaryHeap::new(); // Min-heap for edges

        
        visited.insert(0);

        for &(to, weight) in &self.adjacency_list[0] {
            min_heap.push(Edge {
                from: 0,
                to,
                weight,
            });
        }

        while let Some(edge) = min_heap.pop() {
            if !visited.contains(&edge.to) {
                visited.insert(edge.to);
                mst.push(edge.clone());

                for &(to, weight) in &self.adjacency_list[edge.to] {
                    if !visited.contains(&to) {
                        min_heap.push(Edge { from: edge.to, to, weight });
                    }
                }
            }
        }

        mst
    }
}

{% endhighlight %}









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


