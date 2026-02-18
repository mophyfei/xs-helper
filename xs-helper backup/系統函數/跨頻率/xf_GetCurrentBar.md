# xf_GetCurrentBar -  (系統函數)（跨頻率）

## 語法

傳回指定頻率的K棒編號。  
K棒編號 = xf\_GetCurrentBar(頻率)  
傳入一個參數:  
- 第一個參數是頻率，指定傳入數列的資料期別，支援"D", "W", "M", "AD", "AW", "AM"。

## 說明

傳回指定頻率的K棒序列編號，由1開始，第一筆K棒編號為1，第二筆K棒編號為2，依序遞增。

可以使用這個函數來判斷目前腳本執行的時機點

```xs
value1 = xf_GetCurrentBar(FreqType);


if Length + 1 = 0 then Factor = 1 else Factor = 2 / (Length + 1);


if value1 = 1 then
    xf_XAverage = Series
else
    xf_XAverage = lastXAverage + Factor * (Series - lastXAverage);
```

上述範例利用xf\_GetCurrentBar來判斷目前是否是第一筆K棒。如果是的話則回傳xf\_XAverage的初始數值。
