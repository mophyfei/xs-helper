# FilledAtBroker -  (內建函數)（交易函數）

## 語法

Value1 = FilledAtBroker;

## 說明

此函數會回傳對應商品的實際庫存數量，這個數值會是正、負的整數或 0。

該數值會隨策略設定的現股、資券而取得該類別的庫存數量。

FilledAtBroker 和 Filled 可能會不同，且策略運作時同一個券商帳號內發生的交易都會影響到此函數的回傳數值。

更詳細的說明可參考 [如何使用函數取得商品的實際庫存數量](https://www.xq.com.tw/learn/xspractice/function-inventory/) 此篇文章。
