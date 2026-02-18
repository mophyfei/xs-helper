# StringToTime -  (內建函數)（時間函數）

## 語法

把傳入的字串轉成時間  
時間數值 = StringToTime( 時間字串)  
時間字串的格式為"HH:MM:SS"，而回傳的時間格式為HHMMSS的6碼數字  
時間字串的格式也可以是"HH:MM:SS.fff"，回傳的時間格式則為HHMMSS.fff的9碼數字

## 說明

範例:

```xs
if CurrentTime > StringToTime("13:00:00") Then Begin
	// 目前時間大於中午13:00:00
End;


if CurrentTimeMS > StringToTime("13:00:00.500") Then Begin
	// 目前時間大於中午13:00:00.500
End;
```

請參考[TimeToString函數](api?a=timetostring&b=bif)。
