# ReadTicks -  (系統函數)（價格關係）

## 語法

讀取自上次腳本執行後到目前為止的所有 Tick 成交資料  
本次執行更新的資料筆數 = ReadTicks ( 輸出儲存Tick資料的二維陣列, 輸出最後一筆讀到的Tick序號 ) ;  
  
回測在1分鐘頻率時，不支援模擬逐筆洗價

## 說明

請先參考 [台股逐筆撮合的連續成交Tick序列](https://www.xq.com.tw/lesson/xspractice/%e5%8f%b0%e8%82%a1%e9%80%90%e7%ad%86%e6%92%ae%e5%90%88%e7%9a%84%e7%a9%bf%e5%83%b9tick/) ，關於腳本洗價方式與MultiTick的概念。

### **參數設定說明**

**輸出儲存Tick資料的二維陣列**

```xs
array: tick_array[100, 11](0);
```

必須先宣告一個2維陣列來儲存Tick資料。

第一維(列)是每次洗價的最大讀取筆數；第二維(行)用來儲存每一筆的成交相關資料，必須 >= 11

**輸出腳本執行完畢的最後一筆Tick序號（`Seqno`）**

```xs
var: intrabarpersist readtick_cookie(0);
```

必須先宣告一個`intrabarpersist`的變數，給 **ReadTicks** 內部使用。

### **參數輸出內容說明**

#### **tick\_array**

當 **ReadTicks** 執行完畢後，`tick_array`的每一列代表一筆成交紀錄（或是合併後的 MultiTick），每一行 (column) 包含該筆紀錄的不同欄位資訊。

* 每一列成交紀錄

  回傳的 Tick 資料是倒序的，`tick_array[1, ...]`是離當下最近的那筆成交，`tick_array[2, ...]`是前一筆，依此類推。
* 每一行欄位資訊

  ```xs
    tick_array[..., 1] = Date
    tick_array[..., 2] = Time（MultiTick 合併後，此為序列的統一時間）
    tick_array[..., 3] = Close（若是 MultiTick，則為序列中最後一筆的成交價）
    tick_array[..., 4] = Volume（若是 MultiTick，則為序列中最後一筆的成交量。若需總量請參考欄位10）
    tick_array[..., 5] = 內外盤標記（1: 外盤；-1: 內盤；0: 無法區分）
    tick_array[..., 6] = Tick序號 （若是 MultiTick，則為序列中最後一筆的序號）
    tick_array[..., 7] = 成交方式註記（-1: 集合競價；0: 逐筆撮合下的單筆成交；>0: 逐筆撮合下的 MultiTick，數值代表此序列包含的 Tick 筆數）
    tick_array[..., 8] = 起始Offset（若是MultiTick，為第一筆與最新一筆Tick的相對位置）
    tick_array[..., 9] = 結束Offset（若是MultiTick，為最後一筆與最新一筆Tick的相對位置。若是單筆成交，此值與欄位 8 相同）
    tick_array[..., 10] = 成交量加總（該筆成交的總量。若是 MultiTick，此為序列中所有 Tick 的成交量總和）
    tick_array[..., 11] = 成交值加總（ 該筆成交的總金額(元)。若是 MultiTick，此為序列中所有 Tick 的成交值總和）
  ```
* 請注意

  + 不完整序列

    若在兩次執行之間，系統只收到了 MultiTick 的一部分，**ReadTicks** 會暫不處理，並將`readtick_cookie`停在序列開始前的位置，待下次收到完整序列時再一併回傳。
  + `ReadTicks`不會在每次執行前清空`tick_array`

    它只會從第一列開始覆寫資料。若前一次呼叫回傳了 20 筆資料，而本次只回傳 10 筆，則陣列的第 11 至 20 列將會保留舊的資料。因此，務必使用`ReadTicks`的回傳值來控制迴圈的範圍，避免讀取到過時的資訊。

### **ReadTicks的回傳值**

ReadTicks本身可以用一個變數來存取它的輸出值，代表本次執行在`tick_array`內更新的資料筆數 (即實際填入的行數)
