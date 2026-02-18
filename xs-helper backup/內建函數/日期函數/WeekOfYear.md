# WeekOfYear -  (內建函數)（日期函數）

## 語法

計算傳入日期的是這一年的第幾個星期  
第幾個星期 = WeekOfYear(日期)  
回傳數值的範圍從1到53

## 說明

日期數值是一個8碼的數字，格式為YYYYMMDD:

* YYYY是西元年份，例如2015，
* MM是月份，兩位數字，範圍從01到12，
* DD是日期，兩位數字，範圍從01到31，

日期數值通常是透過CurrentDate，或是Date(資料的日期欄位)，或是其他日期相關函數所產生的日期數值。

回傳的數值則是這一年第幾個星期，可能的數值從1到53。

```xs
Value1 = WeekOfYear(20150101);   // Value1 = 1 (第一週)
```

日期相關的函數請參考[Year函數](api?a=year&b=bif)，[Month函數](api?a=month&b=bif)，[DayOfMonth函數](api?a=dayofmonth&b=bif)，[DayOfWeek函數](api?a=dayofweek&b=bif)，[WeekOfMonth函數](api?a=weekofmonth&b=bif)，以及[WeekOfYear函數](api?a=weekofyear&b=bif)。
