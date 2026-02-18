# Stochastic -  (系統函數)（技術指標）

## 語法

計算KD指標。  
回傳數值=Stochastic(資料期數,K值平滑期數,D值平滑期數,輸出RSV值,輸出K值,輸出D值)  
傳入六個參數:  
  
- 第一個參數是資料期數，指定計算的區間長度。  
- 第二個參數是K值平滑期數，指定計算K值所用的平滑期數。  
- 第三個參數是D值平滑期數，指定計算D值所用的平滑期數。  
- 第四個參數是輸出RSV值，回傳計算完的RSV值。  
- 第五個參數是輸出K值，回傳計算完的K值。  
- 第六個參數是輸出D值，回傳計算完的D值。

## 說明

KD指標為美國交易員George Lane所創，原名為Stochastic Oscillator。

Stochastic函數可計算KD指標的RSV、K及D三個數值。Stochastic函數回傳1時，代表計算成功。RSV、K及D的值是回傳在第4、5、6個參數。

範例：

```xs
value1 = Stochastic(9,3,3,value2,value3,value4);       //計算KD指標
plot1(value3, "K");
plot2(value4, "D");
```
