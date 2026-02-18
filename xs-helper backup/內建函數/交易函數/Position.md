# Position -  (內建函數)（交易函數）

## 語法

取得目前商品的部位。  
Value1 = Position

## 說明

Position代表的是自動交易策略內目前執行商品的預期部位。

Position是一個整數，可以大於0、等於0、也可以小於0。從自動交易腳本內，透過呼叫交易指令，例如[SetPosition](api?a=SetPosition&b=bif)、[Buy](api?a=Buy&b=bif)、[Sell](api?a=Sell&b=bif)，可以改變Position的數值，當數值變大時，代表要執行買進的動作，當數值變小時則代表要執行賣出的動作。

交易腳本在每一次執行(洗價)之前系統會決定Position的數值，接下來的執行過程內，不管有沒有呼叫交易指令，Position的數值都不會改變。等到執行完成之後，系統會決定要執行哪一個交易指令，之後Position的數值就會更動，同時系統也會決定如何送出委託來達到這個預期的部位。

Position跟Close、Value1等內建的變數一樣，都是一個數值序列。Position或是Position[0]代表的是目前這一根bar的數值。Position[1]則是上一根bar執行時的部位數值。要特別注意的是如果在一根bar內透過逐筆洗價執行了多次交易指令的話，Position的數值會在每一次洗價之後就會異動，也就是說Position是一個[intrabarpersist](api?a=intrabarpersist)的變數。

請參考SetPosition語法內關於[Position異動的時機點](api?a=SetPosition&b=bif#PositionChangeTimingAnchor)的相關說明。
