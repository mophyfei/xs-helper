# OutputField -  (內建函數)（一般函數）

## 語法

指定選股的輸出欄位  
OutputField(輸出序號, 數值)  
OutputField(輸出序號, 數值, 小數位數)  
OutputField(輸出序號, 數值, 小數位數, 輸出欄位名稱)  
OutputField1(數值)  
OutputField1(數值, 小數位數)  
OutputField1(數值, 小數位數, 輸出欄位名稱)

## 說明

選股腳本預設只會顯示被選到的商品的商品名稱，成交，漲幅，成交量這幾個欄位。如果有需要在選股結果內輸出更多的欄位的話，則可以透過OutputField函數來新增欄位。

OutputField的語法可以傳入至多四個參數:

* 第一個參數為輸出序號，從1到99，用來指定輸出欄位的順序
* 第二個參數為要輸出的數值
* 第三個參數指定輸出時數值的小數點位數，可以不傳
* 從5.60版之後增加第四個參數，可以傳入輸出欄位的標題。如果不傳的話則預設的欄位標題為"欄位" + 序號。

以下是簡單的範例:

```xs
OutputField(1, GetField("月營收年增率","M"));
OutputField(2, GetField("月營收月增率","M"), 1);
OutputField(3, GetField("月營收月增率","M"), 1, "月營收月增率");
```

以上的範例在執行後會多產生三個欄位，第一個欄位為"欄位1"，內容為月營收年增率。第二個欄位為"欄位2"，內容為月營收年增率轉成一位小數點。第三個欄位為"月營收年增率"，內容與第二個欄位相同。

![OutputField](https://www.xq.com.tw/xstrader/wp-content/uploads/2015/09/OutputField.png)

OutputField指令也可以在函數名稱之後直接加上序號，例如OutputField1, OutputField2等。如果函數名稱內就包含序號的話，則就不需要傳入序號參數。

上述的範例可以改寫為:

```xs
OutputField1(GetField("月營收年增率","M"));
OutputField2(GetField("月營收月增率","M"), 1);
OutputField3(GetField("月營收月增率","M"), 1, "月營收月增率");
```

OutputField指令內所設定的欄位標題也可以透過[SetOutputName函數](api?a=setoutputname&b=bif)來指定。

OutputField 也可以使用 [order](api?a=order) 來指定選股結果區的欄位數值上/下排序，請參考連結說明使用。
