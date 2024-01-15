---
layout: post
title:  "Shortest-Path"
date:   2023-09-28
author: Easyfun
categories: Alorithm
cover:  "/assets/cover-thumb.png"
tags: [documentation,sample]
image: Rustacean.png
---

## 圖形最短路徑法

最短路徑是圖形的經典演算法，在一個有像圖形G=(V,E)中，G每一個邊都有一個比例常數W與之對應，想要求G圖形中某一個頂點V0到其他頂點的最少總和之值，這就稱為最短路徑法。

接下來介紹最短路徑的常見演算法。

### Dijkstra's algorithm

Dijkstra演算法可以找到圖中最短路徑，也可以找到從節點到圖中所有其他節點的最短路徑。還可以追蹤目前已知的從每個節點之間的最短路徑，如果發現更短的路徑，則會更新這些值。

一旦演算法找到來源節點和另一個節點之間的最短路徑，該節點會被標記為「已存取」並添加到路徑中，該過程持續進行，直到圖中的所有節點都以新增至路徑中，這樣就有一條來源節點連接到所有其他節點的路徑，該路徑遵循可能到達每個節點的最短路徑。

Dijkstra's演算法:

    假設S={Vi|Vi ∈ V}，且Vi是已發現的最短路徑，其中V_0 ∈ S是起點。

假設W∉S，定義Dist(w)是從V_0到w的最短路徑，除了w外必屬於S。

⓵ 如果u是目前所找到最短路徑之下一個節點，則必屬於V-S集合中最小花費成本的邊。

⓶ 若u被選中，將u加入S集合中則會產生目前V_0到u最短路徑，對於W∉S，DIST(w)改變成DIST(w) ← Min{DIST(w),DIST(u) + COST(u,w)}

從以上推演出演算法步驟:

Ⓐ 

    G=(V,E)
    D[k]=A[F,K]，k從1到N
    S={F}
    V={1,2,...N}

D為一個N維陣列用來存放某一頂點到其他頂點最短距離

F表示起始頂點

A[F,I]為頂點F到I的距離

V是網路中所有頂點的集合

E是網路中所有邊的組合

S是頂點的集合，初始值是A={F}


Ⓑ 從V-S集合中找到一個頂點x，使D(x)的值為最小值，並把x放入S集合中。

Ⓒ 依公式

D[I]=min(D[i],D[x]+A[x,I])執行，其中(x,I)∈ E來調整D陣列的值，其中I是指x的相鄰個頂點。

Ⓓ 重複執行步驟B，一直到V-S是空集合為止。

以下圖為例，頂點5到各項頂點間的最短路徑。

![alt text]({{ site.baseurl }}/assets/img/DIJ.jpg "Profile Picture"){:.profile}

做法是由頂點5開始，找出頂點5到各項頂點間最小的距離，到達不了以∞表示。

他的步驟為:

🅰 D[0]=∞,D[1]=12,D[2]=∞,D[3]=20,D[4]=14。其中找出最小的頂點，加入S集合中:D[1]。

🅱 D[0]=∞,D[1]=12,D[2]=18,D[3]=20,D[4]=14。D[4]最小，加入S集合中。

🅲 D[0]=26,D[1]=12,D[2]=18,D[3]=20,D[4]=14。D[2]最小，加入S集合中。

🅳 D[0]=26,D[1]=12,D[2]=18,D[3]=20,D[4]=14。D[3]最小，加入S集合中。

🅴 加入最後一個頂點即可得到:

| 步驟 | S | 0 | 1 | 2 | 3 | 4 | 5 | 選擇 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 1 | 5 | ∞ | 12 | ∞ | 20 | 14 | 0 | 1 |
| 2 | 5,1 | ∞ | 12 | 18 | 20 | 14 | 0 | 4 |
| 3 | 5,1,4 | 26 | 12 | 18 | 20 | 14 | 0 | 2 |
| 4 | 5,1,4,2 | 26 | 12 | 18 | 20 | 14 | 0 | 3 |
| 5 | 5,1,4,2,3 | 26 | 12 | 18 | 20 | 14 | 0 | 0 |

由頂點5到其他各項頂點的最短距離:

頂點5 ‒  頂點0:26

頂點5 ‒  頂點1:12

頂點5 ‒  頂點2:18

頂點5 ‒  頂點3:20

頂點5 ‒  頂點4:14

Dijkstra's演算法:

{% highlight rust %}

  fn dijkstra(&self, start: usize) -> HashMap<usize, usize> {
        let mut distances = HashMap::new();
        let mut visited = HashSet::new();
        let mut min_heap = BinaryHeap::new(); // Min-heap for nodes

        distances.insert(start, 0);
        min_heap.push(Node {
            id: start,
            distance: 0,
        });

        while let Some(Node { id, distance }) = min_heap.pop() {
            if !visited.contains(&id) {
                visited.insert(id);

                for &Edge { to, weight } in self.adjacency_list.get(&id).unwrap_or(&Vec::new()) {
                    let new_distance = distance + weight;
                    let current_distance = *distances.get(&to).unwrap_or(&usize::MAX);

                    if new_distance < current_distance {
                        distances.insert(to, new_distance);
                        min_heap.push(Node { id: to, distance: new_distance });
                    }
                }
            }
        }

        distances
    }
}

