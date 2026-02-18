# xf_GetDTValue -  (系統函數)（跨頻率）

## 語法

計算指定頻率的序列值。  
回傳數值=xf\_GetDTValue(頻率,日期)  
傳入二個參數:  
- 第一個參數是頻率，指定傳入數列的資料期別，支援"D", "W", "M", "AD", "AW", "AM"。  
- 第二個參數是日期。

## 說明

經由傳入的日期判斷指定頻率的期別是否有異動

```xs
value1 = xf_getdtvalue("W",date);
if value1 <> value1[1] then plot1(1) else plot1(0);
```

上述範例利用xf\_GetDTValue來判斷目前是否為新的一週。如果是的話則在圖表上顯示為1。
