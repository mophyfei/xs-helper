# xfMin_MACD -  (系統函數)（跨頻率）

## 語法

計算指定頻率的MACD指標值。  
回傳數值=xfMin\_MACD(頻率,數列,短期數,長期數,MACD平滑期數,輸出DIF值,輸出MACD值,輸出OSC值)  
傳入八個參數:  
- 第一個參數是頻率，指定傳入數列的資料期別，支援"1","5","10","15","30","60","D", "W", "M", "AD", "AW", "AM"。  
- 第二個參數是目標頻率的數列，MACD通常是以加權平均收盤價（WeightedClose）來計算。  
- 第三個參數是計算快速線（短期）的期數。  
- 第四個參數是計算慢速線（長期）的期數。  
- 第五個參數是計算MACD使用之平滑期數。  
- 第六個參數是輸出計算完的DIF值。  
- 第七個參數是輸出計算完的MACD值。  
- 第八個參數是輸出計算完的OSC值。  
備註：商品類型僅支援台股與台期權。不支援XS選股、XS選股自訂排行與XS選股回測。

## 說明

xfMin\_MACD是[xf\_MACD](api?a=xf_MACD&b=sys) 函數的跨頻率加強版本，增加了指定分鐘頻率的參數，可以計算指定分鐘頻率的MACD值。

範例：

```xs
value1 = xfMin_MACD("30",xfMin_weightedclose("30"),12,26,9,value2,value3,value4);    //計算30分鐘線MACD
plot1(value2, "30分鐘DIF");
plot2(value3, "30分鐘MACD");
plot3(value4, "30分鐘OSC");
```