{% endhighlight %}

利用最小堆和 HashMap 來實現 Dijkstra's Algorithm，並找到從特定起點到所有其他節點的最短路徑。


### A * 's algorithm

Dijkstra's演算法是尋找最短路徑的過程中算是較不具效率的做法，是因為這個演算法在尋找起點到個頂點距離的過程中，不論哪一個頂點，都要實際去計算起點與個頂點間的距離，來取得最後一個判斷，到底哪一個頂點距離與起點最近。

在需求的考量下，A * 演算法可以說是Dijkstra's演算法的改良，結合了在路徑搜尋過程中，從起點到各項頂點的「實際權重」，及個頂點預估到打中點的「推測權重」兩項，這可以有效減少不必要的搜尋動作，以提高搜尋最短路徑的效率。

A * 演算法也是最短路徑演算法，會預先設定一個「推測權重」一併納入決定最短路徑的考慮因素。推測權重是事先知道的資訊來給定一個預估值，結合這個預估值，可以更有效率搜尋最短路徑。

A * 演算法不像Dijkstra's演算法只考慮起點到這個頂點的實際權重來決定下一步要嘗試的頂點，不同的做法是，A * 演算法在計算從起點到個頂點的權重，會同步考慮從起點到這個頂點的實際權重，再加上該頂點到終點的推測權重，以推估出該頂點從起點到終點的權重。再從其中選出一個權重最小的頂點，並將該頂點標示為以搜尋完畢。接著再計算從搜尋完畢的點出發到各頂點的權重，並再從其中選出一個權重最小的點，依序前面的做法，並將該頂點標示為已搜尋完畢的頂點，以此類推，反覆進行一直到抵達終點，才結束搜尋工作以得到最短路徑的最佳解。

這聽起來挺複雜的，來做A * 演算法步驟:

𝘈 首先決定各項頂點到終點的「推測權重」。「推測權重」的計算方式可以採用各頂點和終點間的直線距離，採用四捨五入後的值，直線距離的計算函數，從這幾種評估函數的計算方式。

𝘉 分別計算從起點可抵達的各個頂點的權重，計算方式是由起點到該頂點的「實際權重」，加上該頂點抵達終點的「推測權重」，計算完畢後選出權重最小的點，並標示為搜尋完畢的點。

𝘊 接著計算從搜尋完畢的點出發到個點的權重，並再從其中選出一個權重最小的點，並將其標示為搜尋完畢的點。以次類推，反覆進行同樣的計算過程，一直到抵達終點。

A * 演算法適用於可以事先獲得或預估各項頂點到終點距離的情況，但是無法取得各項頂點到目的地終點的距離資訊時，就無法使用A * 演算法。也不是任何情況下A * 演算法效率一定優於Dijkstra's演算法。例如:當「推測權重」的距離和實際兩個頂點間的距離相差甚大時，A * 演算法的搜尋效率可能就比Dijkstra's演算法差，甚至還會誤導方向，造成無法得到最短路徑的答案。


{% highlight rust %}

fn astar(&self, start: usize, goal: usize) -> Option<Vec<usize>> {
        let mut costs = HashMap::new();
        let mut visited = HashMap::new();
        let mut min_heap = BinaryHeap::new(); // Min-heap for nodes

        costs.insert(start, 0);
        min_heap.push(Node {
            id: start,
            cost: 0,
            heuristic: 0,
        });

        // 不斷從最小堆積中取出節點，直到堆積為空
        while let Some(Node { id, cost, heuristic }) = min_heap.pop() {

            if id == goal {
                // 檢查當前取出的節點是否是目標節點。如果是，則重建路徑，返回最短路徑
                let mut path = vec![goal];
                let mut current = id;
                while let Some(&prev) = visited.get(&current) {
                    path.push(prev);
                    current = prev;
                }
                path.reverse();
                return Some(path);
            }

            for &Edge { to, weight } in self.adjacency_list.get(&id).unwrap_or(&Vec::new()) {
                let new_cost = cost + weight;
                let current_cost = *costs.get(&to).unwrap_or(&usize::MAX);

                if new_cost < current_cost {
                    let heuristic = self.heuristic(to, goal);
                    let priority = new_cost + heuristic;

                    costs.insert(to, new_cost);
                    // 初始化起點的實際代價為 0
                    visited.insert(to, id);
                    min_heap.push(Node {
                        id: to,
                        cost: new_cost,
                        heuristic,
                    });
                }
            }
        }

        None // No path found
    }

{% endhighlight %}


