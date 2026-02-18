# xf_DirectionMovement -  (系統函數)（跨頻率）

## 語法

計算跨頻率DMI指標。  
回傳數值=xf\_DirectionMovement(頻率,期數,輸出+DI值,輸出-DI值,輸出ADX值)  
傳入五個參數:  
- 第一個參數是頻率，指定傳入數列的資料期別，支援"D", "W", "M", "AD", "AW", "AM"。  
- 第二個參數是計算期數。  
- 第三個參數是輸出計算完的+DI值。  
- 第四個參數是輸出計算完的-DI值。  
- 第五個參數是輸出計算完的ADX值。

## 說明

xf\_DirectionMovement是[DirectionMovement](api?a=DirectionMovement&b=sys) 函數的跨頻率版本，增加了指定頻率的參數，可以計算指定頻率的DMI值。

範例：

```xs
value1 = xf_DirectionMovement("W",14,value2,value3,value4);       //計算14期的週DMI指標
plot1(value2, "週+DI");
plot2(value3, "週-DI");
plot3(value4, "週ADX");
```
