# DayOfMonth -  (內建函數)（日期函數）

## 語法

計算傳入日期的是這個月的第幾天  
天數 = DayOfMonth(日期)  
回傳數值的範圍從1到31

## 說明

日期數值是一個8碼的數字，格式為YYYYMMDD:

* YYYY是西元年份，例如2015，
* MM是月份，兩位數字，範圍從01到12，
* DD是日期，兩位數字，範圍從01到31，

日期數值通常是透過CurrentDate，或是Date(資料的日期欄位)，或是其他日期相關函數所產生的日期數值。

回傳的數值則是這個日期是這個月的第幾天，可能的數值從1到31。

舉例：

```xs
Value1 = DayOfMonth(20150601);	// Value1 = 1
Value2 = DayOfMonth(20150630);  // Value2 = 30
```

日期相關的函數請參考[Year函數](api?a=year&b=bif)，[Month函數](api?a=month&b=bif)，[DayOfMonth函數](api?a=dayofmonth&b=bif)，[DayOfWeek函數](api?a=dayofweek&b=bif)，[WeekOfMonth函數](api?a=weekofmonth&b=bif)，以及[WeekOfYear函數](api?a=weekofyear&b=bif)。
