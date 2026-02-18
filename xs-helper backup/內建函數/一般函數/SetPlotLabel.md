# SetPlotLabel -  (內建函數)（一般函數）

## 語法

設定繪圖序列的名稱  
SetPlotLabel(繪圖序列編號，繪圖序列名稱)

## 說明

SetPlotLabel傳入兩個參數

* 第一個參數是繪圖序列的編號，可以從1到99，
* 第二個參數則是這個序列的名稱，為一個字串值

這個函數可以用來指定這個繪圖序列的名稱，跟[Plot函數](api?a=plot&b=bif)的第二個參數是類似的。

兩者最大的差異是，在Plot函數內的第二個參數目前只支援固定的字串，而SetPlotLabel的第二個參數則可以是一個字串相關的敘述式，使用上比較有彈性。

舉例而言:

```xs
Input: Period(10);


Plot1(Average(Close, Period));
SetPlotLabel(1, Text("天期(", NumToStr(Period, 0), ")"));
```

在上述範例內我們希望指標圖形上面可以看到平均線的天期，例如如果天期是5的話，我們希望指標序列的名稱是"天期(5)"，而如果天期是10的話，則我們希望指標序列的名稱是"天期(10)"。

由於天期是透過[Input語法](api?a=input)傳入的，數值可以動態被修改，沒有辦法寫成一個固定的字串，所以我們使用SetPlotLabel，搭配[Text函數](api?a=text&b=bif)以及[NumToStr函數](api?a=numtostr&b=bif)來組出天期的字串。
