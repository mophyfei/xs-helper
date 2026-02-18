# ExecOffset -  (內建函數)（一般函數）

## 語法

回傳目前函數執行時偏移的K棒筆數  
偏移筆數 = ExecOffset

## 說明

使用在函數內，用來取得目前函數執行時*偏移*的K棒根數。

一般的函數呼叫方式如下:

```xs
Value1 = Average(Volume, 5);
```

此時如果從Average函數內去呼叫ExecOffset時得到的值是0。

如果呼叫的方式改成:

```xs
Value1 = Average(Volume, 5)[1];
```

的話，則在呼叫Average函數時利用\*\*[1]\*\*設定了偏移的K棒個數，此時Average function執行時取得的資料是當時資料往前偏移一筆的結果，也就是說Value1會是前5日成交量的平均值，不包含最新一日的成交量。

在這樣子的使用情境底下，Average函數內讀取**ExecOffset**時會得到1.
