# DefaultSellPrice -  (內建函數)（交易函數）

## 語法

回傳自動交易預設的賣出價格。  
Value1 = DefaultSellPrice

## 說明

自動交易策略可以設定預設的買進(賣出)價格，指定的方式可以是

* 市價 (如果是期貨的話，則是範圍市價)
* 觸發價 +/- 檔數

![defaultsellprice](https://www.xq.com.tw/wp-content/uploads/2020/10/defaultsellprice.png)

如果策略指定預設賣出價格為市價的話，DefaultSellPrice會回傳商品的跌停價(如果商品沒有漲停跌停價的話，則回傳洗價K棒的收盤價)。如果指定的是觸發價 +/- 檔數的話，則回傳依照洗價K棒的收盤價加減檔數後的價格。與[AddSpread](api?a=AddSpread&b=bif)一樣，加減檔數後的數值不會超過商品的漲跌停限制。

使用者可以利用這個函數，取得交易指令的實際委託價格，可以應用於，例如利用金額來換算委託數量。

```xs
input: ordersize_w(10, "每筆交易金額(萬)");


{ 換算委託張數 }
value1 = (ordersize_w * 10000) / (DefaultSellPrice * 1000);
SetPosition(-1 * value1);
```
