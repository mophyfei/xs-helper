# FilledRecordPrice -  (內建函數)（交易函數）

## 語法

取得某一筆成交紀錄的成交價格  
Value1 = FilledRecordPrice(idx)

## 說明

FilledRecordPrice必須傳入一個index參數，代表要取得第幾筆成交紀錄的時間，index的範圍從1開始(第一筆)，不能超過FilledRecordCount。

不管這一筆成交是買進或是賣出，FilledRecordPrice回傳的數值都是大於0的數字。如果需要判斷是買進或是賣出，請使用[FilledRecordBS](api?a=FilledRecordBS&b=bif)函數。
