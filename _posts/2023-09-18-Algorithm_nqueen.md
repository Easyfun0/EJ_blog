---
layout: post
title:  "Nqueen"
date:   2023-09-18
author: Easyfun
categories: Alorithm
cover:  "/assets/cover-thumb.png"
tags: [documentation,sample]
image: Rustacean.png
---

## 八皇后(Eight Queens)

在西洋棋中的皇后可以在沒有限定一步走幾格的前提下，對棋盤中的其他棋子直吃、橫吃、對角斜吃(左右斜吃)，而後放入新皇后，再放入前必須考慮所放位置直線方向、橫線方向或對角線方向是否已被置放舊皇后，否則就會被先放入的舊皇后吃掉。

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


