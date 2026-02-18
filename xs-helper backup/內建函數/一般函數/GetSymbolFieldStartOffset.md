# GetSymbolFieldStartOffset -  (內建函數)（一般函數）

## 語法

判斷欄位初始點  
欄位筆數 = GetSymbolFieldStartOffset("ID", "欄位名稱")  
欄位筆數 = GetSymbolFieldStartOffset("ID", "欄位名稱","頻率")  
回傳目前最新一筆欄位與此欄位的第一筆資料間的欄位筆數。  
如果無此欄位，或是欄位的初始點超過目前bar的位置，則回-1。  
※如果不傳頻率的話，則讀取目前執行頻率的對應欄位。  
※僅支援「選股」腳本類型。

## 說明

GetSymbolFieldStartOffset是GetFieldStartOffset語法的延伸，在取得欄位相關資料時可以指定商品，透過這個函數可以在腳本中取得其他商品的欄位筆數。

以下是一個簡單的範例：

```xs
Value1 = GetSymbolFieldStartOffset("1101.TW", "月營收");　// value1 為取得目前腳本執行頻率的台泥(1101)目前最新一筆月營收欄位與月營收欄位第一筆資料間的欄位筆數。
Value2 = GetSymbolFieldStartOffset("1101.TW", "月營收", "M");　// value2 為取得月頻率的台泥(1101)目前最新一筆月營收欄位與月營收欄位第一筆資料間的欄位筆數。
```

詳細的語法說明可以參考 [GetFieldStartOffset](api?a=GetFieldStartOffset&b=bif)函數。
