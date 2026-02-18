# DirectionMovement -  (系統函數)（技術指標）

## 語法

計算DMI指標。  
回傳數值=DirectionMovement(期數,輸出+DI值,輸出-DI值,輸出ADX值)  
傳入四個參數:  
- 第一個參數是計算期數。  
- 第二個參數是輸出計算完的+DI值。  
- 第三個參數是輸出計算完的-DI值。  
- 第四個參數是輸出計算完的ADX值。

## 說明

DMI趨向指標是威爾德（Wilder）所發明。主要的用途在於作趨勢成立的判斷，因此是屬於較為長期交易的技術指標。

DMI指標包含了+DI、-DI及ADX三個數值。DirectionMovement函數回傳1時，代表計算成功。+DI、-DI及ADX的值是回傳在第2、3、4個參數。

範例：

```xs
value1 = DirectionMovement(14,value2,value3,value4);       //計算14期的DMI指標
plot1(value2, "+DI");
plot2(value3, "-DI");
plot3(value4, "ADX");
```
