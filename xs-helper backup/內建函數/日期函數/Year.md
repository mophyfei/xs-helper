# Year -  (內建函數)（日期函數）

## 語法

計算傳入日期的年度  
年度 = Year(日期)

## 說明

日期數值是一個8碼的數字，格式為YYYYMMDD:

* YYYY是西元年份，例如2015，
* MM是月份，兩位數字，範圍從01到12，
* DD是日期，兩位數字，範圍從01到31，

日期數值通常是透過CurrentDate，或是Date(資料的日期欄位)，或是其他日期相關函數所產生的日期數值。

回傳的數值則是這個日期的所在年度。

舉例：

```xs
Value1 = Year(20150601);　　// Value1 = 2015
```

日期相關的函數請參考[Year函數](api?a=year&b=bif)，[Month函數](api?a=month&b=bif)，[DayOfMonth函數](api?a=dayofmonth&b=bif)，[DayOfWeek函數](api?a=dayofweek&b=bif)，[WeekOfMonth函數](api?a=weekofmonth&b=bif)，以及[WeekOfYear函數](api?a=weekofyear&b=bif)。
