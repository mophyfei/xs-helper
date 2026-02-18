# Sell -  (內建函數)（交易函數）

## 語法

把策略的多單部位變小，進行多單減碼的動作。  
Sell(多單減碼數量)  
Sell(多單減碼數量，委託價格)  
Sell(多單減碼數量，label:="指令標記")  
Sell(多單減碼數量，委託價格，label:="指令標記")

## 說明

Sell函數的作用是把目前多單的部位變小，用來進行多單減碼的動作。請注意Sell減碼之後的最小部位是0，保證不會把目前的部位變成空單(Position < 0)。

Sell函數的第一個參數是減碼部位，必須是一個正整數。第二個參數是此次交易的委託價格，第二個參數如果不傳的話則會使用策略的預設賣出價格。

與SetPosition一樣，也可以透過label函數傳入指令標記。

**範例**

```xs
Sell(1);
Sell(1, Close);
Sell(1, MARKET);
Sell(1, label:="出場1張");
```

**注意事項**

Sell指令只有在目前Position > 0時才會有作用。如果Sell的減碼數量大於目前Position的話，則Position會改成0，也就是說Sell函數不會把目標部位變成空頭(Position < 0)。

Sell(0)是一個特殊用法，如果此時的部位大於0的話，Sell(0)的作用是把部位變成0，如果此時的部位小於0的話，則Sell(0)沒有任何作用。

以下是Sell(N)的執行邏輯：

```xs
if Position > 0 then
    if N = 0 then
        SetPosition(0)  { N=0是特殊用法，把多單部位全部平倉 }
    else
        SetPosition(maxlist(Position - N, 0)); { 從Position(正數)減少N張，最終數字不會小於0 }
```
