# StrCompare -  (內建函數)（字串函數）

## 語法

比較字串是否相同  
數值 = StrCompare(字串1, 字串2)  
數值 = StrCompare(字串1, 字串2, 不區分大小寫)  
傳入二個以上參數:  
- 第一個參數是字串1。  
- 第二個參數是字串2。  
- 第三個參數是選用參數，True表示不區分大小寫，False代表區分大小寫，預設為不區分大小寫。

## 說明

比較二個字串是否相同
當回傳值為 0 時，表示字串1和字串2一樣。
當回傳值為 1 時，表示字串1順序大於字串2。
當回傳值為 -1 時，表示字串1順序小於字串2。

```xs
//預設為不區分大小寫，所以下列二種得到的結果是一樣的
if StrCompare(symbol,"2330.tw") = 0 then plot1(1) else plot1(0);
if StrCompare(symbol,"2330.TW") = 0 then plot2(1) else plot2(0);
//假設主商品是台積電，當區分為大小寫時，plot3會是0、plot4會是1
if StrCompare(symbol,"2330.tw",false) = 0 then plot3(1) else plot3(0);
if StrCompare(symbol,"2330.TW",false) = 0 then plot4(1) else plot4(0);
```
