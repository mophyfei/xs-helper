# GetSymbolField -  (內建函數)（欄位函數）

## 語法

讀取系統內的欄位資料  
欄位數值 = GetSymbolField("ID", "欄位名稱")  
欄位數值 = GetSymbolField("ID", "欄位名稱","頻率")  
欄位數值 = GetSymbolField("ID", "欄位名稱", "頻率", 是否為還原值)  
欄位數值 = GetSymbolField("ID", "欄位名稱", "頻率", 是否為還原值, 設定預設值)  
傳入商品ID、欄位名稱、指定的欄位頻率、是否為還原值 以及 設定預設值。  
如果不傳頻率的話，則讀取目前執行頻率的對應欄位

## 說明

GetSymbolField是GetField語法的延伸，在取得欄位相關資料時可以指定商品，透過這個函數可以在腳本中取得其他商品的欄位資料。

GetSymbolField函數可以傳入五個參數：

* 第一個參數是商品代碼，
* 第二個參數是欄位的中文或是英文名稱，
* 第三個參數則是欄位的頻率，如果省略這個參數的話，則會依照目前腳本執行的頻率來取得對應的資料
* 第四個參數則為「是否為還原值」運用 [Adjusted參數](article.aspx?a=Adjusted) 來設定要讀取的是原始資料，還是還原資料。
* 第五個參數為「設定預設值」運用 Default 參數來設定沒有資料時所回傳的預設值。

當你點選XS編輯器內的編輯選單/插入欄位選項，或是按快捷鍵F7，利用「插入欄位」畫面加入欄位的同時，如果在商品欄位填入有效商品時，系統會自動插入GetSymbolField函數（沒填商品時是插入GetField函數）。插入欄位畫面沒有還原分鐘的選項，如果使用者需要取的還原分鐘相關資料，請直接撰寫在腳本中。

![插入商品選股欄位](https://www.xq.com.tw/xstrader/wp-content/uploads/2016/11/InsertSymbolField_Filter.png)

以下是一個簡單的範例：

```xs
Value1 = getsymbolField("1101.TW", "收盤價");　// value1 為取得目前腳本執行頻率的台泥(1101)收盤價。
Value2 = getsymbolField("1101.TW", "收盤價", "1");　// value2 為取得原始1分鐘頻率的台泥(1101)收盤價。
Value3 = getsymbolField("1101.TW", "收盤價", "1", Adjusted:=true);　// value3 為取得還原1分鐘頻率的台泥(1101)收盤價。
Value4 = getsymbolField("1101.TW", "收盤價", "1", Adjusted:=true, Default:= 0);　// value3 為取得還原1分鐘頻率的台泥(1101)收盤價，當取不到資料時預設值為0。
```

---

另外在「插入欄位」畫面的商品下拉式選單有「標的商品、期貨近/遠/次遠月」選項，選擇後按下插入，系統會自動插入GetSymbolField取得標的商品相關欄位的語法在XS編輯器中，編譯成功後加入相關應用，即可取得商品的「標的商品、期貨近/遠/次遠月」相關欄位數據。

![插入標的商品欄位](https://www.xq.com.tw/wp-content/uploads/2021/07/20210721_%E6%8F%92%E5%85%A5%E6%A8%99%E7%9A%84%E5%95%86%E5%93%81.png)

以下是 GetSymbolField「標的商品、期貨近/遠/次遠月」範例：

```xs
//假設以下情境：
//1. 執行商品：台積電期近月(FICDF*1.TF)
//2. 執行時間：08月24日
//3. 加入雷達，進行盤中雷達實際測試。


if getinfo("IsRealTime") <> 1 then return;
//只跑在即時資料區間段，原因是跑 GetSymbolField("Future*2", "收盤價") 會取得遠月的收盤價，但遠月歷史資料期間短，故僅跑在即時資料區間段。


value1 = GetSymbolField("Underlying", "收盤價");
//value1是台積電期近月之標的商品收盤價；
//也就是台積電(2330.TW)收盤價。


value2 = GetSymbolField("Future*1", "收盤價");
//value2是台積電期近月之期貨近月收盤價；
//在此範例就是台積電期09月(FICDF09.TF)收盤價。


value3 = GetSymbolField("Future*2", "收盤價");
//value3是台積電期近月之期貨遠月收盤價。
//在此範例就是台積電期10月(FICDF10.TF)收盤價。
```

詳細的語法說明可以參考 [GetField](api?a=getfield&b=bif)函數。
