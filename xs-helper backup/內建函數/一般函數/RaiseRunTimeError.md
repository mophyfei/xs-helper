# RaiseRunTimeError -  (內建函數)（一般函數）

## 語法

用來中斷執行中的程式  
RaiseRunTimeError(錯誤訊息)

## 說明

當腳本遇到任何重大錯誤時，可以使用RaiseRunTimeError函數來終止腳本的執行。

舉例而言:

```xs
if q_CurrentShareCapital < 100000000{100,000,000股*10 = 10億} then RaiseRunTimeError("市值小於10億踢除");
```

上述是一個警示腳本，透過 [q\_CurrentShareCapital](api?a=q_CurrentShareCapital) 欄位來判斷商品的股本是否小於10億，如果小於10億是的話則中斷執行。

與下列程式比較:

```xs
if q_CurrentShareCapital < 100000000{100,000,000股*10 = 10億} then return;
```

請注意如果是使用return指令的話，則執行的這一筆bar雖然會被跳出，可是當還有新的K棒時，程式還是會繼續執行，如果判斷是否要跳出的邏輯比較複雜的話，可能會有一些效率上的影響。如果已經確定腳本不需要再執行的話，可以使用RaiseRuntimeError，比較有效率，而且執行的畫面上也可以看到錯誤訊息，方便使用者掌握腳本的狀態。
