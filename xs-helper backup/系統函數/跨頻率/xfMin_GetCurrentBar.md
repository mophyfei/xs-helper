# xfMin_GetCurrentBar -  (系統函數)（跨頻率）

## 語法

傳回指定頻率的K棒編號。  
K棒編號 = xfMin\_GetCurrentBar(頻率)  
傳入一個參數:  
- 第一個參數是頻率，指定傳入數列的資料期別，支援"1","5","10","15","30","60","D", "W", "M", "AD", "AW", "AM"。  
備註：商品類型僅支援台股與台期權。不支援XS選股、XS選股自訂排行與XS選股回測。

## 說明

傳回指定頻率（支援分鐘）的K棒序列編號，由1開始，第一筆K棒編號為1，第二筆K棒編號為2，依序遞增。

可以使用這個函數來判斷目前腳本執行的時機點

```xs
value1 = xfMin_GetCurrentBar(FreqType);


if Length + 1 = 0 then Factor = 1 else Factor = 2 / (Length + 1);


if value1 = 1 then
    xfMin_XAverage = Series
else
    xfMin_XAverage = lastXAverage + Factor * (Series - lastXAverage);
```

上述範例利用xfMin\_GetCurrentBar來判斷目前是否是第一筆K棒。如果是的話則回傳xfMin\_XAverage的初始數值。
