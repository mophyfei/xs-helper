# File -  (內建函數)（一般函數）

## 語法

與Print指令搭配，用來指定Print輸出檔案的位置  
Print(File(檔案路徑), 輸出數值1, 輸出數值2, 輸出數值3)  
Print(File(檔案名稱), 輸出數值1, 輸出數值2, 輸出數值3)

## 說明

[Print指令](api?a=print&b=bif)如果沒有傳入指定檔案欄位時，預設會將所有的Print輸出到XQ安裝目錄底下的XS/Print子目錄內，輸出檔案的名稱為**策略名稱\_商品代碼.log**，例如 **MyStrategy\_2330.TW.log**。

如果使用者希望指定不同的輸出目錄，或是不同的輸出檔案名稱的話，則可以使用File指令來做設定。

File指令有兩種用法:

第一種用法是傳入目錄名稱，例如File("d:\Print")，請注意路徑名稱的結尾必須是""。一旦指定目錄之後，所有的Print輸出檔案都會產生在這個目錄底下，輸出檔案名稱還是**策略名稱\_商品代碼.log**，

第二種用法是傳入檔案名稱，例如File("d:\Print\MyOutput.log")。一旦指定檔案名稱之後，所有Print的輸出都會寫到這個檔案內，包含所有被執行到的商品。

以下是完整的範例:

```xs
Print(File("d:\Print\"), date,symbol,close);
Print(File("d:\Print\MyOutput.log"), date,symbol,close);
```

除了以上用法之外，File內的檔案名稱或是目錄名稱也可以包含以下的特殊字串:

* [StrategyName] ← 轉換成策略名稱，也就是代表雷達名稱、選股策略名稱、指標名稱、自動交易策略名稱
* [StartTime] ← 轉換成策略的啟動時間，格式為HHMMSS
* [Symbol] ← 轉換成執行的商品代碼
* [Freq] ← 轉換成執行的頻率
* [ScriptName] ← 轉換成腳本名稱
* [Date] ← 轉換成YYYYMMDD的日期

如果檔案名稱或是目錄名稱包含以上的特殊字串的話，則XS會把這些字串轉換成執行的商品，頻率，腳本名稱後組成輸出的目錄名稱或是檔案名稱。

舉例而言:

```xs
Print(File("d:\Print\[Date]_[ScriptName]_[Symbol]_[Freq].log"), date, symbol, close);
```

如果腳本名稱是"MyScript", 執行的頻率是日, 執行的商品為2330.TW的話，則Print指令所產生的檔案會是"d:\Print\MyScript\_2330.TW\_D.log"。請注意如果檔案名稱內有包含特殊字串的話，則每個商品的Print檔案會獨立產生，而不是寫到同一個檔案內。

---

如果需要避開重覆Print在同一個檔案，可以運用File指令搭配[StartTime]參數，讓每次執行的Print檔案可以分開不同目錄，檔案維護上比較方便。

以下的範例會把輸出資料分開到不同檔案：

```xs
Print(file("[StrategyName]_[Symbol]_[StartTime].log"), "Date=", NumToStr(Date, 0), "Close=", NumToStr(Close, 2));
```

請參考[File指令](api?a=file&b=bif)，以及[教學文章](https://xstrader.net/print%E8%AA%9E%E6%B3%95%E8%AA%BF%E6%95%B4/)。
