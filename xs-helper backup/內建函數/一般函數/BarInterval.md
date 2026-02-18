# BarInterval -  (內建函數)（一般函數）

## 語法

傳回執行腳本資料的分鐘頻率間隔  
分鐘區間 = BarInterval  
如果頻率是分鐘資料，則回傳分鐘的間隔，例如30分鐘線的話則回傳30，否則一律回傳1

## 說明

一般而言BarInterval函數會跟[BarFreq](api?a=barfreq&b=sys)一起搭配使用，用來判斷目前執行的分鐘頻率的分鐘間隔。

```xs
// 先判斷目前是分鐘線
If BarFreq = "Min" Then
Begin
	If BarInterval = 30 Then
	Begin
		// 資料為30分鐘線
	End;
End;
```
