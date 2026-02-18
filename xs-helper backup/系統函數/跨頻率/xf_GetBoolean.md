# xf_GetBoolean -  (系統函數)（跨頻率）

## 語法

引用指定頻率的數值。  
回傳數值=xf\_GetBoolean(頻率,數列,期別)  
傳入三個參數:  
- 第一個參數是頻率，指定傳入數列的資料期別，支援"D", "W", "M", "AD", "AW", "AM"。  
- 第二個參數是目標頻率的布林數列。  
- 第三個參數是期別，相對目前而言要往前的筆數。

## 說明

在同一個頻率時，我們可以直接利用\*\*變數[3]\*\*取得前3期的變數值。當資料頻率不同時（跨頻率），我們就需要使用xf\_GetValue或xf\_GetBoolean來取得之前的變數值。若變數是數值時，要用xf\_GetValue；若變數是布林值時，要用xf\_GetBoolean。

```xs
input:Length_W(9,"跨頻率週期數");
variable:rsv_w(0),kk_w(0),dd_w(0);
xf_stochastic("W", Length_W, 3, 3, rsv_w, kk_w, dd_w);
condition1 = xf_GetBoolean("W",xf_crossover("W", kk_w, dd_w),1);	//在日線抓周KD黃金交叉
```

相關函數：[xf\_GetValue](api?a=xf_GetValue&b=sys)。
