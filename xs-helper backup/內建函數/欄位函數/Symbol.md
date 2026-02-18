# Symbol -  (內建函數)（欄位函數）

## 語法

回傳目前執行腳本的商品代碼  
回傳代碼 = Symbol

## 說明

Symbol函數回傳目前執行腳本的商品代碼，格式是商品編碼 + '.' + 交易所編碼，例如 "2330.TW"，"TSE.TW"，"IBM.US"等。

如果有多個商品同時執行同一個腳本時(例如策略雷達)，可以利用這個函數來判斷目前執行的商品而做不同的處理，也可以利用這個函數搭配[Print函數](api?a=print&b=bif)來輸出目前執行的商品代碼。

範例:

```xs
If Symbol = "2330.TW" then
begin
    // 目前執行的商品為台積電
end;
```

請參考[SymbolName函數](api?a=symbolname&b=bif)。
