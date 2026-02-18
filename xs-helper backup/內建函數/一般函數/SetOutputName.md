# SetOutputName -  (內建函數)（一般函數）

## 語法

指定選股的輸出欄位標題  
SetOutputName(序號, 欄位標題)  
SetOutputName1(欄位標題)

## 說明

在XS語法內可以使用[OutputField指令](api?a=outputfield&b=bif)來產生選股時的輸出欄位。

預設的輸出欄位的標題為 "欄位"加上"序號"，例如"欄位1", "欄位2"等。為了讓輸出報表更清楚，可以使用SetOutputName指令來設定輸出欄位的名稱。

SetOutputName必須傳入兩個參數:

* 第一個參數是欄位的序號，從1到99，
* 第二個參數是欄位的名稱

例如:

```xs
OutputField1(GetField("月營收年增率","M"));
SetOutputName(1, "月營收年增率");
```

在上面範例內指定第一個輸出欄位的標題為"月營收年增率"。

SetOutputField指令也可以在函數名稱之後直接加上序號，例如SetOutputName1, SetOutputName2等。如果函數名稱內就包含序號的話，則就不需要傳入序號參數。

上面的範例可以改寫成:

```xs
OutputField1(GetField("月營收年增率","M"));
SetOutputName1("月營收年增率");
```

在XQ 5.60版之後，[OutputField指令](api?a=outputfield&b=bif)也增加了可以直接傳入欄位標題的功能。
