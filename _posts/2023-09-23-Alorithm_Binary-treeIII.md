---
layout: post
title:  "LinkedList-TreeII"
date:   2023-09-23
author: Easyfun
categories: Alorithm
cover:  "/assets/cover-thumb.png"
tags: [documentation,sample]
image: Rustacean.png
---

## 二元樹節點搜尋

二元樹在建立過程是依據左子樹<樹根<右子樹的原則，只需從樹根出發比較鍵值，如果比樹根大就往右，否則往左而下，直到相等就可找到打算搜尋的值，如果比到NULL，無法再前進就代表搜尋不到此值。



{% highlight rust %}

    // 搜索特定鍵值
    fn search(&self, target: i32) -> bool {
        if self.data == target {
            return true;
        }
        if target < self.data {
            if let Some(left) = &self.left {
                return left.search(target);
            }
        } else {
            if let Some(right) = &self.right {
                return right.search(target);
            }
        }
        false
    }

{% endhighlight %}

範例:

二元樹節點的資料為[40,20,10,50,30]，輸入一個值，如果節點中有相等值會顯示搜尋次數，如果找不到也會顯示訊息。

{% highlight rust %}

use std::io;

#[derive(Debug)]
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

    // 插入數據到二元搜索樹
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

    // 搜索特定值，並回傳搜索次數
    fn search(&self, target: i32, search_count: &mut i32) -> bool {
        *search_count += 1;
        if self.data == target {
            return true;
        }
        if target < self.data {
            if let Some(left) = &self.left {
                return left.search(target, search_count);
            }
        } else {
            if let Some(right) = &self.right {
                return right.search(target, search_count);
            }
        }
        false
    }
}

fn main() {

    let mut root: Option<Box<TreeNode>> = None;
    let data = vec![7, 1, 4, 2, 13, 12, 11, 15, 9, 5];

    for value in data.iter() {
        if let Some(ref mut node) = root {
            node.insert(*value);
        } else {
            root = Some(Box::new(TreeNode::new(*value)));
        }
    }

    println!("請輸入要搜尋的值:");
    let mut input = String::new();
    io::stdin().read_line(&mut input).expect("無法讀取");

    // 将用户输入的键值转换为整数
    let search_value: i32 = input.trim().parse().expect("無效的輸入");

    // 执行搜索，并初始化搜索次数为 0
    let mut search_count = 0;
    let found = if let Some(ref node) = root {
        node.search(search_value, &mut search_count)
    } else {
        false
    };

    // 输出搜索结果和搜索次数
    if found {
        println!("找到值 {}，搜索次数: {}", search_value, search_count);
    } else {
        println!("未找到值 {}", search_value);
    }
}

{% endhighlight %}

### 二元樹節點插入

二元樹插入和搜尋相似，是在插入後要保持二元搜尋樹的特性。

{% highlight rust %}

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
}

{% endhighlight %}



範例:

輸入一顆二元樹節點的資料為[40,20,10,50,30]，輸入一個值，如不在此二元樹中，則將他加入此樹。

{% highlight rust %}

use std::io;

// 定義一個二元樹節點
#[derive(Debug)]
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

    // 插入到二元樹搜索樹
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
}

// 中序走訪
fn in_order(node: &Option<Box<TreeNode>>) {
    if let Some(ref n) = node {
        in_order(&n.left);
        print!("{} ", n.data);
        in_order(&n.right);
    }
}

fn main() {
    // 創建一個空的二元搜索樹
    let mut root: Option<Box<TreeNode>> = None;

    // 二元樹的初始
    let data = vec![7, 1, 4, 2, 13, 12, 11, 15, 9, 5];

    // 將初始插入到二元樹中
    for value in data.iter() {
        if let Some(ref mut node) = root {
            node.insert(*value);
        } else {
            root = Some(Box::new(TreeNode::new(*value)));
        }
    }

    println!("请输入要插入的键值:");
    let mut input = String::new();
    io::stdin().read_line(&mut input).expect("无法读取输入");

    // 將輸入的值插入
    let new_value: i32 = input.trim().parse().expect("无效的输入");
    if let Some(ref mut node) = root {
        node.insert(new_value);
    } else {
        root = Some(Box::new(TreeNode::new(new_value)));
    }

    println!("中序遍历结果:");
    in_order(&root);
}

{% endhighlight %}


















範例:

輸入一顆二元樹節點的資料為[40,20,10,50,30]，利用鏈結串列來建立，並輸出左右子樹。

{% highlight rust %}

#[derive(Debug)]
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

    // 插入數據到二元搜索樹
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

    // 得到左子樹
    fn left_subtree(&self) -> Option<&TreeNode> {
        self.left.as_deref()
    }

    // 得到右子樹
    fn right_subtree(&self) -> Option<&TreeNode> {
        self.right.as_deref()
    }
}

fn main() {
    let data = vec![5, 6, 24, 8, 12, 3, 17, 1, 9];

    // 建造一個空的二元搜索樹
    let mut root: Option<Box<TreeNode>> = None;

    // 依次插入數據建構二元搜索樹
    for value in data.iter() {
        if let Some(ref mut node) = root {
            node.insert(*value);
        } else {
            root = Some(Box::new(TreeNode::new(*value)));
        }
    }

    // 輸出左子樹
    if let Some(ref node) = root {
        if let Some(left_subtree) = node.left_subtree() {
            println!("左子樹: {:?}", left_subtree);
        } else {
            println!("左子樹为空");
        }
    }

    // 輸出右子樹
    if let Some(ref node) = root {
        if let Some(right_subtree) = node.right_subtree() {
            println!("右子樹: {:?}", right_subtree);
        } else {
            println!("右子樹为空");
        }
    }
}

{% endhighlight %}





![alt text]({{ site.baseurl }}/assets/img/Btree.jpg "Profile Picture"){:.profile}



