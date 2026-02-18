# StrTrim -  (內建函數)（字串函數）

## 語法

str1 = StrTrim(字串);  
str1 = StrTrim(字串, 選項);  
  
- 傳入字串後，去除這個字串開頭以及結尾的空白後回傳字串。  
- 預設是刪除空白。  
- 可以傳入第二個參數選項，指定刪除的範圍。  
- 選項是一個數字，0 = 頭尾都刪除(如同預設)， 1 = 只去除開頭的空白，2 = 只去除結尾的空白。

## 說明

此函數可以用來刪除字串中的開頭和結尾的空白字元，預設是將開頭與結尾的空白都刪除，但可以透過傳入參數的方式來指定只刪除開頭或結尾。

範例：

```xs
str1 = StrTrim("  hello world "); 
//回傳的字串會是"hello world"。


str1 = StrTrim("  hello world ", 0); 
//回傳的字串會是 "hello world"。


str1 = StrTrim("  hello world ", 1); 
//回傳的字會是 "hello world "。


str1 = StrTrim("  hello world ", 2);
//回傳的字串會是"  hello world"。
```
