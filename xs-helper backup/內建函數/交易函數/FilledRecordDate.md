# FilledRecordDate -  (內建函數)（交易函數）

## 語法

取得某一筆成交紀錄的成交日期  
Value1 = FilledRecordDate(idx)

## 說明

FilledRecordDate必須傳入一個index參數，代表要取得第幾筆成交紀錄的日期，index的範圍從1開始(第一筆)，不能超過FilledRecordCount。

回傳一個8位的數字，例如20200110，代表2020年1月10日。
