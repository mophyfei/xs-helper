# TimeValue -  (內建函數)（時間函數）

## 語法

用來取得輸入時間數值的各種不同欄位  
欄位數值 = TimeValue(時間數值，指定欄位)  
指定欄位有 H(回傳小時), M(回傳分鐘), S(回傳秒數), MS(回傳毫秒)

## 說明

時間數值是一個5~6碼的數字，格式是HHMMSS：

* HH是小時，數值範圍從0到23，可以是1位數字或是2位數字
* MM是分鐘，數值範圍從00到59，必須是2位數字
* SS是秒數，數值範圍從00到59，必須是2位數字

時間數值通常是透過CurrentTime，Time(資料的時間欄位)，或是其他時間相關函數所產生的時間數值。

回傳數值依照指定欄位分別定義:

* 如果是 H 的話，則回傳時間的小時數，範圍從0到23(24小時制)
* 如果是 M 的話，則回傳時間的分鐘數，範圍從0到59
* 如果是 S 的話，則回傳時間的秒數，範圍從0到59

---

時間數值也可以是一個含有毫秒的 8~9碼 的數字，格式為HHMMSS.fff：

* HH是小時，數值範圍從0到23，可以是1位數字或是2位數字
* MM是分鐘，數值範圍從00到59，必須是2位數字
* SS是秒數，數值範圍從00到59，必須是2位數字
* fff是毫秒，數值範圍從000到999，必須是3位數字

含有毫秒的時間數值通常是透過CurrentTimeMS，[FilledRecordTimeMS](api?a=FilledRecordTimeMS&b=bif)，或是其他時間相關函數所產生的時間數值。

回傳數值依照指定欄位分別定義:

* 如果是 H 的話，則回傳時間的小時數，範圍從0到23(24小時制)
* 如果是 M 的話，則回傳時間的分鐘數，範圍從0到59
* 如果是 S 的話，則回傳時間的秒數，範圍從0到59
* 如果是 MS 的話，則回傳時間的毫秒，範圍從0到999

以下是一個範例:

```xs
Value1 = TimeValue(CurrentTime, "H");
Value2 = TimeValue(CurrentTimeMS, "MS");


If Value1 >= 12 Then Begin
	// 目前時間是中午12點過後
End;


if Value2 >= 500 Then Begin
	// 目前時間是 500 毫秒過後
End;
```

這個函數可以看成是 [Hour](api?a=hour&b=bif), [Minute](api?a=minute&b=bif), [Second](api?a=second&b=bif), 以及 [MilliSecond](api?a=MilliSecond&b=bif) 還有的綜合體。
