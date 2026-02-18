# IsSupportSymbolField -  (內建函數)（欄位函數）

## 語法

判斷傳入的商品代碼、欄位和頻率是否為可支援的欄位。  
IsSupportSymbolField("TSE.TW", "月營收");

## 說明

IsSupportSymbolField 可根據傳入的商品代碼、欄位和頻率，判斷指定的欄位是否存在，回傳True / False。

需注意此函數並不會判斷對應K棒上是否有資料，只會判斷欄位存在。
故可能會發生函數回傳True，但該根K棒上沒有資料的情況。
