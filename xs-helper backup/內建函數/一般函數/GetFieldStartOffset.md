# GetFieldStartOffset -  (內建函數)（一般函數）

## 語法

判斷欄位初始點  
欄位筆數 = GetFieldStartOffset("欄位名稱")  
欄位筆數 = GetFieldStartOffset("欄位名稱", "頻率")  
回傳目前最新一筆欄位與此欄位的第一筆資料間的欄位筆數。  
如果無此欄位，或是欄位的初始點超過目前bar的位置，則回-1。  
※如果不傳頻率的話，則讀取目前執行頻率的對應欄位。  
※僅支援「選股」腳本類型。

## 說明

請注意，回傳欄位的筆數，是依照傳入的欄位頻率來計算，可能與目前腳本執行的K棒頻率不同。
例如目前可能是跑日線，然後GetFieldStartOffset要查的是月頻率欄位，此時 GetFieldStartOffset所回傳的是月頻率欄位有幾筆。

**選股腳本範例：判斷當月營收是否創掛牌新高**

```xs
value1 = GetFieldStartOffset("月營收", "M");
if value1 = 0 then begin 
    ret = 1;  // 只有1期, 就當成創新高了吧
	outputField1(GetField("月營收", "M"),"月營收");
end else if value1 > 0 then begin
    // 算出前N期的最大值
    value2 = Highest(GetField("月營收", "M")[1], value1);
    if GetField("月營收", "M") > value2 then ret = 1;
	outputField1(GetField("月營收", "M"),"月營收");
end;
```

**範例選股範例：判斷創N期新高**

```xs
input: min_period(12, "最低期別");
value1 = GetFieldStartOffset("月營收", "M");
value2 = GetField("月營收", "M");// 最新一期營收
var: idx(0);
idx = 1;
while idx <= value1 begin
    if GetField("月營收", "M")[idx] < value2 then
        idx = idx + 1
    else
        break;
end;     
if idx >= min_period then begin
    ret=1;
	OutputField(1, idx, 0, "創新高期別");
	OutputField(2, GetField("月營收", "M")[idx], "創新高的月營收");
	OutputField(3, GetFielddate("月營收", "M")[idx], 0, "創新高的月營收資料日期");
end;
```
