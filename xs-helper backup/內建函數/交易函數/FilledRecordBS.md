# FilledRecordBS -  (內建函數)（交易函數）

## 語法

取得某一筆成交紀錄的成交方向，如果是買進的話回傳1，如果是賣出的話則回傳-1  
Value1 = FilledRecordBS(idx)

## 說明

FilledRecordBS必須傳入一個index參數，代表要取得第幾筆成交紀錄的日期，index的範圍從1開始(第一筆)，不能超過FilledRecordCount。

如果這一筆成交紀錄是買進的話，則回傳1，如果是賣出的話則回傳-1。
