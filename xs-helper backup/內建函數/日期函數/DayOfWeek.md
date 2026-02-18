# DayOfWeek -  (內建函數)（日期函數）

## 語法

計算傳入日期的是這個星期的第幾天  
第幾天 = DayOfWeek(日期)  
如果是星期日的話則回傳0, 星期一的話則回傳1, 由此類推

## 說明

日期數值是一個8碼的數字，格式為YYYYMMDD:

* YYYY是西元年份，例如2015，
* MM是月份，兩位數字，範圍從01到12，
* DD是日期，兩位數字，範圍從01到31，

日期數值通常是透過CurrentDate，或是Date(資料的日期欄位)，或是其他日期相關函數所產生的日期數值。

回傳的數值則是這個日期是這個星期的第幾天，可能的數值從0(星期日)到6(星期六)。

範例:

```xs
If DayOfWeek(Date) = 1 Then
Begin
	// 目前K棒資料日期是星期一
End;
```

日期相關的函數請參考[Year函數](api?a=year&b=bif)，[Month函數](api?a=month&b=bif)，[DayOfMonth函數](api?a=dayofmonth&b=bif)，[DayOfWeek函數](api?a=dayofweek&b=bif)，[WeekOfMonth函數](api?a=weekofmonth&b=bif)，以及[WeekOfYear函數](api?a=weekofyear&b=bif)。
