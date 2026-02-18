# DateToString -  (內建函數)（日期函數）

## 語法

把傳入的日期轉成字串  
日期字串 = DateToString(日期數值)  
回傳的日期字串格式為YYYY/MM/DD

## 說明

日期數值是一個8碼的數字，格式為YYYYMMDD:

* YYYY是西元年份，例如2015，
* MM是月份，兩位數字，範圍從01到12，
* DD是日期，兩位數字，範圍從01到31，

日期數值通常是透過CurrentDate，或是Date(資料的日期欄位)，或是其他日期相關函數所產生的日期數值。

回傳字串的格式是"**YYYY/MM/DD**"，其中YYYY為４位年份，MM為月份，從01到12，DD則是日期，從01到31。

舉例而言，如果目前日期是20150601的話，以下的程式碼

```xs
Print(DateToString(CurrentDate));
```

將會印出 "2015/06/01"的字串。

請參考[StringToDate函數](api?a=stringtodate&b=bif)。
