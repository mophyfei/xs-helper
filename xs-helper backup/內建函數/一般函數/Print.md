# Print -  (內建函數)（一般函數）

## 語法

將文字/數值輸出到XSScript編輯器的執行畫面跟檔案內  
Print(數值1, 數值2, 數值3, ...)  
Print(指定檔案,數值1, 數值2, 數值3, ...) ← 交易腳本必須用此法才能列印到檔案。  
※執行選股與執行回測時，Print檔案加總超過 100M 就不會印出。

## 說明

Print函數可以傳入多個參數，使用逗號分隔，參數可以是文字或是數值。每個Print函數會產生一行的輸出，內容為傳入的參數的文字或是數值，每個參數之間有一個空白。

範例:

```xs
Print("Date=", NumToStr(Date, 0), "Close=", NumToStr(Close, 2));
```

把上述指標腳本放入技術分析內，執行時可以在XSScript編輯器的執行畫面內看到輸出，每一筆bar寫出一筆紀錄

![Print紀錄](https://www.xq.com.tw/xstrader/wp-content/uploads/2015/09/Print2.png)

Print函數的執行結果除了在XSScript編輯器內可以看到之外，另外也會產生一個文字檔案。

檔案的輸出位置是在**XQ安裝目錄**底下的**XS\Print**子目錄內，檔案名稱預設為*策略名稱*加上*商品名稱*，檔名為\*\*.log\*\*。以上述為例輸出的檔案名稱為 C:\SysJust\XQ2005\XS\Print\Print範例\_2330.TW.log

策略名稱也就是代表雷達名稱、選股策略名稱、指標名稱或自動交易策略名稱。

使用者也可以利用[File指令](api?a=file&b=bif)來指定輸出的目錄或是檔名，交易腳本必須用此法才能列印到檔案。

以下的範例會把輸出檔案寫在"d:\print"這個目錄內：

```xs
Print(file("d:\print\"), "Date=", NumToStr(Date, 0), "Close=", NumToStr(Close, 2));
```

---

如果需要避開重覆Print在同一個檔案，可以運用[File指令](api?a=file&b=bif)搭配[StartTime]參數，讓每次執行的Print檔案可以分開不同目錄，檔案維護上比較方便。

以下的範例會把輸出資料分開到不同檔案：

```xs
Print(file("[StrategyName]_[Symbol]_[StartTime].log"), "Date=", NumToStr(Date, 0), "Close=", NumToStr(Close, 2));
```

請參考[File指令](api?a=file&b=bif)，以及[教學文章](https://xstrader.net/print%E8%AA%9E%E6%B3%95%E8%AA%BF%E6%95%B4/)。
