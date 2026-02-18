# DaysToExpiration -  (系統函數)（日期相關）

## 語法

計算台股指數類期貨商品的到期天數。  
回傳數值=DaysToExpiration(商品月份,商品年份)  
傳入二個參數:  
- 第一個參數是商品月份。  
- 第二個參數是商品年份。

## 說明

台股指數類期貨商品的到期日期為該月的第三個星期三。此函數會計算特定合約距當期K棒的天數。

回傳值為1，表示當天為結算日。回傳值小於1，表示該合約已到期。

範例：

```xs
value1 = DaysToExpiration(month(date),year(date));
if value1 <= 1 then begin
	value2 = dateadd(date,"M",1);
	value2 = encodedate(year(value2),month(value2),1);
	value1 = DaysToExpiration(month(value2),year(value2));
end;
plot1(value1); //繪製最新的台股指數類期貨到期天數的連線
```

注意，此函數並無調整因放假而導致的到期日異動。
