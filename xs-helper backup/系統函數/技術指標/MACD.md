# MACD -  (系統函數)（技術指標）

## 語法

計算MACD指標值。  
回傳數值=MACD(數列,短期數,長期數,MACD平滑期數,輸出DIF值,輸出MACD值,輸出OSC值)  
傳入七個參數:  
- 第一個參數是數列，MACD通常是以加權平均收盤價（WeightedClose）來計算。  
- 第二個參數是計算快速線（短期）的期數。  
- 第三個參數是計算慢速線（長期）的期數。  
- 第四個參數是計算MACD使用之平滑期數。  
- 第五個參數是輸出計算完的DIF值。  
- 第六個參數是輸出計算完的MACD值。  
- 第七個參數是輸出計算完的OSC值。

## 說明

MACD是由Gerald Appel於1970年代所發明的指標。利用二條快速與慢速指數移動平均線的收斂或發散來判斷價格走勢。

MACD指標包含了DIF、MACD及OSC三個數值。MACD函數回傳1時，代表計算成功。DIF、MACD及OSC的值是回傳在第5、6、7個參數。

範例：

```xs
value1 = MACD(WeightedClose,12,26,9,value2,value3,value4);       //計算MACD
plot1(value2, "DIF");
plot2(value3, "MACD");
plot3(value4, "OSC");
```
