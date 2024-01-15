---
layout: post
title:  "LinkedList-Tree"
date:   2023-09-22
author: Easyfun
categories: Alorithm
cover:  "/assets/cover-thumb.png"
tags: [documentation,sample]
image: Rustacean.png
---

## 鏈結串列實作二元樹

鏈結串列實作二元樹就是用鏈結串列來儲存二元樹，好處是對於節點增加與刪除較容易，但卻不好找到父節點，除非在每一個節點增加一個副欄位。

{% highlight rust %}

struct TreeNode {
    data: i32,
    left: Option<Box<TreeNode>>,
    right: Option<Box<TreeNode>>,
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





### 二元樹走訪(Binary Tree Traversal)

二元樹走訪就是拜訪樹中所有的節點各一次，以二元樹節點來說，每個節點都可區分為左右兩個分支。

![alt text]({{ site.baseurl }}/assets/img/walktree.jpg "Profile Picture"){:.profile}


以二元樹來說，共有ABC,ACB,BAC,BCA,CBA,CAB等六種走訪法。依照二元樹特性，一率由左至右，那就會剩下3種走訪法，BAC,ABC,BCA，還有各自命名與規則。

1.中序走訪(BAC,Preorder):左子樹->樹根->右子樹

2.前序走訪(ABC,Inorder):樹根->左子樹->右子樹

3.後續走訪(BCA,Postorder):左子樹->右子樹->樹根

只需記住樹根的位置就不會前中後搞混。

🦔 中序走訪(Inorder Traversal)

是從樹的左側逐步向下移動，直到無法移動，再追蹤此節點，並向右移動一節點。如果無法再向右移動時，可返回上層的父節點，並重複左、中、右的步驟進行。

➊ 走訪左子樹。

➋ 拜訪樹根。

➌ 走訪右子樹。

![alt text]({{ site.baseurl }}/assets/img/walktree2.jpg "Profile Picture"){:.profile}

順序為:FDHGIBEAC

🦔 後序走訪(Postorder Traversal)

走訪順序是追蹤左子樹，再追蹤右子樹，最後處理根節點，反覆執行此步驟。

➊ 走訪左子樹。

➌ 走訪右子樹。

➌ 拜訪樹根。


![alt text]({{ site.baseurl }}/assets/img/walktree2.jpg "Profile Picture"){:.profile}

順序為:FHIGDEBCA

🦔 前序走訪(Preorder Traversal)

是從根節點走訪，再往左方移動，當無法繼續時，繼續向右方移動，接著再重複執行此步驟。

➊ 拜訪樹根。

➌ 走訪左子樹。

➌ 走訪左子樹。

順序為:ABDFGHIEC

範例:

輸入一顆二元樹節點的資料為[40,20,10,50,30]，利用鏈結串列來建立，並進行中序走訪。

{% highlight rust %}

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

    // 中序走訪輸出節點
    fn in_order(&self) {
        if let Some(ref left) = self.left {
            left.in_order();
        }
        println!("{}", self.data);
        if let Some(ref right) = self.right {
            right.in_order();
        }
    }
}

fn main() {
    let data = vec![5, 6, 24, 8, 12, 3, 17, 1, 9];

    // 創建空的二元搜索樹
    let mut root: Option<Box<TreeNode>> = None;

    // 依次插入二元搜索樹
    for value in data.iter() {
        if let Some(ref mut node) = root {
            node.insert(*value);
        } else {
            root = Some(Box::new(TreeNode::new(*value)));
        }
    }

    // 中序走訪輸出節點
    if let Some(ref node) = root {
        node.in_order();
    }
}

{% endhighlight %}

