# Playsound -  (內建函數)（一般函數）

## 語法

播放指定的音訊檔案

## 說明

在執行該行腳本時，播放設定的音訊檔案

若同一次腳本運算中執行了複數個 PlaySound 函數，只會撥放最後執行的音訊檔案。

在指定檔案時，若沒有指定絕對路徑的話，會從預設資料夾 C:\SysJust\XQ2005\User\Sound 搜尋符合的檔案。

範例:

```xs
PlaySound("GML.wav");
PlaySound("C:\SysJust\XQ2005\User\Sound\GML.wav");
```
