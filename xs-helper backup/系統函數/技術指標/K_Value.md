# K_Value -  (系統函數)（技術指標）

## 語法

計算KD指標中的K值。  
回傳數值=K\_Value(資料期數,K值平滑期數)  
傳入二個參數:  
- 第一個參數是資料期數，指定計算的區間長度。  
- 第二個參數是K值平滑期數，指定計算K值所用的平滑期數。

## 說明

KD指標為美國交易員George Lane所創，原名為Stochastic Oscillator。
K\_Value即隨機指標中的快速線（%K）。

範例：

```xs
value1 = K_Value(9,3);       //計算KD指標中的K值
plot1(value1, "K");
```
