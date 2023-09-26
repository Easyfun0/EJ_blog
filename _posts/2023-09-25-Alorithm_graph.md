---
layout: post
title:  "Graph"
date:   2023-09-25
author: Easyfun
categories: Alorithm
cover:  "/assets/cover-thumb.png"
tags: [documentation,sample]
image: Rustacean.png
---

## 圖形演算法

前面也有介紹到圖形的定義，這邊會來介紹圖形的演算法。

### 圖形的走訪

前幾天的樹追蹤是拜訪樹的每一個節點一次，用的方法有前中後序法，那圖形追蹤的定義，就是G=(V,E)，我們從V開始，經由此節點相鄰的節點而去拜訪G中其他節點，稱為「圖形追蹤」。從某一個頂點V1開始，走訪可經由V1到達的頂點，接著再走訪下一個頂點直到全部的頂點走訪完畢。


在走訪過程可能會重複經過某些頂點及邊線。經由圖形的走訪可以判斷該圖形是否連通，並找出連通單元及路徑。

圖形走訪有兩種「先深後廣走訪」，「先廣後深走訪」。

#### 先深後廣走訪（Depth-First Search）

先深後廣走訪類似前序走訪，從圖形的某一頂點開始走訪，被拜訪過的頂點就做上已拜訪的記號，接著走訪此頂點所有相鄰且未拜訪過頂點中的任意頂點，並做上已拜訪的記號，再以該點為新的起點繼續進行先深後廣的搜索。

這圖形追蹤結合了遞迴及堆疊兩種方法，由於會造成無窮迴圈，所以必須加入一個變數，判斷該點是否已經走訪完畢，這樣講還是很複雜吧，用圖看比較好懂走訪。

![alt text]({{ site.baseurl }}/assets/img/DFS.jpg "Profile Picture"){:.profile}

①  以頂點1為起點，將相鄰的頂點2及頂點5放入堆疊。

![alt text]({{ site.baseurl }}/assets/img/DFS2.jpg "Profile Picture"){:.profile}

②  取出頂點2，將與頂點2相鄰且未拜訪過的頂點3及頂點4放入堆疊。

![alt text]({{ site.baseurl }}/assets/img/DFS3.jpg "Profile Picture"){:.profile}

③  取出頂點3，將與頂點3相鄰且未拜訪過的頂點4及頂點5放入堆疊。

![alt text]({{ site.baseurl }}/assets/img/DFS4.jpg "Profile Picture"){:.profile}

④  取出頂點4，將與頂點4相鄰且未拜訪過的頂點4及頂點5放入堆疊。

![alt text]({{ site.baseurl }}/assets/img/DFS5.jpg "Profile Picture"){:.profile}

⑤  取出頂點5，將與頂點5相鄰且未拜訪過的頂點放入堆疊，因為頂點5相鄰的頂點全部被拜訪過，所以無須再放入堆疊。

![alt text]({{ site.baseurl }}/assets/img/DFS6.jpg "Profile Picture"){:.profile}

⑥  將堆疊內的值取出並判斷是否已經拜訪過，直到堆疊內無節點可走為止。

![alt text]({{ site.baseurl }}/assets/img/BDF.jpg "Profile Picture"){:.profile}


先深後廣的走訪順序: 頂點1、頂點2、頂點3、頂點4、頂點5。

深度優先函數:
{% highlight rust %}

    let mut ptr = &head[current as usize];
    while let Some(link) = ptr {
        if run[link.val as usize] == 0 {
            dfs(link.val, run, head);
        }
        ptr = &link.next;
    }

{% endhighlight %}

#### 先廣後深搜尋法（Breadth-First Search）

先廣後深是以佇列及遞迴來走訪，也是從某一圖形頂點開始走訪，被拜訪過的頂點就做上已拜訪的記號。所有相鄰且未拜訪過頂點中的任意一個頂點，並做上已拜訪的記號，再以該點為新的起點繼續進行先廣後深的搜尋。

①  以頂點1為起點，與頂點1相鄰且未拜訪過的頂點2及頂點5放入佇列。

![alt text]({{ site.baseurl }}/assets/img/BFS.jpg "Profile Picture"){:.profile}

②  取出頂點2，將與頂點2相鄰且未拜訪過的頂點3及頂點4放入佇列。

![alt text]({{ site.baseurl }}/assets/img/BFS2.jpg "Profile Picture"){:.profile}

③  取出頂點5，將與頂點5相鄰且未拜訪過的頂點3及頂點4放入佇列。

![alt text]({{ site.baseurl }}/assets/img/BFS3.jpg "Profile Picture"){:.profile}

④  取出頂點3，將與頂點3相鄰且未拜訪過的頂點4放入佇列。

![alt text]({{ site.baseurl }}/assets/img/BFS4.jpg "Profile Picture"){:.profile}

⑤  取出頂點4，將與頂點4相鄰且未拜訪過的頂點放入ㄓ佇列，因為頂點4相鄰的頂點全部被拜訪過，所以無須再放入佇列。

![alt text]({{ site.baseurl }}/assets/img/BFS5.jpg "Profile Picture"){:.profile}

⑥  將佇列內的值取出並判斷是否已經拜訪過，直到佇列內無節點可走為止。

