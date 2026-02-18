# IsMarketPrice -  (內建函數)（交易函數）

## 語法

判斷傳入的價格是否是市價  
Condition1 = IsMarketPrice(value1)

## 說明

商品的五檔委買委賣價，或是成交明細資料(Tick資料)的買進價，賣出價有可能會是市價。如果腳本希望判斷這種情形時，可以使用IsMarketPrice這個函數。

```xs
//範例
{ 委買最高價格是市價，表示必須送出市價買進才有機會買到 }
if IsMarketPrice(q_BestBid1) then setposition(1);
```

![市價買](https://www.xq.com.tw/wp-content/uploads/2020/10/%E5%B8%82%E5%83%B9%E8%B2%B7.jpg)

```xs
//範例
{ 委賣最低價格是市價，表示必須送出市價賣出才有機會平倉 }
if IsMarketPrice(q_BestAsk1) then setposition(0);
```

![市價賣](https://www.xq.com.tw/wp-content/uploads/2020/10/%E5%B8%82%E5%83%B9%E8%B3%A3.jpg)
