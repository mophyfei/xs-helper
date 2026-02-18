# FilledRecordIsRealtime -  (內建函數)（交易函數）

## 語法

判斷某一筆成交紀錄是發生在策略部位計算區間或是即時K棒洗價區間  
Value1 = FilledRecordIsRealtime(idx)

## 說明

FilledRecordIsRealtime必須傳入一個index參數，代表要取得第幾筆成交紀錄的日期，index的範圍從1開始(第一筆)，不能超過FilledRecordCount。

如果策略啟動了[策略部位計算功能](https://www.xq.com.tw/lesson/xsat/xsat_overview/#fun)的話，在執行過程內系統會使用歷史資料來模擬策略的交易狀態。

如果要判斷某一筆成交紀錄是發生在策略部位計算區間或是發生在即時K棒區間的話，可以透過FilledRecordIsRealtime這個函數。發生在即時K棒區間的成交紀錄會回傳1，發生在策略部位計算區間的則回傳0。

如果策略是用回測方式執行的話，則回傳0 (相當於策略部位計算區間)。
