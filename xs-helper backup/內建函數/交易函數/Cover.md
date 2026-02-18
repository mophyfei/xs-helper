# Cover -  (內建函數)（交易函數）

## 語法

把策略的空單部位變小，進行空單回補(減碼)的動作。  
Cover(空單回補數量)  
Cover(空單回補數量，委託價格)  
Cover(空單回補數量，label:="指令標記")  
Cover(空單回補數量，委託價格, label:="指令標記")

## 說明

Cover函數的作用是把目前空單的部位變小，用來進行空單回補(減碼)的動作。請注意Cover減碼之後的最大部位是0，保證不會把目前的部位變成多單(Position > 0)。

Cover函數的第一個參數是回補部位，必須是一個正整數。第二個參數是此次交易的委託價格，第二個參數如果不傳的話則會使用策略的預設買進價格。

與SetPosition一樣，也可以透過label函數傳入指令標記。

**範例**

```xs
Cover(1);
Cover(1, Close);
Cover(1, MARKET);
Cover(1, label:="回補1張");
```

**注意事項**

Cover指令只有在目前Position < 0時才會有作用。如果Cover的回補數量超過目前空單數量的話，則Position會改成0，也就是說Cover函數不會把目標部位變成多頭(Position > 0)。

Cover(0)是一個特殊用法，如果此時的部位小於0的話，Cover(0)的作用是把部位變成0，如果此時的部位大於0的話，則Cover(0)沒有任何作用。

以下是Cover(N)的執行邏輯：

```xs
if Position < 0 then
    if N = 0 then
        SetPosition(0)  { N=0是特殊用法，把空單部位全部回補 }
    else
        SetPosition(minlist(Position + N, 0)); { 從Position(負數)增加N張，最終數字不會大於0 }
```
