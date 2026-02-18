# xfMin_DirectionMovement -  (系統函數)（跨頻率）

## 語法

計算跨頻率DMI指標。  
回傳數值=xfMin\_DirectionMovement(頻率,期數,輸出+DI值,輸出-DI值,輸出ADX值)  
傳入五個參數:  
- 第一個參數是頻率，指定傳入數列的資料期別，支援"1","5","10","15","30","60","D", "W", "M", "AD", "AW", "AM"。  
- 第二個參數是計算期數。  
- 第三個參數是輸出計算完的+DI值。  
- 第四個參數是輸出計算完的-DI值。  
- 第五個參數是輸出計算完的ADX值。  
備註：商品類型僅支援台股與台期權。不支援XS選股、XS選股自訂排行與XS選股回測。

## 說明

xfMin\_DirectionMovement是[xf\_DirectionMovement](api?a=xf_DirectionMovement&b=sys) 函數的跨頻率加強版本，增加了指定分鐘頻率的參數，可以計算指定分鐘頻率的DMI值。

範例：

```xs
value1 = xfMin_DirectionMovement("30",14,value2,value3,value4);       //計算14期的30分鐘線DMI指標
plot1(value2, "30分+DI");
plot2(value3, "30分週-DI");
plot3(value4, "30分ADX");
```
