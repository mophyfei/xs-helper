# FilledRecordTime -  (內建函數)（交易函數）

## 語法

取得某一筆成交紀錄的成交時間，回傳時間格式HHMMSS  
Value1 = FilledRecordTime(idx)

## 說明

FilledRecordTime必須傳入一個index參數，代表要取得第幾筆成交紀錄的時間，index的範圍從1開始(第一筆)，不能超過FilledRecordCount。

回傳一個最多6位的數字，例如103025，代表10點30分25秒。
