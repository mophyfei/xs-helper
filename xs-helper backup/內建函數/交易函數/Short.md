# Short -  (內建函數)（交易函數）

## 語法

把策略的空單部位變大，進行空單加碼的動作  
Short(空單加碼數量)  
Short(空單加碼數量，委託價格)  
Short(空單加碼數量，label:="指令標記")  
Short(空單加碼數量，委託價格，label:="指令標記")

## 說明

Short函數的作用是把目前空單的部位變大，用來進行空單加碼的動作。

Short函數的第一個參數是加碼部位，必須是一個正整數。第二個參數是此次交易的委託價格，第二個參數如果不傳的話則會使用策略的預設賣出價格。

與SetPosition一樣，也可以透過label函數傳入指令標記。

**範例**

```xs
Short(1);
Short(1, Close);
Short(1, MARKET);
Short(1, label:="放空1張");
```

注意事項

當目前的Position > 0時，執行Short指令時會先把多頭部位全部平倉(部位變成0)，之後再進行空單加碼的動作。

Short(0)是一個特殊用法，如果此時的部位大於0的話，Short(0)的作用是把部位變成0，如果此時的部位小於0的話，則Short(0)沒有任何作用。

以下是Short(N)的執行邏輯：

```xs
if Position > 0 then
    SetPosition(-1 * N)  { 從Position(正數)變成 -N }
else
    SetPosition(Position - N); { 從Position(0或是負數)變成 Position-N }
```
