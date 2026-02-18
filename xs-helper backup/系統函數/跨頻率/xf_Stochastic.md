# xf_Stochastic -  (系統函數)（跨頻率）

## 語法

計算指定頻率的KD指標。  
回傳數值=xf\_Stochastic(頻率,資料期數,K值平滑期數,D值平滑期數,輸出RSV值,輸出K值,輸出D值)  
傳入八個參數:  
- 第一個參數是頻率，指定傳入數列的資料期別，支援"D", "W", "M", "AD", "AW", "AM"。  
- 第二個參數是資料期數，指定計算的區間長度。  
- 第三個參數是K值平滑期數，指定計算K值所用的平滑期數。  
- 第四個參數是D值平滑期數，指定計算D值所用的平滑期數。  
- 第五個參數是輸出RSV值，回傳計算完的RSV值。  
- 第六個參數是輸出K值，回傳計算完的K值。  
- 第七個參數是輸出D值，回傳計算完的D值。

## 說明

xf\_Stochastic是[Stochastic](api?a=Stochastic&b=sys) 函數的跨頻率版本，增加了指定頻率的參數，可以計算指定頻率的Stochastic值。

範例：

```xs
value1 = xf_Stochastic("W",9,3,3,value2,value3,value4);       //計算週KD指標
plot1(value3, "週K");
plot2(value4, "週D");
```