![alt text]({{ site.baseurl }}/assets/img/BDF.jpg "Profile Picture"){:.profile}

先廣後深的走訪順序: 頂點1、頂點2、頂點5、頂點3、頂點4。

{% highlight rust %}

let mut queue = VecDeque::new();
    queue.push_back(start);

    while let Some(node) = queue.pop_front() {
        if run[node as usize] == 0 {
            run[node as usize] = 1;
            println!("[{}]", node);

            let mut ptr = &head[node as usize];
            while let Some(link) = ptr {
                if run[link.val as usize] == 0 {
                    queue.push_back(link.val);
                }
                ptr = &link.next;
            }
        }
    }

{% endhighlight %}















🌳 完滿二元樹(Full Binary Tree)

當二元樹的高度為h，樹的節點為2^-1，h>=0，則稱為「完滿二元樹」。

![alt text]({{ site.baseurl }}/assets/img/Btree.jpg "Profile Picture"){:.profile}

🌳 完整二元樹(Complete Binary Tree)

當二元樹的深度為h，所含的節點數小於2^h-1，其節點的編號方式如同深度h的完滿二元樹一樣，從左到右，由上到下的順序一一對應。

![alt text]({{ site.baseurl }}/assets/img/Btree2.jpg "Profile Picture"){:.profile}

🌳 歪斜樹(Skewed Binary Tree)

當一個二元樹完全沒有左節點或右節點時，則稱為左歪斜樹或右歪斜樹。

![alt text]({{ site.baseurl }}/assets/img/Btree3.jpg "Profile Picture"){:.profile}

🌳 嚴格二元樹(Strictly Binary Tree)

指二元樹的每個非終端點均有非空的左右子樹。

![alt text]({{ site.baseurl }}/assets/img/Btree4.jpg "Profile Picture"){:.profile}


### 陣列實作二元樹

使用循序的一維陣列表示二元樹，先將此二元樹想成一個完滿二元樹，且第n個階度具有2^n-1個節點，並依序存放在此一維陣列中。

先來看看使用一維陣列建立二元樹的表示方法及索引值的配置:

![alt text]({{ site.baseurl }}/assets/img/Atree1.jpg "Profile Picture"){:.profile}

可以看到此一維陣列的索引值的關係

1. 左子樹索引值是父節點索引值 * 2。

2. 右子樹索引值是父節點索引值 * 2+1。

這邊來看如何以一維陣列建立二元樹，也就是建立一個二元搜尋樹，是很好的排序應用模式，因為在建立二元樹時，資料已經有初步的比較判斷，並依照二元樹的建立規則來存放資料。

二元搜尋樹特點:

🪴 可以是空集合，但若不是空集合則節點上一定要有一個鍵值。

🪴 每一個樹根的值需大於左子樹的值。

🪴 每一個樹根的值需小於右子樹的值。

🪴 左子樹也是二元搜尋樹。

🪴 樹的每個節點值都不相同。

將一組資料40,20,10,50,30，建立一顆二元搜尋樹

1.

![alt text]({{ site.baseurl }}/assets/img/Atree.jpg "Profile Picture"){:.profile}

2.

![alt text]({{ site.baseurl }}/assets/img/Atree2.jpg "Profile Picture"){:.profile}

3.

![alt text]({{ site.baseurl }}/assets/img/Atree3.jpg "Profile Picture"){:.profile}

4.

![alt text]({{ site.baseurl }}/assets/img/Atree4.jpg "Profile Picture"){:.profile}

5.

![alt text]({{ site.baseurl }}/assets/img/Atree5.jpg "Profile Picture"){:.profile}


範例:

輸入一顆二元樹節點的資料為[40,20,10,50,30]

{% highlight rust %}

// 定義一個二元樹節點
struct TreeNode {
    data: i32,
    left: Option<Box<TreeNode>>,
    right: Option<Box<TreeNode>>,
}

impl TreeNode {
    fn new(data: i32) -> Self {
        TreeNode {
            data,
            left: None,
            right: None,
        }
    }

    // 插入數據到二元搜尋樹
    fn insert(&mut self, data: i32) {
        if data < self.data {
            if let Some(left) = &mut self.left {
                left.insert(data);
            } else {
                self.left = Some(Box::new(TreeNode::new(data)));
            }
        } else {
            if let Some(right) = &mut self.right {
                right.insert(data);
            } else {
                self.right = Some(Box::new(TreeNode::new(data)));
            }
        }
    }
    
    
    fn to_array(&self, array: &mut Vec<i32>) {
        if let Some(left) = &self.left {
            left.to_array(array);
        }
        array.push(self.data);
        if let Some(right) = &self.right {
            right.to_array(array);
        }
    }
}

fn main() {
    let data = vec![40,20,10,50,30];

    // 創建一個空的二元搜尋樹
    let mut root: Option<Box<TreeNode>> = None;

    // 依次插入數據
    for value in data.iter() {
        if let Some(ref mut node) = root {
            node.insert(*value);
        } else {
            root = Some(Box::new(TreeNode::new(*value)));
        }
    }

    // 轉化二元搜尋樹為一維陣列
    let mut result = Vec::new();
    if let Some(ref node) = root {
        node.to_array(&mut result);
    }

    println!("{:?}", result);
}

{% endhighlight %}





