# GetSymbolFieldDate -  (內建函數)（欄位函數）

## 語法

讀取系統內的欄位的資料日期  
欄位資料日期 = GetSymbolFieldDate("ID", "欄位名稱")  
欄位資料日期 = GetSymbolFieldDate("ID", "欄位名稱","頻率")  
傳入欄位名稱，跟指定的欄位頻率。如果不傳頻率的話，則使用目前執行頻率。回傳欄位的資料日期，格式為YYYYMMDD 8碼數字。

## 說明

GetSymbolFieldDate是GetFieldDate語法的延伸，在取得欄位相關資料的日期時可以指定商品，透過這個函數可以在腳本中取得其他商品欄位的資料日期。

GetSymbolFieldDate函數可以傳入三個參數：

* 第一個參數是商品代碼，
* 第二個參數是欄位的中文或是英文名稱，
* 第三個參數則是欄位的頻率，如果省略這個參數的話，則會依照目前腳本執行的頻率來取得對應的資料

以下是一個簡單的範例（選股腳本）：

```xs
GetSymbolFieldDate("2330.TW","月營收");
GetSymbolFieldDate("2330.TW","月營收","M");
ret=1;
```

詳細的語法說明可以參考 [GetFieldDate](api?a=GetFieldDate&b=bif)函數。
