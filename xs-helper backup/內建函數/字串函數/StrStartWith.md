# StrStartWith -  (內建函數)（字串函數）

## 語法

condition1 = StrStartWith(字串1, 字串2);  
condition1 = StrStartWith(字串1, 字串2, 比對方式);  
  
- 傳入兩個字串, 判斷第一個字串的開頭是否與第二個字串的完整內容相同。  
- 回傳布林值。  
- 預設的比對方式是不分大小寫，可以傳入第三個參數指定比對的方式，True表示不區分大小寫。

## 說明

此函數可以用來比較傳入的第一個字串開頭字母是否和第二個字串相同。
預設的比對方是不區分字母大小寫，但可以透過傳入第三個參數來改變。

範例：

```xs
condition1 = StrStartWith(“ABCDEFG”, “ABC”);
//回傳 True。


condition1 = StrStartWith(“ABCDEFG”, “DEF”);
//回傳 False。


condition1 = StrStartWith(“ABCDEFG”, “abc”);
//回傳 True。(預設是不區分大小寫)


condition1 = StrStartWith(“ABCDEFG”, “abc”, false);
//回傳 False。
```
