# formatMQY -  (系統函數)（日期相關）

## 語法

依目前資料頻率取得代表日期字串。  
回傳字串=formatMQY(參考日期)  
傳入一個參數:  
- 第一個參數是日期，格式為YYYYMMDD的數值。

## 說明

formatMQY回傳的字串為：

* 當頻率為年時，回傳格式為YYYY的字串；例如：2015。
* 當頻率為季時，回傳格式為YYYYQQ的字串；例如：2015Q1。
* 當頻率為月時，回傳格式為YYYYMM的字串；例如：201501。
* 當其他頻率時，回傳格式為YYYYMMDD的字串；例如：20150103。

範例：

```xs
var: string1("");
string1 = formatMQY(date); //將日期轉換為MQY格式的字串
print(date," ",string1);
```
