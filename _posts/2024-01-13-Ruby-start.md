---
layout: post
title:  "Ruby"
date:   2024-01-13
author: Easyfun
categories: Ruby
tags: [documentation,sample]
image: ts.png
---

## Ruby的基本方法

### 呼叫方法

註解:

在Javascript是用//來做註解，Ruby裡是用#做開頭來當做註解。

puts與print:

puts是"put sreing"的簡寫，以便把文字顯示在標準輸出。我們會把一個字串(內容所要顯示的文字)傳遞給puts。

接著在把另一個字串傳遞給下一列的print方法，以便詢問使用者名字。print方法的行為就像puts，但是puts會自動在字串結尾添加一個換列符號以便跳到下一列，但是print並不會這樣做。

Ruby的頂層執行環境(top-level execution environment)納入了這兩個方法以供呼叫。在Ruby中，你不必指定接收者就可以呼叫頂層環境中所定義的方法。

#### 方法引數

puts方法需要取得字串並把它印到標準輸出。

  puts "Hello worrld"

被傳遞給puts方法的字串稱為方法引數(argument)，puts方法可以取得多個引數;只需使用逗號隔開引數就行。

  puts "Hello", "Mike", "I'm fine"

gets:

gets方法(gets是"get string"的簡寫)會從標準輸入讀取一列資料。當呼叫gets，會導致程式暫停直到使用者鍵入他們的值以及按下enter，最後gets會回傳使用者所鍵入的值。

跟puts和print一樣，可在任何一處呼叫gets而不必指定接收者。

#### 方法呼叫中的()可省略

在Ruby中，引數會被放在一對圓括號裡:

  puts("Easy", "Fun")

可改成

  puts "Easy", "Fun"

gets方法會從標準輸入讀取一列資料。他通常不需要任何引數:

  get

即使是有效的，也要避免這樣做

  ~~gets()~~

#### 字串安插

把變數的值安插(interpolate)到字串中。每當被雙引號括住的字串包含#{...}標記時，Ruby會使用大括號裡的值來填空(fill in the blank)。#{...}標記可能會出現在字串中任何位置:開頭、結尾或是中間某處。

#{...}標記中不一定要使用變數-可以使用任何Ruby運算式。

使用inspect和p方法來檢視物件:

每個Ruby物件都會有inspect方法可供呼叫。他會把物件轉換成適合除錯的字串表達形式。他會揭示物件通常不會在程式輸出中呈現的面相。

  puts input.inspect

印出inspect結果的需求很普遍，於是Ruby提供了另一個方式:p方法。他的方式如同puts，不過在印出結果之前，他會對每個引數呼叫inspect。

#### 字串中的規避序列

在p方法的使用，結尾處會有兩個字符，主要有幾種常見的

|  | 字符 | 
| --- | --- |
| \n | 換列(newline) |
| \t | 跳格(tab) |
| \" | 雙引號(double-quote) |
| \\ | 倒斜線(backslash) |

把一個雙引號(")放入被雙引號括住的字串中，他會被視為字串的結尾符號，因此導致錯誤。

我們可以在雙引號前放置一個倒斜線符號來規避雙引號，這樣即使在被雙引號括住的字串中放入雙引號也不會錯誤。

  puts "\"Hello,\" I'm fine."

因為\是規避序列的起始標記，還需要一個方法來表示非規避序列起始標記的倒斜線符號。

  puts "Hello world: \\"

這些規避序列多半僅適用於被雙引號括住的字串。在被單引號括住的字串中，大多數的規避序列只有在字面上的作用。

  puts '\n\t\"'

#### 呼叫字串物件上的chomp方法

如果字串的結尾字符是一個換列符，chomp方法將會移除他，他適合用於清理，例如:gets所回傳的字符。

不同於printf、puts和gets，chomp方法的應用較窄，所以只有字串物件會提供此方法。我們需要把input變數中的字串指定為chomp方法的接收者(receiver)。我們需要對input使用點號運算符。

  print "Your name:"
  name = gets
  puts "Hi, #{name}!"

結果:

  Your name: Mike
  Hi, Mike
  !

  print "Your name:"
  name = gets.chomp
  puts "Hi, #{name}!"

結果:

  Your name: Mike
  Hi, Mike!

Chomp會回傳相同的字串，但是結尾處的換列符已被移除，會將此字串存入名為name的變數，該變數是歡迎詞的一部分，他會一起被印出。

#### 物件提供了哪些方法

無法對任何物件呼叫任何方法，會有錯誤訊息。

  undefined method `upcase' for 42:Integer (NoMethodError)

可以呼叫methods的方法來看

#### 產生隨機數字

rand會在所給定的範圍內產生一個隨機數字，以一個數字做為引數傳遞給rand:

  puts rand(50)

rand所產生的數字將會介於零到「你所指定之最大值減1」之間，我們會得到範圍0~49的隨機數字，而不是1~50的隨機數字，我們只要把rand回傳的值加1即可。

  rand(50) + 1

#### 轉換為字串

當要把字串相加時，會使用加號(+)把字串連接在一起，就像JS可以直接做相加，但在Ruby這樣是沒用的。

在Ruby裡所有的物件都具有to_s方法，此方法可以進行轉換。

  str = "Number is:" + 123.to_s

#### 條件式述句

Ruby條件式敘句(conditional statement):會使的程式碼只在條件相符的情況下執行。

  if 1 < 2
    puts "true"
  end

還有if/elsif/else

  if 1 < 2
    puts "true"
  elsif 1 > 2 
    puts "false"
  else
    puts "不正確"
  end

#### if的相反是unless

Ruby的額外關鍵字: unless。if述句是在條件式結果為真(true)才會被執行，但在unless述句中，只有在條件式為假(false)才會被執行。

  unless true
    puts "我不會被印出！"
  end

unless主要可以讓程式碼變成容易閱讀的。

  if !(light == "red")
    puts "Go"
  end

可以改成

  unless light == "red"
    puts "Go"
  end

#### 迴圈

while迴圈是以一個單字while、一個布林運算式、反覆執行的程式碼來組成。只要條件求值結果為真就會反覆執行迴圈主體。

  number = 1
  while number <= 5
    puts number
    number += 1
  end

until迴圈是while的反義，也可以用until來做迴圈的處理:

  number = 1
  until number > 5
    puts number
    number += 1
  end
  while number <= 5
    puts number
    number += 1
  end
