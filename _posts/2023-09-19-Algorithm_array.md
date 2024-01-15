---
layout: post
title:  "Array"
date:   2023-09-18
author: Easyfun
categories: Alorithm
cover:  "/assets/cover-thumb.png"
tags: [documentation,sample]
image: Rustacean.png
---

## 陣列實作佇列

以陣列結構實作佇列的好處是在演算法算很簡單，但跟堆疊不同之處是需要兩種動作:加入與刪除，假設front指向佇列的前端，rear向佇列的尾端，缺點在於無法事先規劃宣告。

先宣告一個有限容量的陣列

{% highlight rust %}

    let mut queue: Vec<i32> = vec![0; MAXSIZE]; 
    // 使用 Vec 来創建陣列，初始化元素為0

    let mut front = -1;
    let mut rear = -1;

{% endhighlight %}

1.開始時將front與rear都預設為-1，當front=rear時，則為空佇列。

| 事件說明 | front | rear | Q(0) | Q(1) | Q(2) | Q(3) |
| -------- | -------- | -------- | -------- | -------- | -------- | -------- |
| 空佇列Q   | -1 | -1 |

2.加入dataA，front=-1，rear=0，每加入一個元素，將rear值加入1:

| 空佇列Q   | -1 | -1 | dataA | | | |
| ---| --- | --- | --- | --- | --- | --- |

3.加入dataB、dataC，front=-1，rear=2:

| 加入dataB、dataC | -1 | 1 | dataA | dataB | dataC | |
| ---| --- | --- | --- | --- | --- | --- |

4.取出dataA，front=0，rear=2，每取出一個元素，將front值加1:

| 加入dataA | 0 | 2 |  | dataB | dataC | |
| ---| --- | --- | --- | --- | --- | --- |

5.加入dataD，front=0，rear=3，此時當rear=MAXSIZE-1，表示佇列已滿。

| 加入dataD | 0 | 3 |  | dataB | dataC | dataD |
| ---| --- | --- | --- | --- | --- | --- |

6.取出dataB，front=1，rear=3。

| 加入dataB | 1 | 3 |  | | dataC | dataD |
| ---| --- | --- | --- | --- | --- | --- |

### 鏈結串列實作佇列

佇列能以陣列方式來做以外，也可用鏈結串列來做佇列。在宣告佇列類別中，除了和佇列類別中相關的方法外，還必須指向佇列前端以及尾端的指標front和rear。

這邊用學生姓名及成績的結構來建立佇列串列:

    struct Student {
        name: String,
        score: i32,
    }

    struct Queue {
        front: Option<Box<Student>>,
        rear: Option<*mut Student>,
    }



在佇列中加入新節點，就是加入串列的尾端，刪除節點就是將串列最前端的節點刪除。
    
    // 將學生加到末端
    fn enqueue(&mut self, student: Student) {
        let new_node = Box::new(QueueNode {
            student,
            next: None,
        });

        let raw_node = Box::into_raw(new_node);

        if self.is_empty() {
            self.front = Some(unsafe { Box::from_raw(raw_node) });
            self.rear = Some(raw_node);
        } else {
            unsafe {
                (*self.rear.unwrap()).next = Some(Box::from_raw(raw_node));
                self.rear = Some(raw_node);
            }
        }
    }



    // 將前端的刪除
     fn dequeue(&mut self) -> Option<Student> {
            if self.is_empty() {
                None
            } else {
                let old_front = self.front.take().unwrap();
                let student = old_front.student;
                self.front = old_front.next;
                if self.front.is_none() {
                    self.rear = None;
                }
                Some(student)
            }
        }







當在4X4或者8X8的棋盤就變4皇后或8皇后問題。首先當棋盤中置入一個新皇后，且這個位置不會被先前放置的皇后吃掉，就將這個新皇后的位置存入堆疊。

但是放置新皇后的該行的8個位置(以8皇后為例)，都沒有辦法放置新皇后(一放入任何一個位置，就會被先前放置的舊皇后吃掉)。必須由堆疊中取出前一個皇后的位置，並於該行或該列中重新尋找另一個新的位置放置，再將該位置存入堆疊中，這就是回朔演算法的應用。

N皇后的解就是配合堆疊及回朔兩種演算法概念，以逐行或逐列找新皇后位置(如果找不到，則回朔到前一行找尋前一個皇后的另一個位置)的方式，來尋找N皇后問題的其中一組解。

![alt text]({{ site.baseurl }}/assets/img/queen1.jpg "Profile Picture"){:.profile}

4皇后堆疊內容

![alt text]({{ site.baseurl }}/assets/img/queen2.jpg "Profile Picture"){:.profile}

4皇后其中一組解

![alt text]({{ site.baseurl }}/assets/img/queen3.jpg "Profile Picture"){:.profile}

8皇后堆疊內容

![alt text]({{ site.baseurl }}/assets/img/queen4.jpg "Profile Picture"){:.profile}

8皇后其中一組解

範例:

取8皇后問題解決方法:

{% highlight rust %}

fn main() {
    let n = 4;
    let mut board = vec![vec!['.'; n]; n];
    solve_n_queens(&mut board, 0);
}

fn solve_n_queens(board: &mut Vec<Vec<char>>, row: usize) {
    if row == board.len() {
        print_board(board);
        return;
    }

    for col in 0..board.len() {
        if is_safe(board, row, col) {
            board[row][col] = 'Q';
            solve_n_queens(board, row + 1);
            board[row][col] = '.';
        }
    }
}

fn is_safe(board: &Vec<Vec<char>>, row: usize, col: usize) -> bool {
    let n = board.len();

    // 檢查列是否安全
    for i in 0..row {
        if board[i][col] == 'Q' {
            return false;
        }
    }

    // 檢查左上到右下的對角線是否安全
    for i in 0..row {
        let diff = row - i;
        if col >= diff && board[i][col - diff] == 'Q' {
            return false;
        }
        if col + diff < n && board[i][col + diff] == 'Q' {
            return false;
        }
    }

    true
}

fn print_board(board: &Vec<Vec<char>>) {
    for row in board.iter() {
        println!("{}", row.iter().collect::<String>());
    }
    println!();
}

{% endhighlight %}

總共會有92種不同解，如果合併掉那些旋轉跟對稱可以得到的解的話，那只有12個獨立解。


