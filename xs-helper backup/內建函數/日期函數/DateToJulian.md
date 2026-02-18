# DateToJulian -  (內建函數)（日期函數）

## 語法

將YYYYMMDD的8碼日期轉成儒略日(Julian)格式  
儒略日 = DateToJulian(日期數值)

## 說明

在XS系統內日期的標準格式為YYYYMMDD的8碼數字。如果需要執行日期的計算時，一般可以使用[DateAdd函數](api?a=dateadd&b=bif)或是[DateDiff函數](api?a=datediff&b=bif)。

另外一種計算方式，則是把日期轉換成[儒略日格式](https://zh.wikipedia.org/wiki/%E5%84%92%E7%95%A5%E6%97%A5)後再來計算。因為儒略日格式採用**絕對天數**的方式來紀錄日期數值，所以可以直接做數值運算，然後再使用[JulianToDate函數](api?a=JulianToDate&b=bif)轉成YYYYMMDD的8碼日期格式。

```xs
Value1 = DateToJulian(20150601);  // 把20150601轉成Julian格式
Value1 = Value1 + 1;                        // 直接加1天
Value2 = JulianToDate(Value1);       // Value2 = 20150602
```
