# DateValue -  (內建函數)（日期函數）

## 語法

用來取得輸入日期數值的各種不同欄位  
欄位數值 = DateValue(日期數值，指定欄位)  
指定欄位有 Y(回傳年度), M(回傳月份), D(回傳月份的第幾天), DW(回傳星期幾), WM(回傳月份的第幾週), WY(回傳當年的第幾週)

## 說明

日期數值是一個8碼的數字，格式為YYYYMMDD:

* YYYY是西元年份，例如2015，
* MM是月份，兩位數字，範圍從01到12，
* DD是日期，兩位數字，範圍從01到31，

日期數值通常是透過CurrentDate，或是Date(資料的日期欄位)，或是其他日期相關函數所產生的日期數值。

回傳數值依照指定欄位而有不同:

* 如果是Y，則回傳日期的西元年度
* 如果是M，則回傳日期的月份，範圍從1到12
* 如果是D，則回傳這個月的第幾天，範圍從1到31
* 如果是DW，則回傳星期幾，星期天為0, 星期一為1, 星期二為2, 由此類推
* 如果是WM，則回傳這個月份的第幾週，範圍從1到6
* 如果是WY，則回傳當年的第幾週，範圍從1到53

以下是一個範例:

```xs
Value1 = DateValue(20150601, "Y");  // Value1 = 2015
Value2 = DateValue(20150601, "M");  // Value2 = 6
Value3 = DateValue(20150601, "D");  // Value3 = 1
Value4 = DateValue(20150601, "DW"); // Value4 = 1 (星期一)
Value5 = DateValue(20150601, "WM"); // Value5 = 1 (6月第一週)
Value6 = DateValue(20150601, "WY"); // Value6 = 23 (2015年第23週)
```

這個函數可以看成是[Year函數](api?a=year&b=bif)，[Month函數](api?a=month&b=bif)，[DayOfMonth函數](api?a=dayofmonth&b=bif)，[DayOfWeek函數](api?a=dayofweek&b=bif)，[WeekOfMonth函數](api?a=weekofmonth&b=bif)，以及[WeekOfYear函數](api?a=weekofyear&b=bif)等函數的綜合體。
