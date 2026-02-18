# calcvwapdistribution -  (系統函數)（交易相關）

## 語法

計算過去N日的VWAP分佈。  
calcvwapdistribution(計算天數，開始時間，結束時間，一個array)

## 說明

計算過去N日的VWAP分佈

請傳入

* 計算天數
* 開始時間, 例如091000
* 結束時間, 例如095900 (請注意請以1分K的Time為基準)
* 一個array, 用來儲存上述指定區間內每分鐘的累積成交量分佈%,
  + CalcVWAPDistribution會自動設定array的大小,
  + array[1]是從開始時間後第1分鐘的累計成交量%, array[2]是從開始時間到後第2分鐘的累計成交量%, etc.
  + 請注意這是一個累積的數值, 例如array[1] = 2.5, array[2] = 5.4, array[3] = 7.0, ... array[最後一個]=100.0,
