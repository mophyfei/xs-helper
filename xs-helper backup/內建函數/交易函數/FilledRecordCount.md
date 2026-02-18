# FilledRecordCount -  (內建函數)（交易函數）

## 語法

回傳策略執行到目前為止的成交紀錄筆數  
Value1 = FilledRecordCount

## 說明

腳本執行過程內，系統會紀錄每一筆成交紀錄，依照成交時間排序，FilledRecordCount則代表這些成交紀錄的筆數。透過FilledRecordCount，以及其他FilledRecord開頭的函數，包含[FilledRecordDate](api?a=FilledRecordDate&b=bif)，[FilledRecordTime](api?a=FilledRecordTime&b=bif)，[FilledRecordPrice](api?a=FilledRecordPrice&b=bif)，[FilledRecordBS](api?a=FilledRecordBS&b=bif)，[FilledRecordQty](api?a=FilledRecordQty&b=bif)等，來取得完整的成交紀錄資訊。

**範例**

```xs
var: idx(0);
for idx = 1 to FilledRecordCount begin
    value1 = FilledRecordDate(idx);   { 成交日期 }
    value2 = FilledRecordTime(idx);   { 成交時間 }
    value3 = FilledRecordPrice(idx);   { 成交價格 }
    value4 = FilledRecordQty(idx);   { 成交數量 }
    value5 = FilledRecordBS(idx);   { 成交方向: 買進或是賣出 }
    value6 = FilledRecordIsRealtime(idx); { 是否是在即時區間成交 }
end;
```
