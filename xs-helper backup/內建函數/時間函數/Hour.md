# Hour -  (內建函數)（時間函數）

## 語法

計算傳入時間的小時數值  
小時 = Hour(時間數值)  
回傳小時的數值範圍從0到23 (24小時制)

## 說明

時間數值是一個5~6碼的數字，格式是HHMMSS.fff:

* HH是小時，數值範圍從0到23，可以是1位數字或是2位數字
* MM是分鐘，數值範圍從00到59，必須是2位數字
* SS是秒數，數值範圍從00到59，必須是2位數字

時間數值通常是透過CurrentTime，Time(資料的時間欄位)，或是其他時間相關函數所產生的時間數值。

```xs
Value1 = Hour(Time);


If Value1 = 12 Then
Begin
	// 目前分鐘K棒的資料時間是12點
End;
```

上述範例利用Hour取得目前分鐘K棒資料時間的小時數值。

```xs
Value2 = Hour(CurrentTime);
If Value2 >= 12 Then
Begin
	// 目前時間是中午12點過後
End;
```

上述範例則是傳入[CurrentTime](api?a=currentime&b=bif)，也就是目前電腦的時間，格視為 HHMMSS。

---

也可以傳入含有毫秒的時間數值 [CurrentTimeMS](api?a=currentimems&b=bif) 回傳格式為 HHMMSS.fff

* HH是小時，數值範圍從0到23，可以是1位數字或是2位數字
* MM是分鐘，數值範圍從00到59，必須是2位數字
* SS是秒數，數值範圍從00到59，必須是2位數字
* fff是毫秒，數值範圍從000到999，必須是3位數字

```xs
Value3 = Hour(CurrentTimeMS);
If Value3 >= 12 Then
Begin
	// 目前時間是中午12點過後
End;
```

上述範例是傳入[CurrentTimeMS](api?a=currentimems&b=bif)，也是目前電腦的時間，不過是含有毫秒的電腦的時間，格式為 HHMMSS.fff。

時間相關的的函數請參考[Hour函數](api?a=hour&b=bif)，[Minute函數](api?a=minute&b=bif)，以及[Second函數](api?a=second&b=bif)。
