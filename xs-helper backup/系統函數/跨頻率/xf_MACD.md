# xf_MACD -  (系統函數)（跨頻率）

## 語法

計算指定頻率的MACD指標值。  
回傳數值=xf\_MACD(頻率,數列,短期數,長期數,MACD平滑期數,輸出DIF值,輸出MACD值,輸出OSC值)  
傳入八個參數:  
- 第一個參數是頻率，指定傳入數列的資料期別，支援"D", "W", "M", "AD", "AW", "AM"。  
- 第二個參數是目標頻率的數列，MACD通常是以加權平均收盤價（WeightedClose）來計算。  
- 第三個參數是計算快速線（短期）的期數。  
- 第四個參數是計算慢速線（長期）的期數。  
- 第五個參數是計算MACD使用之平滑期數。  
- 第六個參數是輸出計算完的DIF值。  
- 第七個參數是輸出計算完的MACD值。  
- 第八個參數是輸出計算完的OSC值。

## 說明

xf\_MACD是[MACD](api?a=MACD&b=sys) 函數的跨頻率版本，增加了指定頻率的參數，可以計算指定頻率的MACD值。

範例：

```xs
value1 = xf_MACD("W",xf_weightedclose("W"),12,26,9,value2,value3,value4);       //計算週線MACD
plot1(value2, "週DIF");
plot2(value3, "週MACD");
plot3(value4, "週OSC");
```
