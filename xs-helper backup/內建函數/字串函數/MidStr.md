# MidStr -  (內建函數)（字串函數）

## 語法

回傳字串內從指定位置開始的子字串。  
子字串 = MidStr(原始字串，指定位置，子字串長度)

## 說明

這個函數傳入三個參數：

* 第一個參數是原始字串
* 第二個參數是欲取出的子字串的起始位置
* 第三個參數是欲取出的子字串的字元長度

範例:

```xs
Var: str1(""), str2("");


str1 = MidStr("abcdefg", 1, 3);  // str1 = "abc"
str2 = MidStr("abcdefg", 2, 3);  // str1 = "bcd"
```

在上面範例內，str1是"abcdefg"這個字串第一個位置開始長度為3的子字串, "abc"，而str2則是"abcdefg"這個字串第二個位置開始長度為3的子字串, "bcd"。

請參考 [LeftStr函數](api?a=leftstr&b=bif) 以及 [RightStr函數](api?a=rightstr&b=bif)。
