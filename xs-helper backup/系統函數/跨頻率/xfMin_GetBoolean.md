# xfMin_GetBoolean -  (系統函數)（跨頻率）

## 語法

引用指定頻率的數值。  
回傳數值=xfMin\_GetBoolean(頻率,數列,期別)  
傳入三個參數:  
- 第一個參數是頻率，指定傳入數列的資料期別，支援"1","5","10","15","30","60","D", "W", "M", "AD", "AW", "AM"。  
- 第二個參數是布林數列。  
- 第三個參數是期別，相對目前而言要往前的筆數。  
備註：商品類型僅支援台股與台期權。不支援XS選股、XS選股自訂排行與XS選股回測。

## 說明

在同一個頻率時，我們可以直接利用\*\*變數[3]\*\*取得前3期的變數值。當資料頻率不同時（跨頻率），我們就需要使用xfMin\_GetValue或xfMin\_GetBoolean來取得之前的變數值。若變數是數值時，要用xfMin\_GetValue；若變數是布林值時，要用xfMin\_GetBoolean。支援跨分鐘頻率。

```xs
input:Length_Min(9,"跨分鐘頻率期數");
variable:rsv_w(0),kk_w(0),dd_w(0);
xfMin_stochastic("30", Length_Min, 3, 3, rsv_w, kk_w, dd_w);
condition1 = xfMin_GetBoolean("30",xfMin_crossover("30", kk_w, dd_w),1);	//在15分鐘線抓30分鐘線KD黃金交叉
```

相關函數：[xfMin\_GetValue](api?a=xfMin_GetValue&b=sys)。
