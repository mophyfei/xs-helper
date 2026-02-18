# GetSymbolGroup -  (內建函數)（一般函數）

## 語法

用來取得 執行商品/指定商品 支援的相關商品清單。  
Group: myGroup();  
myGroup = GetSymbolGroup("權證");  
myGroup = GetSymbolGroup("TSE23.TW", "成分股");

## 說明

GetSymbolGroup 可用來取得系統內建的商品清單 。

第一個參數指定要取得清單的商品，例如範例中的 TSE23.TW 。 若未指定商品，則預設取得目前執行商品的清單 （ 前提是該清單存在 ） 。

第二個參數指定所需的清單類型，例如範例中的 成分股 ，此參數為必填 。

可參考[商品清單功能](https://www.xq.com.tw/lesson/xspractice/%E5%95%86%E5%93%81%E6%B8%85%E5%96%AE%E5%8A%9F%E8%83%BD/)。
