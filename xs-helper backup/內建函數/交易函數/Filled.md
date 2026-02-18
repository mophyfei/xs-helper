# Filled -  (內建函數)（交易函數）

## 語法

取得目前商品的成交部位。  
Value1 = Filled

## 說明

Filled代表的是自動交易策略內目前執行商品的成交部位。

Filled是一個整數，可以大於0、等於0、也可以小於0。從自動交易腳本內，透過呼叫交易指令，例如[SetPosition](api?a=SetPosition&b=bif)、[Buy](api?a=Buy&b=bif)、[Sell](api?a=Sell&b=bif)，來執行交易的動作。系統執行交易指令之後，[Position](api?a=Position&b=bif)的數值會跟著異動，如果個交易指令成交之後，則Filled的數值也會跟著異動。

舉例而言，假如腳本開始執行時Position是0，Filled也是0。

當腳本呼叫SetPosition(1)之後，系統會送出一筆買進1張的委託，Position會變成1，此時如果尚未成交的話，Filled還是0，等到這一筆委託單成交之後，Filled就會變成1。

如果委託單部分成交的話，則Filled也會反應部分成交的狀態。例如腳本呼叫SetPosition(2)之後，系統會送出一筆買進2張的委託，Position會變成2。此時如果尚未成交的話，Filled是0，如果這一筆委託單成交1張的話，Filled會變成１，等到這一筆委託單完成成交時，Filled就會變成2。

交易腳本在每一次執行(洗價)之前系統會決定Filled的數值，接下來的執行過程內，不管有沒有收到成交回報，Filled的數值都不會改變。

如同Position，Filled也是一個數值序列。Filled或是Filled[0]代表的是目前這一根bar的成交部位數值。Filled[1]則是上一根bar執行時的成交部位數值。如果開啟逐筆洗價的話，每一根bar內可能會執行多次，Filled的數值則會在每一次洗價之前就會依照當時的成交狀態更新。

透過比對Position跟Filled，腳本可以判斷目前腳本的成交情形，進而做不同的後續處理：

**範例#1**

```xs
if Position = 0 and entry_condition then SetPosition(1);
if Filled = 1 then begin
   if Close >= FilledAvgPrice * 1.02 then SetPosition(0);
   if Close <= FilledAvgPrice * 0.98 then SetPosition(0);
end;
```

在這個範例內腳本透過判斷Filled是否是1來決定是否已經成交。一旦成交之後，則透過比對目前價格與未平倉成本[FilledAvgPrice](api?a=FilledAvgPrice&b=bif)是否達到停損停利的範圍來決定是否要進行平倉。

**範例#2**

```xs
if Position = 0 and entry_condition then SetPosition(1);
if TrueAll(Position <> Filled, 3) then SetPosition(1, MARKET);
```

在這個範例內腳本透過比對Position跟Filled的差異來決定是否尚未成交。如果Position跟Filled連續3根bar都不一樣的話，那麼就改用市價買進。
