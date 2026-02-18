# LeftStr -  (內建函數)（字串函數）

## 語法

回傳字串的左邊子字串  
子字串 = LeftStr(原始字串，字元長度)

## 說明

這個函數傳入兩個參數：

* 第一個參數是原始字串
* 第二個參數是欲取出的字元個數

回傳值是從原始字串左邊開始長度為第二個參數的子字串。

```xs
Var: str1("");


str1 = LeftStr("abcdefg", 3);  // str1 = "abc"
```

在上面範例內，str1是"abcdefg"從左邊算起長度為3的子字串，"abc"。

請參考 [RightStr函數](api?a=rightstr&b=bif) 以及 [MidStr函數](api?a=midstr&b=bif)。
