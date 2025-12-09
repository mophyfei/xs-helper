# XS Helper Complete Documentation

Generated on 2025-12-09

---
## BarAdjusted
**Path:** 內建函數 > 一般函數

BarAdjusted -  (內建函數)
一般函數

語法：

回傳執行腳本資料頻率是否為還原頻率。回傳布林值。  
若為還原頻率，則回傳「True」  
若不為還原頻率，則回傳「False」

說明：

運用這個函數來判斷目前的執行頻率是否為還原頻率。

在執行頻率為「分鐘」的資料表達為：

* 如果資料頻率是還原5分鐘，則 BarInterval = 5，BarFreq = "Min"，BarBarAdjusted = true。
* 如果資料頻率是5分鐘，則 BarInterval = 5，BarFreq = "Min"，BarBarAdjusted = false。

在執行頻率為「日」以上頻率的資料表達為：

* 如果資料是還原日線, 則 BarFreq = "AD", BarAdjusted = true。
* 如果資料是日線, 則BarFreq = "D", BarAdjusted = false。

---

應用在指定策略執行的頻率，避免執行頻率設定錯誤的範例語法如下：

* **範例一**：確認執行頻率必須是「還原日」頻率才可執行

```
if BarFreq <> "D" or BarAdjusted <> True  then raiseRunTimeError("僅支援還原日頻率");
```

* **範例二**：確認執行頻率必須是「還原5分鐘」頻率才可執行

```
if BarInterval <> 5 
or barFreq <> "Min" 
or BarAdjusted <> true then raiseRunTimeError("僅支援還原5分鐘線圖");
```

---
## BarFreq
**Path:** 內建函數 > 一般函數

BarFreq -  (內建函數)
一般函數

語法：

傳回執行腳本資料頻率的單位。  
執行頻率 = BarFreq  
回傳以下字串: 分鐘線:"Min",  
日線:"D",周線:"W", 月線:"M",  
還原日線:"AD",還原周線:"AW", 還原月線:"AM",  
季線:"Q", 半年線:"H",年線:"Y"

說明：

一般可以使用這個函數來判斷目前的執行頻率

```
//確認資料必須是日線
if BarFreq <> "D" then return;
```

---
## BarInterval
**Path:** 內建函數 > 一般函數

BarInterval -  (內建函數)
一般函數

語法：

傳回執行腳本資料的分鐘頻率間隔  
分鐘區間 = BarInterval  
如果頻率是分鐘資料，則回傳分鐘的間隔，例如30分鐘線的話則回傳30，否則一律回傳1

說明：

一般而言BarInterval函數會跟[BarFreq](api?a=barfreq&b=sys)一起搭配使用，用來判斷目前執行的分鐘頻率的分鐘間隔。

```
// 先判斷目前是分鐘線
If BarFreq = "Min" Then
Begin
	If BarInterval = 30 Then
	Begin
		// 資料為30分鐘線
	End;
End;
```

---
## CallFunction
**Path:** 內建函數 > 一般函數

CallFunction -  (內建函數)
一般函數

語法：

呼叫函數執行  
回傳數值=CallFunction(函數名稱,參數一,參數二,...)  
傳入一個以上參數:  
- 第一個參數是函數名稱的字串。  
- 第二個參數是被呼叫函數的第一個參數。  
- 第三個參數是被呼叫函數的第二個參數。依此類推。

說明：

從 v6.20 開始，我們開放函數使用中文名稱，更方便大家使用。不過中文函數在被其他腳本呼叫時，會需要透過CallFunction這個函數來執行。

CallFunction的用法很簡單，第一個參數固定是要被呼叫的函數名稱，其餘的參數就是看被呼叫函數需要幾個參數，依序填入即可。

範例：

```
plot1(average(c,5));
plot2(callfunction("average",c,5));
//以上二個寫法效果是一樣的
```

---
## CurrentBar
**Path:** 內建函數 > 一般函數

CurrentBar -  (內建函數)
一般函數

語法：

傳回K棒目前的編號。  
K棒編號 = CurrentBar

說明：

傳回執行腳本時的K棒序列編號，由1開始，第一筆K棒編號為1，第二筆K棒編號為2，依序遞增。

可以使用這個函數來判斷目前腳本執行的時機點

```
if CurrentBar = 1 then
	value1 = Close
else
	value1 = value1[1] + value2 * (Close - value1[1]);
```

上述範例利用CurrentBar來判斷目前是否是第一筆K棒。如果是的話則回傳XAverage的初始數值。

---
## DataAlign
**Path:** 內建函數 > 一般函數

DataAlign -  (內建函數)
一般函數

語法：

設定資料對位方式  
DataAlign(欲設定的資料對位方式)  
如果是絕對對位的話，資料對位方式為0，如果是遞補對位的話，資料對位方式為1

說明：

在腳本執行時如果透過[GetField函數](api?a=getfield&b=bif)讀取開高低收以外的資料欄位時，有可能會遇到需要讀取的欄位資料的資料時間與目前價位資料時間不一致的情形，此時系統會依照**資料對位**的設定方式來決定如何處理。

以下的圖示內我們使用日線頻率的**外資買賣超**資料來說明資料對位的處理邏輯。

![對位方式](https://www.xq.com.tw/xstrader/wp-content/uploads/2015/08/DataAlign.png)

由於每日外資買賣超資料都是在下午16:00以後才會公布，也就是在當日交易區段時價位資料的日期與外資買賣超資料的日期會有一天的差異。所以當以下的腳本在5/28日上午執行時

```
if Close > Close[1] and 
   GetField("外資買賣超") > 0 then ...
```

系統會依照對位方式決定GetField("外資買賣超")函數會取得哪一天的外資買賣超資料。

目前系統支援兩種資料對位方式，分別是

* **絕對對位**

```
DataAlign(0);
```

* **遞補對位**

```
DataAlign(1);
```

在**絕對對位**模式時，GetField("外資買賣超")函數的日期必須與價位日期一致(**同期別**)，也就是說系統會嘗試讀取5/28日的外資買賣超資料，此時由於資料尚未公佈，所以執行會發生失敗(引用資料不存在)。

而在**遞補對位**模式時，系統在找不到5/28日的外資買賣超資料時，會**自動往前尋找**。在上面的範例內 GetField("外資買賣超")則會找到5/27日的外資買賣超資料。

目前系統預設的資料對位方式依照腳本類型而有所不同:

* 如果是指標腳本或是雷達腳本，資料對位方式預設為絕對對位 (DataAlign(0))
* 如果是選股腳本，資料對位方式預設為遞補對位 (DataAlign(1))

選股腳本由於常常需要讀取營收/財報等欄位資料，而這些資料通常公佈時間都是落後於價位日期的，所以預設為遞補對位。

使用者如果知道所需資料的日期的話，也可以透過不同的寫法來讀取到預期的資料，不需要更改對位模式。以上述範例而言，如果這是一個策略雷達腳本的話，由於策略雷達腳本的執行時間通常是在交易時間區段，此時外資買賣超資料一定會落後一期。所以腳本可以修改成下列寫法:

```
if Close > Close[1] and 
   GetField("外資買賣超")[1] > 0 then ...
```

注意到腳本內使用\*\*GetField("外資買賣超")[1]\*\*語法來讀取前一期的外資買賣超。

---
## ExecOffset
**Path:** 內建函數 > 一般函數

ExecOffset -  (內建函數)
一般函數

語法：

回傳目前函數執行時偏移的K棒筆數  
偏移筆數 = ExecOffset

說明：

使用在函數內，用來取得目前函數執行時*偏移*的K棒根數。

一般的函數呼叫方式如下:

```
Value1 = Average(Volume, 5);
```

此時如果從Average函數內去呼叫ExecOffset時得到的值是0。

如果呼叫的方式改成:

```
Value1 = Average(Volume, 5)[1];
```

的話，則在呼叫Average函數時利用\*\*[1]\*\*設定了偏移的K棒個數，此時Average function執行時取得的資料是當時資料往前偏移一筆的結果，也就是說Value1會是前5日成交量的平均值，不包含最新一日的成交量。

在這樣子的使用情境底下，Average函數內讀取**ExecOffset**時會得到1.

---
## File
**Path:** 內建函數 > 一般函數

File -  (內建函數)
一般函數

語法：

與Print指令搭配，用來指定Print輸出檔案的位置  
Print(File(檔案路徑), 輸出數值1, 輸出數值2, 輸出數值3)  
Print(File(檔案名稱), 輸出數值1, 輸出數值2, 輸出數值3)

說明：

[Print指令](api?a=print&b=bif)如果沒有傳入指定檔案欄位時，預設會將所有的Print輸出到XQ安裝目錄底下的XS/Print子目錄內，輸出檔案的名稱為**策略名稱\_商品代碼.log**，例如 **MyStrategy\_2330.TW.log**。

如果使用者希望指定不同的輸出目錄，或是不同的輸出檔案名稱的話，則可以使用File指令來做設定。

File指令有兩種用法:

第一種用法是傳入目錄名稱，例如File("d:\Print")，請注意路徑名稱的結尾必須是""。一旦指定目錄之後，所有的Print輸出檔案都會產生在這個目錄底下，輸出檔案名稱還是**策略名稱\_商品代碼.log**，

第二種用法是傳入檔案名稱，例如File("d:\Print\MyOutput.log")。一旦指定檔案名稱之後，所有Print的輸出都會寫到這個檔案內，包含所有被執行到的商品。

以下是完整的範例:

```
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

```
Print(File("d:\Print\[Date]_[ScriptName]_[Symbol]_[Freq].log"), date, symbol, close);
```

如果腳本名稱是"MyScript", 執行的頻率是日, 執行的商品為2330.TW的話，則Print指令所產生的檔案會是"d:\Print\MyScript\_2330.TW\_D.log"。請注意如果檔案名稱內有包含特殊字串的話，則每個商品的Print檔案會獨立產生，而不是寫到同一個檔案內。

---

如果需要避開重覆Print在同一個檔案，可以運用File指令搭配[StartTime]參數，讓每次執行的Print檔案可以分開不同目錄，檔案維護上比較方便。

以下的範例會把輸出資料分開到不同檔案：

```
Print(file("[StrategyName]_[Symbol]_[StartTime].log"), "Date=", NumToStr(Date, 0), "Close=", NumToStr(Close, 2));
```

請參考[File指令](api?a=file&b=bif)，以及[教學文章](https://xstrader.net/print%E8%AA%9E%E6%B3%95%E8%AA%BF%E6%95%B4/)。

---
## GetBackBar
**Path:** 內建函數 > 一般函數

GetBackBar -  (內建函數)
一般函數

語法：

回傳目前腳本計算所使用的資料引用筆數  
回傳數值= GetBackBar

說明：

回傳目前腳本計算所使用的資料引用筆數，為一固定常數。

關於資料讀取範圍以及最大引用筆數的定義，請參考[資料讀取範圍與腳本執行的關係](https://www.xq.com.tw/lesson/xspractice/%E8%B3%87%E6%96%99%E8%AE%80%E5%8F%96%E7%AF%84%E5%9C%8D%E8%88%87%E8%85%B3%E6%9C%AC%E5%9F%B7%E8%A1%8C%E7%9A%84%E9%97%9C%E4%BF%82/)。

---
## GetBarBack
**Path:** 內建函數 > 一般函數

GetBarBack -  (內建函數)
一般函數

語法：

回傳目前腳本計算所使用的資料引用筆數  
回傳數值= GetBarBack

說明：

回傳目前腳本計算所使用的資料引用筆數，為一固定常數。

關於資料讀取範圍以及最大引用筆數的定義，請參考[資料讀取範圍與腳本執行的關係](https://www.xq.com.tw/lesson/xspractice/%E8%B3%87%E6%96%99%E8%AE%80%E5%8F%96%E7%AF%84%E5%9C%8D%E8%88%87%E8%85%B3%E6%9C%AC%E5%9F%B7%E8%A1%8C%E7%9A%84%E9%97%9C%E4%BF%82/)。

---
## GetBarOffset
**Path:** 內建函數 > 一般函數

GetBarOffset -  (內建函數)
一般函數

語法：

依傳入的交易日的日期取得相對K棒位置。  
回傳數值=GetBarOffset(日期)  
回傳數值=GetBarOffset(日期，時間)  
傳入二個參數：  
- 第一個參數是交易日的日期，格式為YYYYMMDD。  
- 第二個參數是時間，格式為HHMMSS，第二個參數可以不用傳。

說明：

依傳入的交易日的日期取得相對K棒位置。

可以傳入兩個參數：

* 第一個參數是日期，格式為YYYYMMDD。
* 第二個參數是時間，格式為HHMMSS，第二個參數可以不用傳。

當回傳值為0時，表示傳入日期/時間 ≧K棒日期/時間。
當回傳值為1時，代表目前K棒的前一根K棒日期/時間為傳入日期/時間。

範例：

```
value1 = GetBarOffset(20150831); //取得20150831這根K棒的相對位置
value2 = High[value1]; //取得20150831當天的最高價
plot1(value2); //繪出20150831最高價的水平線
```

注意，當無傳入日期這根K棒時，會往前找到最接近的一根。例如：20150829、20150830是非交易日，所以會往前找到20150828這根K棒。也就是說GetBarOffset(20150828)、GetBarOffset(20150829)、GetBarOffset(20150830)的回傳值會是相等的。

---
## GetFieldStartOffset
**Path:** 內建函數 > 一般函數

GetFieldStartOffset -  (內建函數)
一般函數

語法：

判斷欄位初始點  
欄位筆數 = GetFieldStartOffset("欄位名稱")  
欄位筆數 = GetFieldStartOffset("欄位名稱", "頻率")  
回傳目前最新一筆欄位與此欄位的第一筆資料間的欄位筆數。  
如果無此欄位，或是欄位的初始點超過目前bar的位置，則回-1。  
※如果不傳頻率的話，則讀取目前執行頻率的對應欄位。  
※僅支援「選股」腳本類型。

說明：

請注意，回傳欄位的筆數，是依照傳入的欄位頻率來計算，可能與目前腳本執行的K棒頻率不同。
例如目前可能是跑日線，然後GetFieldStartOffset要查的是月頻率欄位，此時 GetFieldStartOffset所回傳的是月頻率欄位有幾筆。

**選股腳本範例：判斷當月營收是否創掛牌新高**

```
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

```
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

---
## GetFirstBarDate
**Path:** 內建函數 > 一般函數

GetFirstBarDate -  (內建函數)
一般函數

語法：

回傳目前腳本計算所使用第一筆資料的日期  
回傳日期= GetFirstBarDate

說明：

回傳目前腳本計算所使用第一筆資料的日期，為一固定常數。

日期格式是一個8碼的數字，如果第一筆資料日期是是2009年1月2日，則回傳20090102

關於資料讀取範圍的定義，請參考[資料讀取範圍與腳本執行的關係](https://www.xq.com.tw/lesson/xspractice/%E8%B3%87%E6%96%99%E8%AE%80%E5%8F%96%E7%AF%84%E5%9C%8D%E8%88%87%E8%85%B3%E6%9C%AC%E5%9F%B7%E8%A1%8C%E7%9A%84%E9%97%9C%E4%BF%82/)。

---
## GetInfo
**Path:** 內建函數 > 一般函數

GetInfo -  (內建函數)
一般函數

語法：

用來取得目前腳本的執行環境資訊。  
回傳數值=GetInfo(資訊名稱)  
傳入一個參數:  
- 第一個參數是資訊名稱字串，可以是"Instance"、"IsRealTime"、"IsTimerMode"、"FilterMode"、"TradeMode"、"AT\_EnableTrade"、"AT\_BID"、"AT\_AccType"或"AT\_AID"

說明：

依傳入的參數回傳相關資訊。

當參數為"Instance"時，可以取得腳本執行的功能：

* 回傳值為1表示自訂指標。
* 回傳值為2表示策略雷達。
* 回傳值為3表示XS選股。
* 回傳值為31表示XS選股自訂排行。
* 回傳值為4表示策略雷達回測（進場）。
* 回傳值為41表示策略雷達回測（出場）。
* 回傳值為5表示自動交易
* 回傳值為6表示自動交易回測

當參數為"IsRealTime"時，可以取得K棒的狀態：

* 回傳值為0表示該筆資料為歷史資料或其他。
* 回傳值為1表示該筆資料為即時成交更新資料，需注意當該筆運算是因為自動洗價觸發時，就算資料不是即時成交更新也會回傳1。

當參數為"IsTimerMode"時，可以判斷該次洗價是否因為自動洗價所觸發，只支援警示腳本和交易腳本：

* 回傳值為1表示該次洗價是因為自動洗價所導致。
* 回傳值為0表示為成交洗價觸發，或是使用在其他腳本上。

當參數為"FilterMode"時，可以取得XS選股的模式：

* 回傳值為1表示XS選股。
* 回傳值為2表示XS選股回溯。
* 回傳值為3表示XS選股回測（進場）。

當參數為"TradeMode"時，可以交易策略目前執行的K棒是否處於資料讀取區間：

* 回傳值為0表示目前執行的K棒處理資料讀取區間，所以交易指令不會執行。
* 回傳值為1表示目前執行的K棒處理策略部位計算區間或是即時區間，交易指令將會執行。

當參數為"AT\_EnableTrade"時，可以取得目前交易策略是否有啟動帳號：

* 回傳值為0表示回測或即時區間但沒有設定交易帳號。
* 回傳值為1表示即時區間且有設定交易帳號。

當參數為"AT\_BID"時，可以取得券商的字串代碼：

* 回傳值為空白字串表示策略沒有設定交易帳號或是在回測。
* 回傳值為SYSTRADE表示策略帳號為模擬交易帳號。
* 回傳值為SYSCAMPUS表示策略帳號為校園模擬競賽。
* 其他券商會回傳各自對應的代碼。

當參數為"AT\_AccType"時，可以取得策略運作的業務類別:

* 回傳值為1表示業務類別為證券。
* 回傳值為2表示業務類別為期貨。
* 回傳值為3表示業務類別為複委託。
* 回傳值為0表示策略沒有設定交易帳號或是在回測。

當參數為"AT\_AID"時，可以取得目前策略運作的帳號:

* 回傳值為空白字串表示策略沒有設定交易帳號或是在回測。
* 回傳值為券商代碼加上交易帳號組成的字串。

關於AT的EnableTrade、BID、AccType以及AID的進一步說明，可以參考[自動交易語法 取得「交易帳號」使用說明](https://www.xq.com.tw/at_id/)

範例：

```
value1 = getinfo("IsRealTime"); //若value1為1，則代表目前計算的是即時資料
plot1(value1);
```

---
## GetSymbolFieldStartOffset
**Path:** 內建函數 > 一般函數

GetSymbolFieldStartOffset -  (內建函數)
一般函數

語法：

判斷欄位初始點  
欄位筆數 = GetSymbolFieldStartOffset("ID", "欄位名稱")  
欄位筆數 = GetSymbolFieldStartOffset("ID", "欄位名稱","頻率")  
回傳目前最新一筆欄位與此欄位的第一筆資料間的欄位筆數。  
如果無此欄位，或是欄位的初始點超過目前bar的位置，則回-1。  
※如果不傳頻率的話，則讀取目前執行頻率的對應欄位。  
※僅支援「選股」腳本類型。

說明：

GetSymbolFieldStartOffset是GetFieldStartOffset語法的延伸，在取得欄位相關資料時可以指定商品，透過這個函數可以在腳本中取得其他商品的欄位筆數。

以下是一個簡單的範例：

```
Value1 = GetSymbolFieldStartOffset("1101.TW", "月營收");　// value1 為取得目前腳本執行頻率的台泥(1101)目前最新一筆月營收欄位與月營收欄位第一筆資料間的欄位筆數。
Value2 = GetSymbolFieldStartOffset("1101.TW", "月營收", "M");　// value2 為取得月頻率的台泥(1101)目前最新一筆月營收欄位與月營收欄位第一筆資料間的欄位筆數。
```

詳細的語法說明可以參考 [GetFieldStartOffset](api?a=GetFieldStartOffset&b=bif)函數。

---
## GetSymbolGroup
**Path:** 內建函數 > 一般函數

GetSymbolGroup -  (內建函數)
一般函數

語法：

用來取得 執行商品/指定商品 支援的相關商品清單。  
Group: myGroup();  
myGroup = GetSymbolGroup("權證");  
myGroup = GetSymbolGroup("TSE23.TW", "成分股");

說明：

GetSymbolGroup 可用來取得系統內建的商品清單 。

第一個參數指定要取得清單的商品，例如範例中的 TSE23.TW 。 若未指定商品，則預設取得目前執行商品的清單 （ 前提是該清單存在 ） 。

第二個參數指定所需的清單類型，例如範例中的 成分股 ，此參數為必填 。

可參考[商品清單功能](https://www.xq.com.tw/lesson/xspractice/%E5%95%86%E5%93%81%E6%B8%85%E5%96%AE%E5%8A%9F%E8%83%BD/)。

---
## GetTBMode
**Path:** 內建函數 > 一般函數

GetTBMode -  (內建函數)
一般函數

語法：

取得自定指標的繪圖模式  
回傳數值= GetTBMode

說明：

取得自定指標的繪圖模式。
回傳數值如下

* SetTBMode(1) 同v5.62行為，腳本資料計算筆數為資料讀取筆數加畫面上的K棒筆數，使用者拉動畫面會進行重算。
* SetTBMode(0) 預設值，腳本資料計算筆數為全部資料，整個數列只算一次，拉動畫面不會重算。

範例:

```
Input: Period(200, "EMA");
Input: TB(1);
SetTBMode(TB);//指定自定指標的繪圖模式，可以變更參數比較一下計算值的差異
Plot1(EMA(Close, Period), "EMA");
Plot2(GetTBMode);//取得自定指標的繪圖模式
```

---
## GetTotalBar
**Path:** 內建函數 > 一般函數

GetTotalBar -  (內建函數)
一般函數

語法：

回傳目前腳本計算所使用的資料筆數  
回傳數值= GetTotalBar

說明：

回傳目前腳本計算所使用的資料筆數，為一固定常數。

關於執行筆數以及最大引用筆數的說明，請參考[資料讀取範圍與腳本執行的關係](https://www.xq.com.tw/lesson/xspractice/%E8%B3%87%E6%96%99%E8%AE%80%E5%8F%96%E7%AF%84%E5%9C%8D%E8%88%87%E8%85%B3%E6%9C%AC%E5%9F%B7%E8%A1%8C%E7%9A%84%E9%97%9C%E4%BF%82/)。

---
## GroupSize
**Path:** 內建函數 > 一般函數

GroupSize -  (內建函數)
一般函數

語法：

回傳指定Group中包含的商品數量。  
Input: myGroup(Group);  
Value1 = GroupSize(myGroup);

說明：

GroupSize函數會回傳商品清單包含的數量，可以此數值避免取用到超出陣列範圍的資料而導致錯誤。

---
## IsFirstCall
**Path:** 內建函數 > 一般函數

IsFirstCall -  (內建函數)
一般函數

語法：

回傳目前計算的K棒（currentbar）是否為事件的第一次洗價  
傳入事件字串：" "、"Bar"、"Date"、"Realtime"、"RealBar"  
回傳布林值=IsFirstCall

說明：

isfirstcall(" ")：此次執行的第一次洗價

isfirstcall("Bar")：此根 Bar 的第一次洗價

isfirstcall("Date")：此交易日的第一次洗價

isfirstcall("Realtime")：此交易日進入即時洗價區間的第一次洗價

isfirstcall("RealBar")：此交易日進入即時洗價區間，首次產生成交事件後的第一次洗價(通常會與"Realtime"一致；若是在揭示未成交K棒時，遇到暫緩開盤或開盤後沒有成交事件，兩者就會產生差異)

詳細說明與範例請參考：

<https://www.xq.com.tw/learn/xspractice/isfirstcall/>

---
## IsLastBar
**Path:** 內建函數 > 一般函數

IsLastBar -  (內建函數)
一般函數

語法：

回傳目前計算的K棒（currentbar）是否為最新的K棒  
回傳布林值=IsLastBar

說明：

當目前計算的K棒為最新的K棒時，回傳True；其他狀況則回傳False。

---
## IsSessionFirstBar
**Path:** 內建函數 > 一般函數

IsSessionFirstBar -  (內建函數)
一般函數

語法：

回傳目前計算的K棒（currentbar）是否為當日第一根K棒  
回傳布林值=IsSessionFirstBar

說明：

當目前計算的K棒為當日第一根K棒時，回傳True；其他狀況則回傳False。

---
## IsSessionLastBar
**Path:** 內建函數 > 一般函數

IsSessionLastBar -  (內建函數)
一般函數

語法：

回傳目前計算的K棒（currentbar）是否為當日最後一根K棒  
回傳布林值=IsSessionLastBar

說明：

當目前計算的K棒為當日最後一根K棒時，回傳True；其他狀況則回傳False。

---
## MaxBarsBack
**Path:** 內建函數 > 一般函數

MaxBarsBack -  (內建函數)
一般函數

語法：

取得腳本執行時所設定的最大引用筆數  
Value1 = MaxBarsBack

說明：

回傳目前腳本計算所使用的資料引用筆數，為一固定常數。

關於資料讀取範圍以及最大引用筆數的定義，請參考[資料讀取範圍與腳本執行的關係](http://www.xq.com.tw/lesson/xspractice/%E8%B3%87%E6%96%99%E8%AE%80%E5%8F%96%E7%AF%84%E5%9C%8D%E8%88%87%E8%85%B3%E6%9C%AC%E5%9F%B7%E8%A1%8C%E7%9A%84%E9%97%9C%E4%BF%82/)。

---
## NoPlot
**Path:** 內建函數 > 一般函數

NoPlot -  (內建函數)
一般函數

語法：

清除指定的指標序列目前這根K棒上面的數值  
NoPlot(指標繪圖序列編號)  
指標繪圖序列編號從1到999

說明：

在指標腳本內我們會使用[Plot函數](api?a=plot&b=bif)來產生不同的數值序列，每一個數值序列有一個編號，從1到999。

如果在某些情形底下我們希望某個序列這一點的值不要畫的話，則可以使用NoPlot的語法來做清除的動作。

以下我們先看一個範例:

```
Value1 = Close - Close[1];
Plot1(Value1);
```

在上面這個腳本內，我們先計算先後兩筆K棒的差值，然後把差值畫在Plot1上面。

如果我們希望只有上漲時才畫的話，那則可以改成這樣子的寫法:

```
Value1 = Close - Close[1];
Plot1(Value1);
If Value1 <= 0 Then NoPlot(1);
```

上述範例內使用NoPlot函數，指定序列1在Value1 <= 0 的時候不要畫圖。

---
## OutputField
**Path:** 內建函數 > 一般函數

OutputField -  (內建函數)
一般函數

語法：

指定選股的輸出欄位  
OutputField(輸出序號, 數值)  
OutputField(輸出序號, 數值, 小數位數)  
OutputField(輸出序號, 數值, 小數位數, 輸出欄位名稱)  
OutputField1(數值)  
OutputField1(數值, 小數位數)  
OutputField1(數值, 小數位數, 輸出欄位名稱)

說明：

選股腳本預設只會顯示被選到的商品的商品名稱，成交，漲幅，成交量這幾個欄位。如果有需要在選股結果內輸出更多的欄位的話，則可以透過OutputField函數來新增欄位。

OutputField的語法可以傳入至多四個參數:

* 第一個參數為輸出序號，從1到99，用來指定輸出欄位的順序
* 第二個參數為要輸出的數值
* 第三個參數指定輸出時數值的小數點位數，可以不傳
* 從5.60版之後增加第四個參數，可以傳入輸出欄位的標題。如果不傳的話則預設的欄位標題為"欄位" + 序號。

以下是簡單的範例:

```
OutputField(1, GetField("月營收年增率","M"));
OutputField(2, GetField("月營收月增率","M"), 1);
OutputField(3, GetField("月營收月增率","M"), 1, "月營收月增率");
```

以上的範例在執行後會多產生三個欄位，第一個欄位為"欄位1"，內容為月營收年增率。第二個欄位為"欄位2"，內容為月營收年增率轉成一位小數點。第三個欄位為"月營收年增率"，內容與第二個欄位相同。

![OutputField](https://www.xq.com.tw/xstrader/wp-content/uploads/2015/09/OutputField.png)

OutputField指令也可以在函數名稱之後直接加上序號，例如OutputField1, OutputField2等。如果函數名稱內就包含序號的話，則就不需要傳入序號參數。

上述的範例可以改寫為:

```
OutputField1(GetField("月營收年增率","M"));
OutputField2(GetField("月營收月增率","M"), 1);
OutputField3(GetField("月營收月增率","M"), 1, "月營收月增率");
```

OutputField指令內所設定的欄位標題也可以透過[SetOutputName函數](api?a=setoutputname&b=bif)來指定。

OutputField 也可以使用 [order](api?a=order) 來指定選股結果區的欄位數值上/下排序，請參考連結說明使用。

---
## Playsound
**Path:** 內建函數 > 一般函數

Playsound -  (內建函數)
一般函數

語法：

播放指定的音訊檔案

說明：

在執行該行腳本時，播放設定的音訊檔案

若同一次腳本運算中執行了複數個 PlaySound 函數，只會撥放最後執行的音訊檔案。

在指定檔案時，若沒有指定絕對路徑的話，會從預設資料夾 C:\SysJust\XQ2005\User\Sound 搜尋符合的檔案。

範例:

```
PlaySound("GML.wav");
PlaySound("C:\SysJust\XQ2005\User\Sound\GML.wav");
```

---
## Plot
**Path:** 內建函數 > 一般函數

Plot -  (內建函數)
一般函數

語法：

產生指標腳本的繪圖序列語法：  
Plot(輸出序號，指標數值)  
Plot(輸出序號，指標數值，繪圖序列名稱)  
Plot(輸出序號，指標數值，繪圖序列名稱，checkbox:=1)  
Plot1(指標數值)  
Plot1(指標數值，繪圖序列名稱)  
Plot1(指標數值，繪圖序列名稱，checkbox:=1)

說明：

在指標腳本內必須使用Plot函數來產生繪圖數列。

每個指標腳本可以產生至多999個繪圖數列，實際使用時必須在Plot之後加上指定的繪圖序列編號，例如**Plot1**, **Plot2**, 到**Plot999**。

Plot函數可以傳入三個參數

* 第一個參數是指標的數值
* 第二個參數是這個繪圖序列的名稱，可以不用傳。如果不傳的話，則繪圖序列的名稱為 "Plot"加上這個序列的編號
* 第三個參數為「是否開啟下拉式選單」提供給使用者勾選顯示指標。可以不用傳，如果不傳的話，則不會有下拉式選單提供選擇。checkbox:=1 為預設顯示指標；checkbox:=0 為預設「不」顯示指標。

**範例#1**

```
Plot1(Average(Close, 5));
Plot2(Close, "收盤價");
```

在範例#1 內輸出兩個繪圖數列，第一個數列為收盤價的五日平均值，圖形名稱為 "Plot1"，第二個數列為收盤價(Close)，圖形名稱為 "收盤價"。

Plot1到Plot99除了可以是一個函數之外，也可以在腳本內被當成數列來引用。

**範例#2**

```
Plot1(Average(Close, 5));
Plot2(Close, "收盤價");
Value1 = Plot2 - Plot1;
Plot3(Value1, "差值");
```

在範例#2 內Value1的數值是繪圖數列2(Plot2)與繪圖數列1(Plot1)的相減值，然後把這個差值畫在Plot3上面。

**範例#3**

```
//checkbox:=1，為預設顯示指標。
//checkbox:=0，為預設「不」顯示指標。
plot1(open,"開盤價",checkbox:=0);
plot2(high,"最高價",checkbox:=0);
plot3(low,"最低價",checkbox:=0);
plot4(close,"收盤價",checkbox:=1);//預設繪製出「收盤價」指標
```

在範例#3 中，有使用到 checkbox 參數，故將此XS指標腳本加入指標後的技術分析副圖，在滑鼠點選下拉式選單圖示如下：
![下拉式選單](https://www.xq.com.tw/wp-content/uploads/2021/10/20211029_checkbox.png)

---
## PlotFill
**Path:** 內建函數 > 一般函數

PlotFill -  (內建函數)
一般函數

語法：

PlotFill(序列編號, vFrom, vTo);  
PlotFill(序列編號, vFrom, vTo, "序列名稱");

說明：

第一個參數是設定序列編號，會是 1~999 的數值，與目前 Plot 的序列編號相同。

第二個和第三個參數分別是當根 K 棒要填色的開始和結束點。 第四個參數是設定序列的名稱，為選填的參數。若沒有設定的話預設會是 " Plot "+序列編號。

此函數能夠在線圖上指定區域填色的功能，例如 KD 指標的超買超賣區間，讓使用者能夠更簡單的辨識指標間的範圍。

詳細說明可參考：[如何運用函數繪製填色區塊](https://www.xq.com.tw/learn/xspractice/function-coloring/) 文章。

---
## PlotK
**Path:** 內建函數 > 一般函數

PlotK -  (內建函數)
一般函數

語法：

在腳本運算的橫軸位置上畫出K棒。  
PlotK(序列編號, vOpen, vHigh, vLow, vClose)  
PlotK(序列編號, vOpen, vHigh, vLow, vClose, "序列名稱")

說明：

序列編號是1~999的數值，與目前XS Plot的序列編號相同。
序列名稱是非必需的參數，如果不傳的話，預設的序列名稱為"Plot"+序列編號，例如”Plot2”。
vOpen, vHigh, vLow, vClose 對應的是K棒的開高低收。

平均K線 (Heikin-Ashi) 範例：

```
var: ha_open(0), ha_high(0), ha_low(0), ha_close(0);


if currentbar = 1 then
  ha_open = (open + close) / 2
else
  ha_open = (open[1] + close[1]) / 2;


ha_close = (open + high + low + close) / 4;
ha_high = maxlist(high, ha_open, ha_close);
ha_low = minlist(low, ha_open, ha_close);


PlotK(1, ha_open, ha_high, ha_low, ha_close, "平均K線");
```

---
## PlotLine
**Path:** 內建函數 > 一般函數

PlotLine -  (內建函數)
一般函數

語法：

繪製直線。  
PlotLine(序列編號, x1, y1, x2, y2)  
PlotLine(序列編號, x1, y1, x2, y2, "序列名稱")

說明：

大家好，今天來跟大家介紹一個XS指標的新功能：PlotLine，用來繪製直線。

請參考：[PlotLine語法的介紹](https://www.xq.com.tw/learn/indicator/plotline/) 文章

---
## Print
**Path:** 內建函數 > 一般函數

Print -  (內建函數)
一般函數

語法：

將文字/數值輸出到XSScript編輯器的執行畫面跟檔案內  
Print(數值1, 數值2, 數值3, ...)  
Print(指定檔案,數值1, 數值2, 數值3, ...) ← 交易腳本必須用此法才能列印到檔案。  
※執行選股與執行回測時，Print檔案加總超過 100M 就不會印出。

說明：

Print函數可以傳入多個參數，使用逗號分隔，參數可以是文字或是數值。每個Print函數會產生一行的輸出，內容為傳入的參數的文字或是數值，每個參數之間有一個空白。

範例:

```
Print("Date=", NumToStr(Date, 0), "Close=", NumToStr(Close, 2));
```

把上述指標腳本放入技術分析內，執行時可以在XSScript編輯器的執行畫面內看到輸出，每一筆bar寫出一筆紀錄

![Print紀錄](https://www.xq.com.tw/xstrader/wp-content/uploads/2015/09/Print2.png)

Print函數的執行結果除了在XSScript編輯器內可以看到之外，另外也會產生一個文字檔案。

檔案的輸出位置是在**XQ安裝目錄**底下的**XS\Print**子目錄內，檔案名稱預設為*策略名稱*加上*商品名稱*，檔名為\*\*.log\*\*。以上述為例輸出的檔案名稱為 C:\SysJust\XQ2005\XS\Print\Print範例\_2330.TW.log

策略名稱也就是代表雷達名稱、選股策略名稱、指標名稱或自動交易策略名稱。

使用者也可以利用[File指令](api?a=file&b=bif)來指定輸出的目錄或是檔名，交易腳本必須用此法才能列印到檔案。

以下的範例會把輸出檔案寫在"d:\print"這個目錄內：

```
Print(file("d:\print\"), "Date=", NumToStr(Date, 0), "Close=", NumToStr(Close, 2));
```

---

如果需要避開重覆Print在同一個檔案，可以運用[File指令](api?a=file&b=bif)搭配[StartTime]參數，讓每次執行的Print檔案可以分開不同目錄，檔案維護上比較方便。

以下的範例會把輸出資料分開到不同檔案：

```
Print(file("[StrategyName]_[Symbol]_[StartTime].log"), "Date=", NumToStr(Date, 0), "Close=", NumToStr(Close, 2));
```

請參考[File指令](api?a=file&b=bif)，以及[教學文章](https://xstrader.net/print%E8%AA%9E%E6%B3%95%E8%AA%BF%E6%95%B4/)。

---
## RaiseRunTimeError
**Path:** 內建函數 > 一般函數

RaiseRunTimeError -  (內建函數)
一般函數

語法：

用來中斷執行中的程式  
RaiseRunTimeError(錯誤訊息)

說明：

當腳本遇到任何重大錯誤時，可以使用RaiseRunTimeError函數來終止腳本的執行。

舉例而言:

```
if q_CurrentShareCapital < 100000000{100,000,000股*10 = 10億} then RaiseRunTimeError("市值小於10億踢除");
```

上述是一個警示腳本，透過 [q\_CurrentShareCapital](api?a=q_CurrentShareCapital) 欄位來判斷商品的股本是否小於10億，如果小於10億是的話則中斷執行。

與下列程式比較:

```
if q_CurrentShareCapital < 100000000{100,000,000股*10 = 10億} then return;
```

請注意如果是使用return指令的話，則執行的這一筆bar雖然會被跳出，可是當還有新的K棒時，程式還是會繼續執行，如果判斷是否要跳出的邏輯比較複雜的話，可能會有一些效率上的影響。如果已經確定腳本不需要再執行的話，可以使用RaiseRuntimeError，比較有效率，而且執行的畫面上也可以看到錯誤訊息，方便使用者掌握腳本的狀態。

---
## SetAlign
**Path:** 內建函數 > 一般函數

SetAlign -  (內建函數)
一般函數

語法：

根據欄位屬性，指定腳本執行時的資料對位計算方式  
SetAlign("籌碼",資料對位計算方式)  
SetAlign("營收財報",資料對位計算方式)

說明：

SetAlign可以根據欄位屬性，指定腳本執行時的資料對位計算方式。

### 說明資料的對位定義

無論是資料欄位或選股欄位，皆會在欄位可用頻率的K棒生成後，將對應時間的歷史資料標記在此根K棒上，舉例來說：

資料欄位 "外盤量" 的可用頻率為分鐘、日、還原日，因此當GetField("外盤量",”10”)[n]時，可以想像在距離最新10分K之前的第n根10分K上，有一筆"外盤量"的資料標記在上面。

至於最新K棒尚未結束之前，GetField("外盤量",”10”)[0]都會不斷被更新至最新10分K的標記當中。
  
在對資料與K棒之間的標記關係，有了理解之後，就要來說明資料的對位方式，有以下兩種：
  
**1. 絕對對位：根據資料名義上的所屬期別來標記對位。**

例如 "大戶持股張數" 最快每周更新一次，那資料就會標記在當周的第一根K棒； "每股現金流量" 最快每季更新一次，那資料就會標記在當季的第一根K棒（標記更新後的持續區間，都會取得期初標記的資料，直到標記再次被更新為止）。

**2. 公布日對位：根據資料何時能被XS取得來標記對位**（不一定等於資料被公司公布的時間，因為第一手資料公布後，上游的資料源會接收、整理後再轉給XQ，接著再轉成XS可以取得的格式）。

例如 "每股現金流量" 第一季的資料在4/16首次可以被XS取得、第二季的資料在7/13首次可以被XS取得。那4/16會標記第一季的資料；7/13的K棒才會標記第二季的資料（4/16~7/12之間的K棒，會取得的是第一季的資料）。

### SetAlign的使用

* **指標**、**警示**、**自動交易腳本**的範例如下：

  ```
  ```
  SetAlign("籌碼", 0);   //指標腳本的預設值是「絕對對位」
  SetAlign("營收財報", 0); //指標腳本的預設值是「絕對對位」

  SetAlign("籌碼", 1);   //警示、自動交易腳本的預設值是「公布日對位」
  SetAlign("營收財報", 1); //警示、自動交易腳本的預設值是「公布日對位」
  ```
  ```
* **選股腳本**則不在SetAlign的支援範圍：
  選股欄位中，除了籌碼欄位是絕對對位以外，其他欄位都是公佈日對位。

---
## SetBackBar
**Path:** 內建函數 > 一般函數

SetBackBar -  (內建函數)
一般函數

語法：

腳本執行時，設置指定頻率的最大引用筆數  
SetBackBar(最大引用筆數)  
SetBackBar(最大引用筆數, "頻率")  
如果不傳頻率的話，則指定目前執行頻率的最大引用筆數。

說明：

腳本執行時，設置指定頻率的最大引用資料範圍。

詳細介紹可以參考[設置指定頻率引用筆數的應用](https://www.xq.com.tw/lesson/xspractice/%E8%A8%AD%E7%BD%AE%E6%8C%87%E5%AE%9A%E9%A0%BB%E7%8E%87%E5%BC%95%E7%94%A8%E7%AD%86%E6%95%B8%E7%9A%84%E6%87%89%E7%94%A8/)。

關於資料讀取範圍以及最大引用筆數的定義，請參考[資料讀取範圍與腳本執行的關係](http://www.xq.com.tw/lesson/xspractice/%E8%B3%87%E6%96%99%E8%AE%80%E5%8F%96%E7%AF%84%E5%9C%8D%E8%88%87%E8%85%B3%E6%9C%AC%E5%9F%B7%E8%A1%8C%E7%9A%84%E9%97%9C%E4%BF%82/)。

---
## SetBarBack
**Path:** 內建函數 > 一般函數

SetBarBack -  (內建函數)
一般函數

語法：

腳本執行時，設置指定頻率的最大引用筆數  
SetBarBack(最大引用筆數)  
SetBarBack(最大引用筆數, "頻率")  
如果不傳頻率的話，則指定目前執行頻率的最大引用筆數。

說明：

腳本執行時，設置指定頻率的最大引用資料範圍。

詳細介紹可以參考[設置指定頻率引用筆數的應用](https://www.xq.com.tw/lesson/xspractice/%E8%A8%AD%E7%BD%AE%E6%8C%87%E5%AE%9A%E9%A0%BB%E7%8E%87%E5%BC%95%E7%94%A8%E7%AD%86%E6%95%B8%E7%9A%84%E6%87%89%E7%94%A8/)。

關於資料讀取範圍以及最大引用筆數的定義，請參考[資料讀取範圍與腳本執行的關係](http://www.xq.com.tw/lesson/xspractice/%E8%B3%87%E6%96%99%E8%AE%80%E5%8F%96%E7%AF%84%E5%9C%8D%E8%88%87%E8%85%B3%E6%9C%AC%E5%9F%B7%E8%A1%8C%E7%9A%84%E9%97%9C%E4%BF%82/)。

---
## SetBarFreq
**Path:** 內建函數 > 一般函數

SetBarFreq -  (內建函數)
一般函數

語法：

指定這個腳本可以支援的頻率(只可使用在選股腳本內)  
SetBarFreq(支援頻率1, 支援頻率2, 支援頻率3, ...)  
可以傳入多個頻率字串

說明：

SetBarFreq可以傳入多個頻率字串，使用逗號分隔，用來指定選股腳本可以使用的頻率。

頻率參數的格式如下

* 日線: "D"，
* 還原日線: "AD"，
* 周線: "W"，
* 還原周線: "AW"，
* 月線: "M"，
* 還原月線: "AM"，
* 季線: "Q"，
* 半年線: "H"，
* 年線: "Y"

範例:

```
SetBarFreq("Q", "Y"); // 指定選股腳本只能執行在季線/年線的頻率上面
```

由於選股腳本內可能會同時運用到多種不同欄位的頻率，而不同頻率的欄位又有可能因為資料公佈的時間差而產生期別上的差異。為了幫助使用者選到合適的執行頻率，XS選股程式在執行時會先分析這個腳本內所使用到的所有欄位的頻率，然後列出可以挑選的頻率。如果使用者希望可以更精確的指定頻率的話，則可以使用SetBarFreq這個函數。

---
## SetBarMode
**Path:** 內建函數 > 一般函數

SetBarMode -  (內建函數)
一般函數

語法：

指定腳本執行時的函數計算方式  
SetBarMode(函數計算方式)

說明：

SetBarMode可以指定函數的計算方式，分別為(0),(1),(2)

對於Setbarmode 0,1,2 三種計算方式解釋:

```
SetBarMode(0); //Auto，預設值
```

由系統判定是simple函數 或是 series 函數

```
SetBarMode(1);  //指定為simple函數
```

Simple型態是指，例如average 這類函數計算方式，今期所計算的平均數與前一期的平均數為個別獨立運用，不會相互有關係
例如:平均數average
(1,2,3,4,5)/5 = 3 ；(2,3,4,5,6)/5 = 4
兩者計算的結果無關聯

```
SetBarMode(2);  //指定為series函數
```

Series 型態是指，例如MACD,RSI 指標，屬於連續性的數值，今期所計算的值會引用到前期的數值來做運算。

例如RSI指標計算「期間內絕對漲幅」的公式為
UP t = UP t-1 + 1 / N ( Ut – UP t-1) ，
(N 為平滑平均天數， t 為當日值， t-1為前一日值)
當期值t會使用到前期的(t-1) 進行運算

---
## SetFirstBarDate
**Path:** 內建函數 > 一般函數

SetFirstBarDate -  (內建函數)
一般函數

語法：

指定腳本執行時第一筆資料的日期(不支援交易腳本)  
SetFirstBarDate(資料開始日期)

說明：

關於資料讀取範圍的定義，請參考[資料讀取範圍與腳本執行的關係](http://www.xq.com.tw/lesson/xspractice/%E8%B3%87%E6%96%99%E8%AE%80%E5%8F%96%E7%AF%84%E5%9C%8D%E8%88%87%E8%85%B3%E6%9C%AC%E5%9F%B7%E8%A1%8C%E7%9A%84%E9%97%9C%E4%BF%82/)。

SetFirstBarDate 函數用於控制腳本執行時，所使用的第一個資料的日期，從而確定資料讀取的起始範圍。

語法為 SetFirstBarDate(YYYYMMDD)，其中 YYYYMMDD 是起始日期的年、月、日，且必須為合理有效的日期。

需要注意的是，SetFirstBarDate **不支援交易腳本**。

**如果在腳本中多次使用 SetFirstBarDate 函數，並設定了不同的日期：**

* 將會採用其中最早（最小值）的日期作為最終的資料開始日期。

     若設定的日期不是合理有效的日期，該行的 SetFirstBarDate 將被視為編譯失敗。

**如果在腳本中同時存在數個 [SetTotalBar](https://xshelp.xq.com.tw/XSHelp/?HelpName=SetTotalBar&group=GENERALFUNC) 和 SetFirstBarDate，並設定了不同的數值時：**

* 系統將分別根據兩者被多次使用時的規則，決定接下來各自採用哪一個 [SetTotalBar](https://xshelp.xq.com.tw/XSHelp/?HelpName=SetTotalBar&group=GENERALFUNC) 與 SetFirstBarDate 做比較。
* 接著採用兩者當中，最後一個成功完成編譯的函數設定。

     若其中一個函數因參數無效（例如 [SetTotalBar](https://xshelp.xq.com.tw/XSHelp/?HelpName=SetTotalBar&group=GENERALFUNC) 的資料讀取筆數為負數，或 SetFirstBarDate 的日期不合理）而編譯失敗，則只有另一個成功編譯的函數設定會被採用。

---
## SetInputName
**Path:** 內建函數 > 一般函數

SetInputName -  (內建函數)
一般函數

語法：

設定輸入參數(Input)的顯示名稱  
SetInputName(序號, 顯示名稱)  
SetInputName1(顯示名稱)

說明：

在XS語法內可以使用[Input語法](api?a=input)來設定腳本輸入的參數。

```
Input: Length(10);


Plot1(Average(Close, Length));
```

例如上面範例內定義了一個輸入參數，名稱為Length，初始值為10。在腳本內可以直接使用**Length**這個變數，而在腳本執行時則可以利用參數設定畫面來動態修改**Length**的數值，以便讓程式的設計更有彈性。

![Input設定](https://www.xq.com.tw/xstrader/wp-content/uploads/2015/09/InputWithName.png)

如果希望在設定畫面上可以看到中文名稱，而不是英文的變數名稱的話，則可以使用SetInputName這個函數。

```
Input: Length(10);


SetInputName(1, "天期");
Plot1(Average(Close, Length));
```

SetInputName必須傳入兩個參數

* 第一個參數是參數的序號，從1開始，
* 第二個參數是參數的顯示名稱

以下是指標設定的畫面，標示處內可以看到透過SetInputName所指定的參數的名稱

![Input設定2](https://www.xq.com.tw/xstrader/wp-content/uploads/2015/09/Input_%E8%A8%AD%E5%AE%9A.png)

SetInputField指令也可以在函數名稱之後直接加上序號，例如SetInputName1, SetInputName2等。如果函數名稱內就包含序號的話，則就不需要傳入序號參數。

上面的範例可以改寫成:

```
Input: Length(10);


SetInputName1("天期");
Plot1(Average(Close, Length));
```

在XQ 5.60版之後，為了讓這個動作更簡單，使用者可以直接在Input語法內指定輸入參數的顯示名稱，上面的範例可以改寫成:

```
Input: Length(10, "天期");


Plot1(Average(Close, Length));
```

新的Input的語法可以讓程式變的更短，而且由於顯示名稱跟Input可以寫在同一行內，使用者不需要再去記憶每個Input的序號，建議大家以後直接使用Input語法來指定輸入參數的顯示名稱。

---
## SetOutputName
**Path:** 內建函數 > 一般函數

SetOutputName -  (內建函數)
一般函數

語法：

指定選股的輸出欄位標題  
SetOutputName(序號, 欄位標題)  
SetOutputName1(欄位標題)

說明：

在XS語法內可以使用[OutputField指令](api?a=outputfield&b=bif)來產生選股時的輸出欄位。

預設的輸出欄位的標題為 "欄位"加上"序號"，例如"欄位1", "欄位2"等。為了讓輸出報表更清楚，可以使用SetOutputName指令來設定輸出欄位的名稱。

SetOutputName必須傳入兩個參數:

* 第一個參數是欄位的序號，從1到99，
* 第二個參數是欄位的名稱

例如:

```
OutputField1(GetField("月營收年增率","M"));
SetOutputName(1, "月營收年增率");
```

在上面範例內指定第一個輸出欄位的標題為"月營收年增率"。

SetOutputField指令也可以在函數名稱之後直接加上序號，例如SetOutputName1, SetOutputName2等。如果函數名稱內就包含序號的話，則就不需要傳入序號參數。

上面的範例可以改寫成:

```
OutputField1(GetField("月營收年增率","M"));
SetOutputName1("月營收年增率");
```

在XQ 5.60版之後，[OutputField指令](api?a=outputfield&b=bif)也增加了可以直接傳入欄位標題的功能。

---
## SetPlotLabel
**Path:** 內建函數 > 一般函數

SetPlotLabel -  (內建函數)
一般函數

語法：

設定繪圖序列的名稱  
SetPlotLabel(繪圖序列編號，繪圖序列名稱)

說明：

SetPlotLabel傳入兩個參數

* 第一個參數是繪圖序列的編號，可以從1到99，
* 第二個參數則是這個序列的名稱，為一個字串值

這個函數可以用來指定這個繪圖序列的名稱，跟[Plot函數](api?a=plot&b=bif)的第二個參數是類似的。

兩者最大的差異是，在Plot函數內的第二個參數目前只支援固定的字串，而SetPlotLabel的第二個參數則可以是一個字串相關的敘述式，使用上比較有彈性。

舉例而言:

```
Input: Period(10);


Plot1(Average(Close, Period));
SetPlotLabel(1, Text("天期(", NumToStr(Period, 0), ")"));
```

在上述範例內我們希望指標圖形上面可以看到平均線的天期，例如如果天期是5的話，我們希望指標序列的名稱是"天期(5)"，而如果天期是10的話，則我們希望指標序列的名稱是"天期(10)"。

由於天期是透過[Input語法](api?a=input)傳入的，數值可以動態被修改，沒有辦法寫成一個固定的字串，所以我們使用SetPlotLabel，搭配[Text函數](api?a=text&b=bif)以及[NumToStr函數](api?a=numtostr&b=bif)來組出天期的字串。

---
## SetRemoveOutlier
**Path:** 內建函數 > 一般函數

SetRemoveOutlier -  (內建函數)
一般函數

語法：

排除Rank語法中的離群值商品，被排除的商品不會進入排行。  
SetRemoveOutlier("zscore", value:=3)  
傳入兩個參數：  
- 第一個參數為排除離群值的方式，有 "zscore" 和 "IQR"。  
- 第二個參數為排除離群值的範圍，zscore預設為3，IQR預設為1.5，此數值需大於0。

說明：

此語法會讓離群值商品在排行前就被排除，也不會被納入計算其他屬性，例如 avgvalue。

此語法需寫在 rank 語法內，且每一個rank只能有一個。

此語法必須在rank內的最上層，不能夠放在 if 或 for 等邏輯判斷內。

以下是簡單範例：

```
Rank myRank Begin 
    RetVal = HVolatility(Close,20);
    SetRemoveOutlier("zscore", value:=3);
    end;
```

此範例會用波動率進行排行，但會先排除掉 zscore 絕對值大於3的商品。

---
## SetTBMode
**Path:** 內建函數 > 一般函數

SetTBMode -  (內建函數)
一般函數

語法：

指定自定指標的繪圖模式  
SetTBMode(繪圖模式)

說明：

指定自定指標的繪圖模式。
支援模式的如下

* SetTBMode(1) 同v5.62行為，腳本資料計算筆數為資料讀取筆數加畫面上的K棒筆數，使用者拉動畫面會進行重算。
* SetTBMode(0) 預設值，腳本資料計算筆數為全部資料，整個數列只算一次，拉動畫面不會重算。

範例:

```
Input: Period(200, "EMA");
SetTBMode(1);//指定自定指標的繪圖模式，可以變更參數比較一下計算值的差異
Plot1(EMA(Close, Period), "EMA");
```

---
## SetTotalBar
**Path:** 內建函數 > 一般函數

SetTotalBar -  (內建函數)
一般函數

語法：

指定腳本執行時的資料讀取範圍  
SetTotalBar(資料讀取筆數)

說明：

關於**資料讀取範圍**的定義，請參考[資料讀取範圍與腳本執行的關係](http://www.xq.com.tw/lesson/xspractice/%E8%B3%87%E6%96%99%E8%AE%80%E5%8F%96%E7%AF%84%E5%9C%8D%E8%88%87%E8%85%B3%E6%9C%AC%E5%9F%B7%E8%A1%8C%E7%9A%84%E9%97%9C%E4%BF%82/)。

SetTotalBar 用於設定腳本執行時，讀取的歷史K棒數量。

SetTotalBar 設定的資料讀取筆數必須為**非負整數**。當腳本引用 SetTotalBar 並指定一個數值時，系統會預先提供指定數值的K棒，並從這些K棒的第一根 (編號為1) 開始執行腳本。

不同腳本類型的規則：

* 指標腳本： 將繪製的歷史 K 棒數量，不包含即時的 K 棒。
* 選股腳本： 判斷目前K棒是否滿足選股條件之前，將執行的 K 棒數量。
* 警示腳本： 開始接收即時價格更新並觸發警示訊號之前，將執行的 K 棒數量，不包含即時的 K 棒。
* 自動交易腳本：([策略部位計算起點](https://www.xq.com.tw/learn/xsat/parameters/#learn04)之前，或)開始接收即時價格更新並觸發警示訊號之前，將執行的 K 棒數量，不包含即時的 K 棒。

**如果在腳本中多次使用 SetTotalBar ，並設定了不同的數值：**

* 將會採用其中編譯成功的最大數值作為最終的K棒總數。

     若設定的數值不是非負整數，該行的SetTotalBar將被視為編譯失敗。

**如果在腳本中同時存在數個 SetTotalBar 和 [SetFirstBarDate](https://xshelp.xq.com.tw/XSHelp/?HelpName=SetFirstBarDate&group=GENERALFUNC) ，並設定了不同的數值時：**

* 系統將分別根據兩者被多次使用時的規則，決定接下來各自採用哪一個 SetTotalBar 與 [SetFirstBarDate](https://xshelp.xq.com.tw/XSHelp/?HelpName=SetFirstBarDate&group=GENERALFUNC) 做比較。
* 接著採用兩者當中，最後一個成功完成編譯的函數設定。

     若其中一個函數因參數無效（例如 SetTotalBar 的資料讀取筆數為負數，或 [SetFirstBarDate](https://xshelp.xq.com.tw/XSHelp/?HelpName=SetFirstBarDate&group=GENERALFUNC) 的日期不合理）而編譯失敗，則只有另一個成功編譯的函數設定會被採用。

---
## SymbolExchange
**Path:** 內建函數 > 一般函數

SymbolExchange -  (內建函數)
一般函數

語法：

目前執行商品的交易所編碼  
回傳代碼 = SymbolExchange

說明：

SymbolExchange函數回傳目前執行商品的交易所編碼，例如 "TW"。

當回傳值為"TW"時，表示商品屬於台灣證券交易所（上市&上櫃）。
當回傳值為"TE"時，表示商品屬於台灣興櫃。
當回傳值為"TF"時，表示商品屬於台灣期貨交易所。
當回傳值為"FS"時，表示商品屬於國際指數。
當回傳值為"FX"時，表示商品屬於外匯。
當回傳值為"HK"時，表示商品屬於香港交易所。
當回傳值為"SH"時，表示商品屬於上海交易所。
當回傳值為"SZ"時，表示商品屬於深圳交易所。
當回傳值為"KS"時，表示商品屬於韓股。
當回傳值為"JP"時，表示商品屬於日股。
當回傳值為"US"時，表示商品屬於美股。
當回傳值為"SG"時，表示商品屬於新加坡。

範例:

```
If SymbolExchange = "TW" then
begin
    // 目前執行的商品為台股商品
end;
```

---
## SymbolType
**Path:** 內建函數 > 一般函數

SymbolType -  (內建函數)
一般函數

語法：

回傳目前執行腳本的商品類型  
回傳代碼 = SymbolType

說明：

SymbolType函數回傳目前執行腳本的商品類型。

當回傳值為1時，表示商品是指數。

當回傳值為2時，表示商品是股票。

當回傳值為3時，表示商品是期貨。

當回傳值為4時，表示商品是權證。

當回傳值為5時，表示商品是選擇權。

當回傳值為6時，表示商品是可轉債。

當回傳值為7時，表示商品是特別股。

範例:

```
If SymbolType = 3 then
begin
    // 目前執行的商品為期貨
end;
```

---
## AddSpread
**Path:** 內建函數 > 交易函數

AddSpread -  (內建函數)
交易函數

語法：

計算調整檔位後的價格  
Value1 = AddSpread(基礎價格, 檔位)

說明：

AddSpread函數用來計算依照商品跳動點加減幾檔後的價格。

呼叫時需要傳入兩個參數：第一個參數是基礎價格，第二個參數是加減檔數，如果要加檔數的話請傳入一個大於0的數值，如果要減檔數的話請傳入一個小於0的數值。如果商品有漲跌停限制的話，計算後的數值不會超過漲跌停限制。

**範例**

```
Value1 = AddSpread(Close, 1);  { 收盤價+1檔 }
Value2 = AddSpread(Low, -1); { 最低價-1檔 }
```

這個函數主要應用在交易的情境，如果目前的執行商品並沒有檔位限制的話(例如加權指數)，則系統會以商品的報價跳動點來當成檔位的計算依據(例如加權指數的話則每一檔是0.01)。

---
## Alert
**Path:** 內建函數 > 交易函數

Alert -  (內建函數)
交易函數

語法：

在自動交易策略的執行過程內產生一筆警示紀錄  
Alert(字串1)  
Alert(字串1, 數值1, ...)

說明：

Alert函數可以傳入一個或是多個參數，參數的型態可以是文字、數字、或是布林值，系統會自動把傳入的參數轉成字串格式，如果傳入多個參數的話，不同參數之間會用空白接續起來。

透過Alert函數，自動交易策略在執行過程內可以產生一筆警示紀錄，達到類似策略雷達警示的提示效果。

當自動交易策略產生警示時，使用者可以透過以下幾種方式看到警示紀錄：

**自動交易策略中心的執行紀錄畫面：**

![自動交易策略中心_執行紀錄](https://xq.com.tw/wp-content/uploads/2022/11/%E8%AD%A6%E7%A4%BA01.png)

**自動交易策略中心的商品監控畫面：**

![商品監控](https://www.xq.com.tw/wp-content/uploads/2020/11/20201223_%E5%95%86%E5%93%81%E7%9B%A3%E6%8E%A7.png)

**警示提示視窗：**

![警示mode](https://xq.com.tw/wp-content/uploads/2022/11/%E8%AD%A6%E7%A4%BA02.jpg)

**自動交易中心開啟推播後，可以傳到手機：**

![策略開啟推撥與手機上的顯示](https://www.xq.com.tw/wp-content/uploads/2020/10/20201130_%E7%AD%96%E7%95%A5%E9%96%8B%E5%95%9F%E6%8E%A8%E6%92%A5%E8%88%87%E6%89%8B%E6%A9%9F%E4%B8%8A%E7%9A%84%E9%A1%AF%E7%A4%BA.jpg)

---
## Buy
**Path:** 內建函數 > 交易函數

Buy -  (內建函數)
交易函數

語法：

把策略的多單部位變大，進行多單加碼的動作。  
Buy(多單加碼數量)  
Buy(多單加碼數量，委託價格)  
Buy(多單加碼數量，label:="指令標記")  
Buy(多單加碼數量，委託價格，label:="指令標記")

說明：

Buy函數的作用是把目前多單的部位變大，用來進行多單加碼的動作。

Buy函數的第一個參數是加碼部位，必須是一個正整數。第二個參數是此次交易的委託價格，第二個參數如果不傳的話則會使用策略的預設買進價格。

與SetPosition一樣，也可以透過label函數傳入指令標記。

**範例**

```
Buy(1);
Buy(1, Close);
Buy(1, MARKET);
Buy(1, label:="買進1張");
```

**注意事項**

當目前的Position < 0時，執行Buy指令時會先把空頭部位全部平倉(部位變成0)，之後再進行加碼的動作。

Buy(0)是一個特殊用法，如果此時的部位小於0的話，Buy(0)的作用是把部位變成0，如果此時的部位大於0的話，則Buy(0)沒有任何作用。

以下是Buy(N)的執行邏輯：

```
if Position < 0 then
    SetPosition(N)    { 從Position(負數)變成N(正數) }
else
    SetPosition(Position + N);  { 從Position(零或是正數)變成Position+N }
```

---
## CancelAllOrders
**Path:** 內建函數 > 交易函數

CancelAllOrders -  (內建函數)
交易函數

語法：

刪除委託單。  
CancelAllOrders()  
CancelAllOrders(label:="指令標記")

說明：

大家好，今天跟大家介紹一個XS自動交易的新語法：CancelAllOrders，目的是用來刪除委託單。

請參考：[CancelAllOrders語法說明](https://www.xq.com.tw/learn/xsat/cancelallorders/) 文章

---
## Cover
**Path:** 內建函數 > 交易函數

Cover -  (內建函數)
交易函數

語法：

把策略的空單部位變小，進行空單回補(減碼)的動作。  
Cover(空單回補數量)  
Cover(空單回補數量，委託價格)  
Cover(空單回補數量，label:="指令標記")  
Cover(空單回補數量，委託價格, label:="指令標記")

說明：

Cover函數的作用是把目前空單的部位變小，用來進行空單回補(減碼)的動作。請注意Cover減碼之後的最大部位是0，保證不會把目前的部位變成多單(Position > 0)。

Cover函數的第一個參數是回補部位，必須是一個正整數。第二個參數是此次交易的委託價格，第二個參數如果不傳的話則會使用策略的預設買進價格。

與SetPosition一樣，也可以透過label函數傳入指令標記。

**範例**

```
Cover(1);
Cover(1, Close);
Cover(1, MARKET);
Cover(1, label:="回補1張");
```

**注意事項**

Cover指令只有在目前Position < 0時才會有作用。如果Cover的回補數量超過目前空單數量的話，則Position會改成0，也就是說Cover函數不會把目標部位變成多頭(Position > 0)。

Cover(0)是一個特殊用法，如果此時的部位小於0的話，Cover(0)的作用是把部位變成0，如果此時的部位大於0的話，則Cover(0)沒有任何作用。

以下是Cover(N)的執行邏輯：

```
if Position < 0 then
    if N = 0 then
        SetPosition(0)  { N=0是特殊用法，把空單部位全部回補 }
    else
        SetPosition(minlist(Position + N, 0)); { 從Position(負數)增加N張，最終數字不會大於0 }
```

---
## DefaultBuyPrice
**Path:** 內建函數 > 交易函數

DefaultBuyPrice -  (內建函數)
交易函數

語法：

回傳自動交易預設的買進價格  
Value1 = DefaultBuyPrice

說明：

自動交易策略可以設定預設的買進(賣出)價格，指定的方式可以是

* 市價 (如果是期貨的話，則是範圍市價)
* 觸發價 +/- 檔數

![defaultbuyprice](https://www.xq.com.tw/wp-content/uploads/2020/10/defaultbuyprice.png)

如果策略指定預設買進價格為市價的話，DefaultBuyPrice會回傳商品的漲停價(如果商品沒有漲停跌停價的話，則回傳洗價K棒的收盤價)。如果指定的是觸發價 +/- 檔數的話，則依照回傳洗價K棒的收盤價加減檔數後的價格。與[AddSpread](api?a=AddSpread&b=bif)一樣，加減檔數後的數值不會超過商品的漲跌停限制。

使用者可以利用這個函數，取得交易指令的實際委託價格，可以應用於，例如利用金額來換算委託數量。

```
input: ordersize_w(10, "每筆交易金額(萬)");


{ 換算委託張數 }
value1 = (ordersize_w * 10000) / (DefaultBuyPrice * 1000);
SetPosition(value1);
```

---
## DefaultSellPrice
**Path:** 內建函數 > 交易函數

DefaultSellPrice -  (內建函數)
交易函數

語法：

回傳自動交易預設的賣出價格。  
Value1 = DefaultSellPrice

說明：

自動交易策略可以設定預設的買進(賣出)價格，指定的方式可以是

* 市價 (如果是期貨的話，則是範圍市價)
* 觸發價 +/- 檔數

![defaultsellprice](https://www.xq.com.tw/wp-content/uploads/2020/10/defaultsellprice.png)

如果策略指定預設賣出價格為市價的話，DefaultSellPrice會回傳商品的跌停價(如果商品沒有漲停跌停價的話，則回傳洗價K棒的收盤價)。如果指定的是觸發價 +/- 檔數的話，則回傳依照洗價K棒的收盤價加減檔數後的價格。與[AddSpread](api?a=AddSpread&b=bif)一樣，加減檔數後的數值不會超過商品的漲跌停限制。

使用者可以利用這個函數，取得交易指令的實際委託價格，可以應用於，例如利用金額來換算委託數量。

```
input: ordersize_w(10, "每筆交易金額(萬)");


{ 換算委託張數 }
value1 = (ordersize_w * 10000) / (DefaultSellPrice * 1000);
SetPosition(-1 * value1);
```

---
## Filled
**Path:** 內建函數 > 交易函數

Filled -  (內建函數)
交易函數

語法：

取得目前商品的成交部位。  
Value1 = Filled

說明：

Filled代表的是自動交易策略內目前執行商品的成交部位。

Filled是一個整數，可以大於0、等於0、也可以小於0。從自動交易腳本內，透過呼叫交易指令，例如[SetPosition](api?a=SetPosition&b=bif)、[Buy](api?a=Buy&b=bif)、[Sell](api?a=Sell&b=bif)，來執行交易的動作。系統執行交易指令之後，[Position](api?a=Position&b=bif)的數值會跟著異動，如果個交易指令成交之後，則Filled的數值也會跟著異動。

舉例而言，假如腳本開始執行時Position是0，Filled也是0。

當腳本呼叫SetPosition(1)之後，系統會送出一筆買進1張的委託，Position會變成1，此時如果尚未成交的話，Filled還是0，等到這一筆委託單成交之後，Filled就會變成1。

如果委託單部分成交的話，則Filled也會反應部分成交的狀態。例如腳本呼叫SetPosition(2)之後，系統會送出一筆買進2張的委託，Position會變成2。此時如果尚未成交的話，Filled是0，如果這一筆委託單成交1張的話，Filled會變成１，等到這一筆委託單完成成交時，Filled就會變成2。

交易腳本在每一次執行(洗價)之前系統會決定Filled的數值，接下來的執行過程內，不管有沒有收到成交回報，Filled的數值都不會改變。

如同Position，Filled也是一個數值序列。Filled或是Filled[0]代表的是目前這一根bar的成交部位數值。Filled[1]則是上一根bar執行時的成交部位數值。如果開啟逐筆洗價的話，每一根bar內可能會執行多次，Filled的數值則會在每一次洗價之前就會依照當時的成交狀態更新。

透過比對Position跟Filled，腳本可以判斷目前腳本的成交情形，進而做不同的後續處理：

**範例#1**

```
if Position = 0 and entry_condition then SetPosition(1);
if Filled = 1 then begin
   if Close >= FilledAvgPrice * 1.02 then SetPosition(0);
   if Close <= FilledAvgPrice * 0.98 then SetPosition(0);
end;
```

在這個範例內腳本透過判斷Filled是否是1來決定是否已經成交。一旦成交之後，則透過比對目前價格與未平倉成本[FilledAvgPrice](api?a=FilledAvgPrice&b=bif)是否達到停損停利的範圍來決定是否要進行平倉。

**範例#2**

```
if Position = 0 and entry_condition then SetPosition(1);
if TrueAll(Position <> Filled, 3) then SetPosition(1, MARKET);
```

在這個範例內腳本透過比對Position跟Filled的差異來決定是否尚未成交。如果Position跟Filled連續3根bar都不一樣的話，那麼就改用市價買進。

---
## FilledAtBroker
**Path:** 內建函數 > 交易函數

FilledAtBroker -  (內建函數)
交易函數

語法：

Value1 = FilledAtBroker;

說明：

此函數會回傳對應商品的實際庫存數量，這個數值會是正、負的整數或 0。

該數值會隨策略設定的現股、資券而取得該類別的庫存數量。

FilledAtBroker 和 Filled 可能會不同，且策略運作時同一個券商帳號內發生的交易都會影響到此函數的回傳數值。

更詳細的說明可參考 [如何使用函數取得商品的實際庫存數量](https://www.xq.com.tw/learn/xspractice/function-inventory/) 此篇文章。

---
## FilledAvgPrice
**Path:** 內建函數 > 交易函數

FilledAvgPrice -  (內建函數)
交易函數

語法：

取得商品目前的未平倉成本。  
Value1 = FilledAvgPrice

說明：

FilledAvgPrice回傳目前商品的未平倉成本。如果目前Filled數值是0的話，則FilledAvgPrice回傳0，否則FilledAvgPrice回傳的是一個大於等於0的數值，與Filled的方向無關。

**範例**

```
if Filled > 0 then begin
    if Close >= FilledAvgPrice * 1.02 then SetPosition(0);
    if Close <= FilledAvgPrice * 0.98 then SetPosition(0);
end;
```

### 未平倉成本計算邏輯

腳本執行過程內，系統會紀錄每一筆成交紀錄，依照成交時間排序，然後依照這些成交紀錄的成交價格/數量，採用先進先出的方式計算商品的未平倉成本。

**範例#1**

假設以下三筆成交記錄, 依照時間順序:

* 第1筆成交：買進1張，100元，
* 第2筆成交：買進1張，102元，
* 第3筆成交：賣出1張，101元

在第1筆成交時，Filled = 1，FilledAvgPrice = 100。

在第2筆成交時，Filled = 2，FilledAvgPrice = (100 + 102) / 2 = 101。

在第3筆成交時，Filled = 1，依照先進先出的沖銷順序， 第3筆賣出1張的成交會沖銷掉第1筆買進1張成交，所以未平倉數量剩下第2筆的買進1張, FilledAvgPrice = 102。

請注意：上面這個計算方式跟把所有成本加總後來除是不一樣的((100 + 102 - 101) / 1 = 101)：全部加總後的平均數值會涵蓋已實現損益。

**範例#2**

* 第1筆成交：買進2張，100元，
* 第2筆成交：買進2張，101元，
* 第3筆成交：買進2張，102元，
* 第4筆成交：賣出3張，101元

在第1筆成交時，Filled = 2，FilledAvgPrice = 100。

在第2筆成交時，Filled = 4，FilledAvgPrice = (100 \* 2 + 101 \* 2) / 4 = 100.5。

在第3筆成交時，Filled = 6，FilledAvgPrice = (100 \* 2 + 101 \* 2 + 102 \* 2) / 6 = 101。

在第4筆成交時，Filled = 3，依照先進先出的沖銷原則，第4筆的賣出3張分別沖掉了第1筆的買進2張，以及第2筆的買進1張，所以剩餘的買進成交是第2筆買進1張，101元，以及第3筆的買進2張，102元，FilledAvgPrice = (101\*1 + 102 \* 2) / 3 = 101.66666。

如果策略在設定時指定要採用[交易帳號的庫存部位](https://www.xq.com.tw/lesson/xsat/xsat_overview/#stock)的話，此時系統就會依照策略的設定修改未平倉成本，之後收到成交之後再依照先進先出的方式調整數值。

**範例#3**

* 策略設定時指定策略部位：部位=2張，成本=100元，
* 第1筆成交：買進1張，102元，
* 第2筆成交：賣出1張，101元，

在策略指定策略部位時，Filled = 2，FilledAvgPrice = 100。

第1筆成交時，Filled = 3，FilledAvgPrice = (100 \* 2 + 102 \* 1) / 3 = 100.66666。

第2筆成交時，Filled = 2，依照先進先出的沖銷方式，這一筆賣出1張會沖銷掉策略指定部位其中1張，所以剩餘的數量是策略指定部位的1張，100元，跟第1筆成交1張的102元，FilledAvgPrice = (100 \* 1 + 102 \* 1) / 2 = 101。

請注意：未平倉成本不包含交易成本，所以使用者如果要估算未平倉損益時，可以在腳本內利用[Ｆ[FilledRecordCount](api?a=FilledRecordCount&b=bif)等函數取得每一筆成交紀錄，然後再自行計算。

---
## FilledEntryDate
**Path:** 內建函數 > 交易函數

FilledEntryDate -  (內建函數)
交易函數

語法：

Value1 = FilledEntryDate;  
取得部位建立日期。  
日期格式是一個8碼的數字，例如如果是2015年6月1日，則回傳20150601。

說明：

此函數會依照策略所保有的交易資料，採用先進先出的沖銷方式來算出的目前未平倉部位中第一筆建立的日期。

---
## FilledEntryTime
**Path:** 內建函數 > 交易函數

FilledEntryTime -  (內建函數)
交易函數

語法：

Value1 = FilledEntryTime ;  
取得部位建立時間。  
時間格式是一個6碼的數字，例如如果是上午的11點30分00秒，則回傳113000。

說明：

此函數會依照策略所保有的交易資料，採用先進先出的沖銷方式來算出的目前未平倉部位中第一筆建立的時間。

---
## FilledEntryTimeMS
**Path:** 內建函數 > 交易函數

FilledEntryTimeMS -  (內建函數)
交易函數

語法：

Value1 = FilledEntryTimeMS;  
取得部位建立時間(精準到毫秒)。  
時間格式是一個9碼的數字，例如如果是上午的11點30分00秒500毫秒，則回傳113000.500。

說明：

此函數會依照策略所保有的交易資料，採用先進先出的沖銷方式來算出的目前未平倉部位中第一筆建立的時間。

---
## FilledRecordBS
**Path:** 內建函數 > 交易函數

FilledRecordBS -  (內建函數)
交易函數

語法：

取得某一筆成交紀錄的成交方向，如果是買進的話回傳1，如果是賣出的話則回傳-1  
Value1 = FilledRecordBS(idx)

說明：

FilledRecordBS必須傳入一個index參數，代表要取得第幾筆成交紀錄的日期，index的範圍從1開始(第一筆)，不能超過FilledRecordCount。

如果這一筆成交紀錄是買進的話，則回傳1，如果是賣出的話則回傳-1。

---
## FilledRecordCount
**Path:** 內建函數 > 交易函數

FilledRecordCount -  (內建函數)
交易函數

語法：

回傳策略執行到目前為止的成交紀錄筆數  
Value1 = FilledRecordCount

說明：

腳本執行過程內，系統會紀錄每一筆成交紀錄，依照成交時間排序，FilledRecordCount則代表這些成交紀錄的筆數。透過FilledRecordCount，以及其他FilledRecord開頭的函數，包含[FilledRecordDate](api?a=FilledRecordDate&b=bif)，[FilledRecordTime](api?a=FilledRecordTime&b=bif)，[FilledRecordPrice](api?a=FilledRecordPrice&b=bif)，[FilledRecordBS](api?a=FilledRecordBS&b=bif)，[FilledRecordQty](api?a=FilledRecordQty&b=bif)等，來取得完整的成交紀錄資訊。

**範例**

```
var: idx(0);
for idx = 1 to FilledRecordCount begin
    value1 = FilledRecordDate(idx);   { 成交日期 }
    value2 = FilledRecordTime(idx);   { 成交時間 }
    value3 = FilledRecordPrice(idx);   { 成交價格 }
    value4 = FilledRecordQty(idx);   { 成交數量 }
    value5 = FilledRecordBS(idx);   { 成交方向: 買進或是賣出 }
    value6 = FilledRecordIsRealtime(idx); { 是否是在即時區間成交 }
end;
```

---
## FilledRecordDate
**Path:** 內建函數 > 交易函數

FilledRecordDate -  (內建函數)
交易函數

語法：

取得某一筆成交紀錄的成交日期  
Value1 = FilledRecordDate(idx)

說明：

FilledRecordDate必須傳入一個index參數，代表要取得第幾筆成交紀錄的日期，index的範圍從1開始(第一筆)，不能超過FilledRecordCount。

回傳一個8位的數字，例如20200110，代表2020年1月10日。

---
## FilledRecordIsRealtime
**Path:** 內建函數 > 交易函數

FilledRecordIsRealtime -  (內建函數)
交易函數

語法：

判斷某一筆成交紀錄是發生在策略部位計算區間或是即時K棒洗價區間  
Value1 = FilledRecordIsRealtime(idx)

說明：

FilledRecordIsRealtime必須傳入一個index參數，代表要取得第幾筆成交紀錄的日期，index的範圍從1開始(第一筆)，不能超過FilledRecordCount。

如果策略啟動了[策略部位計算功能](https://www.xq.com.tw/lesson/xsat/xsat_overview/#fun)的話，在執行過程內系統會使用歷史資料來模擬策略的交易狀態。

如果要判斷某一筆成交紀錄是發生在策略部位計算區間或是發生在即時K棒區間的話，可以透過FilledRecordIsRealtime這個函數。發生在即時K棒區間的成交紀錄會回傳1，發生在策略部位計算區間的則回傳0。

如果策略是用回測方式執行的話，則回傳0 (相當於策略部位計算區間)。

---
## FilledRecordPrice
**Path:** 內建函數 > 交易函數

FilledRecordPrice -  (內建函數)
交易函數

語法：

取得某一筆成交紀錄的成交價格  
Value1 = FilledRecordPrice(idx)

說明：

FilledRecordPrice必須傳入一個index參數，代表要取得第幾筆成交紀錄的時間，index的範圍從1開始(第一筆)，不能超過FilledRecordCount。

不管這一筆成交是買進或是賣出，FilledRecordPrice回傳的數值都是大於0的數字。如果需要判斷是買進或是賣出，請使用[FilledRecordBS](api?a=FilledRecordBS&b=bif)函數。

---
## FilledRecordQty
**Path:** 內建函數 > 交易函數

FilledRecordQty -  (內建函數)
交易函數

語法：

取得某一筆成交紀錄的成交數量  
Value1 = FilledRecordQty(idx)

說明：

FilledRecordQty必須傳入一個index參數，代表要取得第幾筆成交紀錄的時間，index的範圍從1開始(第一筆)，不能超過FilledRecordCount。

不管這一筆成交是買進或是賣出，FilledRecordQty回傳的數值都是大於0的數字。如果需要判斷是買進或是賣出，請使用[FilledRecordBS](api?a=FilledRecordBS&b=bif)函數。

---
## FilledRecordTime
**Path:** 內建函數 > 交易函數

FilledRecordTime -  (內建函數)
交易函數

語法：

取得某一筆成交紀錄的成交時間，回傳時間格式HHMMSS  
Value1 = FilledRecordTime(idx)

說明：

FilledRecordTime必須傳入一個index參數，代表要取得第幾筆成交紀錄的時間，index的範圍從1開始(第一筆)，不能超過FilledRecordCount。

回傳一個最多6位的數字，例如103025，代表10點30分25秒。

---
## FilledRecordTimeMS
**Path:** 內建函數 > 交易函數

FilledRecordTimeMS -  (內建函數)
交易函數

語法：

取得某一筆成交紀錄的成交時間，回傳時間格式HHMMSS.fff  
Value1 = FilledRecordTimeMS(idx)

說明：

FilledRecordTimeMS必須傳入一個index參數，代表要取得第幾筆成交紀錄的時間，index的範圍從1開始(第一筆)，不能超過FilledRecordCount。

回傳一個最多9位的數字，例如103025.123，代表10點30分25秒123毫秒。

---
## IsListedSymbol
**Path:** 內建函數 > 交易函數

IsListedSymbol -  (內建函數)
交易函數

語法：

IsListedSymbol();

說明：

此函數會回傳布林值，判斷運算的商品為策略設定的執行商品，還是後來由於其他因素 (例如自動執行有部位的商品) 而加進來運算的商品。
若為策略設定的執行商品則回傳 True。

舉例來說，假設策略為延續前次執行並勾選自動執行有部位的商品，執行商品設定為指定選股法。
昨日選股法篩選出了2303.TW並買進了一張，今日策略啟動時，選股法篩選出了 2882.TW，因此策略在今日會執行 2303.TW 和 2882.TW。

IsListedSymbol 在 2303.TW時會回傳False，在 2882.TW 則是True。

---
## IsMarketPrice
**Path:** 內建函數 > 交易函數

IsMarketPrice -  (內建函數)
交易函數

語法：

判斷傳入的價格是否是市價  
Condition1 = IsMarketPrice(value1)

說明：

商品的五檔委買委賣價，或是成交明細資料(Tick資料)的買進價，賣出價有可能會是市價。如果腳本希望判斷這種情形時，可以使用IsMarketPrice這個函數。

```
//範例
{ 委買最高價格是市價，表示必須送出市價買進才有機會買到 }
if IsMarketPrice(q_BestBid1) then setposition(1);
```

![市價買](https://www.xq.com.tw/wp-content/uploads/2020/10/%E5%B8%82%E5%83%B9%E8%B2%B7.jpg)

```
//範例
{ 委賣最低價格是市價，表示必須送出市價賣出才有機會平倉 }
if IsMarketPrice(q_BestAsk1) then setposition(0);
```

![市價賣](https://www.xq.com.tw/wp-content/uploads/2020/10/%E5%B8%82%E5%83%B9%E8%B3%A3.jpg)

---
## Market
**Path:** 內建函數 > 交易函數

Market -  (內建函數)
交易函數

語法：

運用目前商品的市價送單  
setposition(1, Market)

說明：

Market函數用來取得目前商品的市價，並搭配SetPosition的語法送單。

---
## Position
**Path:** 內建函數 > 交易函數

Position -  (內建函數)
交易函數

語法：

取得目前商品的部位。  
Value1 = Position

說明：

Position代表的是自動交易策略內目前執行商品的預期部位。

Position是一個整數，可以大於0、等於0、也可以小於0。從自動交易腳本內，透過呼叫交易指令，例如[SetPosition](api?a=SetPosition&b=bif)、[Buy](api?a=Buy&b=bif)、[Sell](api?a=Sell&b=bif)，可以改變Position的數值，當數值變大時，代表要執行買進的動作，當數值變小時則代表要執行賣出的動作。

交易腳本在每一次執行(洗價)之前系統會決定Position的數值，接下來的執行過程內，不管有沒有呼叫交易指令，Position的數值都不會改變。等到執行完成之後，系統會決定要執行哪一個交易指令，之後Position的數值就會更動，同時系統也會決定如何送出委託來達到這個預期的部位。

Position跟Close、Value1等內建的變數一樣，都是一個數值序列。Position或是Position[0]代表的是目前這一根bar的數值。Position[1]則是上一根bar執行時的部位數值。要特別注意的是如果在一根bar內透過逐筆洗價執行了多次交易指令的話，Position的數值會在每一次洗價之後就會異動，也就是說Position是一個[intrabarpersist](api?a=intrabarpersist)的變數。

請參考SetPosition語法內關於[Position異動的時機點](api?a=SetPosition&b=bif#PositionChangeTimingAnchor)的相關說明。

---
## Sell
**Path:** 內建函數 > 交易函數

Sell -  (內建函數)
交易函數

語法：

把策略的多單部位變小，進行多單減碼的動作。  
Sell(多單減碼數量)  
Sell(多單減碼數量，委託價格)  
Sell(多單減碼數量，label:="指令標記")  
Sell(多單減碼數量，委託價格，label:="指令標記")

說明：

Sell函數的作用是把目前多單的部位變小，用來進行多單減碼的動作。請注意Sell減碼之後的最小部位是0，保證不會把目前的部位變成空單(Position < 0)。

Sell函數的第一個參數是減碼部位，必須是一個正整數。第二個參數是此次交易的委託價格，第二個參數如果不傳的話則會使用策略的預設賣出價格。

與SetPosition一樣，也可以透過label函數傳入指令標記。

**範例**

```
Sell(1);
Sell(1, Close);
Sell(1, MARKET);
Sell(1, label:="出場1張");
```

**注意事項**

Sell指令只有在目前Position > 0時才會有作用。如果Sell的減碼數量大於目前Position的話，則Position會改成0，也就是說Sell函數不會把目標部位變成空頭(Position < 0)。

Sell(0)是一個特殊用法，如果此時的部位大於0的話，Sell(0)的作用是把部位變成0，如果此時的部位小於0的話，則Sell(0)沒有任何作用。

以下是Sell(N)的執行邏輯：

```
if Position > 0 then
    if N = 0 then
        SetPosition(0)  { N=0是特殊用法，把多單部位全部平倉 }
    else
        SetPosition(maxlist(Position - N, 0)); { 從Position(正數)減少N張，最終數字不會小於0 }
```

---
## SetPosition
**Path:** 內建函數 > 交易函數

SetPosition -  (內建函數)
交易函數

語法：

調整交易策略至指定的部位。  
SetPosition(目標部位)  
SetPosition(目標部位，委託價格)  
SetPosition(目標部位，label:="指令標記")  
SetPosition(目標部位，委託價格，label:="指令標記")

說明：

SetPosition函數的第一個參數是目標部位(Position)，代表交易策略預期持有的部位。第二個參數是此次交易的委託價格，如果不傳的話則會使用策略的預設買進/賣出價格。

部位(Position)是XS自動交易語法內最重要的觀念，在自動交易策略執行過程內，每個商品會有一個Position的數值，這個數值可以是0(空手)、大於0(多單)、或是小於0(空單)。

當腳本希望作買進的動作時，腳本可以透過SetPosition(或是其他交易函數)，把目標部位變大，例如如果目前Position是0的話，腳本可以呼叫SetPosition(1)，表示希望買進1張。同樣的，如果腳本希望作賣出的動作時，腳本可以透過SetPosition(或是其他函數)，把目標部位變小。

當腳本呼叫SetPosition(或是其他交易函數)時，系統會比對目前的Position以及新的目標部位的差異，然後送出對應的委託單，如果這些委託單完全成交的話，商品的淨成交部位就會跟目標部位是一樣的。

**範例#1**

```
SetPosition(1);
```

把腳本的部位變成1，委託價格使用策略預設的買賣價格。如果原先的Position是0的話，這個指令會買進1張，如果原先的Position是2的話，這個指令會賣出1張。如果原先的Position是1的話，這個指令會檢視目前委託的執行情形，可能會送出改價的委託(如果原先委託尚未成交，且這一次的委託價格跟上一筆委託單的委託價格不一樣的話)，或是不做任何動作。詳細的執行邏輯請參考底下「[交易指令的執行方式](api?a=SetPosition&b=bif#TradeOrderAnchor)」的說明。

**範例#2**

```
SetPosition(1, Close);
```

把腳本的部位變成1，委託價格使用目前的收盤價。SetPosition的第二個參數是委託價格，可以傳入一個固定數值(例如100.0)，或是其他的數值運算(例如Close, Close+1.0, 等)。

**範例#3**

```
SetPosition(-1);
```

把腳本的部位變成-1，委託價格使用策略預設的買賣價格。如果原先的Position是0的話，這個指令會賣出1張，如果原先的Position是-2的話，這個指令會買進1張。

**範例#4**

```
SetPosition(-1, MARKET);
```

把腳本的部位變成-1，委託價格使用市價。系統會依照帳號類型來決定市價單該如何傳送，如果是證券帳號的話，會傳送市價委託，如果是期貨帳號的話，則會傳送範圍市價委託。

**範例#5**

```
SetPosition(Position+1, AddSpread(Close, 1));
```

把腳本的部位變成目前的Position再加1張，所以不管原先的Position是多少，這個指令都會買進1張。委託的價格則是目前收盤價再往上加一檔。

[Position](api?a=Position&b=bif)是一個內建的欄位，腳本可以透過這個欄位取得目前的部位數值。
[AddSpread](api?a=AddSpread&b=bif)函數可以用來計算加減檔數後的價格。

除了SetPosition語法可以改變腳本部位之外，系統同時還提供[Buy](api?a=Buy&b=bif)、[Sell](api?a=Sell&b=bif)、[Short](api?a=Short&b=bif)、[Cover](api?a=Cover&b=bif)這幾種語法，請參考相關說明。

**範例#6**

```
SetPosition(1, label:="我的標記");
```

可以透過label這個參數, 傳入這次SetPosition的標記名稱。請參考底下[交易指令的標記名稱](api?a=SetPosition&b=bif#TradeLabelAnchor)的說明。

## Position跟Filled的關係

[Position](api?a=Position&b=bif)欄位代表腳本的預期部位，而[Filled](api?a=Filled&b=bif)欄位則是到目前為止的成交部位。簡單的來說，當腳本呼叫SetPosition傳入預期部位之後，系統就會依照SetPosition的參數來傳送委託，當收到成交時就會更新Filled欄位，如果Position跟Filled兩個欄位的數值一模一樣時，代表的是目前腳本的部位狀態與使用者的預期是一致的。

關於Position跟Filled，還有兩個地方要務必注意：

第一：一個自動交易策略可能會執行多個商品，每個商品的Position/Filled都是互相獨立的。

第二：Position/Filled的數值是一個相對的數值，與使用者所指定的交易帳號內這個商品的庫存部位未必是一樣的。例如我的交易帳號內可能有0050的庫存2張，可是XS自動交易策略提供多種[設定策略部位的方式](http://www.xq.com.tw/lesson/xsat/xsat_overview/#stock)，可以把Position設定從0開始，或是從2開始，甚至也可以設定成從1開始。

假設策略的Position設定從0開始，接下來的SetPosition(1)，指的是買進1張，如果成交的話，Filled也會是1，這兩個數值與帳號的庫存數值，就可能是不一樣的。

## 委託價格參數

如果SetPosition函數不傳入委託價格的話，則傳送委託時會依照交易策略的預設委託價格設定決定要使用那個價格，請注意交易策略的預設委託價格可以依照買進/賣出分別指定。

![help_自動交易策略_05](https://www.xq.com.tw/wp-content/uploads/2020/10/help_%E8%87%AA%E5%8B%95%E4%BA%A4%E6%98%93%E7%AD%96%E7%95%A5_05-1.png)

如果想要指定委託價格的話，則可以在第二個參數內傳入一個固定數值，或是一個數值運算式，例如

* 100.0
* Close, High, Low
* GetField("漲停價", "D")
* GetField("跌停價", "D")

也可以傳入系統內定的市價參數(MARKET)，代表要用市價來交易。例如

* SetPosition(1, MARKET)

如果傳入MARKET的話，系統會依照帳號類型來決定市價單該如何傳送，如果是證券帳號的話，會傳送市價委託，如果是期貨帳號的話，則會傳送範圍市價委託。

系統也支援檔位計算的函數([AddSpread](api?a=AddSpread&b=bif))，例如：

* SetPosition(1, AddSpread(Close, 2))
* SetPosition(-1, AddSpread(Close, -2))

為了保證委託價格符合商品的交易限制，在執行時系統會做以下的價格處理：

* 以台積電(2330)為例，
* 首先，先依照商品的小數點位數做四捨五入，例如298.765會先被轉成298.77，
* 接著如果這個價格不符合商品的跳動點的話，則會依照買進或是賣出做不同方式的轉換。如果是買進的話，價格會被轉成往下第一個符合跳動點的價格，如果是賣出的話，則會被轉成往上第一個符合跳動點的價格：
  + 如果是買進，298.77會被轉成298.5 (每一跳動點0.5)
  + 如果是賣出的話，298.77會被轉成299.0
* 如果商品有漲跌停限制的話，則系統會保證價格不會超過漲跌停。

## 委託數量的處理

SetPosition的第一個參數是目標部位，系統預期腳本會傳入一個整數的數值(可以是0、大於0或是小於0)。

如果腳本傳入的目標部位不是整數的話，則一律捨去小數位數，例如如果呼叫SetPosition(1.5678)的話，系統會以SetPosition(1)的方式來處理。

由於不同的商品有每次交易最大數量的限制(例如台股每一筆委託單只能送出499張)，如果SetPosition傳入的目標部位與先前的部位的差異超過這個限制的話，為了安全起見，執行時系統會回傳錯誤(超過單筆委託限制)，同時停止這個商品的執行。

## 交易指令的標記名稱

SetPosition所需傳入的參數除了目標部位以及委託價格之外，還可以另外透過label參數，傳入一個字串（字串長度最多64個字)，代表這個交易指令的名稱，當這個交易指令成交時，通知的UI上可以看到這個名稱文字，方便使用者辨識這次的成交原因。

**範例**

```
if Position = 0 then begin
    condition1 = Average(Close, 5) cross over Average(Close, 10);
    condition2 = TrueAll(Close > Close[1], 5);
    if condition1 then SetPosition(1, label:="均線穿越");
    if condition2 then SetPosition(1, label:="連漲5筆");
end;
```

為了避免混淆，同一個腳本內的交易指令的標記名稱必須是唯一的。

![標記名稱_V2](https://www.xq.com.tw/wp-content/uploads/2020/11/20201225_%E6%A8%99%E8%A8%98%E5%90%8D%E7%A8%B1_V2.png)

## Position異動的時機點

我們看以下的腳本範例，這是一個很常見的進場+出場的交易情境：

```
var: entry_condition(false), exit_condition(false);


entry_condition = Average(Close, 5) cross over Average(Close, 20);
exit_condition = Average(Close, 5) cross below Average(Close, 20);


if Position = 0 and entry_condition then SetPosition(1);
if Position = 1 and exit_condition then SetPosition(0);
```

我們知道當腳本發現Position是0，而且entry\_condition是true的時候，腳本會呼叫SetPosition(1)。

那麼腳本呼叫完SetPosition(1)之後，在下一行執行時Position會馬上變成1嗎？

這個問題的答案是：不會。

在每一次系統執行腳本前，系統會決定當時的Position以及Filled的數值。
這兩個變數的數值在執行這一次腳本的過程內都不會改變。當這次腳本執行結束之後，系統知道這次執行時呼叫了SetPosition(1)，所以在執行腳本完成後，才會把Position的數值改成1，同時準備送出委託單。

如果接下來商品收到了價格異動，再次執行腳本的話，此時Position的數值就會是1，而Filled的數值則依照當時是否已經收到了成交來決定是0還是1。

**規則#1: 系統會在執行腳本(洗價)前決定Position跟Filled的數值。**

**規則#2: 這兩個數值在執行腳本(洗價)的過程之間都不會改變。不會因為腳本呼叫了SetPosition之後就馬上改變Position的數值，也不會因為在這一次執行腳本(洗價)期間收到了成交而馬上改變Filled的數值。**

## 交易指令的優先順序

如果一個腳本內有多個交易指令的話，那系統怎麼決定要執行哪些交易指令呢？我們看以下的範例：

```
if condition1 then SetPosition(1);
if condition2 then SetPosition(2);
if condition3 then SetPosition(3);
```

在上面這個範例內，有可能因為condition1，condition2，condition3的狀態而呼叫了不同的交易指令，甚至先呼叫了SetPosition(1)，然後又呼叫了SetPosition(3)。當遇到這種情形時，系統會如何決定要執行哪一個交易指令呢？

目前XS自動交易的執行方式是只執行第一個交易指令，忽略之後的交易指令。以上面腳本範例而言，如果condition1是false，condition2是true，condition3也是true的話，那麼當次洗價的第一個交易指令是SetPosition(2)，所以系統會執行這一個，至於之後呼叫的SetPosition(3)則予以忽略。

## 交易指令的執行方式

當系統收到了SetPosition指令時，系統會依照以下狀態來判斷如何執行這一筆交易指令：

* 目前的Position(執行這一個交易指令之前的Position數值)，
* 目前的成交部位(在腳本內稱之為[Filled](api?a=Filled&b=bif)，代表這個商品執行到目前為止的淨成交張數)，
* 是否先前的交易指令還有尚未完全成交的委託單

**範例#1: SetPosition(1)，買進1張後成交，SetPosition(0)，賣出1張**

我們先舉一個最簡單的例子，假設腳本呼叫了SetPosition(1)：

腳本在剛開始執行時，預設的Position是0，SetPosition這個指令傳入的目標部位是1，所以系統會送出一筆買進1張的委託單，委託價格則是策略預設的買進價格。

執行之後，Position會變成1。在這一筆委託單還沒有成交之前，Filled是0。等到這一筆委託單成交了，則Filled變成1。

接續先前的例子，如果接下來腳本呼叫了SetPosition(0)的話，因為目前的Position是1，新的目標部位是0，所以系統會送出一筆賣出1張的委託單，委託價格則是策略預設的賣出價格。

執行之後，Position會變成0。此時Filled的數值還是是1，等到這一筆賣出委託成交之後，Filled的數值就會變成0。

**範例#2: SetPosition(1)，買進1張後未成交，SetPosition(0)，執行刪單**

接下來我們考慮另外一種執行的情形，同樣的腳本先呼叫了SetPosition(1)，然後系統送出了一筆買進1張的委託單，Position變成1，Filled還是0(尚未成交)。

假如在這一筆委託單還沒有成交之前，腳本就呼叫了SetPosition(0)。此時會發生什麼情形呢？

因為這一筆委託單尚未成交，所以如果要讓Position變成是0的話，最直接的執行方式是在這一筆買進1張的委託尚未成交之前，就先刪除這一筆委託。如果可以順利刪除的話，就符合使用者的預期部位了！

**範例#3: SetPosition(1)，買進1張後成交，SetPosition(2)，再買進1張**

接下來我們來看部位加碼的執行方式。假設腳本先呼叫了SetPosition(1)，然後這一張成交了，此時Position是1，Filled也是1。

假如接下來腳本呼叫了SetPosition(2)的話，系統比對目前的Position是1，目標Position是2，所以送出了一筆買進1張的委託，送出後Position = 2，在還沒有成交前，Filled是1，等到成交之後，Filled就會變成2。

**範例#4: SetPosition(1)，買進1張後沒有成交，SetPosition(2)，刪除先前委託改成買進2張**

延續範例#3的情境，假設腳本先呼叫了SetPosition(1)，在這一筆買進1張的委託尚未成交之前，腳本又呼叫了SetPosition(2)，此時系統會怎麼執行呢？

一種可能的執行方式是保留原先的買進1張的委託，另外再送出一張買進1張的委託，這樣子當兩張都成交時就會符合使用者的預期。

可是這樣子的執行方式比較複雜，爾後假如這兩張委託都沒有成交之後，使用者又更改Position的話，執行的邏輯會越來越難掌握。

目前XS採用的方式是比較簡單的作法：當系統執行SetPosition(2)時，如果發現先前的委託單尚未完成成交的話，執行的邏輯是先刪除這一張委託，然後依照這一張委託的成交數量決定如何傳送下一筆委託：

* 假設刪除委託後沒有成交，此時使用者的實際成交部位是0，而腳本預期的Position是2，所以送出一筆買進2張的委託，
* 假設刪除委託後發現這一筆委託已經成交了，此時使用者的實際成交部位是1，所以送出一筆買進1張的委託，

綜合上述的邏輯可以歸納出XS執行交易指令的邏輯：

* 當收到交易指令時，會檢查目前是否有尚未完成成交的委託，如果沒有的話，則依照目前部位與新的部位的差異，送出一筆買進或是賣出的委託，
* 如果此時還有尚未完成成交的委託的話，則先刪除委託，確認成交的狀態，然後再依照目前成交部位與新的部位的差異，送出一筆買進或是賣出的委託，
* 至多只會保留一筆尚未完成成交的委託

**範例#5: SetPosition(1)，買進1張後沒有成交，再次收到SetPosition(1)，刪除先前委託，改送不同價格的委託**

我們接下來看另外一種情境，假設腳本先呼叫SetPosition(1, 100.0)，此時送出了一筆買進1張的委託，委託價格是100.0。

假設在這一筆委託還沒有成交前，腳本又呼叫了SetPosition(1, 99.0)，此時系統發現新的部位雖然跟原先部位一樣，可是因為還沒有成交，而且委託價格不一樣，所以此時系統會刪除原先的委託，然後改送一筆買進1張的委託，委託價格是99.0。

這樣子的執行邏輯可以應用在追價的情境，例如以下的腳本範例

```
if Position = 0 and entry_condition then begin
    value1 = CurrentBar;
    SetPosition(1);
end;


if Position = 1 and Filled = 0 and CurrentBar - value1 >= 2 then begin
    value1 = CurrentBar;
    SetPosition(1, AddSpread(Close, 5));
end;
```

在這個腳本內我們發現Position是1，Filled還是0，而且離上一次下單已經超過2根bar了，通常會發生這樣子的情形可能是商品的價格已經超過先前委託的價格。此時可以使用SetPosition傳入不同價格的方式來告訴系統我想要改用不同的委託價格。當執行到 SetPosition(1, AddSpread(Close, 5))時，系統就會刪除原先的委託，然後改用新的委託價格來送單。

## 策略初始部位

在先前的範例內，我們提到策略一開始執行時部位預設是從0開始。可是在以下的一些應用情境內，我們會希望策略一開始執行的部位不是0。例如：

* 我們使用的是一個波段的策略，部位持有的期間可能會超過1日。如果依照波段的計算方式的話，目前這個時間點可能是處在已經進場的階段，此時希望策略一開始的部位是1，而不是0，
* 我們希望使用這個策略來幫我的庫存進行平倉或是加碼的動作，所以我希望策略一開始的部位就是我的庫存張數

針對這些應用情境，XS自動交易提供了以下的設定方式：

### 使用歷史資料來模擬計算策略的部位

使用者可以在策略設定內啟動計算策略部位的功能，同時指定策略部位計算的區間(指定日期起點，或是天期)，啟動之後系統就會讀取商品的歷史資料，使用這些歷史資料來執行策略，模擬策略在這段時間的交易部位。

詳細說明請參考「自動交易策略設定」內的「[策略部位計算功能](http://www.xq.com.tw/lesson/xsat/xsat_overview/#fun)」

### 使用交易帳號的庫存來設定策略的部位

使用者可以在指定策略的交易帳號後，利用帳號的庫存資料來設定策略的部位。可以讓策略的部位與帳號的庫存一模一樣，也可以指定策略的部位是部分庫存。

詳細說明請參考「自動交易策略設定」內的「[交易帳號庫存部位整合](http://www.xq.com.tw/lesson/xsat/xsat_overview/#stock)」

以上是 SetPosition 的說明，大家看過後，若想在進一步了解自動交易功能，可以至以下的官網文章查看詳細的介紹，感謝大家的聆聽，預祝大家投資順心，謝謝：

* [自動交易的基本觀念](https://www.xq.com.tw/lesson/xsat/introduction/)
* [如何建立自動交易策略](https://www.xq.com.tw/lesson/xsat/bulid_own_xsat/)
* [自動交易策略參數總覽](https://www.xq.com.tw/lesson/xsat/xsat_overview/)
* [自動交易中心操作介紹](https://www.xq.com.tw/lesson/xsat/xsat_center/)

---
## Short
**Path:** 內建函數 > 交易函數

Short -  (內建函數)
交易函數

語法：

把策略的空單部位變大，進行空單加碼的動作  
Short(空單加碼數量)  
Short(空單加碼數量，委託價格)  
Short(空單加碼數量，label:="指令標記")  
Short(空單加碼數量，委託價格，label:="指令標記")

說明：

Short函數的作用是把目前空單的部位變大，用來進行空單加碼的動作。

Short函數的第一個參數是加碼部位，必須是一個正整數。第二個參數是此次交易的委託價格，第二個參數如果不傳的話則會使用策略的預設賣出價格。

與SetPosition一樣，也可以透過label函數傳入指令標記。

**範例**

```
Short(1);
Short(1, Close);
Short(1, MARKET);
Short(1, label:="放空1張");
```

注意事項

當目前的Position > 0時，執行Short指令時會先把多頭部位全部平倉(部位變成0)，之後再進行空單加碼的動作。

Short(0)是一個特殊用法，如果此時的部位大於0的話，Short(0)的作用是把部位變成0，如果此時的部位小於0的話，則Short(0)沒有任何作用。

以下是Short(N)的執行邏輯：

```
if Position > 0 then
    SetPosition(-1 * N)  { 從Position(正數)變成 -N }
else
    SetPosition(Position - N); { 從Position(0或是負數)變成 Position-N }
```

---
## InStr
**Path:** 內建函數 > 字串函數

InStr -  (內建函數)
字串函數

語法：

回傳某個字串是否是另一個字串的一部分。  
比對位置 = InStr(原始字串, 比對字串);  
比對位置 = InStr(字串1, 字串2, 比對開始位置);  
- 回傳比對字串位於原始字串的位置，如果不存在的話則回0。  
- 第三個參數可以指定比對的開始位置，預設是從第一個位置開始找起。

說明：

這個函數可以傳入參個參數：

* 第一個參數是原始字串。
* 第二個參數是欲比對的字串。
* 第三個參數可以指定比對開始的位置。

如果欲比對的字串是原始字串的一部份的話，則回傳這個字串位於原始字串的位置。反之則回傳0。

```
Value1 = InStr("abcdefg", "bc");  // Value1 = 2
Value2 = InStr("abcdefg", "xyz"); // Value2 = 0
Value3 = InStr("Hello Hello", "Hello", 6);  //Value3 = 7
```

在上述範例內，"bc"是"abcdefg"的一部份，所以Value1的值會是"bc"位於"abcdefg"內的位置，第2個字元。而"xzy"並不是"abcdefg"的一部份，所以Value2 = 0。
Value3 則是因為指定要從第6個位置開始找起，所以會找到第二個Hello，故回7。

---
## LeftStr
**Path:** 內建函數 > 字串函數

LeftStr -  (內建函數)
字串函數

語法：

回傳字串的左邊子字串  
子字串 = LeftStr(原始字串，字元長度)

說明：

這個函數傳入兩個參數：

* 第一個參數是原始字串
* 第二個參數是欲取出的字元個數

回傳值是從原始字串左邊開始長度為第二個參數的子字串。

```
Var: str1("");


str1 = LeftStr("abcdefg", 3);  // str1 = "abc"
```

在上面範例內，str1是"abcdefg"從左邊算起長度為3的子字串，"abc"。

請參考 [RightStr函數](api?a=rightstr&b=bif) 以及 [MidStr函數](api?a=midstr&b=bif)。

---
## LowerStr
**Path:** 內建函數 > 字串函數

LowerStr -  (內建函數)
字串函數

語法：

把字串的每個英文字元轉成小寫  
回傳字串 = LowerStr(原始字串)

說明：

範例如下:

```
Var: str1("");


str1 = LowerStr("ABCDEFG");  // str1 = "abcdefg"
```

請參考[UpperStr函數](api?a=upperstr&b=bif)

---
## MidStr
**Path:** 內建函數 > 字串函數

MidStr -  (內建函數)
字串函數

語法：

回傳字串內從指定位置開始的子字串。  
子字串 = MidStr(原始字串，指定位置，子字串長度)

說明：

這個函數傳入三個參數：

* 第一個參數是原始字串
* 第二個參數是欲取出的子字串的起始位置
* 第三個參數是欲取出的子字串的字元長度

範例:

```
Var: str1(""), str2("");


str1 = MidStr("abcdefg", 1, 3);  // str1 = "abc"
str2 = MidStr("abcdefg", 2, 3);  // str1 = "bcd"
```

在上面範例內，str1是"abcdefg"這個字串第一個位置開始長度為3的子字串, "abc"，而str2則是"abcdefg"這個字串第二個位置開始長度為3的子字串, "bcd"。

請參考 [LeftStr函數](api?a=leftstr&b=bif) 以及 [RightStr函數](api?a=rightstr&b=bif)。

---
## NumToStr
**Path:** 內建函數 > 字串函數

NumToStr -  (內建函數)
字串函數

語法：

回傳數值的字串形式，同時可以指定顯示的小數位數。  
回傳字串 = NumToStr(數值，小數位數)

說明：

NumToStr回傳的字串會依照指定的小數位數來處理，如果實際數值的小數位數**大於**指定的小數位數的話，則採用四捨五入的方式來計算，如果實際數值的小數位數**小於**指定的小數位數的話，則在小數位數後面補0。

舉例說明:

```
Var: Str1(""), Str2(""), Str3("");


Value1 = 144.5;


Str1 = NumToStr(Value1, 0);  // Str1 = "145"
Str2 = NumToStr(Value1, 1);  // Str2 = "144.5"
Str3 = NumToStr(Value2, 2);  // Str3 = "144.50"
```

在上例內，Str1的字串值是144.5四捨五入後換算的結果 "145"，而Str3的字串值則在小數位數1之後補0，以確保有兩位小數 "144.50"。

請參考[StrToNum函數](api?a=strtonum&b=bif)。

---
## RightStr
**Path:** 內建函數 > 字串函數

RightStr -  (內建函數)
字串函數

語法：

回傳字串的右邊子字串  
子字串 = RightStr(原始字串，字元長度)

說明：

這個函數傳入兩個參數：

* 第一個參數是原始字串
* 第二個參數是欲取出的字元個數

回傳值是從原始字串右邊開始長度為第二個參數的子字串。

```
Var: str1("");


str1 = RightStr("abcdefg", 3);  // str1 = "efg"
```

在上面範例內，str1是"abcdefg"從右邊算起長度為3的子字串，"efg"。

請參考 [LeftStr函數](api?a=leftstr&b=bif) 以及 [MidStr函數](api?a=midstr&b=bif)。

---
## StrCompare
**Path:** 內建函數 > 字串函數

StrCompare -  (內建函數)
字串函數

語法：

比較字串是否相同  
數值 = StrCompare(字串1, 字串2)  
數值 = StrCompare(字串1, 字串2, 不區分大小寫)  
傳入二個以上參數:  
- 第一個參數是字串1。  
- 第二個參數是字串2。  
- 第三個參數是選用參數，True表示不區分大小寫，False代表區分大小寫，預設為不區分大小寫。

說明：

比較二個字串是否相同
當回傳值為 0 時，表示字串1和字串2一樣。
當回傳值為 1 時，表示字串1順序大於字串2。
當回傳值為 -1 時，表示字串1順序小於字串2。

```
//預設為不區分大小寫，所以下列二種得到的結果是一樣的
if StrCompare(symbol,"2330.tw") = 0 then plot1(1) else plot1(0);
if StrCompare(symbol,"2330.TW") = 0 then plot2(1) else plot2(0);
//假設主商品是台積電，當區分為大小寫時，plot3會是0、plot4會是1
if StrCompare(symbol,"2330.tw",false) = 0 then plot3(1) else plot3(0);
if StrCompare(symbol,"2330.TW",false) = 0 then plot4(1) else plot4(0);
```

---
## StrEndWith
**Path:** 內建函數 > 字串函數

StrEndWith -  (內建函數)
字串函數

語法：

condition1 = StrEndWith(字串1, 字串2);  
condition1 = StrEndWith(字串1, 字串2, 比對方式);  
  
- 傳入兩個字串，判斷第一個字串的結尾是否與第二個字串的完整內容相同。  
- 回傳布林值。  
- 預設的比對方式是不分大小寫，可以傳入第三個參數指定比對的方式，True表示不區分大小寫。

說明：

此函數可以用來比較傳入的第一個字串結尾字母是否和第二個字串相同。
預設的比對方是不區分字母大小寫，但可以透過傳入第三個參數來改變。

範例：

```
condition1 = StrEndWith(“ABCDEFG”, “DEFG”);
//回傳 True。


condition1 = StrEndWith(“ABCDEFG”, “ABC”);
//回傳 False。


condition1 = StrEndWith(“ABCDEFG”, “defg”)
//回傳 True。(預設是不區分大小寫)


condition1 = StrEndWith(“ABCDEFG”, “defg”, false);
//回傳 False。
```

---
## StrLen
**Path:** 內建函數 > 字串函數

StrLen -  (內建函數)
字串函數

語法：

回傳字串的長度  
字串長度 = StrLen(字串)

說明：

範例如下:

```
Value1 = StrLen("abcdefg");  // Value1 = 7
```

---
## StrSplit
**Path:** 內建函數 > 字串函數

StrSplit -  (內建函數)
字串函數

語法：

value1 = StrSplit(字串, 分隔字元, 輸出陣列);  
  
- 把一個字串依照指定的分隔字串切割成多個子字串。  
- 需傳入三個參數，第一個參數是要切割的字串，第二個參數是分隔字串，第三個參數是一個一維的字串陣列。  
- 切割後的子字串會依序放入輸出陣列內，如果陣列的大小已經固定，則至多只會放入這麼多個子字串，如果陣列是動態陣列，則當陣列空間不夠時，系統會自動調整陣列的大小，以便放入所以切割出來的子字串。  
- 回傳值為切割得到的子字串的個數。

說明：

此函數可以把第一個字串參數用第二個字串參數切割後放入的第三個參數陣列中。

範例：

```
Array: tokens[](""), tokens2[3]("");


value1 = StrSplit("A,B,C,D,E", ",", tokens);  
value2 = StrSplit("A,B,C;D,E", ",", tokens2);
```

value1 會是5。
tokens因為是動態陣列，所以會被自動調整成5個元素的大小。
tokens[1] = "A", tokens[2] = "B", tokens[3]="C", tokens[4]="D", tokens[5]="E"。

因為tokens2的大小已經固定，所以value2會是3。
tokens2[1] = "A", tokens2[2] = "B", tokens2[3] = "C;D"。

---
## StrStartWith
**Path:** 內建函數 > 字串函數

StrStartWith -  (內建函數)
字串函數

語法：

condition1 = StrStartWith(字串1, 字串2);  
condition1 = StrStartWith(字串1, 字串2, 比對方式);  
  
- 傳入兩個字串, 判斷第一個字串的開頭是否與第二個字串的完整內容相同。  
- 回傳布林值。  
- 預設的比對方式是不分大小寫，可以傳入第三個參數指定比對的方式，True表示不區分大小寫。

說明：

此函數可以用來比較傳入的第一個字串開頭字母是否和第二個字串相同。
預設的比對方是不區分字母大小寫，但可以透過傳入第三個參數來改變。

範例：

```
condition1 = StrStartWith(“ABCDEFG”, “ABC”);
//回傳 True。


condition1 = StrStartWith(“ABCDEFG”, “DEF”);
//回傳 False。


condition1 = StrStartWith(“ABCDEFG”, “abc”);
//回傳 True。(預設是不區分大小寫)


condition1 = StrStartWith(“ABCDEFG”, “abc”, false);
//回傳 False。
```

---
## StrToNum
**Path:** 內建函數 > 字串函數

StrToNum -  (內建函數)
字串函數

語法：

將傳入的字串轉成數值後回傳  
回傳數值 = StrToNum(字串)

說明：

範例：

```
Value1 = StrToNum("123.45");  // Value1 = 123.45
```

請參考[NumToStr函數](api?a=numtostr&b=bif)。

---
## StrTrim
**Path:** 內建函數 > 字串函數

StrTrim -  (內建函數)
字串函數

語法：

str1 = StrTrim(字串);  
str1 = StrTrim(字串, 選項);  
  
- 傳入字串後，去除這個字串開頭以及結尾的空白後回傳字串。  
- 預設是刪除空白。  
- 可以傳入第二個參數選項，指定刪除的範圍。  
- 選項是一個數字，0 = 頭尾都刪除(如同預設)， 1 = 只去除開頭的空白，2 = 只去除結尾的空白。

說明：

此函數可以用來刪除字串中的開頭和結尾的空白字元，預設是將開頭與結尾的空白都刪除，但可以透過傳入參數的方式來指定只刪除開頭或結尾。

範例：

```
str1 = StrTrim("  hello world "); 
//回傳的字串會是"hello world"。


str1 = StrTrim("  hello world ", 0); 
//回傳的字串會是 "hello world"。


str1 = StrTrim("  hello world ", 1); 
//回傳的字會是 "hello world "。


str1 = StrTrim("  hello world ", 2);
//回傳的字串會是"  hello world"。
```

---
## Text
**Path:** 內建函數 > 字串函數

Text -  (內建函數)
字串函數

語法：

傳入多個參數，回傳由這些參數連結而成的一個字串  
回傳字串 = Text(參數1, 參數2, 參數3, ...)

說明：

Text函數可以傳入多個參數，使用逗號分隔。函數執行完成後會把這些參數一一轉成對應的字串後再將這些字串連結成一個大字串後回傳。

舉例:

```
Variables: str("");


str = Text("Close=", 10);
```

上述範例執行之後 str變數的值會變成 "Close=10"。

這個函數可以搭配[Print函數](api?a=print&b=bif)來控制印出來的結果。

---
## UpperStr
**Path:** 內建函數 > 字串函數

UpperStr -  (內建函數)
字串函數

語法：

把字串的每個英文字元轉成大寫  
回傳字串 = UpperStr(原始字串)

說明：

範例如下:

```
Var: str1("");


str1 = UpperStr("abcdefg");  // str1 = "ABCDEFG"
```

請參考[LowerStr函數](api?a=lowerstr&b=bif)

---
## AbsValue
**Path:** 內建函數 > 數學函數

AbsValue -  (內建函數)
數學函數

語法：

取絕對值。傳回無正負號的數值  
回傳數值 = AbsValue(數值)

說明：

AbsValue函數用來計算傳入數值的**絕對值**。舉例而言：

```
Value1 = Abs(3);
Value2 = Abs(-3);
```

在上面範例內, Value1跟Value2的數值都是3。

以下使用AbsValue來計算兩條均線的差異，由於腳本只關心差異的大小，所以使用AbsValue函數來取得絕對值，不用考慮正負號。

```
Value1 = Average(Close, 5);
Value2 = Average(Close, 10);
Value3 = AbsValue(Value1 - Value2);
If Value3 <= 0.01 * Close Then Ret = 1;
```

---
## ArcCosine
**Path:** 內建函數 > 數學函數

ArcCosine -  (內建函數)
數學函數

語法：

計算反餘弦函數，請傳入絕對值小於 1 的數字。  
角度 = ArcCosine(數值)

說明：

ArcCosine函數用來計算三角函數的[反餘絃函數](https://zh.wikipedia.org/wiki/%E5%8F%8D%E9%A4%98%E5%BC%A6)。

輸入數值後，算出對應的角度。

範例:

```
Value1 = ArcCosine(0.5);  // Value1 = 60
```

---
## ArcSine
**Path:** 內建函數 > 數學函數

ArcSine -  (內建函數)
數學函數

語法：

計算反正弦函數，請傳入絕對值小於 1 的數字。  
角度 = ArcSine(數值)

說明：

計算三角函數的[反正絃函數](https://zh.wikipedia.org/wiki/%E5%8F%8D%E6%AD%A3%E5%BC%A6)。
輸入數值後，算出對應的角度。

範例:

```
Value1 = ArcSine(0.5);  // Value1 = 30
```

---
## ArcTangent
**Path:** 內建函數 > 數學函數

ArcTangent -  (內建函數)
數學函數

語法：

計算反正切函數  
角度 = ArcTangent(數值)

說明：

計算三角函數的[反正切函數](https://zh.wikipedia.org/wiki/%E5%8F%8D%E6%AD%A3%E5%88%87)。

輸入數值後，算出對應的角度。

範例:

```
Value1 = ArcTangent(1);  // Value1 = 45
```

---
## AvgList
**Path:** 內建函數 > 數學函數

AvgList -  (內建函數)
數學函數

語法：

傳入數個數值，回傳這些數值的的平均值。  
回傳平均值 = AvgList(數值1, 數值2, 數值3, ..)

說明：

使用AvgList時可以傳入多個數值，數值之間使用逗號分隔，例如：

```
Value1 = AvgList(Open, High, Low);
```

上述範例內使用AvgList來計算Typical Price (*(開盤價 + 最高價 + 最低價) / 3*)。

請注意: 如果要計算序列型的數值的平均值的話，則可以使用[Average函數](api?a=average&b=sys)。

---
## Ceiling
**Path:** 內建函數 > 數學函數

Ceiling -  (內建函數)
數學函數

語法：

把小數無條件進位後轉成整數  
回傳數值 = Ceiling(數值)

說明：

回傳小數點無條件進位後的整數。

範例:

```
Value1 = Ceiling(10.0);  // Value1 = 10.0
Value2 = Ceiling(10.1);  // Value2 = 11.0
```

---
## CoTangent
**Path:** 內建函數 > 數學函數

CoTangent -  (內建函數)
數學函數

語法：

計算餘切函數。  
回傳數值 = CoTangent(角度)

說明：

計算三角函數的[餘切函數](https://zh.wikipedia.org/wiki/%E9%A4%98%E5%88%87)。

輸入角度後回傳餘切值。

範例:

```
Value1 = CoTangent(45);  // Value1 = 1.0
```

---
## Combination
**Path:** 內建函數 > 數學函數

Combination -  (內建函數)
數學函數

語法：

計算從集合個數M內取出N個元素的可能組合數目  
回傳數值 = Combination(集合個數M, 欲取出的個數N)

說明：

計算從N個不同數字的集合內取出M個不同數字的[可能組合個數](https://zh.wikipedia.org/wiki/%E7%BB%84%E5%90%88%E6%95%B0%E5%AD%A6)。

範例

```
Value1 = Combination(3, 2); // Value1 = 3
```

假設母集合有三個數字 A, B, C, 則取出任意兩個數字的可能組合數 = (A,B), (B,C), (A,C) 共三種。

請參考 [Permutation函數](api?a=permutation&b=bif)

---
## Cos
**Path:** 內建函數 > 數學函數

Cos -  (內建函數)
數學函數

語法：

計算角度的餘弦值  
餘弦值 = Cos(角度)

說明：

計算三角函數的[餘絃函數](https://zh.wikipedia.org/wiki/%E9%A4%98%E5%BC%A6)。

輸入角度後回傳餘弦值。

範例:

```
Value1 = Cos(60);  // Value1 = 0.5
```

---
## Cosine
**Path:** 內建函數 > 數學函數

Cosine -  (內建函數)
數學函數

語法：

計算角度的餘弦值  
數值 = Cosine(角度)

說明：

計算三角函數的餘絃函數。與[Cos函數](api?a=cos&b=bif)相同。

---
## ExpValue
**Path:** 內建函數 > 數學函數

ExpValue -  (內建函數)
數學函數

語法：

計算自然對數次方運算後的數值  
回傳數值 = ExpValue(數值)

說明：

計算[自然對數次方](https://zh.wikipedia.org/wiki/%E6%8C%87%E6%95%B0%E5%87%BD%E6%95%B0)運算後的數值。

回傳結果為 **e**(自然對數, 約等於2.718281828)的N次方。

```
Value1 = ExpValue(1);  // Value1 = 2.718281828
```

請參考[Log函數](api?a=log&b=bif)。

---
## Factorial
**Path:** 內建函數 > 數學函數

Factorial -  (內建函數)
數學函數

語法：

計算數字的階乘結果  
回傳數值 = Factorial(數字)

說明：

[階乘函數](https://zh.wikipedia.org/wiki/%E9%9A%8E%E4%B9%98)就是由1開始遞增連乘到該整數。例如3的階乘數(3!) = 1 \* 2 \* 3 = 6。

範例:

```
Value1 = Factorial(3); // Value1 = 6
```

---
## Floor
**Path:** 內建函數 > 數學函數

Floor -  (內建函數)
數學函數

語法：

把小數無條件捨去後轉成整數  
回傳數值 = Floor(數值)

說明：

回傳數值的整數部分，小數點後的數字無條件捨去。

範例:

```
Value1 = Floor(10.5); // Value1 = 10
```

請參考 [Ceiling函數](api?a=ceiling&b=bif) 以及 [Round函數](api?a=round&b=bif)

---
## FracPortion
**Path:** 內建函數 > 數學函數

FracPortion -  (內建函數)
數學函數

語法：

計算數值的小數部分  
回傳數值 = FracPortion(數值)

說明：

範例:

```
Value1 = IntPortion(10.5);   // Value1 = 10
Value2 = FracPortion(10.5);  // Value2 = 0.5
```

請參考 [IntPortion函數](api?a=intportion&b=bif)。

---
## IntPortion
**Path:** 內建函數 > 數學函數

IntPortion -  (內建函數)
數學函數

語法：

計算傳入數值的整數部分的數值  
回傳數值 = IntPortion(數值)

說明：

範例:

```
Value1 = IntPortion(10.5);   // Value1 = 10
Value2 = FracPortion(10.5);  // Value2 = 0.5
```

請參考 [FracPortion函數](api?a=fracportion&b=bif)。

---
## Log
**Path:** 內建函數 > 數學函數

Log -  (內建函數)
數學函數

語法：

取以e為底的對數值，請傳入大於0的數字。  
回傳數值 = Log(數值)

說明：

回傳以**e**(自然對數)為底的[對數值](https://zh.wikipedia.org/wiki/%E5%AF%B9%E6%95%B0)。

```
Value1 = ExpValue(1);
Value2 = Log(Value1);  // 約等於1
```

請參考[ExpValue函數](api?a=expvalue&b=bif)。

---
## MaxList
**Path:** 內建函數 > 數學函數

MaxList -  (內建函數)
數學函數

語法：

計算多個數值內的最大值  
回傳數值 = MaxList(數值1, 數值2, 數值3, ..)

說明：

MaxList可以傳入多個數值，數值之間使用逗號分開。

以下是範例:

```
Value1 = Average(Close, 5);
Value2 = Average(Close, 10);
Value3 = Average(Close, 20);
If Open < MinList(Value1, Value2, Value3) And
   Close > MaxList(Value1, Value2, Value3) 
Then Ret = 1;
```

在這個腳本內使用MaxList來算出5日/10日/20日均線的最大值。當開盤價低於均線且收盤價站上均線時觸發訊號。

腳本內同時使用到[MinList函數](api?a=minlist&b=bif)，這個函數的用法類似MaxList，傳入多個數值後回傳這些數值的最小值。

---
## MaxList2
**Path:** 內建函數 > 數學函數

MaxList2 -  (內建函數)
數學函數

語法：

計算多個數值內的最2大值  
回傳數值 = MaxList2(數值1, 數值2, 數值3, ..)

說明：

MaxList2可以傳入多個數值，數值之間使用逗號分開。

範例:

```
Value1 = MaxList2(1, 2, 3, 4, 5);  // Value1 = 4;
```

---
## MinList
**Path:** 內建函數 > 數學函數

MinList -  (內建函數)
數學函數

語法：

計算多個數值內的最小值  
回傳數值 = MinList(數值1, 數值2, 數值3, ..)

說明：

MinList可以傳入多個數值，數值之間使用逗號分開。

以下是一個腳本範例:

```
Value1 = Average(Close, 5);
Value2 = Average(Close, 10);
Value3 = Average(Close, 20);
If Open < MinList(Value1, Value2, Value3) And
   Close > MaxList(Value1, Value2, Value3) 
Then Ret = 1;
```

在這個腳本內使用MinList來算出5日/10日/20日均線的最小值。當開盤價低於均線且收盤價站上均線時觸發訊號。

腳本內同時使用到[MaxList函數](api?a=maxlist&b=bif)，這個函數的用法類似MinList，傳入多個數值後回傳這些數值的最大值。

---
## MinList2
**Path:** 內建函數 > 數學函數

MinList2 -  (內建函數)
數學函數

語法：

計算多個數值內的最2小值  
回傳數值 = MinList2(數值1, 數值2, 數值3, ..)

說明：

MinList2可以傳入多個數值，數值之間使用逗號分開。

範例:

```
Value1 = MinList2(1, 2, 3, 4, 5);  // Value1 = 2;
```

---
## Mod
**Path:** 內建函數 > 數學函數

Mod -  (內建函數)
數學函數

語法：

計算傳入的兩個數值相除後的餘數  
餘數 = Mod(被除數，除數)

說明：

範例:

```
Value1 = Mod(10, 2); // Value1 = 0 (可以整除)
Value2 = Mod(10, 3); // Value2 = 1(不能整除，除完後餘1)
```

---
## Neg
**Path:** 內建函數 > 數學函數

Neg -  (內建函數)
數學函數

語法：

將輸入的數值轉成負的絕對值  
回傳數值 = Neg(數值)

說明：

範例

```
Value1 = Neg(5);  // Value1 = -5
Value2 = Neg(-5);  // Value2 = -5
```

請參考[Pos函數](api?a=pos&b=bif)。

---
## NthMaxList
**Path:** 內建函數 > 數學函數

NthMaxList -  (內建函數)
數學函數

語法：

傳入多個數值，回傳這些數值內由大到小排名第幾的數字。  
回傳數值 = NthMaxList(排名位置, 數值1, 數值2, 數值3, ..)

說明：

**排名位置**從1開始，1表示是回傳排名第一(最大)的數字，2表示回傳排名第二(次大)的數字，以下類推。在**排名位置**之後可以傳入任意個數值，使用逗號分開。

舉例:

```
Value1 = NthMaxList(1, 50, 50, 40, 30);  // Value1 = 50
Value2 = NthMaxList(2, 50, 50, 40, 30);  // Value2 = 50
Value3 = NthMaxList(3, 50, 50, 40, 30);  // Value3 = 40
Value4 = NthMaxList(4, 50, 50, 40, 30);  // Value4 = 30
```

上述計算 50, 50, 40, 30 這四個數字由大到小的排名數字。請注意傳入的數值內有兩個50，分居排名1跟2。

另外一個範例:

```
Value1 = NthMaxList(1, Close, Close[1], Close[2], Close[3], Close[4]);
Value2 = NthMaxList(5, Close, Close[1], Close[2], Close[3], Close[4]);
```

使用NthMaxList取得近5日的最高收盤價以及最低收盤。

當排名位置為1時，NthMaxList函數等同於[MaxList函數](api?a=maxlist&b=bif)。當排名位置為最後一名時，NthMaxList函數等同於[MinList函數](api?a=minlist&b=bif)。

---
## NthMinList
**Path:** 內建函數 > 數學函數

NthMinList -  (內建函數)
數學函數

語法：

傳入多個數值，回傳這些數值內由小到大排名第幾的數字。  
回傳數值 = NthMinList(排名位置, 數值1, 數值2, 數值3, ..)

說明：

**排名位置**從1開始，1表示是回傳排名第一(最小)的數字，2表示回傳排名第二(次小)的數字，以下類推。在**排名位置**之後可以傳入任意個數值，使用逗號分開。

舉例:

```
Value1 = NthMinList(1, 50, 50, 40, 30);  // Value1 = 30
Value2 = NthMinList(2, 50, 50, 40, 30);  // Value2 = 40
Value3 = NthMinList(3, 50, 50, 40, 30);  // Value3 = 50
Value4 = NthMinList(4, 50, 50, 40, 30);  // Value4 = 50
```

上述計算 50, 50, 40, 30 這四個數字由小到大的排名數字。請注意傳入的數值內有兩個50，分居排名3跟4。

請參考 [NthMaxList函數](api?a=nthmaxlist&b=bif)。

---
## Permutation
**Path:** 內建函數 > 數學函數

Permutation -  (內建函數)
數學函數

語法：

計算從集合個數M內取出N個元素的可能排列個數  
回傳數值 = Permutation(集合個數M, 欲取出的個數N)

說明：

計算從N個不同數字的集合內取出M個不同數字的[可能排列個數](https://zh.wikipedia.org/wiki/%E7%BB%84%E5%90%88%E6%95%B0%E5%AD%A6)。

範例:

```
Value1 = Permutation(3, 2);  // Value1 = 6
```

假設母集合有三個數字 A, B, C, 則取出任意兩個不同數字的可能排列方式 = (A,B), (A,C), (B,A), (B,C), (C,A), (C,B) 共六種。

請參考 [Combination函數](api?a=combination&b=bif)

---
## Pos
**Path:** 內建函數 > 數學函數

Pos -  (內建函數)
數學函數

語法：

將輸入的數值轉為正數。  
回傳數值 = Pos(數值)

說明：

這個函數的結果與[AbsValue函數](api?a=absvalue&b=bif)相同。

```
Value1 = Pos(-10);  // Value1 = 10
Value2 = Pos(10);  // Value2 = 10
```

---
## Power
**Path:** 內建函數 > 數學函數

Power -  (內建函數)
數學函數

語法：

計算數字的乘冪數值  
回傳數值 = Power(底數, 指數)

說明：

範例:

```
Value1 = Power(10, 2);  // Value1 = 10的2次方 = 100
```

---
## Random
**Path:** 內建函數 > 數學函數

Random -  (內建函數)
數學函數

語法：

回傳一個介於0跟傳入數值之間的隨機亂數  
隨機亂數 = Random(最大亂數的範圍)

說明：

範例:

```
Value1 = Random(10);
```

Value1的數值會是一個介於0跟10之間的隨機數字 (0 <= Value1 And Value1 < 10)，而且每次執行時Value1的數值都會不相同。

一般而言會在計算統計相關數字時使用隨機數字來模擬可能的數值分配情境。

---
## Round
**Path:** 內建函數 > 數學函數

Round -  (內建函數)
數學函數

語法：

回傳數值四捨五入後的結果，可以指定小數位數。  
回傳數值 = Round(數值，小數位數)

說明：

範例:

```
Value1 = Round(10.547, 0); // Value1 = 11
Value2 = Round(10.547, 1); // Value1 = 10.5
Value3 = Round(10.547, 2); // Value1 = 10.55
```

請參考 [Ceiling函數](api?a=ceiling&b=bif) 以及 [Floor函數](api?a=floor&b=bif)

---
## Sign
**Path:** 內建函數 > 數學函數

Sign -  (內建函數)
數學函數

語法：

回傳數值的正負號  
回傳數值 = Sign(數值)  
如果是正數，回傳1，如果是負數，回傳-1，如果是0，則回傳0

說明：

範例:

```
Value1 = Sign(10);  // Value1 = 1
Value2 = Sign(-10); // Value2 = -1
Value3 = Sign(0);   // Value3 = 0
```

---
## Sin
**Path:** 內建函數 > 數學函數

Sin -  (內建函數)
數學函數

語法：

計算角度的正弦值  
正弦值 = Sin(角度)

說明：

計算三角函數的[正絃函數](https://zh.wikipedia.org/wiki/%E6%AD%A3%E5%BC%A6)。

輸入角度後回傳正弦值。

範例:

```
Value1 = Sin(30);  // Value1 = 0.5
```

---
## Sine
**Path:** 內建函數 > 數學函數

Sine -  (內建函數)
數學函數

語法：

計算角度的正弦值  
正弦值 = Sine(角度)

說明：

計算三角函數的正絃函數，與[Sin函數](api?a=sin&b=bif)相同。

---
## Square
**Path:** 內建函數 > 數學函數

Square -  (內建函數)
數學函數

語法：

計算傳入數值的平方  
回傳數值 = Square(數值)

說明：

範例:

```
Value1 = Square(10);  // Value1 = 100
```

請參考 [SquareRoot函數](api?a=squareroot&b=bif)。

---
## SquareRoot
**Path:** 內建函數 > 數學函數

SquareRoot -  (內建函數)
數學函數

語法：

計算傳入數值的平方根，請傳入大於0的數字。  
回傳數值 = SquareRoot(數值)

說明：

範例:

```
Value1 = SquareRoot(100);  // Value1 = 10
```

請參考 [Square函數](api?a=square&b=bif)。

---
## SumList
**Path:** 內建函數 > 數學函數

SumList -  (內建函數)
數學函數

語法：

傳入數個數值，回傳這些數值的的加總  
回傳加總值 = SumList(數值1, 數值2, 數值3, ..)

說明：

使用SumList時可以傳入數個數值，數值之間用逗號隔開。

範例:

```
Value1 = SumList(Open, High, Low, Close) / 4;
```

上述範例內使用SumList來計算平均價格 (*(開盤價 + 最高價 + 最低價 + 收盤價) / 4*)。

請注意: 如果要計算序列型的數值的加總值的話，則可以使用[Summation函數](api?a=summation&b=sys)。

---
## Tan
**Path:** 內建函數 > 數學函數

Tan -  (內建函數)
數學函數

語法：

計算角度的正切值  
正切值 = Tag(角度)

說明：

計算三角函數的[正切函數](https://zh.wikipedia.org/wiki/%E6%AD%A3%E5%88%87)。

輸入角度，回傳對應的正切值。

範例:

```
Value1 = Tan(45);  // Value1 = 1.0
```

---
## Tangent
**Path:** 內建函數 > 數學函數

Tangent -  (內建函數)
數學函數

語法：

計算角度的正切值  
正切值 = Tangent(角度)

說明：

計算三角函數的正切函數。與[Tan函數](api?a=tan&b=bif)相同。

---
## CurrentDate
**Path:** 內建函數 > 日期函數

CurrentDate -  (內建函數)
日期函數

語法：

回傳腳本的執行日期（歷史區間為當時；即時區間為當日）  
回傳日期 = CurrentDate  
日期格式是一個8碼的數字, 例如如果是2015年6月1日, 則回傳20150601

說明：

**歷史區間為當時**的意思是，例如回溯、回測，都是腳本執行在歷史的時間範圍中，這時的CurrentDate就會是歷史當天，而非今天。

**即時區間為當日**的意思是，腳本執行在當下，因此CurrentDate就會是今天。

---

回傳的日期格式是一個8碼的數字 **YYYYMMDD**:

* YYYY: 執行的年度，例如2015，4碼，
* MM: 執行的月份，數值範圍從01到12，2碼，
* DD: 執行的日期，數值範圍從01到31，2碼

舉例而言，如果執行日期是2015年6月1日，則CurrentDate回傳 20150601。

```
Print("CurrentDate=", CurrentDate);
```

---
## DateAdd
**Path:** 內建函數 > 日期函數

DateAdd -  (內建函數)
日期函數

語法：

用來計算某個日期以前或是以後的日期。  
回傳日期 = DateAdd(日期數值，要運算的日期欄位單位，要增加的數值)  
第一個參數是開始計算的日期，格式為YYYYMMDD的8碼數字  
第二個參數是要運算的日期欄位，如果是年則傳"Y"，如果是月則傳"M"，如果是日則傳"D"  
第三個參數是要增加的數值，可以是正數或是負數  
回傳計算後的日期，格式也是YYYYMMDD的8碼數字

說明：

日期數值是一個8碼的數字，格式為YYYYMMDD:

* YYYY是西元年份，例如2015，
* MM是月份，兩位數字，範圍從01到12，
* DD是日期，兩位數字，範圍從01到31，

第二個參數是要執行運算的日期欄位單位，如果我們要計算第一個日期的後幾年的是哪一天的話，則傳入"Y"，如果我們要計算第一個日期的後幾月是哪一天的話，則傳入"M"，如果我們要計算第一個日期的後幾日的是哪一天的話，則傳入"D"。

第三個參數則是要*增加*的數值，可以是正數(表示往後加), 也可以是負數(表示往前減)。

DateAdd函數回傳的數值也是YYYYMMDD的日期格式。

範例:

```
Value1 = DateAdd(20150601, "Y", 1);  // Value1 = 20160601, 加1年
Value2 = DateAdd(20150601, "M", 1);  // Value2 = 20150701, 加1個月
Value3 = DateAdd(20150601, "D", 1);  // Value3 = 20150602, 加1天
Value4 = DateAdd(20150601, "D", -1); // Value4 = 20150531, 減1天
```

---
## DateDiff
**Path:** 內建函數 > 日期函數

DateDiff -  (內建函數)
日期函數

語法：

計算兩個日期數值的差異天數(第一個日期減第二個日期)  
請傳入日期格式YYYYMMDD  
差異天數 = DateDiff(第一個日期數值，第二個日期數值)

說明：

日期數值為YYYYMMDD的8碼數字，例如CurrentDate，或是Date，或是20150601，或是其他日期相關函數所回傳的日期欄位。

DateDiff回傳的數值是第一個日期減第二個日期的差異天數，如果第一個日期小於第二個日期的話，則回傳的數值是負數。

```
Value1 = DateDiff(20160601, 20160501);  // Value1 = 31(日)
Value2 = DateDiff(20160601, 20160602);  // Value2 = -1(日)
```

一般可以利用這個函數來判斷價位日期之間的關係

```
if High > Highest(Close[1],60) and DateDiff(CurrentDate, Date) < 5 then ret=1;
```

以上的警示範例(使用日資料執行)會在近5日內創60日新高時觸發。注意到腳本內使用DateDiff來判斷創新高的日期(Date)是否與目前電腦日期(也就是執行當日)的差異是在5日之內。

---
## DateToJulian
**Path:** 內建函數 > 日期函數

DateToJulian -  (內建函數)
日期函數

語法：

將YYYYMMDD的8碼日期轉成儒略日(Julian)格式  
儒略日 = DateToJulian(日期數值)

說明：

在XS系統內日期的標準格式為YYYYMMDD的8碼數字。如果需要執行日期的計算時，一般可以使用[DateAdd函數](api?a=dateadd&b=bif)或是[DateDiff函數](api?a=datediff&b=bif)。

另外一種計算方式，則是把日期轉換成[儒略日格式](https://zh.wikipedia.org/wiki/%E5%84%92%E7%95%A5%E6%97%A5)後再來計算。因為儒略日格式採用**絕對天數**的方式來紀錄日期數值，所以可以直接做數值運算，然後再使用[JulianToDate函數](api?a=JulianToDate&b=bif)轉成YYYYMMDD的8碼日期格式。

```
Value1 = DateToJulian(20150601);  // 把20150601轉成Julian格式
Value1 = Value1 + 1;                        // 直接加1天
Value2 = JulianToDate(Value1);       // Value2 = 20150602
```

---
## DateToString
**Path:** 內建函數 > 日期函數

DateToString -  (內建函數)
日期函數

語法：

把傳入的日期轉成字串  
日期字串 = DateToString(日期數值)  
回傳的日期字串格式為YYYY/MM/DD

說明：

日期數值是一個8碼的數字，格式為YYYYMMDD:

* YYYY是西元年份，例如2015，
* MM是月份，兩位數字，範圍從01到12，
* DD是日期，兩位數字，範圍從01到31，

日期數值通常是透過CurrentDate，或是Date(資料的日期欄位)，或是其他日期相關函數所產生的日期數值。

回傳字串的格式是"**YYYY/MM/DD**"，其中YYYY為４位年份，MM為月份，從01到12，DD則是日期，從01到31。

舉例而言，如果目前日期是20150601的話，以下的程式碼

```
Print(DateToString(CurrentDate));
```

將會印出 "2015/06/01"的字串。

請參考[StringToDate函數](api?a=stringtodate&b=bif)。

---
## DateValue
**Path:** 內建函數 > 日期函數

DateValue -  (內建函數)
日期函數

語法：

用來取得輸入日期數值的各種不同欄位  
欄位數值 = DateValue(日期數值，指定欄位)  
指定欄位有 Y(回傳年度), M(回傳月份), D(回傳月份的第幾天), DW(回傳星期幾), WM(回傳月份的第幾週), WY(回傳當年的第幾週)

說明：

日期數值是一個8碼的數字，格式為YYYYMMDD:

* YYYY是西元年份，例如2015，
* MM是月份，兩位數字，範圍從01到12，
* DD是日期，兩位數字，範圍從01到31，

日期數值通常是透過CurrentDate，或是Date(資料的日期欄位)，或是其他日期相關函數所產生的日期數值。

回傳數值依照指定欄位而有不同:

* 如果是Y，則回傳日期的西元年度
* 如果是M，則回傳日期的月份，範圍從1到12
* 如果是D，則回傳這個月的第幾天，範圍從1到31
* 如果是DW，則回傳星期幾，星期天為0, 星期一為1, 星期二為2, 由此類推
* 如果是WM，則回傳這個月份的第幾週，範圍從1到6
* 如果是WY，則回傳當年的第幾週，範圍從1到53

以下是一個範例:

```
Value1 = DateValue(20150601, "Y");  // Value1 = 2015
Value2 = DateValue(20150601, "M");  // Value2 = 6
Value3 = DateValue(20150601, "D");  // Value3 = 1
Value4 = DateValue(20150601, "DW"); // Value4 = 1 (星期一)
Value5 = DateValue(20150601, "WM"); // Value5 = 1 (6月第一週)
Value6 = DateValue(20150601, "WY"); // Value6 = 23 (2015年第23週)
```

這個函數可以看成是[Year函數](api?a=year&b=bif)，[Month函數](api?a=month&b=bif)，[DayOfMonth函數](api?a=dayofmonth&b=bif)，[DayOfWeek函數](api?a=dayofweek&b=bif)，[WeekOfMonth函數](api?a=weekofmonth&b=bif)，以及[WeekOfYear函數](api?a=weekofyear&b=bif)等函數的綜合體。

---
## DayOfMonth
**Path:** 內建函數 > 日期函數

DayOfMonth -  (內建函數)
日期函數

語法：

計算傳入日期的是這個月的第幾天  
天數 = DayOfMonth(日期)  
回傳數值的範圍從1到31

說明：

日期數值是一個8碼的數字，格式為YYYYMMDD:

* YYYY是西元年份，例如2015，
* MM是月份，兩位數字，範圍從01到12，
* DD是日期，兩位數字，範圍從01到31，

日期數值通常是透過CurrentDate，或是Date(資料的日期欄位)，或是其他日期相關函數所產生的日期數值。

回傳的數值則是這個日期是這個月的第幾天，可能的數值從1到31。

舉例：

```
Value1 = DayOfMonth(20150601);	// Value1 = 1
Value2 = DayOfMonth(20150630);  // Value2 = 30
```

日期相關的函數請參考[Year函數](api?a=year&b=bif)，[Month函數](api?a=month&b=bif)，[DayOfMonth函數](api?a=dayofmonth&b=bif)，[DayOfWeek函數](api?a=dayofweek&b=bif)，[WeekOfMonth函數](api?a=weekofmonth&b=bif)，以及[WeekOfYear函數](api?a=weekofyear&b=bif)。

---
## DayOfWeek
**Path:** 內建函數 > 日期函數

DayOfWeek -  (內建函數)
日期函數

語法：

計算傳入日期的是這個星期的第幾天  
第幾天 = DayOfWeek(日期)  
如果是星期日的話則回傳0, 星期一的話則回傳1, 由此類推

說明：

日期數值是一個8碼的數字，格式為YYYYMMDD:

* YYYY是西元年份，例如2015，
* MM是月份，兩位數字，範圍從01到12，
* DD是日期，兩位數字，範圍從01到31，

日期數值通常是透過CurrentDate，或是Date(資料的日期欄位)，或是其他日期相關函數所產生的日期數值。

回傳的數值則是這個日期是這個星期的第幾天，可能的數值從0(星期日)到6(星期六)。

範例:

```
If DayOfWeek(Date) = 1 Then
Begin
	// 目前K棒資料日期是星期一
End;
```

日期相關的函數請參考[Year函數](api?a=year&b=bif)，[Month函數](api?a=month&b=bif)，[DayOfMonth函數](api?a=dayofmonth&b=bif)，[DayOfWeek函數](api?a=dayofweek&b=bif)，[WeekOfMonth函數](api?a=weekofmonth&b=bif)，以及[WeekOfYear函數](api?a=weekofyear&b=bif)。

---
## EncodeDate
**Path:** 內建函數 > 日期函數

EncodeDate -  (內建函數)
日期函數

語法：

傳入年份，月份，日期，回傳對應的日期數值  
日期數值 = EncodeDate(年份, 月份, 日期)

說明：

傳入三個參數:

* 第一個參數是年份(YYYY)，4碼數字，例如 2015，
* 第二個參數是月份(MM)，數值範圍為1到12，
* 第三個參數是日期(DD)，數值範圍為1到31

函數回傳值的格式為8碼的日期數字。

以下範例:

```
Value1 = EncodeDate(2015,1,1);   // Value1 = 20150101
Value2 = EncodeDate(2015,12,31); // Value2 = 20151231
```

---
## FormatDate
**Path:** 內建函數 > 日期函數

FormatDate -  (內建函數)
日期函數

語法：

把傳入的日期數值依照指定的格式組合轉成字串  
字串 = FormatDate(格式字串組合, 日期數值)

說明：

日期數值是一個8碼的數字，格式為YYYYMMDD:

* YYYY是西元年份，例如2015，
* MM是月份，兩位數字，範圍從01到12，
* DD是日期，兩位數字，範圍從01到31，

日期數值通常是透過CurrentDate，或是Date(資料的日期欄位)，或是其他日期相關函數所產生的日期數值。

格式字串可以是以下字串的組合:

* **d**: 回傳這個日期是這個月的第幾天，如果不到兩位數，前面不補零，例如5號的話回傳 "5"，15號的話回傳 "15"，
* **dd**: 回傳這個日期是這個月的第幾天，如果不到兩位數，前面補零，例如5號的話回傳 "05"，15號的話回傳 "15"，
* **ddd**: 回傳這個日期是星期幾的英文縮寫，分別是"Mon", "Tue", "Wed", "Thu", "Fri", "Sat", "Sun"
* **dddd**: 回傳這個日期是星期幾的英文全名，分別是"Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday", "Sunday"
* **M**: 回傳這個日期的月份，如果不到兩位數，前面不補零，例如1月回傳 "1"，12月回傳 "12"，
* **MM**: 回傳這個日期的月份，如果不到兩位數，前面補零，例如1月回傳 "01"，12月回傳 "12"，
* **MMM**: 回傳這個日期的月份英文簡稱，分別是 "Jan", "Feb", "Mar", "Apr", "May", "Jun", "Jul", "Aug", "Sep", "Oct", "Nov", "Dec"，
* **MMMM**: 回傳這個日期的月份英文全名，分別是 "January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"
* **y**: 回傳這個日期的年份後兩碼，如果不到兩位數前面不補零，例如2005年的話則回傳"5", 2015年的話則回傳"15"，
* **yy**: 回傳這個日期的年份後兩碼，如果不到兩位數前面補零，例如2005年的話則回傳"05", 2015年的話則回傳"15"，
* **yyy**: 回傳這個日期的年份後兩碼，如果不到兩位數前面補零，例如2005年的話則回傳"05", 2015年的話則回傳"15"，
* **yyyy**: 回傳這個日期的4碼年份，例如2005年的話則回傳 "2005"，

格式字串內除了可以使用上面字串的組合之外，也可以搭配其他的字元。

範例如下:

```
Value1 = FormatDate("yyyy/MM/dd", 20150601);  // Value1 = "2015/06/01"
```

注意到格式字串內有使用"/"字串，這些額外的字串也會出現在回傳字串內。

---
## JulianToDate
**Path:** 內建函數 > 日期函數

JulianToDate -  (內建函數)
日期函數

語法：

將儒略日(Julian)格式的日期轉成YYYYMMDD的8碼日期格式  
日期 = JulianToDate(儒略日數值)

說明：

把[儒略日格式](http://zh.wikipedia.org/wiki/%E5%84%92%E7%95%A5%E6%97%A5)的日期轉換成YYYYMMDD的8碼日期格式。

請參考[DateToJulian函數](api?a=DateToJulian&b=bif)的說明。

---
## Month
**Path:** 內建函數 > 日期函數

Month -  (內建函數)
日期函數

語法：

計算傳入日期的月份  
月份 = Month(日期)  
回傳月份的範圍從1到12

說明：

日期數值是一個8碼的數字，格式為YYYYMMDD:

* YYYY是西元年份，例如2015，
* MM是月份，兩位數字，範圍從01到12，
* DD是日期，兩位數字，範圍從01到31，

日期數值通常是透過CurrentDate，或是Date(資料的日期欄位)，或是其他日期相關函數所產生的日期數值。
回傳的數值則是這個日期的月份，可能的數值從1到12。

舉例：

```
Value1 = Month(20150601);　　// Value1 = 6
```

日期相關的函數請參考[Year函數](api?a=year&b=bif)，[Month函數](api?a=month&b=bif)，[DayOfMonth函數](api?a=dayofmonth&b=bif)，[DayOfWeek函數](api?a=dayofweek&b=bif)，[WeekOfMonth函數](api?a=weekofmonth&b=bif)，以及[WeekOfYear函數](api?a=weekofyear&b=bif)。

---
## StringToDate
**Path:** 內建函數 > 日期函數

StringToDate -  (內建函數)
日期函數

語法：

把傳入的字串轉成日期  
日期數值 = StringToDate( 日期字串)  
日期字串的格式為"YYYY/MM/DD"，而回傳的日期格式為YYYYMMDD的8碼數字

說明：

範例:

```
Value1 = StringToDate("2015/06/01");  // Value1 = 20150601
```

請參考[DateToString函數](api?a=datetostring&b=bif)。

---
## WeekOfMonth
**Path:** 內建函數 > 日期函數

WeekOfMonth -  (內建函數)
日期函數

語法：

計算傳入日期的是這個月的第幾個星期  
第幾個星期 = WeekOfMonth(日期)  
回傳數值的範圍從1到6

說明：

日期數值是一個8碼的數字，格式為YYYYMMDD:

* YYYY是西元年份，例如2015，
* MM是月份，兩位數字，範圍從01到12，
* DD是日期，兩位數字，範圍從01到31，

日期數值通常是透過CurrentDate，或是Date(資料的日期欄位)，或是其他日期相關函數所產生的日期數值。

回傳的數值則是這個日期是這個月的第幾個星期，可能的數值從1到6。

```
Value1 = WeekOfMonth(20150601);   // Value1 = 1 (第一週)
```

日期相關的函數請參考[Year函數](api?a=year&b=bif)，[Month函數](api?a=month&b=bif)，[DayOfMonth函數](api?a=dayofmonth&b=bif)，[DayOfWeek函數](api?a=dayofweek&b=bif)，[WeekOfMonth函數](api?a=weekofmonth&b=bif)，以及[WeekOfYear函數](api?a=weekofyear&b=bif)。

---
## WeekOfYear
**Path:** 內建函數 > 日期函數

WeekOfYear -  (內建函數)
日期函數

語法：

計算傳入日期的是這一年的第幾個星期  
第幾個星期 = WeekOfYear(日期)  
回傳數值的範圍從1到53

說明：

日期數值是一個8碼的數字，格式為YYYYMMDD:

* YYYY是西元年份，例如2015，
* MM是月份，兩位數字，範圍從01到12，
* DD是日期，兩位數字，範圍從01到31，

日期數值通常是透過CurrentDate，或是Date(資料的日期欄位)，或是其他日期相關函數所產生的日期數值。

回傳的數值則是這一年第幾個星期，可能的數值從1到53。

```
Value1 = WeekOfYear(20150101);   // Value1 = 1 (第一週)
```

日期相關的函數請參考[Year函數](api?a=year&b=bif)，[Month函數](api?a=month&b=bif)，[DayOfMonth函數](api?a=dayofmonth&b=bif)，[DayOfWeek函數](api?a=dayofweek&b=bif)，[WeekOfMonth函數](api?a=weekofmonth&b=bif)，以及[WeekOfYear函數](api?a=weekofyear&b=bif)。

---
## Year
**Path:** 內建函數 > 日期函數

Year -  (內建函數)
日期函數

語法：

計算傳入日期的年度  
年度 = Year(日期)

說明：

日期數值是一個8碼的數字，格式為YYYYMMDD:

* YYYY是西元年份，例如2015，
* MM是月份，兩位數字，範圍從01到12，
* DD是日期，兩位數字，範圍從01到31，

日期數值通常是透過CurrentDate，或是Date(資料的日期欄位)，或是其他日期相關函數所產生的日期數值。

回傳的數值則是這個日期的所在年度。

舉例：

```
Value1 = Year(20150601);　　// Value1 = 2015
```

日期相關的函數請參考[Year函數](api?a=year&b=bif)，[Month函數](api?a=month&b=bif)，[DayOfMonth函數](api?a=dayofmonth&b=bif)，[DayOfWeek函數](api?a=dayofweek&b=bif)，[WeekOfMonth函數](api?a=weekofmonth&b=bif)，以及[WeekOfYear函數](api?a=weekofyear&b=bif)。

---
## CurrentTime
**Path:** 內建函數 > 時間函數

CurrentTime -  (內建函數)
時間函數

語法：

回傳腳本的執行時間（歷史區間為當時；即時區間為當下）  
回傳時間 = CurrentTime  
時間格式是一個6碼的數字, 例如如果是上午的11點30分00秒, 則回傳113000  
注意：不支援選股（執行選股時，數值為0）

說明：

**歷史區間為當時**的意思是，例如回溯、回測，都是腳本執行在歷史的時間範圍中，這時的CurrentTime就會是歷史當時的時間，而非此時此刻。

**即時區間為當下**的意思是，腳本執行在當下，因此CurrentTime就會是此時此刻的時間。

---

時間的格式為5碼或6碼的數字 **HHMMSS**:

* HH: 執行的小時，數值範圍從0到23 (24小時制), 可能是1碼或2碼，
* MM: 執行的分鐘，數值範圍從00到59，兩碼，
* SS: 執行的秒數，數值範圍從00到59，兩碼

舉例而言，如果執行時間是上午的9點30分00秒，則CurrentTime回傳 93000，如果執行時間為下午1點30分00秒，則CurrentTime回傳133000。

我們可以使用這個函數來判斷腳本的執行時間，例如在以下範例內使用CurrentTime來判斷腳本執行時是否已經是中午12:30分之後。

```
If CurrentTime >= 123000 Then
Begin
	// 執行時間是中午12:30分以後
End;
```

---
## CurrentTimeMS
**Path:** 內建函數 > 時間函數

CurrentTimeMS -  (內建函數)
時間函數

語法：

回傳腳本的執行時間（歷史區間為當時；即時區間為當下）  
回傳時間 = CurrentTimeMS  
時間格式是一個9碼的數字, 例如如果是上午的11點30分00秒500毫秒, 則回傳113000.500  
注意：不支援選股（執行選股時，數值為0）

說明：

**歷史區間為當時**的意思是，例如回溯、回測，都是腳本執行在歷史的時間範圍中，這時的CurrentTimeMS就會是歷史當時的時間，而非此時此刻。

**即時區間為當下**的意思是，腳本執行在當下，因此CurrentTimeMS就會是此時此刻的時間。

---

時間的格式為8碼或9碼的數字 **HHMMSS.FFF**:

* HH: 執行的小時，數值範圍從0到23 (24小時制), 可能是1碼或2碼，
* MM: 執行的分鐘，數值範圍從00到59，兩碼，
* SS: 執行的秒數，數值範圍從00到59，兩碼，
* FFF：執行的毫秒，數值範圍從000到999，三碼。

舉例而言，如果執行時間是上午的9點30分00秒500毫秒，則CurrentTimeMS回傳 93000.500，如果執行時間為下午1點10分00秒500毫秒，則CurrentTimeMS回傳131000.500。

我們可以使用這個函數來判斷腳本的執行時間，例如在以下範例內使用CurrentTimeMS來判斷腳本執行時是否已經是中午12點30分00秒500毫秒之後。

```
If CurrentTimeMS >= 123000.500 Then
Begin
	// 執行時間是中午12點30分00秒500毫秒以後
End;
```

---
## EncodeTime
**Path:** 內建函數 > 時間函數

EncodeTime -  (內建函數)
時間函數

語法：

傳入小時，分鐘，秒數，回傳對應的時間數值  
時間數值 = EncodeTime(小時數值, 分鐘數值, 秒數數值)  
時間數值 = EncodeTime(小時數值, 分鐘數值, 秒數數值 ,毫秒數值)

說明：

傳入三個參數:

* 第一個參數是小時(HH)，數值範圍為0到23 (24小時制)
* 第二個參數是分鐘(MM)，數值範圍為0到59，
* 第三個參數是秒數(SS)，數值範圍為0到59
* 第四個參數是毫秒(MS)，數值範圍為0到999，如果省略這個參數的話，則回傳HHMMSS。

函數回傳值的格式為5至9碼的時間數字。

例如傳入HH=9, MM=30, SS=0的話，則回傳93000。如果傳入HH=12, MM=30, SS=0的話，則回傳123000。如果傳入HH=13, MM= 15, SS=0, MS=255 的話，則回傳 131500.255。

以下是應用範例:

```
Value1 = EncodeTime(12, 30, 0);
Value2 = EncodeTime(13, 15, 0, 255);


If CurrentTime >= Value1 Then Begin
	// 目前時間是中午12點30分以後
End;


if CurrentTimeMS >= Value2 Then Begin
	// 目前時間是中午13點15分00秒255毫秒 以後
End;
```

---
## FormatTime
**Path:** 內建函數 > 時間函數

FormatTime -  (內建函數)
時間函數

語法：

把傳入的時間數值依照指定的格式組合轉成字串  
字串 = FormatTime(格式字串組合, 時間數值)

說明：

時間數值是一個5~6碼的數字，格式是HHMMSS:

* HH是小時，數值範圍從0到23，可以是1位數字或是2位數字
* MM是分鐘，數值範圍從00到59，必須是2位數字
* SS是秒數，數值範圍從00到59，必須是2位數字

時間數值通常是透過CurrentTime，Time(資料的時間欄位)，或是其他時間相關函數所產生的時間數值。

CurrentTimeMS，則是含有毫秒的時間數值，是一個 8~9 碼的數字，格式是HHMMSS.fff:

* HH是小時，數值範圍從0到23，可以是1位數字或是2位數字
* MM是分鐘，數值範圍從00到59，必須是2位數字
* SS是秒數，數值範圍從00到59，必須是2位數字
* fff是毫秒，數值範圍從000到999，必須是3位數字

格式字串可以是以下字串的組合:

* **h**: 回傳12小時制的小時，如果不到兩位數，前面不補零，例如上午8點的話回傳 "8"，上午11點回傳 "11", 下午4點回傳 "4"，
* **hh**: 回傳12小時制的小時，如果不到兩位數，前面補零，例如上午8點的話回傳 "08"，上午11點回傳 "11", 下午4點回傳 "04"，
* **H**: 回傳24小時制的小時，如果不到兩位數，前面不補零，例如上午8點的話回傳 "8"，上午11點回傳 "11", 下午4點回傳 "16"，
* **HH**: 回傳24小時制的小時，如果不到兩位數，前面補零，例如上午8點的話回傳 "08"，上午11點回傳 "11", 下午4點回傳 "16"，
* **m**: 回傳分鐘，如果不到兩位數，前面不補零，例如上午8點5的話回傳 "5"，上午8點15分的話則回傳 "15"，
* **mm**: 回傳分鐘，如果不到兩位數，前面補零，例如上午8點5的話回傳 "05"，上午8點15分的話則回傳 "15"，
* **s**: 回傳秒數，如果不到兩位數，前面不補零，例如上午8點5分5秒的話回傳 "5"，上午8點5分15秒的話則回傳 "15"，
* **ss**: 回傳秒數，如果不到兩位數，前面補零，例如上午8點5分5秒的話回傳 "05"，上午8點5分15秒的話則回傳 "15"，
* **t**: 回傳上午或是下午的代碼，例如時間是上午的話則回傳"A", 如果時間是下午的話則回傳"P"，
* **tt**: 回傳上午或是下午的完整代碼，例如時間是上午的話則回傳"AM", 如果時間是下午的話則回傳"PM"，
* **fff**: 回傳毫秒完整代碼，如果不到三位數，前面補零，例如上午8點5分5秒5毫秒的話回傳 "005"，上午8點5分15秒555毫秒的話則回傳 "555"，

格式字串內除了可以使用上面字串的組合之外，也可以搭配其他的字元。

範例如下

```
var:_Str1(""),_Str2("");


_Str1 = FormatTime("HH:mm:ss", 132530.255);  // _Str1 = "13:25:30"
_Str2 = FormatTime("HH:mm:ss:fff", 132530.255);  // _Str2 = "13:25:30"
```

注意到格式字串內有使用":"字串，這些額外的字串也會出現在回傳字串內。

---
## Hour
**Path:** 內建函數 > 時間函數

Hour -  (內建函數)
時間函數

語法：

計算傳入時間的小時數值  
小時 = Hour(時間數值)  
回傳小時的數值範圍從0到23 (24小時制)

說明：

時間數值是一個5~6碼的數字，格式是HHMMSS.fff:

* HH是小時，數值範圍從0到23，可以是1位數字或是2位數字
* MM是分鐘，數值範圍從00到59，必須是2位數字
* SS是秒數，數值範圍從00到59，必須是2位數字

時間數值通常是透過CurrentTime，Time(資料的時間欄位)，或是其他時間相關函數所產生的時間數值。

```
Value1 = Hour(Time);


If Value1 = 12 Then
Begin
	// 目前分鐘K棒的資料時間是12點
End;
```

上述範例利用Hour取得目前分鐘K棒資料時間的小時數值。

```
Value2 = Hour(CurrentTime);
If Value2 >= 12 Then
Begin
	// 目前時間是中午12點過後
End;
```

上述範例則是傳入[CurrentTime](api?a=currentime&b=bif)，也就是目前電腦的時間，格視為 HHMMSS。

---

也可以傳入含有毫秒的時間數值 [CurrentTimeMS](api?a=currentimems&b=bif) 回傳格式為 HHMMSS.fff

* HH是小時，數值範圍從0到23，可以是1位數字或是2位數字
* MM是分鐘，數值範圍從00到59，必須是2位數字
* SS是秒數，數值範圍從00到59，必須是2位數字
* fff是毫秒，數值範圍從000到999，必須是3位數字

```
Value3 = Hour(CurrentTimeMS);
If Value3 >= 12 Then
Begin
	// 目前時間是中午12點過後
End;
```

上述範例是傳入[CurrentTimeMS](api?a=currentimems&b=bif)，也是目前電腦的時間，不過是含有毫秒的電腦的時間，格式為 HHMMSS.fff。

時間相關的的函數請參考[Hour函數](api?a=hour&b=bif)，[Minute函數](api?a=minute&b=bif)，以及[Second函數](api?a=second&b=bif)。

---
## MilliSecond
**Path:** 內建函數 > 時間函數

MilliSecond -  (內建函數)
時間函數

語法：

計算傳入時間的秒數  
毫秒數 = MilliSecond(時間數值)  
回傳秒數的數值範圍從0到999

說明：

時間數值是一個8~9碼的數字，格式是HHMMSS.fff:

* HH是小時，數值範圍從0到23，可以是1位數字或是2位數字
* MM是分鐘，數值範圍從00到59，必須是2位數字
* SS是秒數，數值範圍從00到59，必須是2位數字
* fff是毫秒，數值範圍從0到999，必須是3位數字

時間數值通常是透過CurrentTime，CurrentTimeMS，Time(資料的時間欄位)，或是其他時間相關函數所產生的時間數值。

```
Value1 = CurrentTimeMS;
If Hour(Value1) = 13 And Minute(Value1) = 05 And Second(Value1) >= 30 And MilliSecond(Value1) >= 500 Then
Begin
	// 目前時間 >= 13:05:30.500
End;
```

時間相關的的函數請參考[Hour函數](api?a=hour&b=bif)，[Minute函數](api?a=minute&b=bif)，[Second函數](api?a=second&b=bif)，以及[MilliSecond](api?a=millisecond&b=bif)

---
## Minute
**Path:** 內建函數 > 時間函數

Minute -  (內建函數)
時間函數

語法：

計算傳入時間的秒數  
分鐘數 = Minute(時間數值)  
回傳分鐘數的數值範圍從0到59

說明：

時間數值是一個5~6碼的數字，格式是HHMMSS.fff：

* HH是小時，數值範圍從0到23，可以是1位數字或是2位數字
* MM是分鐘，數值範圍從00到59，必須是2位數字
* SS是秒數，數值範圍從00到59，必須是2位數字

時間數值通常是透過CurrentTime，Time(資料的時間欄位)，或是其他時間相關函數所產生的時間數值。

```
Value1 = Minute(Time);
If Value1 = 0 Then
Begin
	// 目前分鐘K棒資料時間是0分
End;
```

上述範例取得目前分鐘K棒資料時間的分鐘數值。

---

時間數值也可以是一個8~9碼，含有毫秒的數字，格式是HHMMSS.fff：

* HH是小時，數值範圍從0到23，可以是1位數字或是2位數字
* MM是分鐘，數值範圍從00到59，必須是2位數字
* SS是秒數，數值範圍從00到59，必須是2位數字
* fff是毫秒，數值範圍從000到999，必須是3位數字

含有毫秒的時間數值，通常是透過CurrentTimeMS，[FilledRecordTimeMS](api?a=FilledRecordTimeMS&b=bif)，或是其他時間相關函數所產生的時間數值。

```
Value1 = Minute(CurrentTimeMS);
If Value1 = 0 Then
Begin
	// 目前分鐘K棒資料時間是0分
End;
```

上述範例，也是取得目前分鐘K棒資料時間的分鐘數值，不過是傳入含有毫秒的目前電腦時間。

時間相關的的函數請參考[Hour函數](api?a=hour&b=bif)，[Minute函數](api?a=minute&b=bif)，以及[Second函數](api?a=second&b=bif)。

---
## Second
**Path:** 內建函數 > 時間函數

Second -  (內建函數)
時間函數

語法：

計算傳入時間的秒數  
秒數 = Second(時間數值)  
回傳秒數的數值範圍從0到59

說明：

時間數值是一個5~6碼的數字，格式是HHMMSS:

* HH是小時，數值範圍從0到23，可以是1位數字或是2位數字
* MM是分鐘，數值範圍從00到59，必須是2位數字
* SS是秒數，數值範圍從00到59，必須是2位數字

時間數值通常是透過CurrentTime，Time(資料的時間欄位)，或是其他時間相關函數所產生的時間數值。

```
Value1 = CurrentTime;
If Hour(Value1) = 13 And Minute(Value1) = 05 And Second(Value1) >= 30 Then
Begin
	// 目前時間 >= 13:05:30
End;
```

---

時間數值也可以是一個含有毫秒的 8~9 碼的數字，格式是HHMMSS.fff：

* HH是小時，數值範圍從0到23，可以是1位數字或是2位數字
* MM是分鐘，數值範圍從00到59，必須是2位數字
* SS是秒數，數值範圍從00到59，必須是2位數字
* fff是毫秒，數值範圍從000到999，必須是3位數字

含有毫秒的時間數值，通常是透過CurrentTimeMS，[FilledRecordTimeMS](api?a=FilledRecordTimeMS&b=bif)，或是其他時間相關函數所產生的時間數值。

以下的範例是用 CurrentTimeMS 傳入的時間數值

```
Value2 = CurrentTimeMS;
If Hour(Value2) = 13 And Minute(Value2) = 05 And Second(Value2) >= 30 Then
Begin
	// 目前時間 >= 13:05:30
End;
```

時間相關的的函數請參考[Hour函數](api?a=hour&b=bif)，[Minute函數](api?a=minute&b=bif)，[Second函數](api?a=second&b=bif)，以及[MilliSecond](api?a=millisecond&b=bif)

---
## StringToTime
**Path:** 內建函數 > 時間函數

StringToTime -  (內建函數)
時間函數

語法：

把傳入的字串轉成時間  
時間數值 = StringToTime( 時間字串)  
時間字串的格式為"HH:MM:SS"，而回傳的時間格式為HHMMSS的6碼數字  
時間字串的格式也可以是"HH:MM:SS.fff"，回傳的時間格式則為HHMMSS.fff的9碼數字

說明：

範例:

```
if CurrentTime > StringToTime("13:00:00") Then Begin
	// 目前時間大於中午13:00:00
End;


if CurrentTimeMS > StringToTime("13:00:00.500") Then Begin
	// 目前時間大於中午13:00:00.500
End;
```

請參考[TimeToString函數](api?a=timetostring&b=bif)。

---
## TimeAdd
**Path:** 內建函數 > 時間函數

TimeAdd -  (內建函數)
時間函數

語法：

用來計算某個時間以前或是以後的時間。  
回傳時間 = TimeAdd(時間數值，要運算的時間欄位單位，要增加的數值)  
第一個參數是開始計算的時間，格式為HHMMSS，或者HHMMSS.fff的時間數字  
第二個參數是要運算的時間欄位，如果是小時則傳"H"，如果是分鐘則傳"M"，如果是秒的話則傳"S"，如果是毫秒的話則傳"MS"  
第三個參數是要增加的數值，可以是正數或是負數  
回傳計算後的時間，格式也是 HHMMSS 或 HHMMSS.fff 的時間數字

說明：

時間數值是一個5~6碼的數字，格式是HHMMSS：

* HH是小時，數值範圍從0到23，可以是1位數字或是2位數字
* MM是分鐘，數值範圍從00到59，必須是2位數字
* SS是秒數，數值範圍從00到59，必須是2位數字

通常是透過CurrentTime，Time(資料的時間欄位)，或是其他時間相關函數所產生的時間數值。

---

時間數值也可以是一個含有毫秒的 8~9 碼的數字，格式是HHMMSS.fff：

* HH是小時，數值範圍從0到23，可以是1位數字或是2位數字
* MM是分鐘，數值範圍從00到59，必須是2位數字
* SS是秒數，數值範圍從00到59，必須是2位數字
* fff是毫秒，數值範圍從000到999，必須是3位數字

通常是透過CurrentTimeMS，[FilledRecordTimeMS](api?a=FilledRecordTimeMS&b=bif)，或是其他含有毫秒的時間相關函數所產生的時間數值。

第二個參數是要執行運算的時間欄位單位，如果我們要計算第一個時間的後幾小時的話，則傳入"H"，如果我們要計算第一個時間的後幾分的話，則傳入"M"，如果我們要計算第一個時間的後幾秒的話，則傳入"S"，如果我們要計算第一個時間的後幾毫秒的話，則傳入"MS"。

第三個參數則是要*增加*的數值，可以是正數(表示往後加), 也可以是負數(表示往前減)。

TimeAdd回傳的數值也是HHMMSS或者HHMMSS.fff的時間格式。

以下是簡單的範例：

```
Value1 = TimeAdd(120000, "H", 1);	// Value1 = 130000
Value2 = TimeAdd(123000, "M", -30); // Value2 = 120000
Value3 = TimeAdd(120000, "MS", 1); // Value3 = 120000.001
```

以下是一個應用範例，使用１分鐘頻率執行，利用TimeAdd來判斷目前資料是否是位於創新高後的１小時內:

```
Var: HighTime(0);
if Date <> Date[1] then HighTime = 0;
if H > Highest(H[1],60) then HighTime = Time;


if HighTime > 0 And Time > HighTime And Time < TimeAdd(HighTime,"H",1) Then 
Begin
   // 創新高後的1小時內
End;
```

請注意上述範例內當Date不等於Date[1]時(分鐘線資料換日)必須把HighTime清掉，以確保HighTime是當日創新高的時間點。

---
## TimeDiff
**Path:** 內建函數 > 時間函數

TimeDiff -  (內建函數)
時間函數

語法：

計算兩個時間數值的差異  
差異 = TimeDiff(第一個時間數值，第二個時間數值, 要計算差異的單位)  
計算的單位可以是"H"(小時), "M"(分鐘), "S"(秒數), 或是"MS"(毫秒)  
回傳數值是第一個時間減第二個時間差幾個小時，幾分鐘，幾秒鐘，或是幾毫秒

說明：

時間數值是一個5~6碼的數字，格式是HHMMSS:

* HH是小時，數值範圍從0到23，可以是1位數字或是2位數字
* MM是分鐘，數值範圍從00到59，必須是2位數字
* SS是秒數，數值範圍從00到59，必須是2位數字

時間數值通常是透過CurrentTime，Time(資料的時間欄位)，或是其他時間相關函數所產生的時間數值。

---

時間數值也可以是一個含有毫秒的 8~9 碼的數字，格式是HHMMSS.fff：

* HH是小時，數值範圍從0到23，可以是1位數字或是2位數字
* MM是分鐘，數值範圍從00到59，必須是2位數字
* SS是秒數，數值範圍從00到59，必須是2位數字
* fff是毫秒，數值範圍從000到999，必須是3位數字

通常是透過CurrentTimeMS，[FilledRecordTimeMS](api?a=FilledRecordTimeMS&b=bif)，或是其他含有毫秒的時間相關函數所產生的時間數值。

TimeDiff回傳的數值是第一個時間減第二個時間的差異:

* 如果第三個參數是"H"，則回傳兩個時間差換算成小時數，
* 如果第三個參數是"M"，則回傳兩個時間換算成分鐘數，
* 如果第三個參數是"S"，則回傳兩個時間換算成秒數。
* 如果第三個參數是"MS"，則回傳兩個時間換算成毫秒。

如果第一個時間小於第二個時間的話，則回傳的數值是負數。

```
Value1 = TimeDiff(130000, 120000, "H"); // Value1 = 1(小時)
Value2 = TimeDiff(133000, 123000, "M"); // Value2 = 60(分鐘)
Value3 = TimeDiff(133000, 090000, "M"); // Value3 = 270(分鐘)
Value4 = TimeDiff(123000, 130000, "H"); // Value4 = -0.5(小時)
Value5 = TimeDiff(120000.123, 120000, "MS"); // Value5 = 123(毫秒)
```

底下是一個應用範例，使用１分鐘資料。利用TimeDiff來計算大單成交的時間間隔，如果發生的很密集的話則觸發。

```
Var: vTime(0); 
if getfielddate("Date") <> getfielddate("Date")[1] then vTime = 0;
If Volume*Close > 10000 then vTime =Time; // 紀錄交易金額大於1000萬的K棒時間
If vTime <> vTime[1] and absValue(TimeDiff(vTime, vTime[1], "M")) < 5 Then begin
	ret = 1;
	print(vTime, vTime[1], absvalue(TimeDiff(vTime, vTime[1], "M")));
end;
```

---
## TimeToString
**Path:** 內建函數 > 時間函數

TimeToString -  (內建函數)
時間函數

語法：

把傳入的時間轉成字串  
時間字串 = TimeToString(時間數值)  
回傳的時間字串格式為HH:MM:SS  
回傳的時間字串格式也可以為HH:MM:SS.fff

說明：

時間數值是一個5~6碼的數字，格式是HHMMSS:

* HH是小時，數值範圍從0到23，可以是1位數字或是2位數字
* MM是分鐘，數值範圍從00到59，必須是2位數字
* SS是秒數，數值範圍從00到59，必須是2位數字
* fff是毫秒，數值範圍從000到999，必須是3位數字

時間數值通常是透過CurrentTime，CurrentTimeMS，Time(資料的時間欄位)，或是其他時間相關函數所產生的時間數值。

回傳字串的格式是"**HH:MM:SS**"，其中HH(小時)的範圍從00到23 (24小時制，兩碼)，MM(分鐘)的範圍從00到59，SS(秒數)的範圍從00到59。

舉例而言，如果目前時間是9點30分00秒，以下的程式碼

```
Print(TimeToString(CurrentTime));
```

將會印出 "09:30:00"的字串。

---

如果回傳字串的格式是"**HH:MM:SS.fff**"，其中HH(小時)的範圍從00到23 (24小時制，兩碼)，MM(分鐘)的範圍從00到59，SS(秒數)的範圍從00到59，fff(毫秒)的範圍從000到999。

例如目前時間是9點30分00秒500毫秒，以下的程式碼

```
Print(TimeToString(CurrentTimeMS));
```

將會印出 "09:30:00.500"的字串。

請參考[StringToTime函數](api?a=stringtotime&b=bif)。

---
## TimeValue
**Path:** 內建函數 > 時間函數

TimeValue -  (內建函數)
時間函數

語法：

用來取得輸入時間數值的各種不同欄位  
欄位數值 = TimeValue(時間數值，指定欄位)  
指定欄位有 H(回傳小時), M(回傳分鐘), S(回傳秒數), MS(回傳毫秒)

說明：

時間數值是一個5~6碼的數字，格式是HHMMSS：

* HH是小時，數值範圍從0到23，可以是1位數字或是2位數字
* MM是分鐘，數值範圍從00到59，必須是2位數字
* SS是秒數，數值範圍從00到59，必須是2位數字

時間數值通常是透過CurrentTime，Time(資料的時間欄位)，或是其他時間相關函數所產生的時間數值。

回傳數值依照指定欄位分別定義:

* 如果是 H 的話，則回傳時間的小時數，範圍從0到23(24小時制)
* 如果是 M 的話，則回傳時間的分鐘數，範圍從0到59
* 如果是 S 的話，則回傳時間的秒數，範圍從0到59

---

時間數值也可以是一個含有毫秒的 8~9碼 的數字，格式為HHMMSS.fff：

* HH是小時，數值範圍從0到23，可以是1位數字或是2位數字
* MM是分鐘，數值範圍從00到59，必須是2位數字
* SS是秒數，數值範圍從00到59，必須是2位數字
* fff是毫秒，數值範圍從000到999，必須是3位數字

含有毫秒的時間數值通常是透過CurrentTimeMS，[FilledRecordTimeMS](api?a=FilledRecordTimeMS&b=bif)，或是其他時間相關函數所產生的時間數值。

回傳數值依照指定欄位分別定義:

* 如果是 H 的話，則回傳時間的小時數，範圍從0到23(24小時制)
* 如果是 M 的話，則回傳時間的分鐘數，範圍從0到59
* 如果是 S 的話，則回傳時間的秒數，範圍從0到59
* 如果是 MS 的話，則回傳時間的毫秒，範圍從0到999

以下是一個範例:

```
Value1 = TimeValue(CurrentTime, "H");
Value2 = TimeValue(CurrentTimeMS, "MS");


If Value1 >= 12 Then Begin
	// 目前時間是中午12點過後
End;


if Value2 >= 500 Then Begin
	// 目前時間是 500 毫秒過後
End;
```

這個函數可以看成是 [Hour](api?a=hour&b=bif), [Minute](api?a=minute&b=bif), [Second](api?a=second&b=bif), 以及 [MilliSecond](api?a=MilliSecond&b=bif) 還有的綜合體。

---
## CheckField
**Path:** 內建函數 > 欄位函數

CheckField -  (內建函數)
欄位函數

語法：

判斷欄位資料是否存在，回傳 True / False。  
CheckField("外盤量", "D");

說明：

CheckField會依據傳入的商品代碼、欄位和頻率來判斷該資料是否能夠取用，回傳True / False。

---
## CheckSymbolField
**Path:** 內建函數 > 欄位函數

CheckSymbolField -  (內建函數)
欄位函數

語法：

判斷指定商品的欄位資料是否存在，回傳 True / False。  
CheckField("2330", "外盤量", "D");

說明：

CheckSymbolField會依據傳入的商品代碼、欄位和頻率來判斷該資料是否能夠取用，回傳True / False。

---
## GetField
**Path:** 內建函數 > 欄位函數

GetField -  (內建函數)
欄位函數

語法：

讀取系統內的欄位資料  
欄位數值 = GetField("欄位名稱")  
欄位數值 = GetField("欄位名稱", "頻率")  
欄位數值 = GetField("欄位名稱", "頻率", 是否為還原值)  
欄位數值 = GetField("欄位名稱", "頻率", 是否為還原值, 設定預設值)  
傳入欄位名稱、指定的欄位頻率、是否為還原值 以及 設定預設值。  
如果不傳頻率的話，則讀取目前執行頻率的對應欄位。  
如果不傳是否為還原值，則讀取原始值資料。

說明：

交易者在制定交易策略時，往往除了開高低收成交量之外，也會想要利用交易市場內獨特的資料，例如台股市場的融資券及法人的進出資料，來作為交易策略的運算基礎。為了讓交易者可以如願以償，XS語法內提供了GetField這個函數，來協助交易者完成這樣的工作。

GetField函數可以傳入四個參數:

* 第一個參數是欄位的中文或是英文名稱，
* 第二個參數則是欄位的頻率，如果省略這個參數的話，則會依照目前腳本執行的頻率來取得對應的資料
* 第三個參數則為「是否為還原值」運用 [Adjusted參數](article.aspx?a=Adjusted) 來設定要讀取的是原始資料，還是還原資料。
* 第四個參數則是「設定預設值」運用Default參數來設定若遇到此欄位對應K棒沒有資料的狀況要回傳的數值。

以下是一個簡單的範例:

```
Value1 = GetField("收盤價");　// value1 為取得目前腳本執行頻率的收盤價。
Value2 = GetField("收盤價", "1");　// value2 為取得原始1分鐘頻率的收盤價。
Value3 = GetField("收盤價", "1", Adjusted:=true);　// value3 為取得還原1分鐘頻率的收盤價。
Value4 = GetField("本益比", "D", Default := 0);    // 當運算的K棒沒有對應的本益比時，則回傳0。
```

設定預設值並無法解決所有的欄位錯誤，例如 取未來值 (ex. GetField("Close", "1", Default:=0)[-1]) 以及 不支援的欄位/頻率 (ex.GetField("本益比", "Y", Default := 0)) 還是會發生錯誤。

GetField可以使用的欄位分成兩種類型:

* 第一種是資料欄位，支援指標腳本、警示腳本以及交易腳本。
* 第二種是選股欄位，支援選股腳本。

使用者可以點選XS編輯器內的編輯選單/插入欄位選項，或是按快捷鍵F7，進入插入欄位畫面內搜尋或是瀏覽需要的欄位，從插入欄位畫面內也可以找到欄位的說明等資訊。

GetField欄位除了可以取得某個欄位在目前K棒的數值之外，也可以取得某個欄位的前期值:

```
If Close > Close[1] And 
   GetField("外資買賣超") > GetField("外資買賣超")[1] 
Then 
   Ret = 1;
```

在上述警示範例內使用GetField("外資買賣超")來讀取外資的買賣超張數。注意到**GetField("外資買賣超")[1]**的用法：GetField函數的回傳值是跟**Close**一樣是一個序列，可以使用[]的語法來讀取前期值。

資料欄位支援Tick、分鐘、日、週、月、季、半年、年、還原日、還原月、還月季等頻率，視欄位內容決定支援的頻率，在插入欄位的畫面中會列出欄位支援的商品及頻率。

選股腳本不支援Tick、分鐘頻率，同樣也能在呼叫GetField時指定引用的資料頻率，

當使用者選擇「外資買賣超」欄位時，頻率選項內會列出這個欄位可以被使用的頻率：日、週、月。如果使用者選擇週的話，則按插入後編輯器內會出現以下的程式碼:

```
GetField("外資買賣超","W");
```

注意到GetField的第二個參數傳入了 "W"，代表要抓取週線頻率。當GetField函數內傳入了指定頻率時，則不管腳本執行的頻率是什麼，系統一定會回傳這個欄位指定的頻率內容。

以下是GetField的頻率代碼清單：

* Tick: 1 Tick
* 1: 1分鐘。也可使用其他數字（1、2、3、5、10、15、20、30、45、60、90、120、135、180、240），代表對應的分鐘線資料
* D: 日資料
* W: 週資料
* M: 月資料
* Q: 季資料
* H: 半年資料
* Y: 年資料
* AD: 還原日資料
* AW: 還原週資料
* AM: 還原月資料

如果在頻率選項內選擇「**預設**」的話，則產生的GetField程式碼內將不會傳入第二個參數。這表示當GetField執行時回傳的資料頻率會跟腳本執行的頻率是一樣的。

如果GetField函數內傳入了頻率參數，而且這個頻率跟目前腳本執行的頻率不一樣的話，則我們稱這種情形為**跨頻率**。

```
If Close > Close[1] and
   GetField("外資買賣超","W") > GetField("外資買賣超","W") [1] then ret = 1;
```

上述選股腳本內我們使用日頻率來執行，所以Close, Close[1]都是日頻率的資料，而GetField("外資買賣超","W")則是週頻率的資料。 當腳本執行時遇到要讀取不同頻率的資料時，系統會使用以下的方式來決定不同頻率的資料的日期:

目前腳本執行的頻率為主頻率，以上例而言主頻率為日頻率，如果要讀取週頻率資料時，以日資料的計算日期來取得當下的週資料；例如，台積電(2330) 在2018/07/16~2018/07/20期間中：

* 週一07/16的週線外資買賣超為-2071張、週線開盤價為224.5元、週線最高價為225元、週線最低價為223.5元、週線收盤價為223.5元、週線成交量為16107張。
* 週二07/17的週線外資買賣超為-3371張、週線開盤價為224.5元、週線最高價為225元、週線最低價為221元、週線收盤價為221.5元、週線成交量為38661張。
* 週三07/18的週線外資買賣超為-1733張、週線開盤價為224.5元、週線最高價為225元、週線最低價為221元、週線收盤價為223元、週線成交量為84464張。
* 週四07/19的週線外資買賣超為6021張、週線開盤價為224.5元、週線最高價為227元、週線最低價為221元、週線收盤價為224.5元、週線成交量為128440張。
* 週五07/20的週線外資買賣超為43423張、週線開盤價為224.5元、週線最高價為237.5元、週線最低價為221元、週線收盤價為237.5元、週線成交量為232092張。

如果依照上述邏輯所對應出來的期別資料不存在的話，則系統會依照資料對位(DataAlign)的設定方式來決定如何處理：

* 如果是絕對對位的話，則腳本執行時會發生錯誤(資料不存在)，
* 如果是遞補對位的話，則系統會往前尋找有資料的期別。以上述為例，假如日期為2018/07/16日，腳本內要去讀取週資料時，依照日期對位邏輯，系統會先找尋2018/07/16日的週資料，如果資料不存在的話則會往前找尋2018/07/13日的週資料，如果還不存在的話則會往前繼續尋找，直到找到為止。

通常在選股腳本內會使用遞補對位(預設值)來來處理財報/營收等資料期別比交易價位資料期別落後公佈的情形。

其他與欄位相關的函數還有 [GetFieldDate函數](api?a=getfielddate&b=bif)以及[GetQuote函數](api?a=getquote&b=bif)。

也可以利用[GetSymbolField](api?a=GetSymbolField&b=bif)取得指定商品的欄位資料（**跨商品**），語法和GetField類似，只需要在第一個參數加上指定商品的SymbolID。

---
## GetFieldDate
**Path:** 內建函數 > 欄位函數

GetFieldDate -  (內建函數)
欄位函數

語法：

讀取系統內的欄位的資料日期  
欄位資料日期 = GetFieldDate(欄位名稱)  
欄位資料日期 = GetFieldDate(欄位名稱, 頻率)  
傳入欄位名稱，跟指定的欄位頻率。如果不傳頻率的話，則使用目前執行頻率。回傳欄位的資料日期，格式為YYYYMMDD 8碼數字。

說明：

在腳本內可以使用[GetField函數](api?a=getfield&b=bif)來取得各種不同的欄位資料，而欄位資料的期別則會依照腳本的對位邏輯來決定。

如果使用者希望知道實際的欄位資料日期的話，則可以使用GetFieldDate這個函數。

GetFieldDate函數傳入的參數與GetField是一樣的，需要傳入欄位名稱，以及指定的期別。與GetField不同的是，GetField回傳的是這個欄位的數值，而GetFieldDate回傳的則是這個欄位的資料日期。

由於營收/財報等資料的公佈日期往往落後於目前的日期，所以如果在運算時需要清楚的知道資料的日期的話，則可以使用GetFieldDate這個函數。

在以下的選股腳本內，我們使用GetFieldDate來判斷最新一期月營收的日期，然後利用這個日期來估計最新一季的獲利:

```
Var: mm(0);


mm = Month(GetFieldDate("月營收","M"));
if mm=1 or mm=4 or mm=7 or mm=10
then value1=GetField("月營收","M") * 3;


if mm=2 or mm=5 or mm=8 or mm=11
then value1=GetField("月營收","M") * 2 + GetField("月營收","M")[1];


if mm=3 or mm=6 or mm=9 or mm=12
then value1=GetField("月營收","M")+GetField("月營收","M")[1]+GetField("月營收","M")[2];


// 預估獲利(單位=百萬) = 季營收 * 毛利率 - 營業費用
//
value2 = value1 * GetField("營業毛利率","Q") - GetField("營業費用","Q");


OutputField1(value1, "預估單季營收(億)");
OutputField2(value2 / 100, "預估單季本業獲利(億)");


ret = 1;
```

當最新一期的月營收的月份是1月/4月/7月/10月時, 我們估算當季的營收為最新這個月的營收 \* 3，如果最新一期的月營收的月份是2月/5月/8月/11月時，因為當季的營收已經公佈了兩個月了，所以我們用當季的第一次營收 ( GetField("月營收", "M")[1] ) 來加上最新這個月的營收 \* 2來當成這一季的估季營收，如果最新一期的月營收的月份是3月/6月/9月/12月的話，則因為當季所有月份的營收都已經公佈了，所以我們就把近三期的月營收加起來。

計算完當季的估計營收之後，接下來就可以使用最新一期的毛利率等資料來估算獲利了。

在上面範例內估算營收的邏輯只是一個範例，如果知道了月份之後，也可以利用月營收年成長率來等資料來做其他的推算。

---
## GetFieldPublishDate
**Path:** 內建函數 > 欄位函數

GetFieldPublishDate -  (內建函數)
欄位函數

語法：

用來取得指定欄位在XQ中更新的日期。  
  
GetFieldPublishDate(欄位名稱, 頻率);  
第一個參數是要查詢的欄位名稱。  
第二個參數是要查詢的欄位頻率，預設值為執行頻率。  
回傳值為YYYYMMDD的8碼數字。

說明：

此函數能夠取得指定的欄位在XQ中更新的日期，亦可以透過 [N] 的方式來取得該欄位前 N 期的更新日。

例如 GetFieldPublishDate("月營收", "M")[1] 回傳的是上個月月營收的公布日 (也就是 GetField("月營收", "M")[1] 的公佈日)。

---
## GetQuote
**Path:** 內建函數 > 欄位函數

GetQuote -  (內建函數)
欄位函數

語法：

讀取系統內的報價欄位資料 (僅支援警示、交易腳本)  
報價欄位數值 = GetQuote(欄位名稱)  
報價欄位數值 = q\_欄位英文名稱  
報價欄位不支援回測功能

說明：

在XS腳本內除了可以讀取資料欄位之外([GetField函數](api?a=getfield&b=bif))，也可以讀取即時的報價欄位。

即時報價欄位涵蓋的範圍有:

* 由交易所提供的即時報價，如五檔委託價量以及即時成交等資料
* 由系統統計的市場行情資訊，如加權指數漲跌家數，當日內外盤量等資料
* 商品的最新一期基本資料相關數據，例如發行股數，每股淨值，最新一季的毛利率等

使用者可以點選XS編輯器內的編輯選單/插入欄位選項，或是按快捷鍵F7，在插入欄位畫面內點選**報價**標籤，然後就可以搜尋或是瀏覽需要的報價欄位。

使用者可以在XS編輯器中的功能列按下「欄位」圖示，或是按快捷鍵F7，進入插入欄位視窗後，先選取「報價欄位」標籤頁，再點選欲加入報價欄位的名稱，最後按下「插入」按鈕，就會在XS編輯器中產生 q\_ 開頭的 GetQuote 報價欄位語法。

![插入欄位GetQ](https://www.xq.com.tw/wp-content/uploads/2021/07/0722Y21_GetQ.png)

目前即時報價欄位僅能使用在警示與交易類型的腳本。而且由於報價欄位的數值是採用即時更新的方式，只提供最新的數值，無法取得前期值，因此無法使用在回測上，在使用上要注意這個特性。

以下是一個使用即時報價欄位的範例:

```
Input: OpenGap(1);
Input: Downpercent(1);


if  q_DailyHigh = q_DailyOpen and
    q_DailyOpen > q_RefPrice * (1+ OpenGap/100) and
    q_Last < q_DailyHigh * (1 - Downpercent/100)
then ret=1;
```

上面的這個警示腳本內判斷如果當日開在最高([q\_DailyHigh](api?a=q_dailyhigh) = [q\_DailyOpen](api?a=q_dailyopen))，而且是跳空開出 (q\_DailyOpen > [q\_RefPrice](api?a=q_refprice) \* (1 + 跳空比例/100)，而目前的價格 [q\_Last](api?a=q_last)已經回落一定範圍的話則觸發警示。在這裡q\_DailyHigh為當日的最高價, q\_DailyOpen為當日的開盤價, q\_RefPrice為當日的參考價, q\_Last為當日的最新價格，這些都是常用的報價欄位。

---
## GetSymbolField
**Path:** 內建函數 > 欄位函數

GetSymbolField -  (內建函數)
欄位函數

語法：

讀取系統內的欄位資料  
欄位數值 = GetSymbolField("ID", "欄位名稱")  
欄位數值 = GetSymbolField("ID", "欄位名稱","頻率")  
欄位數值 = GetSymbolField("ID", "欄位名稱", "頻率", 是否為還原值)  
欄位數值 = GetSymbolField("ID", "欄位名稱", "頻率", 是否為還原值, 設定預設值)  
傳入商品ID、欄位名稱、指定的欄位頻率、是否為還原值 以及 設定預設值。  
如果不傳頻率的話，則讀取目前執行頻率的對應欄位

說明：

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

```
Value1 = getsymbolField("1101.TW", "收盤價");　// value1 為取得目前腳本執行頻率的台泥(1101)收盤價。
Value2 = getsymbolField("1101.TW", "收盤價", "1");　// value2 為取得原始1分鐘頻率的台泥(1101)收盤價。
Value3 = getsymbolField("1101.TW", "收盤價", "1", Adjusted:=true);　// value3 為取得還原1分鐘頻率的台泥(1101)收盤價。
Value4 = getsymbolField("1101.TW", "收盤價", "1", Adjusted:=true, Default:= 0);　// value3 為取得還原1分鐘頻率的台泥(1101)收盤價，當取不到資料時預設值為0。
```

---

另外在「插入欄位」畫面的商品下拉式選單有「標的商品、期貨近/遠/次遠月」選項，選擇後按下插入，系統會自動插入GetSymbolField取得標的商品相關欄位的語法在XS編輯器中，編譯成功後加入相關應用，即可取得商品的「標的商品、期貨近/遠/次遠月」相關欄位數據。

![插入標的商品欄位](https://www.xq.com.tw/wp-content/uploads/2021/07/20210721_%E6%8F%92%E5%85%A5%E6%A8%99%E7%9A%84%E5%95%86%E5%93%81.png)

以下是 GetSymbolField「標的商品、期貨近/遠/次遠月」範例：

```
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

---
## GetSymbolFieldDate
**Path:** 內建函數 > 欄位函數

GetSymbolFieldDate -  (內建函數)
欄位函數

語法：

讀取系統內的欄位的資料日期  
欄位資料日期 = GetSymbolFieldDate("ID", "欄位名稱")  
欄位資料日期 = GetSymbolFieldDate("ID", "欄位名稱","頻率")  
傳入欄位名稱，跟指定的欄位頻率。如果不傳頻率的話，則使用目前執行頻率。回傳欄位的資料日期，格式為YYYYMMDD 8碼數字。

說明：

GetSymbolFieldDate是GetFieldDate語法的延伸，在取得欄位相關資料的日期時可以指定商品，透過這個函數可以在腳本中取得其他商品欄位的資料日期。

GetSymbolFieldDate函數可以傳入三個參數：

* 第一個參數是商品代碼，
* 第二個參數是欄位的中文或是英文名稱，
* 第三個參數則是欄位的頻率，如果省略這個參數的話，則會依照目前腳本執行的頻率來取得對應的資料

以下是一個簡單的範例（選股腳本）：

```
GetSymbolFieldDate("2330.TW","月營收");
GetSymbolFieldDate("2330.TW","月營收","M");
ret=1;
```

詳細的語法說明可以參考 [GetFieldDate](api?a=GetFieldDate&b=bif)函數。

---
## GetSymbolInfo
**Path:** 內建函數 > 欄位函數

GetSymbolInfo -  (內建函數)
欄位函數

語法：

讀取系統內商品資訊欄位資料  
回傳最新的商品資訊 = GetSymbolInfo("商品資訊欄位名稱")

說明：

交易者在制定交易策略時，除了使用交易市場的數據用來計算相關數值外，也會想要利用商品相關資訊，例如台股商品的注意股及買賣現沖的資訊，來作為交易策略的判斷依據。為了讓交易者可以如願以償，XS語法內提供了GetSymbolInfo這個函數，來協助交易者完成這樣的工作。

GetSymbolInfo函數可以傳入商品資訊欄位的中文或是英文名稱，以下是一個簡單的範例：

```
var:aa(""),aaa("");
aa = GetSymbolInfo("交易所");//回傳最新的實際掛牌交易所（支援台股、權證與可轉債）
aaa= GetSymbolInfo("exchange");//回傳最新的實際掛牌交易所（支援台股、權證與可轉債）
```

GetSymbolInfo可以使用的商品資訊欄位為：

* 交易所：回傳商品掛牌的交易所。格式是字串。支援台股、台(權證)、台(可轉債)、台(特別股)、美(股票)
* 即將處置結束股：回傳當日是否是處置期間的最後一日。格式是布林值。支援台股、台(權證)、台(可轉債)
* 近期處置結束股：回傳商品是否在過去7日內剛結束處置。格式是布林值。支援台股、台(權證)、台(可轉債)
* 累計異常注意股：回傳商品當日是否為累計異常注意股。格式是布林值。支援台股、台(權證)、台(可轉債)、台(特別股)
* 處置股：回傳當日是否為處置股(警示股)。格式是布林值。支援台股、台(權證)、台(可轉債)、台(特別股)
* 注意股：回傳當日是否為注意股。格式是布林值。支援台股、台(權證)、台(可轉債)、台(特別股)
* 買賣現沖：回傳當日是否可以現股當沖。格式是布林值。支援台股。
* 先買現沖：回傳當日是否可以現股先買後賣。格式是布林值。支援台股。
* 可放空：回傳當日是否可以融券放空。格式是布林值。支援台股。
* 平可空：回傳當日是否可以在平盤以下融券放空。格式是布林值。支援台股。
* 期貨近月：回傳商品的相關期貨近月商品代碼。格式是字串。例如FITXN01.TF、FICDF01.TF。支援台股、期貨。
* 期貨遠月：回傳商品的相關期貨遠月商品代碼。格式是字串。例如FITXN02.TF、FICDF02.TF。支援台股、期貨。
* 期貨次遠月：回傳商品的相關期貨次遠月商品代碼。格式是字串。例如FITXN03.TF。支援台股、期貨。
* 有認購權證：回傳商品是否有發行認購型權證商品。格式是布林值。支援台股、大盤。
* 有認售權證：回傳商品是否有發行認售型權證商品。格式是布林值。支援台股、大盤。
* 有牛證：回傳商品是否有發行牛證權證商品。格式是布林值。支援台股、大盤。
* 有熊證：回傳商品是否有發行熊證權證商品。格式是布林值。支援台股、大盤。
* 有期貨：回傳商品是否有衍生的期貨商品。格式是布林值。支援台股、大盤。
* 有選擇權：回傳商品是否有衍生的選擇權商品。格式是布林值。支援台股、大盤。
* 有可轉債：回傳商品是否有發行可轉債(CB)商品。格式是布林值。支援台股。
* 執行比例：回傳權證商品的執行比例。格式是數值。台(權證)
* 履約價：回傳選擇權商品或是權證商品的履約價。格式是數值。台(權證)、選擇權。
* 買賣權：回傳選擇權商品或是權證商品的買賣權別。格式是字串(CALL/PUT)。台(權證)、選擇權。
* 到期日：回傳商品的到期日。欄位格式為西元年月日，例如20221101。支援台(權證)、台(可轉債)、期貨、選擇權、美(特別股)
* 標的物：回傳衍生性商品所對應的標的商品代碼。格式是字串。支援台(權證)、台(可轉債)、台(特別股)、期貨、選擇權。
* 轉換價格：回傳可轉債商品(CB)最新的轉換價格。格式是數值。支援台(可轉債)
* 可轉換日：回傳可轉債商品(CB)的可轉換日期。欄位格式為西元年月日，例如20221101。台(可轉債)
* 票面利率：回傳可轉債商品(CB)或是特別股的票面利率。格式是數值。台(可轉債)、台(特別股)、美(特別股)
* 擔保品：回傳可轉換商品(CB)是否有擔保。格式是布林值。台(可轉債)
* 發行張數：回傳可轉債商品(CB)的發行張數。台(可轉債)
* 交易幣別：回傳商品的交易幣別。格式是字串，例如"台幣"，"美元"等。支援台股。
* 交易單位：回傳商品的交易單位。格式是數值。支援台股。
* 面額：回傳股票每一股的面額(是多少元)。格式是數值。支援台股。
* 面額幣別：回傳股票的發行幣別。格式是字串，例如"台幣"，"美元"等。支援台股。
* ETD：是否是交易所交易債券(ETD)。格式是布林值。支援美(特別股)
* 第一個回購日：回傳美股特別股的第一個回購日期。欄位格式為西元年月日，例如20221101。支援美(特別股)

---
## IsSupportField
**Path:** 內建函數 > 欄位函數

IsSupportField -  (內建函數)
欄位函數

語法：

判斷傳入的欄位和頻率是否為可支援的欄位。  
IsSupportField("月營收", "M");

說明：

IsSupportField 可根據傳入的欄位和頻率，判斷指定的欄位是否存在，回傳True / False。

需注意此函數並不會判斷對應K棒上是否有資料，只會判斷欄位存在。
故可能會發生函數回傳True，但該根K棒上沒有資料的情況。

---
## IsSupportSymbolField
**Path:** 內建函數 > 欄位函數

IsSupportSymbolField -  (內建函數)
欄位函數

語法：

判斷傳入的商品代碼、欄位和頻率是否為可支援的欄位。  
IsSupportSymbolField("TSE.TW", "月營收");

說明：

IsSupportSymbolField 可根據傳入的商品代碼、欄位和頻率，判斷指定的欄位是否存在，回傳True / False。

需注意此函數並不會判斷對應K棒上是否有資料，只會判斷欄位存在。
故可能會發生函數回傳True，但該根K棒上沒有資料的情況。

---
## Symbol
**Path:** 內建函數 > 欄位函數

Symbol -  (內建函數)
欄位函數

語法：

回傳目前執行腳本的商品代碼  
回傳代碼 = Symbol

說明：

Symbol函數回傳目前執行腳本的商品代碼，格式是商品編碼 + '.' + 交易所編碼，例如 "2330.TW"，"TSE.TW"，"IBM.US"等。

如果有多個商品同時執行同一個腳本時(例如策略雷達)，可以利用這個函數來判斷目前執行的商品而做不同的處理，也可以利用這個函數搭配[Print函數](api?a=print&b=bif)來輸出目前執行的商品代碼。

範例:

```
If Symbol = "2330.TW" then
begin
    // 目前執行的商品為台積電
end;
```

請參考[SymbolName函數](api?a=symbolname&b=bif)。

---
## SymbolName
**Path:** 內建函數 > 欄位函數

SymbolName -  (內建函數)
欄位函數

語法：

回傳目前執行腳本的商品名稱  
回傳名稱 = SymbolName

說明：

SymbolName函數回傳目前執行腳本的商品名稱，例如 "台積電"，"鴻海"，等。

通常可以在[Print函數](api?a=print&b=sys)內使用這個函數，在列印的內容內把商品名稱印出來。

範例:

```
Print("商品名稱", SymbolName);
```

請參考[Symbol函數](api?a=symbol&b=bif)。

---
## UserID
**Path:** 內建函數 > 欄位函數

UserID -  (內建函數)
欄位函數

語法：

回傳XQ登入帳號  
回傳使用者帳號 = UserID

說明：

UserID函數回傳目前XQ登入者的使用者代碼。

```
Print("目前使用者代碼", UserID);
```

---
## Array_Compare
**Path:** 內建函數 > 陣列函數

Array\_Compare -  (內建函數)
陣列函數

語法：

比較陣列A跟陣列B內的元素  
Array\_Compare(陣列A, 陣列A開始比對的位置, 陣列B, 陣列B開始比對的位置, 比對的個數)  
如果陣列A比較大則回傳1，如果陣列B比較大則回傳-1，如果相同的話則回傳0，例外情形回傳-2

說明：

比對執行的方式如下:

* 先比較 **陣列A[比對開始位置]** 跟 **陣列B[比對開始位置]** 這兩個數值，如果陣列A的值比較大的話則回傳1，如果陣列B的值比較大的話則回傳-1，否則繼續比對兩個陣列的下一個數值(**陣列A[比對開始位置+1]** 與 **陣列B[比對開始位置+1]**)。
* 同樣的，如果陣列A的值比較大的話則回傳1，如果陣列B的值比較大的話則回傳-1，否則繼續比對下去直到比對個數超過為止。
* 如果比到最後都是一樣的話，則回傳0。
* 如果比對的範圍超過Array的大小的話，則回傳-2。

舉例:

```
Array: arrA[5](0); // 宣告arrA是一個有5個元素的陣列，初始值都是0
Array: arrB[5](0); // 宣告arrB是一個有5個元素的陣列，初始值都是0
Array: arrC[5](0); // 宣告arrC是一個有5個元素的陣列，初始值都是0


arrA[1] = 0;  arrA[2] = 10; arrA[3] = 20; arrA[4] = 30; arrA[5] = 40;
arrB[1] = 0;  arrB[2] = 0;  arrB[3] = 10; arrB[4] = 20; arrB[5] = 30;
arrC[1] = 0;  arrC[2] = 20; arrC[3] = 30; arrC[4] = 40; arrC[5] = 50;


Value1 = Array_Compare(arrA, 1, arrB, 1, 3); // 範例1: Value1 = 1
Value2 = Array_Compare(arrA, 1, arrC, 1, 3); // 範例2: Value2 = -1
Value3 = Array_Compare(arrA, 1, arrB, 2, 3); // 範例3: Value3 = 0
Value4 = Array_Compare(arrA, 1, arrB, 1, 8); // 範例4: Value4 = -2
```

第一個範例比對arrA的第一個位置開始的三個數字跟arrB的第一個位置開始的三個數字，也就是比對 (arrA[1], arrA[2], arrA[3])這三個數字與 (arrB[1], arrB[2], arrB[3])這三個數字的差異。其中 arrA的三個數字分別為 (0, 10, 20), 而 arrB的三個數字分別為 (0, 0, 10)。比對時兩邊的第一個數字是相同的(都是0)，而第二個數字 arrA的10 > arrB的0，所以回傳1。

第二個範例比對arrA的第一個位置開始的三個數字跟arrC的第一個位置開始的三個數字，也就是比對 (arrA[1], arrA[2], arrA[3])這三個數字與 (arrC[1], arrC[2], arrC[3])這三個數字的差異。其中 arrA的三個數字分別為 (0, 10, 20), 而 arrC的三個數字分別為 (0, 20, 30)。比對時兩邊的第一個數字是相同的(都是0)，而第二個數字 arrA的10 < arrC的20，所以回傳-1。

第三個範例比對arrA的第一個位置開始的三個數字跟arrB的第二個位置開始的三個數字，也就是比對 (arrA[1], arrA[2], arrA[3])這三個數字與 (arrB[2], arrB[3], arrB[4])這三個數字的差異。其中 arrA的三個數字分別為 (0, 10, 20), 而 arrB的三個數字分別為 (0, 10, 20)。由於這三個數字都一樣，所以回傳0。

第四個範例內，從第一個位置開始比，總共比8個，可是陣列內只有5個元素，超過範圍，於是回傳-2。

---
## Array_Copy
**Path:** 內建函數 > 陣列函數

Array\_Copy -  (內建函數)
陣列函數

語法：

把陣列A的元素複製到陣列B內  
Array\_Copy(陣列A, 陣列A開始複製的位置, 陣列B, 陣列B開始儲存複製資料的位置, 複製的個數)  
如果成功則回傳0，否則回傳小於0的錯誤碼

說明：

請參考以下範例:

```
Array: arrA[5](0); // 宣告arrA是一個有5個元素的陣列，初始值都是0
Array: arrB[5](0); // 宣告arrB是一個有5個元素的陣列，初始值都是0
Array: arrC[5](0); // 宣告arrC是一個有5個元素的陣列，初始值都是0


arrA[1] = 1;  arrA[2] = 2; arrA[3] = 3; arrA[4] = 4; arrA[5] = 5;


Array_Copy(arrA, 1, arrB, 1, 5); // 執行後 arrB = [1, 2, 3, 4, 5]
Array_Copy(arrA, 1, arrC, 2, 3); // 執行後 arrC = [0, 1, 2, 3, 0]
```

第一個範例內，指定從arrA的第一個位置開始複製到arrB的第一個位置，總共複製5個元素，所以執行完成後arrB的內容會是[1, 2, 3, 4, 5]，剛好跟arrA的數值完全一樣。

第二個範例內，指定從arrA的第一個位置開始複製到arrC的第二個位置，總共複製3個元素，也就是說:

* arrA[1] 複製到 arrC[2]
* arrA[2] 複製到 arrC[3]
* arrA[3] 複製到 arrC[4]

所以執行完成後 arrC的內容會是 [0, 1, 2, 3, 0]，注意到arrC的初始值為0，所以沒有被複製到的位置還是保留初始值。

---
## Array_GetMaxIndex
**Path:** 內建函數 > 陣列函數

Array\_GetMaxIndex -  (內建函數)
陣列函數

語法：

取得陣列內的元素個數  
元素個數 = Array\_GetMaxIndex(陣列變數)

說明：

回傳陣列內的元素個數。

```
Array: arrA[5](0); // 宣告arrA是一個有5個元素的陣列，初始值都是0


Value1 = Array_GetMaxIndex(arrA);  // Value1 = 5
```

我們可以利用這個函數來動態取得陣列的大小，讓程式更容易維護：

```
Array: arrA[5](0); // 宣告arrA是一個有5個元素的陣列，初始值都是0
Var: I(0);


For I = 1 To Array_GetMaxIndex(arrA) 
Begin
  arrA[I] = arrA[I] + 1;
End;
```

在上述範例內雖然我們已經知道arrA的大小為5，可是我們還是可以利用 Array\_GetMaxIndex 來取得 arrA 的大小。未來程式如果有需要調整arrA的大小時，程式內迴圈的程式碼可以不需要修改，方便程式的維護。

---
## Array_GetType
**Path:** 內建函數 > 陣列函數

Array\_GetType -  (內建函數)
陣列函數

語法：

回傳陣列的資料類型  
資料類型 = Array\_GetType(陣列)  
回傳數值如果是2，表示為邏輯值True/False陣列，如果是3，表示為字串陣列，如果是7，則表示為數值陣列

說明：

請看下列範例程式跟註解說明:

```
Array: arrNumber[5](0);
Array: arrString[5]("");
Array: arrBoolean[5](true);


Value1 = Array_GetType(arrNumber);  // Value1 = 7
Value2 = Array_GetType(arrString);     // Value2 = 3
Value3 = Array_GetType(arrBoolean); // Value3 = 2
```

---
## Array_SetMaxIndex
**Path:** 內建函數 > 陣列函數

Array\_SetMaxIndex -  (內建函數)
陣列函數

語法：

重新設定陣列的大小  
僅支援一維陣列  
Array\_SetMaxIndex(陣列，陣列內的元素個數)

說明：

設定[動態陣列](api?a=array&b=keyword)的大小。

```
Var: Count(0);
Array: NumArray[](0);


If High > Highest(High,20)[1] Then Count = Count + 1;


Array_SetMaxIndex(NumArray, Count);
NumArray[Count] = High;
```

在上述範例內，我們希望可以儲存破20期新高的所有價格。由於執行過程內可能會發生多次創新高的情形，所以我們使用陣列來儲存這些創新高的價位。又由於無法知道創新高的出現次數，所以程式使用動態陣列來儲存這些價格。在上面的範例內，Count就是目前已經創新高的個數，而當又出現創新高的情形時，程式就使用Array\_SetMaxIndex來擴充陣列的大小。

---
## Array_SetValRange
**Path:** 內建函數 > 陣列函數

Array\_SetValRange -  (內建函數)
陣列函數

語法：

把陣列內某段元素改成指定的數值  
Array\_SetValRange(陣列，開始位置，結束位置，新設定的數值)

說明：

Array\_SetValRange需要傳入四個參數:

* 第一個參數是陣列變數，
* 第二個參數是設定數值的開始位置，位置從1開始，
* 第三個參數是設定數值的結束位置，位置從1開始，
* 第四個參數是要設定的數值

執行時，從這個陣列的開始位置一直到結束位置的每個元素的數值都會被改成為新設定的數值。

```
Array: arr[5](0); // 宣告arrA是一個有5個元素的陣列，初始值都是0


arr[1] = 1;  arr[2] = 2; arr[3] = 3; arr[4] = 4; arr[5] = 5;


Array_SetValRange(arr, 1, 3, 0); // arr[1] = 0, arr[2] = 0, arr[3] = 0, arr[4] = 4, arr[5] = 5
```

在上例內呼叫Array\_SetValRange，位置從1到3，新設定的數值為0。所以執行結束後arr[1], arr[2], arr[3]的數值都會被改成0，而arr[4]跟arr[5]的值則維持不變。

---
## Array_Sort
**Path:** 內建函數 > 陣列函數

Array\_Sort -  (內建函數)
陣列函數

語法：

把陣列內的某段元素進行排序。  
Array\_Sort(陣列，執行排序的開始位置，執行排序的結束位置，排序的順序)  
排序的順序如果是true的話則由小排到大，如果是false的話則由大排到小

說明：

Array\_Sort需要傳入四個參數:

* 第一個參數是要排序的陣列變數，
* 第二個參數是這個陣列內執行排序的開始位置。位置從1開始，
* 第三個參數是這個陣列內執行排序的結束位置。位置從1開始，
* 第四個參數決定排序的順序，如果是**true**的話，則由小排到大，如果是**false**的話，則由大排到小，

執行後這個陣列內指定範圍內元素將會依照指定的排序方式重新排列。

舉例:

```
Array: arr[5](0); // 宣告arrA是一個有5個元素的陣列，初始值都是0


arr[1] = 1;  arr[2] = 3; arr[3] = 5; arr[4] = 2; arr[5] = 4;


Array_Sort(arr, 1, 5, true);   // arr = [1, 2, 3, 4, 5]
Array_Sort(arr, 1, 5, false);  // arr = [5, 4, 3, 2, 1]
```

上例內第一次呼叫Array\_Sort時，傳入的順序是true，所以會從小排到大，執行完成後arr的內容變成
[1, 2, 3, 4, 5]。

第二次呼叫Array\_Sort時，傳入的順序是false，所以會從大排到小，執行完成後arr的內容變成[5, 4, 3, 2, 1]。

---
## Array_Sort2d
**Path:** 內建函數 > 陣列函數

Array\_Sort2d -  (內建函數)
陣列函數

語法：

把二維陣列內的某段元素進行排序。  
Array\_Sort2d(陣列，執行排序的開始位置，執行排序的結束位置，排序的比較欄位，排序的順序)  
排序的順序如果是true的話則由小排到大，如果是false的話則由大排到小

說明：

Array\_Sort2d需要傳入五個參數:

* 第一個參數是要排序的二維陣列變數，
* 第二個參數是這個陣列內執行排序的開始位置。位置從1開始，
* 第三個參數是這個陣列內執行排序的結束位置。位置從1開始，
* 第四個參數是決定排序的基準位置。位置從1開始，
* 第五個參數決定排序的順序，如果是**true**的話，則由小排到大，如果是**false**的話，則由大排到小，

執行後這個陣列內指定範圍內元素將會依照指定的排序方式重新排列。

舉例:

```
Array: datum[15, 6](0); // 宣告datum是一個有15（列）6（行）的二維陣列，初始值都是0
var:i(0);


for i = 1 to 15 begin
   datum[i, 1] = time[i];
   datum[i, 2] = open[i];
   datum[i, 3] = high[i];
   datum[i, 4] = low[i];
   datum[i, 5] = close[i];
   datum[i, 6] = volume[i];
end;
 
array_sort2d(datum, 1, 15, 6, true); //datum = [最小volume, 次小volume, ... 最大volume]
array_sort2d(datum, 1, 15, 6, false); //datum = [最大volume, 次大volume, ... 最小volume]
```

上例內第一次呼叫array\_sort2d時，傳入的順序是true，所以會從小排到大，執行後datum的內容以第六行排序，排序後同列的資料會以第六行為基準一起移動。

第二次呼叫array\_sort2d時，傳入的順序是false，所以會從大排到小，執行後datum的內容以第六行排序，排序後同列的資料會以第六行為基準一起移動。

---
## Array_Sum
**Path:** 內建函數 > 陣列函數

Array\_Sum -  (內建函數)
陣列函數

語法：

回傳陣列內元素的加總數值  
加總數值 = Array\_Sum(陣列, 開始位置, 結束位置)

說明：

Array\_Sum除了要傳入陣列變數之外，尚須傳入要進行加總的開始位置跟結束位置。

舉例:

```
Array: arr[5](0); // 宣告arrA是一個有5個元素的陣列，初始值都是0


arr[1] = 1;  arr[2] = 2; arr[3] = 3; arr[4] = 4; arr[5] = 5;


Value1 = Array_Sum(arr, 1, 5); // Value1 = 15 (1 + 2 + 3 + 4 + 5)
Value2 = Array_Sum(arr, 1, 3); // Value2 = 6 (1 + 2 + 3)
```

上例內Value1是arr這個陣列從第一個元素加總到第五個元素的數值，也就是等於arr[1] + arr[2] + arr[3] + arr[4] + arr[5] = 15，而Value2則是從第一個元素到第三個元素的加總 (1 + 2 + 3 = 6)。

---
## 委比
**Path:** 報價欄位 > 五檔統計

|  |  |
| --- | --- |
| 欄位名稱 | 委比 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("委比"); Value1 = GetQuote("OrderRatio"); Value1 = q_OrderRatio; ``` |
| 欄位分類 | 五檔統計 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 台股 大盤 類股指數 期貨 選擇權 香港股票 大陸股票 美(股票) |
| 說明 | 目前委買賣差佔委買賣比重(%)。  計算公式為(總委買－總委賣)／(總委買＋總委賣) \* 100%。數值的單位為%，當委比為8%時，回傳值為8。 |

---
## 委買
**Path:** 報價欄位 > 五檔統計

|  |  |
| --- | --- |
| 欄位名稱 | 委買 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("委買"); Value1 = GetQuote("BestBidSize"); Value1 = GetQuote("BidSize"); Value1 = GetQuote("最佳委買"); Value1 = q_BestBidSize; Value1 = q_BidSize; ``` |
| 欄位分類 | 五檔統計 |
| 單位 | 台股:張；期貨/選擇權:口；其他市場股票:股 |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 台股 期貨 選擇權 香港股票 大陸股票 美(股票) |
| 說明 | 目前委託簿內買方最高買價的委託量。 數值單位依照商品而異。  如果是台期貨商品的話，則比對五檔委託簿上面最高買價以及衍生委託單的委買價：   * 如果五檔的最高買價比較高，則是取此買價的委託量， * 如果衍生委託單的委買價比較高，則是取衍生委託單的委買量， * 如果兩者相同，則是取五檔最高委買價的委託量以及衍生委託單的委買量的加總。 |

---
## 委買1
**Path:** 報價欄位 > 五檔統計

|  |  |
| --- | --- |
| 欄位名稱 | 委買1 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("委買1"); Value1 = GetQuote("BestBidSize1"); Value1 = q_BestBidSize1; ``` |
| 欄位分類 | 五檔統計 |
| 單位 | 台股:張；期貨/選擇權:口；其他市場股票:股 |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 台股 期貨 選擇權 香港股票 大陸股票 美(股票) |
| 說明 | 委託簿上的第一檔買進委託量。 數值單位依照商品而異。 |

---
## 委買2
**Path:** 報價欄位 > 五檔統計

|  |  |
| --- | --- |
| 欄位名稱 | 委買2 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("委買2"); Value1 = GetQuote("BestBidSize2"); Value1 = q_BestBidSize2; ``` |
| 欄位分類 | 五檔統計 |
| 單位 | 台股:張；期貨/選擇權:口；其他市場股票:股 |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 台股 期貨 選擇權 香港股票 大陸股票 美(股票) |
| 說明 | 委託簿上的第二檔買進委託量。 數值單位依照商品而異。 |

---
## 委買3
**Path:** 報價欄位 > 五檔統計

|  |  |
| --- | --- |
| 欄位名稱 | 委買3 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("委買3"); Value1 = GetQuote("BestBidSize3"); Value1 = q_BestBidSize3; ``` |
| 欄位分類 | 五檔統計 |
| 單位 | 台股:張；期貨/選擇權:口；其他市場股票:股 |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 台股 期貨 選擇權 香港股票 大陸股票 美(股票) |
| 說明 | 委託簿上的第三檔買進委託量。 數值單位依照商品而異。 |

---
## 委買4
**Path:** 報價欄位 > 五檔統計

|  |  |
| --- | --- |
| 欄位名稱 | 委買4 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("委買4"); Value1 = GetQuote("BestBidSize4"); Value1 = q_BestBidSize4; ``` |
| 欄位分類 | 五檔統計 |
| 單位 | 台股:張；期貨/選擇權:口；其他市場股票:股 |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 台股 期貨 選擇權 香港股票 大陸股票 美(股票) |
| 說明 | 委託簿上的第四檔買進委託量。 數值單位依照商品而異。 |

---
## 委買5
**Path:** 報價欄位 > 五檔統計

|  |  |
| --- | --- |
| 欄位名稱 | 委買5 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("委買5"); Value1 = GetQuote("BestBidSize5"); Value1 = q_BestBidSize5; ``` |
| 欄位分類 | 五檔統計 |
| 單位 | 台股:張；期貨/選擇權:口；其他市場股票:股 |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 台股 期貨 選擇權 香港股票 大陸股票 美(股票) |
| 說明 | 委託簿上的第五檔買進委託量。 數值單位依照商品而異。 |

---
## 委買賣差
**Path:** 報價欄位 > 五檔統計

|  |  |
| --- | --- |
| 欄位名稱 | 委買賣差 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("委買賣差"); Value1 = GetQuote("BidAskDiff"); Value1 = q_BidAskDiff; ``` |
| 欄位分類 | 五檔統計 |
| 單位 | 台股:張；大盤/類股:張；期貨/選擇權:口；其他市場股票:股 |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 台股 大盤 類股指數 期貨 選擇權 香港股票 大陸股票 美(股票) |
| 說明 | 目前總委買與總委賣的差異。 計算公式 : 總委買－總委賣。 數值單位依照商品而異。 |

---
## 委賣
**Path:** 報價欄位 > 五檔統計

|  |  |
| --- | --- |
| 欄位名稱 | 委賣 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("委賣"); Value1 = GetQuote("BestAskSize"); Value1 = GetQuote("AskSize"); Value1 = GetQuote("最佳委賣"); Value1 = q_BestAskSize; Value1 = q_AskSize; ``` |
| 欄位分類 | 五檔統計 |
| 單位 | 台股:張；期貨/選擇權:口；其他市場股票:股 |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 台股 期貨 選擇權 香港股票 大陸股票 美(股票) |
| 說明 | 目前委託簿內賣方最低賣價的委託量。 數值單位依照商品而異。  如果是台期貨商品的話，則比對五檔委託簿上面最低賣價以及衍生委託單的委賣價：   * 如果五檔的最低賣價比較低，則是取此賣價的委託量， * 如果衍生委託單的委賣價比較低，則是取衍生委託單的委賣量， * 如果兩者相同，則是取五檔最低委賣價的委託量以及衍生委託單的委賣量的加總。 |

---
## 委賣1
**Path:** 報價欄位 > 五檔統計

|  |  |
| --- | --- |
| 欄位名稱 | 委賣1 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("委賣1"); Value1 = GetQuote("BestAskSize1"); Value1 = q_BestAskSize1; ``` |
| 欄位分類 | 五檔統計 |
| 單位 | 台股:張；期貨/選擇權:口；其他市場股票:股 |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 台股 期貨 選擇權 香港股票 大陸股票 美(股票) |
| 說明 | 委託簿上的第一檔賣出委託量。 數值單位依照商品而異。 |

---
## 委賣2
**Path:** 報價欄位 > 五檔統計

|  |  |
| --- | --- |
| 欄位名稱 | 委賣2 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("委賣2"); Value1 = GetQuote("BestAskSize2"); Value1 = q_BestAskSize2; ``` |
| 欄位分類 | 五檔統計 |
| 單位 | 台股:張；期貨/選擇權:口；其他市場股票:股 |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 台股 期貨 選擇權 香港股票 大陸股票 美(股票) |
| 說明 | 委託簿上的第二檔賣出委託量。 數值單位依照商品而異。 |

---
## 委賣3
**Path:** 報價欄位 > 五檔統計

|  |  |
| --- | --- |
| 欄位名稱 | 委賣3 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("委賣3"); Value1 = GetQuote("BestAskSize3"); Value1 = q_BestAskSize3; ``` |
| 欄位分類 | 五檔統計 |
| 單位 | 台股:張；期貨/選擇權:口；其他市場股票:股 |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 台股 期貨 選擇權 香港股票 大陸股票 美(股票) |
| 說明 | 委託簿上的第三檔賣出委託量。 數值單位依照商品而異。 |

---
## 委賣4
**Path:** 報價欄位 > 五檔統計

|  |  |
| --- | --- |
| 欄位名稱 | 委賣4 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("委賣4"); Value1 = GetQuote("BestAskSize4"); Value1 = q_BestAskSize4; ``` |
| 欄位分類 | 五檔統計 |
| 單位 | 台股:張；期貨/選擇權:口；其他市場股票:股 |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 台股 期貨 選擇權 香港股票 大陸股票 美(股票) |
| 說明 | 委託簿上的第四檔賣出委託量。 數值單位依照商品而異。 |

---
## 委賣5
**Path:** 報價欄位 > 五檔統計

|  |  |
| --- | --- |
| 欄位名稱 | 委賣5 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("委賣5"); Value1 = GetQuote("BestAskSize5"); Value1 = q_BestAskSize5; ``` |
| 欄位分類 | 五檔統計 |
| 單位 | 台股:張；期貨/選擇權:口；其他市場股票:股 |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 台股 期貨 選擇權 香港股票 大陸股票 美(股票) |
| 說明 | 委託簿上的第五檔賣出委託量。 數值單位依照商品而異。 |

---
## 總委買
**Path:** 報價欄位 > 五檔統計

|  |  |
| --- | --- |
| 欄位名稱 | 總委買 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("總委買"); Value1 = GetQuote("SumBidSize"); Value1 = q_SumBidSize; ``` |
| 欄位分類 | 五檔統計 |
| 單位 | 台股:張；大盤/類股:張；期貨/選擇權:口；其他市場股票:股 |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 台股 大盤 類股指數 期貨 選擇權 香港股票 大陸股票 美(股票) |
| 說明 | 委託簿上的前五檔委買數量的加總。 數值單位依照商品而異。  如果是大盤或是類股指數的話，數值是成分股所有商品的總委買張數的加總。 |

---
## 總委賣
**Path:** 報價欄位 > 五檔統計

|  |  |
| --- | --- |
| 欄位名稱 | 總委賣 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("總委賣"); Value1 = GetQuote("SumAskSize"); Value1 = q_SumAskSize; ``` |
| 欄位分類 | 五檔統計 |
| 單位 | 台股:張；大盤/類股:張；期貨/選擇權:口；其他市場股票:股 |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 台股 大盤 類股指數 期貨 選擇權 香港股票 大陸股票 美(股票) |
| 說明 | 委託簿上的前五檔委賣數量的加總。 數值單位依照商品而異。  如果是大盤或是類股指數的話，數值是成分股所有商品的總委賣張數的加總。 |

---
## 買進
**Path:** 報價欄位 > 五檔統計

|  |  |
| --- | --- |
| 欄位名稱 | 買進 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("買進"); Value1 = GetQuote("Bid"); Value1 = q_Bid; ``` |
| 欄位分類 | 五檔統計 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 台股 期貨 選擇權 香港股票 大陸股票 美(股票) |
| 說明 | 目前委託簿內買方最高的買價。  如果是台期貨商品的話，則是取五檔委託簿上面最高的買價以及衍生委託單的委買價的最大者。 |

---
## 買進1
**Path:** 報價欄位 > 五檔統計

|  |  |
| --- | --- |
| 欄位名稱 | 買進1 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("買進1"); Value1 = GetQuote("BestBid1"); Value1 = q_BestBid1; ``` |
| 欄位分類 | 五檔統計 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 台股 期貨 選擇權 香港股票 大陸股票 美(股票) |
| 說明 | 委託簿上的第一檔買進價。 |

---
## 買進2
**Path:** 報價欄位 > 五檔統計

|  |  |
| --- | --- |
| 欄位名稱 | 買進2 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("買進2"); Value1 = GetQuote("BestBid2"); Value1 = q_BestBid2; ``` |
| 欄位分類 | 五檔統計 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 台股 期貨 選擇權 香港股票 大陸股票 美(股票) |
| 說明 | 委託簿上的第二檔買進價。 |

---
## 買進3
**Path:** 報價欄位 > 五檔統計

|  |  |
| --- | --- |
| 欄位名稱 | 買進3 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("買進3"); Value1 = GetQuote("BestBid3"); Value1 = q_BestBid3; ``` |
| 欄位分類 | 五檔統計 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 台股 期貨 選擇權 香港股票 大陸股票 美(股票) |
| 說明 | 委託簿上的第三檔買進價。 |

---
## 買進4
**Path:** 報價欄位 > 五檔統計

|  |  |
| --- | --- |
| 欄位名稱 | 買進4 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("買進4"); Value1 = GetQuote("BestBid4"); Value1 = q_BestBid4; ``` |
| 欄位分類 | 五檔統計 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 台股 期貨 選擇權 香港股票 大陸股票 美(股票) |
| 說明 | 委託簿上的第四檔買進價。 |

---
## 買進5
**Path:** 報價欄位 > 五檔統計

|  |  |
| --- | --- |
| 欄位名稱 | 買進5 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("買進5"); Value1 = GetQuote("BestBid5"); Value1 = q_BestBid5; ``` |
| 欄位分類 | 五檔統計 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 台股 期貨 選擇權 香港股票 大陸股票 美(股票) |
| 說明 | 委託簿上的第五檔買進價。 |

---
## 賣出
**Path:** 報價欄位 > 五檔統計

|  |  |
| --- | --- |
| 欄位名稱 | 賣出 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("賣出"); Value1 = GetQuote("Ask"); Value1 = q_Ask; ``` |
| 欄位分類 | 五檔統計 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 台股 期貨 選擇權 香港股票 大陸股票 美(股票) |
| 說明 | 目前委託簿內賣方最低的賣價。  如果是台期貨商品的話，則是取五檔委託簿上面最低的賣價以及衍生委託單的委賣價的最低者。 |

---
## 賣出1
**Path:** 報價欄位 > 五檔統計

|  |  |
| --- | --- |
| 欄位名稱 | 賣出1 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("賣出1"); Value1 = GetQuote("BestAsk1"); Value1 = q_BestAsk1; ``` |
| 欄位分類 | 五檔統計 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 台股 期貨 選擇權 香港股票 大陸股票 美(股票) |
| 說明 | 委託簿上的第一檔賣出價。 |

---
## 賣出2
**Path:** 報價欄位 > 五檔統計

|  |  |
| --- | --- |
| 欄位名稱 | 賣出2 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("賣出2"); Value1 = GetQuote("BestAsk2"); Value1 = q_BestAsk2; ``` |
| 欄位分類 | 五檔統計 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 台股 期貨 選擇權 香港股票 大陸股票 美(股票) |
| 說明 | 委託簿上的第二檔賣出價。 |

---
## 賣出3
**Path:** 報價欄位 > 五檔統計

|  |  |
| --- | --- |
| 欄位名稱 | 賣出3 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("賣出3"); Value1 = GetQuote("BestAsk3"); Value1 = q_BestAsk3; ``` |
| 欄位分類 | 五檔統計 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 台股 期貨 選擇權 香港股票 大陸股票 美(股票) |
| 說明 | 委託簿上的第三檔賣出價。 |

---
## 賣出4
**Path:** 報價欄位 > 五檔統計

|  |  |
| --- | --- |
| 欄位名稱 | 賣出4 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("賣出4"); Value1 = GetQuote("BestAsk4"); Value1 = q_BestAsk4; ``` |
| 欄位分類 | 五檔統計 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 台股 期貨 選擇權 香港股票 大陸股票 美(股票) |
| 說明 | 委託簿上的第四檔賣出價。 |

---
## 賣出5
**Path:** 報價欄位 > 五檔統計

|  |  |
| --- | --- |
| 欄位名稱 | 賣出5 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("賣出5"); Value1 = GetQuote("BestAsk5"); Value1 = q_BestAsk5; ``` |
| 欄位分類 | 五檔統計 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 台股 期貨 選擇權 香港股票 大陸股票 美(股票) |
| 說明 | 委託簿上的第五檔賣出價。 |

---
## 一年前收盤價
**Path:** 報價欄位 > 價格

|  |  |
| --- | --- |
| 欄位名稱 | 一年前收盤價 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("一年前收盤價"); Value1 = GetQuote("Close1Yago"); Value1 = q_Close1Yago; ``` |
| 欄位分類 | 價格 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 全部 |
| 說明 | 一年前的收盤價。可以用來計算區間漲跌幅。  若今日為2023/9/22日，一年前收盤價是取2022/9/22的價位，若2022/9/22不是交易日，則往前找最接近的交易日。  如果是股票的話採用還原資料計算。 |

---
## 一月前收盤價
**Path:** 報價欄位 > 價格

|  |  |
| --- | --- |
| 欄位名稱 | 一月前收盤價 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("一月前收盤價"); Value1 = GetQuote("Close1Mago"); Value1 = q_Close1Mago; ``` |
| 欄位分類 | 價格 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 全部 |
| 說明 | 一個月前的收盤價。可以用來計算區間漲跌幅。  若今日為9/22日，一個月前收盤價是取8/22的價位，若8/22不是交易日，則往前找最接近的交易日。  如果是股票的話採用還原資料計算。 |

---
## 一週前收盤價
**Path:** 報價欄位 > 價格

|  |  |
| --- | --- |
| 欄位名稱 | 一週前收盤價 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("一週前收盤價"); Value1 = GetQuote("Close1Wago"); Value1 = q_Close1Wago; ``` |
| 欄位分類 | 價格 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 全部 |
| 說明 | 五個交易日前的收盤價。可以用來計算區間漲跌幅。  如果是股票的話採用還原資料計算。 |

---
## 三月前收盤價
**Path:** 報價欄位 > 價格

|  |  |
| --- | --- |
| 欄位名稱 | 三月前收盤價 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("三月前收盤價"); Value1 = GetQuote("Close3Mago"); Value1 = q_Close3Mago; ``` |
| 欄位分類 | 價格 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 全部 |
| 說明 | 三個月前的收盤價。可以用來計算區間漲跌幅。  若今日為9/22日，三個月前收盤價是取6/22的價位，若6/22不是交易日，則往前找最接近的交易日。  如果是股票的話採用還原資料計算。 |

---
## 價差
**Path:** 報價欄位 > 價格

|  |  |
| --- | --- |
| 欄位名稱 | 價差 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("價差"); Value1 = GetQuote("Spread"); Value1 = q_Spread; ``` |
| 欄位分類 | 價格 |
| 單位 | 點 |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 大盤 期貨 |
| 說明 | 期貨價格與現貨價格之間的差額。  計算公式：期貨價差＝期貨價格－現貨價格。  例如：加權指數為16800，而大台指為16795的話，那麼基差＝16795 ─ 16800＝-5。 |

---
## 內外盤
**Path:** 報價欄位 > 價格

|  |  |
| --- | --- |
| 欄位名稱 | 內外盤 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("內外盤"); Value1 = GetQuote("BidAskFlag"); Value1 = q_BidAskFlag; ``` |
| 欄位分類 | 價格 |
| 單位 | 值 |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 全部 |
| 說明 | 最後一筆成交價的內外盤標記。數值是1時表示是外盤成交，數值是-1時為內盤成交，如果是0的話則表示無法判斷。  系統會依照成交發生當時五檔委託簿的最佳委買價格以及最佳委賣價格來判斷此筆是外盤成交或是內盤成交。  如果成交的價格貼近委賣價格的話，則標示為外盤成交，一般會視為主動「買進」。如果成交的價格貼近委買價格的話，則標示為內盤成交，一般會視為主動「賣出」。  如果是指數類型的商品的話，則是比前一價上漲者為外盤，下跌者為內盤。  也可以使用 GetField("內外盤", "Tick") 來表示。 |

---
## 前一價
**Path:** 報價欄位 > 價格

|  |  |
| --- | --- |
| 欄位名稱 | 前一價 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("前一價"); Value1 = GetQuote("PreMatch1"); Value1 = q_PreMatch1; ``` |
| 欄位分類 | 價格 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 全部 |
| 說明 | 最新一筆成交價的前一筆成交價格。也可以使用 GetField("Close", "Tick")[1] 來表示。 |

---
## 前三價
**Path:** 報價欄位 > 價格

|  |  |
| --- | --- |
| 欄位名稱 | 前三價 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("前三價"); Value1 = GetQuote("PreMatch3"); Value1 = q_PreMatch3; ``` |
| 欄位分類 | 價格 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 全部 |
| 說明 | 最新一筆成交價的前第三筆成交價格。也可以使用 GetField("Close", "Tick")[3] 來表示。 |

---
## 前二價
**Path:** 報價欄位 > 價格

|  |  |
| --- | --- |
| 欄位名稱 | 前二價 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("前二價"); Value1 = GetQuote("PreMatch2"); Value1 = q_PreMatch2; ``` |
| 欄位分類 | 價格 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 全部 |
| 說明 | 最新一筆成交價的前第二筆成交價格。也可以使用 GetField("Close", "Tick")[2] 來表示。 |

---
## 前四價
**Path:** 報價欄位 > 價格

|  |  |
| --- | --- |
| 欄位名稱 | 前四價 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("前四價"); Value1 = GetQuote("PreMatch4"); Value1 = q_PreMatch4; ``` |
| 欄位分類 | 價格 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 全部 |
| 說明 | 最新一筆成交價的前第四筆成交價格。也可以使用 GetField("Close", "Tick")[4] 來表示。 |

---
## 去年收盤價
**Path:** 報價欄位 > 價格

|  |  |
| --- | --- |
| 欄位名稱 | 去年收盤價 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("去年收盤價"); Value1 = GetQuote("CloseOfLastYear"); Value1 = q_CloseOfLastYear; ``` |
| 欄位分類 | 價格 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 全部 |
| 說明 | 去年最後一個交易日的收盤價。可以用來計算區間漲跌幅。  若今日為2023/9/22日，去年收盤價是取2022/12/31的價位，若2022/12/31不是交易日，則往前找最接近的交易日。  如果是股票的話採用還原資料計算。 |

---
## 參考價
**Path:** 報價欄位 > 價格

|  |  |
| --- | --- |
| 欄位名稱 | 參考價 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("參考價"); Value1 = GetQuote("RefPrice"); Value1 = q_RefPrice; ``` |
| 欄位分類 | 價格 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 台股 |
| 說明 | 當日的參考價。也可以使用 GetField("RefPrice", "D") 來表示。  一般而言是前一交易日的收盤價，如果股票商品有除權息的話，則是除權息後的參考價。  除權息的計算公式如下(實際數值以交易所公佈為主)： 1.除息參考價為股價－股息 2.除權參考價為股價／(1+配股率) 3.同時除權息參考價為 (股價-股息)／(1+配股率) |

---
## 均價
**Path:** 報價欄位 > 價格

|  |  |
| --- | --- |
| 欄位名稱 | 均價 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("均價"); Value1 = GetQuote("AvgPrice"); Value1 = q_AvgPrice; ``` |
| 欄位分類 | 價格 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 台股 大盤 類股指數 台(權證) 期貨 選擇權 香港股票 大陸股票 美股 |
| 說明 | 當日的平均成交價。也可以使用 GetField("均價", "D") 來表示。  計算方式是累加開盤後每筆成交的價格乘上數量，然後除以累計的成交數量，得出當日平均的成交價。 |

---
## 基差
**Path:** 報價欄位 > 價格

|  |  |
| --- | --- |
| 欄位名稱 | 基差 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("基差"); Value1 = GetQuote("Basis"); Value1 = q_Basis; ``` |
| 欄位分類 | 價格 |
| 單位 | 點 |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 大盤 期貨 |
| 說明 | 現貨價格與期貨價格之間的差異。也可以使用 GetField("基差") 來表示。  計算公式：期貨基差＝現貨價格－期貨價格。  例如：加權指數為16800，而大台指為16795的話，那麼基差＝16800 ─ 16795＝5。 |

---
## 成交
**Path:** 報價欄位 > 價格

|  |  |
| --- | --- |
| 欄位名稱 | 成交 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("成交"); Value1 = GetQuote("Last"); Value1 = q_Last; ``` |
| 欄位分類 | 價格 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 全部 |
| 說明 | 最新一筆成交的價格。等同於 Close 。 |

---
## 振幅
**Path:** 報價欄位 > 價格

|  |  |
| --- | --- |
| 欄位名稱 | 振幅 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("振幅"); Value1 = GetQuote("DayAmplitude"); Value1 = GetQuote("當日振幅"); Value1 = q_DayAmplitude; ``` |
| 欄位分類 | 價格 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 全部 |
| 說明 | 當日的振幅。  振幅計算公式 = (當日最高價 - 當日最低價)／參考價＊100，數值單位為%，若振幅為2.5%，則回傳2.5。 |

---
## 最低_日_
**Path:** 報價欄位 > 價格

|  |  |
| --- | --- |
| 欄位名稱 | 最低(日) (報價欄位) |
| 語法 | ``` Value1 = GetQuote("最低(日)"); Value1 = GetQuote("DailyLow"); Value1 = q_DailyLow; ``` |
| 欄位分類 | 價格 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 全部 |
| 說明 | 當日的最低價。也可以使用 GetField("Low", "D") 來表示。 |

---
## 最高_日_
**Path:** 報價欄位 > 價格

|  |  |
| --- | --- |
| 欄位名稱 | 最高(日) (報價欄位) |
| 語法 | ``` Value1 = GetQuote("最高(日)"); Value1 = GetQuote("DailyHigh"); Value1 = q_DailyHigh; ``` |
| 欄位分類 | 價格 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 全部 |
| 說明 | 當日的最高價。也可以使用 GetField("High", "D") 來表示。 |

---
## 漲停價
**Path:** 報價欄位 > 價格

|  |  |
| --- | --- |
| 欄位名稱 | 漲停價 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("漲停價"); Value1 = GetQuote("DailyUplimit"); Value1 = GetQuote("今日漲停價"); Value1 = q_DailyUplimit; ``` |
| 欄位分類 | 價格 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 台股 期貨 選擇權 大陸股票 |
| 說明 | 當日的漲停價。也可以使用 GetField("漲停價", "D") 來表示。 |

---
## 漲跌幅
**Path:** 報價欄位 > 價格

|  |  |
| --- | --- |
| 欄位名稱 | 漲跌幅 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("漲跌幅"); Value1 = GetQuote("PriceChangeRatio"); Value1 = GetQuote("當日漲幅%"); Value1 = q_PriceChangeRatio; ``` |
| 欄位分類 | 價格 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 全部 |
| 說明 | 當日的漲跌幅。  漲跌幅計算方式為(最新一筆成交價 ─ 參考價)／參考價 ＊ 100。數值的單位是%，當漲跌幅為7%時，回傳7。 |

---
## 買賣價差百分比
**Path:** 報價欄位 > 價格

|  |  |
| --- | --- |
| 欄位名稱 | 買賣價差百分比 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("買賣價差百分比"); Value1 = GetQuote("BidAskDiffRatio"); Value1 = q_BidAskDiffRatio; ``` |
| 欄位分類 | 價格 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 台股 期貨 選擇權 香港股票 大陸股票 美(股票) |
| 說明 | 買賣價差占買價的比例。 計算公式：(賣價 ─ 買價)／買價＊100%。  例如：最佳一檔的買進價位為14.25，最佳一檔賣出價位為14.40，則買賣價差比為(14.40 - 14.25) /14.25\*100%=1.05%，回傳值為1.05。  當買賣價差百分比越大時，表示要成交的話必需支付更多的成本(要花更多的錢去追逐賣價)。 |

---
## 跌停價
**Path:** 報價欄位 > 價格

|  |  |
| --- | --- |
| 欄位名稱 | 跌停價 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("跌停價"); Value1 = GetQuote("DailyDownlimit"); Value1 = GetQuote("今日跌停價"); Value1 = q_DailyDownlimit; ``` |
| 欄位分類 | 價格 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 台股 期貨 選擇權 大陸股票 |
| 說明 | 當日的跌停價。也可以使用 GetField("跌停價", "D") 來表示。 |

---
## 開盤_日_
**Path:** 報價欄位 > 價格

|  |  |
| --- | --- |
| 欄位名稱 | 開盤(日) (報價欄位) |
| 語法 | ``` Value1 = GetQuote("開盤(日)"); Value1 = GetQuote("DailyOpen"); Value1 = q_DailyOpen; ``` |
| 欄位分類 | 價格 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 全部 |
| 說明 | 當日的開盤價。也可以使用 GetField("Open", "D") 來表示。 |

---
## 上漲家數
**Path:** 報價欄位 > 市場統計

|  |  |
| --- | --- |
| 欄位名稱 | 上漲家數 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("上漲家數"); Value1 = GetQuote("UpSecurities"); Value1 = GetQuote("即時上漲家數"); Value1 = q_UpSecurities; ``` |
| 欄位分類 | 市場統計 |
| 單位 | 家 |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 大盤 類股指數 |
| 說明 | 指數成分股內目前上漲的家數。也可以使用GetField("上漲家數", "D")來表示。  大盤商品的統計會扣除權證商品。  請注意：上漲家數與漲停家數是分別統計的，上漲家數並不包含漲停的家數。 |

---
## 下跌家數
**Path:** 報價欄位 > 市場統計

|  |  |
| --- | --- |
| 欄位名稱 | 下跌家數 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("下跌家數"); Value1 = GetQuote("DownSecurities"); Value1 = GetQuote("即時下跌家數"); Value1 = q_DownSecurities; ``` |
| 欄位分類 | 市場統計 |
| 單位 | 家 |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 大盤 類股指數 |
| 說明 | 指數成分股內目前下跌的家數。也可以使用GetField("下跌家數", "D")來表示。  大盤商品的統計會扣除權證商品。  請注意：下跌家數與跌停家數是分別統計的，下跌家數並不包含跌停的家數。 |

---
## 漲停家數
**Path:** 報價欄位 > 市場統計

|  |  |
| --- | --- |
| 欄位名稱 | 漲停家數 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("漲停家數"); Value1 = GetQuote("UpLimitSecs"); Value1 = GetQuote("即時漲停家數"); Value1 = q_UpLimitSecs; ``` |
| 欄位分類 | 市場統計 |
| 單位 | 家 |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 大盤 類股指數 |
| 說明 | 指數成分股內目前漲停的家數。也可以使用GetField("漲停家數", "D")來表示。  大盤商品的統計會扣除權證商品。 |

---
## 跌停家數
**Path:** 報價欄位 > 市場統計

|  |  |
| --- | --- |
| 欄位名稱 | 跌停家數 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("跌停家數"); Value1 = GetQuote("DownLimitSecs"); Value1 = GetQuote("即時跌停家數"); Value1 = q_DownLimitSecs; ``` |
| 欄位分類 | 市場統計 |
| 單位 | 家 |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 大盤 類股指數 |
| 說明 | 指數成分股內目前跌停的家數。也可以使用GetField("跌停家數", "D")來表示。  大盤商品的統計會扣除權證商品。 |

---
## 估計量
**Path:** 報價欄位 > 常用

|  |  |
| --- | --- |
| 欄位名稱 | 估計量 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("估計量"); Value1 = GetQuote("EstimatedTotalVolume"); Value1 = q_EstimatedTotalVolume; ``` |
| 欄位分類 | 常用 |
| 單位 | 個股:張，大盤:元 |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 大盤 台(股票) |
| 說明 | 當日收盤的預估成交量。也可以使用 GetField("估計量", "D") 來表示。  大盤或是個股的估計的方式是統計過去一段時間內每日盤中的分鐘累計成交量，算出每分鐘成交量的分佈比例，然後依照當日開盤迄今的累計成交量，來推算收盤時可能的成交量。  類股商品（細產業指標、主題指標）的計算方式為成分股預估量乘以均價的加總。 |

---
## 參考價
**Path:** 報價欄位 > 常用

|  |  |
| --- | --- |
| 欄位名稱 | 參考價 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("參考價"); Value1 = GetQuote("RefPrice"); Value1 = q_RefPrice; ``` |
| 欄位分類 | 常用 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 台股 |
| 說明 | 當日的參考價。也可以使用 GetField("RefPrice", "D") 來表示。  一般而言是前一交易日的收盤價，如果股票商品有除權息的話，則是除權息後的參考價。  除權息的計算公式如下(實際數值以交易所公佈為主)： 1.除息參考價為股價－股息 2.除權參考價為股價／(1+配股率) 3.同時除權息參考價為 (股價-股息)／(1+配股率) |

---
## 單量
**Path:** 報價欄位 > 常用

|  |  |
| --- | --- |
| 欄位名稱 | 單量 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("單量"); Value1 = GetQuote("TickVolume"); Value1 = q_TickVolume; ``` |
| 欄位分類 | 常用 |
| 單位 | 台股:張；大盤/指數:元；期貨/選擇權:口；其他市場股票:股 |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 全部 |
| 說明 | 最後一筆成交的單量。也可以使用 GetField("Volume", "Tick") 來表示。 數值單位依照商品類型而異。 |

---
## 成交
**Path:** 報價欄位 > 常用

|  |  |
| --- | --- |
| 欄位名稱 | 成交 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("成交"); Value1 = GetQuote("Last"); Value1 = q_Last; ``` |
| 欄位分類 | 常用 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 全部 |
| 說明 | 最新一筆成交的價格。等同於 Close 。 |

---
## 成交時間
**Path:** 報價欄位 > 常用

|  |  |
| --- | --- |
| 欄位名稱 | 成交時間 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("成交時間"); Value1 = GetQuote("Time"); Value1 = q_Time; ``` |
| 欄位分類 | 常用 |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 全部 |
| 說明 | 最新一筆成交的時間。也可以使用 GetField("Time", "Tick") 來表示。  時間格式是hhmmss的6位數字，例如103025代表10點30分25秒。 |

---
## 昨量
**Path:** 報價欄位 > 常用

|  |  |
| --- | --- |
| 欄位名稱 | 昨量 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("昨量"); Value1 = GetQuote("PreTotalVolume"); Value1 = q_PreTotalVolume; ``` |
| 欄位分類 | 常用 |
| 單位 | 台股:張；大盤/指數:元；期貨/選擇權:口；其他市場股票:股 |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 全部 |
| 說明 | 前一個交易日的盤中成交量，不包含零股以及鉅額交易。 也可以使用 GetField("Volume", "D")[1] 來表示。 數值單位依照商品類型而異。 |

---
## 最低_日_
**Path:** 報價欄位 > 常用

|  |  |
| --- | --- |
| 欄位名稱 | 最低(日) (報價欄位) |
| 語法 | ``` Value1 = GetQuote("最低(日)"); Value1 = GetQuote("DailyLow"); Value1 = q_DailyLow; ``` |
| 欄位分類 | 常用 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 全部 |
| 說明 | 當日的最低價。也可以使用 GetField("Low", "D") 來表示。 |

---
## 最高_日_
**Path:** 報價欄位 > 常用

|  |  |
| --- | --- |
| 欄位名稱 | 最高(日) (報價欄位) |
| 語法 | ``` Value1 = GetQuote("最高(日)"); Value1 = GetQuote("DailyHigh"); Value1 = q_DailyHigh; ``` |
| 欄位分類 | 常用 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 全部 |
| 說明 | 當日的最高價。也可以使用 GetField("High", "D") 來表示。 |

---
## 總量_日_
**Path:** 報價欄位 > 常用

|  |  |
| --- | --- |
| 欄位名稱 | 總量(日) (報價欄位) |
| 語法 | ``` Value1 = GetQuote("總量(日)"); Value1 = GetQuote("DailyVolume"); Value1 = q_DailyVolume; ``` |
| 欄位分類 | 常用 |
| 單位 | 台股:張；大盤/指數:元；期貨/選擇權:口；其他市場股票:股 |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 全部 |
| 說明 | 當日的總成交量。也可以使用 GetField("Volume", "D") 來表示。 數值單位依照商品類型而異。 |

---
## 買進
**Path:** 報價欄位 > 常用

|  |  |
| --- | --- |
| 欄位名稱 | 買進 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("買進"); Value1 = GetQuote("Bid"); Value1 = q_Bid; ``` |
| 欄位分類 | 常用 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 台股 期貨 選擇權 香港股票 大陸股票 美(股票) |
| 說明 | 目前委託簿內買方最高的買價。  如果是台期貨商品的話，則是取五檔委託簿上面最高的買價以及衍生委託單的委買價的最大者。 |

---
## 賣出
**Path:** 報價欄位 > 常用

|  |  |
| --- | --- |
| 欄位名稱 | 賣出 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("賣出"); Value1 = GetQuote("Ask"); Value1 = q_Ask; ``` |
| 欄位分類 | 常用 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 台股 期貨 選擇權 香港股票 大陸股票 美(股票) |
| 說明 | 目前委託簿內賣方最低的賣價。  如果是台期貨商品的話，則是取五檔委託簿上面最低的賣價以及衍生委託單的委賣價的最低者。 |

---
## 開盤_日_
**Path:** 報價欄位 > 常用

|  |  |
| --- | --- |
| 欄位名稱 | 開盤(日) (報價欄位) |
| 語法 | ``` Value1 = GetQuote("開盤(日)"); Value1 = GetQuote("DailyOpen"); Value1 = q_DailyOpen; ``` |
| 欄位分類 | 常用 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 全部 |
| 說明 | 當日的開盤價。也可以使用 GetField("Open", "D") 來表示。 |

---
## Delta
**Path:** 報價欄位 > 期權

|  |  |
| --- | --- |
| 欄位名稱 | Delta (報價欄位) |
| 語法 | ``` Value1 = GetQuote("Delta"); Value1 = GetQuote("Delta"); Value1 = q_Delta; ``` |
| 欄位分類 | 期權 |
| 單位 | 值 |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 台(權證) 選擇權 |
| 說明 | 選擇權／權證商品的Delta值，用來衡量標的商品價格的變動幅度對商品價格的影響。  公式是，當標的商品價格變動Y元，選擇權(或權證)價格將變動Delta＊Y(＊執行比例)元。  例如，台指選擇權履約價16500的買權，其Delta值為0.5，表示當加權指數上漲100點時，此買權的價格會上漲50元。  Delta計算式，請參考Black & Scholes模型。 |

---
## Gamma
**Path:** 報價欄位 > 期權

|  |  |
| --- | --- |
| 欄位名稱 | Gamma (報價欄位) |
| 語法 | ``` Value1 = GetQuote("Gamma"); Value1 = GetQuote("Gamma"); Value1 = q_Gamma; ``` |
| 欄位分類 | 期權 |
| 單位 | 值 |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 台(權證) 選擇權 |
| 說明 | 選擇權／權證商品的Gamma值，用來衡量標的商品價格變動幅度對商品Delta的影響。  公式是，當標的商品價格變動Y元，選擇權(或權證)的Delta值將變動Gamma＊Y。  例如，台指選擇權履約價16500的買權，其Delta值為0.5，Gamma值為0.0008，表示當加權指數上漲100點時，此買權的Delta會增加0.08。  Gamma計算式，請參考Black & Scholes模型。 |

---
## RHO
**Path:** 報價欄位 > 期權

|  |  |
| --- | --- |
| 欄位名稱 | RHO (報價欄位) |
| 語法 | ``` Value1 = GetQuote("RHO"); Value1 = GetQuote("RHO"); Value1 = q_RHO; ``` |
| 欄位分類 | 期權 |
| 單位 | % |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 台(權證) 選擇權 |
| 說明 | 選擇權／權證商品的RHO值，用來衡量利率的變動幅度對商品價格的影響。  公式是，當利率變動Y%，選擇權(或權證)價格將變動RHO＊Y(＊執行比例)元。  假設，台指選擇權履約價16500的買權，其RHO值為4.2，表示當利率上漲1%，此買權的價格會上漲4.2元。  RHO計算式，請參考Black & Scholes模型。 |

---
## Theta
**Path:** 報價欄位 > 期權

|  |  |
| --- | --- |
| 欄位名稱 | Theta (報價欄位) |
| 語法 | ``` Value1 = GetQuote("Theta"); Value1 = GetQuote("Theta"); Value1 = q_Theta; ``` |
| 欄位分類 | 期權 |
| 單位 | 值 |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 台(權證) 選擇權 |
| 說明 | 選擇權／權證商品的Theta值，用來衡量時間的流逝對商品價格的影響。  公式是，每經過一天，選擇權(或權證)價格將減少(Theta / 365)元。  例如，台指選擇權履約價16500的買權，Theta值為-5，表示此買權每天會減損0.0137(=-5/365)元。  Theta計算式，請參考Black & Scholes模型。 |

---
## Vega
**Path:** 報價欄位 > 期權

|  |  |
| --- | --- |
| 欄位名稱 | Vega (報價欄位) |
| 語法 | ``` Value1 = GetQuote("Vega"); Value1 = GetQuote("Vega"); Value1 = q_Vega; ``` |
| 欄位分類 | 期權 |
| 單位 | % |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 台(權證) 選擇權 |
| 說明 | 選擇權／權證商品的Vega值，用來衡量標的商品隱含波動率的變動幅度對商品價格的影響。  公式是，當標的商品價格變動Y%，選擇權(或權證)價格將變動Vega＊Y(＊執行比例)元。  例如，台指選擇權履約價16500的買權，其Vega值為16，表示當加權指數的隱含波動率上漲1%時，此買權的價格會上漲16元。  Vega計算式，請參考Black & Scholes模型。 |

---
## 價內外百分比
**Path:** 報價欄位 > 期權

|  |  |
| --- | --- |
| 欄位名稱 | 價內外百分比 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("價內外百分比"); Value1 = GetQuote("IOofMoney"); Value1 = q_IOofMoney; ``` |
| 欄位分類 | 期權 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 台(權證) 選擇權 |
| 說明 | 選擇權／權證商品的價內外百分比，也就是履約價與標的股現價之差。以正值表示價內，負值表示價外。  計算公式： 買權／認購權證＝(標的股價格－履約價) / 履約價*100%。 賣權／認售權證＝(履約價－標的股價格) / 履約價*100%。  回傳數值的單位為％，若價內外%之值為-30.68%，回傳值為-30.68。 |

---
## 內含值
**Path:** 報價欄位 > 期權

|  |  |
| --- | --- |
| 欄位名稱 | 內含值 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("內含值"); Value1 = GetQuote("IntrinsicValue"); Value1 = q_IntrinsicValue; ``` |
| 欄位分類 | 期權 |
| 單位 | 點 |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 台(權證) 選擇權 |
| 說明 | 選擇權的內含值(Intrinsic value)，也就是標的股現價與履約價的差異。  如果是買權/認購權證的話，計算公式為MAX(標的股現價－履約價，0)＊執行比例。 如果是賣權/認售權證的話，計算公式為MAX(履約價－標的股現價，0)＊執行比例。 |

---
## 到期日
**Path:** 報價欄位 > 期權

|  |  |
| --- | --- |
| 欄位名稱 | 到期日 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("到期日"); Value1 = GetQuote("ExpiredDate"); Value1 = q_ExpiredDate; ``` |
| 欄位分類 | 期權 |
| 單位 | 日期 |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 台(權證) 期貨 選擇權 |
| 說明 | 商品的到期日。欄位格式為西元年月日，例如20221101。也可以使用GetSymbolInfo("到期日")來表示。 |

---
## 剩餘交易日
**Path:** 報價欄位 > 期權

|  |  |
| --- | --- |
| 欄位名稱 | 剩餘交易日 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("剩餘交易日"); Value1 = GetQuote("RemainTradingDays"); Value1 = q_RemainTradingDays; ``` |
| 欄位分類 | 期權 |
| 單位 | 天 |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 台(權證) 期貨 選擇權 |
| 說明 | 距離最後交易日的可交易天數。 |

---
## 剩餘日
**Path:** 報價欄位 > 期權

|  |  |
| --- | --- |
| 欄位名稱 | 剩餘日 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("剩餘日"); Value1 = GetQuote("RemainDays"); Value1 = q_RemainDays; ``` |
| 欄位分類 | 期權 |
| 單位 | 天 |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 台(權證) 期貨 選擇權 |
| 說明 | 距離最後交易日的天數(包括例假日)。 |

---
## 執行比例
**Path:** 報價欄位 > 期權

|  |  |
| --- | --- |
| 欄位名稱 | 執行比例 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("執行比例"); Value1 = GetQuote("ContractRatio"); Value1 = q_ContractRatio; ``` |
| 欄位分類 | 期權 |
| 單位 | 值 |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 台(權證) |
| 說明 | 回傳權證商品的執行比例。格式是數值，例如執行比例為0.5時則回傳0.5。也可以使用GetSymbolInfo("執行比例")來表示。  若認購權證執行比例為0.5，表示一張權證到期時可用履約價格購買0.5張股票。也就是每2張權證可認購1張標的股。 |

---
## 履約價
**Path:** 報價欄位 > 期權

|  |  |
| --- | --- |
| 欄位名稱 | 履約價 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("履約價"); Value1 = GetQuote("StrikePrice"); Value1 = q_StrikePrice; ``` |
| 欄位分類 | 期權 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 台(權證) 選擇權 |
| 說明 | 選擇權／權證商品的履約價。也可以使用 GetSymbolInfo("履約價") 來表示。 |

---
## 損益兩平
**Path:** 報價欄位 > 期權

|  |  |
| --- | --- |
| 欄位名稱 | 損益兩平 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("損益兩平"); Value1 = GetQuote("BreakEvenPoint"); Value1 = q_BreakEvenPoint; ``` |
| 欄位分類 | 期權 |
| 單位 | 值 |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 台(權證) |
| 說明 | 讓權證商品可以達到損益兩平的標的股價格。  認購權證的損益兩平價格是履約價＋權證價格 /執行比率。 認售權證的損益兩平價格則是履約價 ─ 權證價格 /執行比率。  舉例而言，假如認購權證的履約價是100，目前權證價格為1，而執行比例是0.2的話，那麼損益兩平的價格是100＋1／0.2＝105。 |

---
## 時間價值
**Path:** 報價欄位 > 期權

|  |  |
| --- | --- |
| 欄位名稱 | 時間價值 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("時間價值"); Value1 = GetQuote("TimeValue"); Value1 = q_TimeValue; ``` |
| 欄位分類 | 期權 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 台(權證) 選擇權 |
| 說明 | 選擇權／權證商品的時間價值(Time value)。  如果是選擇權的話，計算公式為 MAX(選擇權價格 ─ 內含值，0)。  如果是權證商品的話，則先計算權證商品的內含值：如果是認購權證的話(CALL)，內含值為MAX(標的股現價－履約價，0)＊執行比例，如果是認售權證的話(PUT)，內含值為MAX(履約價－標的股現價，0)＊執行比例。  有了內含值後，權證商品的時間價值也是同樣的公式：MAX(權證價格 ─ 內含值，0)。 |

---
## 最後交易日
**Path:** 報價欄位 > 期權

|  |  |
| --- | --- |
| 欄位名稱 | 最後交易日 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("最後交易日"); Value1 = GetQuote("LastTradingDate"); Value1 = q_LastTradingDate; ``` |
| 欄位分類 | 期權 |
| 單位 | 日期 |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 台(權證) 期貨 選擇權 |
| 說明 | 商品的最後交易日。欄位格式為西元年月日，例如20221101。 |

---
## 有效槓桿
**Path:** 報價欄位 > 期權

|  |  |
| --- | --- |
| 欄位名稱 | 有效槓桿 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("有效槓桿"); Value1 = GetQuote("Leverage"); Value1 = q_Leverage; ``` |
| 欄位分類 | 期權 |
| 單位 | 比值 |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 台(權證) 選擇權 |
| 說明 | 選擇權／權證商品的有效槓桿倍數，代表標的股每變動1%時商品價格的變動幅度。  計算公式：標的股價格 / 商品價格 ＊ Delta值。  假設，標的股價格為30，商品價格3，Delta為0.05，則有效槓桿為5(倍)。 |

---
## 標的價格
**Path:** 報價欄位 > 期權

|  |  |
| --- | --- |
| 欄位名稱 | 標的價格 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("標的價格"); Value1 = GetQuote("TargetPrice"); Value1 = q_TargetPrice; ``` |
| 欄位分類 | 期權 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 台(權證) 選擇權 |
| 說明 | 選擇權／權證商品的標的股目前的價格。 |

---
## 標的漲跌
**Path:** 報價欄位 > 期權

|  |  |
| --- | --- |
| 欄位名稱 | 標的漲跌 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("標的漲跌"); Value1 = GetQuote("TargetChange"); Value1 = q_TargetChange; ``` |
| 欄位分類 | 期權 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 台(權證) 選擇權 |
| 說明 | 選擇權／權證商品的標的股目前的漲跌。 |

---
## 標的漲跌幅
**Path:** 報價欄位 > 期權

|  |  |
| --- | --- |
| 欄位名稱 | 標的漲跌幅 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("標的漲跌幅"); Value1 = GetQuote("TargetChangeRatio"); Value1 = GetQuote("標的漲幅百分比"); Value1 = q_TargetChangeRatio; ``` |
| 欄位分類 | 期權 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 台(權證) 選擇權 |
| 說明 | 選擇權／權證商品的標的股目前的漲跌幅。單位是％，例如標的股上漲0.5％，則回傳0.5。 |

---
## 波動率
**Path:** 報價欄位 > 期權

|  |  |
| --- | --- |
| 欄位名稱 | 波動率 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("波動率"); Value1 = GetQuote("Volatility"); Value1 = q_Volatility; ``` |
| 欄位分類 | 期權 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 台股 大盤 類股指數 選擇權 香港股票 |
| 說明 | 選擇權／權證商品標的股的歷史波動率。單位是％，例如數值如果是23.12％，則回傳23.12。  計算方式是使用最近20天的股價變化，算出年化的歷史波動率，以此來衡量標的股股價的波動幅度。  除了選擇權／權證商品之外，有發行衍生性商品的台股或是指數商品也都支援此欄位。 |

---
## 波動率差額
**Path:** 報價欄位 > 期權

|  |  |
| --- | --- |
| 欄位名稱 | 波動率差額 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("波動率差額"); Value1 = GetQuote("VolatilityDiff"); Value1 = q_VolatilityDiff; ``` |
| 欄位分類 | 期權 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 台(權證) 選擇權 |
| 說明 | 歷史波動率與隱含波動率的差異。 計算公式：歷史波動率 ─ 隱含波動率。  例如歷史波動率為34.03%，隱含波動率為49.16，則波動率差額為-15.13%，回傳值為-15.13。 |

---
## 溢價率百分比
**Path:** 報價欄位 > 期權

|  |  |
| --- | --- |
| 欄位名稱 | 溢價率百分比 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("溢價率百分比"); Value1 = GetQuote("PremiumRatio"); Value1 = q_PremiumRatio; ``` |
| 欄位分類 | 期權 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 台(權證) |
| 說明 | 損益兩平價格與標的價格的差異幅度比例。數值越大，代表要達到損益兩平越不容易，風險就越大。  認購權證的溢價率為(損益兩平 ─ 標的股價格)／標的股價格＊100％。 認售權證的溢價率為(標的股價格 ─ 損益兩平)／標的股價格＊100％。  舉例而言，如果一檔認購權證的損益兩平價格是105，而標的股的價格是90，那麼溢價率＝(105-90)/90 \* 100% = 16.67%，欄位回傳值則為16.67。 |

---
## 獲利率百分比
**Path:** 報價欄位 > 期權

|  |  |
| --- | --- |
| 欄位名稱 | 獲利率百分比 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("獲利率百分比"); Value1 = GetQuote("Profitability"); Value1 = q_Profitability; ``` |
| 欄位分類 | 期權 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 台(權證) 選擇權 |
| 說明 | 理論價格與市場賣價差佔賣價的比例。 計算公式＝(理論價 ─ 賣價)／賣價 ＊ 100%。  假設權證的理論價格是1.5，市場最佳一檔賣價是1.6，那麼獲利率為(1.5 - 1.6) / 1.6 \* 100 = -6.25%，欄位的回傳值則為-6.25。 |

---
## 理論價
**Path:** 報價欄位 > 期權

|  |  |
| --- | --- |
| 欄位名稱 | 理論價 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("理論價"); Value1 = GetQuote("TheoreticalPrice"); Value1 = q_TheoreticalPrice; ``` |
| 欄位分類 | 期權 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 台(權證) 選擇權 |
| 說明 | 選擇權／權證商品的理論價格，採用Black-Scholes模型計算。也可以使用 GetField("理論價", "D") 來表示。  可以比對現價與理論價的差異，判斷目前價格的乖離程度。 |

---
## 買賣權成交量比率
**Path:** 報價欄位 > 期權

|  |  |
| --- | --- |
| 欄位名稱 | 買賣權成交量比率 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("買賣權成交量比率"); Value1 = GetQuote("CPTradeRatio"); Value1 = q_CPTradeRatio; ``` |
| 欄位分類 | 期權 |
| 單位 | 比值 |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 選擇權 |
| 說明 | 同契約所有賣權商品(PUT)的成交量／所有買權商品(CALL)的成交量。 |

---
## 買賣權未平倉量比率
**Path:** 報價欄位 > 期權

|  |  |
| --- | --- |
| 欄位名稱 | 買賣權未平倉量比率 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("買賣權未平倉量比率"); Value1 = GetQuote("CPOIRatio"); Value1 = q_CPOIRatio; ``` |
| 欄位分類 | 期權 |
| 單位 | 比值 |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 選擇權 |
| 說明 | 同契約所有賣權商品(PUT)的未平倉量／買權商品(CALL)的未平倉量。 |

---
## 買進隱含波動率
**Path:** 報價欄位 > 期權

|  |  |
| --- | --- |
| 欄位名稱 | 買進隱含波動率 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("買進隱含波動率"); Value1 = GetQuote("ImpliedVolatilityonBuy"); Value1 = q_ImpliedVolatilityonBuy; ``` |
| 欄位分類 | 期權 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 台(權證) 選擇權 |
| 說明 | 以最新的買進價格所推算出來的隱含波動率(IV)。 單位是％，例如例如數值如果是23.12％，則回傳23.12。  隱含波動率是以商品在市場上的最新價格以Black-Scholes 模型反推出的標的商品波動率。隱含波動率越高就代表標的波動度越高(價格行情震盪越劇烈)。 |

---
## 賣出隱含波動率
**Path:** 報價欄位 > 期權

|  |  |
| --- | --- |
| 欄位名稱 | 賣出隱含波動率 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("賣出隱含波動率"); Value1 = GetQuote("ImpliedVolatilityonSell"); Value1 = q_ImpliedVolatilityonSell; ``` |
| 欄位分類 | 期權 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 台(權證) 選擇權 |
| 說明 | 以最新的賣出價格所推算出來的隱含波動率(IV)。 單位是％，例如例如數值如果是23.12％，則回傳23.12。  隱含波動率是以商品在市場上的最新價格以Black-Scholes 模型反推出的標的商品波動率。隱含波動率越高就代表標的波動度越高(價格行情震盪越劇烈)。 |

---
## 每股淨值
**Path:** 報價欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 每股淨值 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("每股淨值"); Value1 = GetQuote("NetValuePerShare"); Value1 = q_NetValuePerShare; ``` |
| 欄位分類 | 財務 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 台股 香港股票 大陸股票 美(股票) |
| 說明 | 最新一期財報的每股淨值。  台股採財務比率表的「每股淨值(F)(TSE公告數)」項目。 美股採財務比率表上的「每股淨值」項目。  台股，陸股，港股採用的是累計財報的數值，而美股則是單季財報的數值。 |

---
## 每股營收
**Path:** 報價欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 每股營收 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("每股營收"); Value1 = GetQuote("OpeRevenuePerShare"); Value1 = q_OpeRevenuePerShare; ``` |
| 欄位分類 | 財務 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 台股 香港股票 大陸股票 美(股票) |
| 說明 | 當期財報的「每股營業額(元)」欄位。  台股，陸股，港股採用的是累計財報的數值，而美股則是單季財報的數值。 |

---
## 每股盈餘
**Path:** 報價欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 每股盈餘 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("每股盈餘"); Value1 = GetQuote("CurrentEPS"); Value1 = GetQuote("即期每股盈餘"); Value1 = q_CurrentEPS; ``` |
| 欄位分類 | 財務 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 台股 香港股票 大陸股票 美(股票) |
| 說明 | 最新一期財報的「每股盈餘」欄位。  美股採用的是損益表上的「基本每股盈餘(由淨利計算)」項目。  目前支援台股，港股，陸股以及美股。 |

---
## 毛利率
**Path:** 報價欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 毛利率 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("毛利率"); Value1 = GetQuote("GrossMarginRate"); Value1 = q_GrossMarginRate; ``` |
| 欄位分類 | 財務 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 台股 香港股票 大陸股票 美(股票) |
| 說明 | 最新一期財報的「營業毛利率」欄位。單位為％，例如營業毛利率為25%時則回傳25。  台股，陸股，港股採用的是累計財報的數值，而美股則是單季財報的數值。 |

---
## 營收年增率
**Path:** 報價欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 營收年增率 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("營收年增率"); Value1 = GetQuote("RevenueYoY"); Value1 = GetQuote("營收年增率%"); Value1 = q_RevenueYoY; ``` |
| 欄位分類 | 財務 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 台股 香港股票 大陸股票 美(股票) |
| 說明 | 最新一期營收較去年同期營收的成長率。單位為%，例如營收年增率為10%，則回傳10。  計算公式 ＝(本期營收－去年同期) / 去年同期\*100%。  台股營收是每月一期，陸股/美股是每季一期，而港股則是每半年一期。 |

---
## 營收月份
**Path:** 報價欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 營收月份 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("營收月份"); Value1 = GetQuote("RevenueMonth"); Value1 = q_RevenueMonth; ``` |
| 欄位分類 | 財務 |
| 單位 | 日期 |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 台股 香港股票 大陸股票 美(股票) |
| 說明 | 最新一期營收的年月別。資料格式為YYYYMM的6碼數字，例如最新一期營收為2023/8月，則回傳值為202308。 |

---
## 營收期增率
**Path:** 報價欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 營收期增率 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("營收期增率"); Value1 = GetQuote("RevenueGrowth"); Value1 = GetQuote("營收期增率%"); Value1 = q_RevenueGrowth; ``` |
| 欄位分類 | 財務 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 台股 香港股票 大陸股票 美(股票) |
| 說明 | 最新一期營收較上一期營收的成長率。單位為%，例如營收期增率為10%，則回傳10。  計算公式＝(本期營收－上期營收) / 上期營收\*100%。  台股營收是每月一期，陸股/美股是每季一期，而港股則是每半年一期。 |

---
## 營益率
**Path:** 報價欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 營益率 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("營益率"); Value1 = GetQuote("OpeProfitMarginRate"); Value1 = q_OpeProfitMarginRate; ``` |
| 欄位分類 | 財務 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 台股 香港股票 大陸股票 美(股票) |
| 說明 | 最新一期財報的「營業利益率」欄位。單位為％，例如營業利益率為25%時則回傳25。  台股，陸股，港股採用的是累計財報的數值，而美股則是單季財報的數值。 |

---
## 股東權益報酬率
**Path:** 報價欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 股東權益報酬率 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("股東權益報酬率"); Value1 = GetQuote("CurrentROE"); Value1 = GetQuote("即期股東報酬率%"); Value1 = q_CurrentROE; ``` |
| 欄位分類 | 財務 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 台股 香港股票 大陸股票 美(股票) |
| 說明 | 最新一期財報的「股東權益報酬率」(ROE)欄位。單位為％，例如股東權益報酬率為10%時則回傳10。  台股採用的是財務比率表的「ROE(A)─稅後」項目。 美股、陸股、港股採用的是財務比率表上的「股東權益報酬率」項目。  台股，陸股，港股採用的是累計財報的數值，而美股則是單季財報的數值。 |

---
## 財報期別
**Path:** 報價欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 財報期別 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("財報期別"); Value1 = GetQuote("FinancialStatementsTime"); Value1 = q_FinancialStatementsTime; ``` |
| 欄位分類 | 財務 |
| 單位 | 日期 |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 台股 香港股票 大陸股票 美(股票) |
| 說明 | 最新一期財報的年月別。資料格式為YYYYMM的6碼數字。財報期別如果是季，則以3、6、9、12來表示1Q、2Q、3Q、4Q。財報期別如果是半年，則以6、12來代表H、2H。例如最新一期財報為2022/2Q，則回傳值為202206。 |

---
## 估計量
**Path:** 報價欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 估計量 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("估計量"); Value1 = GetQuote("EstimatedTotalVolume"); Value1 = q_EstimatedTotalVolume; ``` |
| 欄位分類 | 量能 |
| 單位 | 個股:張，大盤:元 |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 大盤 台(股票) |
| 說明 | 當日收盤的預估成交量。也可以使用 GetField("估計量", "D") 來表示。  大盤或是個股的估計的方式是統計過去一段時間內每日盤中的分鐘累計成交量，算出每分鐘成交量的分佈比例，然後依照當日開盤迄今的累計成交量，來推算收盤時可能的成交量。  類股商品（細產業指標、主題指標）的計算方式為成分股預估量乘以均價的加總。 |

---
## 內盤量
**Path:** 報價欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 內盤量 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("內盤量"); Value1 = GetQuote("InSize"); Value1 = GetQuote("當日內盤量"); Value1 = q_InSize; ``` |
| 欄位分類 | 量能 |
| 單位 | 台股:張；大盤/指數:元；期貨/選擇權:口；其他市場股票:股 |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 台股 大盤 類股指數 期貨 選擇權 香港股票 大陸股票 |
| 說明 | 當日累計迄今的內盤量。也可以使用 GetField("內盤量", "D") 來表示。 數值單位依照商品類型而異。  系統會依照成交發生當時委託簿的最佳委買價格以及最佳委賣價格來判斷此筆是外盤成交或是內盤成交。如果成交的價格貼近委買價格的話，則標示為內盤成交，一般會視為主動「賣出」。  如果是指數類型商品的話，則是比前一價下跌的所有成交量的加總。 |

---
## 單量
**Path:** 報價欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 單量 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("單量"); Value1 = GetQuote("TickVolume"); Value1 = q_TickVolume; ``` |
| 欄位分類 | 量能 |
| 單位 | 台股:張；大盤/指數:元；期貨/選擇權:口；其他市場股票:股 |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 全部 |
| 說明 | 最後一筆成交的單量。也可以使用 GetField("Volume", "Tick") 來表示。 數值單位依照商品類型而異。 |

---
## 外盤量
**Path:** 報價欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 外盤量 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("外盤量"); Value1 = GetQuote("OutSize"); Value1 = GetQuote("當日外盤量"); Value1 = q_OutSize; ``` |
| 欄位分類 | 量能 |
| 單位 | 台股:張；大盤/指數:元；期貨/選擇權:口；其他市場股票:股 |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 台股 大盤 類股指數 期貨 選擇權 香港股票 大陸股票 |
| 說明 | 當日累計迄今的外盤量。也可以使用 GetField("外盤量", "D") 來表示。 數值單位依照商品類型而異。  系統會依照成交發生當時委託簿的最佳委買價格以及最佳委賣價格來判斷此筆是外盤成交或是內盤成交。如果成交的價格貼近委賣價格的話，則標示為外盤成交，一般會視為主動「買進」。  如果是指數類型商品的話，則是比前一價上漲的所有成交量的加總。 |

---
## 委買均
**Path:** 報價欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 委買均 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("委買均"); Value1 = GetQuote("AvgLongUnits"); Value1 = q_AvgLongUnits; ``` |
| 欄位分類 | 量能 |
| 單位 | 大盤:張；期貨/選擇權:口 |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 大盤 期貨 選擇權 |
| 說明 | 每一筆委買的平均張數/口數。 計算公式：累計委買／累委買筆。 |

---
## 委賣均
**Path:** 報價欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 委賣均 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("委賣均"); Value1 = GetQuote("AvgShortUnits"); Value1 = q_AvgShortUnits; ``` |
| 欄位分類 | 量能 |
| 單位 | 大盤:張；期貨/選擇權:口 |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 大盤 期貨 選擇權 |
| 說明 | 每一筆委賣的平均張數/口數。 計算公式：累計委賣／累委賣筆。 |

---
## 成交均量
**Path:** 報價欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 成交均量 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("成交均量"); Value1 = GetQuote("AvgDealedShare"); Value1 = q_AvgDealedShare; ``` |
| 欄位分類 | 量能 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 大盤 |
| 說明 | 整體市場(扣除權證)的每筆成交平均張數。 計算公式：累計成交／累成交筆。 |

---
## 成交比重
**Path:** 報價欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 成交比重 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("成交比重"); Value1 = GetQuote("CashDirect"); Value1 = GetQuote("即時資金流向"); Value1 = q_CashDirect; ``` |
| 欄位分類 | 量能 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 台股 類股指數 |
| 說明 | 成交值占對應大盤的成交比重(％)。 單位是%，例如數值是5.6％的話，則回傳5.6。  支援台股以及類股指數，計算公式是成交值／對應大盤成交值＊100％。 |

---
## 成交金額_元_
**Path:** 報價欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 成交金額(元) (報價欄位) |
| 語法 | ``` Value1 = GetQuote("成交金額(元)"); Value1 = GetQuote("TotalAmount"); Value1 = GetQuote("成交值"); Value1 = q_TotalAmount; ``` |
| 欄位分類 | 量能 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 台股 大盤 類股指數 香港股票 大陸股票 美(股票) |
| 說明 | 當日的總成交金額。也可以使用 GetField("成交金額", "D") 來表示。 |

---
## 昨量
**Path:** 報價欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 昨量 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("昨量"); Value1 = GetQuote("PreTotalVolume"); Value1 = q_PreTotalVolume; ``` |
| 欄位分類 | 量能 |
| 單位 | 台股:張；大盤/指數:元；期貨/選擇權:口；其他市場股票:股 |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 全部 |
| 說明 | 前一個交易日的盤中成交量，不包含零股以及鉅額交易。 也可以使用 GetField("Volume", "D")[1] 來表示。 數值單位依照商品類型而異。 |

---
## 累委買筆
**Path:** 報價欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 累委買筆 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("累委買筆"); Value1 = GetQuote("CulBidTicks"); Value1 = GetQuote("累計買筆"); Value1 = q_CulBidTicks; ``` |
| 欄位分類 | 量能 |
| 單位 | 筆 |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 大盤 期貨 選擇權 |
| 說明 | 開盤迄今的總委買筆數。也可以使用 GetField("累委買筆", "D") 來表示。  證券市場的數值為扣除權證的統計。 |

---
## 累委賣筆
**Path:** 報價欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 累委賣筆 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("累委賣筆"); Value1 = GetQuote("CulAskTicks"); Value1 = GetQuote("累計賣筆"); Value1 = q_CulAskTicks; ``` |
| 欄位分類 | 量能 |
| 單位 | 筆 |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 大盤 期貨 選擇權 |
| 說明 | 開盤迄今的總委賣筆數。也可以使用 GetField("累委賣筆", "D") 來表示。  證券市場的數值為扣除權證的統計。 |

---
## 累成交筆
**Path:** 報價欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 累成交筆 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("累成交筆"); Value1 = GetQuote("CulMatchTicks"); Value1 = q_CulMatchTicks; ``` |
| 欄位分類 | 量能 |
| 單位 | 筆 |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 大盤 |
| 說明 | 開盤迄今整體市場(扣除權證)的成交總筆數。也可以使用 GetField("累成交筆", "D") 來表示。 |

---
## 累計委買
**Path:** 報價欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 累計委買 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("累計委買"); Value1 = GetQuote("BidUnits"); Value1 = GetQuote("即時委買張數"); Value1 = q_BidUnits; ``` |
| 欄位分類 | 量能 |
| 單位 | 大盤:張；期貨/選擇權:口 |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 大盤 期貨 選擇權 |
| 說明 | 開盤迄今的總委買張數/口數。也可以使用 GetField("累計委買", "D") 來表示。  證券市場的數值為扣除權證的統計。 |

---
## 累計委賣
**Path:** 報價欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 累計委賣 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("累計委賣"); Value1 = GetQuote("AskUnits"); Value1 = GetQuote("即時委賣張數"); Value1 = q_AskUnits; ``` |
| 欄位分類 | 量能 |
| 單位 | 大盤:張；期貨/選擇權:口 |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 大盤 期貨 選擇權 |
| 說明 | 開盤迄今的總委賣張數/口數。也可以使用 GetField("累計委賣", "D") 來表示。  證券市場的數值為扣除權證的統計。 |

---
## 累計成交
**Path:** 報價欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 累計成交 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("累計成交"); Value1 = GetQuote("MatchUnit"); Value1 = GetQuote("即時成交張數"); Value1 = q_MatchUnit; ``` |
| 欄位分類 | 量能 |
| 單位 | 大盤:張；期貨/選擇權:口 |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 大盤 期貨 選擇權 |
| 說明 | 開盤迄今的總成交張數/口數，也可以使用GetField("累計成交", "D")來表示。  證券市場的數值為扣除權證的統計。 |

---
## 累買成筆
**Path:** 報價欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 累買成筆 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("累買成筆"); Value1 = GetQuote("CulBuyTicks"); Value1 = q_CulBuyTicks; ``` |
| 欄位分類 | 量能 |
| 單位 | 筆 |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 期貨 選擇權 |
| 說明 | 開盤迄今以買進成交的總筆數。也可以使用GetField("累買成筆", "D")來表示。  可以使用此數值(與累賣成筆)來估算盤中的趨勢：如果累賣成筆>累買成筆的話，代表每一筆以賣出成交的單量會小於以買進成交的單量，一般會視為大戶(單量大)目前站在買方。 |

---
## 累賣成筆
**Path:** 報價欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 累賣成筆 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("累賣成筆"); Value1 = GetQuote("CulSellTicks"); Value1 = q_CulSellTicks; ``` |
| 欄位分類 | 量能 |
| 單位 | 筆 |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 期貨 選擇權 |
| 說明 | 開盤迄今以賣出成交的總筆數。也可以使用GetField("累賣成筆", "D")來表示。  可以使用此數值(與累買成筆)來估算盤中的趨勢：如果累賣成筆>累買成筆的話，代表每一筆以賣出成交的單量會小於以買進成交的單量，一般會視為大戶(單量大)目前站在買方。 |

---
## 總成交次數
**Path:** 報價欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 總成交次數 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("總成交次數"); Value1 = GetQuote("TotalTicks"); Value1 = q_TotalTicks; ``` |
| 欄位分類 | 量能 |
| 單位 | 筆 |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 台股 期貨 選擇權 香港股票 大陸股票 美(股票) |
| 說明 | 當日到目前為止的成交明細資料總筆數。也可以使用 GetField("總成交次數","D") 來表示。 |

---
## 總量_日_
**Path:** 報價欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 總量(日) (報價欄位) |
| 語法 | ``` Value1 = GetQuote("總量(日)"); Value1 = GetQuote("DailyVolume"); Value1 = q_DailyVolume; ``` |
| 欄位分類 | 量能 |
| 單位 | 台股:張；大盤/指數:元；期貨/選擇權:口；其他市場股票:股 |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 全部 |
| 說明 | 當日的總成交量。也可以使用 GetField("Volume", "D") 來表示。 數值單位依照商品類型而異。 |

---
## 量比
**Path:** 報價欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 量比 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("量比"); Value1 = GetQuote("VolumeRatio"); Value1 = q_VolumeRatio; ``` |
| 欄位分類 | 量能 |
| 單位 | 比值 |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 台(股票) 香港股票 大陸股票 |
| 說明 | 當日估計量對比昨日成交量的放大比例。 計算公式為：估計量 / 昨量。  數值大於1時表示今日收盤的成交量可能會比昨日成交量大，數值越大代表放大程度可能越大。 |

---
## 開盤委買
**Path:** 報價欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 開盤委買 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("開盤委買"); Value1 = GetQuote("BoughtLotsAtOpen"); Value1 = q_BoughtLotsAtOpen; ``` |
| 欄位分類 | 量能 |
| 單位 | 大盤/類股/台股:張；期貨/選擇權:口 |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 台股 大盤 類股指數 期貨 選擇權 |
| 說明 | 開盤時委託買進的數量。也可以使用 GetField("開盤委買", "D") 來表示。  台股的數值是開盤第一筆成交前五檔委買的數量加總。  類股商品（細產業指標、主題指標）的計算方式則是成分股的開盤委買的加總。  大盤/期貨/選擇權的數值則是交易所公布的委買委賣資料的第一筆數值。大盤的數值會扣除權證。 |

---
## 開盤委賣
**Path:** 報價欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 開盤委賣 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("開盤委賣"); Value1 = GetQuote("SoldLotsAtOpen"); Value1 = q_SoldLotsAtOpen; ``` |
| 欄位分類 | 量能 |
| 單位 | 大盤/類股/台股:張；期貨/選擇權:口 |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 台股 大盤 類股指數 期貨 選擇權 |
| 說明 | 開盤時委託賣出的數量。也可以使用 GetField("開盤委賣", "D") 來表示。  台股的數值是開盤第一筆成交前五檔委賣的數量加總。  類股商品（細產業指標、主題指標）的計算方式則是成分股的開盤委賣的加總。  大盤/期貨/選擇權的數值則是交易所公布的委買委賣資料的第一筆數值。大盤的數值會扣除權證。 |

---
## 開盤買均
**Path:** 報價欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 開盤買均 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("開盤買均"); Value1 = GetQuote("BoughtAverageAtOpen"); Value1 = q_BoughtAverageAtOpen; ``` |
| 欄位分類 | 量能 |
| 單位 | 大盤:張；期貨/選擇權:口 |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 大盤 期貨 選擇權 |
| 說明 | 開盤時每筆委託買進張數/口數。 計算公式：開盤委買／開盤買筆。  大盤的數值是整體市場扣除權證後的數值。 |

---
## 開盤買筆
**Path:** 報價欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 開盤買筆 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("開盤買筆"); Value1 = GetQuote("BoughtTickAtOpen"); Value1 = q_BoughtTickAtOpen; ``` |
| 欄位分類 | 量能 |
| 單位 | 筆 |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 大盤 期貨 選擇權 |
| 說明 | 開盤時委託買進的筆數。也可以使用GetField("開盤買筆", "D")來表示。  大盤的數值會扣除權證。 |

---
## 開盤賣均
**Path:** 報價欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 開盤賣均 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("開盤賣均"); Value1 = GetQuote("SoldAverageAtOpen"); Value1 = q_SoldAverageAtOpen; ``` |
| 欄位分類 | 量能 |
| 單位 | 大盤:張；期貨/選擇權:口 |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 大盤 期貨 選擇權 |
| 說明 | 開盤時每筆委託賣出張數/口數。 計算公式：開盤委賣／開盤賣筆。  大盤的數值是整體市場扣除權證後的數值。 |

---
## 開盤賣筆
**Path:** 報價欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 開盤賣筆 (報價欄位) |
| 語法 | ``` Value1 = GetQuote("開盤賣筆"); Value1 = GetQuote("SoldTickAtOpen"); Value1 = q_SoldTickAtOpen; ``` |
| 欄位分類 | 量能 |
| 單位 | 筆 |
| 格式 | 數值 |
| 支援腳本 | 警示 交易 函數 |
| 支援商品 | 大盤 期貨 選擇權 |
| 說明 | 開盤時委託賣出的筆數。也可以使用GetField("開盤賣筆", "D")來表示。  大盤的數值會扣除權證。 |

---
## ArrayLinearRegSlope
**Path:** 系統函數 > Array函數

ArrayLinearRegSlope -  (系統函數)
Array函數

語法：

利用陣列來計算的線性迴歸的斜率。  
回傳陣列=ArrayLinearRegSlope(陣列,期數)  
傳入二個參數:  
- 第一個參數是陣列。  
- 第二個參數是期數。

說明：

利用最小平方法計算線性迴歸的斜率。

---
## ArrayMASeries
**Path:** 系統函數 > Array函數

ArrayMASeries -  (系統函數)
Array函數

語法：

將均線數值序列轉成陣列。  
回傳陣列=ArrayMASeries(數列,數列期數,陣列)  
傳入三個參數:  
- 第一個參數是數列。  
- 第二個參數是數列期數。  
- 第三個參數是陣列。

說明：

將某個數值序列的均線轉成陣列型態。

---
## ArraySeries
**Path:** 系統函數 > Array函數

ArraySeries -  (系統函數)
Array函數

語法：

將數值序列轉成陣列。  
回傳陣列=ArraySeries(數列,數列期數,陣列)  
傳入三個參數:  
- 第一個參數是數列。  
- 第二個參數是數列期數。  
- 第三個參數是陣列。

說明：

將某個數值序列轉成陣列型態。

---
## ArrayXDaySeries
**Path:** 系統函數 > Array函數

ArrayXDaySeries -  (系統函數)
Array函數

語法：

以陣列儲存跨頻率的序列值。  
回傳陣列=ArrayXDaySeries(序列,最大引用筆數,陣列)  
傳入三個參數:  
- 第一個參數是序列。  
- 第二個參數是最大引用筆數。  
- 第三個參數是陣列。

說明：

以Array儲存跨頻率的序列值，傳入一個序列。

範例：

```
Array: CloseArray[](0);
ArrayXDaySeries(GetField("收盤價","D"),SBB_length,_DayValue);
```

---
## EnterMarketCloseTime
**Path:** 系統函數 > 交易相關

EnterMarketCloseTime -  (系統函數)
交易相關

語法：

回傳布林值。  
判斷是否已經進入收盤階段：用來判斷不再進場 or 平倉當日部位。  
使用時須傳入N，代表在最後可以送單前N分鐘就認定進入收盤階段，  
例如如果傳1，而且是台股的話, 那在13:24:00就會回傳True，代表已經進入收盤階段。  
※請注意：這個函數只支援台股, 以及台灣期貨市場內的常用商品, 也不考慮部分外匯期貨 or 其他市場期貨, 例如東証指。

說明：

可至 XQ 系統的 XScrip 編輯器，開啟「系統-交易相關」資料夾底下的 EnterMarketCloseTime 函數腳本，查看詳細函數語法撰寫邏輯。

---
## calcvwapdistribution
**Path:** 系統函數 > 交易相關

calcvwapdistribution -  (系統函數)
交易相關

語法：

計算過去N日的VWAP分佈。  
calcvwapdistribution(計算天數，開始時間，結束時間，一個array)

說明：

計算過去N日的VWAP分佈

請傳入

* 計算天數
* 開始時間, 例如091000
* 結束時間, 例如095900 (請注意請以1分K的Time為基準)
* 一個array, 用來儲存上述指定區間內每分鐘的累積成交量分佈%,
  + CalcVWAPDistribution會自動設定array的大小,
  + array[1]是從開始時間後第1分鐘的累計成交量%, array[2]是從開始時間到後第2分鐘的累計成交量%, etc.
  + 請注意這是一個累積的數值, 例如array[1] = 2.5, array[2] = 5.4, array[3] = 7.0, ... array[最後一個]=100.0,

---
## AvgPrice
**Path:** 系統函數 > 價格取得

AvgPrice -  (系統函數)
價格取得

語法：

取得利用K棒的開高低收所計算出的平均價格。  
回傳數值=AvgPrice  
---  
※請注意：AvgPrice 與 getfield("AvgPrice") 是不同的數值，  
getfield("AvgPrice") 是今日的平均成交價，也就是「當日每筆的成交金額加總／當日成交量」

說明：

計算公式：

平均價格 = (當期開盤價 + 當期最高價 + 當期最低價 + 當期收盤價)/4

範例：

```
plot1(avgprice);    //繪製當天平均價格的連線
plot2(avgprice[1]); //繪製前一天平均價格的連線
```

---
## CloseD
**Path:** 系統函數 > 價格取得

CloseD -  (系統函數)
價格取得

語法：

取得日線的收盤價。  
僅限使用於日線以下之頻率。  
回傳數值=CloseD(期別)  
傳入一個參數:  
- 第一個參數是期別，和序列引用定義相同，0表當期、1表前一期...依此類推。

說明：

當使用頻率小於日線時，用CloseD可以找到某期的日收盤價。

範例：

```
plot1(CloseD(0)); //繪製當日收盤價的連線
plot2(CloseD(1)); //繪製前一日收盤價的連線
```

相關的函數包含:

* [OpenD](api?a=opend&b=sys), [OpenW](api?a=openw&b=sys), [OpenM](api?a=openm&b=sys), [OpenQ](api?a=openq&b=sys), [OpenH](api?a=openh&b=sys), [OpenY](api?a=openy&b=sys)
* [HighD](api?a=highd&b=sys), [HighW](api?a=highw&b=sys), [HighM](api?a=highm&b=sys), [HighQ](api?a=highq&b=sys), [HighH](api?a=highh&b=sys), [HighY](api?a-highy&b=sys)
* [LowD](api?a=lowd&b=sys), [LowW](api?a=loww&b=sys), [LowM](api?a=lowm&b=sys), [LowQ](api?a=lowq&b=sys), [LowH](api?a=lowh&b=sys), [LowY](api?a=lowy&b=sys)
* [CloseD](api?a=closed&b=sys), [CloseW](api?a=closew&b=sys), [CloseM](api?a=closem&b=sys), [CloseQ](api?a=closeq&b=sys), [CloseH](api?a=closeh&b=sys), [CloseY](api?a=closey&b=sys)

---
## CloseH
**Path:** 系統函數 > 價格取得

CloseH -  (系統函數)
價格取得

語法：

取得半年線的收盤價。  
僅限使用於半年線以下之頻率。  
數值=CloseH(期別)  
傳入一個參數:  
- 第一個參數是期別，和序列引用定義相同，0表當期、1表前一期...依此類推。

說明：

當使用頻率小於半年線時，用CloseH可以找到某期的半年收盤價。

範例：

```
plot1(CloseH(0)); //繪製當期半年線收盤價的連線
```

相關的函數包含:

* [OpenD](api?a=opend&b=sys), [OpenW](api?a=openw&b=sys), [OpenM](api?a=openm&b=sys), [OpenQ](api?a=openq&b=sys), [OpenH](api?a=openh&b=sys), [OpenY](api?a=openy&b=sys)
* [HighD](api?a=highd&b=sys), [HighW](api?a=highw&b=sys), [HighM](api?a=highm&b=sys), [HighQ](api?a=highq&b=sys), [HighH](api?a=highh&b=sys), [HighY](api?a-highy&b=sys)
* [LowD](api?a=lowd&b=sys), [LowW](api?a=loww&b=sys), [LowM](api?a=lowm&b=sys), [LowQ](api?a=lowq&b=sys), [LowH](api?a=lowh&b=sys), [LowY](api?a=lowy&b=sys)
* [CloseD](api?a=closed&b=sys), [CloseW](api?a=closew&b=sys), [CloseM](api?a=closem&b=sys), [CloseQ](api?a=closeq&b=sys), [CloseH](api?a=closeh&b=sys), [CloseY](api?a=closey&b=sys)

---
## CloseM
**Path:** 系統函數 > 價格取得

CloseM -  (系統函數)
價格取得

語法：

取得月線的收盤價。  
僅限使用於月線以下之頻率。  
回傳數值=CloseM(期別)  
傳入一個參數:  
- 第一個參數是期別，和序列引用定義相同，0表當期、1表前一期...依此類推。

說明：

當使用頻率小於月線時，用CloseM可以找到某期的月收盤價。

範例：

```
plot1(CloseM(0)); //繪製當月收盤價的連線
plot2(CloseM(1)); //繪製前一月收盤價的連線
```

相關的函數包含:

* [OpenD](api?a=opend&b=sys), [OpenW](api?a=openw&b=sys), [OpenM](api?a=openm&b=sys), [OpenQ](api?a=openq&b=sys), [OpenH](api?a=openh&b=sys), [OpenY](api?a=openy&b=sys)
* [HighD](api?a=highd&b=sys), [HighW](api?a=highw&b=sys), [HighM](api?a=highm&b=sys), [HighQ](api?a=highq&b=sys), [HighH](api?a=highh&b=sys), [HighY](api?a-highy&b=sys)
* [LowD](api?a=lowd&b=sys), [LowW](api?a=loww&b=sys), [LowM](api?a=lowm&b=sys), [LowQ](api?a=lowq&b=sys), [LowH](api?a=lowh&b=sys), [LowY](api?a=lowy&b=sys)
* [CloseD](api?a=closed&b=sys), [CloseW](api?a=closew&b=sys), [CloseM](api?a=closem&b=sys), [CloseQ](api?a=closeq&b=sys), [CloseH](api?a=closeh&b=sys), [CloseY](api?a=closey&b=sys)

---
## CloseQ
**Path:** 系統函數 > 價格取得

CloseQ -  (系統函數)
價格取得

語法：

取得季線的收盤價。  
僅限使用於季線以下之頻率。  
回傳數值=CloseQ(期別)  
傳入一個參數:  
- 第一個參數是期別，和序列引用定義相同，0表當期、1表前一期...依此類推。

說明：

當使用頻率小於季線時，用CloseQ可以找到某期的季收盤價。

範例：

```
plot1(CloseQ(0)); //繪製當季收盤價的連線
plot2(CloseQ(1)); //繪製前一季收盤價的連線
```

相關的函數包含:

* [OpenD](api?a=opend&b=sys), [OpenW](api?a=openw&b=sys), [OpenM](api?a=openm&b=sys), [OpenQ](api?a=openq&b=sys), [OpenH](api?a=openh&b=sys), [OpenY](api?a=openy&b=sys)
* [HighD](api?a=highd&b=sys), [HighW](api?a=highw&b=sys), [HighM](api?a=highm&b=sys), [HighQ](api?a=highq&b=sys), [HighH](api?a=highh&b=sys), [HighY](api?a-highy&b=sys)
* [LowD](api?a=lowd&b=sys), [LowW](api?a=loww&b=sys), [LowM](api?a=lowm&b=sys), [LowQ](api?a=lowq&b=sys), [LowH](api?a=lowh&b=sys), [LowY](api?a=lowy&b=sys)
* [CloseD](api?a=closed&b=sys), [CloseW](api?a=closew&b=sys), [CloseM](api?a=closem&b=sys), [CloseQ](api?a=closeq&b=sys), [CloseH](api?a=closeh&b=sys), [CloseY](api?a=closey&b=sys)

---
## CloseW
**Path:** 系統函數 > 價格取得

CloseW -  (系統函數)
價格取得

語法：

取得週線的收盤價。  
僅限使用於週線以下之頻率。  
回傳數值=CloseW(期別)  
傳入一個參數:  
- 第一個參數是期別，和序列引用定義相同，0表當期、1表前一期...依此類推。

說明：

當使用頻率小於週線時，用CloseW可以找到某期的週收盤價。

範例：

```
plot1(CloseW(0)); //繪製當週收盤價的連線
plot2(CloseW(1)); //繪製前一週收盤價的連線
```

相關的函數包含:

* [OpenD](api?a=opend&b=sys), [OpenW](api?a=openw&b=sys), [OpenM](api?a=openm&b=sys), [OpenQ](api?a=openq&b=sys), [OpenH](api?a=openh&b=sys), [OpenY](api?a=openy&b=sys)
* [HighD](api?a=highd&b=sys), [HighW](api?a=highw&b=sys), [HighM](api?a=highm&b=sys), [HighQ](api?a=highq&b=sys), [HighH](api?a=highh&b=sys), [HighY](api?a-highy&b=sys)
* [LowD](api?a=lowd&b=sys), [LowW](api?a=loww&b=sys), [LowM](api?a=lowm&b=sys), [LowQ](api?a=lowq&b=sys), [LowH](api?a=lowh&b=sys), [LowY](api?a=lowy&b=sys)
* [CloseD](api?a=closed&b=sys), [CloseW](api?a=closew&b=sys), [CloseM](api?a=closem&b=sys), [CloseQ](api?a=closeq&b=sys), [CloseH](api?a=closeh&b=sys), [CloseY](api?a=closey&b=sys)

---
## CloseY
**Path:** 系統函數 > 價格取得

CloseY -  (系統函數)
價格取得

語法：

取得年線的收盤價。  
僅限使用於年線以下之頻率。  
回傳數值=CloseY(期別)  
傳入一個參數:  
- 第一個參數是期別，和序列引用定義相同，0表當期、1表前一期...依此類推。

說明：

當使用頻率小於年線時，用CloseY可以找到某期的年收盤價。

範例：

```
plot1(CloseY(0)); //繪製當年收盤價的連線
plot2(CloseY(1)); //繪製前一年收盤價的連線
```

相關的函數包含:

* [OpenD](api?a=opend&b=sys), [OpenW](api?a=openw&b=sys), [OpenM](api?a=openm&b=sys), [OpenQ](api?a=openq&b=sys), [OpenH](api?a=openh&b=sys), [OpenY](api?a=openy&b=sys)
* [HighD](api?a=highd&b=sys), [HighW](api?a=highw&b=sys), [HighM](api?a=highm&b=sys), [HighQ](api?a=highq&b=sys), [HighH](api?a=highh&b=sys), [HighY](api?a-highy&b=sys)
* [LowD](api?a=lowd&b=sys), [LowW](api?a=loww&b=sys), [LowM](api?a=lowm&b=sys), [LowQ](api?a=lowq&b=sys), [LowH](api?a=lowh&b=sys), [LowY](api?a=lowy&b=sys)
* [CloseD](api?a=closed&b=sys), [CloseW](api?a=closew&b=sys), [CloseM](api?a=closem&b=sys), [CloseQ](api?a=closeq&b=sys), [CloseH](api?a=closeh&b=sys), [CloseY](api?a=closey&b=sys)

---
## FastHighest
**Path:** 系統函數 > 價格取得

FastHighest -  (系統函數)
價格取得

語法：

計算序列資料的最大值。  
回傳數值=FastHighest(數列,期數)  
傳入二個參數:  
- 第一個參數是數列，通常是開高低收的價格數列。  
- 第二個參數是期數。

說明：

以最新一筆資料為基準點，輸入要計算的期數，然後計算過去期數的極大值。

FastHighest函數為[Highest](api?a=highest&b=sys)函數的快速計算版本。

在運算極值的時候，會用 For 迴圈往前抓到極值紀錄後，之後執行腳本就會用當下的序列資料與紀錄極值相比，若大於紀錄極值則更新輸出極值與輸出極值的相對K棒位置。因為不會每根 K 棒都用 For 迴圈往前抓極值，所以腳本運行會更加快速。

範例：

```
plot1(FastHighest(high,5));    //繪製5期最高價的最大值的連線
```

---
## FastLowest
**Path:** 系統函數 > 價格取得

FastLowest -  (系統函數)
價格取得

語法：

計算序列資料的最小值。  
回傳數值=FastLowest(數列,期數)  
傳入二個參數:  
- 第一個參數是數列，通常是開高低收的價格數列。  
- 第二個參數是期數。

說明：

以最新一筆資料為基準點，輸入要計算的期數，然後計算過去期數的極小值。

FastLowest函數為[Lowest](api?a=lowest&b=sys)函數的快速計算版本。

在運算極值的時候，會用 For 迴圈往前抓到極值紀錄後，之後執行腳本就會用當下的序列資料與紀錄極值相比，若大於紀錄極值則更新輸出極值與輸出極值的相對K棒位置。因為不會每根 K 棒都用 For 迴圈往前抓極值，所以腳本運行會更加快速。

範例：

```
plot1(FastLowest(low,5));    //繪製5期最低價的最小值的連線
```

---
## HighD
**Path:** 系統函數 > 價格取得

HighD -  (系統函數)
價格取得

語法：

取得日線的最高價。  
僅限使用於日線以下之頻率。  
回傳數值=HighD(期別)  
傳入一個參數:  
- 第一個參數是期別，和序列引用定義相同，0表當期、1表前一期...依此類推。

說明：

當使用頻率小於日線時，用HighD可以找到某期的日最高價。

範例：

```
plot1(HighD(0)); //繪製當日最高價的連線
plot2(HighD(1)); //繪製前一日最高價的連線
```

相關的函數包含:

* [OpenD](api?a=opend&b=sys), [OpenW](api?a=openw&b=sys), [OpenM](api?a=openm&b=sys), [OpenQ](api?a=openq&b=sys), [OpenH](api?a=openh&b=sys), [OpenY](api?a=openy&b=sys)
* [HighD](api?a=highd&b=sys), [HighW](api?a=highw&b=sys), [HighM](api?a=highm&b=sys), [HighQ](api?a=highq&b=sys), [HighH](api?a=highh&b=sys), [HighY](api?a-highy&b=sys)
* [LowD](api?a=lowd&b=sys), [LowW](api?a=loww&b=sys), [LowM](api?a=lowm&b=sys), [LowQ](api?a=lowq&b=sys), [LowH](api?a=lowh&b=sys), [LowY](api?a=lowy&b=sys)
* [CloseD](api?a=closed&b=sys), [CloseW](api?a=closew&b=sys), [CloseM](api?a=closem&b=sys), [CloseQ](api?a=closeq&b=sys), [CloseH](api?a=closeh&b=sys), [CloseY](api?a=closey&b=sys)

---
## HighH
**Path:** 系統函數 > 價格取得

HighH -  (系統函數)
價格取得

語法：

取得半年線的最高價。  
僅限使用於半年線以下之頻率。  
回傳數值=HighH(期別)  
傳入一個參數:  
- 第一個參數是期別，和序列引用定義相同，0表當期、1表前一期...依此類推。

說明：

當使用頻率小於半年線時，用HighH可以找到某期的半年最高價。

範例：

```
plot1(HighH(0)); //繪製當期半年線最高價的連線
```

---
## HighM
**Path:** 系統函數 > 價格取得

HighM -  (系統函數)
價格取得

語法：

取得月線的最高價。  
僅限使用於月線以下之頻率。  
回傳數值=HighM(期別)  
傳入一個參數:  
- 第一個參數是期別，和序列引用定義相同，0表當期、1表前一期...依此類推。

說明：

當使用頻率小於月線時，用HighM可以找到某期的月最高價。

範例：

```
plot1(HighM(0)); //繪製當月最高價的連線
plot2(HighM(1)); //繪製前一月最高價的連線
```

相關的函數包含:

* [OpenD](api?a=opend&b=sys), [OpenW](api?a=openw&b=sys), [OpenM](api?a=openm&b=sys), [OpenQ](api?a=openq&b=sys), [OpenH](api?a=openh&b=sys), [OpenY](api?a=openy&b=sys)
* [HighD](api?a=highd&b=sys), [HighW](api?a=highw&b=sys), [HighM](api?a=highm&b=sys), [HighQ](api?a=highq&b=sys), [HighH](api?a=highh&b=sys), [HighY](api?a-highy&b=sys)
* [LowD](api?a=lowd&b=sys), [LowW](api?a=loww&b=sys), [LowM](api?a=lowm&b=sys), [LowQ](api?a=lowq&b=sys), [LowH](api?a=lowh&b=sys), [LowY](api?a=lowy&b=sys)
* [CloseD](api?a=closed&b=sys), [CloseW](api?a=closew&b=sys), [CloseM](api?a=closem&b=sys), [CloseQ](api?a=closeq&b=sys), [CloseH](api?a=closeh&b=sys), [CloseY](api?a=closey&b=sys)

---
## HighQ
**Path:** 系統函數 > 價格取得

HighQ -  (系統函數)
價格取得

語法：

取得季線的最高價。  
僅限使用於季線以下之頻率。  
回傳數值=HighQ(期別)  
傳入一個參數:  
- 第一個參數是期別，和序列引用定義相同，0表當期、1表前一期...依此類推。

說明：

當使用頻率小於季線時，用HighQ可以找到某期的季最高價。

範例：

```
plot1(HighQ(0)); //繪製當季最高價的連線
plot2(HighQ(1)); //繪製前一季最高價的連線
```

相關的函數包含:

* [OpenD](api?a=opend&b=sys), [OpenW](api?a=openw&b=sys), [OpenM](api?a=openm&b=sys), [OpenQ](api?a=openq&b=sys), [OpenH](api?a=openh&b=sys), [OpenY](api?a=openy&b=sys)
* [HighD](api?a=highd&b=sys), [HighW](api?a=highw&b=sys), [HighM](api?a=highm&b=sys), [HighQ](api?a=highq&b=sys), [HighH](api?a=highh&b=sys), [HighY](api?a-highy&b=sys)
* [LowD](api?a=lowd&b=sys), [LowW](api?a=loww&b=sys), [LowM](api?a=lowm&b=sys), [LowQ](api?a=lowq&b=sys), [LowH](api?a=lowh&b=sys), [LowY](api?a=lowy&b=sys)
* [CloseD](api?a=closed&b=sys), [CloseW](api?a=closew&b=sys), [CloseM](api?a=closem&b=sys), [CloseQ](api?a=closeq&b=sys), [CloseH](api?a=closeh&b=sys), [CloseY](api?a=closey&b=sys)

---
## HighW
**Path:** 系統函數 > 價格取得

HighW -  (系統函數)
價格取得

語法：

取得週線的最高價。  
僅限使用於週線以下之頻率。  
回傳數值=HighW(期別)  
傳入一個參數:  
- 第一個參數是期別，和序列引用定義相同，0表當期、1表前一期...依此類推。

說明：

當使用頻率小於週線時，用HighW可以找到某期的週最高價。

範例：

```
plot1(HighW(0)); //繪製當週最高價的連線
plot2(HighW(1)); //繪製前一週最高價的連線
```

相關的函數包含:

* [OpenD](api?a=opend&b=sys), [OpenW](api?a=openw&b=sys), [OpenM](api?a=openm&b=sys), [OpenQ](api?a=openq&b=sys), [OpenH](api?a=openh&b=sys), [OpenY](api?a=openy&b=sys)
* [HighD](api?a=highd&b=sys), [HighW](api?a=highw&b=sys), [HighM](api?a=highm&b=sys), [HighQ](api?a=highq&b=sys), [HighH](api?a=highh&b=sys), [HighY](api?a-highy&b=sys)
* [LowD](api?a=lowd&b=sys), [LowW](api?a=loww&b=sys), [LowM](api?a=lowm&b=sys), [LowQ](api?a=lowq&b=sys), [LowH](api?a=lowh&b=sys), [LowY](api?a=lowy&b=sys)
* [CloseD](api?a=closed&b=sys), [CloseW](api?a=closew&b=sys), [CloseM](api?a=closem&b=sys), [CloseQ](api?a=closeq&b=sys), [CloseH](api?a=closeh&b=sys), [CloseY](api?a=closey&b=sys)

---
## HighY
**Path:** 系統函數 > 價格取得

HighY -  (系統函數)
價格取得

語法：

取得年線的最高價。  
僅限使用於年線以下之頻率。  
回傳數值=HighY(期別)  
傳入一個參數:  
- 第一個參數是期別，和序列引用定義相同，0表當期、1表前一期...依此類推。

說明：

當使用頻率小於年線時，用HighY可以找到某期的年最高價。

範例：

```
plot1(HighY(0)); //繪製當年最高價的連線
plot2(HighY(1)); //繪製前一年最高價的連線
```

相關的函數包含:

* [OpenD](api?a=opend&b=sys), [OpenW](api?a=openw&b=sys), [OpenM](api?a=openm&b=sys), [OpenQ](api?a=openq&b=sys), [OpenH](api?a=openh&b=sys), [OpenY](api?a=openy&b=sys)
* [HighD](api?a=highd&b=sys), [HighW](api?a=highw&b=sys), [HighM](api?a=highm&b=sys), [HighQ](api?a=highq&b=sys), [HighH](api?a=highh&b=sys), [HighY](api?a-highy&b=sys)
* [LowD](api?a=lowd&b=sys), [LowW](api?a=loww&b=sys), [LowM](api?a=lowm&b=sys), [LowQ](api?a=lowq&b=sys), [LowH](api?a=lowh&b=sys), [LowY](api?a=lowy&b=sys)
* [CloseD](api?a=closed&b=sys), [CloseW](api?a=closew&b=sys), [CloseM](api?a=closem&b=sys), [CloseQ](api?a=closeq&b=sys), [CloseH](api?a=closeh&b=sys), [CloseY](api?a=closey&b=sys)

---
## Highest
**Path:** 系統函數 > 價格取得

Highest -  (系統函數)
價格取得

語法：

計算序列資料的最大值。  
回傳數值=Highest(數列,期數)  
傳入二個參數:  
- 第一個參數是數列，通常是開高低收的價格數列。  
- 第二個參數是期數。

說明：

以最新一筆資料為基準點，輸入要計算的期數，然後計算過去期數的極大值。

Highest函數與[FastHighest](api?a=FastHighest&b=sys)函數的運算方式一致，都是用 [Extremes](api?a=Extremes&b=sys) 函數抓極大值。

範例：

```
plot1(Highest(high,5));    //繪製5期最高價的最大值的連線
```

---
## LowD
**Path:** 系統函數 > 價格取得

LowD -  (系統函數)
價格取得

語法：

取得日線的最低價。  
僅限使用於日線以下之頻率。  
回傳數值=LowD(期別)  
傳入一個參數:  
- 第一個參數是期別，和序列引用定義相同，0表當期、1表前一期...依此類推。

說明：

當使用頻率小於日線時，用LowD可以找到某期的日最低價。

範例：

```
plot1(LowD(0)); //繪製當日最低價的連線
plot2(LowD(1)); //繪製前一日最低價的連線
```

相關的函數包含:

* [OpenD](api?a=opend&b=sys), [OpenW](api?a=openw&b=sys), [OpenM](api?a=openm&b=sys), [OpenQ](api?a=openq&b=sys), [OpenH](api?a=openh&b=sys), [OpenY](api?a=openy&b=sys)
* [HighD](api?a=highd&b=sys), [HighW](api?a=highw&b=sys), [HighM](api?a=highm&b=sys), [HighQ](api?a=highq&b=sys), [HighH](api?a=highh&b=sys), [HighY](api?a-highy&b=sys)
* [LowD](api?a=lowd&b=sys), [LowW](api?a=loww&b=sys), [LowM](api?a=lowm&b=sys), [LowQ](api?a=lowq&b=sys), [LowH](api?a=lowh&b=sys), [LowY](api?a=lowy&b=sys)
* [CloseD](api?a=closed&b=sys), [CloseW](api?a=closew&b=sys), [CloseM](api?a=closem&b=sys), [CloseQ](api?a=closeq&b=sys), [CloseH](api?a=closeh&b=sys), [CloseY](api?a=closey&b=sys)

---
## LowH
**Path:** 系統函數 > 價格取得

LowH -  (系統函數)
價格取得

語法：

取得半年線的最低價。  
僅限使用於半年線以下之頻率。  
回傳數值=LowH(期別)  
傳入一個參數:  
- 第一個參數是期別，和序列引用定義相同，0表當期、1表前一期...依此類推。

說明：

當使用頻率小於半年線時，用LowH可以找到某期的半年最低價。

範例：

```
plot1(LowH(0)); //繪製當期半年線最低價的連線
```

相關的函數包含:

* [OpenD](api?a=opend&b=sys), [OpenW](api?a=openw&b=sys), [OpenM](api?a=openm&b=sys), [OpenQ](api?a=openq&b=sys), [OpenH](api?a=openh&b=sys), [OpenY](api?a=openy&b=sys)
* [HighD](api?a=highd&b=sys), [HighW](api?a=highw&b=sys), [HighM](api?a=highm&b=sys), [HighQ](api?a=highq&b=sys), [HighH](api?a=highh&b=sys), [HighY](api?a-highy&b=sys)
* [LowD](api?a=lowd&b=sys), [LowW](api?a=loww&b=sys), [LowM](api?a=lowm&b=sys), [LowQ](api?a=lowq&b=sys), [LowH](api?a=lowh&b=sys), [LowY](api?a=lowy&b=sys)
* [CloseD](api?a=closed&b=sys), [CloseW](api?a=closew&b=sys), [CloseM](api?a=closem&b=sys), [CloseQ](api?a=closeq&b=sys), [CloseH](api?a=closeh&b=sys), [CloseY](api?a=closey&b=sys)

---
## LowM
**Path:** 系統函數 > 價格取得

LowM -  (系統函數)
價格取得

語法：

取得月線的最低價。  
僅限使用於月線以下之頻率。  
回傳數值=LowM(期別)  
傳入一個參數:  
- 第一個參數是期別，和序列引用定義相同，0表當期、1表前一期...依此類推。

說明：

當使用頻率小於月線時，用LowM可以找到某期的月最低價。

範例：

```
plot1(LowM(0)); //繪製當月最低價的連線
plot2(LowM(1)); //繪製前一月最低價的連線
```

相關的函數包含:

* [OpenD](api?a=opend&b=sys), [OpenW](api?a=openw&b=sys), [OpenM](api?a=openm&b=sys), [OpenQ](api?a=openq&b=sys), [OpenH](api?a=openh&b=sys), [OpenY](api?a=openy&b=sys)
* [HighD](api?a=highd&b=sys), [HighW](api?a=highw&b=sys), [HighM](api?a=highm&b=sys), [HighQ](api?a=highq&b=sys), [HighH](api?a=highh&b=sys), [HighY](api?a-highy&b=sys)
* [LowD](api?a=lowd&b=sys), [LowW](api?a=loww&b=sys), [LowM](api?a=lowm&b=sys), [LowQ](api?a=lowq&b=sys), [LowH](api?a=lowh&b=sys), [LowY](api?a=lowy&b=sys)
* [CloseD](api?a=closed&b=sys), [CloseW](api?a=closew&b=sys), [CloseM](api?a=closem&b=sys), [CloseQ](api?a=closeq&b=sys), [CloseH](api?a=closeh&b=sys), [CloseY](api?a=closey&b=sys)

---
## LowQ
**Path:** 系統函數 > 價格取得

LowQ -  (系統函數)
價格取得

語法：

取得季線的最低價。  
僅限使用於季線以下之頻率。  
回傳數值=LowQ(期別)  
傳入一個參數:  
- 第一個參數是期別，和序列引用定義相同，0表當期、1表前一期...依此類推。

說明：

當使用頻率小於季線時，用LowQ可以找到某期的季最低價。

範例：

```
plot1(LowQ(0)); //繪製當季最低價的連線
plot2(LowQ(1)); //繪製前一季最低價的連線
```

相關的函數包含:

* [OpenD](api?a=opend&b=sys), [OpenW](api?a=openw&b=sys), [OpenM](api?a=openm&b=sys), [OpenQ](api?a=openq&b=sys), [OpenH](api?a=openh&b=sys), [OpenY](api?a=openy&b=sys)
* [HighD](api?a=highd&b=sys), [HighW](api?a=highw&b=sys), [HighM](api?a=highm&b=sys), [HighQ](api?a=highq&b=sys), [HighH](api?a=highh&b=sys), [HighY](api?a-highy&b=sys)
* [LowD](api?a=lowd&b=sys), [LowW](api?a=loww&b=sys), [LowM](api?a=lowm&b=sys), [LowQ](api?a=lowq&b=sys), [LowH](api?a=lowh&b=sys), [LowY](api?a=lowy&b=sys)
* [CloseD](api?a=closed&b=sys), [CloseW](api?a=closew&b=sys), [CloseM](api?a=closem&b=sys), [CloseQ](api?a=closeq&b=sys), [CloseH](api?a=closeh&b=sys), [CloseY](api?a=closey&b=sys)

---
## LowW
**Path:** 系統函數 > 價格取得

LowW -  (系統函數)
價格取得

語法：

取得週線的最低價。  
僅限使用於週線以下之頻率。  
回傳數值=LowW(期別)  
傳入一個參數:  
- 第一個參數是期別，和序列引用定義相同，0表當期、1表前一期...依此類推。

說明：

當使用頻率小於週線時，用LowW可以找到某期的週最低價。

範例：

```
plot1(LowW(0)); //繪製當週最低價的連線
plot2(LowW(1)); //繪製前一週最低價的連線
```

相關的函數包含:

* [OpenD](api?a=opend&b=sys), [OpenW](api?a=openw&b=sys), [OpenM](api?a=openm&b=sys), [OpenQ](api?a=openq&b=sys), [OpenH](api?a=openh&b=sys), [OpenY](api?a=openy&b=sys)
* [HighD](api?a=highd&b=sys), [HighW](api?a=highw&b=sys), [HighM](api?a=highm&b=sys), [HighQ](api?a=highq&b=sys), [HighH](api?a=highh&b=sys), [HighY](api?a-highy&b=sys)
* [LowD](api?a=lowd&b=sys), [LowW](api?a=loww&b=sys), [LowM](api?a=lowm&b=sys), [LowQ](api?a=lowq&b=sys), [LowH](api?a=lowh&b=sys), [LowY](api?a=lowy&b=sys)
* [CloseD](api?a=closed&b=sys), [CloseW](api?a=closew&b=sys), [CloseM](api?a=closem&b=sys), [CloseQ](api?a=closeq&b=sys), [CloseH](api?a=closeh&b=sys), [CloseY](api?a=closey&b=sys)

---
## LowY
**Path:** 系統函數 > 價格取得

LowY -  (系統函數)
價格取得

語法：

取得年線的最低價。  
僅限使用於年線以下之頻率。  
回傳數值=LowY(期別)  
傳入一個參數:  
- 第一個參數是期別，和序列引用定義相同，0表當期、1表前一期...依此類推。

說明：

當使用頻率小於年線時，用LowY可以找到某期的年最低價。

範例：

```
plot1(LowY(0)); //繪製當年最低價的連線
plot2(LowY(1)); //繪製前一年最低價的連線
```

相關的函數包含:

* [OpenD](api?a=opend&b=sys), [OpenW](api?a=openw&b=sys), [OpenM](api?a=openm&b=sys), [OpenQ](api?a=openq&b=sys), [OpenH](api?a=openh&b=sys), [OpenY](api?a=openy&b=sys)
* [HighD](api?a=highd&b=sys), [HighW](api?a=highw&b=sys), [HighM](api?a=highm&b=sys), [HighQ](api?a=highq&b=sys), [HighH](api?a=highh&b=sys), [HighY](api?a-highy&b=sys)
* [LowD](api?a=lowd&b=sys), [LowW](api?a=loww&b=sys), [LowM](api?a=lowm&b=sys), [LowQ](api?a=lowq&b=sys), [LowH](api?a=lowh&b=sys), [LowY](api?a=lowy&b=sys)
* [CloseD](api?a=closed&b=sys), [CloseW](api?a=closew&b=sys), [CloseM](api?a=closem&b=sys), [CloseQ](api?a=closeq&b=sys), [CloseH](api?a=closeh&b=sys), [CloseY](api?a=closey&b=sys)

---
## Lowest
**Path:** 系統函數 > 價格取得

Lowest -  (系統函數)
價格取得

語法：

計算序列資料的最小值。  
回傳數值=Lowest(數列,期數)  
傳入二個參數:  
- 第一個參數是數列，通常是開高低收的價格數列。  
- 第二個參數是期數。

說明：

以最新一筆資料為基準點，輸入要計算的期數，然後計算過去期數的極小值。

Lowest函數與[FastLowest](api?a=FastLowest&b=sys)函數的運算方式一致，都是用 [Extremes](api?a=Extremes&b=sys) 函數抓極小值。

範例：

```
plot1(Lowest(low,5));    //繪製5期最低價的最小值的連線
```

---
## OpenD
**Path:** 系統函數 > 價格取得

OpenD -  (系統函數)
價格取得

語法：

取得日線的開盤價。  
僅限使用於日線以下之頻率。  
回傳數值=OpenD(期別)  
傳入一個參數:  
- 第一個參數是期別，和序列引用定義相同，0表當期、1表前一期...依此類推。

說明：

當使用頻率小於日線時，用OpenD可以找到某期的日開盤價。

範例：

```
plot1(OpenD(0)); //繪製當日開盤價的連線
plot2(OpenD(1)); //繪製前一日開盤價的連線
```

相關的函數包含:

* [OpenD](api?a=opend&b=sys), [OpenW](api?a=openw&b=sys), [OpenM](api?a=openm&b=sys), [OpenQ](api?a=openq&b=sys), [OpenH](api?a=openh&b=sys), [OpenY](api?a=openy&b=sys)
* [HighD](api?a=highd&b=sys), [HighW](api?a=highw&b=sys), [HighM](api?a=highm&b=sys), [HighQ](api?a=highq&b=sys), [HighH](api?a=highh&b=sys), [HighY](api?a-highy&b=sys)
* [LowD](api?a=lowd&b=sys), [LowW](api?a=loww&b=sys), [LowM](api?a=lowm&b=sys), [LowQ](api?a=lowq&b=sys), [LowH](api?a=lowh&b=sys), [LowY](api?a=lowy&b=sys)
* [CloseD](api?a=closed&b=sys), [CloseW](api?a=closew&b=sys), [CloseM](api?a=closem&b=sys), [CloseQ](api?a=closeq&b=sys), [CloseH](api?a=closeh&b=sys), [CloseY](api?a=closey&b=sys)

---
## OpenH
**Path:** 系統函數 > 價格取得

OpenH -  (系統函數)
價格取得

語法：

取得半年線的開盤價。  
僅限使用於半年線以下之頻率。  
回傳數值=OpenH(期別)  
傳入一個參數:  
- 第一個參數是期別，和序列引用定義相同，0表當期、1表前一期...依此類推。

說明：

當使用頻率小於半年線時，用OpenH可以找到某期的半年開盤價。

範例：

```
plot1(OpenH(0)); //繪製當期半年線開盤價的連線
```

相關的函數包含:

* [OpenD](api?a=opend&b=sys), [OpenW](api?a=openw&b=sys), [OpenM](api?a=openm&b=sys), [OpenQ](api?a=openq&b=sys), [OpenH](api?a=openh&b=sys), [OpenY](api?a=openy&b=sys)
* [HighD](api?a=highd&b=sys), [HighW](api?a=highw&b=sys), [HighM](api?a=highm&b=sys), [HighQ](api?a=highq&b=sys), [HighH](api?a=highh&b=sys), [HighY](api?a-highy&b=sys)
* [LowD](api?a=lowd&b=sys), [LowW](api?a=loww&b=sys), [LowM](api?a=lowm&b=sys), [LowQ](api?a=lowq&b=sys), [LowH](api?a=lowh&b=sys), [LowY](api?a=lowy&b=sys)
* [CloseD](api?a=closed&b=sys), [CloseW](api?a=closew&b=sys), [CloseM](api?a=closem&b=sys), [CloseQ](api?a=closeq&b=sys), [CloseH](api?a=closeh&b=sys), [CloseY](api?a=closey&b=sys)

---
## OpenM
**Path:** 系統函數 > 價格取得

OpenM -  (系統函數)
價格取得

語法：

取得月線的開盤價。  
僅限使用於月線以下之頻率。  
回傳數值=OpenM(期別)  
傳入一個參數:  
- 第一個參數是期別，和序列引用定義相同，0表當期、1表前一期...依此類推。

說明：

當使用頻率小於月線時，用OpenM可以找到某期的月開盤價。

範例：

```
plot1(OpenM(0)); //繪製當月開盤價的連線
plot2(OpenM(1)); //繪製前一月開盤價的連線
```

相關的函數包含:

* [OpenD](api?a=opend&b=sys), [OpenW](api?a=openw&b=sys), [OpenM](api?a=openm&b=sys), [OpenQ](api?a=openq&b=sys), [OpenH](api?a=openh&b=sys), [OpenY](api?a=openy&b=sys)
* [HighD](api?a=highd&b=sys), [HighW](api?a=highw&b=sys), [HighM](api?a=highm&b=sys), [HighQ](api?a=highq&b=sys), [HighH](api?a=highh&b=sys), [HighY](api?a-highy&b=sys)
* [LowD](api?a=lowd&b=sys), [LowW](api?a=loww&b=sys), [LowM](api?a=lowm&b=sys), [LowQ](api?a=lowq&b=sys), [LowH](api?a=lowh&b=sys), [LowY](api?a=lowy&b=sys)
* [CloseD](api?a=closed&b=sys), [CloseW](api?a=closew&b=sys), [CloseM](api?a=closem&b=sys), [CloseQ](api?a=closeq&b=sys), [CloseH](api?a=closeh&b=sys), [CloseY](api?a=closey&b=sys)

---
## OpenQ
**Path:** 系統函數 > 價格取得

OpenQ -  (系統函數)
價格取得

語法：

取得季線的開盤價。  
僅限使用於季線以下之頻率。  
回傳數值=OpenQ(期別)  
傳入一個參數:  
- 第一個參數是期別，和序列引用定義相同，0表當期、1表前一期...依此類推。

說明：

當使用頻率小於季線時，用OpenQ可以找到某期的季開盤價。

範例：

```
plot1(OpenQ(0)); //繪製當季開盤價的連線
plot2(OpenQ(1)); //繪製前一季開盤價的連線
```

相關的函數包含:

* [OpenD](api?a=opend&b=sys), [OpenW](api?a=openw&b=sys), [OpenM](api?a=openm&b=sys), [OpenQ](api?a=openq&b=sys), [OpenH](api?a=openh&b=sys), [OpenY](api?a=openy&b=sys)
* [HighD](api?a=highd&b=sys), [HighW](api?a=highw&b=sys), [HighM](api?a=highm&b=sys), [HighQ](api?a=highq&b=sys), [HighH](api?a=highh&b=sys), [HighY](api?a-highy&b=sys)
* [LowD](api?a=lowd&b=sys), [LowW](api?a=loww&b=sys), [LowM](api?a=lowm&b=sys), [LowQ](api?a=lowq&b=sys), [LowH](api?a=lowh&b=sys), [LowY](api?a=lowy&b=sys)
* [CloseD](api?a=closed&b=sys), [CloseW](api?a=closew&b=sys), [CloseM](api?a=closem&b=sys), [CloseQ](api?a=closeq&b=sys), [CloseH](api?a=closeh&b=sys), [CloseY](api?a=closey&b=sys)

---
## OpenW
**Path:** 系統函數 > 價格取得

OpenW -  (系統函數)
價格取得

語法：

取得週線的開盤價。  
僅限使用於週線以下之頻率。  
回傳數值=OpenW(期別)  
傳入一個參數:  
- 第一個參數是期別，和序列引用定義相同，0表當期、1表前一期...依此類推。

說明：

當使用頻率小於週線時，用OpenW可以找到某期的週開盤價。

範例：

```
plot1(OpenW(0)); //繪製當週開盤價的連線
plot2(OpenW(1)); //繪製前一週開盤價的連線
```

相關的函數包含:

* [OpenD](api?a=opend&b=sys), [OpenW](api?a=openw&b=sys), [OpenM](api?a=openm&b=sys), [OpenQ](api?a=openq&b=sys), [OpenH](api?a=openh&b=sys), [OpenY](api?a=openy&b=sys)
* [HighD](api?a=highd&b=sys), [HighW](api?a=highw&b=sys), [HighM](api?a=highm&b=sys), [HighQ](api?a=highq&b=sys), [HighH](api?a=highh&b=sys), [HighY](api?a-highy&b=sys)
* [LowD](api?a=lowd&b=sys), [LowW](api?a=loww&b=sys), [LowM](api?a=lowm&b=sys), [LowQ](api?a=lowq&b=sys), [LowH](api?a=lowh&b=sys), [LowY](api?a=lowy&b=sys)
* [CloseD](api?a=closed&b=sys), [CloseW](api?a=closew&b=sys), [CloseM](api?a=closem&b=sys), [CloseQ](api?a=closeq&b=sys), [CloseH](api?a=closeh&b=sys), [CloseY](api?a=closey&b=sys)

---
## OpenY
**Path:** 系統函數 > 價格取得

OpenY -  (系統函數)
價格取得

語法：

取得年線的開盤價。  
僅限使用於年線以下之頻率。  
回傳數值=OpenY(期別)  
傳入一個參數:  
- 第一個參數是期別，和序列引用定義相同，0表當期、1表前一期...依此類推。

說明：

當使用頻率小於年線時，用OpenY可以找到某期的年開盤價。

範例：

```
plot1(OpenY(0)); //繪製當年開盤價的連線
plot2(OpenY(1)); //繪製前一年開盤價的連線
```

相關的函數包含:

* [OpenD](api?a=opend&b=sys), [OpenW](api?a=openw&b=sys), [OpenM](api?a=openm&b=sys), [OpenQ](api?a=openq&b=sys), [OpenH](api?a=openh&b=sys), [OpenY](api?a=openy&b=sys)
* [HighD](api?a=highd&b=sys), [HighW](api?a=highw&b=sys), [HighM](api?a=highm&b=sys), [HighQ](api?a=highq&b=sys), [HighH](api?a=highh&b=sys), [HighY](api?a-highy&b=sys)
* [LowD](api?a=lowd&b=sys), [LowW](api?a=loww&b=sys), [LowM](api?a=lowm&b=sys), [LowQ](api?a=lowq&b=sys), [LowH](api?a=lowh&b=sys), [LowY](api?a=lowy&b=sys)
* [CloseD](api?a=closed&b=sys), [CloseW](api?a=closew&b=sys), [CloseM](api?a=closem&b=sys), [CloseQ](api?a=closeq&b=sys), [CloseH](api?a=closeh&b=sys), [CloseY](api?a=closey&b=sys)

---
## TrueHigh
**Path:** 系統函數 > 價格取得

TrueHigh -  (系統函數)
價格取得

語法：

取得價格真實區間(TrueRange)的高點。  
回傳數值=TrueHigh

說明：

計算方法為比較當根K棒的高點與前根K棒的收盤價，取數值較大者。

範例：

```
plot1(TrueHigh);    //繪製當期真實區間高點的連線
plot2(TrueHigh[1]); //繪製前一期真實區間高點的連線
```

請參考 [TrueLow函數](api?a=truelow&b=sys)以及[TrueRange函數](api?a=truerange&b=sys)。

---
## TrueLow
**Path:** 系統函數 > 價格取得

TrueLow -  (系統函數)
價格取得

語法：

取得價格真實區間(TrueRange)的低點。  
回傳數值=TrueLow

說明：

計算方法為比較當根K棒的低點與前根K棒的收盤價，取數值較小者。

範例：

```
plot1(TrueLow);    //繪製當期真實區間低點的連線
plot2(TrueLow[1]); //繪製前一期真實區間低點的連線
```

請參考 [TrueHigh函數](api?a=truehigh&b=sys)以及[TrueRange函數](api?a=truerange&b=sys)。

---
## TypicalPrice
**Path:** 系統函數 > 價格取得

TypicalPrice -  (系統函數)
價格取得

語法：

傳回技術分析的典型價。  
回傳數值=TypicalPrice

說明：

計算公式：

典型價 = (當期最高價 + 當期最低價 + 當期收盤價)/3

範例：

```
plot1(TypicalPrice);    //繪製當期典型價的連線
plot2(TypicalPrice[1]); //繪製前一期典型價的連線
```

---
## WeightedClose
**Path:** 系統函數 > 價格取得

WeightedClose -  (系統函數)
價格取得

語法：

計算技術分析的加權平均收盤價。  
回傳數值=WeightedClose

說明：

加權平均價給予收盤價較大的權重，著名的MACD指標即是利用加權平均價做計算。

計算公式：

WeightedClose = (當期最高價 + 當期最低價 + 2\*當期收盤價)/4

範例：

```
plot1(WeightedClose);    //繪製當期加權平均收盤價的連線
plot2(WeightedClose[1]); //繪製前一期加權平均收盤價的連線
```

---
## Average
**Path:** 系統函數 > 價格計算

Average -  (系統函數)
價格計算

語法：

計算序列資料的移動平均。  
回傳數值=Average(數列,期數)  
傳入二個參數:  
- 第一個參數是數列，通常是開高低收的價格數列。  
- 第二個參數是期數。

說明：

計算序列資料的移動平均。

以最新一筆資料為基準點，輸入要計算的期數，然後計算過去期數的序列平均值。

範例：

```
value1 = Average(Close,5); //計算5期收盤價的移動平均
```

---
## AvgDeviation
**Path:** 系統函數 > 價格計算

AvgDeviation -  (系統函數)
價格計算

語法：

計算序列資料的平均差。  
回傳數值=AvgDeviation(數列,期數)  
傳入二個參數:  
- 第一個參數是數列，通常是開高低收的價格數列。  
- 第二個參數是期數。

說明：

以最新一筆資料為基準點，輸入要計算的期數，然後計算過去期數的平均差。

範例：

```
value1 = AvgDeviation(Close,5); //計算5期收盤價的平均差
```

---
## DwLimit
**Path:** 系統函數 > 價格計算

DwLimit -  (系統函數)
價格計算

語法：

依傳入之參考價計算跌停價。  
回傳數值=DwLimit(參考價)

說明：

範例：

```
value1 = DwLimit(CloseD(1)); //計算當期之跌停價
```

---
## EMA
**Path:** 系統函數 > 價格計算

EMA -  (系統函數)
價格計算

語法：

計算序列資料的XQ指數移動平均。  
回傳數值=EMA(數列,期數)  
傳入二個參數:  
- 第一個參數是數列，通常是開高低收的價格數列。  
- 第二個參數是期數。

說明：

以最新一筆資料為基準點，輸入要計算的期數，然後計算過去期數的XQ指數移動平均值。

XQ指數移動平均和一般指數移動平均（XAverage）的計算差異在於最前面幾期（小於平均期數）時的計算公式不同。

當K棒編號 ≦ 期數時：

當期均價 = (當期價格 + (前期均價\*(K棒編號-1)))/K棒編號 (換句話說，當期均價 = 這幾期的算數平均)

當K棒編號 > 期數時：

當期均價 = (2/(n+1))\*當期價格 + (n-1)/(n+1)\*前期均價

範例：

```
value1 = EMA(Close,5); //計算5期收盤價的XQ EMA
```

---
## Range
**Path:** 系統函數 > 價格計算

Range -  (系統函數)
價格計算

語法：

傳回當根K棒的高低價差。  
回傳數值=Range

說明：

計算公式：

Range = (當期最高價 - 當期最低價)

範例：

```
plot1(Range);    //繪製當期K棒高低價差的連線
plot2(Range[1]); //繪製前一期K棒高低價差的連線
```

---
## RateOfChange
**Path:** 系統函數 > 價格計算

RateOfChange -  (系統函數)
價格計算

語法：

計算序列資料的變化率。  
回傳數值=RateOfChange(數列,期數)  
傳入二個參數:  
- 第一個參數是數列，通常是開高低收的價格數列。  
- 第二個參數是期數。

說明：

以前N根K棒資料為基準點，計算至當根K棒的的數值變化率。

計算公式：

RateOfChange = ( 當期價格 ／ N期前價格 - 1 ) \* 100

範例：

```
value1 = RateOfChange(Close,5); //計算5期收盤價的變化率
```

---
## SimpleHighestBar
**Path:** 系統函數 > 價格計算

SimpleHighestBar -  (系統函數)
價格計算

語法：

取得區間內數列最大值的K棒位置。  
回傳數值=SimpleHighestBar(數列,期數)  
傳入二個參數:  
- 第一個參數是數列，通常是開高低收的價格數列。  
- 第二個參數是期數。

說明：

取得區間內數列最大值的K棒位置。
可以使用這個函數來取得欄位區間內新高的位置。

---
## SimpleLowestBar
**Path:** 系統函數 > 價格計算

SimpleLowestBar -  (系統函數)
價格計算

語法：

取得區間內數列最小值的K棒位置。  
回傳數值=SimpleLowestBar(數列,期數)  
傳入二個參數:  
- 第一個參數是數列，通常是開高低收的價格數列。  
- 第二個參數是期數。

說明：

取得區間內數列最小值的K棒位置。
可以使用這個函數來取得欄位區間內新低的位置。

---
## Summation
**Path:** 系統函數 > 價格計算

Summation -  (系統函數)
價格計算

語法：

計算序列資料的總和。  
回傳數值=Summation(數列,期數)  
傳入二個參數:  
- 第一個參數是數列，通常是開高低收的價格數列。  
- 第二個參數是期數。

說明：

以最新一筆資料為基準點，輸入要計算的期數，然後計算過去期數的數值總和。

範例：

```
value1 = Summation(Close,5); //計算5期收盤價的總和
```

---
## TrueRange
**Path:** 系統函數 > 價格計算

TrueRange -  (系統函數)
價格計算

語法：

傳回當根K棒的真實區間。  
回傳數值=TrueRange

說明：

真實區間是韋爾達（J. W. Wilder）於1978年發表於New Concepts in Technical Trading Systems中測量價格波動性的方法。

根據韋爾達的定義，真實區間為下列三項中的最大值：

* 當期最高價至最低價的幅度。
* 當期最低價與前期收盤價的幅度。
* 當期最高價至前期收盤價的幅度。

範例：

```
plot1(TrueRange);    //繪製當期K棒真實區間的連線
plot2(TrueRange[1]); //繪製前一期K棒真實區間的連線
```

---
## UpLimit
**Path:** 系統函數 > 價格計算

UpLimit -  (系統函數)
價格計算

語法：

依傳入之參考價計算漲停價。  
回傳數值=UpLimit(參考價)

說明：

範例：

```
value1 = UpLimit(CloseD(1)); //計算當期之漲停價
```

---
## WMA
**Path:** 系統函數 > 價格計算

WMA -  (系統函數)
價格計算

語法：

計算序列資料的加權移動平均。  
回傳數值=WMA(數列,期數)  
傳入二個參數:  
- 第一個參數是數列，通常是開高低收的價格數列。  
- 第二個參數是期數。

說明：

以最新一筆資料為基準點，輸入要計算的期數，然後計算過去期數的加權移動平均值。

計算公式：

WMAt=(Pt \* n + Pt-1 \* (n-1) + Pt-2 \* (n-2) + ... + Pt-n+1) / (n + (n-1) + (n-2) + ... + 1)

範例：

```
value1 = WMA(Close,5); //計算5期收盤價的加權移動平均
```

---
## XAverage
**Path:** 系統函數 > 價格計算

XAverage -  (系統函數)
價格計算

語法：

計算序列資料的指數移動平均。  
回傳數值=XAverage(數列,期數)  
傳入二個參數:  
- 第一個參數是數列，通常是開高低收的價格數列。  
- 第二個參數是期數。

說明：

以最新一筆資料為基準點，輸入要計算的期數，然後計算過去期數的指數移動平均值。

計算公式：

當期均價 = (2/(n+1))\*當期價格 + (n-1)/(n+1)\*前期均價

範例：

```
value1 = XAverage(Close,5); //計算5期收盤價的指數移動平均
```

---
## Extremes
**Path:** 系統函數 > 價格關係

Extremes -  (系統函數)
價格關係

語法：

計算序列資料的極大值或極小值。  
以最新一筆資料為基準點，輸入要計算的期數，然後計算過去期數的極值。  
回傳數值=Extremes(數列,期數,要計算極大值或極小值,輸出極值,輸出極值K棒相對位置)  
傳入五個參數:  
- 第一個參數是數列，通常是開高低收的價格數列。  
- 第二個參數是期數。  
- 第三個參數是要計算極大值或極小值；1為極大值、-1為極小值。  
- 第四個參數為傳址參數，會回傳計算完的極值。  
- 第五個參數是傳址參數，會回傳極值K棒相對於當期K棒的期數。

說明：

計算成功時回傳值為1，結果會回傳在第4、5個參數。

範例：

```
value1 = extremes(high,5,1,value2,value3); //計算5期最高價的極大值
plot1(value2);                             //繪製5期最高價的極大值的連線
```

---
## ExtremesArray
**Path:** 系統函數 > 價格關係

ExtremesArray -  (系統函數)
價格關係

語法：

計算陣列資料的極大值或極小值。  
回傳數值=ExtremesArray(陣列,陣列大小,要計算極大值或極小值,輸出極值,輸出極值的陣列索引值)  
傳入五個參數:  
- 第一個參數是要計算的陣列。  
- 第二個參數是陣列大小。  
- 第三個參數是要計算極大值或極小值；1為極大值、-1為極小值。  
- 第四個參數為傳址參數，會回傳計算完的極值。  
- 第五個參數是傳址參數，會回傳極值的陣列索引值。

說明：

計算成功時回傳值為1，結果會回傳在第4、5個參數。

範例：

```
Array: arrA[5](0); // 宣告arrA是一個有5個元素的陣列，初始值都是0


arrA[1] = 0;  arrA[2] = 10; arrA[3] = 20; arrA[4] = 30; arrA[5] = 40;


value1 = extremesarray(arrA,5,-1,value2,value3); //計算陣列arrA的極小值
plot1(value2);                                   //繪製陣列arrA的極小值的連線
```

---
## FastHighestBar
**Path:** 系統函數 > 價格關係

FastHighestBar -  (系統函數)
價格關係

語法：

計算序列資料的最大值的相對位置。  
回傳數值=FastHighestBar(數列,期數)  
傳入二個參數:  
- 第一個參數是數列，通常是開高低收的價格數列。  
- 第二個參數是期數。

說明：

以最新一筆資料為基準點，輸入要計算的期數，然後計算過去期數的極大值的相對位置。
如果有同樣兩個以上的極大值，則傳回離現在最近的那個。

FastHighestBar函數為[HighestBar](api?a=HighestBar&b=sys)函數的快速計算版本。

在運算極值的時候，會用 For 迴圈往前抓到極值紀錄後，之後執行腳本就會用當下的序列資料與紀錄極值相比，若大於紀錄極值則更新輸出極值與輸出極值的相對K棒位置。因為不會每根 K 棒都用 For 迴圈往前抓極值，所以腳本運行會更加快速。

範例：

```
plot1(FastHighestBar(high,5));    //繪製近5期最高的最高價相對位置的連線
```

---
## FastLowestBar
**Path:** 系統函數 > 價格關係

FastLowestBar -  (系統函數)
價格關係

語法：

計算序列資料的最小值的相對位置。  
回傳數值=FastLowestBar(數列,期數)  
傳入二個參數:  
- 第一個參數是數列，通常是開高低收的價格數列。  
- 第二個參數是期數。

說明：

以最新一筆資料為基準點，輸入要計算的期數，然後計算過去期數的極小值的相對位置。
如果有同樣兩個以上的極小值，則傳回離現在最近的那個。

FastLowestBar函數為[LowestBar](api?a=LowestBar&b=sys)函數的快速計算版本。

在運算極值的時候，會用 For 迴圈往前抓到極值紀錄後，之後執行腳本就會用當下的序列資料與紀錄極值相比，若大於紀錄極值則更新輸出極值與輸出極值的相對K棒位置。因為不會每根 K 棒都用 For 迴圈往前抓極值，所以腳本運行會更加快速。

範例：

```
plot1(FastLowestBar(low,5));    //繪製近5期最低的最低價相對位置的連線
```

---
## HighDays
**Path:** 系統函數 > 價格關係

HighDays -  (系統函數)
價格關係

語法：

計算指定期間內創新高的次數。  
回傳數值=HighDays(期數)  
傳入一個參數:  
- 第一個參數是期數。期數含當期，最小值為1。

說明：

範例：

```
plot1(HighDays(10)); //繪製近10期創新高次數的連線
```

請參考[LowDays函數](api?a=lowdays&b=sys)。

---
## HighestArray
**Path:** 系統函數 > 價格關係

HighestArray -  (系統函數)
價格關係

語法：

計算陣列資料的最大值。  
回傳數值=HighestArray(陣列,陣列大小)  
傳入二個參數:  
- 第一個參數是要計算的陣列。  
- 第二個參數是陣列大小。

說明：

範例：

```
Array: arrA[5](0); // 宣告arrA是一個有5個元素的陣列，初始值都是0


arrA[1] = 0;  arrA[2] = 10; arrA[3] = 20; arrA[4] = 30; arrA[5] = 40;


value1 = HighestArray(arrA,5); //計算陣列arrA的極大值
plot1(value1);                 //繪製陣列arrA的極大值的連線
```

---
## HighestBar
**Path:** 系統函數 > 價格關係

HighestBar -  (系統函數)
價格關係

語法：

計算序列資料的最大值的相對位置。  
回傳數值=HighestBar(數列,期數)  
傳入二個參數:  
- 第一個參數是數列，通常是開高低收的價格數列。  
- 第二個參數是期數。

說明：

以最新一筆資料為基準點，輸入要計算的期數，然後計算過去期數的極大值的相對位置。
如果有同樣兩個以上的極大值，則傳回離現在最近的那個。

範例：

```
plot1(HighestBar(high,5));    //繪製近5期最高的最高價相對位置的連線
```

---
## LowDays
**Path:** 系統函數 > 價格關係

LowDays -  (系統函數)
價格關係

語法：

計算指定期間內創新低的次數。  
回傳數值=LowDays(期數)  
傳入一個參數:  
- 第一個參數是期數。期數含當期，最小值為1。

說明：

範例：

```
plot1(LowDays(10)); //繪製近10期創新低次數的連線
```

請參考[HighDays函數](api?a=highdays&b=sys)。

---
## LowestArray
**Path:** 系統函數 > 價格關係

LowestArray -  (系統函數)
價格關係

語法：

計算陣列資料的最小值。  
回傳數值=LowestArray(陣列,陣列大小)  
傳入二個參數:  
- 第一個參數是要計算的陣列。  
- 第二個參數是陣列大小。

說明：

範例：

```
Array: arrA[5](0); // 宣告arrA是一個有5個元素的陣列，初始值都是0


arrA[1] = 0;  arrA[2] = 10; arrA[3] = 20; arrA[4] = 30; arrA[5] = 40;


value1 = LowestArray(arrA,5);  //計算陣列arrA的極小值
plot1(value1);                 //繪製陣列arrA的極小值的連線
```

---
## LowestBar
**Path:** 系統函數 > 價格關係

LowestBar -  (系統函數)
價格關係

語法：

計算序列資料的最小值的相對位置。  
回傳數值=LowestBar(數列,期數)  
傳入二個參數:  
- 第一個參數是數列，通常是開高低收的價格數列。  
- 第二個參數是期數。

說明：

以最新一筆資料為基準點，輸入要計算的期數，然後計算過去期數的極小值的相對位置。
如果有同樣兩個以上的極小值，則傳回離現在最近的那個。

範例：

```
plot1(LowestBar(low,5));    //繪製近5期最低的最低價相對位置的連線
```

---
## MoM
**Path:** 系統函數 > 價格關係

MoM -  (系統函數)
價格關係

語法：

計算序列資料的月變化率(換算成百分比)  
限用於月頻率資料。  
回傳數值=MoM(數列)

說明：

範例：

```
plot1(MoM(close));    //繪製收盤價的月變化率連線
```

---
## NthExtremes
**Path:** 系統函數 > 價格關係

NthExtremes -  (系統函數)
價格關係

語法：

計算序列資料的第N個極大值或極小值。  
回傳數值=NthExtremes(數列,期數,第幾個極值,要計算極大值或極小值,輸出極值,輸出極值K棒相對位置)  
傳入六個參數:  
- 第一個參數是數列，通常是開高低收的價格數列。  
- 第二個參數是期數。  
- 第三個參數是要計算極值的序號。  
- 第四個參數是要計算極大值或極小值；1為極大值、-1為極小值。  
- 第五個參數為傳址參數，會回傳計算完的極值。  
- 第六個參數是傳址參數，會回傳極值K棒相對於當期K棒的期數。

說明：

以最新一筆資料為基準點，輸入要計算的期數，然後計算過去期數的第N個極值。

計算成功時回傳值為1，結果會回傳在第5、6個參數。

範例：

```
value1 = nthextremes(high,10,2,1,value2,value3); //計算10期內第二個最高價
plot1(value2);                                   //繪製10期內第二個最高價的連線
```

---
## NthExtremesArray
**Path:** 系統函數 > 價格關係

NthExtremesArray -  (系統函數)
價格關係

語法：

計算陣列資料的第N個極大值或極小值。  
回傳數值=ExtremesArray(陣列,陣列大小,第幾個極值,要計算極大值或極小值,輸出極值,輸出極值的陣列索引值)  
傳入六個參數:  
- 第一個參數是要計算的陣列。  
- 第二個參數是陣列大小。  
- 第三個參數是要計算極值的序號。  
- 第四個參數是要計算極大值或極小值；1為極大值、-1為極小值。  
- 第五個參數為傳址參數，會回傳計算完的極值。  
- 第六個參數是傳址參數，會回傳極值的陣列索引值。

說明：

計算成功時回傳值為1，結果會回傳在第5、6個參數。

範例：

```
Array: arrA[5](0); // 宣告arrA是一個有5個元素的陣列，初始值都是0


arrA[1] = 0;  arrA[2] = 10; arrA[3] = 20; arrA[4] = 30; arrA[5] = 40;


value1 = NthExtremesArray(arrA,5,3,-1,value2,value3); //計算陣列arrA的第三個極小值
plot1(value2);                                   //繪製陣列arrA的第三個極小值的連線
```

---
## NthHighest
**Path:** 系統函數 > 價格關係

NthHighest -  (系統函數)
價格關係

語法：

計算序列資料的第N個極大值。  
回傳數值=NthHighest(第幾個極大值,數列,期數)  
傳入三個參數:  
- 第一個參數是要計算極大值的序號。  
- 第二個參數是數列，通常是開高低收的價格數列。  
- 第三個參數是期數。

說明：

以最新一筆資料為基準點，輸入要計算的期數，然後計算過去期數的第N個極大值。

範例：

```
value1 = NthHighest(2,high,5); //計算近5期次高的最高價
plot1(value1);                 //繪製近5期次高的最高價的連線
```

---
## NthHighestArray
**Path:** 系統函數 > 價格關係

NthHighestArray -  (系統函數)
價格關係

語法：

計算陣列資料的第N個極大值。  
回傳數值=NthHighestArray(陣列,陣列大小,第幾個極大值)  
傳入三個參數:  
- 第一個參數是要計算的陣列。  
- 第二個參數是陣列大小。  
- 第三個參數是要計算極大值的序號。

說明：

範例：

```
Array: arrA[5](0); // 宣告arrA是一個有5個元素的陣列，初始值都是0


arrA[1] = 0;  arrA[2] = 10; arrA[3] = 20; arrA[4] = 30; arrA[5] = 40;


value1 = NthHighestArray(arrA,5,3); //計算陣列arrA的第三個極大值
plot1(value1);                      //繪製陣列arrA的第三個極大值的連線
```

---
## NthHighestBar
**Path:** 系統函數 > 價格關係

NthHighestBar -  (系統函數)
價格關係

語法：

計算序列資料的第N個極大值距當期K棒的相對位置。  
回傳數值=NthHighestBar(第幾個極大值,數列,期數)  
傳入三個參數:  
- 第一個參數是要計算極大值的序號。  
- 第二個參數是數列，通常是開高低收的價格數列。  
- 第三個參數是期數。

說明：

範例：

```
value1 = NthHighestBar(2,high,5); //計算近5期次高的最高價的相對位置
plot1(value1);                    //繪製近5期次高的最高價相對位置的連線
```

---
## NthLowest
**Path:** 系統函數 > 價格關係

NthLowest -  (系統函數)
價格關係

語法：

計算序列資料的第N個極小值。  
回傳數值=NthLowest(第幾個極小值,數列,期數)  
傳入三個參數:  
- 第一個參數是要計算極小值的序號。  
- 第二個參數是數列，通常是開高低收的價格數列。  
- 第三個參數是期數。

說明：

以最新一筆資料為基準點，輸入要計算的期數，然後計算過去期數的第N個極小值。

範例：

```
value1 = NthLowest(2,low,5); //計算近5期次低的最低價
plot1(value1);               //繪製近5期次低的最低價的連線
```

---
## NthLowestArray
**Path:** 系統函數 > 價格關係

NthLowestArray -  (系統函數)
價格關係

語法：

計算陣列資料的第N個極小值。  
回傳數值=NthLowestArray(陣列,陣列大小,第幾個極小值)  
傳入三個參數:  
- 第一個參數是要計算的陣列。  
- 第二個參數是陣列大小。  
- 第三個參數是要計算極小值的序號。

說明：

範例：

```
Array: arrA[5](0); // 宣告arrA是一個有5個元素的陣列，初始值都是0


arrA[1] = 0;  arrA[2] = 10; arrA[3] = 20; arrA[4] = 30; arrA[5] = 40;


value1 = NthLowestArray(arrA,5,3); //計算陣列arrA的第三個極小值
plot1(value1);                      //繪製陣列arrA的第三個極小值的連線
```

---
## NthLowestBar
**Path:** 系統函數 > 價格關係

NthLowestBar -  (系統函數)
價格關係

語法：

計算序列資料的第N個極小值距當期K棒的相對位置。  
回傳數值=NthLowestBar(第幾個極小值,數列,期數)  
傳入三個參數:  
- 第一個參數是要計算極小值的序號。  
- 第二個參數是數列，通常是開高低收的價格數列。  
- 第三個參數是期數。

說明：

範例：

```
value1 = NthLowestBar(2,low,5); //計算近5期次低的最低價的相對位置
plot1(value1);                  //繪製近5期次低的最低價相對位置的連線
```

---
## OHLCPeriodsAgo
**Path:** 系統函數 > 價格關係

OHLCPeriodsAgo -  (系統函數)
價格關係

語法：

計算指定頻率K棒的開盤價，最高價，最低價，收盤價。  
僅能取得目前計算數列較高頻率之K棒資料  
回傳數值=OHLCPeriodsAgo(頻率,K棒相對位置,輸出之開盤價,輸出之最高價,輸出之最低價,輸出之收盤價,)  
傳入六個參數:  
- 第一個參數是頻率，1:日線、2:週線、3:月線、3.25:季、3.5 半年、4:年線  
- 第二個參數是K棒相對位置，和序列引用定義相同，0表當期、1表前一期...依此類推。  
- 第三個參數為傳址參數，會回傳指定頻率的開盤價。  
- 第四個參數為傳址參數，會回傳指定頻率的最高價。  
- 第五個參數為傳址參數，會回傳指定頻率的最低價。  
- 第六個參數為傳址參數，會回傳指定頻率的收盤價。

說明：

取得特定頻率、特定K棒的開盤價、最高價、最低價和收盤價

計算成功時回傳值為1，結果會回傳在第3~6個參數。

範例：

```
value1 = OHLCPeriodsAgo(2,1,value2,value3,value4,value5); //計算前期週線的開高低收價
plot1(value4);                                            //繪製前期週線最低價的連線
```

---
## QoQ
**Path:** 系統函數 > 價格關係

QoQ -  (系統函數)
價格關係

語法：

計算序列資料的季變化率(換算成百分比)  
限用於季頻率資料。  
回傳數值=QoQ(數列)

說明：

範例：

```
plot1(QoQ(close));    //繪製收盤價的季變化率連線
```

---
## ReadTicks
**Path:** 系統函數 > 價格關係

ReadTicks -  (系統函數)
價格關係

語法：

讀取自上次腳本執行後到目前為止的所有 Tick 成交資料  
本次執行更新的資料筆數 = ReadTicks ( 輸出儲存Tick資料的二維陣列, 輸出最後一筆讀到的Tick序號 ) ;  
  
回測在1分鐘頻率時，不支援模擬逐筆洗價

說明：

請先參考 [台股逐筆撮合的連續成交Tick序列](https://www.xq.com.tw/lesson/xspractice/%e5%8f%b0%e8%82%a1%e9%80%90%e7%ad%86%e6%92%ae%e5%90%88%e7%9a%84%e7%a9%bf%e5%83%b9tick/) ，關於腳本洗價方式與MultiTick的概念。

### **參數設定說明**

**輸出儲存Tick資料的二維陣列**

```
array: tick_array[100, 11](0);
```

必須先宣告一個2維陣列來儲存Tick資料。

第一維(列)是每次洗價的最大讀取筆數；第二維(行)用來儲存每一筆的成交相關資料，必須 >= 11

**輸出腳本執行完畢的最後一筆Tick序號（`Seqno`）**

```
var: intrabarpersist readtick_cookie(0);
```

必須先宣告一個`intrabarpersist`的變數，給 **ReadTicks** 內部使用。

### **參數輸出內容說明**

#### **tick\_array**

當 **ReadTicks** 執行完畢後，`tick_array`的每一列代表一筆成交紀錄（或是合併後的 MultiTick），每一行 (column) 包含該筆紀錄的不同欄位資訊。

* 每一列成交紀錄

  回傳的 Tick 資料是倒序的，`tick_array[1, ...]`是離當下最近的那筆成交，`tick_array[2, ...]`是前一筆，依此類推。
* 每一行欄位資訊

  ```
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

---
## SimpleHighest
**Path:** 系統函數 > 價格關係

SimpleHighest -  (系統函數)
價格關係

語法：

計算序列資料的最大值。  
回傳數值=SimpleHighest(數列,期數)  
傳入二個參數:  
- 第一個參數是數列，通常是開高低收的價格數列。  
- 第二個參數是期數。

說明：

以最新一筆資料為基準點，輸入要計算的期數，然後計算過去期數的極大值。

每次呼叫SimpleHighest時，腳本都會依照指定期別，往前抓取期別內的每一筆資料來計算。

系統另外提供[Highest](api?a=highest&b=sys)函數，也可以用來計算過去期數的最大值，兩者的差異請參考Highest函數的說明。

範例：

```
plot1(SimpleHighest(high,5));    //繪製5期最高價的最大值的連線
```

---
## SimpleLowest
**Path:** 系統函數 > 價格關係

SimpleLowest -  (系統函數)
價格關係

語法：

計算序列資料的最小值。  
回傳數值=SimpleLowest(數列,期數)  
傳入二個參數:  
- 第一個參數是數列，通常是開高低收的價格數列。  
- 第二個參數是期數。

說明：

以最新一筆資料為基準點，輸入要計算的期數，然後計算過去期數的極小值。

每次呼叫SimpleLowest時，腳本都會依照指定期別，往前抓取期別內的每一筆資料來計算。

系統另外提供[Lowest](api?a=lowest&b=sys)函數，也可以用來計算過去期數的最大值，兩者的差異請參考Lowest函數的說明。

範例：

```
plot1(SimpleLowest(low,5));    //繪製5期最低價的最小值的連線
```

---
## YoY
**Path:** 系統函數 > 價格關係

YoY -  (系統函數)
價格關係

語法：

計算序列資料的年變化率(換算成百分比)  
限用於年頻率資料。  
回傳數值=YoY(數列)

說明：

範例：

```
plot1(YoY(close));    //繪製收盤價的年變化率連線
```

---
## ACC
**Path:** 系統函數 > 技術指標

ACC -  (系統函數)
技術指標

語法：

ACC加速量指標(Acceleration)。用來觀察行情價格變化的加速度幅度。  
回傳數值=ACC(期數)

說明：

ACC是將MTM運動量指標再做一次動量運算的指標。

範例：

```
value1 = ACC(10);       //計算收盤價10期的加速量指標
plot1(value1, "ACC");
```

---
## ADI
**Path:** 系統函數 > 技術指標

ADI -  (系統函數)
技術指標

語法：

計算累積分配指標值。  
回傳數值=ADI

說明：

ADI指標的原文是Accumulation Distribution Index，按原文直譯的名稱是「累積分配指標」，其實它真正的意思是「漲跌力道聚散指標」，Accumulation指的是上漲力道在累積，而Distribution指的是上漲力道在消散之意，所以是「聚散指標」。

當日價格上漲時，表示上升力道戰勝，將此上升力道累積起來。

若當日是下跌，便從上升累積力道中減去下降的力道。

範例：

```
value1 = ADI;           //計算累積分配指標
plot1(value1, "A/DI");
```

---
## ADO
**Path:** 系統函數 > 技術指標

ADO -  (系統函數)
技術指標

語法：

計算聚散擺盪指標值。  
回傳數值=ADO

說明：

ADO指標的原文是Accumulation／Distribution Oscillator，直譯為「聚散擺盪」指標。

計算公式如下：

ADO＝( BP + SP ) ／ ( 2 \* ( 最高價 － 最低價 ) ) \* 100

其中

* BP = 最高價－開盤價
* SP = 收盤盤－最低價

範例：

```
value1 = ADO;              //計算聚散擺盪指標
plot1(value1, "A/D-Osc");
```

---
## AR
**Path:** 系統函數 > 技術指標

AR -  (系統函數)
技術指標

語法：

計算人氣指標值，買賣氣勢強度的測試指標，觀察開盤後對股價的現實反應。  
回傳數值=AR(期數)

說明：

AR值高時，代表行情很活潑，當AR值介於0.25至1.85(25%至185%)間時，屬於盤整行情。
AR值低時，表示人氣不足。

計算公式如下：

AR ＝ (今日最高價－今日開盤價) N日內總合 ／ (今日開盤價－今日最低價) N日內總合

範例：

```
value1 = AR(26);       //計算26期的AR指標
plot1(value1, "AR");
```

---
## ATR
**Path:** 系統函數 > 技術指標

ATR -  (系統函數)
技術指標

語法：

計算平均真實區間指標值。  
回傳數值=ATR(期數)

說明：

平均真實區間是真實區間（True Range）的平均。

真實區間是韋爾達（J. W. Wilder）於1978年發表於New Concepts in Technical Trading Systems中測量價格波動性的方法。

根據韋爾達的定義，真實區間為下列三項中的最大值：

* 當期最高價至最低價的幅度。
* 當期最低價與前期收盤價的幅度。
* 當期最高價至前期收盤價的幅度。

範例：

```
value1 = ATR(14);       //計算14期的ATR指標
plot1(value1, "ATR");
```

---
## BR
**Path:** 系統函數 > 技術指標

BR -  (系統函數)
技術指標

語法：

計算意願指標值，用來觀察開盤前對股價的預期心理。  
回傳數值=BR(期數)

說明：

BR買賣意願指標是一種被用來配合[AR](api?a=AR&b=sys)買賣人氣指標作分析的技術指標。它的公式推算原理方式也相當的類似於AR指標，利用統計某一段時日內，前一日收盤價位分別與當日最高價及當日最低價位二者的差距和的比值，以此來作為該段期間內，買賣行情雙方力道強弱的參考指標。

計算公式：

BR ＝ (今日最高價－昨日收盤價) N日內總合 ／ (昨日收盤價－今日最低價)絕對值的N日內總合

範例：

```
value1 = BR(26);       //計算26期的BR指標
plot1(value1, "BR");
```

---
## Bias
**Path:** 系統函數 > 技術指標

Bias -  (系統函數)
技術指標

語法：

計算一段時間內的收盤價與均線的乖離程度。  
回傳數值=Bias(期數)

說明：

以百分比幅度值表示移動平均線到底離股票價位有多遠。如果我們把移動平均線值當成是市場的平均買入成本的話，則乖離率可以想像成是目前市場上平均的獲利率。

另外，乖離率用來觀察均線回歸的現象。當乖離率來到歷史的極大值或極小值附近時，可能會是趨勢的反轉點。

範例：

```
value1 = Bias(5);       //計算5期的乖離率
plot1(value1, "Bias");
```

---
## BiasDiff
**Path:** 系統函數 > 技術指標

BiasDiff -  (系統函數)
技術指標

語法：

計算長短期乖離率的差值。  
回傳數值=BiasDiff(短期期數,長期期數)  
傳入二個參數:  
- 第一個參數是較短的期數。  
- 第二個參數是較長的期數。

說明：

除了價格數列和均線間會出現均線回歸的現象外，長短期的均線也會反復出現發散、收斂的循環現象。乖離率差是用來觀察長短期均線相對關係。

範例：

```
value1 = BiasDiff(3,6);       //計算3期與6期的乖離率差
plot1(value1, "BiasDiff");
```

---
## BollingerBand
**Path:** 系統函數 > 技術指標

BollingerBand -  (系統函數)
技術指標

語法：

計算包寧傑通道線。  
回傳數值=BollingerBand(數列,期數,標準差倍數)  
傳入三個參數:  
- 第一個參數是數列，通常是開高低收的價格數列。  
- 第二個參數是計算均線的期數。  
- 第三個參數是決定通道寬度的標準差倍數，輸入正值時為計算上通道，輸入負值時為計算下通道。

說明：

包寧傑通道為John Bollinger在1980年代發明的指標。以價格均線為中心點，往上N個標準差為通道上限（壓力線）；往下減去N個標準差，為通道下限（支撐線）。

範例：

```
value1 = BollingerBand(Close,20,2);       //計算20期、2個標準差寬的包寧傑通道上限
plot1(value1, "BollingerUpperBand");
```

---
## CCI
**Path:** 系統函數 > 技術指標

CCI -  (系統函數)
技術指標

語法：

計算CCI技術指標值。  
回傳數值=CCI(期數)

說明：

CCI順勢指標為Donald Lambert在1980年代發明的指標。CCI大多時間會落在+100到-100之間，主要的功能在於針對商品本身所具有的週期循環特性，來尋找出行情的高低價位區來。

若CCI值從一高點急速往下降時，可視為是賣出的訊號；同理若CCI值從一低點快速往上升時，可視之為買進的訊號。

範例：

```
value1 = CCI(14);       //計算14期的CCI
plot1(value1, "CCI");
```

註：CCI函數和[CommodityChannel](api?a=CommodityChannel&b=sys)函數相同。

---
## CommodityChannel
**Path:** 系統函數 > 技術指標

CommodityChannel -  (系統函數)
技術指標

語法：

計算CCI技術指標值。  
回傳數值=CommodityChannel(期數)

說明：

CCI順勢指標為Donald Lambert在1980年代發明的指標。CCI大多時間會落在+100到-100之間，主要的功能在於針對商品本身所具有的週期循環特性，來尋找出行情的高低價位區來。

若CCI值從一高點急速往下降時，可視為是賣出的訊號；同理若CCI值從一低點快速往上升時，可視之為買進的訊號。

範例：

```
value1 = CommodityChannel(14);       //計算14期的CCI
plot1(value1, "CCI");
```

註：CommodityChannel函數和[CCI](api?a=CCI&b=sys)函數相同。

---
## DIF
**Path:** 系統函數 > 技術指標

DIF -  (系統函數)
技術指標

語法：

計算MACD中的DIF值。  
回傳數值=DIF(短期數,長期數)  
傳入二個參數:  
- 第一個參數是計算快速線（短期）的期數。  
- 第二個參數是計算慢速線（長期）的期數。

說明：

MACD是由Gerald Appel於1970年代所發明的指標。利用二條快速與慢速指數移動平均線的收斂或發散來判斷價格走勢。

DIF是快速線與慢速線的差值。

範例：

```
value1 = DIF(12,26);       //計算MACD中的DIF值
plot1(value1, "DIF");
```

---
## DMO
**Path:** 系統函數 > 技術指標

DMO -  (系統函數)
技術指標

語法：

計算DMO指標。  
回傳數值=DMO(期數)

說明：

DMO指標（Directional Movement Oscillator）是利用DMI趨向指標指標中的正負DI線計算，以此二條線的差值做為新的指標線。

範例：

```
value1 = DMO(14);       //計算14期的DMO指標
plot1(value1, "DMO");
```

---
## DPO
**Path:** 系統函數 > 技術指標

DPO -  (系統函數)
技術指標

語法：

計算DPO指標。  
回傳數值=DPO(期數)

說明：

非趨勢價格擺盪指標（Detrended Price Oscillator）是Walt Bressert所發明的指標。他研究商品的循環後發現，一個長期波動中包含了多個短期波動。因此，觀察短期波動的變化規律，可以估計長期波動高低點出現的時機。例如：二個短期循環底部，形成一個長期循環底部。因此，DPO指標刻意忽略較長期的波動，一方面可以減少週期干擾的混淆，也可以凸顯個別週期的波動。

範例：

```
value1 = DPO(10);       //計算10期的DPO指標
plot1(value1, "DPO");
```

---
## D_Value
**Path:** 系統函數 > 技術指標

D\_Value -  (系統函數)
技術指標

語法：

計算KD指標中的D值。  
回傳數值=D\_Value(資料期數,D值平滑期數)  
傳入二個參數:  
- 第一個參數是資料期數，指定計算的區間長度。  
- 第二個參數是D值平滑期數，指定計算D值所用的平滑期數。

說明：

KD指標為美國交易員George Lane所創，原名為Stochastic Oscillator。
D\_Value即隨機指標中的慢速線（%D）。

範例：

```
value1 = D_Value(9,3);       //計算KD指標中的D值
plot1(value1, "D");
```

---
## DirectionMovement
**Path:** 系統函數 > 技術指標

DirectionMovement -  (系統函數)
技術指標

語法：

計算DMI指標。  
回傳數值=DirectionMovement(期數,輸出+DI值,輸出-DI值,輸出ADX值)  
傳入四個參數:  
- 第一個參數是計算期數。  
- 第二個參數是輸出計算完的+DI值。  
- 第三個參數是輸出計算完的-DI值。  
- 第四個參數是輸出計算完的ADX值。

說明：

DMI趨向指標是威爾德（Wilder）所發明。主要的用途在於作趨勢成立的判斷，因此是屬於較為長期交易的技術指標。

DMI指標包含了+DI、-DI及ADX三個數值。DirectionMovement函數回傳1時，代表計算成功。+DI、-DI及ADX的值是回傳在第2、3、4個參數。

範例：

```
value1 = DirectionMovement(14,value2,value3,value4);       //計算14期的DMI指標
plot1(value2, "+DI");
plot2(value3, "-DI");
plot3(value4, "ADX");
```

---
## EMP
**Path:** 系統函數 > 技術指標

EMP -  (系統函數)
技術指標

語法：

計算Empty Line指標值  
回傳數值=EMP

說明：

Empty Line是移動平均線的一種變化，它的數值的計算方式是用3期、6期、12期與24期的移動平均線值取平均後產生的。

計算公式：

EMP = （3期收盤價平均 + 6期收盤價平均 + 12期收盤價平均 + 24期收盤價平均）／4

範例：

```
value1 = EMP;       //計算EMP指標
plot1(value1, "EMP");
```

---
## ERC
**Path:** 系統函數 > 技術指標

ERC -  (系統函數)
技術指標

語法：

計算價格變動率的指數移動平均。  
回傳數值=ERC(資料期數,平滑期數)  
傳入二個參數:  
- 第一個參數是計算RC的資料期數。  
- 第二個參數是計算ERC的平滑期數。

說明：

計算當期對N期前的變動幅度，再對此數值取指數移動平均。

範例：

```
value1 = ERC(12,12);       //計算12期的ERC指標
plot1(value1, "ERC");
```

---
## HL_Osc
**Path:** 系統函數 > 技術指標

HL\_Osc -  (系統函數)
技術指標

語法：

計算HL-Osc指標。  
回傳數值=HL\_Osc

說明：

高低價擺盪指標（High Low Oscillator）是以當日高點至昨收這段價差佔真實區間的比例來判斷強弱勢。

範例：

```
value1 = HL_Osc;       //計算HL-Osc指標
plot1(value1, "HL-Osc");
```

---
## KO成交量擺盪指標
**Path:** 系統函數 > 技術指標

KO成交量擺盪指標 -  (系統函數)
技術指標

語法：

計算KO成交量擺盪值。  
回傳數值=callfunction("KO成交量擺盪指標",短期數,長期數,雜訊平滑期數);  
傳入三個參數:  
- 第一個參數是短期數，計算短期KO成交量的簡單移動平均。  
- 第二個參數是長期數，計算長期KO成交量的簡單移動平均。

說明：

範例：

```
value1 =callfunction("KO成交量擺盪指標", Length1, Length2);
```

---
## KST確認指標
**Path:** 系統函數 > 技術指標

KST確認指標 -  (系統函數)
技術指標

語法：

計算KST確認指標值。  
回傳數值=callfunction("KST確認指標");

說明：

詳細說明請參考[交易的點點滴滴](https://xstrader.net/KST%E7%A2%BA%E8%AA%8D%E6%8C%87%E6%A8%99/)。

範例：

```
value1=callfunction("KST確認指標");
```

---
## K_Value
**Path:** 系統函數 > 技術指標

K\_Value -  (系統函數)
技術指標

語法：

計算KD指標中的K值。  
回傳數值=K\_Value(資料期數,K值平滑期數)  
傳入二個參數:  
- 第一個參數是資料期數，指定計算的區間長度。  
- 第二個參數是K值平滑期數，指定計算K值所用的平滑期數。

說明：

KD指標為美國交易員George Lane所創，原名為Stochastic Oscillator。
K\_Value即隨機指標中的快速線（%K）。

範例：

```
value1 = K_Value(9,3);       //計算KD指標中的K值
plot1(value1, "K");
```

---
## KeltnerLB
**Path:** 系統函數 > 技術指標

KeltnerLB -  (系統函數)
技術指標

語法：

計算肯特納通道的下通道線（支撐線）。  
回傳數值=KeltnerLB(通道倍數)

說明：

肯特納通道是原始概念是由Chester W. Keltner於1960年代所發明。但現在較常用的版本是由Linda Bradford Raschke於1980年代所出的修正版本。

原始版本的肯特納通道是以10期的典型價移動平均線為中心線，然後以10期的當日高低價差平均為通道寬度。

新版的肯特納通道是以20期的收盤價指數移動平均線為中心線，然後以2.5倍的20期真實範圍平均做通道寬度。我們提供的是較新版本的肯特納通道。

範例：

```
value1 = KeltnerLB(2.5);       //計算肯特納通道的下通道線
plot1(value1, "KeltnerLowerBand");
```

---
## KeltnerMA
**Path:** 系統函數 > 技術指標

KeltnerMA -  (系統函數)
技術指標

語法：

計算肯特納通道的中心線。  
回傳數值=KeltnerMA(期數)

說明：

肯特納通道是原始概念是由Chester W. Keltner於1960年代所發明。但現在較常用的版本是由Linda Bradford Raschke於1980年代所出的修正版本。

原始版本的肯特納通道是以10期的典型價移動平均線為中心線，然後以10期的當日高低價差平均為通道寬度。

新版的肯特納通道是以20期的收盤價指數移動平均線為中心線，然後以2.5倍的20期真實範圍平均做通道寬度。我們提供的是較新版本的肯特納通道。

範例：

```
value1 = KeltnerMA(20);       //計算肯特納通道的中心線
plot1(value1, "KeltnerMid");
```

---
## KeltnerUB
**Path:** 系統函數 > 技術指標

KeltnerUB -  (系統函數)
技術指標

語法：

計算肯特納通道的上通道線（壓力線）。  
回傳數值=KeltnerUB(通道倍數)

說明：

肯特納通道是原始概念是由Chester W. Keltner於1960年代所發明。但現在較常用的版本是由Linda Bradford Raschke於1980年代所出的修正版本。

原始版本的肯特納通道是以10期的典型價移動平均線為中心線，然後以10期的當日高低價差平均為通道寬度。

新版的肯特納通道是以20期的收盤價指數移動平均線為中心線，然後以2.5倍的20期真實範圍平均做通道寬度。我們提供的是較新版本的肯特納通道。

範例：

```
value1 = KeltnerUB(2.5);       //計算肯特納通道的上通道線
plot1(value1, "KeltnerUpperBand");
```

---
## MACD
**Path:** 系統函數 > 技術指標

MACD -  (系統函數)
技術指標

語法：

計算MACD指標值。  
回傳數值=MACD(數列,短期數,長期數,MACD平滑期數,輸出DIF值,輸出MACD值,輸出OSC值)  
傳入七個參數:  
- 第一個參數是數列，MACD通常是以加權平均收盤價（WeightedClose）來計算。  
- 第二個參數是計算快速線（短期）的期數。  
- 第三個參數是計算慢速線（長期）的期數。  
- 第四個參數是計算MACD使用之平滑期數。  
- 第五個參數是輸出計算完的DIF值。  
- 第六個參數是輸出計算完的MACD值。  
- 第七個參數是輸出計算完的OSC值。

說明：

MACD是由Gerald Appel於1970年代所發明的指標。利用二條快速與慢速指數移動平均線的收斂或發散來判斷價格走勢。

MACD指標包含了DIF、MACD及OSC三個數值。MACD函數回傳1時，代表計算成功。DIF、MACD及OSC的值是回傳在第5、6、7個參數。

範例：

```
value1 = MACD(WeightedClose,12,26,9,value2,value3,value4);       //計算MACD
plot1(value2, "DIF");
plot2(value3, "MACD");
plot3(value4, "OSC");
```

---
## MAM
**Path:** 系統函數 > 技術指標

MAM -  (系統函數)
技術指標

語法：

計算MAM指標。  
回傳數值=MAM(移動平均期數,動量期數)  
傳入二個參數:  
- 第一個參數是用來計算移動平均的期數。  
- 第二個參數是用來計算動量的期數。

說明：

移動平均動量指標（Moving Average Momentum）的用法、概念和Momentum相同，但計算方式改為計算移動平均的動量。

計算公式：當期移動平均值，減去n期前的移動平均值。

範例：

```
value1 = MAM(10,10);       //計算MAM指標
plot1(value1, "MAM");
```

---
## MA_Osc
**Path:** 系統函數 > 技術指標

MA\_Osc -  (系統函數)
技術指標

語法：

計算移動平均線擺盪指標。  
回傳數值=MA\_Osc(短期數,長期數)  
傳入二個參數:  
- 第一個參數是計算快速線（短期）的期數。  
- 第二個參數是計算慢速線（長期）的期數。

說明：

MA\_Osc是計算二條不同期數的簡單移動平均線差值而得。

範例：

```
value1 = MA_Osc(5,10);       //計算MA-Osc
plot1(value1, "MA_Osc");
```

---
## MI
**Path:** 系統函數 > 技術指標

MI -  (系統函數)
技術指標

語法：

計算質量指標。  
回傳數值=MI(期數1,期數2)  
傳入二個參數:  
- 第一個參數是計算指數移動平均的期數。  
- 第二個參數是計算總和的期數。

說明：

MI指標（Mass Index）是由Donald Dorsey所發明。質量指標是觀察高低價範圍的變化，試圖找出趨勢的反轉點。

計算公式：

EMA1 = (最高價－最低價)的9日EMA
EMA2 = EMA1 的9日EMA
MI = ( EMA1 ／ EMA2 ) 的N2日簡單加總

範例：

```
value1 = MI(9,25);       //計算收盤價10期的質量指標
plot1(value1, "MI");
```

---
## MO
**Path:** 系統函數 > 技術指標

MO -  (系統函數)
技術指標

語法：

計算收盤價變動的百分比。  
回傳數值=MO(期數)

說明：

動量震盪指標（Momentum Oscillator）和MTM相類似，都是測量收盤價的變動量。

差別在於MTM指標是計算變動絕對值，而MO為變動的百分比來衡量變動量。

計算公式：

MO = ( 當期收盤價 ／ 前N期收盤價 ) \* 100

範例：

```
value1 = MO(10);       //計算收盤價10期的運動量指標
plot1(value1, "MO");
```

---
## MTM
**Path:** 系統函數 > 技術指標

MTM -  (系統函數)
技術指標

語法：

計算收盤價的變動量。  
回傳數值=MTM(期數)

說明：

以收盤價計算目前K棒與N根K棒之前的差值。

計算公式：

MTM = 當期收盤價 - 前N期收盤價

範例：

```
value1 = MTM(10);       //計算收盤價10期的運動量指標
plot1(value1, "MTM");
```

與[Momentum函數](api?a=momentum&b=sys)相同。

---
## MTM_MA
**Path:** 系統函數 > 技術指標

MTM\_MA -  (系統函數)
技術指標

語法：

平均運動量指標，計算運動量指標的簡單移動平均。  
回傳數值=MTM\_MA(期數)

說明：

計算公式：

MTM\_MA = （當期收盤價 - 前N期收盤價）的N期平均

範例：

```
value1 = MTM_MA(10);       //計算收盤價10期的平均運動量指標
plot1(value1, "MTM_MA");
```

---
## Momentum
**Path:** 系統函數 > 技術指標

Momentum -  (系統函數)
技術指標

語法：

計算數列指定期間的變動量。  
回傳數值=Momentum(數列,期數)  
傳入二個參數:  
- 第一個參數是數列，通常是開高低收的價格數列。  
- 第二個參數是期數。

說明：

計算目前K棒與N根K棒之前的差值。

計算公式：

Momentum = 當期價格 - 前N期價格

範例：

```
value1 = Momentum(Close,10);       //計算收盤價10期的運動量指標
plot1(value1, "MTM");
```

---
## PSY
**Path:** 系統函數 > 技術指標

PSY -  (系統函數)
技術指標

語法：

人氣指標心理線。  
回傳數值=PSY(期數)

說明：

計算特定期間內，行情上漲期數的比例。

計算公式：

PSY = 100 \* 上漲次數／總次數

範例：

```
value1 = PSY(12);       //計算12期的心理線指標
plot1(value1, "PSY");
```

---
## PercentR
**Path:** 系統函數 > 技術指標

PercentR -  (系統函數)
技術指標

語法：

計算威廉指標（或稱百分比R、%R）。  
回傳數值=PercentR(期數)

說明：

由Larry Williams所提出，將當前收盤價在指定期間內的區間最高價與最低價之間位置，以百分比表示。

範例：

```
value1 = PercentR(12) - 100;       //計算12期的威廉指標
plot1(value1, "%R");
```

---
## Q指標
**Path:** 系統函數 > 技術指標

Q指標 -  (系統函數)
技術指標

語法：

計算技術分析的Q值。  
回傳數值=callfunction("Q指標",期數,平滑期數,雜訊平滑期數);  
傳入三個參數:  
- 第一個參數是期數，指定要計算累積價格變化的區間。  
- 第二個參數是平滑期數，計算累積價格變化的簡單移動平均。  
- 第三個參數是雜訊平滑期數，計算雜訊的簡單移動平均。

說明：

詳細說明請參考[交易的點點滴滴](https://xstrader.net/%E8%87%AA%E8%A8%82%E6%8C%87%E6%A8%99%E7%9A%84%E6%92%B0%E5%AF%AB%E6%8A%80%E5%B7%A7%E4%BB%A5q%E6%8C%87%E6%A8%99%E7%82%BA%E4%BE%8B/)。

---
## RC
**Path:** 系統函數 > 技術指標

RC -  (系統函數)
技術指標

語法：

計算價格變動率。  
回傳數值=RC(期數)

說明：

計算當期對N期前的變動幅度。

計算公式：

RC = （當期收盤價／前N期收盤價）／前N期收盤價

範例：

```
value1 = RC(12);       //計算12期的RC指標
plot1(value1, "RC");
```

---
## RSI
**Path:** 系統函數 > 技術指標

RSI -  (系統函數)
技術指標

語法：

計算相對強弱指標數值。  
回傳數值=RSI(數列,期數)  
傳入二個參數:  
- 第一個參數是數列，通常是開高低收的價格數列。  
- 第二個參數是期數。

說明：

RSI是相對強弱指標（Relative Strength Index）用於衡量一個資產的過去價格變動，並評估其是否處於超買或超賣的狀態。

RSI的計算基於資產的價格變動幅度，通常在一段時間內（例如14個交易日）內進行計算。指標的值位於0到100之間，通常被分成以下幾個區域：

* 80以上：表示該資產可能處於超買狀態，可能會有回調或調整的風險。
* 20以下：表示該資產可能處於超賣狀態，可能會有反彈或上漲的機會。

投資者可以利用RSI指標來識別潛在的轉折點，例如在RSI進入超買或超賣區域時，可能預示著價格反轉的機會。

範例：

```
value1 = RSI(Close,6);       //計算6期的RSI指標
plot1(value1, "RSI");
```

---
## RSV
**Path:** 系統函數 > 技術指標

RSV -  (系統函數)
技術指標

語法：

未成熟隨機值  
回傳數值=RSV(期數)

說明：

計算收盤價在指定期間內的區間最高價與最低價之間位置，以百分比表示。後續可用來計算KD指標的K值與D值。

計算公式：

RSV=(收盤價－n日之最低價)／(n日內最高價－n日最低價)＊100

範例：

```
value1 = RSV(9);       //計算9期的RSV指標
plot1(value1, "RSV");
```

---
## SAR
**Path:** 系統函數 > 技術指標

SAR -  (系統函數)
技術指標

語法：

SAR停損點轉向指標，或稱為拋物線型指標，為一種設定停損點相當有效力的指標。  
回傳數值=SAR(加速因子初始值,加速因子遞增值,加速因子最大值)  
傳入三個參數:  
- 第一個參數是加速因子初始值。  
- 第二個參數是加速因子遞增值。  
- 第三個參數是加速因子最大值。

說明：

由威爾德（Wilder）發明的停損點轉向操作系統（SAR, Stop And Reverse）或者稱之為拋物線型指標（Parabolic Time/Price），是一種隨著時間的延續，用類似於拋物曲線的追趕方式，來追趕價位行情，以便設定出買賣操作時的停損點反轉值，使得一旦行情觸及停損反轉時，可以依照訊號進行買賣的動作。

範例：

```
value1 = SAR(0.02, 0.02, 0.2);       //計算SAR指標
plot1(value1, "SAR");
```

---
## Stochastic
**Path:** 系統函數 > 技術指標

Stochastic -  (系統函數)
技術指標

語法：

計算KD指標。  
回傳數值=Stochastic(資料期數,K值平滑期數,D值平滑期數,輸出RSV值,輸出K值,輸出D值)  
傳入六個參數:  
  
- 第一個參數是資料期數，指定計算的區間長度。  
- 第二個參數是K值平滑期數，指定計算K值所用的平滑期數。  
- 第三個參數是D值平滑期數，指定計算D值所用的平滑期數。  
- 第四個參數是輸出RSV值，回傳計算完的RSV值。  
- 第五個參數是輸出K值，回傳計算完的K值。  
- 第六個參數是輸出D值，回傳計算完的D值。

說明：

KD指標為美國交易員George Lane所創，原名為Stochastic Oscillator。

Stochastic函數可計算KD指標的RSV、K及D三個數值。Stochastic函數回傳1時，代表計算成功。RSV、K及D的值是回傳在第4、5、6個參數。

範例：

```
value1 = Stochastic(9,3,3,value2,value3,value4);       //計算KD指標
plot1(value3, "K");
plot2(value4, "D");
```

---
## TRIX
**Path:** 系統函數 > 技術指標

TRIX -  (系統函數)
技術指標

語法：

三重指數平滑移動平均指標。  
回傳數值=TRIX(數列,期數)  
傳入二個參數:  
- 第一個參數是數列，通常是開高低收的價格數列。  
- 第二個參數是期數。

說明：

TRIX是在1980年代由Jack Hutson所發明的指標。TRIX指的是對股價X取TRIPLE （三次） 平滑的意思。將數值計算三次指數移動平均（EMA，也就是MACD式平滑法）之後的數列，再計算其變動率而得。

範例：

```
value1 = TRIX(Close, 9);       //計算9期TRIX指標
plot1(value1, "TRIX");
```

---
## TechScore
**Path:** 系統函數 > 技術指標

TechScore -  (系統函數)
技術指標

語法：

計算多空判斷分數。  
回傳數值=TechScore

說明：

利用多種指標計算多空分數，綜合研判後續走勢。使用的指標包括：Arron指標、隨機漫步指標、順勢指標、CMO錢德動量擺動指標、RSI、MACD、MTM、KD、DMI、AR、ACC、TRIX、SAR及均線計14種。

多空判斷分數會介於0~14之間，12以上是超買區、3以下是超賣區。

範例：

```
value1 = TechScore;       //計算多空判斷分數
plot1(value1, "多空指標");
```

---
## VA
**Path:** 系統函數 > 技術指標

VA -  (系統函數)
技術指標

語法：

計算成交量累散佈指標。  
回傳數值=VA

說明：

VA指標的原文是Volume Accumulation／Distribution指標，原文的意思直譯的話，稱為「成交量累積散佈」指標。它的公式與修正OBV公式完全相同，沒有兩樣。

VA屬於累積型的指標，指標值會因計算的起始點不同而有所差異。

範例：

```
value1 = VA;       //計算成交量累散佈指標
plot1(value1, "VA");
```

---
## VAO
**Path:** 系統函數 > 技術指標

VAO -  (系統函數)
技術指標

語法：

計算成交量累散佈擺盪指標。  
回傳數值=VAO

說明：

VAO指標的原文是Volume Accumulation／Distribution Oscillator指標，按照原文的意思直譯為「成交量累積散佈擺盪」指標。累積指的自然是籌碼集中累積到少數人手上的現象，而散佈自然是指籌碼散佈分散到多數人手上的現象。這個指標與VA指標（即修正OBV指標）幾乎雷同，唯一的差別是VA指標要逐日累加，VAO不累加而已。

範例：

```
value1 = VAO;       //計算成交量累散佈擺盪指標
plot1(value1, "VAO");
```

---
## VHF
**Path:** 系統函數 > 技術指標

VHF -  (系統函數)
技術指標

語法：

計算垂直水平過濾指標。  
回傳數值=VHF(期數)

說明：

本指標由亞當．懷特（Adam White）所發明，用來確定價格的變動是處於水平變動（區間橫向整理期），或是垂直變動（固定趨勢的大幅漲跌期）的階段。然後投資人可依此選用趨勢指標或震盪指標，來決定買賣的時機。

範例：

```
value1 = VHF(42);       //計算42期的VHF指標
plot1(value1, "VHF");
```

---
## VPT
**Path:** 系統函數 > 技術指標

VPT -  (系統函數)
技術指標

語法：

計算量價趨勢指標。  
回傳數值=VPT

說明：

VPT（Volume Price Trend）量價趨勢指標，或被稱為PVT指標，也是一種類似於OBV的技術指標。而它所累算的是價格漲跌幅與成交量的乘積。

範例：

```
value1 = VPT;       //計算量價趨勢指標
plot1(value1, "VPT");
```

---
## VR
**Path:** 系統函數 > 技術指標

VR -  (系統函數)
技術指標

語法：

計算成交量比率指標。  
回傳數值=VR(期數)

說明：

VR(Volume Ratio)中文譯為「容量比率」，或者又稱為「成交量比率指標」。其計算方式為針對特定期間內，累加上漲日的成交量作為分子，以及累加下跌日的成交量作為分母，然後將此兩數相除即得。

VR指標的設計原意，是要利用量價關係的基本原理，來作為研判股價的可能變動方向。

如果VR公式中的分子與分母大小相同，則VR的數值會剛好等於100，表示計算期間內的上漲成交量等於下跌成交量。當VR大於100，表示上漲成交量大於下跌成交量；而VR小於100，則表示上漲成交量小於下跌成交量。

不過，由於在股巿中，股價上漲時成交量的擴增比較沒有上限，可以擴增到數倍（甚或數十倍）之多；而股價下跌時成交量的下降反而比較有限，頂多只降個幾十個百分點（最多不會超過100%）。所以VR指標不會出現剛好以100為中心，上下波動幅度相同的情況。而是VR的高點會明顯大於100很多，低點則小於100的程度不大。

範例：

```
value1 = VR(26);       //計算26期的VR指標
plot1(value1, "VR");
```

---
## VVA
**Path:** 系統函數 > 技術指標

VVA -  (系統函數)
技術指標

語法：

計算VVA指標。  
回傳數值=VVA

說明：

OBV的變形累算方法，除了VA成交量累散佈指標以外，還有另外一種的變化方式，即VVA指標(Variable Volume Accumulation/Distribution)。

VVA屬於累積型的指標，指標值會因計算的起始點不同而有所差異。

範例：

```
value1 = VVA;       //計算VVA指標
plot1(value1, "VVA");
```

---
## WAD
**Path:** 系統函數 > 技術指標

WAD -  (系統函數)
技術指標

語法：

計算威廉多空力度線(Williams Accumulation/Distribution)。  
回傳數值=WAD

說明：

由Larry Williams所發明的指標，屬於累積型的指標，指標值會因計算的起始點不同而有所差異。

範例：

```
value1 = WAD;              //計算威廉多空力度線
plot1(value1, "WA/D");
```

---
## BarsLast
**Path:** 系統函數 > 日期相關

BarsLast -  (系統函數)
日期相關

語法：

取得上一次條件成立到當前的K棒數  
回傳數值=BarsLast(條件數列)

說明：

計算目前K棒與上次條件成立K棒的期數差。例如，上次KD黃金交叉是幾天前。

回傳值為0表示條件成立當期，回傳值為1表示前1期條件成立，依此類推。

範例：

```
value1 = average(C,5);
value2 = average(C,20);
value3 = barslast(value1 cross over value2);       //計算上次5日均線和20日均線黃金交叉的期數差
value4 = low[value3];       //取得上次均線黃金交叉時的最低價做為支撐價
plot1(value4);       //繪製支撐價的連線
```

---
## DaysToExpiration
**Path:** 系統函數 > 日期相關

DaysToExpiration -  (系統函數)
日期相關

語法：

計算台股指數類期貨商品的到期天數。  
回傳數值=DaysToExpiration(商品月份,商品年份)  
傳入二個參數:  
- 第一個參數是商品月份。  
- 第二個參數是商品年份。

說明：

台股指數類期貨商品的到期日期為該月的第三個星期三。此函數會計算特定合約距當期K棒的天數。

回傳值為1，表示當天為結算日。回傳值小於1，表示該合約已到期。

範例：

```
value1 = DaysToExpiration(month(date),year(date));
if value1 <= 1 then begin
	value2 = dateadd(date,"M",1);
	value2 = encodedate(year(value2),month(value2),1);
	value1 = DaysToExpiration(month(value2),year(value2));
end;
plot1(value1); //繪製最新的台股指數類期貨到期天數的連線
```

注意，此函數並無調整因放假而導致的到期日異動。

---
## DownTrend
**Path:** 系統函數 > 日期相關

DownTrend -  (系統函數)
日期相關

語法：

判斷某個序列是否趨勢向下。  
回傳布林值=DownTrend(數列,期數)  
傳入二個參數:  
- 第一個參數是數列，可以是GetField("欄位名稱")。  
- 第二個參數是期數。

說明：

計算序列資料是否趨勢向下。回傳布林值。
若為趨勢向上，則回傳「True」
若不為趨勢向上，則回傳「False」

---
## GetLastTradeDate
**Path:** 系統函數 > 日期相關

GetLastTradeDate -  (系統函數)
日期相關

語法：

取得台股指數類期貨商品的到期日期（該月第三個星期三）。  
回傳數值=GetLastTradeDate(商品月份,商品年份)  
傳入二個參數:  
- 第一個參數是商品月份。  
- 第二個參數是商品年份。

說明：

依台灣期貨交易所規定台股指數類期貨商品的到期日期為該月的第三個星期三。

函數回傳值的格式為8碼數字: **YYYYMMDD**。

範例：

```
value1 = GetLastTradeDate(7,2015); //取得台股指數類期貨2015年7月合約的到期日
```

注意，此函數並無調整因放假而導致的到期日異動。

---
## LastDayOfMonth
**Path:** 系統函數 > 日期相關

LastDayOfMonth -  (系統函數)
日期相關

語法：

取得指定月份的天數。  
回傳數值=LastDayOfMonth(月份)  
傳入一個參數:  
- 第一個參數是月份，1為1月、2為2月...依此類推。

說明：

傳回指定月份的天數。

例如：一月有31天、二月只有28天、四月有30天。

範例：

```
value1 = LastDayOfMonth(month(date)); //取得當月的天數
```

---
## NDaysAngle
**Path:** 系統函數 > 日期相關

NDaysAngle -  (系統函數)
日期相關

語法：

計算股價N期走勢的角度  
回傳數值=NDaysAngle(期數)

說明：

計算股價N期走勢的角度。回傳數值。
若為上漲趨勢，則回傳「0 ~ 90」度
若為下跌趨勢，則回傳「0 ~ -90」度

範例：
input:\_Length(10,"期數"); //計算10期走勢的角度
plot1(NDaysAngle(\_Length),"走勢角度");

---
## NthDayofMonth
**Path:** 系統函數 > 日期相關

NthDayofMonth -  (系統函數)
日期相關

語法：

取得指定日期後第N個星期幾的日期。  
回傳數值=NthDayOfMonth(參考日期,第幾個,星期幾)  
傳入三個參數:  
- 第一個參數是日期，格式為YYYYMMDD的數值。  
- 第二個參數是第幾個，可以是正數(表示往後找), 也可以是負數(表示往前找)。  
- 第三個參數是星期幾：0為星期日，1為星期一，2為星期二，3為星期三，4為星期四，5為星期五，6為星期六

說明：

NthDayOfMonth回傳的數值是YYYYMMDD的日期格式。

範例：

```
value1 = NthDayOfMonth(date,3,1)); //取得未來第3個星期一的日期
```

---
## UpTrend
**Path:** 系統函數 > 日期相關

UpTrend -  (系統函數)
日期相關

語法：

判斷某個數列是否趨勢向上。  
回傳布林值=UpTrend(數列,期數)  
傳入二個參數:  
- 第一個參數是數列，可以是GetField("欄位名稱")。  
- 第二個參數是期數。

說明：

計算序列資料是否趨勢向上。回傳布林值。
若為趨勢向上，則回傳「True」
若不為趨勢向上，則回傳「False」

---
## angleprice
**Path:** 系統函數 > 日期相關

angleprice -  (系統函數)
日期相關

語法：

傳回「N期至今的角度的趨勢線價格」  
回傳數值=Angleprice(期數,角度)  
傳入二個參數:  
- 第一個參數是期數。  
- 第二個參數是角度。

說明：

用前N期到現在的角度，運算出趨勢線的價格。

範例：

```
value1 = angleprice(5,0);
plot1(value1); //Value1為前五期的開盤價，因為第二個參數是0度，所以會等於前五期的開盤價。
```

更多的資訊，請參考[常用語法匯總](https://www.xq.com.tw/xstrader/%E5%B8%B8%E7%94%A8%E7%9A%84%E8%AA%9E%E6%B3%95%E5%8C%AF%E7%B8%BD/)

---
## formatMQY
**Path:** 系統函數 > 日期相關

formatMQY -  (系統函數)
日期相關

語法：

依目前資料頻率取得代表日期字串。  
回傳字串=formatMQY(參考日期)  
傳入一個參數:  
- 第一個參數是日期，格式為YYYYMMDD的數值。

說明：

formatMQY回傳的字串為：

* 當頻率為年時，回傳格式為YYYY的字串；例如：2015。
* 當頻率為季時，回傳格式為YYYYQQ的字串；例如：2015Q1。
* 當頻率為月時，回傳格式為YYYYMM的字串；例如：201501。
* 當其他頻率時，回傳格式為YYYYMMDD的字串；例如：20150103。

範例：

```
var: string1("");
string1 = formatMQY(date); //將日期轉換為MQY格式的字串
print(date," ",string1);
```

---
## BSDelta
**Path:** 系統函數 > 期權相關

BSDelta -  (系統函數)
期權相關

語法：

以BlackScholes模型計算選擇權Delta  
回傳數值=BSDelta(買賣權,標的價格,履約價,到期天數,無風險利率,持有成本,波動率)  
傳入七個參數:  
- 第一個參數是買賣權，C表買權、P表賣權。  
- 第二個參數是標的價格。  
- 第三個參數是履約價。  
- 第四個參數是到期天數。  
- 第五個參數是無風險利率，百分比值。若無風險利率為2%，請輸入2。  
- 第六個參數是持有成本，百分比值。若持有成本為1%，請輸入1。  
- 第七個參數是波動率，百分比值。若持有成本為30%，請輸入30。

說明：

BS選擇權定價模型為諾貝爾經濟學獎得主Robert Merton和Myron Scholes於1973所發表。依據下列六個參數決定選擇權的理論價：標的價格、履約價、到期天數、無風險利率、持有成本、波動率。其中只有履約價和到期天數是由合約所規定，其餘參數皆會隨市場狀況而變動。

範例：

```
value1 = BSDelta("C",8800,9000,20,2,0,25);       //計算波動率25%、20天後到期之台指選擇權9000的Call在指數為8800點的Delta值
plot1(value1, "Delta");
```

註：更多參數說明請參考[BlackScholes公式](api?a=BlackScholesModel&b=sys)。

---
## BSGamma
**Path:** 系統函數 > 期權相關

BSGamma -  (系統函數)
期權相關

語法：

以BlackScholes模型計算選擇權Gamma  
回傳數值=BSGamma(買賣權,標的價格,履約價,到期天數,無風險利率,持有成本,波動率)  
傳入七個參數:  
- 第一個參數是買賣權，C表買權、P表賣權。  
- 第二個參數是標的價格。  
- 第三個參數是履約價。  
- 第四個參數是到期天數。  
- 第五個參數是無風險利率，百分比值。若無風險利率為2%，請輸入2。  
- 第六個參數是持有成本，百分比值。若持有成本為1%，請輸入1。  
- 第七個參數是波動率，百分比值。若持有成本為30%，請輸入30。

說明：

Gamma為選擇權敏感度分析的參數之一，用來衡量Delta相對於標的價格變動的敏感度。

函數回傳值為標的價格每變動1％，選擇權Delta變化的百分比。Gamma永遠為正值，選擇權在價平時Gamma值最大。例如，假設選擇權的Delta為0.56、Gamma為0.0015，則表示只要標的價格上漲1點，Delta會由0.56增加至0.5615（0.56+1\*0.0015=0.5615）

範例：

```
value1 = BSGamma("C",8800,9000,20,2,0,25);       //計算波動率25%、20天後到期之台指選擇權9000的Call在指數為8800點的Gamma值
plot1(value1, "Gamma");
```

註：更多參數說明請參考[BlackScholes公式](api?a=BlackScholesModel&b=sys)。

---
## BSPrice
**Path:** 系統函數 > 期權相關

BSPrice -  (系統函數)
期權相關

語法：

以BlackScholes模型計算選擇權理論價  
回傳數值=BSPrice(買賣權,標的價格,履約價,到期天數,無風險利率,持有成本,波動率)  
傳入七個參數:  
- 第一個參數是買賣權，C表買權、P表賣權。  
- 第二個參數是標的價格。  
- 第三個參數是履約價。  
- 第四個參數是到期天數。  
- 第五個參數是無風險利率，百分比值。若無風險利率為2%，請輸入2。  
- 第六個參數是持有成本，百分比值。若持有成本為1%，請輸入1。  
- 第七個參數是波動率，百分比值。若持有成本為30%，請輸入30。

說明：

BS選擇權定價模型為諾貝爾經濟學獎得主Robert Merton和Myron Scholes於1973所發表。依據下列六個參數決定選擇權的理論價：標的價格、履約價、到期天數、無風險利率、持有成本、波動率。其中只有履約價和到期天數是由合約所規定，其餘參數皆會隨市場狀況而變動。

範例：

```
value1 = BSPrice("C",8800,9000,20,2,0,25);       //計算波動率25%、20天後到期之台指選擇權9000的Call在指數為8800點的理論價
plot1(value1, "理論價");
```

註：更多參數說明請參考[BlackScholes公式](api?a=BlackScholesModel&b=sys)。

---
## BSTheta
**Path:** 系統函數 > 期權相關

BSTheta -  (系統函數)
期權相關

語法：

以BlackScholes模型計算選擇權Theta  
回傳數值=BSTheta(買賣權,標的價格,履約價,到期天數,無風險利率,持有成本,波動率)  
傳入七個參數:  
- 第一個參數是買賣權，C表買權、P表賣權。  
- 第二個參數是標的價格。  
- 第三個參數是履約價。  
- 第四個參數是到期天數。  
- 第五個參數是無風險利率，百分比值。若無風險利率為2%，請輸入2。  
- 第六個參數是持有成本，百分比值。若持有成本為1%，請輸入1。  
- 第七個參數是波動率，百分比值。若持有成本為30%，請輸入30。

說明：

Theta為選擇權敏感度分析的參數之一，用來衡量時間變化對選擇權價格的影響。

函數回傳值為選擇權剩餘天數每減少一天，選擇權理論價的變化。Theta永遠為負值，表示選擇權的時間價值隨著到期日的接近而消失。如果選擇權的Theta為-2，在所有條件維持不變的情況下，明日選擇權的理論價會由今天的70下降為68（70-1\*2=68）。

範例：

```
value1 = BSTheta("C",8800,9000,20,2,0,25);       //計算波動率25%、20天後到期之台指選擇權9000的Call在指數為8800點的Theta值
plot1(value1, "Theta");
```

註：更多參數說明請參考[BlackScholes公式](api?a=BlackScholesModel&b=sys)。

---
## BSVega
**Path:** 系統函數 > 期權相關

BSVega -  (系統函數)
期權相關

語法：

以BlackScholes模型計算選擇權Vega  
回傳數值=BSVega(買賣權,標的價格,履約價,到期天數,無風險利率,持有成本,波動率)  
傳入七個參數:  
- 第一個參數是買賣權，C表買權、P表賣權。  
- 第二個參數是標的價格。  
- 第三個參數是履約價。  
- 第四個參數是到期天數。  
- 第五個參數是無風險利率，百分比值。若無風險利率為2%，請輸入2。  
- 第六個參數是持有成本，百分比值。若持有成本為1%，請輸入1。  
- 第七個參數是波動率，百分比值。若持有成本為30%，請輸入30。

說明：

BS選擇權定價模型為諾貝爾經濟學獎得主Robert Merton和Myron Scholes於1973所發表。依據下列六個參數決定選擇權的理論價：標的價格、履約價、到期天數、無風險利率、持有成本、波動率。其中只有履約價和到期天數是由合約所規定，其餘參數皆會隨市場狀況而變動。

範例：

```
value1 = BSPrice("C",8800,9000,20,2,0,25);       //計算波動率25%、20天後到期之台指選擇權9000的Call在指數為8800點的理論價
plot1(value1, "理論價");
```

註：更多參數說明請參考[BlackScholes公式](api?a=BlackScholesModel&b=sys)。

---
## DaysToExpirationTF
**Path:** 系統函數 > 期權相關

DaysToExpirationTF -  (系統函數)
期權相關

語法：

自動計算台股指數類期貨、選擇權商品的到期天數。  
回傳數值=DaysToExpirationTF

說明：

自動依目前執行的商品計算對應的到期天數。支援週選擇權。

回傳值為1，表示當天為結算日。回傳值小於1，表示該合約已到期。

範例：

```
value1 = DaysToExpirationTF;
plot1(value1); //繪製最新的台股指數類期貨到期天數的連線
```

注意，此函數並無調整因放假而導致的到期日異動。

---
## HVolatility
**Path:** 系統函數 > 期權相關

HVolatility -  (系統函數)
期權相關

語法：

計算序列資料的波動率。  
回傳數值=HVolatility(數列,期數)  
傳入二個參數:  
- 第一個參數是數列，通常是開高低收的價格數列。  
- 第二個參數是期數。

說明：

歷史波動率代表的是價格過往的波動性高低。波動率是影響選擇權價格的重要因子，而歷史波動率會是一個很好的參考。

範例：

```
value1 = HVolatility(Close,20);       //計算20天的歷史波動率
plot1(value1,"20天歷史波動率");
```

---
## IVolatility
**Path:** 系統函數 > 期權相關

IVolatility -  (系統函數)
期權相關

語法：

計算選擇權的隱含波動率  
回傳數值=IVolatility(買賣權,標的價格,履約價,到期天數,無風險利率,持有成本,選擇權現價)  
傳入七個參數:  
- 第一個參數是買賣權，C表買權、P表賣權。  
- 第二個參數是標的價格。  
- 第三個參數是履約價。  
- 第四個參數是到期天數。  
- 第五個參數是無風險利率，百分比值。若無風險利率為2%，請輸入2。  
- 第六個參數是持有成本，百分比值。若持有成本為1%，請輸入1。  
- 第七個參數是選擇權現價。

說明：

隱含波動率是經由選擇權市場價格反推而得的波動率。隱含波動率的水準顯示了選擇權價格的高低（貴或便宜），也代表了市場參與者對未來的預期。通常會搭配歷史波動率來判斷市場是否出現狂熱或恐慌的狀況。

範例：

```
value1 = IVolatility("C",8800,9000,20,2,0,40);       //計算8800買權的隱含波動率
plot1(value1,"隱含波動率");
```

---
## IsXLOrder
**Path:** 系統函數 > 期權相關

IsXLOrder -  (系統函數)
期權相關

語法：

判斷成交金額是否是特大單。  
回傳布林值=IsXLOrder

說明：

級距表請參考：
[台股逐筆功能行情端相關異動](https://www.xq.com.tw/%e5%8f%b0%e8%82%a1%e9%80%90%e7%ad%86%e5%8a%9f%e8%83%bd%e8%a1%8c%e6%83%85%e7%ab%af%e7%9b%b8%e9%97%9c%e7%95%b0%e5%8b%95/)

---
## IsXOrder
**Path:** 系統函數 > 期權相關

IsXOrder -  (系統函數)
期權相關

語法：

回傳目前的成交金額是否是大單(大單+特大單)。  
回傳布林值=IsXOrder

說明：

級距表請參考：
[台股逐筆功能行情端相關異動](https://www.xq.com.tw/%e5%8f%b0%e8%82%a1%e9%80%90%e7%ad%86%e5%8a%9f%e8%83%bd%e8%a1%8c%e6%83%85%e7%ab%af%e7%9b%b8%e9%97%9c%e7%95%b0%e5%8b%95/)

---
## NORMSDIST
**Path:** 系統函數 > 期權相關

NORMSDIST -  (系統函數)
期權相關

語法：

計算數值的標準常態累加分配函數。  
回傳數值=NORMSDIST(數值)  
傳入一個參數:  
- 第一個參數是數值。

說明：

計算在平均值為0、標準差為1的標準常態分配下，數值小於傳入值的累計機率。

本函數利用多項式計算近似值，精確度到小數點以下六位。

範例：

```
value1 = NORMSDIST(1.33);       //1.33的標準常態累加分配函數
plot1(value1);
```

---
## blackscholesmodel
**Path:** 系統函數 > 期權相關

blackscholesmodel -  (系統函數)
期權相關

語法：

利用BlackScholes選擇權評價模型計算理論價及Greeks  
回傳數值=BlackScholesModel (買賣權,標的價格,履約價,到期天數,無風險利率,持有成本,波動率,輸出理論價,輸出Delta,輸出Gamma,輸出Vega,輸出Theta,輸出Rho)  
傳入十三個參數:  
- 第一個參數是買賣權，C表買權、P表賣權。  
- 第二個參數是標的價格。  
- 第三個參數是履約價。  
- 第四個參數是到期天數。  
- 第五個參數是無風險利率，百分比值。若無風險利率為2%，請輸入2。  
- 第六個參數是持有成本，百分比值。若持有成本為1%，請輸入1。  
- 第七個參數是波動率，百分比值。若持有成本為30%，請輸入30。  
- 第八個參數為傳址參數，會回傳計算完的選擇權理論價。  
- 第九個參數為傳址參數，會回傳計算完的選擇權Delta。  
- 第十個參數為傳址參數，會回傳計算完的選擇權Gamma。  
- 第十一個參數為傳址參數，會回傳計算完的選擇權Vega。  
- 第十二個參數為傳址參數，會回傳計算完的選擇權Theta。  
- 第十三個參數為傳址參數，會回傳計算完的選擇權Rho。

說明：

BS選擇權定價模型為諾貝爾經濟學獎得主Robert Merton和Myron Scholes於1973所發表。依據下列六個參數決定選擇權的理論價：標的價格、履約價、到期天數、無風險利率、持有成本、波動率。其中只有履約價和到期天數是由合約所規定，其餘參數皆會隨市場狀況而變動。

持有成本會因標的商品的不同而異：

* 股票選擇權的持有成本為無風險利率（r）-股票殖利率
* 期貨選擇權的持有成本為0
* 外匯選擇權的持有成本為無風險利率（r）-外國無風險利率

這個函數可以依照使用者傳入的參數，計算選擇權的理論價、Delta、Gamma、Vega、Theta及Rho。

範例：

```
value1 = BlackScholesModel("C",8800,9000,20,2,0,25,value2,value3,value4,value5,value6,value7);       //計算波動率25%、20天後到期之台指選擇權9000的Call在指數為8800點的理論價
plot1(value2, "理論價");   
plot2(value3, "Delta");   
plot3(value4, "Gamma");   
plot4(value5, "Vega");   
plot5(value6, "Theta");   
plot6(value7, "Rho");
```

---
## CoefficientR
**Path:** 系統函數 > 統計分析

CoefficientR -  (系統函數)
統計分析

語法：

計算兩個數列的Pearson積差相關係數。  
回傳數值=CoefficientR(數列一,數列二,期數)  
傳入三個參數:  
- 第一個參數是第一個數列，通常是開高低收的價格數列。  
- 第二個參數是第二個數列，通常是開高低收的價格數列。  
- 第三個參數是期數。

說明：

以最新一筆資料為基準點，輸入要計算的期數，然後計算過去期數的相關係數。

回傳數值會介於-1到1之間。如果無法計算，會傳回-2。

範例：

```
value1 = GetField("外資買賣超金額");
value2 = rateofchange(close,1);          //計算當日漲跌幅
value3 = coefficientr(value1,value2,20); //計算外資買賣超金額與大盤漲跌幅的相關係數
plot1(value3);
```

---
## Correlation
**Path:** 系統函數 > 統計分析

Correlation -  (系統函數)
統計分析

語法：

計算兩個數列的相關係數。  
回傳數值=Correlation(數列一,數列二,期數)  
傳入三個參數:  
- 第一個參數是第一個數列，通常是開高低收的價格數列。  
- 第二個參數是第二個數列，通常是開高低收的價格數列。  
- 第三個參數是期數。

說明：

以最新一筆資料為基準點，輸入要計算的期數，然後計算過去期數的相關係數。

回傳數值會介於-1到1之間。如果無法計算，會傳回-2。

範例：

```
value1 = GetField("外資買賣超金額");
value2 = rateofchange(close,1);          //計算當日漲跌幅
value3 = Correlation(value1,value2,20); //計算外資買賣超金額與大盤漲跌幅的相關係數
plot1(value3);
```

---
## Covariance
**Path:** 系統函數 > 統計分析

Covariance -  (系統函數)
統計分析

語法：

計算兩個數列的共變異數。  
回傳數值=Covariance(數列一,數列二,期數)  
傳入三個參數:  
- 第一個參數是第一個數列，通常是開高低收的價格數列。  
- 第二個參數是第二個數列，通常是開高低收的價格數列。  
- 第三個參數是期數。

說明：

以最新一筆資料為基準點，輸入要計算的期數，然後計算過去期數的共變異數。

範例：

```
value1 = GetField("外資買賣超金額");
value2 = rateofchange(close,1);        //計算當日漲跌幅
value3 = Covariance(value1,value2,20); //計算外資買賣超金額與大盤漲跌幅的共變異數
plot1(value3);
```

---
## RSquare
**Path:** 系統函數 > 統計分析

RSquare -  (系統函數)
統計分析

語法：

計算兩個數列的R平方值。  
回傳數值=RSquare(數列一,數列二,期數)  
傳入三個參數:  
- 第一個參數是第一個數列，通常是開高低收的價格數列。  
- 第二個參數是第二個數列，通常是開高低收的價格數列。  
- 第三個參數是期數。

說明：

以最新一筆資料為基準點，輸入要計算的期數，然後計算過去期數的R平方值。

範例：

```
value1 = GetField("外資買賣超金額");
value2 = rateofchange(close,1);          //計算當日漲跌幅
value3 = RSquare(value1,value2,20); //計算外資買賣超金額與大盤漲跌幅的R平方值
plot1(value3);
```

---
## StandardDev
**Path:** 系統函數 > 統計分析

StandardDev -  (系統函數)
統計分析

語法：

計算數列的標準差。  
回傳數值=StandardDev(數列,期數,母體或樣本)  
傳入三個參數:  
- 第一個參數是數列，通常是開高低收的價格數列。  
- 第二個參數是期數。  
- 第三個參數是指定計算是母體標準差(1)或樣本標準差(2)。

說明：

以最新一筆資料為基準點，輸入要計算的期數，然後計算過去期數的標準差。

提供母體（Population）或樣本（Sample）二種計算方式。

範例：

```
value1 = StandardDev(close,20,2); //計算收盤價的標準差
plot1(value1);
```

---
## VariancePS
**Path:** 系統函數 > 統計分析

VariancePS -  (系統函數)
統計分析

語法：

計算數列的變異數。  
回傳數值=VariancePS(數列,期數,母體或樣本)  
傳入三個參數:  
- 第一個參數是數列，通常是開高低收的價格數列。  
- 第二個參數是期數。  
- 第三個參數是指定計算是母體變異數(1)或樣本變異數(2)。

說明：

以最新一筆資料為基準點，輸入要計算的期數，然後計算過去期數的變異數。

提供母體（Population）或樣本（Sample）二種計算方式。

範例：

```
value1 = VariancePS(close,20,2); //計算收盤價的變異數
plot1(value1);
```

---
## Angle
**Path:** 系統函數 > 趨勢分析

Angle -  (系統函數)
趨勢分析

語法：

計算任意二個日期的走勢角度。  
回傳數值=Angle(日期1,日期2)  
傳入二個參數:  
- 第一個參數是日期1。  
- 第二個參數是日期2，需大於日期1。

說明：

任意二個日期間的價格角度可以代表趨勢的強度。我們利用第一個日期的開盤價及第二個日期的收盤價計算整段走勢的角度。

當回傳數值大於0時，代表趨勢向上；當回傳數值小於0時，代表趨勢向下。

範例：

```
value1 = Angle(date[3],date); //計算近四期走勢的角度
```

---
## LinearReg
**Path:** 系統函數 > 趨勢分析

LinearReg -  (系統函數)
趨勢分析

語法：

計算線性迴歸的斜率與角度，以及預測資料投影點的位置。  
回傳數值=LinearReg(數列,期數,預測值的相對K棒位置,輸出斜率,輸出弧度,輸出X軸截距,輸出預測值)  
傳入七個參數:  
- 第一個參數是數列，通常是開高低收的價格數列。  
- 第二個參數是期數。  
- 第三個參數是預測值的相對K棒位置，和序列引用定義相同，0表當期、1表前一期、-1表後一期。  
- 第四個參數是輸出計算完的線性回歸線斜率。  
- 第五個參數是輸出計算完的線性回歸線弧度。  
- 第六個參數是輸出計算完的線性回歸線X軸截距。  
- 第七個參數是輸出計算完的線性回歸線預測值。

說明：

LinearReg函數回傳1時，代表計算成功。斜率、弧度、X軸截距及預測值是回傳在第4、5、6、7個參數。

範例：

```
value1 = linearreg(close,20,-1,value2,value3,value4,value5); //計算收盤價20期的線性迴歸
plot1(value5);                                               //繪製明天的收盤價線性迴歸預測值連線
```

---
## LinearRegAngle
**Path:** 系統函數 > 趨勢分析

LinearRegAngle -  (系統函數)
趨勢分析

語法：

計算線性迴歸的弧度。  
回傳數值=LinearRegAngle(數列,期數)  
傳入二個參數:  
- 第一個參數是數列，通常是開高低收的價格數列。  
- 第二個參數是期數。

說明：

利用最小平方法計算線性回歸的角度。

範例：

```
value1 = LinearRegAngle(close,20); //計算收盤價20期的線性迴歸線角度
```

---
## LinearRegSlope
**Path:** 系統函數 > 趨勢分析

LinearRegSlope -  (系統函數)
趨勢分析

語法：

計算線性迴歸的斜率。  
回傳數值=LinearRegSlope(數列,期數)  
傳入二個參數:  
- 第一個參數是數列，通常是開高低收的價格數列。  
- 第二個參數是期數。

說明：

利用最小平方法計算線性回歸的斜率。

範例：

```
value1 = LinearRegSlope(close,20); //計算收盤價20期的線性迴歸線斜率
```

---
## SwingHigh
**Path:** 系統函數 > 趨勢分析

SwingHigh -  (系統函數)
趨勢分析

語法：

計算數列最近N個的轉折高點數值。  
回傳數值=SwingHigh(數列,期數,左肩期數,右肩期數,第幾個高點)  
傳入五個參數:  
- 第一個參數是數列，通常是開高低收的價格數列。  
- 第二個參數是要尋找轉折點的樣本期數。  
- 第三個參數是高點左側要有幾筆較低的數值。  
- 第四個參數是高點右側要有幾筆較低的數值。  
- 第五個參數是第幾個高點，依圖表由右往左(時間新到舊)，1為最近一次的高點、2為第二近的高點。

說明：

若某一筆資料的左右二側數值在指定的期數內都比該筆資料低的話，則定義為轉折高點。

當無法找到對應的轉折高點時，回傳值為-1。

範例：

```
value1 = SwingHigh(High,20,3,3,2); //找出過去20期內，第2個轉折高點
```

---
## SwingHighBar
**Path:** 系統函數 > 趨勢分析

SwingHighBar -  (系統函數)
趨勢分析

語法：

計算數列最近N個的轉折高點的相對位置。  
回傳數值=SwingHighBar(數列,期數,左肩期數,右肩期數,第幾個高點)  
傳入五個參數:  
- 第一個參數是數列，通常是開高低收的價格數列。  
- 第二個參數是要尋找轉折點的樣本期數。  
- 第三個參數是高點左側要有幾筆較低的數值。  
- 第四個參數是高點右側要有幾筆較低的數值。  
- 第五個參數是第幾個高點，依圖表由右往左(時間新到舊)，1為最近一次的高點、2為第二近的高點。

說明：

若某一筆資料的左右二側數值在指定的期數內都比該筆資料低的話，則定義為轉折高點。

當無法找到對應的轉折高點時，回傳值為-1。

範例：

```
value1 = SwingHighBar(High,20,3,3,2); //找出過去20期內，第2個轉折高點的相對位置
```

---
## SwingLow
**Path:** 系統函數 > 趨勢分析

SwingLow -  (系統函數)
趨勢分析

語法：

計算數列最近N個的轉折低點數值。  
回傳數值=SwingLow(數列,期數,左肩期數,右肩期數,第幾個低點)  
傳入五個參數:  
- 第一個參數是數列，通常是開高低收的價格數列。  
- 第二個參數是要尋找轉折點的樣本期數。  
- 第三個參數是高點左側要有幾筆較高的數值。  
- 第四個參數是高點右側要有幾筆較高的數值。  
- 第五個參數是第幾個低點，依圖表由右往左(時間新到舊)，1為最近一次的低點、2為第二近的低點。

說明：

若某一筆資料的左右二側數值在指定的期數內都比該筆資料高的話，則定義為轉折低點。

當無法找到對應的轉折低點時，回傳值為-1。

範例：

```
value1 = SwingLow(Low,20,3,3,1); //找出過去20期內，第1個轉折低點
```

---
## SwingLowBar
**Path:** 系統函數 > 趨勢分析

SwingLowBar -  (系統函數)
趨勢分析

語法：

計算數列最近N個的轉折低點的相對位置。  
回傳數值=SwingLowBar(數列,期數,左肩期數,右肩期數,第幾個低點)  
傳入五個參數:  
- 第一個參數是數列，通常是開高低收的價格數列。  
- 第二個參數是要尋找轉折點的樣本期數。  
- 第三個參數是高點左側要有幾筆較高的數值。  
- 第四個參數是高點右側要有幾筆較高的數值。  
- 第五個參數是第幾個低點，依圖表由右往左(時間新到舊)，1為最近一次的低點、2為第二近的低點。

說明：

若某一筆資料的左右二側數值在指定的期數內都比該筆資料高的話，則定義為轉折低點。

當無法找到對應的轉折低點時，回傳值為-1。

範例：

```
value1 = SwingLowBar(Low,20,3,3,1); //找出過去20期內，第1個轉折低點的相對位置
```

---
## TSELSindex
**Path:** 系統函數 > 趨勢分析

TSELSindex -  (系統函數)
趨勢分析

語法：

利用外資買賣超金額計算大盤的多空狀態。  
回傳數值=TSELSindex(期數,標準)  
傳入二個參數:  
- 第一個參數是期數，指定要計算的區間。  
- 第二個參數是標準，要買超至少幾日才算多頭。

說明：

詳細說明請參考[交易的點點滴滴](https://xstrader.net/%E6%89%93%E9%80%A0%E8%87%AA%E5%B7%B1%E7%9A%84%E5%A4%A7%E7%9B%A4%E5%A4%9A%E7%A9%BA%E5%87%BD%E6%95%B8/)。

---
## TSEMFI
**Path:** 系統函數 > 趨勢分析

TSEMFI -  (系統函數)
趨勢分析

語法：

利用大盤的資金流向指標來判斷多空方向  
回傳數值 = TSEMFI  
如果是多頭，回傳1，如果是空頭，回傳0

說明：

利用大盤的資金流向指標來判斷多空方向  
回傳數值 = TSEMFI  
如果是多頭，回傳1，如果是空頭，回傳0

---
## TimeSeriesForecast
**Path:** 系統函數 > 趨勢分析

TimeSeriesForecast -  (系統函數)
趨勢分析

語法：

計算線性迴歸預測資料投影點的位置。  
回傳數值=TimeSeriesForecast(數列,期數,預測值的相對K棒位置)  
傳入三個參數:  
- 第一個參數是數列，通常是開高低收的價格數列。  
- 第二個參數是期數。  
- 第三個參數是預測值的相對K棒位置，和序列引用定義相同，0表當期、1表前一期、-1表後一期。

說明：

利用最小平方法計算線性回歸的預測值。

範例：

```
value1 = TimeSeriesForecast(close,20,-1); //計算收盤價20期的線性迴歸
plot1(value1);                            //繪製明天的收盤價線性迴歸預測值連線
```

---
## UpShadow
**Path:** 系統函數 > 趨勢分析

UpShadow -  (系統函數)
趨勢分析

語法：

回傳目前K棒上影線佔整根K棒的比例。  
回傳數值=UpShadow

說明：

範例：

```
value1 =UpShadow; //計算當期K棒之上影線佔整根K棒比例
value2 =UpShadow[1]; //計算前一期K棒之上影線佔整根K棒比例
```

---
## BollingerBandWidth
**Path:** 系統函數 > 跨頻率

BollingerBandWidth -  (系統函數)
跨頻率

語法：

計算布林帶寬度指標（Bandwidth，BW）。  
回傳數值=BollingerBandWidth(數列,期數,上通道寬度的標準差倍數,下通道寬度的標準差倍數)  
傳入四個參數:  
- 第一個參數是數列，通常是開高低收的價格數列。  
- 第二個參數是計算均線的期數。  
- 第三個參數是決定上通道寬度的標準差倍數。  
- 第四個參數是決定下通道寬度的標準差倍數。

說明：

布林帶寬度指標（Bandwidth，BW），是由布林帶中軌及上、下軌衍生出的指標，利用股價波動範圍以判斷趨勢的強度與轉折；計算公式通常為：布林帶寬度指標值 = （布林帶上軌值 - 布林帶下軌值）÷ 布林帶中軌值。

範例：

```
value1 = BollingerBandWidth(Close,20,2,2);	//計算20期、上下通道寬度的標準差倍數為2
plot1(value1, "BollingerBandWidth");
```

---
## PercentB
**Path:** 系統函數 > 跨頻率

PercentB -  (系統函數)
跨頻率

語法：

計算%b指標（Percent b，PB）。  
回傳數值=PercentB(數列,期數,上通道寬度的標準差倍數,下通道寬度的標準差倍數)  
傳入四個參數:  
- 第一個參數是數列，通常是開高低收的價格數列。  
- 第二個參數是計算均線的期數。  
- 第三個參數是決定上通道寬度的標準差倍數。  
- 第四個參數是決定下通道寬度的標準差倍數。

說明：

%b指標（Percent b，PB），是從布林值演化過來的，了解%B指標前，建議熟悉布林通道(BollingerBand)；%b指標以數值形式呈現收盤價在布林帶中的位置，計算公式通常為：%b 值 = （收盤價 - 布林帶下軌值） ÷ （布林帶上軌值 - 布林帶下軌值）。

範例：

```
value1 = PercentB(Close,20,2,2);	//計算20期、上下通道寬度的標準差倍數為2
plot1(value1, "%b指標");
```

---
## TurnOverRate
**Path:** 系統函數 > 跨頻率

TurnOverRate -  (系統函數)
跨頻率

語法：

計算股票周轉率（或稱換手率）。  
回傳數值=TurnOverRate(期數)

說明：

股票周轉率是在一段時間內的股票交易數量與股票發行總數之比。當週轉率高時，代表股票在投資人之間轉換頻率過高，也表示市場的流動性高；週轉率高也可能是因為市場中充斥太多投機者搶短線所造成的。

範例：

```
value1 = TurnOverRate(10);	//計算10期的周轉率
plot1(value1, "周轉率");
```

---
## xfMin_CrossOver
**Path:** 系統函數 > 跨頻率

xfMin\_CrossOver -  (系統函數)
跨頻率

語法：

判斷指定頻率的數列一是否由下往上穿越數列二，又稱黃金交叉。  
回傳布林值=xfMin\_CrossOver(頻率,數列一,數列二)  
傳入三個參數:  
- 第一個參數是頻率，指定傳入數列的資料期別，支援"1","5","10","15","30","60","D", "W", "M", "AD", "AW", "AM"。  
- 第二個參數是目標頻率的數列一。  
- 第三個參數是目標頻率的數列二。  
備註：商品類型僅支援台股與台期權。不支援XS選股、XS選股自訂排行與XS選股回測。

說明：

如果出現黃金交叉傳回True，其他狀況傳回False。

範例：

```
condition1 = xfMin_CrossOver("30",Average(GetField("收盤價","30"),5),Average(GetField("收盤價","30") ,10)); //判斷30分鐘線5期均線和10期均線是否黃金交叉
```

---
## xfMin_CrossUnder
**Path:** 系統函數 > 跨頻率

xfMin\_CrossUnder -  (系統函數)
跨頻率

語法：

判斷指定頻率的數列一是否由上往下穿越數列二，又稱死亡交叉。  
回傳布林值=xfMin\_CrossUnder(頻率,數列一,數列二)  
傳入三個參數:  
- 第一個參數是頻率，指定傳入數列的資料期別，支援"1","5","10","15","30","60","D", "W", "M", "AD", "AW", "AM"。  
- 第二個參數是目標頻率的數列一。  
- 第三個參數是目標頻率的數列二。  
備註：商品類型僅支援台股與台期權。不支援XS選股、XS選股自訂排行與XS選股回測。

說明：

如果出現死亡交叉傳回True，其他狀況傳回False。

範例：

```
condition1 = xfMin_CrossUnder("30",Average(GetField("close","30"),5),Average(GetField("close","30") ,10) ); //判斷30分鐘線5期均線和10期均線是否死亡交叉
```

---
## xfMin_DirectionMovement
**Path:** 系統函數 > 跨頻率

xfMin\_DirectionMovement -  (系統函數)
跨頻率

語法：

計算跨頻率DMI指標。  
回傳數值=xfMin\_DirectionMovement(頻率,期數,輸出+DI值,輸出-DI值,輸出ADX值)  
傳入五個參數:  
- 第一個參數是頻率，指定傳入數列的資料期別，支援"1","5","10","15","30","60","D", "W", "M", "AD", "AW", "AM"。  
- 第二個參數是計算期數。  
- 第三個參數是輸出計算完的+DI值。  
- 第四個參數是輸出計算完的-DI值。  
- 第五個參數是輸出計算完的ADX值。  
備註：商品類型僅支援台股與台期權。不支援XS選股、XS選股自訂排行與XS選股回測。

說明：

xfMin\_DirectionMovement是[xf\_DirectionMovement](api?a=xf_DirectionMovement&b=sys) 函數的跨頻率加強版本，增加了指定分鐘頻率的參數，可以計算指定分鐘頻率的DMI值。

範例：

```
value1 = xfMin_DirectionMovement("30",14,value2,value3,value4);       //計算14期的30分鐘線DMI指標
plot1(value2, "30分+DI");
plot2(value3, "30分週-DI");
plot3(value4, "30分ADX");
```

---
## xfMin_EMA
**Path:** 系統函數 > 跨頻率

xfMin\_EMA -  (系統函數)
跨頻率

語法：

計算指定頻率的XQ指數移動平均。  
回傳數值=xfMin\_EMA(頻率,數列,期數)  
傳入三個參數:  
- 第一個參數是頻率，指定傳入數列的資料期別，支援"1","5","10","15","30","60","D", "W", "M", "AD", "AW", "AM"。  
- 第二個參數是目標頻率的數列，通常是開高低收的價格數列。  
- 第三個參數是期數。  
備註：商品類型僅支援台股與台期權。不支援XS選股、XS選股自訂排行與XS選股回測。

說明：

xfMin\_EMA是[xf\_EMA](api?a=xf_EMA&b=sys)函數的跨頻率加強版本，增加了指定分鐘頻率的參數，可以計算指定分鐘頻率的EMA值。

範例：

```
value1 = xfMin_EMA("30", GetField("Close", "30"),5); //計算30分鐘線5期收盤價的XQ EMA
```

---
## xfMin_GetBoolean
**Path:** 系統函數 > 跨頻率

xfMin\_GetBoolean -  (系統函數)
跨頻率

語法：

引用指定頻率的數值。  
回傳數值=xfMin\_GetBoolean(頻率,數列,期別)  
傳入三個參數:  
- 第一個參數是頻率，指定傳入數列的資料期別，支援"1","5","10","15","30","60","D", "W", "M", "AD", "AW", "AM"。  
- 第二個參數是布林數列。  
- 第三個參數是期別，相對目前而言要往前的筆數。  
備註：商品類型僅支援台股與台期權。不支援XS選股、XS選股自訂排行與XS選股回測。

說明：

在同一個頻率時，我們可以直接利用\*\*變數[3]\*\*取得前3期的變數值。當資料頻率不同時（跨頻率），我們就需要使用xfMin\_GetValue或xfMin\_GetBoolean來取得之前的變數值。若變數是數值時，要用xfMin\_GetValue；若變數是布林值時，要用xfMin\_GetBoolean。支援跨分鐘頻率。

```
input:Length_Min(9,"跨分鐘頻率期數");
variable:rsv_w(0),kk_w(0),dd_w(0);
xfMin_stochastic("30", Length_Min, 3, 3, rsv_w, kk_w, dd_w);
condition1 = xfMin_GetBoolean("30",xfMin_crossover("30", kk_w, dd_w),1);	//在15分鐘線抓30分鐘線KD黃金交叉
```

相關函數：[xfMin\_GetValue](api?a=xfMin_GetValue&b=sys)。

---
## xfMin_GetCurrentBar
**Path:** 系統函數 > 跨頻率

xfMin\_GetCurrentBar -  (系統函數)
跨頻率

語法：

傳回指定頻率的K棒編號。  
K棒編號 = xfMin\_GetCurrentBar(頻率)  
傳入一個參數:  
- 第一個參數是頻率，指定傳入數列的資料期別，支援"1","5","10","15","30","60","D", "W", "M", "AD", "AW", "AM"。  
備註：商品類型僅支援台股與台期權。不支援XS選股、XS選股自訂排行與XS選股回測。

說明：

傳回指定頻率（支援分鐘）的K棒序列編號，由1開始，第一筆K棒編號為1，第二筆K棒編號為2，依序遞增。

可以使用這個函數來判斷目前腳本執行的時機點

```
value1 = xfMin_GetCurrentBar(FreqType);


if Length + 1 = 0 then Factor = 1 else Factor = 2 / (Length + 1);


if value1 = 1 then
    xfMin_XAverage = Series
else
    xfMin_XAverage = lastXAverage + Factor * (Series - lastXAverage);
```

上述範例利用xfMin\_GetCurrentBar來判斷目前是否是第一筆K棒。如果是的話則回傳xfMin\_XAverage的初始數值。

---
## xfMin_GetDTValue
**Path:** 系統函數 > 跨頻率

xfMin\_GetDTValue -  (系統函數)
跨頻率

語法：

計算指定頻率的序列值。  
回傳數值=xfMin\_GetDTValue(頻率,日期)  
傳入二個參數:  
- 第一個參數是頻率，指定傳入數列的資料期別，支援"1","5","10","15","30","60","D", "W", "M", "AD", "AW", "AM"。  
- 第二個參數是日期。  
備註：商品類型僅支援台股與台期權。不支援XS選股、XS選股自訂排行與XS選股回測。

說明：

經由傳入的日期判斷指定頻率的期別是否有異動，支援指定分鐘頻率。

```
value1 = xfMin_getdtvalue("30",date);
if value1 <> value1[1] then plot1(1) else plot1(0);
```

上述範例利用xfMin\_GetDTValue來判斷目前是否為新的30分鐘。如果是的話則在圖表上顯示為1。

---
## xfMin_GetValue
**Path:** 系統函數 > 跨頻率

xfMin\_GetValue -  (系統函數)
跨頻率

語法：

引用指定頻率的數值。  
回傳數值=xfMin\_GetValue(頻率,數列,期別)  
傳入三個參數:  
- 第一個參數是頻率，指定傳入數列的資料期別，支援"1","5","10","15","30","60","D", "W", "M", "AD", "AW", "AM"。  
- 第二個參數是目標頻率的數列。  
- 第三個參數是期別，相對目前而言要往前的筆數。  
備註：商品類型僅支援台股與台期權。不支援XS選股、XS選股自訂排行與XS選股回測。

說明：

在同一個頻率時，我們可以直接利用\*\*變數[3]\*\*取得前3期的變數值。當資料頻率不同時（跨頻率），我們就需要使用xfMin\_GetValue或xfMin\_GetBoolean來取得之前的變數值。若變數是數值時，要用xfMin\_GetValue；若變數是布林值時，要用xfMin\_GetBoolean。支援跨分鐘頻率。

```
value1 = xfMin_WeightedClose("30");            //計算30分鐘線的加權平均價
value2 = xfMin_GetValue("30",value1,1);        //取得上一期30分鐘線的加權平均價
plot1(value2);
plot2(value1[1]);                        //可以比較一下和value2的差異
```

相關函數：[xfMin\_GetBoolean](api?a=xfMin_GetBoolean&b=sys)。

---
## xfMin_MACD
**Path:** 系統函數 > 跨頻率

xfMin\_MACD -  (系統函數)
跨頻率

語法：

計算指定頻率的MACD指標值。  
回傳數值=xfMin\_MACD(頻率,數列,短期數,長期數,MACD平滑期數,輸出DIF值,輸出MACD值,輸出OSC值)  
傳入八個參數:  
- 第一個參數是頻率，指定傳入數列的資料期別，支援"1","5","10","15","30","60","D", "W", "M", "AD", "AW", "AM"。  
- 第二個參數是目標頻率的數列，MACD通常是以加權平均收盤價（WeightedClose）來計算。  
- 第三個參數是計算快速線（短期）的期數。  
- 第四個參數是計算慢速線（長期）的期數。  
- 第五個參數是計算MACD使用之平滑期數。  
- 第六個參數是輸出計算完的DIF值。  
- 第七個參數是輸出計算完的MACD值。  
- 第八個參數是輸出計算完的OSC值。  
備註：商品類型僅支援台股與台期權。不支援XS選股、XS選股自訂排行與XS選股回測。

說明：

xfMin\_MACD是[xf\_MACD](api?a=xf_MACD&b=sys) 函數的跨頻率加強版本，增加了指定分鐘頻率的參數，可以計算指定分鐘頻率的MACD值。

範例：

```
value1 = xfMin_MACD("30",xfMin_weightedclose("30"),12,26,9,value2,value3,value4);    //計算30分鐘線MACD
plot1(value2, "30分鐘DIF");
plot2(value3, "30分鐘MACD");
plot3(value4, "30分鐘OSC");
```

---
## xfMin_PercentR
**Path:** 系統函數 > 跨頻率

xfMin\_PercentR -  (系統函數)
跨頻率

語法：

計算指定頻率的威廉指標值。  
回傳數值=xfMin\_PercentR(頻率,期數)  
傳入二個參數:  
- 第一個參數是頻率，指定傳入數列的資料期別，支援"1","5","10","15","30","60","D", "W", "M", "AD", "AW", "AM"。  
- 第二個參數是計算威廉指標的期數。  
備註：商品類型僅支援台股與台期權。不支援XS選股、XS選股自訂排行與XS選股回測。

說明：

xfMin\_PercentR是[xf\_PercentR](api?a=xf_PercentR&b=sys) 函數的跨頻率加強版本，增加了指定分鐘頻率的參數，可以計算指定分鐘頻率的PercentR值。

範例：

```
value1 = xfMin_PercentR("30", 14) - 100;       //計算30分鐘線威廉指標
Plot1(value1, "30分鐘威廉指標");
```

---
## xfMin_RSI
**Path:** 系統函數 > 跨頻率

xfMin\_RSI -  (系統函數)
跨頻率

語法：

計算指定頻率的相對強弱指標數值。  
回傳數值=xfMin\_RSI(頻率,數列,期數)  
傳入三個參數:  
- 第一個參數是頻率，指定傳入數列的資料期別，支援"1","5","10","15","30","60","D", "W", "M", "AD", "AW", "AM"。  
- 第二個參數是目標頻率的數列，通常是開高低收的價格數列。  
- 第三個參數是期數。  
備註：商品類型僅支援台股與台期權。不支援XS選股、XS選股自訂排行與XS選股回測。

說明：

xfMin\_RSI是[xf\_RSI](api?a=xf_RSI&b=sys) 函數的跨頻率加強版本，增加了指定分鐘頻率的參數，可以計算指定分鐘頻率的RSI值。

範例：

```
value1 = xfMin_RSI("30",GetField("Close","30"),6);       //計算6期的30分鐘線RSI指標
plot1(value1, "30分RSI");
```

---
## xfMin_Stochastic
**Path:** 系統函數 > 跨頻率

xfMin\_Stochastic -  (系統函數)
跨頻率

語法：

計算指定頻率的KD指標。  
回傳數值=xfMin\_Stochastic(頻率,資料期數,K值平滑期數,D值平滑期數,輸出RSV值,輸出K值,輸出D值)  
傳入八個參數:  
- 第一個參數是頻率，指定傳入數列的資料期別，支援"1","5","10","15","30","60","D", "W", "M", "AD", "AW", "AM"。  
- 第二個參數是資料期數，指定計算的區間長度。  
- 第三個參數是K值平滑期數，指定計算K值所用的平滑期數。  
- 第四個參數是D值平滑期數，指定計算D值所用的平滑期數。  
- 第五個參數是輸出RSV值，回傳計算完的RSV值。  
- 第六個參數是輸出K值，回傳計算完的K值。  
- 第七個參數是輸出D值，回傳計算完的D值。  
備註：商品類型僅支援台股與台期權。不支援XS選股、XS選股自訂排行與XS選股回測。

說明：

xfMin\_Stochastic是[xf\_Stochastic](api?a=xf_Stochastic&b=sys) 函數的跨頻率加強版本，增加了指定分鐘頻率的參數，可以計算指定分鐘頻率的Stochastic值。

範例：

```
value1 = xfMin_Stochastic("30",9,3,3,value2,value3,value4);       //計算30分鐘線KD指標
plot1(value3, "30分K");
plot2(value4, "30分D");
```

---
## xfMin_WeightedClose
**Path:** 系統函數 > 跨頻率

xfMin\_WeightedClose -  (系統函數)
跨頻率

語法：

計算指定頻率的加權平均收盤價。  
回傳數值=xfMin\_WeightedClose(頻率)  
傳入一個參數:  
- 第一個參數是頻率，指定傳入數列的資料期別，支援"1","5","10","15","30","60","D", "W", "M", "AD", "AW", "AM"。  
備註：商品類型僅支援台股與台期權。不支援XS選股、XS選股自訂排行與XS選股回測。

說明：

xfMin\_WeightedClose是[xf\_WeightedClose](api?a=xf_WeightedClose&b=sys) 函數的跨頻率加強版本，增加了指定分鐘頻率的參數，可以計算指定分鐘頻率的WeightedClose值。

範例：

```
plot1(xfMin_WeightedClose("30"));    //繪製30分鐘線加權平均收盤價的連線
```

---
## xfMin_XAverage
**Path:** 系統函數 > 跨頻率

xfMin\_XAverage -  (系統函數)
跨頻率

語法：

計算指定頻率的指數移動平均。  
回傳數值=xfMin\_Xaverage(頻率,數列,期數)  
傳入三個參數:  
- 第一個參數是頻率，指定傳入數列的資料期別，支援"1","5","10","15","30","60","D", "W", "M", "AD", "AW", "AM"。  
- 第二個參數是目標頻率的數列，通常是開高低收的價格數列。  
- 第三個參數是期數。  
備註：商品類型僅支援台股與台期權。不支援XS選股、XS選股自訂排行與XS選股回測。

說明：

xfMin\_XAverage是[xf\_XAverage](api?a=xf_XAverage&b=sys) 函數的跨頻率加強版本，增加了指定分鐘頻率的參數，可以計算指定分鐘頻率的XAverage值。

範例：

```
value1 = xfMin_XAverage("30",GetField("Close","30"),5); //計算30分鐘線5期收盤價的指數移動平均
```

---
## xf_CrossOver
**Path:** 系統函數 > 跨頻率

xf\_CrossOver -  (系統函數)
跨頻率

語法：

判斷指定頻率的數列一是否由下往上穿越數列二，又稱黃金交叉。  
回傳布林值=xf\_CrossOver(頻率,數列一,數列二)  
傳入三個參數:  
- 第一個參數是頻率，指定傳入數列的資料期別，支援"D", "W", "M", "AD", "AW", "AM"。  
- 第二個參數是目標頻率的數列一。  
- 第三個參數是目標頻率的數列二。

說明：

如果出現黃金交叉傳回True，其他狀況傳回False。

範例：

```
condition1 = xf_CrossOver("W",Average(GetField("收盤價","W"),5),Average(GetField("收盤價","W") ,10)); //判斷週線5期均線和10期均線是否黃金交叉
```

---
## xf_CrossUnder
**Path:** 系統函數 > 跨頻率

xf\_CrossUnder -  (系統函數)
跨頻率

語法：

判斷指定頻率的數列一是否由上往下穿越數列二，又稱死亡交叉。  
回傳布林值=xf\_CrossUnder(頻率,數列一,數列二)  
傳入三個參數:  
- 第一個參數是頻率，指定傳入數列的資料期別，支援"D", "W", "M", "AD", "AW", "AM"。  
- 第二個參數是目標頻率的數列一。  
- 第三個參數是目標頻率的數列二。

說明：

如果出現死亡交叉傳回True，其他狀況傳回False。

範例：

```
condition1 = xf_CrossUnder("W",Average(GetField("close","W"),5),Average(GetField("close","W") ,10) ); //判斷週線5期均線和10期均線是否死亡交叉
```

---
## xf_DirectionMovement
**Path:** 系統函數 > 跨頻率

xf\_DirectionMovement -  (系統函數)
跨頻率

語法：

計算跨頻率DMI指標。  
回傳數值=xf\_DirectionMovement(頻率,期數,輸出+DI值,輸出-DI值,輸出ADX值)  
傳入五個參數:  
- 第一個參數是頻率，指定傳入數列的資料期別，支援"D", "W", "M", "AD", "AW", "AM"。  
- 第二個參數是計算期數。  
- 第三個參數是輸出計算完的+DI值。  
- 第四個參數是輸出計算完的-DI值。  
- 第五個參數是輸出計算完的ADX值。

說明：

xf\_DirectionMovement是[DirectionMovement](api?a=DirectionMovement&b=sys) 函數的跨頻率版本，增加了指定頻率的參數，可以計算指定頻率的DMI值。

範例：

```
value1 = xf_DirectionMovement("W",14,value2,value3,value4);       //計算14期的週DMI指標
plot1(value2, "週+DI");
plot2(value3, "週-DI");
plot3(value4, "週ADX");
```

---
## xf_EMA
**Path:** 系統函數 > 跨頻率

xf\_EMA -  (系統函數)
跨頻率

語法：

計算指定頻率的XQ指數移動平均。  
回傳數值=xf\_EMA(頻率,數列,期數)  
傳入三個參數:  
- 第一個參數是頻率，指定傳入數列的資料期別，支援"D", "W", "M", "AD", "AW", "AM"。  
- 第二個參數是目標頻率的數列，通常是開高低收的價格數列。  
- 第三個參數是期數。

說明：

xf\_EMA是[EMA](api?a=EMA&b=sys)函數的跨頻率版本，增加了指定頻率的參數，可以計算指定頻率的EMA值。

範例：

```
value1 = xf_EMA("W", Close,5); //計算週線5期收盤價的XQ EMA
```

---
## xf_GetBoolean
**Path:** 系統函數 > 跨頻率

xf\_GetBoolean -  (系統函數)
跨頻率

語法：

引用指定頻率的數值。  
回傳數值=xf\_GetBoolean(頻率,數列,期別)  
傳入三個參數:  
- 第一個參數是頻率，指定傳入數列的資料期別，支援"D", "W", "M", "AD", "AW", "AM"。  
- 第二個參數是目標頻率的布林數列。  
- 第三個參數是期別，相對目前而言要往前的筆數。

說明：

在同一個頻率時，我們可以直接利用\*\*變數[3]\*\*取得前3期的變數值。當資料頻率不同時（跨頻率），我們就需要使用xf\_GetValue或xf\_GetBoolean來取得之前的變數值。若變數是數值時，要用xf\_GetValue；若變數是布林值時，要用xf\_GetBoolean。

```
input:Length_W(9,"跨頻率週期數");
variable:rsv_w(0),kk_w(0),dd_w(0);
xf_stochastic("W", Length_W, 3, 3, rsv_w, kk_w, dd_w);
condition1 = xf_GetBoolean("W",xf_crossover("W", kk_w, dd_w),1);	//在日線抓周KD黃金交叉
```

相關函數：[xf\_GetValue](api?a=xf_GetValue&b=sys)。

---
## xf_GetCurrentBar
**Path:** 系統函數 > 跨頻率

xf\_GetCurrentBar -  (系統函數)
跨頻率

語法：

傳回指定頻率的K棒編號。  
K棒編號 = xf\_GetCurrentBar(頻率)  
傳入一個參數:  
- 第一個參數是頻率，指定傳入數列的資料期別，支援"D", "W", "M", "AD", "AW", "AM"。

說明：

傳回指定頻率的K棒序列編號，由1開始，第一筆K棒編號為1，第二筆K棒編號為2，依序遞增。

可以使用這個函數來判斷目前腳本執行的時機點

```
value1 = xf_GetCurrentBar(FreqType);


if Length + 1 = 0 then Factor = 1 else Factor = 2 / (Length + 1);


if value1 = 1 then
    xf_XAverage = Series
else
    xf_XAverage = lastXAverage + Factor * (Series - lastXAverage);
```

上述範例利用xf\_GetCurrentBar來判斷目前是否是第一筆K棒。如果是的話則回傳xf\_XAverage的初始數值。

---
## xf_GetDTValue
**Path:** 系統函數 > 跨頻率

xf\_GetDTValue -  (系統函數)
跨頻率

語法：

計算指定頻率的序列值。  
回傳數值=xf\_GetDTValue(頻率,日期)  
傳入二個參數:  
- 第一個參數是頻率，指定傳入數列的資料期別，支援"D", "W", "M", "AD", "AW", "AM"。  
- 第二個參數是日期。

說明：

經由傳入的日期判斷指定頻率的期別是否有異動

```
value1 = xf_getdtvalue("W",date);
if value1 <> value1[1] then plot1(1) else plot1(0);
```

上述範例利用xf\_GetDTValue來判斷目前是否為新的一週。如果是的話則在圖表上顯示為1。

---
## xf_GetValue
**Path:** 系統函數 > 跨頻率

xf\_GetValue -  (系統函數)
跨頻率

語法：

引用指定頻率的數值。  
回傳數值=xf\_GetValue(頻率,數列,期別)  
傳入三個參數:  
- 第一個參數是頻率，指定傳入數列的資料期別，支援"D", "W", "M", "AD", "AW", "AM"。  
- 第二個參數是目標頻率的數列。  
- 第三個參數是期別，相對目前而言要往前的筆數。

說明：

在同一個頻率時，我們可以直接利用\*\*變數[3]\*\*取得前3期的變數值。當資料頻率不同時（跨頻率），我們就需要使用xf\_GetValue或xf\_GetBoolean來取得之前的變數值。若變數是數值時，要用xf\_GetValue；若變數是布林值時，要用xf\_GetBoolean。

```
value1 = xf_WeightedClose("W");            //計算週線的加權平均價
value2 = xf_GetValue("W",value1,1);        //取得上一週的加權平均價
plot1(value2);
plot2(value1[1]);                        //可以比較一下和value2的差異
```

相關函數：[xf\_GetBoolean](api?a=xf_GetBoolean&b=sys)。

---
## xf_MACD
**Path:** 系統函數 > 跨頻率

xf\_MACD -  (系統函數)
跨頻率

語法：

計算指定頻率的MACD指標值。  
回傳數值=xf\_MACD(頻率,數列,短期數,長期數,MACD平滑期數,輸出DIF值,輸出MACD值,輸出OSC值)  
傳入八個參數:  
- 第一個參數是頻率，指定傳入數列的資料期別，支援"D", "W", "M", "AD", "AW", "AM"。  
- 第二個參數是目標頻率的數列，MACD通常是以加權平均收盤價（WeightedClose）來計算。  
- 第三個參數是計算快速線（短期）的期數。  
- 第四個參數是計算慢速線（長期）的期數。  
- 第五個參數是計算MACD使用之平滑期數。  
- 第六個參數是輸出計算完的DIF值。  
- 第七個參數是輸出計算完的MACD值。  
- 第八個參數是輸出計算完的OSC值。

說明：

xf\_MACD是[MACD](api?a=MACD&b=sys) 函數的跨頻率版本，增加了指定頻率的參數，可以計算指定頻率的MACD值。

範例：

```
value1 = xf_MACD("W",xf_weightedclose("W"),12,26,9,value2,value3,value4);       //計算週線MACD
plot1(value2, "週DIF");
plot2(value3, "週MACD");
plot3(value4, "週OSC");
```

---
## xf_PercentR
**Path:** 系統函數 > 跨頻率

xf\_PercentR -  (系統函數)
跨頻率

語法：

計算指定頻率的威廉指標值。  
回傳數值=xf\_PercentR(頻率,期數)  
傳入二個參數:  
- 第一個參數是頻率，指定傳入數列的資料期別，支援"D", "W", "M", "AD", "AW", "AM"。  
- 第二個參數是計算威廉指標的期數。

說明：

xf\_PercentR是[PercentR](api?a=PercentR&b=sys) 函數的跨頻率版本，增加了指定頻率的參數，可以計算指定頻率的PercentR值。

範例：

```
value1 = xf_PercentR("W", 14) - 100;       //計算週線威廉指標
Plot1(value1, "週威廉指標");
```

---
## xf_RSI
**Path:** 系統函數 > 跨頻率

xf\_RSI -  (系統函數)
跨頻率

語法：

計算指定頻率的相對強弱指標數值。  
回傳數值=xf\_RSI(頻率,數列,期數)  
傳入三個參數:  
- 第一個參數是頻率，指定傳入數列的資料期別，支援"D", "W", "M", "AD", "AW", "AM"。  
- 第二個參數是目標頻率的數列，通常是開高低收的價格數列。  
- 第三個參數是期數。

說明：

xf\_RSI是[RSI](api?a=RSI&b=sys) 函數的跨頻率版本，增加了指定頻率的參數，可以計算指定頻率的RSI值。

範例：

```
value1 = xf_RSI("W",GetField("Close","W"),6);       //計算6期的週RSI指標
plot1(value1, "週RSI");
```

---
## xf_Stochastic
**Path:** 系統函數 > 跨頻率

xf\_Stochastic -  (系統函數)
跨頻率

語法：

計算指定頻率的KD指標。  
回傳數值=xf\_Stochastic(頻率,資料期數,K值平滑期數,D值平滑期數,輸出RSV值,輸出K值,輸出D值)  
傳入八個參數:  
- 第一個參數是頻率，指定傳入數列的資料期別，支援"D", "W", "M", "AD", "AW", "AM"。  
- 第二個參數是資料期數，指定計算的區間長度。  
- 第三個參數是K值平滑期數，指定計算K值所用的平滑期數。  
- 第四個參數是D值平滑期數，指定計算D值所用的平滑期數。  
- 第五個參數是輸出RSV值，回傳計算完的RSV值。  
- 第六個參數是輸出K值，回傳計算完的K值。  
- 第七個參數是輸出D值，回傳計算完的D值。

說明：

xf\_Stochastic是[Stochastic](api?a=Stochastic&b=sys) 函數的跨頻率版本，增加了指定頻率的參數，可以計算指定頻率的Stochastic值。

範例：

```
value1 = xf_Stochastic("W",9,3,3,value2,value3,value4);       //計算週KD指標
plot1(value3, "週K");
plot2(value4, "週D");
```

---
## xf_WeightedClose
**Path:** 系統函數 > 跨頻率

xf\_WeightedClose -  (系統函數)
跨頻率

語法：

計算指定頻率的加權平均收盤價。  
回傳數值=xf\_WeightedClose(頻率)  
傳入一個參數:  
- 第一個參數是頻率，指定傳入數列的資料期別，支援"D", "W", "M", "AD", "AW", "AM"。

說明：

xf\_WeightedClose是[WeightedClose](api?a=WeightedClose&b=sys) 函數的跨頻率版本，增加了指定頻率的參數，可以計算指定頻率的WeightedClose值。

範例：

```
plot1(xf_WeightedClose("W"));    //繪製週線加權平均收盤價的連線
```

---
## xf_XAverage
**Path:** 系統函數 > 跨頻率

xf\_XAverage -  (系統函數)
跨頻率

語法：

計算指定頻率的指數移動平均。  
回傳數值=xf\_XAverage(頻率,數列,期數)  
傳入三個參數:  
- 第一個參數是頻率，指定傳入數列的資料期別，支援"D", "W", "M", "AD", "AW", "AM"。  
- 第二個參數是目標頻率的數列，通常是開高低收的價格數列。  
- 第三個參數是期數。

說明：

xf\_XAverage是[XAverage](api?a=XAverage&b=sys) 函數的跨頻率版本，增加了指定頻率的參數，可以計算指定頻率的XAverage值。

範例：

```
value1 = xf_XAverage("W",GetField("Close","W"),5); //計算週線5期收盤價的指數移動平均
```

---
## xfmin_MTM
**Path:** 系統函數 > 跨頻率

xfmin\_MTM -  (系統函數)
跨頻率

語法：

計算指定頻率的威廉指標值。  
回傳數值=xfMin\_MTM(頻率,期數)  
傳入二個參數:  
- 第一個參數是頻率，指定傳入數列的資料期別，支援"1","5","10","15","30","60","D", "W", "M", "AD", "AW", "AM"。  
- 第二個參數是計算MTM指標的期數。  
備註：商品類型僅支援台股與台期權。不支援XS選股、XS選股自訂排行與XS選股回測。

說明：

xfMin\_MTM是[MTM](api?a=MTM&b=sys) 函數的跨頻率版本，增加了指定頻率的參數，可以計算指定頻率的MTM值。

範例：

```
value1 = xfMin_MTM("5", 10);       //value1 = 五分鐘MTM
Plot1(value1, "5分鐘MTM");
```

---
## AverageIF
**Path:** 系統函數 > 邏輯判斷

AverageIF -  (系統函數)
邏輯判斷

語法：

計算符合條件的數值平均。  
回傳數值=AverageIF(條件數列,資料數列,期數)  
傳入三個參數:  
- 第一個參數是條件數列，當條件值為True時，才會納入計算平均。  
- 第二個參數是資料數列，通常是開高低收的價格數列。  
- 第三個參數是期數。

說明：

範例：

```
value1 = averageif(open>close[1],rateofchange(close,1),5); //計算開高時漲跌幅5期平均
```

---
## CountIf
**Path:** 系統函數 > 邏輯判斷

CountIf -  (系統函數)
邏輯判斷

語法：

計算符合條件的次數。  
回傳數值=CountIf(條件數列,期數)  
傳入二個參數:  
- 第一個參數是條件數列，當條件值為True時，才會納入計算。  
- 第二個參數是期數。

說明：

CountIF會計算特定期間內符合某些條件的次數。

範例：

```
value1 = CountIf(open>close[1],5); //計算過去5天開高的次數
```

---
## CountIfARow
**Path:** 系統函數 > 邏輯判斷

CountIfARow -  (系統函數)
邏輯判斷

語法：

計算符合條件連續符合的次數。  
回傳數值=CountIfARow(條件數列,期數)  
傳入二個參數:  
- 第一個參數是條件數列。  
- 第二個參數是期數。

說明：

以最新的資料為基準，往前計算連續符合條件的次數。

範例：

```
value1 = CountIfARow(close>close[1],5); //計算過去5天連續收紅的次數
```

註：CountIfARow函數和[TrueCount](api?a=TrueCount&b=sys)函數相同。

---
## CrossOver
**Path:** 系統函數 > 邏輯判斷

CrossOver -  (系統函數)
邏輯判斷

語法：

判斷數列一是否由下往上穿越數列二，又稱黃金交叉。  
回傳布林值=CrossOver(數列一,數列二)  
傳入二個參數:  
- 第一個參數是數列一。  
- 第二個參數是數列二。

說明：

如果出現黃金交叉傳回True，其他狀況傳回False。

範例：

```
condition1 = CrossOver(Average(close,5),Average(close,10)); //判斷5期均線和10期均線是否黃金交叉
```

---
## CrossUnder
**Path:** 系統函數 > 邏輯判斷

CrossUnder -  (系統函數)
邏輯判斷

語法：

判斷數列一是否由上往下穿越數列二，又稱死亡交叉。  
回傳布林值=CrossUnder(數列一,數列二)  
傳入二個參數:  
- 第一個參數是數列一。  
- 第二個參數是數列二。

說明：

如果出現死亡交叉傳回True，其他狀況傳回False。

範例：

```
condition1 = CrossUnder(Average(close,5),Average(close,10)); //判斷5期均線和10期均線是否死亡交叉
```

---
## DateTime
**Path:** 系統函數 > 邏輯判斷

DateTime -  (系統函數)
邏輯判斷

語法：

資料的日期與時間，格式為14碼的數字。  
當日期為西元2015年6月30日，且時間為12點30分00秒時，回傳值為20150630123000。

說明：

資料的日期與時間，格式為14碼的數字。  
當日期為西元2015年6月30日，且時間為12點30分00秒時，回傳值為20150630123000。

---
## Filter
**Path:** 系統函數 > 邏輯判斷

Filter -  (系統函數)
邏輯判斷

語法：

過濾指定期數內重複出現的警示。  
回傳布林值=Filter(條件布林值,過濾期數)  
傳入二個參數:  
- 第一個參數是代表被過濾條件是否成立的布林值。  
- 第二個參數是要過濾重複出現警示的期數。

說明：

當條件成立後，過濾在指定期數內重複出現的警示。例如：當股價第一次創20日新高時，未來5天再創20日新高就不再警示。

回傳TRUE，表示過濾後仍然成立的條件，意即原始條件為TRUE且距離原始條件成立的期數大於過濾期數。
回傳FALSE，表示過濾後不成立的條件，意即原始條件為FALSE，或是距離原始條件成立的期數小於等於過濾期數。

範例：

```
//警示腳本
condition1 = high = highest(high,20);       //判斷今高是否為20期之高點
condition2 = filter(condition1,5);       //過濾未來5期內重複的創新高警示
if condition2 then ret = 1;
```

---
## GetBarOffsetForYears
**Path:** 系統函數 > 邏輯判斷

GetBarOffsetForYears -  (系統函數)
邏輯判斷

語法：

依年份取得相對K棒位置。  
回傳數值=GetBarOffsetForYears(數值)

說明：

依傳入的年份計算相對K棒的位置。
當回傳值為0時，表示傳入年份 ≧K棒根數。

---
## IFF
**Path:** 系統函數 > 邏輯判斷

IFF -  (系統函數)
邏輯判斷

語法：

依條件成立狀況回傳對應的值。  
回傳數值=IFF(條件,條件成立時的回傳值,條件不成立時的回傳值)  
傳入三個參數:  
- 第一個參數是條件。  
- 第二個參數是條件成立時的回傳值。  
- 第三個參數是條件不成立時的回傳值。

說明：

把IF/Else的邏輯判斷用函數的形式來表示。

範例：

```
value1 = IFF(Close>Close[1],1,0); //當上漲時value1為1，其他狀況時value1為0
```

---
## SummationIf
**Path:** 系統函數 > 邏輯判斷

SummationIf -  (系統函數)
邏輯判斷

語法：

計算符合條件的數值總和。  
回傳數值=SummationIf(條件數列,資料數列,期數)  
傳入三個參數:  
- 第一個參數是條件數列，當條件值為True時，才會納入計算總和。  
- 第二個參數是資料數列，通常是開高低收的價格數列。  
- 第三個參數是期數。

說明：

當某期資料符合某項判斷準則時，才將該期資料計入加總。

範例：

```
value1 = SummationIf(open>close[1],rateofchange(close,1),5); //計算近5期開高時的漲跌幅總和
```

---
## TrueAll
**Path:** 系統函數 > 邏輯判斷

TrueAll -  (系統函數)
邏輯判斷

語法：

判斷條件數列在指定期數內是否同時成立。  
回傳布林值=TrueAll(條件數列,期數)  
傳入二個參數:  
- 第一個參數是條件數列。  
- 第二個參數是期數。

說明：

當期間內所有條件皆成立時，回傳True；其他狀況則回傳False

範例：

```
condition1 = TrueAll(Close>Close[1],3); //判斷K棒是否連續三期上漲
```

---
## TrueAny
**Path:** 系統函數 > 邏輯判斷

TrueAny -  (系統函數)
邏輯判斷

語法：

判斷條件數列在指定期數內是否有任何一筆成立。  
回傳布林值=TrueAny(條件數列,期數)  
傳入二個參數:  
- 第一個參數是條件數列。  
- 第二個參數是期數。

說明：

當期間內有一筆以上條件成立時，回傳True；其他狀況則回傳False

範例：

```
condition1 = TrueAny(Close>Close[1],3); //判斷最近三期是否有任一期K棒上漲
```

---
## TrueCount
**Path:** 系統函數 > 邏輯判斷

TrueCount -  (系統函數)
邏輯判斷

語法：

計算符合條件連續符合的次數。  
回傳數值=TrueCount(條件數列,期數)  
傳入二個參數:  
- 第一個參數是條件數列。  
- 第二個參數是期數。

說明：

以最新的資料為基準，往前計算連續符合條件的次數。

範例：

```
value1 = TrueCount(close>close[1],5); //計算過去5天連續收紅的次數
```

註：TrueCount函數和[CountIfARow](api?a=CountIfARow&b=sys)函數相同。

---
## DiffBidAskVolumeLxL
**Path:** 系統函數 > 量能相關

DiffBidAskVolumeLxL -  (系統函數)
量能相關

語法：

傳回「近15分鐘大戶買賣超」的數值  
回傳數值 = DiffBidAskVolumeLxL  
僅支援1分鐘頻率與台股商品。

說明：

DiffBidAskVolumeLxL為近15分鐘大戶買賣超的函數，
該函數運算出來的數值，與XS指標的「流動大戶買賣力」指標相同。

---
## DiffBidAskVolumeXL
**Path:** 系統函數 > 量能相關

DiffBidAskVolumeXL -  (系統函數)
量能相關

語法：

傳回「近15分鐘特大單買賣超」的張數  
回傳數值 = DiffBidAskVolumeXL  
僅支援1分鐘頻率與台股商品。

說明：

DiffBidAskVolumeXL為近15分鐘特大單買賣超張數的函數。

範例：

```
value1 = DiffBidAskVolumeXL;
plot1(value1); //value1為近15分鐘特大單買賣超張數
```

---
## DiffTradeVolumeAtAskBid
**Path:** 系統函數 > 量能相關

DiffTradeVolumeAtAskBid -  (系統函數)
量能相關

語法：

傳回「分時買賣力」的數值  
回傳數值 = DiffTradeVolumeAtAskBid  
僅支援分鐘與日頻率（含還原）  
支援台股與期權商品。

說明：

DiffTradeVolumeAtAskBid為分時買賣力的函數，
該函數運算出來的數值，與XS指標的「分時買賣力」指標相同。

---
## DiffUpDownVolume
**Path:** 系統函數 > 量能相關

DiffUpDownVolume -  (系統函數)
量能相關

語法：

傳回「分時漲跌成交量」的數值  
回傳數值 = DiffUpDownVolume  
僅支援分鐘與日頻率（含還原）  
支援台股與期權商品。

說明：

DiffUpDownVolume為分時漲跌成交量的函數，
該函數運算出來的數值，與XS指標的「分時漲跌成交量」指標相同。

---
## 庫藏股結束日期
**Path:** 資料欄位 > 事件

|  |  |
| --- | --- |
| 欄位名稱 | 庫藏股結束日期 (資料欄位) |
| 語法 | ``` Value1 = GetField("庫藏股結束日期"); Value1 = GetField("Stockenddate"); ``` |
| 欄位分類 | 事件 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 單一 |
| 支援商品 | 台股 |
| 說明 | 最新一次庫藏股預計買回期間的結束日期。  欄位格式為西元年月日，例如20221101。  可以檢查GetFieldDate("庫藏股結束日期")是否等於0來判斷是否有資料。 |

---
## 庫藏股開始日期
**Path:** 資料欄位 > 事件

|  |  |
| --- | --- |
| 欄位名稱 | 庫藏股開始日期 (資料欄位) |
| 語法 | ``` Value1 = GetField("庫藏股開始日期"); Value1 = GetField("Stockstartdate"); ``` |
| 欄位分類 | 事件 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 單一 |
| 支援商品 | 台股 |
| 說明 | 最新一次庫藏股預計買回期間的開始日期。  欄位格式為西元年月日，例如20221101。  可以檢查GetFieldDate("庫藏股開始日期")是否等於0來判斷是否有資料。 |

---
## 新股上市日
**Path:** 資料欄位 > 事件

|  |  |
| --- | --- |
| 欄位名稱 | 新股上市日 (資料欄位) |
| 語法 | ``` Value1 = GetField("新股上市日"); Value1 = GetField("LaunchDayOfNewShares"); ``` |
| 欄位分類 | 事件 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 單一 |
| 支援商品 | 台股 |
| 說明 | 最近一次新股上市日期。  欄位格式為西元年月日，例如20221101。  系統依照最近一次公布的股東會，除權，現增，或是減資，回傳最接近的新股上市日。 |

---
## 最後過戶日期
**Path:** 資料欄位 > 事件

|  |  |
| --- | --- |
| 欄位名稱 | 最後過戶日期 (資料欄位) |
| 語法 | ``` Value1 = GetField("最後過戶日期"); Value1 = GetField("LastTransferDay"); ``` |
| 欄位分類 | 事件 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 單一 |
| 支援商品 | 台股 |
| 說明 | 最新一期的除權息最後過戶日日期，以西元年月日來表示，當日期為2023年6月18日，回傳值為20230618。 會依計算日期取得最新的除權息最後過戶日期。 資料來源：TEJ台灣經濟新報。 |

---
## 法說會日期
**Path:** 資料欄位 > 事件

|  |  |
| --- | --- |
| 欄位名稱 | 法說會日期 (資料欄位) |
| 語法 | ``` Value1 = GetField("法說會日期"); Value1 = GetField("InvestorConferenceDate"); ``` |
| 欄位分類 | 事件 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 單一 |
| 支援商品 | 台股 |
| 說明 | 最新一期法說會日期。  欄位格式為西元年月日，例如20221101。  資料來源為台灣證券交易所。  請注意：目前只紀錄各公司每季財報說明、業績說明會，以及於證交所所舉辦的法說會的日期。其餘如投資機構所邀請的法說會，如投資論壇、投資展望說明會、台股企業日等，則不紀錄於此欄位中。 |

---
## 減資新股上市日
**Path:** 資料欄位 > 事件

|  |  |
| --- | --- |
| 欄位名稱 | 減資新股上市日 (資料欄位) |
| 語法 | ``` Value1 = GetField("減資新股上市日"); Value1 = GetField("LaunchDayOfCapitalReduction"); ``` |
| 欄位分類 | 事件 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 單一 |
| 支援商品 | 台股 |
| 說明 | 最近一次減資的新股上市日。  欄位格式為西元年月日，例如20221101。  可以檢查GetFieldDate("減資新股上市日")是否等於0來判斷是否有減資的資料。 |

---
## 減資日期
**Path:** 資料欄位 > 事件

|  |  |
| --- | --- |
| 欄位名稱 | 減資日期 (資料欄位) |
| 語法 | ``` Value1 = GetField("減資日期"); Value1 = GetField("CapitalReductionDate"); ``` |
| 欄位分類 | 事件 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 單一 |
| 支援商品 | 台股 |
| 說明 | 最近一次減資日期日期。  欄位格式為西元年月日，例如20221101。  可以檢查GetFieldDate("減資日期")是否等於0來判斷是否有減資的資料。 |

---
## 減資最後過戶日
**Path:** 資料欄位 > 事件

|  |  |
| --- | --- |
| 欄位名稱 | 減資最後過戶日 (資料欄位) |
| 語法 | ``` Value1 = GetField("減資最後過戶日"); Value1 = GetField("LastTransferDateOfCapitalReduction"); ``` |
| 欄位分類 | 事件 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 單一 |
| 支援商品 | 台股 |
| 說明 | 最近一次減資的最後過戶日。  欄位格式為西元年月日，例如20221101。  可以檢查GetFieldDate("減資最後過戶日")是否等於0來判斷是否有減資的資料。 |

---
## 減資比例
**Path:** 資料欄位 > 事件

|  |  |
| --- | --- |
| 欄位名稱 | 減資比例 (資料欄位) |
| 語法 | ``` Value1 = GetField("減資比例"); Value1 = GetField("CapitalReductionRatio"); ``` |
| 欄位分類 | 事件 |
| 單位 | % |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 單一 |
| 支援商品 | 台股 |
| 說明 | 最近一次減資的比例。  單位是％，例如20代表減資20%。  可以檢查GetFieldDate("減資比例")是否等於0來判斷是否有減資的資料。 |

---
## 現增價格
**Path:** 資料欄位 > 事件

|  |  |
| --- | --- |
| 欄位名稱 | 現增價格 (資料欄位) |
| 語法 | ``` Value1 = GetField("現增價格"); Value1 = GetField("CashIncrementAmount"); ``` |
| 欄位分類 | 事件 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 單一 |
| 支援商品 | 台股 |
| 說明 | 最新一期現金增資的發行價格。  可以檢查GetFieldDate("現增價格")是否等於0來判斷是否有現金增資的資料。 |

---
## 現增新股上市日
**Path:** 資料欄位 > 事件

|  |  |
| --- | --- |
| 欄位名稱 | 現增新股上市日 (資料欄位) |
| 語法 | ``` Value1 = GetField("現增新股上市日"); Value1 = GetField("LaunchDayOfNewSharesCashIncrement"); ``` |
| 欄位分類 | 事件 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 單一 |
| 支援商品 | 台股 |
| 說明 | 最近一次現金增資的新股上市日。  欄位格式為西元年月日，例如20221101。  可以檢查GetFieldDate("現增新股上市日")是否等於0來判斷是否有現金增資的資料。 |

---
## 現增最後過戶日
**Path:** 資料欄位 > 事件

|  |  |
| --- | --- |
| 欄位名稱 | 現增最後過戶日 (資料欄位) |
| 語法 | ``` Value1 = GetField("現增最後過戶日"); Value1 = GetField("LastTransferDayOfCashIncrement"); ``` |
| 欄位分類 | 事件 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 單一 |
| 支援商品 | 台股 |
| 說明 | 最近一次現金增資的最後過戶日。  欄位格式為西元年月日，例如20221101。  可以檢查GetFieldDate("現增最後過戶日")是否等於0來判斷是否有現金增資的資料。 |

---
## 現增繳款日期
**Path:** 資料欄位 > 事件

|  |  |
| --- | --- |
| 欄位名稱 | 現增繳款日期 (資料欄位) |
| 語法 | ``` Value1 = GetField("現增繳款日期"); Value1 = GetField("DateofCashIncrement"); ``` |
| 欄位分類 | 事件 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 單一 |
| 支援商品 | 台股 |
| 說明 | 最近一次現金增資的最後繳款日。  欄位格式為西元年月日，例如20221101。  可以檢查GetFieldDate("現增繳款日期")是否等於0來判斷是否有現金增資的資料。 |

---
## 股東會日期
**Path:** 資料欄位 > 事件

|  |  |
| --- | --- |
| 欄位名稱 | 股東會日期 (資料欄位) |
| 語法 | ``` Value1 = GetField("股東會日期"); Value1 = GetField("DateofMeetingofShareHolders"); ``` |
| 欄位分類 | 事件 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 單一 |
| 支援商品 | 台股 |
| 說明 | 最近一次股東會的日期。  欄位格式為西元年月日，例如20221101。 |

---
## 處置結束日期
**Path:** 資料欄位 > 事件

|  |  |
| --- | --- |
| 欄位名稱 | 處置結束日期 (資料欄位) |
| 語法 | ``` Value1 = GetField("處置結束日期"); Value1 = GetField("DispositionED"); ``` |
| 欄位分類 | 事件 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 單一 |
| 支援商品 | 台股 台(權證) 台(可轉債) 台(特別股) |
| 說明 | 最近一次處置期間的結束日期。  欄位格式為西元年月日，例如20221101。  請注意：這個日期可能是「未來」的日期(例如已經進入處置期間，可是尚未結束)，也可能是「過去」的日期(例如已經結束處置)。  可以檢查GetFieldDate("處置結束日期")是否等於0來判斷是否有資料。 |

---
## 處置開始日期
**Path:** 資料欄位 > 事件

|  |  |
| --- | --- |
| 欄位名稱 | 處置開始日期 (資料欄位) |
| 語法 | ``` Value1 = GetField("處置開始日期"); Value1 = GetField("DispositionSD"); ``` |
| 欄位分類 | 事件 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 單一 |
| 支援商品 | 台股 台(權證) 台(可轉債) 台(特別股) |
| 說明 | 最近一次處置期間的開始日期。  欄位格式為西元年月日，例如20221101。  請注意：這個日期可能是「未來」的日期(例如剛公佈處置，可是尚未進入處置期間)，也可能是「過去」的日期(例如已經進入處置，或是已經結束處置)。  可以檢查GetFieldDate("處置開始日期")是否等於0來判斷是否有資料。 |

---
## 融券最後回補日
**Path:** 資料欄位 > 事件

|  |  |
| --- | --- |
| 欄位名稱 | 融券最後回補日 (資料欄位) |
| 語法 | ``` Value1 = GetField("融券最後回補日"); Value1 = GetField("ShortSaleFinalRecallDate"); ``` |
| 欄位分類 | 事件 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 單一 |
| 支援商品 | 台股 |
| 說明 | 最近一次融券最後回補日的日期。  欄位格式為西元年月日，例如20221101。  系統依照最近一次公布的股東會，除權息，現增，或是減資，回傳最接近的融券最後回補日。 |

---
## 除息值
**Path:** 資料欄位 > 事件

|  |  |
| --- | --- |
| 欄位名稱 | 除息值 (資料欄位) |
| 語法 | ``` Value1 = GetField("除息值"); Value1 = GetField("ExDividendValue"); ``` |
| 欄位分類 | 事件 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 單一 |
| 支援商品 | 台股 |
| 說明 | 最近一次公佈的除息值。  單位為元(每1股配發幾元現金)。  例如：如果每股配發0.5元現金的話，則數值為0.5。  請注意：這個數值可能是即將除息的數值(例如已經公佈要除息，可是尚未除息)，也可能是過去一次除息的數值(例如已經除息，可是下次除息日尚未確定)。  可以檢查GetFieldDate("除息值")是否等於0來判斷是否有資料。 |

---
## 除息年度
**Path:** 資料欄位 > 事件

|  |  |
| --- | --- |
| 欄位名稱 | 除息年度 (資料欄位) |
| 語法 | ``` Value1 = GetField("除息年度"); Value1 = GetField("ExDividendYear"); ``` |
| 欄位分類 | 事件 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 單一 |
| 支援商品 | 台(股票) |
| 說明 | 最近一次公佈的除息資料所對應的股息所屬年度。  欄位格式為西元年，例如2015。  可以檢查GetFieldDate("除息年度")是否等於0來判斷是否有資料。 |

---
## 除息日期
**Path:** 資料欄位 > 事件

|  |  |
| --- | --- |
| 欄位名稱 | 除息日期 (資料欄位) |
| 語法 | ``` Value1 = GetField("除息日期"); Value1 = GetField("ExDividendDate"); ``` |
| 欄位分類 | 事件 |
| 單位 | 日期 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 單一 |
| 支援商品 | 台股 |
| 說明 | 最近一次公佈的除息日期。  欄位格式為西元年月日，例如20150618。  請注意：這個日期可能是「未來」的日期(例如剛公佈要除息，可是尚未除息)，也可能是「過去」的日期(例如已經除完息，可是尚未公佈下次除息的日期)。  可以檢查GetFieldDate("除息日期")是否等於0來判斷是否有資料。 |

---
## 除權值
**Path:** 資料欄位 > 事件

|  |  |
| --- | --- |
| 欄位名稱 | 除權值 (資料欄位) |
| 語法 | ``` Value1 = GetField("除權值"); Value1 = GetField("ExRightValue"); ``` |
| 欄位分類 | 事件 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 單一 |
| 支援商品 | 台股 |
| 說明 | 最近一次公佈的除權值。  台股的話單位是元(每1股配發幾股股票)，美股的話則是除權(Split)的比例。  舉例而言，在台股，如果每股配發0.5股的話(每1000股配發500股)，那數值是0.5。  在美股，如果分割的比例是1:2的話(每1股變成0.5股)，則數值為0.5，如果是2:1的話(每1股變成2股)，則數值為2。  請注意：這個數值可能是即將除權的數值(例如已經公佈要除權，可是尚未除權)，也可能是過去一次除權的數值(例如已經除權，可是下次除權日尚未確定)。  可以檢查GetFieldDate("除權值")是否等於0來判斷是否有資料。 |

---
## 除權年度
**Path:** 資料欄位 > 事件

|  |  |
| --- | --- |
| 欄位名稱 | 除權年度 (資料欄位) |
| 語法 | ``` Value1 = GetField("除權年度"); Value1 = GetField("ExRightYear"); ``` |
| 欄位分類 | 事件 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 單一 |
| 支援商品 | 台(股票) |
| 說明 | 最近一次公佈的除權資料所對應的股利所屬年度。  欄位格式為西元年，例如2015。  可以檢查GetFieldDate("除權年度")是否等於0來判斷是否有資料。 |

---
## 除權息值
**Path:** 資料欄位 > 事件

|  |  |
| --- | --- |
| 欄位名稱 | 除權息值 (資料欄位) |
| 語法 | ``` Value1 = GetField("除權息值"); Value1 = GetField("ExDividendRightValue"); ``` |
| 欄位分類 | 事件 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 單一 |
| 支援商品 | 台股 |
| 說明 | 最近一次公佈的除權或是除息的數值。  如果股票在同一日除權+除息的話，則回傳配發的現金與配發的股票的加總，否則則回傳最近一次的除權值，或是除息值。  僅支援台股。  例如：如果股票同一日除權1元(每1000股配發100股)且除息0.5元的話，則數值為1.5。  可以檢查GetFieldDate("除權息值")是否等於0來判斷是否有資料。 |

---
## 除權息年度
**Path:** 資料欄位 > 事件

|  |  |
| --- | --- |
| 欄位名稱 | 除權息年度 (資料欄位) |
| 語法 | ``` Value1 = GetField("除權息年度"); Value1 = GetField("ExDividendRightYear"); ``` |
| 欄位分類 | 事件 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 單一 |
| 支援商品 | 台(股票) |
| 說明 | 最近一次公佈的除權或是除息資料所對應的股利或是股息所屬年度。  欄位格式為西元年，例如2015。  如果股票在同一日除權+除息的話，則回傳這個日期，否則則回傳最近一次的除權年度，或是除息年度。  可以檢查GetFieldDate("除權息年度")是否等於0來判斷是否有資料。 |

---
## 除權息日期
**Path:** 資料欄位 > 事件

|  |  |
| --- | --- |
| 欄位名稱 | 除權息日期 (資料欄位) |
| 語法 | ``` Value1 = GetField("除權息日期"); Value1 = GetField("ExDividendRightDate"); ``` |
| 欄位分類 | 事件 |
| 單位 | 日期 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 單一 |
| 支援商品 | 台股 |
| 說明 | 最近一次公佈的除權或是除息日期。  欄位格式為西元年月日，例如20150618。  如果股票在同一日除權+除息的話，則回傳這個日期，否則則回傳最近一次的除權日期，或是除息日期。  請注意：這個日期可能是「未來」的日期(例如剛公佈要除息/除權，可是尚未除息/除權)，也可能是「過去」的日期(例如已經除完息/除完權，可是尚未公佈下次除息/除權的日期)。  可以檢查GetFieldDate("除權息日期")是否等於0來判斷是否有資料。 |

---
## 除權日期
**Path:** 資料欄位 > 事件

|  |  |
| --- | --- |
| 欄位名稱 | 除權日期 (資料欄位) |
| 語法 | ``` Value1 = GetField("除權日期"); Value1 = GetField("ExRightDate"); ``` |
| 欄位分類 | 事件 |
| 單位 | 日期 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 單一 |
| 支援商品 | 台股 |
| 說明 | 最近一次公佈的除權日期。  欄位格式為西元年月日，例如20150618。  請注意：這個日期可能是「未來」的日期(例如剛公佈要除權，可是尚未除權)，也可能是「過去」的日期(例如已經除完權，可是尚未公佈下次除權的日期)。  可以檢查GetFieldDate("除權日期")是否等於0來判斷是否有資料。 |

---
## 內外盤
**Path:** 資料欄位 > 價格

|  |  |
| --- | --- |
| 欄位名稱 | 內外盤 (資料欄位) |
| 語法 | ``` Value1 = GetField("內外盤"); Value1 = GetField("BidAskFlag"); ``` |
| 欄位分類 | 價格 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | Tick |
| 支援商品 | 全部 |
| 說明 | 這一筆成交明細內的「內外盤」標記。  數值是1時表示是外盤成交，數值是-1時為內盤成交，如果是0的話則表示無法判斷。  系統會依照成交發生當時五檔委託簿的最佳委買價格以及最佳委賣價格來判斷此筆是外盤成交或是內盤成交。  如果成交的價格貼近委賣價格的話，則標示為外盤成交，一般會視為主動「買進」。如果成交的價格貼近委買價格的話，則標示為內盤成交，一般會視為主動「賣出」。 |

---
## 參考價
**Path:** 資料欄位 > 價格

|  |  |
| --- | --- |
| 欄位名稱 | 參考價 (資料欄位) |
| 語法 | ``` Value1 = GetField("參考價"); Value1 = GetField("RefPrice"); ``` |
| 欄位分類 | 價格 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 |
| 支援商品 | 台股 |
| 說明 | 當日交易的參考價。一般而言是前一交易日的收盤價，如果股票商品有除權息的話，則是除權息後的參考價。  除權息的計算公式如下(實際數值以交易所公佈為主)： 1.除息參考價為股價－股息 2.除權參考價為股價／(1+配股率) 3.同時除權息參考價為 (股價-股息)／(1+配股率) |

---
## 均價
**Path:** 資料欄位 > 價格

|  |  |
| --- | --- |
| 欄位名稱 | 均價 (資料欄位) |
| 語法 | ``` Value1 = GetField("均價"); Value1 = GetField("AvgPrice"); ``` |
| 欄位分類 | 價格 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 分鐘 日 |
| 支援商品 | 台股 大盤 類股指數 期貨 選擇權 香港股票 大陸股票 |
| 說明 | 每日的平均成交價。  計算方式是累加每日開盤後每筆成交的價格乘上數量，然後除以累計的成交數量，得出當日平均的成交價。  如果是分鐘頻率的話，數值為開盤到現在為止的成交均價。如果是日頻率的話則是當日的成交均價。 |

---
## 基差
**Path:** 資料欄位 > 價格

|  |  |
| --- | --- |
| 欄位名稱 | 基差 (資料欄位) |
| 語法 | ``` Value1 = GetField("基差"); Value1 = GetField("Basis"); ``` |
| 欄位分類 | 價格 |
| 單位 | 點 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 分鐘 日 週 月 |
| 支援商品 | 大盤 期貨 |
| 說明 | 現貨價格與期貨價格之間的差異。  例如：加權指數為16800，而大台指為16795的話，那麼基差＝16800 ─ 16795＝5。 |

---
## 強弱指標
**Path:** 資料欄位 > 價格

|  |  |
| --- | --- |
| 欄位名稱 | 強弱指標 (資料欄位) |
| 語法 | ``` Value1 = GetField("強弱指標"); Value1 = GetField("StrongWeak"); ``` |
| 欄位分類 | 價格 |
| 單位 | % |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 |
| 支援商品 | 台股 類股指數 |
| 說明 | 商品漲跌幅與對應大盤漲跌幅的差異，用來衡量商品是否比大盤強。  強弱指標的計算公式是(商品漲跌幅－對應大盤漲跌幅)。單位是％，例如商品漲幅是2.5％，對應大盤漲幅是1.8%，那麼強弱指標則為+0.7。 |

---
## 投資建議目標價
**Path:** 資料欄位 > 價格

|  |  |
| --- | --- |
| 欄位名稱 | 投資建議目標價 (資料欄位) |
| 語法 | ``` Value1 = GetField("投資建議目標價"); Value1 = GetField("AnalystPriceTargets"); ``` |
| 欄位分類 | 價格 |
| 單位 | 台股:元；美股:元(美元) |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 單一 |
| 支援商品 | 美(股票) |
| 說明 | 市場機構對個股的股價預估平均值。 美股股票才適用本欄位。  每個市場機構會不定期的對股票提供評等，同時也會提供預估目標價。 基於所有機構所提供的預估目標價，採平均計算後，就會得到投資目標價欄位。  請注意：   1. 此欄位會不定期的更新 2. 並不是所有的股票都會有這個欄位 |

---
## 收盤價
**Path:** 資料欄位 > 價格

|  |  |
| --- | --- |
| 欄位名稱 | 收盤價 (資料欄位) |
| 語法 | ``` Value1 = GetField("收盤價"); Value1 = GetField("Close"); ``` |
| 欄位分類 | 價格 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | Tick 分鐘 日 週 月 還原日 還原週 還原月 季 半年 年 |
| 支援商品 | 全部 |
| 說明 | K棒資料的收盤價(最後一個成交價位)。  語法上也可以使用Close來取得同樣的數值。 |

---
## 最低價
**Path:** 資料欄位 > 價格

|  |  |
| --- | --- |
| 欄位名稱 | 最低價 (資料欄位) |
| 語法 | ``` Value1 = GetField("最低價"); Value1 = GetField("Low"); ``` |
| 欄位分類 | 價格 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 分鐘 日 週 月 還原日 還原週 還原月 季 半年 年 |
| 支援商品 | 全部 |
| 說明 | K棒資料的最低成交價位。  語法上也可以使用Low來取得同樣的數值。 |

---
## 最高價
**Path:** 資料欄位 > 價格

|  |  |
| --- | --- |
| 欄位名稱 | 最高價 (資料欄位) |
| 語法 | ``` Value1 = GetField("最高價"); Value1 = GetField("High"); ``` |
| 欄位分類 | 價格 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 分鐘 日 週 月 還原日 還原週 還原月 季 半年 年 |
| 支援商品 | 全部 |
| 說明 | K棒資料的最高成交價位。  語法上也可以使用High來取得同樣的數值。 |

---
## 漲停價
**Path:** 資料欄位 > 價格

|  |  |
| --- | --- |
| 欄位名稱 | 漲停價 (資料欄位) |
| 語法 | ``` Value1 = GetField("漲停價"); Value1 = GetField("UpLimit"); ``` |
| 欄位分類 | 價格 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 |
| 支援商品 | 台股 |
| 說明 | 當日的漲停價。 |

---
## 買入價
**Path:** 資料欄位 > 價格

|  |  |
| --- | --- |
| 欄位名稱 | 買入價 (資料欄位) |
| 語法 | ``` Value1 = GetField("買入價"); Value1 = GetField("BidPrice"); ``` |
| 欄位分類 | 價格 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | Tick |
| 支援商品 | 全部 |
| 說明 | 這一筆成交明細內的「買進」價格。  「買進」價格為成交發生當時五檔委託簿的最佳委買價格。 |

---
## 賣出價
**Path:** 資料欄位 > 價格

|  |  |
| --- | --- |
| 欄位名稱 | 賣出價 (資料欄位) |
| 語法 | ``` Value1 = GetField("賣出價"); Value1 = GetField("AskPrice"); ``` |
| 欄位分類 | 價格 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | Tick |
| 支援商品 | 全部 |
| 說明 | 這一筆成交明細內的「賣出」價格。  「賣出」價格為成交發生當時五檔委託簿的最佳委賣價格。 |

---
## 跌停價
**Path:** 資料欄位 > 價格

|  |  |
| --- | --- |
| 欄位名稱 | 跌停價 (資料欄位) |
| 語法 | ``` Value1 = GetField("跌停價"); Value1 = GetField("DownLimit"); ``` |
| 欄位分類 | 價格 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 |
| 支援商品 | 台股 |
| 說明 | 當日的跌停價。 |

---
## 開盤價
**Path:** 資料欄位 > 價格

|  |  |
| --- | --- |
| 欄位名稱 | 開盤價 (資料欄位) |
| 語法 | ``` Value1 = GetField("開盤價"); Value1 = GetField("Open"); ``` |
| 欄位分類 | 價格 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 分鐘 日 週 月 還原日 還原週 還原月 季 半年 年 |
| 支援商品 | 全部 |
| 說明 | K棒資料的開盤價(第一個成交價位)。  語法上也可以使用Open來取得同樣的數值。 |

---
## 投資建議評級
**Path:** 資料欄位 > 基本

|  |  |
| --- | --- |
| 欄位名稱 | 投資建議評級 (資料欄位) |
| 語法 | ``` Value1 = GetField("投資建議評級"); Value1 = GetField("RecommendationRating"); ``` |
| 欄位分類 | 基本 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 單一 |
| 支援商品 | 美(股票) |
| 說明 | 市場機構對個股的投資建議評等分數。  美股股票才適用本欄位。  每個市場機構會不定期的對股票提供評等，評等分數分成５級，分別是：1=強烈買進，2=買進(Outperform)，3＝中立, 4=賣出(Underperform), 5=強烈賣出。  基於所有機構所提供的評等分數，採平均計算後，就會得到投資建議評等分數。  這個分數介於1 ~ 5之間，依照上述的評等方式，可以對應出以下的關係： 1 <= x < 1.5：強烈買進 1.5 <= x < 2.5：買進(Outperform) 2.5 <= x < 3.5：中立 3.5 <= x < 4.5：賣出(Underperform) 4.5 <= x < 5：強烈賣出  請注意：   1. 此欄位會不定期的更新 2. 並不是所有的股票都會有這個欄位 |

---
## 月營收
**Path:** 資料欄位 > 基本

|  |  |
| --- | --- |
| 欄位名稱 | 月營收 (資料欄位) |
| 語法 | ``` Value1 = GetField("月營收"); Value1 = GetField("Sales"); ``` |
| 欄位分類 | 基本 |
| 單位 | 億 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 月 |
| 支援商品 | 台股 大盤 |
| 說明 | 每個月的營業收入。 |

---
## 本益比
**Path:** 資料欄位 > 基本

|  |  |
| --- | --- |
| 欄位名稱 | 本益比 (資料欄位) |
| 語法 | ``` Value1 = GetField("本益比"); Value1 = GetField("PE"); ``` |
| 欄位分類 | 基本 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 |
| 支援商品 | 台(股票) 美(股票) |
| 說明 | 股票的本益比。  計算公式為收盤價／每股稅後盈餘。  台股的每股稅後盈餘＝近四季稅後盈餘／發行參考股數。實際數值以交易所公佈值為準。  美股的每股稅後盈餘＝近四季本期稅後淨利加總／最新一季的總流通在外股數。  當每股稅後盈餘為0或負值時，則不計算本益比。 |

---
## 殖利率
**Path:** 資料欄位 > 基本

|  |  |
| --- | --- |
| 欄位名稱 | 殖利率 (資料欄位) |
| 語法 | ``` Value1 = GetField("殖利率"); Value1 = GetField("Yield"); ``` |
| 欄位分類 | 基本 |
| 單位 | % |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 |
| 支援商品 | 台(股票) 美(股票) 美(特別股) 美(ETF) |
| 說明 | 股票的殖利率。單位是%，例如殖利率為5.6％的話，則回傳5.6。  殖利率計算的公式如下：  台股：(最近一個完整股利年度的現金股利＋股票股利)／收盤價＊100%  美股/ETF：近1年股利／收盤價＊100%。  美國特別股：(票面利率＊每股面額)／收盤價＊100%。 |

---
## 發行張數_張_
**Path:** 資料欄位 > 基本

|  |  |
| --- | --- |
| 欄位名稱 | 發行張數(張) (資料欄位) |
| 語法 | ``` Value1 = GetField("發行張數(張)"); Value1 = GetField("OutStandingShares"); ``` |
| 欄位分類 | 基本 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 季 半年 年 |
| 支援商品 | 台股 |
| 說明 | 當期的發行張數。  資料來源為交易所。 |

---
## 總市值_元_
**Path:** 資料欄位 > 基本

|  |  |
| --- | --- |
| 欄位名稱 | 總市值(元) (資料欄位) |
| 語法 | ``` Value1 = GetField("總市值(元)"); Value1 = GetField("TotalMarketValue"); Value1 = GetField("總市值"); ``` |
| 欄位分類 | 基本 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 季 半年 年 |
| 支援商品 | 台股 大盤 類股指數 |
| 說明 | 股票或是指數的市值。股票市值以發行股數 ＊ 收盤價來計算，指數則是成分股的市值加總。單位為元。  週以上頻率，是取期底值。 |

---
## 股本_億_
**Path:** 資料欄位 > 基本

|  |  |
| --- | --- |
| 欄位名稱 | 股本(億) (資料欄位) |
| 語法 | ``` Value1 = GetField("股本(億)"); Value1 = GetField("CurrentCapitalinBillion"); ``` |
| 欄位分類 | 基本 |
| 單位 | 億元 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 |
| 支援商品 | 台股 |
| 說明 | 股票的股本，換算成億元。 |

---
## 股本_元_
**Path:** 資料欄位 > 基本

|  |  |
| --- | --- |
| 欄位名稱 | 股本(元) (資料欄位) |
| 語法 | ``` Value1 = GetField("股本(元)"); Value1 = GetField("CurrentCapital"); ``` |
| 欄位分類 | 基本 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 |
| 支援商品 | 台股 |
| 說明 | 股票的股本，換算成元。 |

---
## 財報股本_億_
**Path:** 資料欄位 > 基本

|  |  |
| --- | --- |
| 欄位名稱 | 財報股本(億) (資料欄位) |
| 語法 | ``` Value1 = GetField("財報股本(億)"); Value1 = GetField("CurrentCapitalin100Million"); Value1 = GetField("億元股本"); Value1 = GetField("加權平均股本"); ``` |
| 欄位分類 | 基本 |
| 單位 | 億元 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 季 半年 年 |
| 支援商品 | 台股 |
| 說明 | 公司財報(季、年)上載明的股本，換算成億元。 |

---
## TW50KD多空家數
**Path:** 資料欄位 > 市場統計

|  |  |
| --- | --- |
| 欄位名稱 | TW50KD多空家數 (資料欄位) |
| 語法 | ``` Value1 = GetField("TW50KD多空家數"); Value1 = GetField("TW50KDLongShortSecurities"); ``` |
| 欄位分類 | 市場統計 |
| 單位 | 家 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 分鐘 日 |
| 支援商品 | 大盤 |
| 說明 | 統計台灣50成分股內，目前K > D的家數。 KD指標計算參數為：資料期數為9，K值平滑期數為3，D值平滑期數為3。使用1分鐘資料來做計算。  僅支援TSE50.SJ 商品。  系統每分鐘會統計符合的家數，而腳本端則依照執行頻率(支援分鐘/日頻率)來取得當分鐘最後的符合家數。  建議撰寫方式： value1 = getsymbolfield("TSE50.SJ","TW50KD多空家數"); |

---
## TW50MTM多空家數
**Path:** 資料欄位 > 市場統計

|  |  |
| --- | --- |
| 欄位名稱 | TW50MTM多空家數 (資料欄位) |
| 語法 | ``` Value1 = GetField("TW50MTM多空家數"); Value1 = GetField("TW50MTMLongShortSecurities"); ``` |
| 欄位分類 | 市場統計 |
| 單位 | 家 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 分鐘 日 |
| 支援商品 | 大盤 |
| 說明 | 統計台灣50成分股內，Momentum(10) > 0的家數。 Momentum(N)的公式 = 最近1分鐘的Close - N分鐘前的Close。也就是說這個欄位是最近1分鐘較10分鐘前上漲的家數。  僅支援TSE50.SJ 商品。  系統每分鐘會統計符合的家數，而腳本端則依照執行頻率(支援分鐘/日頻率)來取得當分鐘最後的符合家數。  建議撰寫方式： value1 = getsymbolfield("TSE50.SJ","TW50MTM多空家數"); |

---
## TW50上昇趨勢家數
**Path:** 資料欄位 > 市場統計

|  |  |
| --- | --- |
| 欄位名稱 | TW50上昇趨勢家數 (資料欄位) |
| 語法 | ``` Value1 = GetField("TW50上昇趨勢家數"); Value1 = GetField("TW50UpTrendSecurities"); ``` |
| 欄位分類 | 市場統計 |
| 單位 | 家 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 分鐘 日 |
| 支援商品 | 大盤 |
| 說明 | 統計台灣50成份股內趨勢向上的家數。 趨勢向上的定義是計算近6分鐘(含目前這一分鐘)的線性回歸線(LinearRegression)是否向上。  僅支援TSE50.SJ 商品。  系統每分鐘會統計符合的家數，而腳本端則依照執行頻率(支援分鐘/日頻率)來取得當分鐘最後的符合家數。  建議撰寫方式： value1 = getsymbolfield("TSE50.SJ","TW50上昇趨勢家數"); |

---
## TW50價格上漲家數
**Path:** 資料欄位 > 市場統計

|  |  |
| --- | --- |
| 欄位名稱 | TW50價格上漲家數 (資料欄位) |
| 語法 | ``` Value1 = GetField("TW50價格上漲家數"); Value1 = GetField("TW50PriceUpSecurities"); ``` |
| 欄位分類 | 市場統計 |
| 單位 | 家 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 分鐘 日 |
| 支援商品 | 大盤 |
| 說明 | 統計台灣50成份股內，目前這一分鐘收盤價大於前一分鐘收盤價的家數。  僅支援TSE50.SJ 商品。  系統每分鐘會統計符合的家數，而腳本端則依照執行頻率(支援分鐘/日頻率)來取得當分鐘最後的符合家數。  建議撰寫方式： value1 = getsymbolfield("TSE50.SJ","TW50價格上漲家數"); |

---
## TW50創新低家數
**Path:** 資料欄位 > 市場統計

|  |  |
| --- | --- |
| 欄位名稱 | TW50創新低家數 (資料欄位) |
| 語法 | ``` Value1 = GetField("TW50創新低家數"); Value1 = GetField("TW50NewLowSecurities"); ``` |
| 欄位分類 | 市場統計 |
| 單位 | 家 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 分鐘 日 |
| 支援商品 | 大盤 |
| 說明 | 統計台灣50成分股，目前這1分鐘的最低價小於最近20分鐘最低價的家數。  僅支援TSE50.SJ 商品。  系統每分鐘會統計符合的家數，而腳本端則依照執行頻率(支援分鐘/日頻率)來取得當分鐘最後的符合家數。  建議撰寫方式： value1 = getsymbolfield("TSE50.SJ","TW50創新低家數"); |

---
## TW50創新高家數
**Path:** 資料欄位 > 市場統計

|  |  |
| --- | --- |
| 欄位名稱 | TW50創新高家數 (資料欄位) |
| 語法 | ``` Value1 = GetField("TW50創新高家數"); Value1 = GetField("TW50NewHighSecurities"); ``` |
| 欄位分類 | 市場統計 |
| 單位 | 家 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 分鐘 日 |
| 支援商品 | 大盤 |
| 說明 | 統計台灣50成分股，目前這1分鐘的最高價大於最近20分鐘最高價的家數。  僅支援TSE50.SJ 商品。  系統每分鐘會統計符合的家數，而腳本端則依照執行頻率(支援分鐘/日頻率)來取得當分鐘最後的符合家數。  建議撰寫方式： value1 = getsymbolfield("TSE50.SJ","TW50創新高家數"); |

---
## TW50均線多空家數
**Path:** 資料欄位 > 市場統計

|  |  |
| --- | --- |
| 欄位名稱 | TW50均線多空家數 (資料欄位) |
| 語法 | ``` Value1 = GetField("TW50均線多空家數"); Value1 = GetField("TW50MALongShortSecurities"); ``` |
| 欄位分類 | 市場統計 |
| 單位 | 家 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 分鐘 日 |
| 支援商品 | 大盤 |
| 說明 | 統計台灣50成分股內，目前收盤價大於前10分鐘均價的家數。  僅支援TSE50.SJ 商品。  系統每分鐘會統計符合的家數，而腳本端則依照執行頻率(支援分鐘/日頻率)來取得當分鐘最後的符合家數。  建議撰寫方式： value1 = getsymbolfield("TSE50.SJ","TW50均線多空家數"); |

---
## TW50大單成交次數
**Path:** 資料欄位 > 市場統計

|  |  |
| --- | --- |
| 欄位名稱 | TW50大單成交次數 (資料欄位) |
| 語法 | ``` Value1 = GetField("TW50大單成交次數"); Value1 = GetField("TW50BidTickCountXL"); ``` |
| 欄位分類 | 市場統計 |
| 單位 | 次 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 分鐘 日 |
| 支援商品 | 大盤 |
| 說明 | 加總台灣50成分股每檔商品最近10分鐘的買進大單+買進特大單的平均次數。  僅支援TSE50.SJ 商品。  統計的方式不跨日，所以開盤未滿10分鐘前則依照開盤的分鐘數來平均。  建議撰寫方式： value1 = getsymbolfield("TSE50.SJ","TW50大單成交次數"); |

---
## TW50大單買進金額
**Path:** 資料欄位 > 市場統計

|  |  |
| --- | --- |
| 欄位名稱 | TW50大單買進金額 (資料欄位) |
| 語法 | ``` Value1 = GetField("TW50大單買進金額"); Value1 = GetField("TW50BidVolumePriceXLTrend"); ``` |
| 欄位分類 | 市場統計 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 分鐘 日 |
| 支援商品 | 大盤 |
| 說明 | 加總台灣50成分股每檔商品的分鐘買進大單+買進特大單的金額。  分鐘買進金額為開盤迄今最近10分鐘的分鐘均量。  僅支援TSE50.SJ 商品。  統計的方式不跨日，所以開盤未滿10分鐘前則依照開盤的分鐘數來平均。  建議撰寫方式： value1 = getsymbolfield("TSE50.SJ","TW50大單買進金額"); |

---
## TW50大戶買賣力
**Path:** 資料欄位 > 市場統計

|  |  |
| --- | --- |
| 欄位名稱 | TW50大戶買賣力 (資料欄位) |
| 語法 | ``` Value1 = GetField("TW50大戶買賣力"); Value1 = GetField("TW50BigSharesBidAskPower"); ``` |
| 欄位分類 | 市場統計 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 分鐘 日 |
| 支援商品 | 大盤 |
| 說明 | 回傳台灣50成分股每檔商品當根K棒的大戶買賣力金額。  大戶買賣力的定義是買進的(大單+特大單)金額 - 賣出的(大單+特大單)金額。  僅支援TSE50.SJ 商品。  建議撰寫方式： value1 = getsymbolfield("TSE50.SJ","TW50大戶買賣力"); |

---
## TW50紅K家數
**Path:** 資料欄位 > 市場統計

|  |  |
| --- | --- |
| 欄位名稱 | TW50紅K家數 (資料欄位) |
| 語法 | ``` Value1 = GetField("TW50紅K家數"); Value1 = GetField("TW50BullKSecurities"); ``` |
| 欄位分類 | 市場統計 |
| 單位 | 家 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 分鐘 日 |
| 支援商品 | 大盤 |
| 說明 | 統計台灣50成分股內，目前這1分鐘是紅K棒的家數。  僅支援TSE50.SJ 商品。  系統每分鐘會統計符合的家數，而腳本端則依照執行頻率(支援分鐘/日頻率)來取得當分鐘最後的符合家數。  建議撰寫方式： value1 = getsymbolfield("TSE50.SJ","TW50紅K家數"); |

---
## 上漲家數
**Path:** 資料欄位 > 市場統計

|  |  |
| --- | --- |
| 欄位名稱 | 上漲家數 (資料欄位) |
| 語法 | ``` Value1 = GetField("上漲家數"); Value1 = GetField("UpSecurities"); ``` |
| 欄位分類 | 市場統計 |
| 單位 | 家 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 分鐘 日 |
| 支援商品 | 大盤 類股指數 |
| 說明 | 指數成分股內目前上漲的家數。  統計資料盤中會定時更新，如果是分鐘頻率的話會取得當下最新數值，如果是日頻率的話則是當日最後的數值。  大盤商品的統計會扣除權證商品。  請注意：上漲家數與漲停家數是分別統計的，上漲家數並不包含漲停的家數。 |

---
## 下跌家數
**Path:** 資料欄位 > 市場統計

|  |  |
| --- | --- |
| 欄位名稱 | 下跌家數 (資料欄位) |
| 語法 | ``` Value1 = GetField("下跌家數"); Value1 = GetField("DownSecurities"); ``` |
| 欄位分類 | 市場統計 |
| 單位 | 家 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 分鐘 日 |
| 支援商品 | 大盤 類股指數 |
| 說明 | 指數成分股內目前下跌的家數。  統計資料盤中會定時更新，如果是分鐘頻率的話會取得當下最新數值，如果是日頻率的話則是當日最後的數值。  大盤商品的統計會扣除權證商品。  請注意：下跌家數與跌停家數是分別統計的，下跌家數並不包含跌停的家數。 |

---
## 內盤家數
**Path:** 資料欄位 > 市場統計

|  |  |
| --- | --- |
| 欄位名稱 | 內盤家數 (資料欄位) |
| 語法 | ``` Value1 = GetField("內盤家數"); Value1 = GetField("BidSecurities"); ``` |
| 欄位分類 | 市場統計 |
| 單位 | 家 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 分鐘 日 |
| 支援商品 | 大盤 類股指數 |
| 說明 | 指數成分股內目前以內盤成交的家數。  統計資料盤中會定時更新，如果是分鐘頻率的話會取得當下最新數值，如果是日頻率的話則是當日最後的數值。  大盤商品的統計會扣除權證商品。 |

---
## 外盤家數
**Path:** 資料欄位 > 市場統計

|  |  |
| --- | --- |
| 欄位名稱 | 外盤家數 (資料欄位) |
| 語法 | ``` Value1 = GetField("外盤家數"); Value1 = GetField("AskSecurities"); ``` |
| 欄位分類 | 市場統計 |
| 單位 | 家 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 分鐘 日 |
| 支援商品 | 大盤 類股指數 |
| 說明 | 指數成分股內目前以外盤成交的家數。  統計資料盤中會定時更新，如果是分鐘頻率的話會取得當下最新數值，如果是日頻率的話則是當日最後的數值。  大盤商品的統計會扣除權證商品。 |

---
## 漲停家數
**Path:** 資料欄位 > 市場統計

|  |  |
| --- | --- |
| 欄位名稱 | 漲停家數 (資料欄位) |
| 語法 | ``` Value1 = GetField("漲停家數"); Value1 = GetField("UpLimitSecs"); ``` |
| 欄位分類 | 市場統計 |
| 單位 | 家 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 分鐘 日 |
| 支援商品 | 大盤 類股指數 |
| 說明 | 指數成分股內目前漲停的家數。  統計資料盤中會定時更新，如果是分鐘頻率的話會取得當下最新數值，如果是日頻率的話則是當日最後的數值。  大盤商品的統計會扣除權證商品。 |

---
## 跌停家數
**Path:** 資料欄位 > 市場統計

|  |  |
| --- | --- |
| 欄位名稱 | 跌停家數 (資料欄位) |
| 語法 | ``` Value1 = GetField("跌停家數"); Value1 = GetField("DownLimitSecs"); ``` |
| 欄位分類 | 市場統計 |
| 單位 | 家 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 分鐘 日 |
| 支援商品 | 大盤 類股指數 |
| 說明 | 指數成分股內目前跌停的家數。  統計資料盤中會定時更新，如果是分鐘頻率的話會取得當下最新數值，如果是日頻率的話則是當日最後的數值。  大盤商品的統計會扣除權證商品。 |

---
## 騰落指標
**Path:** 資料欄位 > 市場統計

|  |  |
| --- | --- |
| 欄位名稱 | 騰落指標 (資料欄位) |
| 語法 | ``` Value1 = GetField("騰落指標"); Value1 = GetField("UDIndicator"); ``` |
| 欄位分類 | 市場統計 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 |
| 支援商品 | 大盤 類股指數 |
| 說明 | 長期累積的(上漲家數─下跌家數)的數值。  這個指標也稱為Advance-Decline Line，透過累算每日市場漲跌家數的差值，來反應出整體市場行情漲升力道的強弱變化，並且以此來研判出未來行情大勢可能的發展方向。  請參考： <https://www.moneydj.com/KMDJ/Wiki/wikiViewer.aspx?keyid=6819746d-ce57-4753-98d2-d840d2cc6219> |

---
## 估計量
**Path:** 資料欄位 > 常用

|  |  |
| --- | --- |
| 欄位名稱 | 估計量 (資料欄位) |
| 語法 | ``` Value1 = GetField("估計量"); Value1 = GetField("EstimateVolume"); ``` |
| 欄位分類 | 常用 |
| 單位 | 個股:張，大盤:元，類股:元 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 分鐘 日 |
| 支援商品 | 大盤 類股指數 台(股票) |
| 說明 | 當日收盤的預估成交量。  大盤或是個股的估計的方式是統計過去一段時間內每日盤中的分鐘累計成交量，算出每分鐘成交量的分佈比例，然後依照當日開盤迄今的累計成交量，來推算收盤時可能的成交量。  類股商品（細產業指標、主題指標）的計算方式為成分股預估量乘以均價的加總。 |

---
## 內盤量
**Path:** 資料欄位 > 常用

|  |  |
| --- | --- |
| 欄位名稱 | 內盤量 (資料欄位) |
| 語法 | ``` Value1 = GetField("內盤量"); Value1 = GetField("TradeVolumeAtBid"); Value1 = GetField("內盤金額"); ``` |
| 欄位分類 | 常用 |
| 單位 | 台股:張；大盤/指數:元 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 分鐘 日 還原日 |
| 支援商品 | 台股 大盤 類股指數 期貨 選擇權 香港股票 大陸股票 |
| 說明 | 這一筆K棒的所有成交內標記為內盤成交的加總數量。  系統會依照成交發生當時委託簿的最佳委買價格以及最佳委賣價格來判斷此筆是外盤成交或是內盤成交。如果成交的價格貼近委買價格的話，則標示為內盤成交，一般會視為主動「賣出」。  如果是指數類型商品的話，則是比前一價下跌的所有成交量的加總。 |

---
## 參考價
**Path:** 資料欄位 > 常用

|  |  |
| --- | --- |
| 欄位名稱 | 參考價 (資料欄位) |
| 語法 | ``` Value1 = GetField("參考價"); Value1 = GetField("RefPrice"); ``` |
| 欄位分類 | 常用 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 |
| 支援商品 | 台股 |
| 說明 | 當日交易的參考價。一般而言是前一交易日的收盤價，如果股票商品有除權息的話，則是除權息後的參考價。  除權息的計算公式如下(實際數值以交易所公佈為主)： 1.除息參考價為股價－股息 2.除權參考價為股價／(1+配股率) 3.同時除權息參考價為 (股價-股息)／(1+配股率) |

---
## 均價
**Path:** 資料欄位 > 常用

|  |  |
| --- | --- |
| 欄位名稱 | 均價 (資料欄位) |
| 語法 | ``` Value1 = GetField("均價"); Value1 = GetField("AvgPrice"); ``` |
| 欄位分類 | 常用 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 分鐘 日 |
| 支援商品 | 台股 大盤 類股指數 期貨 選擇權 香港股票 大陸股票 |
| 說明 | 每日的平均成交價。  計算方式是累加每日開盤後每筆成交的價格乘上數量，然後除以累計的成交數量，得出當日平均的成交價。  如果是分鐘頻率的話，數值為開盤到現在為止的成交均價。如果是日頻率的話則是當日的成交均價。 |

---
## 外盤量
**Path:** 資料欄位 > 常用

|  |  |
| --- | --- |
| 欄位名稱 | 外盤量 (資料欄位) |
| 語法 | ``` Value1 = GetField("外盤量"); Value1 = GetField("TradeVolumeAtAsk"); Value1 = GetField("外盤金額"); ``` |
| 欄位分類 | 常用 |
| 單位 | 台股:張；大盤/指數:元 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 分鐘 日 還原日 |
| 支援商品 | 台股 大盤 類股指數 期貨 選擇權 香港股票 大陸股票 |
| 說明 | 這一筆K棒的所有成交內被標記為外盤成交的加總數量。  系統會依照成交發生當時委託簿的最佳委買價格以及最佳委賣價格來判斷此筆是外盤成交或是內盤成交。如果成交的價格貼近委賣價格的話，則標示為外盤成交，一般會視為主動「買進」。  如果是指數類型商品的話，則是比前一價上漲的所有成交量的加總。 |

---
## 成交量
**Path:** 資料欄位 > 常用

|  |  |
| --- | --- |
| 欄位名稱 | 成交量 (資料欄位) |
| 語法 | ``` Value1 = GetField("成交量"); Value1 = GetField("Volume"); ``` |
| 參數範圍 | Call、Put |
| 欄位分類 | 常用 |
| 單位 | 台股:張；大盤/類股:元；期貨/選擇權:口；其他市場股票:股 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | Tick 分鐘 日 週 月 季 半年 年 |
| 支援商品 | 全部 |
| 說明 | K棒資料的成交量。  數值單位依照商品類型而異，例如台股的話成交量的單位是張，可是美股的話成交量的單位則是股。  語法上也可以使用Volume來取得同樣的數值。 |

---
## 成交金額_元_
**Path:** 資料欄位 > 常用

|  |  |
| --- | --- |
| 欄位名稱 | 成交金額(元) (資料欄位) |
| 語法 | ``` Value1 = GetField("成交金額(元)"); Value1 = GetField("TradeValue"); Value1 = GetField("成交金額"); ``` |
| 欄位分類 | 常用 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 分鐘 日 週 月 還原日 還原週 還原月 |
| 支援商品 | 台股 大盤 類股指數 香港股票 大陸股票 美(股票) |
| 說明 | 成交的加總金額。  如果是個股的話，計算方式是統計每筆成交數量乘上成交價。  如果是大盤或是指數型商品的話，此欄位與成交量是相同的。  數值依照統計區間而易，如果是分鐘頻率的話，則是該分鐘統計區間的每筆成交金額的加總，如果是週月以上的話則是每日成交金額的加總。 |

---
## 收盤價
**Path:** 資料欄位 > 常用

|  |  |
| --- | --- |
| 欄位名稱 | 收盤價 (資料欄位) |
| 語法 | ``` Value1 = GetField("收盤價"); Value1 = GetField("Close"); ``` |
| 欄位分類 | 常用 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | Tick 分鐘 日 週 月 還原日 還原週 還原月 季 半年 年 |
| 支援商品 | 全部 |
| 說明 | K棒資料的收盤價(最後一個成交價位)。  語法上也可以使用Close來取得同樣的數值。 |

---
## 日期
**Path:** 資料欄位 > 常用

|  |  |
| --- | --- |
| 欄位名稱 | 日期 (資料欄位) |
| 語法 | ``` Value1 = GetField("日期"); Value1 = GetField("Date"); ``` |
| 欄位分類 | 常用 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | Tick 分鐘 日 週 月 季 半年 年 |
| 支援商品 | 全部 |
| 說明 | K棒資料的日期。欄位格式為西元年月日，例如20221101。  語法上也可以使用Date來取得同樣的數值。 |

---
## 時間
**Path:** 資料欄位 > 常用

|  |  |
| --- | --- |
| 欄位名稱 | 時間 (資料欄位) |
| 語法 | ``` Value1 = GetField("時間"); Value1 = GetField("Time"); ``` |
| 欄位分類 | 常用 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | Tick 分鐘 日 週 月 季 半年 年 |
| 支援商品 | 全部 |
| 說明 | K棒資料的時間(時分秒)。格式是hhmmss的6位數字，例如103000代表10點30分0秒。  時間採用24小時制，所以時間如果是下午1點20分30秒則會回傳132030。  如果頻率是日線以上的話，時間欄位會回傳0。  語法上也可以使用Time來取得同樣的數值。 |

---
## 最低價
**Path:** 資料欄位 > 常用

|  |  |
| --- | --- |
| 欄位名稱 | 最低價 (資料欄位) |
| 語法 | ``` Value1 = GetField("最低價"); Value1 = GetField("Low"); ``` |
| 欄位分類 | 常用 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 分鐘 日 週 月 還原日 還原週 還原月 季 半年 年 |
| 支援商品 | 全部 |
| 說明 | K棒資料的最低成交價位。  語法上也可以使用Low來取得同樣的數值。 |

---
## 最高價
**Path:** 資料欄位 > 常用

|  |  |
| --- | --- |
| 欄位名稱 | 最高價 (資料欄位) |
| 語法 | ``` Value1 = GetField("最高價"); Value1 = GetField("High"); ``` |
| 欄位分類 | 常用 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 分鐘 日 週 月 還原日 還原週 還原月 季 半年 年 |
| 支援商品 | 全部 |
| 說明 | K棒資料的最高成交價位。  語法上也可以使用High來取得同樣的數值。 |

---
## 漲停價
**Path:** 資料欄位 > 常用

|  |  |
| --- | --- |
| 欄位名稱 | 漲停價 (資料欄位) |
| 語法 | ``` Value1 = GetField("漲停價"); Value1 = GetField("UpLimit"); ``` |
| 欄位分類 | 常用 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 |
| 支援商品 | 台股 |
| 說明 | 當日的漲停價。 |

---
## 買入價
**Path:** 資料欄位 > 常用

|  |  |
| --- | --- |
| 欄位名稱 | 買入價 (資料欄位) |
| 語法 | ``` Value1 = GetField("買入價"); Value1 = GetField("BidPrice"); ``` |
| 欄位分類 | 常用 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | Tick |
| 支援商品 | 全部 |
| 說明 | 這一筆成交明細內的「買進」價格。  「買進」價格為成交發生當時五檔委託簿的最佳委買價格。 |

---
## 賣出價
**Path:** 資料欄位 > 常用

|  |  |
| --- | --- |
| 欄位名稱 | 賣出價 (資料欄位) |
| 語法 | ``` Value1 = GetField("賣出價"); Value1 = GetField("AskPrice"); ``` |
| 欄位分類 | 常用 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | Tick |
| 支援商品 | 全部 |
| 說明 | 這一筆成交明細內的「賣出」價格。  「賣出」價格為成交發生當時五檔委託簿的最佳委賣價格。 |

---
## 跌停價
**Path:** 資料欄位 > 常用

|  |  |
| --- | --- |
| 欄位名稱 | 跌停價 (資料欄位) |
| 語法 | ``` Value1 = GetField("跌停價"); Value1 = GetField("DownLimit"); ``` |
| 欄位分類 | 常用 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 |
| 支援商品 | 台股 |
| 說明 | 當日的跌停價。 |

---
## 開盤價
**Path:** 資料欄位 > 常用

|  |  |
| --- | --- |
| 欄位名稱 | 開盤價 (資料欄位) |
| 語法 | ``` Value1 = GetField("開盤價"); Value1 = GetField("Open"); ``` |
| 欄位分類 | 常用 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 分鐘 日 週 月 還原日 還原週 還原月 季 半年 年 |
| 支援商品 | 全部 |
| 說明 | K棒資料的開盤價(第一個成交價位)。  語法上也可以使用Open來取得同樣的數值。 |

---
## Delta
**Path:** 資料欄位 > 期權

|  |  |
| --- | --- |
| 欄位名稱 | Delta (資料欄位) |
| 語法 | ``` Value1 = GetField("Delta"); Value1 = GetField("Delta"); ``` |
| 欄位分類 | 期權 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 分鐘 日 週 月 |
| 支援商品 | 台(權證) 選擇權 |
| 說明 | 選擇權／權證商品的Delta值，用來衡量標的商品價格的變動幅度對商品價格的影響。  公式是，當標的商品價格變動Y元，選擇權(或權證)價格將變動Delta＊Y(＊執行比例)元。  例如，台指選擇權履約價16500的買權，其Delta值為0.5，表示當加權指數上漲100點時，此買權的價格會上漲50元。  Delta計算式，請參考Black & Scholes模型。 |

---
## Gamma
**Path:** 資料欄位 > 期權

|  |  |
| --- | --- |
| 欄位名稱 | Gamma (資料欄位) |
| 語法 | ``` Value1 = GetField("Gamma"); Value1 = GetField("Gamma"); ``` |
| 欄位分類 | 期權 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 分鐘 日 週 月 |
| 支援商品 | 台(權證) 選擇權 |
| 說明 | 選擇權／權證商品的Gamma值，用來衡量標的商品價格變動幅度對商品Delta的影響。  公式是，當標的商品價格變動Y元，選擇權(或權證)的Delta值將變動Gamma＊Y。  例如，台指選擇權履約價16500的買權，其Delta值為0.5，Gamma值為0.0008，表示當加權指數上漲100點時，此買權的Delta會增加0.08。  Gamma計算式，請參考Black & Scholes模型。 |

---
## RHO
**Path:** 資料欄位 > 期權

|  |  |
| --- | --- |
| 欄位名稱 | RHO (資料欄位) |
| 語法 | ``` Value1 = GetField("RHO"); Value1 = GetField("RHO"); ``` |
| 欄位分類 | 期權 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 分鐘 日 週 月 |
| 支援商品 | 台(權證) 選擇權 |
| 說明 | 選擇權／權證商品的RHO值，用來衡量利率的變動幅度對商品價格的影響。  公式是，當利率變動Y%，選擇權(或權證)價格將變動RHO＊Y(＊執行比例)元。  假設，台指選擇權履約價16500的買權，其RHO值為4.2，表示當利率上漲1%，此買權的價格會上漲4.2元。  RHO計算式，請參考Black & Scholes模型。 |

---
## Theta
**Path:** 資料欄位 > 期權

|  |  |
| --- | --- |
| 欄位名稱 | Theta (資料欄位) |
| 語法 | ``` Value1 = GetField("Theta"); Value1 = GetField("Theta"); ``` |
| 欄位分類 | 期權 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 分鐘 日 週 月 |
| 支援商品 | 台(權證) 選擇權 |
| 說明 | 選擇權／權證商品的Theta值，用來衡量時間的流逝對商品價格的影響。  公式是，每經過一天，選擇權(或權證)價格將減少(Theta / 365)元。  例如，台指選擇權履約價16500的買權，Theta值為-5，表示此買權每天會減損0.0137(=-5/365)元。  Theta計算式，請參考Black & Scholes模型。 |

---
## Vega
**Path:** 資料欄位 > 期權

|  |  |
| --- | --- |
| 欄位名稱 | Vega (資料欄位) |
| 語法 | ``` Value1 = GetField("Vega"); Value1 = GetField("Vega"); ``` |
| 欄位分類 | 期權 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 分鐘 日 週 月 |
| 支援商品 | 台(權證) 選擇權 |
| 說明 | 選擇權／權證商品的Vega值，用來衡量標的商品隱含波動率的變動幅度對商品價格的影響。  公式是，當標的商品價格變動Y%，選擇權(或權證)價格將變動Vega＊Y(＊執行比例)元。  例如，台指選擇權履約價16500的買權，其Vega值為16，表示當加權指數的隱含波動率上漲1%時，此買權的價格會上漲16元。  Vega計算式，請參考Black & Scholes模型。 |

---
## 三大法人交易買口
**Path:** 資料欄位 > 期權

|  |  |
| --- | --- |
| 欄位名稱 | 三大法人交易買口 (資料欄位) |
| 語法 | ``` Value1 = GetField("三大法人交易買口"); Value1 = GetField("MITLongLots"); ``` |
| 參數範圍 | Call、Put |
| 欄位分類 | 期權 |
| 單位 | 口 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 |
| 支援商品 | 期貨 選擇權 |
| 說明 | 三大法人多方的交易口數。  如果是期貨商品的話，數值是所有月份契約(含週契約)的加總。  如果是選擇權商品的話，系統會根據目前商品的買賣權別，回傳買權或是賣權所有履約價/月份契約(含週契約)的加總。  如果商品是選擇權現貨商品的話，則可以傳入參數，分別取得買權或是賣權的數值：  GetSymbolField("TXO00.TF", "三大法人交易買口", param:="CALL") GetSymbolField("TXO00.TF", "三大法人交易買口", param:="PUT") |

---
## 三大法人交易買進金額
**Path:** 資料欄位 > 期權

|  |  |
| --- | --- |
| 欄位名稱 | 三大法人交易買進金額 (資料欄位) |
| 語法 | ``` Value1 = GetField("三大法人交易買進金額"); Value1 = GetField("MITBAmount"); ``` |
| 參數範圍 | Call、Put |
| 欄位分類 | 期權 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 |
| 支援商品 | 期貨 選擇權 |
| 說明 | 三大法人多方的交易金額。 |

---
## 三大法人交易賣出金額
**Path:** 資料欄位 > 期權

|  |  |
| --- | --- |
| 欄位名稱 | 三大法人交易賣出金額 (資料欄位) |
| 語法 | ``` Value1 = GetField("三大法人交易賣出金額"); Value1 = GetField("MITSAmount"); ``` |
| 參數範圍 | Call、Put |
| 欄位分類 | 期權 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 |
| 支援商品 | 期貨 選擇權 |
| 說明 | 三大法人空方的交易金額。 |

---
## 三大法人交易賣口
**Path:** 資料欄位 > 期權

|  |  |
| --- | --- |
| 欄位名稱 | 三大法人交易賣口 (資料欄位) |
| 語法 | ``` Value1 = GetField("三大法人交易賣口"); Value1 = GetField("MITSShortLots"); ``` |
| 參數範圍 | Call、Put |
| 欄位分類 | 期權 |
| 單位 | 口 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 |
| 支援商品 | 期貨 選擇權 |
| 說明 | 三大法人空方的交易口數。  如果是期貨商品的話，數值是所有月份契約(含週契約)的加總。  如果是選擇權商品的話，系統會根據目前商品的買賣權別，回傳買權或是賣權所有履約價/月份契約(含週契約)的加總。  如果商品是選擇權現貨商品的話，則可以傳入參數，分別取得買權或是賣權的數值：  GetSymbolField("TXO00.TF", "三大法人交易賣口", param:="CALL") GetSymbolField("TXO00.TF", "三大法人交易賣口", param:="PUT") |

---
## 三大法人買方未平倉
**Path:** 資料欄位 > 期權

|  |  |
| --- | --- |
| 欄位名稱 | 三大法人買方未平倉 (資料欄位) |
| 語法 | ``` Value1 = GetField("三大法人買方未平倉"); Value1 = GetField("MITLongOI"); ``` |
| 參數範圍 | Call、Put |
| 欄位分類 | 期權 |
| 單位 | 口 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 |
| 支援商品 | 期貨 選擇權 |
| 說明 | 三大法人多方的未平倉口數。  如果是期貨商品的話，數值是所有月份契約(含週契約)的加總。  如果是選擇權商品的話，系統會根據目前商品的買賣權別，回傳買權或是賣權所有履約價/月份契約(含週契約)的加總。  如果商品是選擇權現貨商品的話，則可以傳入參數，分別取得買權或是賣權的數值：  GetSymbolField("TXO00.TF", "三大法人買方未平倉口數", param:="CALL") GetSymbolField("TXO00.TF", "三大法人買方未平倉口數", param:="PUT") |

---
## 三大法人買方未平倉金額
**Path:** 資料欄位 > 期權

|  |  |
| --- | --- |
| 欄位名稱 | 三大法人買方未平倉金額 (資料欄位) |
| 語法 | ``` Value1 = GetField("三大法人買方未平倉金額"); Value1 = GetField("MITLongOIAmount"); ``` |
| 參數範圍 | Call、Put |
| 欄位分類 | 期權 |
| 單位 | 千元 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 |
| 支援商品 | 期貨 選擇權 |
| 說明 | 三大法人多方的未平倉金額。  如果是期貨商品的話，數值是所有月份契約(含週契約)的加總。  如果是選擇權商品的話，系統會根據目前商品的買賣權別，回傳買權或是賣權所有履約價/月份契約(含週契約)的加總。  如果商品是選擇權現貨商品的話，則可以傳入參數，分別取得買權或是賣權的數值：  GetSymbolField("TXO00.TF", "三大法人買方未平倉金額", param:="CALL") GetSymbolField("TXO00.TF", "三大法人買方未平倉金額", param:="PUT") |

---
## 三大法人賣方未平倉
**Path:** 資料欄位 > 期權

|  |  |
| --- | --- |
| 欄位名稱 | 三大法人賣方未平倉 (資料欄位) |
| 語法 | ``` Value1 = GetField("三大法人賣方未平倉"); Value1 = GetField("MITShortOI"); ``` |
| 參數範圍 | Call、Put |
| 欄位分類 | 期權 |
| 單位 | 口 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 |
| 支援商品 | 期貨 選擇權 |
| 說明 | 三大法人空方的未平倉口數。  如果是期貨商品的話，數值是所有月份契約(含週契約)的加總。  如果是選擇權商品的話，系統會根據目前商品的買賣權別，回傳買權或是賣權所有履約價/月份契約(含週契約)的加總。  如果商品是選擇權現貨商品的話，則可以傳入參數，分別取得買權或是賣權的數值：  GetSymbolField("TXO00.TF", "三大法人賣方未平倉口數", param:="CALL") GetSymbolField("TXO00.TF", "三大法人賣方未平倉口數", param:="PUT") |

---
## 三大法人賣方未平倉金額
**Path:** 資料欄位 > 期權

|  |  |
| --- | --- |
| 欄位名稱 | 三大法人賣方未平倉金額 (資料欄位) |
| 語法 | ``` Value1 = GetField("三大法人賣方未平倉金額"); Value1 = GetField("MITShortOIAmount"); ``` |
| 參數範圍 | Call、Put |
| 欄位分類 | 期權 |
| 單位 | 千元 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 |
| 支援商品 | 期貨 選擇權 |
| 說明 | 三大法人空方的未平倉金額。  如果是期貨商品的話，數值是所有月份契約(含週契約)的加總。  如果是選擇權商品的話，系統會根據目前商品的買賣權別，回傳買權或是賣權所有履約價/月份契約(含週契約)的加總。  如果商品是選擇權現貨商品的話，則可以傳入參數，分別取得買權或是賣權的數值：  GetSymbolField("TXO00.TF", "三大法人買方未平倉金額", param:="CALL") GetSymbolField("TXO00.TF", "三大法人買方未平倉金額", param:="PUT") |

---
## 五大交易人未沖銷買口
**Path:** 資料欄位 > 期權

|  |  |
| --- | --- |
| 欄位名稱 | 五大交易人未沖銷買口 (資料欄位) |
| 語法 | ``` Value1 = GetField("五大交易人未沖銷買口"); Value1 = GetField("LT5BuyOI"); ``` |
| 參數範圍 | Call、Put |
| 欄位分類 | 期權 |
| 單位 | 口 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 |
| 支援商品 | 期貨 選擇權 |
| 說明 | 前五大交易人買方未沖銷口數。  如果是期貨商品的話，數值是所有月份契約(含週契約)的加總。  如果是選擇權商品的話，系統會根據目前商品的買賣權別，回傳買權或是賣權所有履約價/月份契約(含週契約)的加總。  如果商品是選擇權現貨商品的話，則可以傳入參數，分別取得買權或是賣權的數值：  GetSymbolField("TXO00.TF", "五大交易人未沖銷買口", param:="CALL") GetSymbolField("TXO00.TF", "五大交易人未沖銷買口", param:="PUT") |

---
## 五大交易人未沖銷賣口
**Path:** 資料欄位 > 期權

|  |  |
| --- | --- |
| 欄位名稱 | 五大交易人未沖銷賣口 (資料欄位) |
| 語法 | ``` Value1 = GetField("五大交易人未沖銷賣口"); Value1 = GetField("LT5SellOI"); ``` |
| 參數範圍 | Call、Put |
| 欄位分類 | 期權 |
| 單位 | 口 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 |
| 支援商品 | 期貨 選擇權 |
| 說明 | 前五大交易人賣方未沖銷口數。  如果是期貨商品的話，數值是所有月份契約(含週契約)的加總。  如果是選擇權商品的話，系統會根據目前商品的買賣權別，回傳買權或是賣權所有履約價/月份契約(含週契約)的加總。  如果商品是選擇權現貨商品的話，則可以傳入參數，分別取得買權或是賣權的數值：  GetSymbolField("TXO00.TF", "五大交易人未沖銷賣口", param:="CALL") GetSymbolField("TXO00.TF", "五大交易人未沖銷賣口", param:="PUT") |

---
## 五大法人未沖銷買口
**Path:** 資料欄位 > 期權

|  |  |
| --- | --- |
| 欄位名稱 | 五大法人未沖銷買口 (資料欄位) |
| 語法 | ``` Value1 = GetField("五大法人未沖銷買口"); Value1 = GetField("LT5SIIBuyOI"); ``` |
| 參數範圍 | Call、Put |
| 欄位分類 | 期權 |
| 單位 | 口 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 |
| 支援商品 | 期貨 選擇權 |
| 說明 | 前五大法人買方未沖銷口數。  如果是期貨商品的話，數值是所有月份契約(含週契約)的加總。  如果是選擇權商品的話，系統會根據目前商品的買賣權別，回傳買權或是賣權所有履約價/月份契約(含週契約)的加總。  如果商品是選擇權現貨商品的話，則可以傳入參數，分別取得買權或是賣權的數值：  GetSymbolField("TXO00.TF", "五大法人未沖銷買口", param:="CALL") GetSymbolField("TXO00.TF", "五大法人未沖銷買口", param:="PUT") |

---
## 五大法人未沖銷賣口
**Path:** 資料欄位 > 期權

|  |  |
| --- | --- |
| 欄位名稱 | 五大法人未沖銷賣口 (資料欄位) |
| 語法 | ``` Value1 = GetField("五大法人未沖銷賣口"); Value1 = GetField("LT5SIISellOI"); ``` |
| 參數範圍 | Call、Put |
| 欄位分類 | 期權 |
| 單位 | 口 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 |
| 支援商品 | 期貨 選擇權 |
| 說明 | 前五大法人賣方未沖銷口數。  如果是期貨商品的話，數值是所有月份契約(含週契約)的加總。  如果是選擇權商品的話，系統會根據目前商品的買賣權別，回傳買權或是賣權所有履約價/月份契約(含週契約)的加總。  如果商品是選擇權現貨商品的話，則可以傳入參數，分別取得買權或是賣權的數值：  GetSymbolField("TXO00.TF", "五大法人未沖銷賣口", param:="CALL") GetSymbolField("TXO00.TF", "五大法人未沖銷賣口", param:="PUT") |

---
## 估計除息點數
**Path:** 資料欄位 > 期權

|  |  |
| --- | --- |
| 欄位名稱 | 估計除息點數 (資料欄位) |
| 語法 | ``` Value1 = GetField("估計除息點數"); Value1 = GetField("EstimatedExDividendPoint"); ``` |
| 欄位分類 | 期權 |
| 單位 | 點 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 |
| 支援商品 | 期貨 |
| 說明 | 受成分股內除息所影響的估計點數。  計算方式是加總從今日到到期日之間，所有成分股的除息所影響的點數。影響點數的計算方式是現金股利／當日收盤價＊商品佔指數的比重。 |

---
## 內含值
**Path:** 資料欄位 > 期權

|  |  |
| --- | --- |
| 欄位名稱 | 內含值 (資料欄位) |
| 語法 | ``` Value1 = GetField("內含值"); Value1 = GetField("IntrinsicValue"); ``` |
| 欄位分類 | 期權 |
| 單位 | 點 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 |
| 支援商品 | 選擇權 |
| 說明 | 選擇權的內含值(Intrinsic value)，也就是標的股現價與履約價的差異。  如果是買權的話，計算公式為MAX(標的股現價－履約價，0)。 如果是賣權的話，計算公式為MAX(履約價－標的股現價，0)。  週頻率以上為統計期間的期底值。 |

---
## 十大交易人未沖銷買口
**Path:** 資料欄位 > 期權

|  |  |
| --- | --- |
| 欄位名稱 | 十大交易人未沖銷買口 (資料欄位) |
| 語法 | ``` Value1 = GetField("十大交易人未沖銷買口"); Value1 = GetField("LT10BuyOI"); ``` |
| 參數範圍 | Call、Put |
| 欄位分類 | 期權 |
| 單位 | 口 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 |
| 支援商品 | 期貨 選擇權 |
| 說明 | 前十大交易人買方未沖銷口數。  如果是期貨商品的話，數值是所有月份契約(含週契約)的加總。  如果是選擇權商品的話，系統會根據目前商品的買賣權別，回傳買權或是賣權所有履約價/月份契約(含週契約)的加總。  如果商品是選擇權現貨商品的話，則可以傳入參數，分別取得買權或是賣權的數值：  GetSymbolField("TXO00.TF", "十大交易人未沖銷買口", param:="Call") GetSymbolField("TXO00.TF", "十大交易人未沖銷買口", param:="Put") |

---
## 十大交易人未沖銷賣口
**Path:** 資料欄位 > 期權

|  |  |
| --- | --- |
| 欄位名稱 | 十大交易人未沖銷賣口 (資料欄位) |
| 語法 | ``` Value1 = GetField("十大交易人未沖銷賣口"); Value1 = GetField("LT10SellOI"); ``` |
| 參數範圍 | Call、Put |
| 欄位分類 | 期權 |
| 單位 | 口 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 |
| 支援商品 | 期貨 選擇權 |
| 說明 | 前十大交易人賣方未沖銷口數。  如果是期貨商品的話，數值是所有月份契約(含週契約)的加總。  如果是選擇權商品的話，系統會根據目前商品的買賣權別，回傳買權或是賣權所有履約價/月份契約(含週契約)的加總。  如果商品是選擇權現貨商品的話，則可以傳入參數，分別取得買權或是賣權的數值：  GetSymbolField("TXO00.TF", "十大交易人未沖銷賣口", param:="CALL") GetSymbolField("TXO00.TF", "十大交易人未沖銷賣口", param:="PUT") |

---
## 十大法人未沖銷買口
**Path:** 資料欄位 > 期權

|  |  |
| --- | --- |
| 欄位名稱 | 十大法人未沖銷買口 (資料欄位) |
| 語法 | ``` Value1 = GetField("十大法人未沖銷買口"); Value1 = GetField("LT10SIIBuyOI"); ``` |
| 參數範圍 | Call、Put |
| 欄位分類 | 期權 |
| 單位 | 口 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 |
| 支援商品 | 期貨 選擇權 |
| 說明 | 前十大法人買方未沖銷口數。  如果是期貨商品的話，數值是所有月份契約(含週契約)的加總。  如果是選擇權商品的話，系統會根據目前商品的買賣權別，回傳買權或是賣權所有履約價/月份契約(含週契約)的加總。  如果商品是選擇權現貨商品的話，則可以傳入參數，分別取得買權或是賣權的數值：  GetSymbolField("TXO00.TF", "十大法人未沖銷買口", param:="CALL") GetSymbolField("TXO00.TF", "十大法人未沖銷買口", param:="PUT") |

---
## 十大法人未沖銷賣口
**Path:** 資料欄位 > 期權

|  |  |
| --- | --- |
| 欄位名稱 | 十大法人未沖銷賣口 (資料欄位) |
| 語法 | ``` Value1 = GetField("十大法人未沖銷賣口"); Value1 = GetField("LT10SIISellOI"); ``` |
| 參數範圍 | Call、Put |
| 欄位分類 | 期權 |
| 單位 | 口 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 |
| 支援商品 | 期貨 選擇權 |
| 說明 | 前十大法人賣方未沖銷口數。  如果是期貨商品的話，數值是所有月份契約(含週契約)的加總。  如果是選擇權商品的話，系統會根據目前商品的買賣權別，回傳買權或是賣權所有履約價/月份契約(含週契約)的加總。  如果商品是選擇權現貨商品的話，則可以傳入參數，分別取得買權或是賣權的數值：  GetSymbolField("TXO00.TF", "十大法人未沖銷賣口", param:="Call") GetSymbolField("TXO00.TF", "十大法人未沖銷賣口", param:="Put") |

---
## 外資交易買口
**Path:** 資料欄位 > 期權

|  |  |
| --- | --- |
| 欄位名稱 | 外資交易買口 (資料欄位) |
| 語法 | ``` Value1 = GetField("外資交易買口"); Value1 = GetField("FINILongLots"); ``` |
| 參數範圍 | Call、Put |
| 欄位分類 | 期權 |
| 單位 | 口 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 |
| 支援商品 | 期貨 選擇權 |
| 說明 | 外資多方的交易口數。  如果是期貨商品的話，數值是所有月份契約(含週契約)的加總。  如果是選擇權商品的話，系統會根據目前商品的買賣權別，回傳買權或是賣權所有履約價/月份契約(含週契約)的加總。  如果商品是選擇權現貨商品的話，則可以傳入參數，分別取得買權或是賣權的數值：  GetSymbolField("TXO00.TF", "外資交易買口", param:="CALL") GetSymbolField("TXO00.TF", "外資交易買口", param:="PUT") |

---
## 外資交易買進金額
**Path:** 資料欄位 > 期權

|  |  |
| --- | --- |
| 欄位名稱 | 外資交易買進金額 (資料欄位) |
| 語法 | ``` Value1 = GetField("外資交易買進金額"); Value1 = GetField("FINIBAmount"); ``` |
| 參數範圍 | Call、Put |
| 欄位分類 | 期權 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 |
| 支援商品 | 期貨 選擇權 |
| 說明 | 外資多方的交易金額。 |

---
## 外資交易賣出金額
**Path:** 資料欄位 > 期權

|  |  |
| --- | --- |
| 欄位名稱 | 外資交易賣出金額 (資料欄位) |
| 語法 | ``` Value1 = GetField("外資交易賣出金額"); Value1 = GetField("FINISAmount"); ``` |
| 參數範圍 | Call、Put |
| 欄位分類 | 期權 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 |
| 支援商品 | 期貨 選擇權 |
| 說明 | 外資空方的交易金額。 |

---
## 外資交易賣口
**Path:** 資料欄位 > 期權

|  |  |
| --- | --- |
| 欄位名稱 | 外資交易賣口 (資料欄位) |
| 語法 | ``` Value1 = GetField("外資交易賣口"); Value1 = GetField("FINIShortLots"); ``` |
| 參數範圍 | Call、Put |
| 欄位分類 | 期權 |
| 單位 | 口 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 |
| 支援商品 | 期貨 選擇權 |
| 說明 | 外資空方的交易口數。  如果是期貨商品的話，數值是所有月份契約(含週契約)的加總。  如果是選擇權商品的話，系統會根據目前商品的買賣權別，回傳買權或是賣權所有履約價/月份契約(含週契約)的加總。  如果商品是選擇權現貨商品的話，則可以傳入參數，分別取得買權或是賣權的數值：  GetSymbolField("TXO00.TF", "外資交易賣口", param:="CALL") GetSymbolField("TXO00.TF", "外資交易賣口", param:="PUT") |

---
## 外資買方未平倉口數
**Path:** 資料欄位 > 期權

|  |  |
| --- | --- |
| 欄位名稱 | 外資買方未平倉口數 (資料欄位) |
| 語法 | ``` Value1 = GetField("外資買方未平倉口數"); Value1 = GetField("FINILongOI"); ``` |
| 參數範圍 | Call、Put |
| 欄位分類 | 期權 |
| 單位 | 口 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 |
| 支援商品 | 期貨 選擇權 |
| 說明 | 外資多方的未平倉口數。  如果是期貨商品的話，數值是所有月份契約(含週契約)的加總。  如果是選擇權商品的話，系統會根據目前商品的買賣權別，回傳買權或是賣權所有履約價/月份契約(含週契約)的加總。  如果商品是選擇權現貨商品的話，則可以傳入參數，分別取得買權或是賣權的數值：  GetSymbolField("TXO00.TF", "外資買方未平倉口數", param:="CALL") GetSymbolField("TXO00.TF", "外資買方未平倉口數", param:="PUT") |

---
## 外資買方未平倉金額
**Path:** 資料欄位 > 期權

|  |  |
| --- | --- |
| 欄位名稱 | 外資買方未平倉金額 (資料欄位) |
| 語法 | ``` Value1 = GetField("外資買方未平倉金額"); Value1 = GetField("FINILongOIAmount"); ``` |
| 參數範圍 | Call、Put |
| 欄位分類 | 期權 |
| 單位 | 千元 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 |
| 支援商品 | 期貨 選擇權 |
| 說明 | 外資多方的未平倉金額。  如果是期貨商品的話，數值是所有月份契約(含週契約)的加總。  如果是選擇權商品的話，系統會根據目前商品的買賣權別，回傳買權或是賣權所有履約價/月份契約(含週契約)的加總。  如果商品是選擇權現貨商品的話，則可以傳入參數，分別取得買權或是賣權的數值：  GetSymbolField("TXO00.TF", "外資買方未平倉金額", param:="CALL") GetSymbolField("TXO00.TF", "外資買方未平倉金額", param:="PUT") |

---
## 外資賣方未平倉口數
**Path:** 資料欄位 > 期權

|  |  |
| --- | --- |
| 欄位名稱 | 外資賣方未平倉口數 (資料欄位) |
| 語法 | ``` Value1 = GetField("外資賣方未平倉口數"); Value1 = GetField("FINIShortOI"); ``` |
| 參數範圍 | Call、Put |
| 欄位分類 | 期權 |
| 單位 | 口 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 |
| 支援商品 | 期貨 選擇權 |
| 說明 | 外資空方的未平倉口數。  如果是期貨商品的話，數值是所有月份契約(含週契約)的加總。  如果是選擇權商品的話，系統會根據目前商品的買賣權別，回傳買權或是賣權所有履約價/月份契約(含週契約)的加總。  如果商品是選擇權現貨商品的話，則可以傳入參數，分別取得買權或是賣權的數值：  GetSymbolField("TXO00.TF", "外資賣方未平倉口數", param:="CALL") GetSymbolField("TXO00.TF", "外資賣方未平倉口數", param:="PUT") |

---
## 外資賣方未平倉金額
**Path:** 資料欄位 > 期權

|  |  |
| --- | --- |
| 欄位名稱 | 外資賣方未平倉金額 (資料欄位) |
| 語法 | ``` Value1 = GetField("外資賣方未平倉金額"); Value1 = GetField("FINIShortOIAmount"); ``` |
| 參數範圍 | Call、Put |
| 欄位分類 | 期權 |
| 單位 | 千元 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 |
| 支援商品 | 期貨 選擇權 |
| 說明 | 外資空方的未平倉金額。  如果是期貨商品的話，數值是所有月份契約(含週契約)的加總。  如果是選擇權商品的話，系統會根據目前商品的買賣權別，回傳買權或是賣權所有履約價/月份契約(含週契約)的加總。  如果商品是選擇權現貨商品的話，則可以傳入參數，分別取得買權或是賣權的數值：  GetSymbolField("TXO00.TF", "外資買方未平倉金額", param:="CALL") GetSymbolField("TXO00.TF", "外資買方未平倉金額", param:="PUT") |

---
## 投信交易買口
**Path:** 資料欄位 > 期權

|  |  |
| --- | --- |
| 欄位名稱 | 投信交易買口 (資料欄位) |
| 語法 | ``` Value1 = GetField("投信交易買口"); Value1 = GetField("SBuyLots"); ``` |
| 參數範圍 | Call、Put |
| 欄位分類 | 期權 |
| 單位 | 口 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 |
| 支援商品 | 期貨 選擇權 |
| 說明 | 投信多方的交易口數。  如果是期貨商品的話，數值是所有月份契約(含週契約)的加總。  如果是選擇權商品的話，系統會根據目前商品的買賣權別，回傳買權或是賣權所有履約價/月份契約(含週契約)的加總。  如果商品是選擇權現貨商品的話，則可以傳入參數，分別取得買權或是賣權的數值：  GetSymbolField("TXO00.TF", "投信交易買口", param:="CALL") GetSymbolField("TXO00.TF", "投信交易買口", param:="PUT") |

---
## 投信交易買進金額
**Path:** 資料欄位 > 期權

|  |  |
| --- | --- |
| 欄位名稱 | 投信交易買進金額 (資料欄位) |
| 語法 | ``` Value1 = GetField("投信交易買進金額"); Value1 = GetField("SBuyAmount"); ``` |
| 參數範圍 | Call、Put |
| 欄位分類 | 期權 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 |
| 支援商品 | 期貨 選擇權 |
| 說明 | 投信多方的交易金額。 |

---
## 投信交易賣出金額
**Path:** 資料欄位 > 期權

|  |  |
| --- | --- |
| 欄位名稱 | 投信交易賣出金額 (資料欄位) |
| 語法 | ``` Value1 = GetField("投信交易賣出金額"); Value1 = GetField("SSellAmount"); ``` |
| 參數範圍 | Call、Put |
| 欄位分類 | 期權 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 |
| 支援商品 | 期貨 選擇權 |
| 說明 | 投信空方的交易金額。 |

---
## 投信交易賣口
**Path:** 資料欄位 > 期權

|  |  |
| --- | --- |
| 欄位名稱 | 投信交易賣口 (資料欄位) |
| 語法 | ``` Value1 = GetField("投信交易賣口"); Value1 = GetField("SSellLots"); ``` |
| 參數範圍 | Call、Put |
| 欄位分類 | 期權 |
| 單位 | 口 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 |
| 支援商品 | 期貨 選擇權 |
| 說明 | 投信空方的交易口數。  如果是期貨商品的話，數值是所有月份契約(含週契約)的加總。  如果是選擇權商品的話，系統會根據目前商品的買賣權別，回傳買權或是賣權所有履約價/月份契約(含週契約)的加總。  如果商品是選擇權現貨商品的話，則可以傳入參數，分別取得買權或是賣權的數值：  GetSymbolField("TXO00.TF", "投信交易賣口", param:="CALL") GetSymbolField("TXO00.TF", "投信交易賣口", param:="PUT") |

---
## 投信買方未平倉口數
**Path:** 資料欄位 > 期權

|  |  |
| --- | --- |
| 欄位名稱 | 投信買方未平倉口數 (資料欄位) |
| 語法 | ``` Value1 = GetField("投信買方未平倉口數"); Value1 = GetField("SLongOI"); ``` |
| 參數範圍 | Call、Put |
| 欄位分類 | 期權 |
| 單位 | 口 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 |
| 支援商品 | 期貨 選擇權 |
| 說明 | 投信多方的未平倉口數。  如果是期貨商品的話，數值是所有月份契約(含週契約)的加總。  如果是選擇權商品的話，系統會根據目前商品的買賣權別，回傳買權或是賣權所有履約價/月份契約(含週契約)的加總。  如果商品是選擇權現貨商品的話，則可以傳入參數，分別取得買權或是賣權的數值：  GetSymbolField("TXO00.TF", "投信買方未平倉口數", param:="CALL") GetSymbolField("TXO00.TF", "投信買方未平倉口數", param:="PUT") |

---
## 投信買方未平倉金額
**Path:** 資料欄位 > 期權

|  |  |
| --- | --- |
| 欄位名稱 | 投信買方未平倉金額 (資料欄位) |
| 語法 | ``` Value1 = GetField("投信買方未平倉金額"); Value1 = GetField("SLongOIAmount"); ``` |
| 參數範圍 | Call、Put |
| 欄位分類 | 期權 |
| 單位 | 千元 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 |
| 支援商品 | 期貨 選擇權 |
| 說明 | 投信多方的未平倉金額。  如果是期貨商品的話，數值是所有月份契約(含週契約)的加總。  如果是選擇權商品的話，系統會根據目前商品的買賣權別，回傳買權或是賣權所有履約價/月份契約(含週契約)的加總。  如果商品是選擇權現貨商品的話，則可以傳入參數，分別取得買權或是賣權的數值：  GetSymbolField("TXO00.TF", "投信買方未平倉金額", param:="CALL") GetSymbolField("TXO00.TF", "投信買方未平倉金額", param:="PUT") |

---
## 投信賣方未平倉口數
**Path:** 資料欄位 > 期權

|  |  |
| --- | --- |
| 欄位名稱 | 投信賣方未平倉口數 (資料欄位) |
| 語法 | ``` Value1 = GetField("投信賣方未平倉口數"); Value1 = GetField("SShortOI"); ``` |
| 參數範圍 | Call、Put |
| 欄位分類 | 期權 |
| 單位 | 口 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 |
| 支援商品 | 期貨 選擇權 |
| 說明 | 投信空方的未平倉口數。  如果是期貨商品的話，數值是所有月份契約(含週契約)的加總。  如果是選擇權商品的話，系統會根據目前商品的買賣權別，回傳買權或是賣權所有履約價/月份契約(含週契約)的加總。  如果商品是選擇權現貨商品的話，則可以傳入參數，分別取得買權或是賣權的數值：  GetSymbolField("TXO00.TF", "投信賣方未平倉口數", param:="CALL") GetSymbolField("TXO00.TF", "投信賣方未平倉口數", param:="PUT") |

---
## 投信賣方未平倉金額
**Path:** 資料欄位 > 期權

|  |  |
| --- | --- |
| 欄位名稱 | 投信賣方未平倉金額 (資料欄位) |
| 語法 | ``` Value1 = GetField("投信賣方未平倉金額"); Value1 = GetField("SShortOIAmount"); ``` |
| 參數範圍 | Call、Put |
| 欄位分類 | 期權 |
| 單位 | 千元 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 |
| 支援商品 | 期貨 選擇權 |
| 說明 | 投信空方的未平倉金額。  如果是期貨商品的話，數值是所有月份契約(含週契約)的加總。  如果是選擇權商品的話，系統會根據目前商品的買賣權別，回傳買權或是賣權所有履約價/月份契約(含週契約)的加總。  如果商品是選擇權現貨商品的話，則可以傳入參數，分別取得買權或是賣權的數值：  GetSymbolField("TXO00.TF", "投信買方未平倉金額", param:="CALL") GetSymbolField("TXO00.TF", "投信買方未平倉金額", param:="PUT") |

---
## 時間價值
**Path:** 資料欄位 > 期權

|  |  |
| --- | --- |
| 欄位名稱 | 時間價值 (資料欄位) |
| 語法 | ``` Value1 = GetField("時間價值"); Value1 = GetField("TimeValue"); ``` |
| 欄位分類 | 期權 |
| 單位 | 點 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 |
| 支援商品 | 台股 選擇權 |
| 說明 | 選擇權或是權證商品的時間價值(Time value)。  如果是選擇權的話，計算公式為 MAX(選擇權價格 ─ 內含值，0)。  如果是權證商品的話，則先計算權證商品的內含值：如果是認購權證的話(CALL)，內含值為MAX(標的股現價－履約價，0)＊執行比例，如果是認售權證的話(PUT)，內含值為MAX(履約價－標的股現價，0)＊執行比例。  有了內含值後，權證商品的時間價值也是同樣的公式：MAX(權證價格 ─ 內含值，0)。  週頻率以上為統計期間的期底值。 |

---
## 未平倉
**Path:** 資料欄位 > 期權

|  |  |
| --- | --- |
| 欄位名稱 | 未平倉 (資料欄位) |
| 語法 | ``` Value1 = GetField("未平倉"); Value1 = GetField("OpenInterest"); ``` |
| 參數範圍 | Call、Put |
| 欄位分類 | 期權 |
| 單位 | 口 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 |
| 支援商品 | 期貨 選擇權 |
| 說明 | 未平倉口數。 |

---
## 波動性指數
**Path:** 資料欄位 > 期權

|  |  |
| --- | --- |
| 欄位名稱 | 波動性指數 (資料欄位) |
| 語法 | ``` Value1 = GetField("波動性指數"); Value1 = GetField("VolaIndex"); ``` |
| 欄位分類 | 期權 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 |
| 支援商品 | 期貨 選擇權 |
| 說明 | 期交所所公布的「臺指選擇權波動率指數」。  此指標是期交所以CBOE VIX指數編製公式計算出的當月臺指選擇權的波動率指數。  亦可使用GetSymbolField("VIX.TF", "Close")來取得最新的數值。 |

---
## 波動率
**Path:** 資料欄位 > 期權

|  |  |
| --- | --- |
| 欄位名稱 | 波動率 (資料欄位) |
| 語法 | ``` Value1 = GetField("波動率"); Value1 = GetField("Volatility"); ``` |
| 欄位分類 | 期權 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 分鐘 日 週 月 還原日 還原週 還原月 季 半年 年 |
| 支援商品 | 台股 大盤 類股指數 台(權證) 香港股票 |
| 說明 | 選擇權／權證商品標的股的歷史波動率。 單位是％，例如數值如果是23.12％，則回傳23.12。  計算方式是使用最近20天的股價變化，算出年化的歷史波動率，以此來衡量標的股股價的波動幅度。  除了選擇權／權證商品之外，有發行衍生性商品的台股或是指數商品也都支援此欄位。  週頻率以上為統計期間的期底值。 |

---
## 理論價
**Path:** 資料欄位 > 期權

|  |  |
| --- | --- |
| 欄位名稱 | 理論價 (資料欄位) |
| 語法 | ``` Value1 = GetField("理論價"); Value1 = GetField("TheoreticalPrice"); ``` |
| 欄位分類 | 期權 |
| 單位 | 點 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 分鐘 日 週 月 |
| 支援商品 | 台(權證) 選擇權 |
| 說明 | 選擇權／權證商品的理論價格，採用Black-Scholes模型計算。  可以比對現價與理論價的差異，判斷目前價格的乖離程度。  週頻率以上為統計期間的期底值。 |

---
## 自營商交易買口
**Path:** 資料欄位 > 期權

|  |  |
| --- | --- |
| 欄位名稱 | 自營商交易買口 (資料欄位) |
| 語法 | ``` Value1 = GetField("自營商交易買口"); Value1 = GetField("DBuyLots"); ``` |
| 參數範圍 | Call、Put |
| 欄位分類 | 期權 |
| 單位 | 口 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 |
| 支援商品 | 期貨 選擇權 |
| 說明 | 自營商多方的交易口數。  如果是期貨商品的話，數值是所有月份契約(含週契約)的加總。  如果是選擇權商品的話，系統會根據目前商品的買賣權別，回傳買權或是賣權所有履約價/月份契約(含週契約)的加總。  如果商品是選擇權現貨商品的話，則可以傳入參數，分別取得買權或是賣權的數值：  GetSymbolField("TXO00.TF", "自營商交易買口", param:="CALL") GetSymbolField("TXO00.TF", "自營商交易買口", param:="PUT") |

---
## 自營商交易買進金額
**Path:** 資料欄位 > 期權

|  |  |
| --- | --- |
| 欄位名稱 | 自營商交易買進金額 (資料欄位) |
| 語法 | ``` Value1 = GetField("自營商交易買進金額"); Value1 = GetField("DBuyAmount"); ``` |
| 參數範圍 | Call、Put |
| 欄位分類 | 期權 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 |
| 支援商品 | 期貨 選擇權 |
| 說明 | 自營商多方的交易金額。 |

---
## 自營商交易賣出金額
**Path:** 資料欄位 > 期權

|  |  |
| --- | --- |
| 欄位名稱 | 自營商交易賣出金額 (資料欄位) |
| 語法 | ``` Value1 = GetField("自營商交易賣出金額"); Value1 = GetField("DSellAmount"); ``` |
| 參數範圍 | Call、Put |
| 欄位分類 | 期權 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 |
| 支援商品 | 期貨 選擇權 |
| 說明 | 自營商空方的交易金額。 |

---
## 自營商交易賣口
**Path:** 資料欄位 > 期權

|  |  |
| --- | --- |
| 欄位名稱 | 自營商交易賣口 (資料欄位) |
| 語法 | ``` Value1 = GetField("自營商交易賣口"); Value1 = GetField("DSellLots"); ``` |
| 參數範圍 | Call、Put |
| 欄位分類 | 期權 |
| 單位 | 口 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 |
| 支援商品 | 期貨 選擇權 |
| 說明 | 自營商空方的交易口數。  如果是期貨商品的話，數值是所有月份契約(含週契約)的加總。  如果是選擇權商品的話，系統會根據目前商品的買賣權別，回傳買權或是賣權所有履約價/月份契約(含週契約)的加總。  如果商品是選擇權現貨商品的話，則可以傳入參數，分別取得買權或是賣權的數值：  GetSymbolField("TXO00.TF", "自營商交易賣口", param:="CALL") GetSymbolField("TXO00.TF", "自營商交易賣口", param:="PUT") |

---
## 自營商買方未平倉口數
**Path:** 資料欄位 > 期權

|  |  |
| --- | --- |
| 欄位名稱 | 自營商買方未平倉口數 (資料欄位) |
| 語法 | ``` Value1 = GetField("自營商買方未平倉口數"); Value1 = GetField("DLongOI"); ``` |
| 參數範圍 | Call、Put |
| 欄位分類 | 期權 |
| 單位 | 口 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 |
| 支援商品 | 期貨 選擇權 |
| 說明 | 自營商多方的未平倉口數。  如果是期貨商品的話，數值是所有月份契約(含週契約)的加總。  如果是選擇權商品的話，系統會根據目前商品的買賣權別，回傳買權或是賣權所有履約價/月份契約(含週契約)的加總。  如果商品是選擇權現貨商品的話，則可以傳入參數，分別取得買權或是賣權的數值：  GetSymbolField("TXO00.TF", "自營商買方未平倉口數", param:="CALL") GetSymbolField("TXO00.TF", "自營商買方未平倉口數", param:="PUT") |

---
## 自營商買方未平倉金額
**Path:** 資料欄位 > 期權

|  |  |
| --- | --- |
| 欄位名稱 | 自營商買方未平倉金額 (資料欄位) |
| 語法 | ``` Value1 = GetField("自營商買方未平倉金額"); Value1 = GetField("DLongOIAmount"); ``` |
| 參數範圍 | Call、Put |
| 欄位分類 | 期權 |
| 單位 | 千元 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 |
| 支援商品 | 期貨 選擇權 |
| 說明 | 自營商多方的未平倉金額。  如果是期貨商品的話，數值是所有月份契約(含週契約)的加總。  如果是選擇權商品的話，系統會根據目前商品的買賣權別，回傳買權或是賣權所有履約價/月份契約(含週契約)的加總。  如果商品是選擇權現貨商品的話，則可以傳入參數，分別取得買權或是賣權的數值：  GetSymbolField("TXO00.TF", "自營商買方未平倉金額", param:="CALL") GetSymbolField("TXO00.TF", "自營商買方未平倉金額", param:="PUT") |

---
## 自營商賣方未平倉口數
**Path:** 資料欄位 > 期權

|  |  |
| --- | --- |
| 欄位名稱 | 自營商賣方未平倉口數 (資料欄位) |
| 語法 | ``` Value1 = GetField("自營商賣方未平倉口數"); Value1 = GetField("DShortOI"); ``` |
| 參數範圍 | Call、Put |
| 欄位分類 | 期權 |
| 單位 | 口 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 |
| 支援商品 | 期貨 選擇權 |
| 說明 | 自營商空方的未平倉口數。  如果是期貨商品的話，數值是所有月份契約(含週契約)的加總。  如果是選擇權商品的話，系統會根據目前商品的買賣權別，回傳買權或是賣權所有履約價/月份契約(含週契約)的加總。  如果商品是選擇權現貨商品的話，則可以傳入參數，分別取得買權或是賣權的數值：  GetSymbolField("TXO00.TF", "自營商賣方未平倉口數", param:="CALL") GetSymbolField("TXO00.TF", "自營商賣方未平倉口數", param:="PUT") |

---
## 自營商賣方未平倉金額
**Path:** 資料欄位 > 期權

|  |  |
| --- | --- |
| 欄位名稱 | 自營商賣方未平倉金額 (資料欄位) |
| 語法 | ``` Value1 = GetField("自營商賣方未平倉金額"); Value1 = GetField("DShortOIAmount"); ``` |
| 參數範圍 | Call、Put |
| 欄位分類 | 期權 |
| 單位 | 千元 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 |
| 支援商品 | 期貨 選擇權 |
| 說明 | 自營商空方的未平倉金額。  如果是期貨商品的話，數值是所有月份契約(含週契約)的加總。  如果是選擇權商品的話，系統會根據目前商品的買賣權別，回傳買權或是賣權所有履約價/月份契約(含週契約)的加總。  如果商品是選擇權現貨商品的話，則可以傳入參數，分別取得買權或是賣權的數值：  GetSymbolField("TXO00.TF", "自營商買方未平倉金額", param:="CALL") GetSymbolField("TXO00.TF", "自營商買方未平倉金額", param:="PUT") |

---
## 買權成交量
**Path:** 資料欄位 > 期權

|  |  |
| --- | --- |
| 欄位名稱 | 買權成交量 (資料欄位) |
| 語法 | ``` Value1 = GetField("買權成交量"); Value1 = GetField("CallTotalVolume"); ``` |
| 欄位分類 | 期權 |
| 單位 | 口 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 |
| 支援商品 | 選擇權 |
| 說明 | 開盤到現在的買權成交量總和。 夜盤商品不納入計算。 |

---
## 買權未平倉量
**Path:** 資料欄位 > 期權

|  |  |
| --- | --- |
| 欄位名稱 | 買權未平倉量 (資料欄位) |
| 語法 | ``` Value1 = GetField("買權未平倉量"); Value1 = GetField("CallTotalOI"); ``` |
| 欄位分類 | 期權 |
| 單位 | 口 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 |
| 支援商品 | 選擇權 |
| 說明 | 同契約所有買權商品(CALL)的未平倉量。  統計範圍包含所有履約價/月份(含週契約)。 |

---
## 買賣權成交量比率
**Path:** 資料欄位 > 期權

|  |  |
| --- | --- |
| 欄位名稱 | 買賣權成交量比率 (資料欄位) |
| 語法 | ``` Value1 = GetField("買賣權成交量比率"); Value1 = GetField("CPTradeRatio"); ``` |
| 欄位分類 | 期權 |
| 單位 | 比值 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 |
| 支援商品 | 選擇權 |
| 說明 | 同契約所有賣權商品(PUT)的成交量／所有買權商品(CALL)的成交量。  統計範圍包含所有履約價/月份(含週契約)，也包含夜盤商品。 |

---
## 買賣權未平倉量比率
**Path:** 資料欄位 > 期權

|  |  |
| --- | --- |
| 欄位名稱 | 買賣權未平倉量比率 (資料欄位) |
| 語法 | ``` Value1 = GetField("買賣權未平倉量比率"); Value1 = GetField("CPOIRatio"); ``` |
| 欄位分類 | 期權 |
| 單位 | 比值 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 |
| 支援商品 | 選擇權 |
| 說明 | 同契約所有賣權商品(PUT)的未平倉量／買權商品(CALL)的未平倉量。  統計範圍包含所有履約價/月份(含週契約)。 |

---
## 賣權成交量
**Path:** 資料欄位 > 期權

|  |  |
| --- | --- |
| 欄位名稱 | 賣權成交量 (資料欄位) |
| 語法 | ``` Value1 = GetField("賣權成交量"); Value1 = GetField("PutTotalVolume"); ``` |
| 欄位分類 | 期權 |
| 單位 | 口 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 |
| 支援商品 | 選擇權 |
| 說明 | 開盤到現在的賣權成交量總和。夜盤商品不納入計算。 |

---
## 賣權未平倉量
**Path:** 資料欄位 > 期權

|  |  |
| --- | --- |
| 欄位名稱 | 賣權未平倉量 (資料欄位) |
| 語法 | ``` Value1 = GetField("賣權未平倉量"); Value1 = GetField("PutTotalOI"); ``` |
| 欄位分類 | 期權 |
| 單位 | 口 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 |
| 支援商品 | 選擇權 |
| 說明 | 同契約所有賣權商品(PUT)的未平倉量。  統計範圍包含所有履約價/月份(含週契約)。 |

---
## 隱含波動率
**Path:** 資料欄位 > 期權

|  |  |
| --- | --- |
| 欄位名稱 | 隱含波動率 (資料欄位) |
| 語法 | ``` Value1 = GetField("隱含波動率"); Value1 = GetField("ImpliedVolatility"); ``` |
| 欄位分類 | 期權 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 分鐘 日 週 月 還原日 還原週 還原月 季 半年 年 |
| 支援商品 | 台(權證) 選擇權 |
| 說明 | 選擇權／權證商品的隱含波動率(IV)。 單位是％，例如例如數值如果是23.12％，則回傳23.12。  隱含波動率是以商品在市場上的最新成交價格以Black-Scholes 模型反推出的標的商品波動率。隱含波動率越高就代表標的波動度越高(價格行情震盪越劇烈)。  週頻率以上為統計期間的期底值。 |

---
## CB剩餘張數
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | CB剩餘張數 (資料欄位) |
| 語法 | ``` Value1 = GetField("CB剩餘張數"); Value1 = GetField("CBConversionAmount"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 週 月 季 半年 年 |
| 支援商品 | 台(可轉債) |
| 說明 | 目前流通在外的CB張數（就是還沒有轉換的）季底值，每週更新。 |

---
## 主力成本
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 主力成本 (資料欄位) |
| 語法 | ``` Value1 = GetField("主力成本"); Value1 = GetField("LeaderCost"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 還原日 還原週 還原月 季 半年 年 |
| 支援商品 | 台股 |
| 說明 | 系統估計的長期主力持股成本。  透過每日的主力買賣超以及買進賣出成本，系統推估出主力目前的持股成本。 |

---
## 主力持股
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 主力持股 (資料欄位) |
| 語法 | ``` Value1 = GetField("主力持股"); Value1 = GetField("Leadersharesheld"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 還原日 還原週 還原月 季 半年 年 |
| 支援商品 | 台股 類股指數 |
| 說明 | 系統估計的長期主力持股數量。  透過每日的主力買賣超，系統推估出主力目前的持股總數量。 |

---
## 主力累計買賣超金額
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 主力累計買賣超金額 (資料欄位) |
| 語法 | ``` Value1 = GetField("主力累計買賣超金額"); Value1 = GetField("MBSCumAmount"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 |
| 支援商品 | 大盤 |
| 說明 | 累計的主力買賣超金額。  資料從2023年開始累積。 |

---
## 主力買張
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 主力買張 (資料欄位) |
| 語法 | ``` Value1 = GetField("主力買張"); Value1 = GetField("Leadertotalbuy"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 還原日 還原週 還原月 季 半年 年 |
| 支援商品 | 台股 |
| 說明 | 前十五大買超分點的買超張數加總。  系統每日統計券商分點對個股的買賣記錄，依照買超張數(買進張數 - 賣出張數)排序，找出前15大分點，然後把這15個分點對這檔個股的買超張數加總，計算出主力買張這個數值。  週頻率以上為期間累計值。 |

---
## 主力買賣超張數
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 主力買賣超張數 (資料欄位) |
| 語法 | ``` Value1 = GetField("主力買賣超張數"); Value1 = GetField("LeaderDifference"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 還原日 還原週 還原月 季 半年 年 |
| 支援商品 | 台股 類股指數 |
| 說明 | 主力買張 ─ 主力賣張。  系統每日統計券商分點對個股的買賣記錄，找出買超(買進張數─賣出張數)的前15大分點，加總這15個分點的買超張數，稱之為主力買張。同時也找出賣超(賣出張數─買進張數)的前15大分點，加總這15個分點的賣超張數，稱之為主力賣張。  主力買賣超，就是主力買張 ─ 主力賣張。  類股商品（細產業指標、主題指標）計算方式為成分股加總。  週頻率以上為期間累計值。 |

---
## 主力買進金額
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 主力買進金額 (資料欄位) |
| 語法 | ``` Value1 = GetField("主力買進金額"); Value1 = GetField("MBAmount"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 |
| 支援商品 | 大盤 |
| 說明 | 前十五大買超分點的買超金額合計。  如果是個股的話，數值為前15大主力買進券商當日買超金額(買進金額─賣超金額)的加總。  如果是大盤的話，則依照每個券商分點對所有成分股的買超張數(買進張數─賣超張數)找出前15大分點，然後加總這些分點對所有成分股的買超金額。 |

---
## 主力賣出金額
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 主力賣出金額 (資料欄位) |
| 語法 | ``` Value1 = GetField("主力賣出金額"); Value1 = GetField("MSAmount"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 |
| 支援商品 | 大盤 |
| 說明 | 前十五大賣超分點的賣超金額合計。  如果是個股的話，數值為前15大主力賣出券商當日賣超金額(賣出金額─買進金額)的加總。  如果是大盤的話，則依照每個券商分點對所有成分股的賣超張數(賣出張數─買進張數)找出前15大分點，然後加總這些分點對所有成分股的賣超金額。 |

---
## 主力賣張
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 主力賣張 (資料欄位) |
| 語法 | ``` Value1 = GetField("主力賣張"); Value1 = GetField("Leadertotalsell"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 還原日 還原週 還原月 季 半年 年 |
| 支援商品 | 台股 |
| 說明 | 前十五大賣超分點的賣超張數加總。  系統每日統計券商分點對個股的買賣記錄，依照賣超張數(賣出張數 - 買進張數)排序，找出前15大分點，然後把這15個分點對這檔個股的賣超張數加總，計算出主力賣張這個數值。  週頻率以上為期間累計值。 |

---
## 主動性交易比重
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 主動性交易比重 (資料欄位) |
| 語法 | ``` Value1 = GetField("主動性交易比重"); Value1 = GetField("ActTradeRatio"); ``` |
| 欄位分類 | 籌碼 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 還原日 還原週 還原月 季 半年 年 |
| 支援商品 | 台股 |
| 說明 | 控盤者的主動買盤+主動賣盤佔淨成交張數的比重。  系統每日統計券商分點對個股的買賣記錄，找出成交值大於100萬的買進紀錄，如果這些買進記錄的價格大於昨收價或是開盤價的話，那這些就稱之為控盤者的主動買盤。  同時，系統也會找出成交值大於100萬的賣出紀錄，如果這些賣出記錄的價格小於昨收價或是開盤價的話，那這些就稱之為控盤者的主動賣盤。  主動性交易比重就是主動買盤與主動賣盤的數量加總／(成交量─資券當沖)。  這個數值可以與主動買力或是主動賣力搭配應用。例如主動買力／主動性交易比重就等於是主動買盤佔(主動買盤＋主動賣盤)的比重，可以以此衡量多方控盤者拉抬的力道。 |

---
## 主動買力
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 主動買力 (資料欄位) |
| 語法 | ``` Value1 = GetField("主動買力"); Value1 = GetField("ActLongForce"); ``` |
| 欄位分類 | 籌碼 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 還原日 還原週 還原月 季 半年 年 |
| 支援商品 | 台股 |
| 說明 | 控盤者的主動買盤佔淨成交張數的比重。  系統每日統計券商分點對個股的買賣記錄，找出成交值大於100萬的買進紀錄，如果這些買進記錄的價格大於昨收價或是開盤價的話，那這些就稱之為控盤者的主動買盤。  主動買力就是主動買盤的數量加總／(成交量─資券當沖)。  請注意主動買力的數值是實際比值，並沒有換算成百分比。 |

---
## 主動賣力
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 主動賣力 (資料欄位) |
| 語法 | ``` Value1 = GetField("主動賣力"); Value1 = GetField("ActShortForce"); ``` |
| 欄位分類 | 籌碼 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 還原日 還原週 還原月 季 半年 年 |
| 支援商品 | 台股 |
| 說明 | 控盤者的主動賣盤佔淨成交張數的比重。  系統每日統計券商分點對個股的買賣記錄，找出成交值大於100萬的賣出紀錄，如果這些賣出記錄的價格小於昨收價或是開盤價的話，那這些就稱之為控盤者的主動賣盤。  主動賣力就是主動賣盤的數量加總／(成交量─資券當沖)。  請注意主動賣力的數值是實際比值，並沒有換算成百分比。 |

---
## 借券張數
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 借券張數 (資料欄位) |
| 語法 | ``` Value1 = GetField("借券張數"); Value1 = GetField("SBLBorrowing"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 還原日 還原週 還原月 季 半年 年 |
| 支援商品 | 台股 大盤 |
| 說明 | 借券的張數。  週頻率以上為期間累計值。  借券數量係指當日投資人透過證交所借券中心或向證券商、證金公司借入之證券數量。這個數量並不一定等於借券放空的數量。 |

---
## 借券賣出張數
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 借券賣出張數 (資料欄位) |
| 語法 | ``` Value1 = GetField("借券賣出張數"); Value1 = GetField("SBUnits"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 還原日 還原週 還原月 季 半年 年 |
| 支援商品 | 台股 大盤 |
| 說明 | 借券內實際賣出(放空)的張數。  週頻率以上為期間累計值。 |

---
## 借券賣出還券張數
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 借券賣出還券張數 (資料欄位) |
| 語法 | ``` Value1 = GetField("借券賣出還券張數"); Value1 = GetField("SBStcokDiff"); Value1 = GetField("借券賣出庫存異動張數"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 還原日 還原週 還原月 季 半年 年 |
| 支援商品 | 台股 大盤 |
| 說明 | 償還先前借券賣出的張數。  週頻率以上為期間累計值。  請注意：這個欄位使用負值來代表償還的動作，所以如果要計算當日借券賣出餘額的異動的話，請使用借券賣出張數＋借券賣出還券張數。 |

---
## 借券賣出餘額張數
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 借券賣出餘額張數 (資料欄位) |
| 語法 | ``` Value1 = GetField("借券賣出餘額張數"); Value1 = GetField("SBRemain"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 還原日 還原週 還原月 季 半年 年 |
| 支援商品 | 台股 大盤 |
| 說明 | 借券餘額張數，也就是到目前為止借券的淨張數。  借券賣出餘額約等於前一日的借券賣出餘額＋當日的借券賣出張數＋當日的借券賣出還券張數。  請注意：上面公式內會加上「借券賣出還券張數」的原因是因為這欄位是負值。  週頻率以上為統計期間的期底值。 |

---
## 借券餘額張數
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 借券餘額張數 (資料欄位) |
| 語法 | ``` Value1 = GetField("借券餘額張數"); Value1 = GetField("SBLbalance"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 還原日 還原週 還原月 季 半年 年 |
| 支援商品 | 台股 大盤 |
| 說明 | 借券餘額張數，也就是到目前為止借券的淨張數。  借券餘額約等於前一日的借券餘額＋當日的借券張數─當日的還券張數。  借券餘額的增加，代表著有越來越多人準備放空。可是是否有實際放空行為，必須觀察「借券賣出餘額」欄位的變化。  週頻率以上為統計期間的期底值。 |

---
## 內部人持股
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 內部人持股 (資料欄位) |
| 語法 | ``` Value1 = GetField("內部人持股"); Value1 = GetField("InsiderHodings"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 股 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 |
| 支援商品 | 美(股票) |
| 說明 | 依美國證券交易委員會規定公告的「FORM 4 」所計算出的持股異動內部人的總持有股數。 美股股票才適用本欄位。  依照SEC的規定，公司內部人(包括董事、經理人與持股超過10%的股東)買賣公司股票後必須在2日內申報。  請注意：   1. 此欄位的日期是實際股票買賣的日期，可是因為申報時間，可能會延後公佈。 2. 由於內部人可能透過其他方式取得持股，例如Warrant的轉換等，所以這個欄位的數值未必會等於前一期的內部人持股加上當期的內部人的持股異動。 |

---
## 內部人持股張數
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 內部人持股張數 (資料欄位) |
| 語法 | ``` Value1 = GetField("內部人持股張數"); Value1 = GetField("Insidersharesheld"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 月 |
| 支援商品 | 台股 |
| 說明 | 公司內部人持股張數。  內部人統計的範圍包含董監以及公司內部的經理人。  資料每月更新一次。 |

---
## 內部人持股比例
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 內部人持股比例 (資料欄位) |
| 語法 | ``` Value1 = GetField("內部人持股比例"); Value1 = GetField("Insidersharesheldratio"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 月 |
| 支援商品 | 台股 |
| 說明 | 公司內部人持股占公司股本的比例。  計算公式為內部人持股張數／公司股本＊100%。  內部人統計的範圍包含董監以及公司內部的經理人。  資料每月更新一次。 |

---
## 內部人持股異動
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 內部人持股異動 (資料欄位) |
| 語法 | ``` Value1 = GetField("內部人持股異動"); Value1 = GetField("InsiderHodingsDiff"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 股 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 |
| 支援商品 | 美(股票) |
| 說明 | 依美國證券交易委員會規定公告的「FORM 4 」所計算出的公司內部人總異動股數。正值表示買入總股數，負值表示賣出總股數。  如果是週／月頻率的話，則是當週／當月內每一日內部人總異動股數的加總。  美股股票才適用本欄位。  依照SEC的規定，公司內部人(包括董事、經理人與持股超過10%的股東)買賣公司股票後必須在2日內申報。請注意此欄位的日期是實際股票買賣的日期，可是因為申報時間，可能會延後公佈。 |

---
## 分公司交易家數
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 分公司交易家數 (資料欄位) |
| 語法 | ``` Value1 = GetField("分公司交易家數"); Value1 = GetField("BranchesOfTrading"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 家 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 |
| 支援商品 | 台股 類股指數 |
| 說明 | 統計有買進或是賣出的券商分點家數。  類股商品（細產業指標、主題指標）計算方式為統計成份股內有買進或是賣出的分點的集合家數。例如成份股#1的交易分點為a, b, 成份股#2的交易分點為b, c, 則類股商品的交易家數則為3。 |

---
## 分公司淨買超金額家數
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 分公司淨買超金額家數 (資料欄位) |
| 語法 | ``` Value1 = GetField("分公司淨買超金額家數"); Value1 = GetField("BranchesOfNetBought"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 家 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 |
| 支援商品 | 台股 類股指數 |
| 說明 | 統計買超的券商分點家數。  買超的定義是買進金額 - 賣出金額 > 0。  類股商品（細產業指標、主題指標）計算方式為統計成份股內符合買超的分點的集合家數。例如成份股#1的買超分點為a, b, 成份股#2的買超分點為b, c, 則類股商品的買超家數則為3。 |

---
## 分公司淨賣超金額家數
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 分公司淨賣超金額家數 (資料欄位) |
| 語法 | ``` Value1 = GetField("分公司淨賣超金額家數"); Value1 = GetField("BranchesOfNetSold"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 家 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 |
| 支援商品 | 台股 類股指數 |
| 說明 | 統計賣超的券商分點家數。  賣超的定義是賣出金額 - 買進金額 > 0。  類股商品（細產業指標、主題指標）計算方式為統計成份股內符合賣超的分點的集合家數。例如成份股#1的賣超分點為a, b, 成份股#2的賣超分點為b, c, 則類股商品的賣超家數則為3。 |

---
## 分公司買進家數
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 分公司買進家數 (資料欄位) |
| 語法 | ``` Value1 = GetField("分公司買進家數"); Value1 = GetField("LongBranches"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 家 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 |
| 支援商品 | 台股 |
| 說明 | 統計買進金額大於零的券商分點家數。  請注意：只要有買進的券商分點就會納入統計，與該分點是否買超無關。 |

---
## 分公司賣出家數
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 分公司賣出家數 (資料欄位) |
| 語法 | ``` Value1 = GetField("分公司賣出家數"); Value1 = GetField("ShortBranches"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 家 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 |
| 支援商品 | 台股 |
| 說明 | 統計賣出金額大於零的券商分點家數。  請注意：只要有賣出的券商分點就會納入統計，與該分點是否賣超無關。 |

---
## 券資比
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 券資比 (資料欄位) |
| 語法 | ``` Value1 = GetField("券資比"); Value1 = GetField("ShortSale／Margin"); Value1 = GetField("MSRatio"); Value1 = GetField("資券比"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 還原日 還原週 還原月 季 半年 年 |
| 支援商品 | 台股 大盤 類股指數 |
| 說明 | 融券餘額與融資餘額的比例。計算公式：融券餘額張數 / 融資餘額張數 \* 100%。  單位是％，例如券資比為10%的話，則回傳10。  類股商品（細產業指標、主題指標）的融券餘額張數以及融資餘額張數均為成分股餘額張數的加總。 大盤商品的融券餘額張數／融資餘額張數為交易所公布值。  週頻率以上為統計期間的期底值。 |

---
## 吉尼係數
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 吉尼係數 (資料欄位) |
| 語法 | ``` Value1 = GetField("吉尼係數"); Value1 = GetField("GiniCoefficient"); ``` |
| 欄位分類 | 籌碼 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 |
| 支援商品 | 台股 |
| 說明 | 使用券商分點交易比重所推估出來的吉尼系統。  數值介於０與１之間，愈接近0表示集中度越低（也就是越平均），愈接近１則表示集中度越高（也就是越不平均）。  系統將分點對個股的進出數值，統計成兩個數列，縱軸是「券商家數累積百分比」，橫軸是「累加券商買賣金額佔所有券商買賣金額百分比」，然後作出羅倫茲曲線( Lorenz Curve )後計算出吉尼係數的數值。  當吉尼係數越大時，表示買賣金額集中在少數券商，而吉尼係數越小的話，則表示買賣金額平均在許多分點上。 |

---
## 地緣券商買賣超張數
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 地緣券商買賣超張數 (資料欄位) |
| 語法 | ``` Value1 = GetField("地緣券商買賣超張數"); Value1 = GetField("GBDifference"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 |
| 支援商品 | 台股 類股指數 |
| 說明 | 「地緣券商」的買進張數 － 賣出張數。  地緣券商為公司地址附近的分點券商(不包含外資券商)。  預設是公司地址周圍三公里內的券商，如果分點過多，則改為1公里內，如果找不到分點的話則擴充到10公里以內。  類股商品（細產業指標、主題指標）計算方式為成分股加總。  週頻率以上為期間累計值。 |

---
## 外資成本
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 外資成本 (資料欄位) |
| 語法 | ``` Value1 = GetField("外資成本"); Value1 = GetField("FBSCost"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 還原日 還原週 還原月 季 半年 年 |
| 支援商品 | 台股 |
| 說明 | 系統估算的外資持股成本。  計算方式是透過每日外資買張，賣張，以及當日的價格，估算出外資的長期持有成本。  週頻率以上為統計期間的期底值。 |

---
## 外資持股
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 外資持股 (資料欄位) |
| 語法 | ``` Value1 = GetField("外資持股"); Value1 = GetField("Fsharesheld"); Value1 = GetField("外資持股張數"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 還原日 還原週 還原月 季 半年 年 |
| 支援商品 | 台股 類股指數 |
| 說明 | 外資持有的張數。  類股商品（細產業指標、主題指標）的數值則是成分股的外資持有張數的加總。  請注意：外資持股的數值由交易所每日公布。影響外資持股因素，除集中市場外資淨買賣數外，另有非集中市場交易因素例如：海外存託憑證異動股數、海外可轉換公司債轉換股數、上市公司增減資或股東會、除權申報或ETF申購買回、投資人變更國籍等，所以未必會等於外資買賣超的累計數值。  週頻率以上為統計期間的期底值。 |

---
## 外資持股比例
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 外資持股比例 (資料欄位) |
| 語法 | ``` Value1 = GetField("外資持股比例"); Value1 = GetField("Fsharesheldratio"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 還原日 還原週 還原月 季 半年 年 |
| 支援商品 | 台股 |
| 說明 | 外資持股的比例。 單位為%，例如：持股比例若為20.5%，則回傳20.5。  請注意：持股比例的數值由交易所直接公布。  週頻率以上為統計期間的期底值。 |

---
## 外資買張
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 外資買張 (資料欄位) |
| 語法 | ``` Value1 = GetField("外資買張"); Value1 = GetField("Ftotalbuy"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 還原日 還原週 還原月 季 半年 年 |
| 支援商品 | 台股 |
| 說明 | 外資買進張數。  週頻率以上為期間累計值。 |

---
## 外資買賣超
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 外資買賣超 (資料欄位) |
| 語法 | ``` Value1 = GetField("外資買賣超"); Value1 = GetField("Fdifference"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 台股,類股: 張. 大盤: 元 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 還原日 還原週 還原月 季 半年 年 |
| 支援商品 | 台股 大盤 類股指數 |
| 說明 | 外資買賣超。數值內容/單位依商品類型而異。  台股商品的數值為外資買張 ─ 外資賣張。 類股商品（細產業指標、主題指標）的數值則是成分股的外資買賣超的加總。 大盤商品的數值則是外資買進金額 ─ 外資賣出金額，單位是元。  週頻率以上為期間累計值。 |

---
## 外資買賣超張數
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 外資買賣超張數 (資料欄位) |
| 語法 | ``` Value1 = GetField("外資買賣超張數"); Value1 = GetField("FdifferenceonStock"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 還原日 還原週 還原月 季 半年 年 |
| 支援商品 | 台股 類股指數 |
| 說明 | 外資買賣超張數。  台股商品的數值為外資買張 ─ 外資賣張。 類股商品（細產業指標、主題指標）的數值則是成分股的外資買賣超的加總。  週頻率以上為期間累計值。 |

---
## 外資買賣超金額
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 外資買賣超金額 (資料欄位) |
| 語法 | ``` Value1 = GetField("外資買賣超金額"); Value1 = GetField("FBSAmount"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 季 半年 年 |
| 支援商品 | 大盤 |
| 說明 | 外資買賣超金額，計算公式為外資買進金額 ─ 外資賣出金額。  週頻率以上為期間累計值。 |

---
## 外資買進金額
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 外資買進金額 (資料欄位) |
| 語法 | ``` Value1 = GetField("外資買進金額"); Value1 = GetField("FBAmount"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 季 半年 年 |
| 支援商品 | 大盤 |
| 說明 | 外資買進金額。  週頻率以上為期間累計值。 |

---
## 外資賣出金額
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 外資賣出金額 (資料欄位) |
| 語法 | ``` Value1 = GetField("外資賣出金額"); Value1 = GetField("FSAmount"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 季 半年 年 |
| 支援商品 | 大盤 |
| 說明 | 外資賣出金額。  週頻率以上為期間累計值。 |

---
## 外資賣張
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 外資賣張 (資料欄位) |
| 語法 | ``` Value1 = GetField("外資賣張"); Value1 = GetField("Ftotalsell"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 還原日 還原週 還原月 季 半年 年 |
| 支援商品 | 台股 |
| 說明 | 外資賣出張數。  週頻率以上為期間累計值。 |

---
## 大戶持股人數
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 大戶持股人數 (資料欄位) |
| 語法 | ``` Value1 = GetField("大戶持股人數",param:=400); Value1 = GetField("Bigsharesheldpeople",param:=400); ``` |
| 參數範圍 | 1、5、10、15、20、30、40、50、100、200、400、600、800、1000 |
| 欄位分類 | 籌碼 |
| 單位 | 人 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 週 月 季 半年 年 |
| 支援商品 | 台股 類股指數 |
| 說明 | 由集保公司所提供的指定級距以上的持股人數資料。 每週更新一次。  集保公司每週末會公佈當週每檔股票不同持股級距的持股人數，張數等資料。  目前公佈的級距包含1張以下，1張～5張，5張～10張，10張～15張，15張～20張，20張～30張，30張～40張，40張～50張，50張～100張，100張～200張，200張～400張，400張～600張，600張～800張，800張～1000張，以及1000張以上，其中每張為1000股。  大戶持股人數，則是統計某個級距以上的持股人數的加總，單位為人。例如400張大戶持股人數為400～600張，600～800張，800～1000張，以及1000張以上這幾個級距的持股人數的加總。  語法上可以使用： value1 = GetField("大戶持股人數", param:=400); 來取得400張以上的大戶持股人數。  指數型商品(例如類股指數，細產業指標、主題指標)亦提供此欄位，計算方式則是依照成份股的數值進行加總。 |

---
## 大戶持股張數
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 大戶持股張數 (資料欄位) |
| 語法 | ``` Value1 = GetField("大戶持股張數",param:=400); Value1 = GetField("Bigsharesheld",param:=400); ``` |
| 參數範圍 | 1、5、10、15、20、30、40、50、100、200、400、600、800、1000 |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 週 月 季 半年 年 |
| 支援商品 | 台股 類股指數 |
| 說明 | 由集保公司所提供的指定級距以上的持股張數資料。 每週更新一次。  集保公司每週末會公佈當週每檔股票不同持股級距的持股人數，張數等資料。  目前公佈的級距包含1張以下，1張～5張，5張～10張，10張～15張，15張～20張，20張～30張，30張～40張，40張～50張，50張～100張，100張～200張，200張～400張，400張～600張，600張～800張，800張～1000張，以及1000張以上，其中每張為1000股。  大戶持股張數，則是統計某個級距以上的持股張數的加總，單位為張。例如400張大戶持股張數為400～600張，600～800張，800～1000張，以及1000張以上這幾個級距的持股張數的加總。  語法上可以使用： value1 = GetField("大戶持股張數", param:=400); 來取得400張以上的大戶持股張數。  指數型商品(例如類股指數，細產業指標、主題指標)亦提供此欄位，計算方式則是依照成份股的數值進行加總。 |

---
## 大戶持股比例
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 大戶持股比例 (資料欄位) |
| 語法 | ``` Value1 = GetField("大戶持股比例",param:=400); Value1 = GetField("Bigsharesheldratio",param:=400); ``` |
| 參數範圍 | 1、5、10、15、20、30、40、50、100、200、400、600、800、1000 |
| 欄位分類 | 籌碼 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 週 月 季 半年 年 |
| 支援商品 | 台股 類股指數 |
| 說明 | 由集保公司所提供的指定級距以上的持股比例資料。 每週更新一次。  集保公司每週末會公佈當週每檔股票不同持股級距的持股人數，張數等資料。  目前公佈的級距包含1張以下，1張～5張，5張～10張，10張～15張，15張～20張，20張～30張，30張～40張，40張～50張，50張～100張，100張～200張，200張～400張，400張～600張，600張～800張，800張～1000張，以及1000張以上，其中每張為1000股。  大戶持股比例，則是統計某個級距以上的持股人數/總持股人數的比例，單位為百分比。例如400張大戶持股比例為400～600張，600～800張，800～1000張，以及1000張以上這幾個級距的持股人數加總/總持股人數的比例。  語法上可以使用： value1 = GetField("大戶持股比例", param:=400); 來取得400張以上的大戶持股比例。  指數型商品(例如類股指數，細產業指標、主題指標)亦提供此欄位，計算方式則是依照成份股的數值進行加總。 |

---
## 官股券商累計買賣超金額
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 官股券商累計買賣超金額 (資料欄位) |
| 語法 | ``` Value1 = GetField("官股券商累計買賣超金額"); Value1 = GetField("OBBSCumAmount"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 |
| 支援商品 | 大盤 |
| 說明 | 「官股券商」每日買賣超金額的累積值。  資料起點為2013/10。 |

---
## 官股券商買賣超張數
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 官股券商買賣超張數 (資料欄位) |
| 語法 | ``` Value1 = GetField("官股券商買賣超張數"); Value1 = GetField("OBDifference"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 |
| 支援商品 | 台股 類股指數 |
| 說明 | 「官股券商」買進張數 - 賣出張數。  官股券商名單為: 臺銀，土銀，合庫，兆豐，第一金，華南永昌，彰銀以及台企銀。  類股商品（細產業指標、主題指標）計算方式為成分股加總。  週頻率以上為期間累計值。  請注意：因為統計時是以股為單位，所以欄位數值可能會包含小數(未滿1張時)。 |

---
## 官股券商買進金額
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 官股券商買進金額 (資料欄位) |
| 語法 | ``` Value1 = GetField("官股券商買進金額"); Value1 = GetField("OBBAmount"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 |
| 支援商品 | 台股 大盤 |
| 說明 | 「官股券商」對市場所有股票的合計買進金額。  官股券商名單為: 臺銀，土銀，合庫，兆豐，第一金，華南永昌，彰銀以及台企銀。  週頻率以上為期間累計值。 |

---
## 官股券商賣出金額
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 官股券商賣出金額 (資料欄位) |
| 語法 | ``` Value1 = GetField("官股券商賣出金額"); Value1 = GetField("OBSAmount"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 |
| 支援商品 | 台股 大盤 |
| 說明 | 「官股券商」對市場所有股票的合計賣出金額。  官股券商名單為: 臺銀，土銀，合庫，兆豐，第一金，華南永昌，彰銀以及台企銀。  週頻率以上為期間累計值。 |

---
## 實戶買張
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 實戶買張 (資料欄位) |
| 語法 | ``` Value1 = GetField("實戶買張"); Value1 = GetField("Operatortotalbuy"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 還原日 還原週 還原月 季 半年 年 |
| 支援商品 | 台股 |
| 說明 | 單一價格成交值介於50萬到100萬之間的合計買進張數。  系統每日統計券商分點對個股的買賣記錄，把成交值介於50萬到100萬之間的買進張數加總起來，稱之為實戶買張。  請注意：交易所提供分點統計資料內，同一價格的買進成交會自動合併成一筆資料。所以成交值是依照合併後的資料來計算。  週頻率以上為期間累計值。 |

---
## 實戶買賣超張數
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 實戶買賣超張數 (資料欄位) |
| 語法 | ``` Value1 = GetField("實戶買賣超張數"); Value1 = GetField("Operatordifference"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 還原日 還原週 還原月 季 半年 年 |
| 支援商品 | 台股 |
| 說明 | 實戶買張 ─ 實戶賣張。  實戶買張是單一價格成交值介於50萬到100萬之間的合計買進張數，而實戶賣張則是同樣級距的合計賣出張數。  週頻率以上為期間累計值。 |

---
## 實戶賣張
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 實戶賣張 (資料欄位) |
| 語法 | ``` Value1 = GetField("實戶賣張"); Value1 = GetField("OperatortotalSell"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 還原日 還原週 還原月 季 半年 年 |
| 支援商品 | 台股 |
| 說明 | 單一價格成交值介於50萬到100萬之間的合計賣出張數。  系統每日統計券商分點對個股的買賣記錄，把成交值介於50萬到100萬之間的賣出張數加總起來，稱之為實戶賣張。  請注意：交易所提供分點統計資料內，同一價格的賣出成交會自動合併成一筆資料。所以成交值是依照合併後的資料來計算。  週頻率以上為期間累計值。 |

---
## 實質買盤比
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 實質買盤比 (資料欄位) |
| 語法 | ``` Value1 = GetField("實質買盤比"); Value1 = GetField("RealLongRatio"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 還原日 還原週 還原月 季 半年 年 |
| 支援商品 | 台股 大盤 類股指數 |
| 說明 | 實質買進張數佔成交量的比例。  計算公式為：(成交量－融資買進張數－資券相抵張數－融券回補張數)／成交量 \* 100％。  指數類型商品則是統計所有成分股的資料加總。 |

---
## 實質賣盤比
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 實質賣盤比 (資料欄位) |
| 語法 | ``` Value1 = GetField("實質賣盤比"); Value1 = GetField("RealShortRatio"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 還原日 還原週 還原月 季 半年 年 |
| 支援商品 | 台股 大盤 類股指數 |
| 說明 | 實質賣出張數佔成交量的比例。  計算公式為：(成交量－融券賣出張數－資券相抵張數－融資賣出張數)／成交量 \* 100％。  指數類型商品則是統計所有成分股的資料加總。 |

---
## 市場總分點數
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 市場總分點數 (資料欄位) |
| 語法 | ``` Value1 = GetField("市場總分點數"); Value1 = GetField("TotalBranches"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 家 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 |
| 支援商品 | 台股 |
| 說明 | 市場所有的券商分點家數。  包含所有有經營經紀業務的券商分點以及總公司。  這個數值是系統統計交易「分公司」相關欄位(例如分公司交易家數，分公司買進家數，分公司賣出家數等)的母體總數量。 |

---
## 庫藏股實際買回張數
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 庫藏股實際買回張數 (資料欄位) |
| 語法 | ``` Value1 = GetField("庫藏股實際買回張數"); Value1 = GetField("Stockbuyvalue"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 單一 |
| 支援商品 | 台股 |
| 說明 | 最新一次庫藏股公告的實際買回張數。  使用時可以搭配庫藏股開始日期，庫藏股結束日期，以及庫藏股預計買回張數等欄位。 |

---
## 庫藏股申請家數
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 庫藏股申請家數 (資料欄位) |
| 語法 | ``` Value1 = GetField("庫藏股申請家數"); Value1 = GetField("BranchesStock"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 家 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 還原日 還原週 還原月 季 半年 年 |
| 支援商品 | 大盤 類股指數 |
| 說明 | 市場目前正在實施庫藏股的家數。  類股商品（細產業指標、主題指標）計算方式為成分股加總。  週頻率以上為期間累計值。 |

---
## 庫藏股申請總市值
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 庫藏股申請總市值 (資料欄位) |
| 語法 | ``` Value1 = GetField("庫藏股申請總市值"); Value1 = GetField("StockTotalValue"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 千元 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 還原日 還原週 還原月 季 半年 年 |
| 支援商品 | 大盤 |
| 說明 | 市場目前正在實施庫藏股的庫藏股市值的加總。  庫藏股市值的計算公式是用預計買回張數＊買回日期第一日的收盤價。 |

---
## 庫藏股預計買回張數
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 庫藏股預計買回張數 (資料欄位) |
| 語法 | ``` Value1 = GetField("庫藏股預計買回張數"); Value1 = GetField("Stockestivalue"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 單一 |
| 支援商品 | 台股 |
| 說明 | 最新一次庫藏股公告的預計買回張數。  使用時可以搭配庫藏股開始日期以及庫藏股結束日期來確認此欄位的有效期間。 |

---
## 投信成本
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 投信成本 (資料欄位) |
| 語法 | ``` Value1 = GetField("投信成本"); Value1 = GetField("SBSCost"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 還原日 還原週 還原月 季 半年 年 |
| 支援商品 | 台股 |
| 說明 | 系統估算的投信持股成本。  計算方式是透過每日投信買張，賣張，以及當日的價格，估算出投信的長期持有成本。  週頻率以上為統計期間的期底值。 |

---
## 投信持股
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 投信持股 (資料欄位) |
| 語法 | ``` Value1 = GetField("投信持股"); Value1 = GetField("Ssharesheld"); Value1 = GetField("投信持股張數"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 還原日 還原週 還原月 季 半年 年 |
| 支援商品 | 台股 類股指數 |
| 說明 | 系統估計值，計算方式是累計長期的投信買賣超張數。  類股商品（細產業指標、主題指標）的數值則是成分股的投信持股的加總。  請注意：投信估計持股，除了依每日買賣超計算外，會利用投信投顧公會公告的基金季持股明細(或半年持股明細，視基金類型)進行個股投信的庫存數值調整（季末的持股值可能會與前一日會有較大的落差）。  週頻率以上為統計期間的期底值。 |

---
## 投信持股比例
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 投信持股比例 (資料欄位) |
| 語法 | ``` Value1 = GetField("投信持股比例"); Value1 = GetField("Ssharesheldratio"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 還原日 還原週 還原月 季 半年 |
| 支援商品 | 台股 |
| 說明 | 系統估算值，計算公式為：投信持股/流通在外張數 \* 100%。  單位為%，例如：持股比例若為20.5%，則回傳20.5。  週頻率以上為統計期間的期底值。 |

---
## 投信買張
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 投信買張 (資料欄位) |
| 語法 | ``` Value1 = GetField("投信買張"); Value1 = GetField("Stotalbuy"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 還原日 還原週 還原月 季 半年 年 |
| 支援商品 | 台股 |
| 說明 | 投信買進張數。  週頻率以上為期間累計值。 |

---
## 投信買賣超
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 投信買賣超 (資料欄位) |
| 語法 | ``` Value1 = GetField("投信買賣超"); Value1 = GetField("Sdifference"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 台股,類股: 張. 大盤: 元 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 還原日 還原週 還原月 季 半年 年 |
| 支援商品 | 台股 大盤 類股指數 |
| 說明 | 投信買賣超。數值內容/單位依商品類型而異。  台股商品的數值為投信買張 ─ 投信賣張。 類股商品（細產業指標、主題指標）的數值則是成分股的投信買賣超的加總。 大盤商品的數值則是投信買進金額 ─ 投信賣出金額，單位是元。  週頻率以上為期間累計值。 |

---
## 投信買賣超張數
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 投信買賣超張數 (資料欄位) |
| 語法 | ``` Value1 = GetField("投信買賣超張數"); Value1 = GetField("SdifferenceonStock"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 還原日 還原週 還原月 季 半年 年 |
| 支援商品 | 台股 類股指數 |
| 說明 | 投信買賣超張數。  台股商品的數值為投信買張 ─ 投信賣張。 類股商品（細產業指標、主題指標）的數值則是成分股的投信買賣超的加總。  週頻率以上為期間累計值。 |

---
## 投信買賣超金額
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 投信買賣超金額 (資料欄位) |
| 語法 | ``` Value1 = GetField("投信買賣超金額"); Value1 = GetField("SBSAmount"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 季 半年 年 |
| 支援商品 | 大盤 |
| 說明 | 投信買賣超金額，計算公式為投信買進金額 ─ 投信賣出金額。  週頻率以上為期間累計值。 |

---
## 投信買進金額
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 投信買進金額 (資料欄位) |
| 語法 | ``` Value1 = GetField("投信買進金額"); Value1 = GetField("SBAmount"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 季 半年 年 |
| 支援商品 | 大盤 |
| 說明 | 投信買進金額。  週頻率以上為期間累計值。 |

---
## 投信賣出金額
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 投信賣出金額 (資料欄位) |
| 語法 | ``` Value1 = GetField("投信賣出金額"); Value1 = GetField("SSAmount"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 季 半年 年 |
| 支援商品 | 大盤 |
| 說明 | 投信賣出金額。  週頻率以上為期間累計值。 |

---
## 投信賣張
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 投信賣張 (資料欄位) |
| 語法 | ``` Value1 = GetField("投信賣張"); Value1 = GetField("Stotalsell"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 還原日 還原週 還原月 季 半年 年 |
| 支援商品 | 台股 |
| 說明 | 投信賣出張數。  週頻率以上為期間累計值。 |

---
## 控盤者成本線
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 控盤者成本線 (資料欄位) |
| 語法 | ``` Value1 = GetField("控盤者成本線"); Value1 = GetField("CostLine"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 還原日 還原週 還原月 季 半年 年 |
| 支援商品 | 台股 |
| 說明 | 系統估計的控盤者的持股成本。  控盤者的資料是每日單一價格成交值大於100萬的買賣記錄。  系統依此資料，估計出控盤者的長期持有成本。 |

---
## 控盤者買張
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 控盤者買張 (資料欄位) |
| 語法 | ``` Value1 = GetField("控盤者買張"); Value1 = GetField("Controllertotalbuy"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 還原日 還原週 還原月 季 半年 年 |
| 支援商品 | 台股 |
| 說明 | 單一價格成交值大於100萬的合計買進張數。  系統每日統計券商分點對個股的買賣記錄，把成交值大於100萬的買進張數加總起來，稱之為控盤者買張。  請注意：交易所提供分點統計資料內，同一價格的買進成交會自動合併成一筆資料。所以成交值是依照合併後的資料來計算。  週頻率以上為期間累計值。 |

---
## 控盤者買賣超張數
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 控盤者買賣超張數 (資料欄位) |
| 語法 | ``` Value1 = GetField("控盤者買賣超張數"); Value1 = GetField("Controllerdifference"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 還原日 還原週 還原月 季 半年 年 |
| 支援商品 | 台股 |
| 說明 | 控盤者買張 ─ 控盤者賣張。  控盤者買張是單一價格成交值大於100萬之間的合計買進張數，而控盤者賣張則是同樣級距的合計賣出張數。  週頻率以上為期間累計值。 |

---
## 控盤者賣張
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 控盤者賣張 (資料欄位) |
| 語法 | ``` Value1 = GetField("控盤者賣張"); Value1 = GetField("Controllertotalsell"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 還原日 還原週 還原月 季 半年 年 |
| 支援商品 | 台股 |
| 說明 | 單一價格成交值大於100萬的合計賣出張數。  系統每日統計券商分點對個股的買賣記錄，把成交值大於100萬的賣出張數加總起來，稱之為控盤者賣張。  請注意：交易所提供分點統計資料內，同一價格的賣出成交會自動合併成一筆資料。所以成交值是依照合併後的資料來計算。  週頻率以上為期間累計值。 |

---
## 收集派發指標
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 收集派發指標 (資料欄位) |
| 語法 | ``` Value1 = GetField("收集派發指標"); Value1 = GetField("ADIndicator"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 家 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 還原日 還原週 還原月 季 半年 年 |
| 支援商品 | 台股 |
| 說明 | 分公司賣出家數 － 分公司買進家數。  當賣出家數大於買進家數時(數值>0)，表示籌碼由多數人流向少數人手中，籌碼趨於集中，反之則表示籌碼趨於發散。  週頻率以上為期間累計值。 |

---
## 散戶持股人數
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 散戶持股人數 (資料欄位) |
| 語法 | ``` Value1 = GetField("散戶持股人數",param:=400); Value1 = GetField("Retailsharesheldpeople",param:=400); ``` |
| 參數範圍 | 1、5、10、15、20、30、40、50、100、200、400、600、800、1000 |
| 欄位分類 | 籌碼 |
| 單位 | 人 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 週 月 季 半年 年 |
| 支援商品 | 台股 類股指數 |
| 說明 | 由集保公司所提供的指定級距以下的持股比例資料。 每週更新一次。  集保公司每週末會公佈當週每檔股票不同持股級距的持股人數，張數等資料。  目前公佈的級距包含1張以下，1張～5張，5張～10張，10張～15張，15張～20張，20張～30張，30張～40張，40張～50張，50張～100張，100張～200張，200張～400張，400張～600張，600張～800張，800張～1000張，以及1000張以上，其中每張為1000股。  散戶持股人數，則是統計某個級距以下的持股人數的加總，單位為人。例如10張持股人數為1張以下，1張～5張，以及5張～10張這幾個級距的持股人數的加總。  語法上可以使用： value1 = GetField("散戶持股人數", param:=10); 來取得10張以下的散戶持股人數。  指數型商品(例如類股指數，細產業指標、主題指標)亦提供此欄位，計算方式則是依照成份股的數值進行加總。 |

---
## 散戶持股張數
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 散戶持股張數 (資料欄位) |
| 語法 | ``` Value1 = GetField("散戶持股張數",param:=400); Value1 = GetField("Retailsharesheld",param:=400); ``` |
| 參數範圍 | 1、5、10、15、20、30、40、50、100、200、400、600、800、1000 |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 週 月 季 半年 年 |
| 支援商品 | 台股 類股指數 |
| 說明 | 由集保公司所提供的指定級距以下的持股比例資料。 每週更新一次。  集保公司每週末會公佈當週每檔股票不同持股級距的持股人數，張數等資料。  目前公佈的級距包含1張以下，1張～5張，5張～10張，10張～15張，15張～20張，20張～30張，30張～40張，40張～50張，50張～100張，100張～200張，200張～400張，400張～600張，600張～800張，800張～1000張，以及1000張以上，其中每張為1000股。  散戶持股張數，則是統計某個級距以下的持股張數的加總，單位為張。例如10張持股張數為1張以下，1張～5張，以及5張～10張這幾個級距的持股張數的加總。  語法上可以使用： value1 = GetField("散戶持股張數", param:=10); 來取得10張以下的散戶持股張數。  指數型商品(例如類股指數，細產業指標、主題指標)亦提供此欄位，計算方式則是依照成份股的數值進行加總。 |

---
## 散戶持股比例
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 散戶持股比例 (資料欄位) |
| 語法 | ``` Value1 = GetField("散戶持股比例",param:=400); Value1 = GetField("Retailsharesheldratio",param:=400); ``` |
| 參數範圍 | 1、5、10、15、20、30、40、50、100、200、400、600、800、1000 |
| 欄位分類 | 籌碼 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 週 月 季 半年 年 |
| 支援商品 | 台股 類股指數 |
| 說明 | 由集保公司所提供的指定級距以下的持股比例資料。 每週更新一次。  集保公司每週末會公佈當週每檔股票不同持股級距的持股人數，張數等資料。  目前公佈的級距包含1張以下，1張～5張，5張～10張，10張～15張，15張～20張，20張～30張，30張～40張，40張～50張，50張～100張，100張～200張，200張～400張，400張～600張，600張～800張，800張～1000張，以及1000張以上，其中每張為1000股。  散戶持股比例，則是統計某個級距以下的持股人數/總持股人數的比例，單位為百分比。例如10張持股比例為1張以下，1張～5張，以及5張～10張這幾個級距的持股人數加總/總持股人數的比例。  語法上可以使用： value1 = GetField("散戶持股比例", param:=10); 來取得10張以下的散戶持股比例。  指數型商品(例如類股指數，細產業指標、主題指標)亦提供此欄位，計算方式則是依照成份股的數值進行加總。 |

---
## 散戶買張
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 散戶買張 (資料欄位) |
| 語法 | ``` Value1 = GetField("散戶買張"); Value1 = GetField("Retailtotalbuy"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 還原日 還原週 還原月 季 半年 年 |
| 支援商品 | 台股 |
| 說明 | 單一價格成交值小於50萬的合計買進張數。  系統每日統計券商分點對個股的買賣記錄，把成交值小於50萬的買進張數加總起來，稱之為散戶買張。  請注意：交易所提供分點統計資料內，同一價格的買進成交會自動合併成一筆資料。所以成交值是依照合併後的資料來計算。  週頻率以上為期間累計值。 |

---
## 散戶買賣超張數
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 散戶買賣超張數 (資料欄位) |
| 語法 | ``` Value1 = GetField("散戶買賣超張數"); Value1 = GetField("Retaildifference"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 還原日 還原週 還原月 季 半年 年 |
| 支援商品 | 台股 |
| 說明 | 散戶買張 ─ 散戶賣張。  散戶買張是單一價格成交值小於50萬的合計買進張數，而散戶賣張則是同樣級距的合計賣出張數。  週頻率以上為期間累計值。 |

---
## 散戶賣張
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 散戶賣張 (資料欄位) |
| 語法 | ``` Value1 = GetField("散戶賣張"); Value1 = GetField("Retailtotalsell"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 還原日 還原週 還原月 季 半年 年 |
| 支援商品 | 台股 |
| 說明 | 單一價格成交值小於50萬的合計賣出張數。  系統每日統計券商分點對個股的買賣記錄，把成交值小於50萬的賣出張數加總起來，稱之為散戶賣張。  請注意：交易所提供分點統計資料內，同一價格的賣出成交會自動合併成一筆資料。所以成交值是依照合併後的資料來計算。  週頻率以上為期間累計值。 |

---
## 新產能預計量產日期
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 新產能預計量產日期 (資料欄位) |
| 語法 | ``` Value1 = GetField("新產能預計量產日期"); Value1 = GetField("DateOfNewProductionCapacity"); ``` |
| 欄位分類 | 籌碼 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 單一 |
| 支援商品 | 台股 |
| 說明 | 最近可能增加產能的預估量產日期。  欄位格式為西元年月日，例如20221101。  新增產能的資料來源為各公司年報或是新聞媒體上所公開的資訊，例如公司新廠預計投入營運的日期。  可以檢查GetFieldDate("新產能預計量產日期")是否等於0來判斷該公司是否有此資料。 |

---
## 機構持股
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 機構持股 (資料欄位) |
| 語法 | ``` Value1 = GetField("機構持股"); Value1 = GetField("13FHoldings"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 股 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 季 |
| 支援商品 | 美(股票) |
| 說明 | 依美國證券交易委員會規定每季公告的「SEC Form 13F」所計算出的投資機構總持有股數。  美股股票才適用本欄位。  依照SEC的規定，管理超過一億美元以上的機構投資人，需在每季結束後的45天內向美國證券交易委員會提供報告，載明所有持股的明細。  透過申報資料，系統會加總所有機構人對單一股票的總持股數量，每季提供一筆數值，更新時間約在當季結束後45日後陸續更新。 |

---
## 機構持股比重
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 機構持股比重 (資料欄位) |
| 語法 | ``` Value1 = GetField("機構持股比重"); Value1 = GetField("13FHoldingsRate"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | % |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 季 |
| 支援商品 | 美(股票) |
| 說明 | 依美國證券交易委員會規定每季公告的「SEC Form 13F」所計算出的投資機構總持有股數佔流通在外股數的比重。  單位為%，例如：欄位數值75%，則回傳75。  美股股票才適用本欄位。  依照SEC的規定，管理超過一億美元以上的機構投資人，需在每季結束後的45天內向美國證券交易委員會提供報告，載明所有持股的明細。  透過申報資料，系統會加總所有機構人對單一股票的總持股數量，每季提供一筆數值，更新時間約在當季結束後45日後陸續更新。 |

---
## 法人持股
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 法人持股 (資料欄位) |
| 語法 | ``` Value1 = GetField("法人持股"); Value1 = GetField("InvestorSharesHeld"); Value1 = GetField("法人持股張數"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 還原日 還原週 還原月 季 半年 年 |
| 支援商品 | 台股 類股指數 |
| 說明 | 三大法人(外資、投信、自營商)合計估計持股張數。  計算公式是外資持股(交易所公布數值)＋投信持股(系統估計值)＋自營商持股(系統估計值)。  類股商品（細產業指標、主題指標）的數值則是成分股的法人持股的加總。  週頻率以上為統計期間的期底值。 |

---
## 法人持股比例
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 法人持股比例 (資料欄位) |
| 語法 | ``` Value1 = GetField("法人持股比例"); Value1 = GetField("InvestorSharesHeldRatio"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 還原日 還原週 還原月 季 半年 年 |
| 支援商品 | 台股 |
| 說明 | 系統估算值，計算公式為：法人持股/流通在外張數 \* 100%。  單位為%，例如：持股比例若為20.5%，則回傳20.5。  週頻率以上為統計期間的期底值。 |

---
## 法人買張
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 法人買張 (資料欄位) |
| 語法 | ``` Value1 = GetField("法人買張"); Value1 = GetField("InvestorTotalBuy"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 還原日 還原週 還原月 季 半年 年 |
| 支援商品 | 台股 |
| 說明 | 三大法人(外資、投信、自營商)合計買進張數。 計算公式：外資買張＋投信買張＋自營商買張。  週頻率以上為期間累計值。 |

---
## 法人買賣超
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 法人買賣超 (資料欄位) |
| 語法 | ``` Value1 = GetField("法人買賣超"); Value1 = GetField("InvestorDifferenceTotal"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 台股,類股: 張. 大盤: 元 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 還原日 還原週 還原月 季 半年 年 |
| 支援商品 | 台股 大盤 類股指數 |
| 說明 | 三大法人買賣超。數值內容/單位依商品類型而異。  台股商品的數值為三大法人合計買賣超張數。 類股商品（細產業指標、主題指標）的數值則是成分股的三大法人買賣超張數的加總。 大盤商品的數值則是三大法人合計買賣超金額，單位是元。  週頻率以上為期間累計值。 |

---
## 法人買賣超張數
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 法人買賣超張數 (資料欄位) |
| 語法 | ``` Value1 = GetField("法人買賣超張數"); Value1 = GetField("InvestorDifference"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 還原日 還原週 還原月 季 半年 年 |
| 支援商品 | 台股 類股指數 |
| 說明 | 三大法人(外資、投信、自營商)合計買賣超張數。  台股商品的數值為法人買張 ─ 法人賣張。 類股商品（細產業指標、主題指標）的數值則是成分股的法人買賣超的加總。 週頻率以上為期間累計值。 |

---
## 法人買賣超金額
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 法人買賣超金額 (資料欄位) |
| 語法 | ``` Value1 = GetField("法人買賣超金額"); Value1 = GetField("InvestorDifferenceAmount"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 季 半年 年 |
| 支援商品 | 大盤 |
| 說明 | 三大法人(外資、投信、自營商)合計買賣超金額。 計算公式：外資買賣超金額＋投信買賣超金額＋自營商買賣超金額。  週頻率以上為期間累計值。 |

---
## 法人買進比重
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 法人買進比重 (資料欄位) |
| 語法 | ``` Value1 = GetField("法人買進比重"); Value1 = GetField("InvestorLongRatio"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 |
| 支援商品 | 大盤 |
| 說明 | 法人買進金額占大盤成交量的比重。 單位是％，例如買進比重為12%的話會回傳12。  計算公式：法人買進金額／大盤成交金額＊100%。  週頻率以上為期間買進金額加總／期間大盤成交金額加總。 |

---
## 法人買進金額
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 法人買進金額 (資料欄位) |
| 語法 | ``` Value1 = GetField("法人買進金額"); Value1 = GetField("InvestorTotalBuyAmount"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 季 半年 年 |
| 支援商品 | 大盤 |
| 說明 | 三大法人(外資、投信、自營商)合計買進金額。 計算公式：外資買進金額＋投信買進金額＋自營商買進金額。  週頻率以上為期間累計值。 |

---
## 法人賣出比重
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 法人賣出比重 (資料欄位) |
| 語法 | ``` Value1 = GetField("法人賣出比重"); Value1 = GetField("InvestorShortRatio"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 |
| 支援商品 | 大盤 |
| 說明 | 法人賣出金額占大盤成交量的比重。 單位是％，例如賣出比重為12%的話會回傳12。  計算公式：法人賣出金額／大盤成交金額＊100%。  週頻率以上為期間賣出金額加總／期間大盤成交金額加總。 |

---
## 法人賣出金額
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 法人賣出金額 (資料欄位) |
| 語法 | ``` Value1 = GetField("法人賣出金額"); Value1 = GetField("InvestorTotalSellAmount"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 季 半年 年 |
| 支援商品 | 大盤 |
| 說明 | 三大法人(外資、投信、自營商)合計賣出金額。 計算公式：外資賣出金額＋投信賣出金額＋自營商賣出金額。  週頻率以上為期間累計值。 |

---
## 法人賣張
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 法人賣張 (資料欄位) |
| 語法 | ``` Value1 = GetField("法人賣張"); Value1 = GetField("InvestorTotalSell"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 還原日 還原週 還原月 季 半年 年 |
| 支援商品 | 台股 |
| 說明 | 三大法人(外資、投信、自營商)合計賣出張數。 計算公式：外資賣張＋投信賣張＋自營商賣張。  週頻率以上為期間累計值。 |

---
## 現券償還張數
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 現券償還張數 (資料欄位) |
| 語法 | ``` Value1 = GetField("現券償還張數"); Value1 = GetField("ShortSalesRS"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 還原日 還原週 還原月 季 半年 年 |
| 支援商品 | 台股 大盤 |
| 說明 | 以現股方式償還融券的張數。  現券償還指的是以現股來償還融券放空的部位。  週頻率以上為期間累計值。 |

---
## 現增比率
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 現增比率 (資料欄位) |
| 語法 | ``` Value1 = GetField("現增比率"); Value1 = GetField("RatioofCashIncrement"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 股 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 單一 |
| 支援商品 | 台股 |
| 說明 | 最新一期現金增資內每千股可認購新股的股數。  例如：如果每千股可以認購20股的話，則數值為20。  可以檢查GetFieldDate("現增比率")是否等於0來判斷是否有現金增資的資料。 |

---
## 現增金額
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 現增金額 (資料欄位) |
| 語法 | ``` Value1 = GetField("現增金額"); Value1 = GetField("PriceofCashIncrement"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 億 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 單一 |
| 支援商品 | 台股 |
| 說明 | 最新一期現金增資的發行總金額。  可以檢查GetFieldDate("現增金額")是否等於0來判斷是否有現金增資的資料。 |

---
## 現股當沖張數
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 現股當沖張數 (資料欄位) |
| 語法 | ``` Value1 = GetField("現股當沖張數"); Value1 = GetField("NormalDayTrades"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 還原日 還原週 還原月 季 半年 年 |
| 支援商品 | 台股 大盤 類股指數 |
| 說明 | 現股當日沖銷的張數。  類股商品（細產業指標、主題指標）的數值為成分股現股當沖張數的加總。 大盤商品的數值為交易所公布值。  週頻率以上為期間累計值。 |

---
## 現股當沖買進金額
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 現股當沖買進金額 (資料欄位) |
| 語法 | ``` Value1 = GetField("現股當沖買進金額"); Value1 = GetField("DayTradeBValue"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 還原日 還原週 還原月 季 半年 年 |
| 支援商品 | 台股 大盤 類股指數 |
| 說明 | 現股當日沖銷的總買進金額。  類股商品（細產業指標、主題指標）的數值為成分股現股當沖買進金額的加總。 大盤商品的數值為交易所公布值。  週頻率以上為期間累計值。 |

---
## 現股當沖賣出金額
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 現股當沖賣出金額 (資料欄位) |
| 語法 | ``` Value1 = GetField("現股當沖賣出金額"); Value1 = GetField("DayTradeSValue"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 還原日 還原週 還原月 季 半年 年 |
| 支援商品 | 台股 大盤 類股指數 |
| 說明 | 現股當日沖銷的總賣出金額。  類股商品（細產業指標、主題指標）的數值為成分股現股當沖賣出金額的加總。 大盤商品的數值為交易所公布值。  週頻率以上為期間累計值。 |

---
## 現金償還張數
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 現金償還張數 (資料欄位) |
| 語法 | ``` Value1 = GetField("現金償還張數"); Value1 = GetField("POMRC"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 還原日 還原週 還原月 季 半年 年 |
| 支援商品 | 台股 大盤 |
| 說明 | 以現金方式償還融資的張數。  現金償還指的是以補足現金把融資股票轉成現股。  週頻率以上為期間累計值。 |

---
## 申報人數
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 申報人數 (資料欄位) |
| 語法 | ``` Value1 = GetField("申報人數"); Value1 = GetField("DTPeople"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 人 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 |
| 支援商品 | 大盤 類股指數 |
| 說明 | 內部人申報轉讓持股的人數加總。  類股商品（細產業指標、主題指標）計算方式為成分股加總。  週頻率以上為期間累計值。 |

---
## 申報家數
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 申報家數 (資料欄位) |
| 語法 | ``` Value1 = GetField("申報家數"); Value1 = GetField("DTBranches"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 家 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 |
| 支援商品 | 大盤 類股指數 |
| 說明 | 內部人申報轉讓持股的家數加總。  類股商品（細產業指標、主題指標）計算方式為成分股加總。  週頻率以上為期間累計值。 |

---
## 申報總市值
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 申報總市值 (資料欄位) |
| 語法 | ``` Value1 = GetField("申報總市值"); Value1 = GetField("DTTotalValue"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 |
| 支援商品 | 大盤 類股指數 |
| 說明 | 內部人申報轉讓持股的市值加總。  申報轉讓市值的計算方式是轉讓張數乘上當日收盤價。  類股商品（細產業指標、主題指標）計算方式為成分股加總。  週頻率以上為期間累計值。 |

---
## 當日沖銷張數
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 當日沖銷張數 (資料欄位) |
| 語法 | ``` Value1 = GetField("當日沖銷張數"); Value1 = GetField("Daytradeshares"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 還原日 還原週 還原月 季 半年 年 |
| 支援商品 | 台股 大盤 |
| 說明 | 當日沖銷的總張數。計算公式：現股當沖張數＋資券互抵張數。  大盤商品以及類股商品（細產業指標、主題指標）的數值為成分股當日沖銷張數的加總。  週頻率以上為期間累計值。 |

---
## 綜合前十大券商累計買賣超金額
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 綜合前十大券商累計買賣超金額 (資料欄位) |
| 語法 | ``` Value1 = GetField("綜合前十大券商累計買賣超金額"); Value1 = GetField("IB10BSCumAmount"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 |
| 支援商品 | 大盤 |
| 說明 | 「綜合前十大券商」每日買賣超金額的累積值。  綜合前十大券商名單為:統一、元富、台新、玉山、國泰、群益金鼎、凱基、富邦、元大、永豐金。  一般在看籌碼分析的時候，我們會去研究今天的買賣盤是來自那一類的力量，一般分為公司派，外資，基金公司，主力，實戶，業內，散戶及當沖客。但其實還有一股力量叫代操，代操分成兩類，一類是檯面上的，一類是檯面下的，檯面上的主要是投信及投顧，這個力量其實很龐大。  這些資金的進出場，沒有算在投信買賣超裡頭，如果我們想要知道代操資金的動向，必須另謀他法。  我們有觀察到，代操體系一般不會自己養大量研究員，還是會需要券商的研究資源來支援，所以他們下單的券商，絕大多數是能提供研究資源的大型券商法人部，而這些法人部的業績一般是算到券商總公司，所以我們才會把前十大券商總公司合計成為一個群組來觀察他們的買賣超情況。  更多資訊請參考：[籌碼大數據裡的“前十大券商總公司”釋疑](https://www.xq.com.tw/xstrader/%E7%B1%8C%E7%A2%BC%E5%A4%A7%E6%95%B8%E6%93%9A%E8%A3%A1%E7%9A%84%E5%89%8D%E5%8D%81%E5%A4%A7%E5%88%B8%E5%95%86%E7%B8%BD%E5%85%AC%E5%8F%B8%E9%87%8B%E7%96%91/)  資料起點為2013/10。 |

---
## 綜合前十大券商買賣超張數
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 綜合前十大券商買賣超張數 (資料欄位) |
| 語法 | ``` Value1 = GetField("綜合前十大券商買賣超張數"); Value1 = GetField("IB10Difference"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 |
| 支援商品 | 台股 類股指數 |
| 說明 | 「綜合前十大券商」買進張數 － 賣出張數。  綜合前十大券商名單為:統一、元富、台新、玉山、國泰、群益金鼎、凱基、富邦、元大、永豐金。  一般在看籌碼分析的時候，我們會去研究今天的買賣盤是來自那一類的力量，一般分為公司派，外資，基金公司，主力，實戶，業內，散戶及當沖客。但其實還有一股力量叫代操，代操分成兩類，一類是檯面上的，一類是檯面下的，檯面上的主要是投信及投顧，這個力量其實很龐大。  這些資金的進出場，沒有算在投信買賣超裡頭，如果我們想要知道代操資金的動向，必須另謀他法。  我們有觀察到，代操體系一般不會自己養大量研究員，還是會需要券商的研究資源來支援，所以他們下單的券商，絕大多數是能提供研究資源的大型券商法人部，而這些法人部的業績一般是算到券商總公司，所以我們才會把前十大券商總公司合計成為一個群組來觀察他們的買賣超情況。  更多資訊請參考：[籌碼大數據裡的“前十大券商總公司”釋疑](https://www.xq.com.tw/xstrader/%E7%B1%8C%E7%A2%BC%E5%A4%A7%E6%95%B8%E6%93%9A%E8%A3%A1%E7%9A%84%E5%89%8D%E5%8D%81%E5%A4%A7%E5%88%B8%E5%95%86%E7%B8%BD%E5%85%AC%E5%8F%B8%E9%87%8B%E7%96%91/)  類股商品（細產業指標、主題指標）計算方式為成分股加總。  週頻率以上為期間累計值。  請注意：因為統計時是以股為單位，所以欄位數值可能會包含小數(未滿1張時)。 |

---
## 綜合前十大券商買進金額
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 綜合前十大券商買進金額 (資料欄位) |
| 語法 | ``` Value1 = GetField("綜合前十大券商買進金額"); Value1 = GetField("IB10BAmount"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 季 半年 年 |
| 支援商品 | 大盤 |
| 說明 | 「綜合前十大券商」對市場所有股票的合計買進金額。  綜合前十大券商名單為：統一、元富、台新、玉山、國泰、群益金鼎、凱基、富邦、元大、永豐金。  一般在看籌碼分析的時候，我們會去研究今天的買賣盤是來自那一類的力量，一般分為公司派，外資，基金公司，主力，實戶，業內，散戶及當沖客。但其實還有一股力量叫代操，代操分成兩類，一類是檯面上的，一類是檯面下的，檯面上的主要是投信及投顧，這個力量其實很龐大。  這些資金的進出場，沒有算在投信買賣超裡頭，如果我們想要知道代操資金的動向，必須另謀他法。  我們有觀察到，代操體系一般不會自己養大量研究員，還是會需要券商的研究資源來支援，所以他們下單的券商，絕大多數是能提供研究資源的大型券商法人部，而這些法人部的業績一般是算到券商總公司，所以我們才會把前十大券商總公司合計成為一個群組來觀察他們的買賣超情況。  更多資訊請參考：[籌碼大數據裡的“前十大券商總公司”釋疑](https://www.xq.com.tw/xstrader/%E7%B1%8C%E7%A2%BC%E5%A4%A7%E6%95%B8%E6%93%9A%E8%A3%A1%E7%9A%84%E5%89%8D%E5%8D%81%E5%A4%A7%E5%88%B8%E5%95%86%E7%B8%BD%E5%85%AC%E5%8F%B8%E9%87%8B%E7%96%91/)  週頻率以上為期間累計值。 |

---
## 綜合前十大券商賣出金額
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 綜合前十大券商賣出金額 (資料欄位) |
| 語法 | ``` Value1 = GetField("綜合前十大券商賣出金額"); Value1 = GetField("IB10SAmount"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 季 半年 年 |
| 支援商品 | 大盤 |
| 說明 | 「綜合前十大券商」對市場所有股票的合計賣出金額。  綜合前十大券商名單為:統一、元富、台新、玉山、國泰、群益金鼎、凱基、富邦、元大、永豐金。  一般在看籌碼分析的時候，我們會去研究今天的買賣盤是來自那一類的力量，一般分為公司派，外資，基金公司，主力，實戶，業內，散戶及當沖客。但其實還有一股力量叫代操，代操分成兩類，一類是檯面上的，一類是檯面下的，檯面上的主要是投信及投顧，這個力量其實很龐大。  這些資金的進出場，沒有算在投信買賣超裡頭，如果我們想要知道代操資金的動向，必須另謀他法。  我們有觀察到，代操體系一般不會自己養大量研究員，還是會需要券商的研究資源來支援，所以他們下單的券商，絕大多數是能提供研究資源的大型券商法人部，而這些法人部的業績一般是算到券商總公司，所以我們才會把前十大券商總公司合計成為一個群組來觀察他們的買賣超情況。  更多資訊請參考：[籌碼大數據裡的“前十大券商總公司”釋疑](https://www.xq.com.tw/xstrader/%E7%B1%8C%E7%A2%BC%E5%A4%A7%E6%95%B8%E6%93%9A%E8%A3%A1%E7%9A%84%E5%89%8D%E5%8D%81%E5%A4%A7%E5%88%B8%E5%95%86%E7%B8%BD%E5%85%AC%E5%8F%B8%E9%87%8B%E7%96%91/)  週頻率以上為期間累計值。 |

---
## 總持股人數
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 總持股人數 (資料欄位) |
| 語法 | ``` Value1 = GetField("總持股人數"); Value1 = GetField("TotalSharesHeldPeople"); Value1 = GetField("總股東人數"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 人 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 週 月 季 半年 年 |
| 支援商品 | 台股 類股指數 台(可轉債) |
| 說明 | 由集保公司所提供的總持股人數。 股票每週更新一次，可轉債(CB)則是每月更新一次。  指數型商品(例如類股指數，細產業指標、主題指標)亦提供此欄位，計算方式則是依照成份股的數值進行加總。 |

---
## 自營商成本
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 自營商成本 (資料欄位) |
| 語法 | ``` Value1 = GetField("自營商成本"); Value1 = GetField("DBSCost"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 還原日 還原週 還原月 季 半年 年 |
| 支援商品 | 台股 |
| 說明 | 系統估算的自營商持股成本。  計算方式是透過每日自營商買張，賣張，以及當日的價格，估算出自營商的長期持有成本。  週頻率以上為統計期間的期底值。 |

---
## 自營商持股
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 自營商持股 (資料欄位) |
| 語法 | ``` Value1 = GetField("自營商持股"); Value1 = GetField("Dsharesheld"); Value1 = GetField("自營商持股張數"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 還原日 還原週 還原月 季 半年 年 |
| 支援商品 | 台股 類股指數 |
| 說明 | 系統估計值，計算方式是累計長期的自營商買賣超張數。  類股商品（細產業指標、主題指標）的數值則是成分股的自營商持股的加總。  週頻率以上為統計期間的期底值。 |

---
## 自營商持股比例
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 自營商持股比例 (資料欄位) |
| 語法 | ``` Value1 = GetField("自營商持股比例"); Value1 = GetField("Dsharesheldratio"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 還原日 還原週 還原月 季 半年 年 |
| 支援商品 | 台股 |
| 說明 | 系統估算值，計算公式為：自營商持股/流通在外張數 \* 100%。  單位為%，例如：持股比例若為20.5%，則回傳20.5。  週頻率以上為統計期間的期底值。 |

---
## 自營商自行買賣買張
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 自營商自行買賣買張 (資料欄位) |
| 語法 | ``` Value1 = GetField("自營商自行買賣買張"); Value1 = GetField("DSelftotalbuy"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 季 半年 年 |
| 支援商品 | 台股 大盤 |
| 說明 | 自營商買進張數內歸屬於自行買賣的數量。  週頻率以上為期間累計值。 |

---
## 自營商自行買賣買賣超
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 自營商自行買賣買賣超 (資料欄位) |
| 語法 | ``` Value1 = GetField("自營商自行買賣買賣超"); Value1 = GetField("DSelfdifference"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 季 半年 年 |
| 支援商品 | 台股 大盤 |
| 說明 | 自營商買賣超內歸屬於自行買賣的數量。數值內容/單位依商品類型而異。  台股商品的數值為自營商自行買賣買張 ─ 自營商自行買賣賣張。 大盤商品的數值則是自營商自行買賣買進金額 ─ 自營商自行買賣賣出金額，單位是元。  週頻率以上為期間累計值。 |

---
## 自營商自行買賣買賣超金額
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 自營商自行買賣買賣超金額 (資料欄位) |
| 語法 | ``` Value1 = GetField("自營商自行買賣買賣超金額"); Value1 = GetField("DSelfDifferenceAmount"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 |
| 支援商品 | 大盤 |
| 說明 | 計算公式為自營商自行買賣買進金額 ─ 自營商自行買賣賣出金額。  週頻率以上為期間累計值。 |

---
## 自營商自行買賣買進金額
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 自營商自行買賣買進金額 (資料欄位) |
| 語法 | ``` Value1 = GetField("自營商自行買賣買進金額"); Value1 = GetField("DSelfTotalBuyAmount"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 |
| 支援商品 | 大盤 |
| 說明 | 自營商買進金額內歸屬於自行買賣的數量。  週頻率以上為期間累計值。 |

---
## 自營商自行買賣賣出金額
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 自營商自行買賣賣出金額 (資料欄位) |
| 語法 | ``` Value1 = GetField("自營商自行買賣賣出金額"); Value1 = GetField("DSelfTotalSellAmount"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 |
| 支援商品 | 大盤 |
| 說明 | 自營商賣出金額內歸屬於自行買賣的數量。  週頻率以上為期間累計值。 |

---
## 自營商自行買賣賣張
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 自營商自行買賣賣張 (資料欄位) |
| 語法 | ``` Value1 = GetField("自營商自行買賣賣張"); Value1 = GetField("DSelftotalsell"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 季 半年 年 |
| 支援商品 | 台股 大盤 |
| 說明 | 自營商買進張數內歸屬於自行買賣的數量。  週頻率以上為期間累計值。 |

---
## 自營商買張
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 自營商買張 (資料欄位) |
| 語法 | ``` Value1 = GetField("自營商買張"); Value1 = GetField("Dtotalbuy"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 還原日 還原週 還原月 季 半年 年 |
| 支援商品 | 台股 |
| 說明 | 自營商買進張數。  週頻率以上為期間累計值。 |

---
## 自營商買賣超
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 自營商買賣超 (資料欄位) |
| 語法 | ``` Value1 = GetField("自營商買賣超"); Value1 = GetField("Ddifference"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 台股,類股: 張. 大盤:元 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 還原日 還原週 還原月 季 半年 |
| 支援商品 | 台股 大盤 類股指數 |
| 說明 | 自營商買賣超。數值內容/單位依商品類型而異。  台股商品的數值為自營商買張 ─ 自營商賣張。 類股商品（細產業指標、主題指標）的數值則是成分股的自營商買賣超的加總。 大盤商品的數值則是自營商買進金額 ─ 自營商賣出金額，單位是元。  週頻率以上為期間累計值。 |

---
## 自營商買賣超張數
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 自營商買賣超張數 (資料欄位) |
| 語法 | ``` Value1 = GetField("自營商買賣超張數"); Value1 = GetField("DdifferenceonStock"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 還原日 還原週 還原月 季 半年 |
| 支援商品 | 台股 類股指數 |
| 說明 | 自營商買賣超張數。  台股商品的數值為自營商買張 ─ 自營商賣張。 類股商品（細產業指標、主題指標）的數值則是成分股的自營商買賣超的加總。  週頻率以上為期間累計值。 |

---
## 自營商買賣超金額
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 自營商買賣超金額 (資料欄位) |
| 語法 | ``` Value1 = GetField("自營商買賣超金額"); Value1 = GetField("DBSAmount"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 季 半年 年 |
| 支援商品 | 大盤 |
| 說明 | 自營商買賣超金額，計算公式為自營商買進金額 ─ 自營商賣出金額。  週頻率以上為期間累計值。 |

---
## 自營商買進金額
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 自營商買進金額 (資料欄位) |
| 語法 | ``` Value1 = GetField("自營商買進金額"); Value1 = GetField("DBAmount"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 季 半年 年 |
| 支援商品 | 大盤 |
| 說明 | 自營商買進金額。  週頻率以上為期間累計值。 |

---
## 自營商賣出金額
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 自營商賣出金額 (資料欄位) |
| 語法 | ``` Value1 = GetField("自營商賣出金額"); Value1 = GetField("DSAmount"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 季 半年 年 |
| 支援商品 | 大盤 |
| 說明 | 自營商賣出金額。  週頻率以上為期間累計值。 |

---
## 自營商賣張
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 自營商賣張 (資料欄位) |
| 語法 | ``` Value1 = GetField("自營商賣張"); Value1 = GetField("Dtotalsell"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 還原日 還原週 還原月 季 半年 年 |
| 支援商品 | 台股 |
| 說明 | 自營商賣出張數。  週頻率以上為期間累計值。 |

---
## 自營商避險買張
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 自營商避險買張 (資料欄位) |
| 語法 | ``` Value1 = GetField("自營商避險買張"); Value1 = GetField("DtotalHedgebuy"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 季 半年 年 |
| 支援商品 | 台股 大盤 |
| 說明 | 自營商買進張數內歸屬於避險的數量。  週頻率以上為期間累計值。 |

---
## 自營商避險買賣超
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 自營商避險買賣超 (資料欄位) |
| 語法 | ``` Value1 = GetField("自營商避險買賣超"); Value1 = GetField("DHedgedifference"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 季 半年 年 |
| 支援商品 | 台股 大盤 |
| 說明 | 自營商買賣超內歸屬於避險的數量。數值內容/單位依商品類型而異。  台股商品的數值為自營商避險買張 ─ 自營商避險賣張。 大盤商品的數值則是自營商避險買進金額 ─ 自營商避險賣出金額，單位是元。  週頻率以上為期間累計值。 |

---
## 自營商避險買賣超金額
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 自營商避險買賣超金額 (資料欄位) |
| 語法 | ``` Value1 = GetField("自營商避險買賣超金額"); Value1 = GetField("DHedgeDifferenceAmount"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 |
| 支援商品 | 大盤 |
| 說明 | 計算公式為自營商避險買進金額 ─ 自營商避險賣出金額。  週頻率以上為期間累計值。 |

---
## 自營商避險買進金額
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 自營商避險買進金額 (資料欄位) |
| 語法 | ``` Value1 = GetField("自營商避險買進金額"); Value1 = GetField("DTotalHedgeBuyAmount"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 |
| 支援商品 | 大盤 |
| 說明 | 自營商買進金額內歸屬於避險的數量。  週頻率以上為期間累計值。 |

---
## 自營商避險賣出金額
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 自營商避險賣出金額 (資料欄位) |
| 語法 | ``` Value1 = GetField("自營商避險賣出金額"); Value1 = GetField("DTotalHedgeSellAmount"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 |
| 支援商品 | 大盤 |
| 說明 | 自營商賣出金額內歸屬於避險的數量。  週頻率以上為期間累計值。 |

---
## 自營商避險賣張
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 自營商避險賣張 (資料欄位) |
| 語法 | ``` Value1 = GetField("自營商避險賣張"); Value1 = GetField("DtotalHedgesell"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 季 半年 年 |
| 支援商品 | 台股 大盤 |
| 說明 | 自營商買進張數內歸屬於避險的數量。  週頻率以上為期間累計值。 |

---
## 董監持股佔股本比例
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 董監持股佔股本比例 (資料欄位) |
| 語法 | ``` Value1 = GetField("董監持股佔股本比例"); Value1 = GetField("DirectorHeld／Capital"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | % |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 月 |
| 支援商品 | 台股 |
| 說明 | 董監持股占公司股本的比例。  計算公式為董監持股張數／公司股本＊100%。  董監持股統計的範圍包含公司內的董事以及監察人。  資料每月更新一次。 |

---
## 董監質設比例
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 董監質設比例 (資料欄位) |
| 語法 | ``` Value1 = GetField("董監質設比例"); Value1 = GetField("DSmortgageratio"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | % |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 月 |
| 支援商品 | 台股 |
| 說明 | 董監持股內質設的比例。  計算公式：董監持股內目前質設的張數／董監持股＊100%。  資料每月更新一次。 |

---
## 融券使用率
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 融券使用率 (資料欄位) |
| 語法 | ``` Value1 = GetField("融券使用率"); Value1 = GetField("ShortSell％"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 季 半年 年 |
| 支援商品 | 台股 |
| 說明 | 融券使用的比例。計算公式：融券餘額 / 融資限額 \* 100%。  單位是％，例如融券使用率為70%的話，則回傳70。  週頻率以上為統計期間的期底值。 |

---
## 融券增減張數
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 融券增減張數 (資料欄位) |
| 語法 | ``` Value1 = GetField("融券增減張數"); Value1 = GetField("Shortsalenew"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 台股/類股/大盤:張；陸股:股 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 還原日 還原週 還原月 季 半年 年 |
| 支援商品 | 台股 大盤 類股指數 大陸股票 |
| 說明 | 融券餘額(張數)與前一期的異動張數。  融券餘額張數為到目前為止以融券賣出的淨張數。  類股商品（細產業指標、主題指標）的數值為成分股融券增減張數的加總。  週頻率以上為期間累計值。 |

---
## 融券買進張數
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 融券買進張數 (資料欄位) |
| 語法 | ``` Value1 = GetField("融券買進張數"); Value1 = GetField("ShortSaleRecall"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 還原日 還原週 還原月 季 半年 年 |
| 支援商品 | 台股 大盤 |
| 說明 | 融券買進的張數。  週頻率以上為期間累計值。 |

---
## 融券賣出張數
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 融券賣出張數 (資料欄位) |
| 語法 | ``` Value1 = GetField("融券賣出張數"); Value1 = GetField("ShortSales"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 還原日 還原週 還原月 季 半年 年 |
| 支援商品 | 台股 大盤 |
| 說明 | 融券賣出的張數。  週頻率以上為期間累計值。 |

---
## 融券餘額張數
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 融券餘額張數 (資料欄位) |
| 語法 | ``` Value1 = GetField("融券餘額張數"); Value1 = GetField("Shortsaleremain"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 台股/類股/大盤:張；陸股:股 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 還原日 還原週 還原月 季 半年 年 |
| 支援商品 | 台股 大盤 類股指數 大陸股票 |
| 說明 | 融券餘額張數，也就是到目前為止以融券賣出的淨張數。  這個數值代表著還有多少融券張數尚未償還。  類股商品（細產業指標、主題指標）的數值為成分股融券餘額張數的加總。  週頻率以上為統計期間的期底值。 |

---
## 融資使用率
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 融資使用率 (資料欄位) |
| 語法 | ``` Value1 = GetField("融資使用率"); Value1 = GetField("Pomusingratio"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 還原日 還原週 還原月 季 半年 年 |
| 支援商品 | 台股 |
| 說明 | 融資使用的比例。計算公式：融資餘額／融資限額 \* 100%。  單位是％，例如融資使用率為70%的話，則回傳70。  週頻率以上為統計期間的期底值。 |

---
## 融資增減
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 融資增減 (資料欄位) |
| 語法 | ``` Value1 = GetField("融資增減"); Value1 = GetField("pomDiff"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 台股,類股:張. 大盤:元 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 還原日 還原週 還原月 季 半年 年 |
| 支援商品 | 台股 大盤 類股指數 |
| 說明 | 融資餘額與前一期的異動。數值內容/單位依商品類型而異。  台股商品的數值為融資增減張數。 類股商品（細產業指標、主題指標）的數值則是成分股融資增減張數的加總。 大盤商品的數值則是融資增減金額，單位是元。  週頻率以上為期間累計值。 |

---
## 融資增減張數
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 融資增減張數 (資料欄位) |
| 語法 | ``` Value1 = GetField("融資增減張數"); Value1 = GetField("Pomnew"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 台股/類股/大盤: 張；陸股:元 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 還原日 還原週 還原月 季 半年 年 |
| 支援商品 | 台股 大盤 類股指數 大陸股票 |
| 說明 | 融資餘額(張數)與前一期的異動張數。  融資餘額張數為到目前為止以融資買進的淨張數。  類股商品（細產業指標、主題指標）的數值為成分股融資增減張數的加總。  週頻率以上為期間累計值。 |

---
## 融資增減金額
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 融資增減金額 (資料欄位) |
| 語法 | ``` Value1 = GetField("融資增減金額"); Value1 = GetField("IndexPOMDiff"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 季 半年 年 |
| 支援商品 | 大盤 |
| 說明 | 融資餘額(金額)與前一期的異動金額。  融資餘額金額為到目前為止以融資買進的淨金額(元)。  週頻率以上為期間累計值。 |

---
## 融資維持率
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 融資維持率 (資料欄位) |
| 語法 | ``` Value1 = GetField("融資維持率"); Value1 = GetField("Pomremainratio"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 季 半年 年 |
| 支援商品 | 台股 大盤 |
| 說明 | 系統估算的大盤或是個股的融資維持率，也就是股票市值與融資金額的比例。  計算公式:  個股融資維持率=收盤價／(融資成本線 \* 0.6) \* 100%  大盤融資維持率=所有融資股票市值／大盤融資餘額 \* 100%  依查詢頻率取期底值。  註：  個股融資成本=(昨日融資買進成本 \* (融資餘額-融資買進) + 收盤價 \* 融資買進) / 融資餘額 |

---
## 融資買進張數
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 融資買進張數 (資料欄位) |
| 語法 | ``` Value1 = GetField("融資買進張數"); Value1 = GetField("POMBuy"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 還原週 還原月 季 半年 年 |
| 支援商品 | 台股 大盤 |
| 說明 | 融資買進的張數。  週頻率以上為期間累計值。 |

---
## 融資買進金額
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 融資買進金額 (資料欄位) |
| 語法 | ``` Value1 = GetField("融資買進金額"); Value1 = GetField("IndexPOMBuySideAmount"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 |
| 支援商品 | 大盤 |
| 說明 | 融資買進的總金額。  週頻率以上為期間累計值。 |

---
## 融資賣出張數
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 融資賣出張數 (資料欄位) |
| 語法 | ``` Value1 = GetField("融資賣出張數"); Value1 = GetField("POMSell"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 還原日 還原週 還原月 季 半年 年 |
| 支援商品 | 台股 大盤 |
| 說明 | 融資買進的張數。  週頻率以上為期間累計值。 |

---
## 融資賣出金額
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 融資賣出金額 (資料欄位) |
| 語法 | ``` Value1 = GetField("融資賣出金額"); Value1 = GetField("IndexPOMSellSideAmount"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 |
| 支援商品 | 大盤 |
| 說明 | 融資賣出的總金額。  週頻率以上為期間累計值。 |

---
## 融資餘額
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 融資餘額 (資料欄位) |
| 語法 | ``` Value1 = GetField("融資餘額"); Value1 = GetField("Pomremainbycomm"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 台股,類股:張. 大盤:元. |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 還原日 還原週 還原月 季 半年 年 |
| 支援商品 | 台股 大盤 類股指數 |
| 說明 | 融資餘額數值。數值內容/單位依商品類型而異。  台股商品的數值為融資餘額張數。 類股商品（細產業指標、主題指標）的數值則是成分股融資餘額張數的加總。 大盤商品的數值則是融資餘額金額，單位是元。  週頻率以上為統計期間的期底值。 |

---
## 融資餘額張數
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 融資餘額張數 (資料欄位) |
| 語法 | ``` Value1 = GetField("融資餘額張數"); Value1 = GetField("Pomremain"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 台股/類股/大盤: 張；陸股:元 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 還原日 還原週 還原月 季 半年 年 |
| 支援商品 | 台股 大盤 類股指數 大陸股票 |
| 說明 | 融資餘額張數，也就是到目前以融資買進的淨張數。  這個數值代表著還有多少融資張數尚未償還。  類股商品（細產業指標、主題指標）的數值為成分股融資餘額張數的加總。  週頻率以上為統計期間的期底值。 |

---
## 融資餘額金額
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 融資餘額金額 (資料欄位) |
| 語法 | ``` Value1 = GetField("融資餘額金額"); Value1 = GetField("IndexPOMRemain"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 季 半年 年 |
| 支援商品 | 大盤 |
| 說明 | 融資餘額金額，也就是到目前以融資買進的淨金額。  這個數值代表著還有多少融資金額尚未償還。  週頻率以上為統計期間的期底值。 |

---
## 買家數
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 買家數 (資料欄位) |
| 語法 | ``` Value1 = GetField("買家數"); Value1 = GetField("NetLongBranches"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 家 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 |
| 支援商品 | 台股 |
| 說明 | 統計買超張數大於零的券商家數。  同一個券商的所有分點加總統計，如果合計為買超，則算為1家。 |

---
## 買進公司家數
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 買進公司家數 (資料欄位) |
| 語法 | ``` Value1 = GetField("買進公司家數"); Value1 = GetField("LongSecurityFirms"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 家 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 |
| 支援商品 | 台股 |
| 說明 | 統計買進金額大於零的券商家數。  同一個券商的所有分點會合併統計，例如元大館前、元大忠孝都有買進個股，只會被計算為"元大"一家。 |

---
## 資券互抵張數
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 資券互抵張數 (資料欄位) |
| 語法 | ``` Value1 = GetField("資券互抵張數"); Value1 = GetField("MarginTrades"); Value1 = GetField("當沖張數"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 還原日 還原週 還原月 季 半年 年 |
| 支援商品 | 台股 大盤 類股指數 |
| 說明 | 資券當沖(信用當沖)的張數。  資券當沖指的是在同一日有融資買進、融券賣出所相抵的張數。  類股商品（細產業指標、主題指標）的數值為成分股資券互抵張數的加總。 大盤商品的數值為交易所公布值。  週頻率以上為期間累計值。 |

---
## 賣出公司家數
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 賣出公司家數 (資料欄位) |
| 語法 | ``` Value1 = GetField("賣出公司家數"); Value1 = GetField("ShortSecurityFirms"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 家 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 |
| 支援商品 | 台股 |
| 說明 | 統計賣出金額大於零的券商家數。  同一個券商的所有分點會合併統計，例如元大館前、元大忠孝都有買進個股，只會被計算為"元大"一家。 |

---
## 賣家數
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 賣家數 (資料欄位) |
| 語法 | ``` Value1 = GetField("賣家數"); Value1 = GetField("NetShortBranches"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 家 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 |
| 支援商品 | 台股 |
| 說明 | 統計賣超張數大於零的券商家數。  同一個券商的所有分點加總統計，如果合計為賣超，則算為1家。 |

---
## 還券張數
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 還券張數 (資料欄位) |
| 語法 | ``` Value1 = GetField("還券張數"); Value1 = GetField("SBLreturn"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 還原日 還原週 還原月 季 半年 年 |
| 支援商品 | 台股 大盤 |
| 說明 | 償還先前借券的張數。  週頻率以上為期間累計值。 |

---
## 關聯券商買賣超張數
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 關聯券商買賣超張數 (資料欄位) |
| 語法 | ``` Value1 = GetField("關聯券商買賣超張數"); Value1 = GetField("CorrelationBrokerDifference"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 |
| 支援商品 | 台股 |
| 說明 | 「關聯券商」買進張數 － 賣出張數。  關聯券商是比對每週集保股權分散表內「千張大戶持股」的變化與當週主力分點的買賣超，來找出千張大戶所在的分點，這些分點就稱為這檔商品的關聯券商。  資料從2017/06開始提供。 |

---
## 關鍵券商買賣超張數
**Path:** 資料欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 關鍵券商買賣超張數 (資料欄位) |
| 語法 | ``` Value1 = GetField("關鍵券商買賣超張數"); Value1 = GetField("KeyBrokerDifference"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 |
| 支援商品 | 台股 類股指數 |
| 說明 | 「關鍵券商」買進張數 － 賣出張數。  關鍵券商是個股實施庫藏股期間買超最大的券商分點，以此來追蹤大股東的進出動向。  類股商品（細產業指標、主題指標）計算方式為成分股加總。  請注意：因為統計時是以股為單位，所以欄位數值可能會包含小數(未滿1張時)。  資料從2013年開始提供。 |

---
## GDP比例
**Path:** 資料欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | GDP比例 (資料欄位) |
| 語法 | ``` Value1 = GetField("GDP比例"); Value1 = GetField("ToGDPRatio"); ``` |
| 欄位分類 | 量能 |
| 單位 | % |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 季 半年 年 |
| 支援商品 | 大盤 類股指數 |
| 說明 | 指數市值佔GDP的比例。單位是％，如果佔比是150％的話，則回傳150。  市值佔GDP比例可以用來觀察股市的熱度。計算公式：成分股總市值 / GDP ＊ 100%。  週以上頻率，是取期底值。 |

---
## 上漲量
**Path:** 資料欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 上漲量 (資料欄位) |
| 語法 | ``` Value1 = GetField("上漲量"); Value1 = GetField("UpVolume"); ``` |
| 欄位分類 | 量能 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 分鐘 日 |
| 支援商品 | 台股 大盤 類股指數 期貨 選擇權 |
| 說明 | 這一筆K棒的所有成交內比前一價上漲的成交量的加總。  例如，這一筆K棒內有4筆成交，分別是30元2張，30元3張，31元4張，30元5張，而前一筆K棒的收盤價是29。那麼上漲量=6張，下跌量=5張。  計算方法如下：   * 第一筆成交30元比前一價29元上漲(上漲量=2張) * 第二筆成交30元跟前一價30元相同，不予計算 * 第三筆31元比前一價30元上漲(上漲量=2張+4張=6張) * 第四筆30元比前一價31元下跌(下跌量=5張) |

---
## 下跌量
**Path:** 資料欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 下跌量 (資料欄位) |
| 語法 | ``` Value1 = GetField("下跌量"); Value1 = GetField("DownVolume"); ``` |
| 欄位分類 | 量能 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 分鐘 日 |
| 支援商品 | 台股 大盤 類股指數 期貨 選擇權 |
| 說明 | 這一筆K棒的所有成交內比前一價下跌的成交量的加總。  例如，這一筆K棒內有4筆成交，分別是30元2張，30元3張，31元4張，30元5張，而前一筆K棒的收盤價是29。那麼上漲量=6張，下跌量=5張。  計算方法如下：   * 第一筆成交30元比前一價29元上漲(上漲量=2張) * 第二筆成交30元跟前一價30元相同，不予計算 * 第三筆31元比前一價30元上漲(上漲量=2張+4張=6張) * 第四筆30元比前一價31元下跌(下跌量=5張) |

---
## 估計量
**Path:** 資料欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 估計量 (資料欄位) |
| 語法 | ``` Value1 = GetField("估計量"); Value1 = GetField("EstimateVolume"); ``` |
| 欄位分類 | 量能 |
| 單位 | 個股:張，大盤:元，類股:元 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 分鐘 日 |
| 支援商品 | 大盤 類股指數 台(股票) |
| 說明 | 當日收盤的預估成交量。  大盤或是個股的估計的方式是統計過去一段時間內每日盤中的分鐘累計成交量，算出每分鐘成交量的分佈比例，然後依照當日開盤迄今的累計成交量，來推算收盤時可能的成交量。  類股商品（細產業指標、主題指標）的計算方式為成分股預估量乘以均價的加總。 |

---
## 內盤均量
**Path:** 資料欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 內盤均量 (資料欄位) |
| 語法 | ``` Value1 = GetField("內盤均量"); Value1 = GetField("InAvgVolume"); ``` |
| 欄位分類 | 量能 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 還原日 |
| 支援商品 | 台股 |
| 說明 | 計算公式：內盤成交量／內盤成交次數。 |

---
## 內盤成交次數
**Path:** 資料欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 內盤成交次數 (資料欄位) |
| 語法 | ``` Value1 = GetField("內盤成交次數"); Value1 = GetField("TotalInTicks"); ``` |
| 欄位分類 | 量能 |
| 單位 | 次 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 分鐘 日 |
| 支援商品 | 台股 期貨 選擇權 香港股票 大陸股票 |
| 說明 | 這一筆K棒的所有成交內以內盤成交的次數。 |

---
## 內盤量
**Path:** 資料欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 內盤量 (資料欄位) |
| 語法 | ``` Value1 = GetField("內盤量"); Value1 = GetField("TradeVolumeAtBid"); Value1 = GetField("內盤金額"); ``` |
| 欄位分類 | 量能 |
| 單位 | 台股:張；大盤/指數:元 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 分鐘 日 還原日 |
| 支援商品 | 台股 大盤 類股指數 期貨 選擇權 香港股票 大陸股票 |
| 說明 | 這一筆K棒的所有成交內標記為內盤成交的加總數量。  系統會依照成交發生當時委託簿的最佳委買價格以及最佳委賣價格來判斷此筆是外盤成交或是內盤成交。如果成交的價格貼近委買價格的話，則標示為內盤成交，一般會視為主動「賣出」。  如果是指數類型商品的話，則是比前一價下跌的所有成交量的加總。 |

---
## 外盤均量
**Path:** 資料欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 外盤均量 (資料欄位) |
| 語法 | ``` Value1 = GetField("外盤均量"); Value1 = GetField("OutAvgVolume"); ``` |
| 欄位分類 | 量能 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 還原日 |
| 支援商品 | 台股 |
| 說明 | 計算公式 : 外盤成交量／外盤成交次數。 |

---
## 外盤成交次數
**Path:** 資料欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 外盤成交次數 (資料欄位) |
| 語法 | ``` Value1 = GetField("外盤成交次數"); Value1 = GetField("TotalOutTicks"); ``` |
| 欄位分類 | 量能 |
| 單位 | 次 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 分鐘 日 |
| 支援商品 | 台股 期貨 選擇權 香港股票 大陸股票 |
| 說明 | 這一筆K棒的所有成交內以外盤成交的次數。 |

---
## 外盤量
**Path:** 資料欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 外盤量 (資料欄位) |
| 語法 | ``` Value1 = GetField("外盤量"); Value1 = GetField("TradeVolumeAtAsk"); Value1 = GetField("外盤金額"); ``` |
| 欄位分類 | 量能 |
| 單位 | 台股:張；大盤/指數:元 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 分鐘 日 還原日 |
| 支援商品 | 台股 大盤 類股指數 期貨 選擇權 香港股票 大陸股票 |
| 說明 | 這一筆K棒的所有成交內被標記為外盤成交的加總數量。  系統會依照成交發生當時委託簿的最佳委買價格以及最佳委賣價格來判斷此筆是外盤成交或是內盤成交。如果成交的價格貼近委賣價格的話，則標示為外盤成交，一般會視為主動「買進」。  如果是指數類型商品的話，則是比前一價上漲的所有成交量的加總。 |

---
## 委買均
**Path:** 資料欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 委買均 (資料欄位) |
| 語法 | ``` Value1 = GetField("委買均"); Value1 = GetField("AvgLongShare"); Value1 = GetField("AvgLongUnits"); Value1 = GetField("均買張數"); Value1 = GetField("均買口數"); ``` |
| 欄位分類 | 量能 |
| 單位 | 大盤:張；期貨/選擇權:口 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 |
| 支援商品 | 大盤 期貨 選擇權 |
| 說明 | 每一筆委買的平均張數/口數。 計算公式：累計委買／累委買筆。 |

---
## 委賣均
**Path:** 資料欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 委賣均 (資料欄位) |
| 語法 | ``` Value1 = GetField("委賣均"); Value1 = GetField("AvgShortShare"); Value1 = GetField("AvgShortUnits"); Value1 = GetField("均賣張數"); Value1 = GetField("均賣口數"); ``` |
| 欄位分類 | 量能 |
| 單位 | 大盤:張；期貨/選擇權:口 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 |
| 支援商品 | 大盤 期貨 選擇權 |
| 說明 | 每一筆委賣的平均張數/口數。 計算公式：累計委賣／累委賣筆。 |

---
## 成交均量
**Path:** 資料欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 成交均量 (資料欄位) |
| 語法 | ``` Value1 = GetField("成交均量"); Value1 = GetField("AvgDealedShare"); ``` |
| 欄位分類 | 量能 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 |
| 支援商品 | 大盤 |
| 說明 | 整體市場(扣除權證)的每筆成交平均張數。 計算公式：累計成交／累成交筆。 |

---
## 成交量
**Path:** 資料欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 成交量 (資料欄位) |
| 語法 | ``` Value1 = GetField("成交量"); Value1 = GetField("Volume"); ``` |
| 參數範圍 | Call、Put |
| 欄位分類 | 量能 |
| 單位 | 台股:張；大盤/類股:元；期貨/選擇權:口；其他市場股票:股 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | Tick 分鐘 日 週 月 季 半年 年 |
| 支援商品 | 全部 |
| 說明 | K棒資料的成交量。  數值單位依照商品類型而異，例如台股的話成交量的單位是張，可是美股的話成交量的單位則是股。  語法上也可以使用Volume來取得同樣的數值。 |

---
## 成交金額_元_
**Path:** 資料欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 成交金額(元) (資料欄位) |
| 語法 | ``` Value1 = GetField("成交金額(元)"); Value1 = GetField("TradeValue"); Value1 = GetField("成交金額"); ``` |
| 欄位分類 | 量能 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 分鐘 日 週 月 還原日 還原週 還原月 |
| 支援商品 | 台股 大盤 類股指數 香港股票 大陸股票 美(股票) |
| 說明 | 成交的加總金額。  如果是個股的話，計算方式是統計每筆成交數量乘上成交價。  如果是大盤或是指數型商品的話，此欄位與成交量是相同的。  數值依照統計區間而易，如果是分鐘頻率的話，則是該分鐘統計區間的每筆成交金額的加總，如果是週月以上的話則是每日成交金額的加總。 |

---
## 新聞正向分數
**Path:** 資料欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 新聞正向分數 (資料欄位) |
| 語法 | ``` Value1 = GetField("新聞正向分數"); Value1 = GetField("NewsPosScore"); ``` |
| 欄位分類 | 量能 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 |
| 支援商品 | 台(股票) |
| 說明 | 依據個股新聞內容，判斷該則新聞為正向，負向，或是中立(觀望)。  新聞正向分數則是正向的新聞則數。  請注意：   1. 並非每檔商品都會有數值， 2. 每日會定時更新數值，非交易日的資料會彙整到前一個交易日， 3. 資料從2019/1/1日開始提供。 |

---
## 新聞聲量分數
**Path:** 資料欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 新聞聲量分數 (資料欄位) |
| 語法 | ``` Value1 = GetField("新聞聲量分數"); Value1 = GetField("NewsVolScore"); ``` |
| 欄位分類 | 量能 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 |
| 支援商品 | 台(股票) |
| 說明 | 依據個股新聞內容，判斷該則新聞為正向，負向，或是中立(觀望)。  新聞聲量分數則是正向的新聞則數 + 負向的新聞則數 + 觀望的新聞則數。  請注意：   1. 並非每檔商品都會有數值， 2. 每日會定時更新數值，非交易日的資料會彙整到前一個交易日， 3. 資料從2019/1/1日開始提供。 |

---
## 新聞負向分數
**Path:** 資料欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 新聞負向分數 (資料欄位) |
| 語法 | ``` Value1 = GetField("新聞負向分數"); Value1 = GetField("NewsNegScore"); ``` |
| 欄位分類 | 量能 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 |
| 支援商品 | 台(股票) |
| 說明 | 依據個股新聞內容，判斷該則新聞為正向，負向，或是中立(觀望)。  新聞負向分數則是負向的新聞則數。  請注意：   1. 並非每檔商品都會有數值， 2. 每日會定時更新數值，非交易日的資料會彙整到前一個交易日， 3. 資料從2019/1/1日開始提供。 |

---
## 當日序號
**Path:** 資料欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 當日序號 (資料欄位) |
| 語法 | ``` Value1 = GetField("當日序號"); Value1 = GetField("SeqNo"); ``` |
| 欄位分類 | 量能 |
| 單位 | 筆 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | Tick |
| 支援商品 | 全部 |
| 說明 | 當日的第幾筆成交。數值從1開始。 |

---
## 盤中整股成交量
**Path:** 資料欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 盤中整股成交量 (資料欄位) |
| 語法 | ``` Value1 = GetField("盤中整股成交量"); Value1 = GetField("BoardLotVolume"); ``` |
| 欄位分類 | 量能 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 |
| 支援商品 | 台股 |
| 說明 | 盤中整股成交量 = 成交量 - 盤中零股成交量 - [盤後量](https://xshelp.xq.com.tw/XSHelp/?HelpName=%E7%9B%A4%E5%BE%8C%E9%87%8F&group=TVOLUME)(盤後整股成交量) - [零股量](https://xshelp.xq.com.tw/XSHelp/?HelpName=%E9%9B%B6%E8%82%A1%E9%87%8F&group=FVOLUME)(盤後零股成交量) - [鉅額交易量](https://xshelp.xq.com.tw/XSHelp/?HelpName=%E9%89%85%E9%A1%8D%E4%BA%A4%E6%98%93%E9%87%8F&group=FVOLUME) |

---
## 盤中零股成交量
**Path:** 資料欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 盤中零股成交量 (資料欄位) |
| 語法 | ``` Value1 = GetField("盤中零股成交量"); Value1 = GetField("OddLotVolume"); ``` |
| 欄位分類 | 量能 |
| 單位 | 股 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 |
| 支援商品 | 台股 |
| 說明 | 盤中零股成交量 = 成交量 - 盤中整股成交量 - [盤後量](https://xshelp.xq.com.tw/XSHelp/?HelpName=%E7%9B%A4%E5%BE%8C%E9%87%8F&group=TVOLUME)(盤後整股成交量) - [零股量](https://xshelp.xq.com.tw/XSHelp/?HelpName=%E9%9B%B6%E8%82%A1%E9%87%8F&group=FVOLUME)(盤後零股成交量) - [鉅額交易量](https://xshelp.xq.com.tw/XSHelp/?HelpName=%E9%89%85%E9%A1%8D%E4%BA%A4%E6%98%93%E9%87%8F&group=FVOLUME) |

---
## 盤後量
**Path:** 資料欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 盤後量 (資料欄位) |
| 語法 | ``` Value1 = GetField("盤後量"); Value1 = GetField("OffHourVolume"); Value1 = GetField("盤後整股成交量"); ``` |
| 欄位分類 | 量能 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 |
| 支援商品 | 台股 |
| 說明 | 盤後定價交易區間的整股成交張數(不含鉅額交易量) |

---
## 累委買筆
**Path:** 資料欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 累委買筆 (資料欄位) |
| 語法 | ``` Value1 = GetField("累委買筆"); Value1 = GetField("TotalBidTx"); ``` |
| 欄位分類 | 量能 |
| 單位 | 筆 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 分鐘 日 |
| 支援商品 | 大盤 期貨 選擇權 |
| 說明 | 開盤迄今的總委買筆數。  交易所盤中會定時公布此資訊。如果是分鐘頻率的話會取得當下最新數值，如果是日頻率的話則是當日最後的數值。  證券市場的數值為扣除權證的統計。 |

---
## 累委賣筆
**Path:** 資料欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 累委賣筆 (資料欄位) |
| 語法 | ``` Value1 = GetField("累委賣筆"); Value1 = GetField("TotalAskTx"); ``` |
| 欄位分類 | 量能 |
| 單位 | 筆 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 分鐘 日 |
| 支援商品 | 大盤 期貨 選擇權 |
| 說明 | 開盤迄今的總委賣筆數。  交易所盤中會定時公布此資訊。如果是分鐘頻率的話會取得當下最新數值，如果是日頻率的話則是當日最後的數值。  證券市場的數值為扣除權證的統計。 |

---
## 累成交筆
**Path:** 資料欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 累成交筆 (資料欄位) |
| 語法 | ``` Value1 = GetField("累成交筆"); Value1 = GetField("TotalMatchTx"); ``` |
| 欄位分類 | 量能 |
| 單位 | 筆 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 分鐘 日 |
| 支援商品 | 大盤 |
| 說明 | 開盤迄今整體市場(扣除權證)的成交總筆數。  交易所盤中會定時公布此資訊。如果是分鐘頻率的話會取得當下最新數值，如果是日頻率的話則是當日最後的數值。 |

---
## 累計委買
**Path:** 資料欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 累計委買 (資料欄位) |
| 語法 | ``` Value1 = GetField("累計委買"); Value1 = GetField("BidUnits"); Value1 = GetField("委買張數"); Value1 = GetField("委買口數"); ``` |
| 欄位分類 | 量能 |
| 單位 | 大盤:張；期貨/選擇權:口 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 分鐘 日 |
| 支援商品 | 大盤 期貨 選擇權 |
| 說明 | 開盤迄今的總委買張數/口數。  交易所盤中會定時公布此資訊。如果是分鐘頻率的話會取得當下最新數值，如果是日頻率的話則是當日最後的數值。  證券市場的數值為扣除權證的統計。 |

---
## 累計委賣
**Path:** 資料欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 累計委賣 (資料欄位) |
| 語法 | ``` Value1 = GetField("累計委賣"); Value1 = GetField("AskUnits"); Value1 = GetField("委賣張數"); Value1 = GetField("委賣口數"); ``` |
| 欄位分類 | 量能 |
| 單位 | 大盤:張；期貨/選擇權:口 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 分鐘 日 |
| 支援商品 | 大盤 期貨 選擇權 |
| 說明 | 開盤迄今的總委賣張數/口數。  交易所盤中會定時公布此資訊。如果是分鐘頻率的話會取得當下最新數值，如果是日頻率的話則是當日最後的數值。  證券市場的數值為扣除權證的統計。 |

---
## 累計成交
**Path:** 資料欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 累計成交 (資料欄位) |
| 語法 | ``` Value1 = GetField("累計成交"); Value1 = GetField("MatchUnit"); Value1 = GetField("成交張數"); ``` |
| 欄位分類 | 量能 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 分鐘 日 |
| 支援商品 | 大盤 |
| 說明 | 開盤迄今整體市場(扣除權證)的總成交張數。  交易所盤中會定時公布此資訊。如果是分鐘頻率的話會取得當下最新數值，如果是日頻率的話則是當日最後的數值。  請注意此數值與大盤成交量(金額)是不一樣的統計數據。 |

---
## 累買成筆
**Path:** 資料欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 累買成筆 (資料欄位) |
| 語法 | ``` Value1 = GetField("累買成筆"); Value1 = GetField("TotalBidMatchTx"); ``` |
| 欄位分類 | 量能 |
| 單位 | 筆 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 分鐘 日 |
| 支援商品 | 期貨 選擇權 |
| 說明 | 開盤迄今以買進成交的總筆數。  交易所盤中會定時公布此資訊。如果是分鐘頻率的話會取得當下最新數值，如果是日頻率的話則是當日最後的數值。  可以使用此數值(與累賣成筆)來估算盤中的趨勢：如果累賣成筆>累買成筆的話，代表每一筆以賣出成交的單量會小於以買進成交的單量，一般會視為大戶(單量大)目前站在買方。 |

---
## 累賣成筆
**Path:** 資料欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 累賣成筆 (資料欄位) |
| 語法 | ``` Value1 = GetField("累賣成筆"); Value1 = GetField("TotalAskMatchTx"); ``` |
| 欄位分類 | 量能 |
| 單位 | 筆 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 分鐘 日 |
| 支援商品 | 期貨 選擇權 |
| 說明 | 開盤迄今以賣出成交的總筆數。  交易所盤中會定時公布此資訊。如果是分鐘頻率的話會取得當下最新數值，如果是日頻率的話則是當日最後的數值。  可以使用此數值(與累買成筆)來估算盤中的趨勢：如果累賣成筆>累買成筆的話，代表每一筆以賣出成交的單量會小於以買進成交的單量，一般會視為大戶(單量大)目前站在買方。 |

---
## 總成交次數
**Path:** 資料欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 總成交次數 (資料欄位) |
| 語法 | ``` Value1 = GetField("總成交次數"); Value1 = GetField("TotalTicks"); ``` |
| 欄位分類 | 量能 |
| 單位 | 筆 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 分鐘 日 |
| 支援商品 | 台股 期貨 選擇權 香港股票 大陸股票 美(股票) |
| 說明 | 成交的總筆數。  資料來源為交易所盤中所傳送的成交明細的筆數。  如果分鐘頻率的話則為該分鐘統計區間內的成交明細筆數。 |

---
## 買進中單成交次數
**Path:** 資料欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 買進中單成交次數 (資料欄位) |
| 語法 | ``` Value1 = GetField("買進中單成交次數"); Value1 = GetField("BidTickCount_M"); ``` |
| 欄位分類 | 量能 |
| 單位 | 次 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 分鐘 日 |
| 支援商品 | 類股指數 台(股票) |
| 說明 | 買進中單的成交次數。  系統依照商品每筆成交的價格以及成交金額，判斷這筆成交是小單，中單，大單，或是特大單。如果是外盤成交，則為買進，內盤成交的話則為賣出。  詳細的定義請參考：https://www.xq.com.tw/台股逐筆功能行情端相關異動/。  欄位支援分鐘頻率(該分鐘區間的累計)，以及日頻率(當日開盤迄今的累計)。  如果是指數型商品的話，數值則是成分股的累計。  資料從2020年開始提供。 |

---
## 買進中單量
**Path:** 資料欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 買進中單量 (資料欄位) |
| 語法 | ``` Value1 = GetField("買進中單量"); Value1 = GetField("BidVolume_M"); ``` |
| 欄位分類 | 量能 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 分鐘 日 |
| 支援商品 | 類股指數 台(股票) |
| 說明 | 買進中單的成交量。  系統依照商品每筆成交的價格以及成交金額，判斷這筆成交是小單，中單，大單，或是特大單。如果是外盤成交，則為買進，內盤成交的話則為賣出。  詳細的定義請參考：https://www.xq.com.tw/台股逐筆功能行情端相關異動/。  欄位支援分鐘頻率(該分鐘區間的累計)，以及日頻率(當日開盤迄今的累計)。  如果是指數型商品的話，數值則是成分股的累計。  資料從2020年開始提供。 |

---
## 買進中單金額
**Path:** 資料欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 買進中單金額 (資料欄位) |
| 語法 | ``` Value1 = GetField("買進中單金額"); Value1 = GetField("BidVolumePrice_M"); ``` |
| 欄位分類 | 量能 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 分鐘 日 |
| 支援商品 | 類股指數 台(股票) |
| 說明 | 買進中單的金額。  系統依照商品每筆成交的價格以及成交金額，判斷這筆成交是小單，中單，大單，或是特大單。如果是外盤成交，則為買進，內盤成交的話則為賣出。  詳細的定義請參考：https://www.xq.com.tw/台股逐筆功能行情端相關異動/。  欄位支援分鐘頻率(該分鐘區間的累計)，以及日頻率(當日開盤迄今的累計)。  如果是指數型商品的話，數值則是成分股的累計。  資料從2020年開始提供。 |

---
## 買進大單成交次數
**Path:** 資料欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 買進大單成交次數 (資料欄位) |
| 語法 | ``` Value1 = GetField("買進大單成交次數"); Value1 = GetField("BidTickCount_L"); ``` |
| 欄位分類 | 量能 |
| 單位 | 次 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 分鐘 日 |
| 支援商品 | 類股指數 台(股票) |
| 說明 | 買進大單的成交次數。  系統依照商品每筆成交的價格以及成交金額，判斷這筆成交是小單，中單，大單，或是特大單。如果是外盤成交，則為買進，內盤成交的話則為賣出。  詳細的定義請參考：https://www.xq.com.tw/台股逐筆功能行情端相關異動/。  欄位支援分鐘頻率(該分鐘區間的累計)，以及日頻率(當日開盤迄今的累計)。  如果是指數型商品的話，數值則是成分股的累計。  資料從2020年開始提供。 |

---
## 買進大單量
**Path:** 資料欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 買進大單量 (資料欄位) |
| 語法 | ``` Value1 = GetField("買進大單量"); Value1 = GetField("BidVolume_L"); ``` |
| 欄位分類 | 量能 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 分鐘 日 |
| 支援商品 | 類股指數 台(股票) |
| 說明 | 買進大單的成交量。  系統依照商品每筆成交的價格以及成交金額，判斷這筆成交是小單，中單，大單，或是特大單。如果是外盤成交，則為買進，內盤成交的話則為賣出。  詳細的定義請參考：https://www.xq.com.tw/台股逐筆功能行情端相關異動/。  欄位支援分鐘頻率(該分鐘區間的累計)，以及日頻率(當日開盤迄今的累計)。  如果是指數型商品的話，數值則是成分股的累計。  資料從2020年開始提供。 |

---
## 買進大單金額
**Path:** 資料欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 買進大單金額 (資料欄位) |
| 語法 | ``` Value1 = GetField("買進大單金額"); Value1 = GetField("BidVolumePrice_L"); ``` |
| 欄位分類 | 量能 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 分鐘 日 |
| 支援商品 | 類股指數 台(股票) |
| 說明 | 買進大單的成交金額。  系統依照商品每筆成交的價格以及成交金額，判斷這筆成交是小單，中單，大單，或是特大單。如果是外盤成交，則為買進，內盤成交的話則為賣出。  詳細的定義請參考：https://www.xq.com.tw/台股逐筆功能行情端相關異動/。  欄位支援分鐘頻率(該分鐘區間的累計)，以及日頻率(當日開盤迄今的累計)。  如果是指數型商品的話，數值則是成分股的累計。  資料從2020年開始提供。 |

---
## 買進小單成交次數
**Path:** 資料欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 買進小單成交次數 (資料欄位) |
| 語法 | ``` Value1 = GetField("買進小單成交次數"); Value1 = GetField("BidTickCount_S"); ``` |
| 欄位分類 | 量能 |
| 單位 | 次 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 分鐘 日 |
| 支援商品 | 類股指數 台(股票) |
| 說明 | 買進小單的成交次數。  系統依照商品每筆成交的價格以及成交金額，判斷這筆成交是小單，中單，大單，或是特大單。如果是外盤成交，則為買進，內盤成交的話則為賣出。  詳細的定義請參考：https://www.xq.com.tw/台股逐筆功能行情端相關異動/。  欄位支援分鐘頻率(該分鐘區間的累計)，以及日頻率(當日開盤迄今的累計)。  如果是指數型商品的話，數值則是成分股的累計。  資料從2020年開始提供。 |

---
## 買進小單量
**Path:** 資料欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 買進小單量 (資料欄位) |
| 語法 | ``` Value1 = GetField("買進小單量"); Value1 = GetField("BidVolume_S"); ``` |
| 欄位分類 | 量能 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 分鐘 日 |
| 支援商品 | 類股指數 台(股票) |
| 說明 | 買進小單的成交量。  系統依照商品每筆成交的價格以及成交金額，判斷這筆成交是小單，中單，大單，或是特大單。如果是外盤成交，則為買進，內盤成交的話則為賣出。  詳細的定義請參考：https://www.xq.com.tw/台股逐筆功能行情端相關異動/。  欄位支援分鐘頻率(該分鐘區間的累計)，以及日頻率(當日開盤迄今的累計)。  如果是指數型商品的話，數值則是成分股的累計。  資料從2020年開始提供。 |

---
## 買進小單金額
**Path:** 資料欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 買進小單金額 (資料欄位) |
| 語法 | ``` Value1 = GetField("買進小單金額"); Value1 = GetField("BidVolumePrice_S"); ``` |
| 欄位分類 | 量能 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 分鐘 日 |
| 支援商品 | 類股指數 台(股票) |
| 說明 | 買進小單的成交金額。  系統依照商品每筆成交的價格以及成交金額，判斷這筆成交是小單，中單，大單，或是特大單。如果是外盤成交，則為買進，內盤成交的話則為賣出。  詳細的定義請參考：https://www.xq.com.tw/台股逐筆功能行情端相關異動/。  欄位支援分鐘頻率(該分鐘區間的累計)，以及日頻率(當日開盤迄今的累計)。  如果是指數型商品的話，數值則是成分股的累計。  資料從2020年開始提供。 |

---
## 買進特大單成交次數
**Path:** 資料欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 買進特大單成交次數 (資料欄位) |
| 語法 | ``` Value1 = GetField("買進特大單成交次數"); Value1 = GetField("BidTickCount_XL"); ``` |
| 欄位分類 | 量能 |
| 單位 | 次 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 分鐘 日 |
| 支援商品 | 類股指數 台(股票) |
| 說明 | 買進特大單的成交次數。  系統依照商品每筆成交的價格以及成交金額，判斷這筆成交是小單，中單，大單，或是特大單。如果是外盤成交，則為買進，內盤成交的話則為賣出。  詳細的定義請參考：https://www.xq.com.tw/台股逐筆功能行情端相關異動/。  欄位支援分鐘頻率(該分鐘區間的累計)，以及日頻率(當日開盤迄今的累計)。  如果是指數型商品的話，數值則是成分股的累計。  資料從2020年開始提供。 |

---
## 買進特大單量
**Path:** 資料欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 買進特大單量 (資料欄位) |
| 語法 | ``` Value1 = GetField("買進特大單量"); Value1 = GetField("BidVolume_XL"); ``` |
| 欄位分類 | 量能 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 分鐘 日 |
| 支援商品 | 類股指數 台(股票) |
| 說明 | 買進特大單的成交量。  系統依照商品每筆成交的價格以及成交金額，判斷這筆成交是小單，中單，大單，或是特大單。如果是外盤成交，則為買進，內盤成交的話則為賣出。  詳細的定義請參考：https://www.xq.com.tw/台股逐筆功能行情端相關異動/。  欄位支援分鐘頻率(該分鐘區間的累計)，以及日頻率(當日開盤迄今的累計)。  如果是指數型商品的話，數值則是成分股的累計。  資料從2020年開始提供。 |

---
## 買進特大單金額
**Path:** 資料欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 買進特大單金額 (資料欄位) |
| 語法 | ``` Value1 = GetField("買進特大單金額"); Value1 = GetField("BidVolumePrice_XL"); ``` |
| 欄位分類 | 量能 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 分鐘 日 |
| 支援商品 | 類股指數 台(股票) |
| 說明 | 買進特大單的成交金額。  系統依照商品每筆成交的價格以及成交金額，判斷這筆成交是小單，中單，大單，或是特大單。如果是外盤成交，則為買進，內盤成交的話則為賣出。  詳細的定義請參考：https://www.xq.com.tw/台股逐筆功能行情端相關異動/。  欄位支援分鐘頻率(該分鐘區間的累計)，以及日頻率(當日開盤迄今的累計)。  如果是指數型商品的話，數值則是成分股的累計。  資料從2020年開始提供。 |

---
## 資金流向
**Path:** 資料欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 資金流向 (資料欄位) |
| 語法 | ``` Value1 = GetField("資金流向"); Value1 = GetField("CashDirect"); ``` |
| 欄位分類 | 量能 |
| 單位 | % |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 週 月 還原日 還原週 還原月 季 半年 年 |
| 支援商品 | 台股 類股指數 |
| 說明 | 成交值占對應大盤的成交比重(％)。 單位是%，例如數值是5.6％的話，則回傳5.6。  支援台股以及類股指數，計算公式是成交值／對應大盤成交值＊100％。 |

---
## 賣出中單成交次數
**Path:** 資料欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 賣出中單成交次數 (資料欄位) |
| 語法 | ``` Value1 = GetField("賣出中單成交次數"); Value1 = GetField("AskTickCount_M"); ``` |
| 欄位分類 | 量能 |
| 單位 | 次 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 分鐘 日 |
| 支援商品 | 類股指數 台(股票) |
| 說明 | 賣出中單的成交次數。  系統依照商品每筆成交的價格以及成交金額，判斷這筆成交是小單，中單，大單，或是特大單。如果是外盤成交，則為買進，內盤成交的話則為賣出。  詳細的定義請參考：https://www.xq.com.tw/台股逐筆功能行情端相關異動/。  欄位支援分鐘頻率(該分鐘區間的累計)，以及日頻率(當日開盤迄今的累計)。  如果是指數型商品的話，數值則是成分股的累計。  資料從2020年開始提供。 |

---
## 賣出中單量
**Path:** 資料欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 賣出中單量 (資料欄位) |
| 語法 | ``` Value1 = GetField("賣出中單量"); Value1 = GetField("AskVolume_M"); ``` |
| 欄位分類 | 量能 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 分鐘 日 |
| 支援商品 | 類股指數 台(股票) |
| 說明 | 賣出中單的成交量。  系統依照商品每筆成交的價格以及成交金額，判斷這筆成交是小單，中單，大單，或是特大單。如果是外盤成交，則為買進，內盤成交的話則為賣出。  詳細的定義請參考：https://www.xq.com.tw/台股逐筆功能行情端相關異動/。  欄位支援分鐘頻率(該分鐘區間的累計)，以及日頻率(當日開盤迄今的累計)。  如果是指數型商品的話，數值則是成分股的累計。  資料從2020年開始提供。 |

---
## 賣出中單金額
**Path:** 資料欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 賣出中單金額 (資料欄位) |
| 語法 | ``` Value1 = GetField("賣出中單金額"); Value1 = GetField("AskVolumePrice_M"); ``` |
| 欄位分類 | 量能 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 分鐘 日 |
| 支援商品 | 類股指數 台(股票) |
| 說明 | 賣出中單的成交金額。  系統依照商品每筆成交的價格以及成交金額，判斷這筆成交是小單，中單，大單，或是特大單。如果是外盤成交，則為買進，內盤成交的話則為賣出。  詳細的定義請參考：https://www.xq.com.tw/台股逐筆功能行情端相關異動/。  欄位支援分鐘頻率(該分鐘區間的累計)，以及日頻率(當日開盤迄今的累計)。  如果是指數型商品的話，數值則是成分股的累計。  資料從2020年開始提供。 |

---
## 賣出大單成交次數
**Path:** 資料欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 賣出大單成交次數 (資料欄位) |
| 語法 | ``` Value1 = GetField("賣出大單成交次數"); Value1 = GetField("AskTickCount_L"); ``` |
| 欄位分類 | 量能 |
| 單位 | 次 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 分鐘 日 |
| 支援商品 | 類股指數 台(股票) |
| 說明 | 賣出大單的成交次數。  系統依照商品每筆成交的價格以及成交金額，判斷這筆成交是小單，中單，大單，或是特大單。如果是外盤成交，則為買進，內盤成交的話則為賣出。  詳細的定義請參考：https://www.xq.com.tw/台股逐筆功能行情端相關異動/。  欄位支援分鐘頻率(該分鐘區間的累計)，以及日頻率(當日開盤迄今的累計)。  如果是指數型商品的話，數值則是成分股的累計。  資料從2020年開始提供。 |

---
## 賣出大單量
**Path:** 資料欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 賣出大單量 (資料欄位) |
| 語法 | ``` Value1 = GetField("賣出大單量"); Value1 = GetField("AskVolume_L"); ``` |
| 欄位分類 | 量能 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 分鐘 日 |
| 支援商品 | 類股指數 台(股票) |
| 說明 | 賣出大單的成交量。  系統依照商品每筆成交的價格以及成交金額，判斷這筆成交是小單，中單，大單，或是特大單。如果是外盤成交，則為買進，內盤成交的話則為賣出。  詳細的定義請參考：https://www.xq.com.tw/台股逐筆功能行情端相關異動/。  欄位支援分鐘頻率(該分鐘區間的累計)，以及日頻率(當日開盤迄今的累計)。  如果是指數型商品的話，數值則是成分股的累計。  資料從2020年開始提供。 |

---
## 賣出大單金額
**Path:** 資料欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 賣出大單金額 (資料欄位) |
| 語法 | ``` Value1 = GetField("賣出大單金額"); Value1 = GetField("AskVolumePrice_L"); ``` |
| 欄位分類 | 量能 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 分鐘 日 |
| 支援商品 | 類股指數 台(股票) |
| 說明 | 賣出大單的成交金額。  系統依照商品每筆成交的價格以及成交金額，判斷這筆成交是小單，中單，大單，或是特大單。如果是外盤成交，則為買進，內盤成交的話則為賣出。  詳細的定義請參考：https://www.xq.com.tw/台股逐筆功能行情端相關異動/。  欄位支援分鐘頻率(該分鐘區間的累計)，以及日頻率(當日開盤迄今的累計)。  如果是指數型商品的話，數值則是成分股的累計。  資料從2020年開始提供。 |

---
## 賣出小單成交次數
**Path:** 資料欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 賣出小單成交次數 (資料欄位) |
| 語法 | ``` Value1 = GetField("賣出小單成交次數"); Value1 = GetField("AskTickCount_S"); ``` |
| 欄位分類 | 量能 |
| 單位 | 次 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 分鐘 日 |
| 支援商品 | 類股指數 台(股票) |
| 說明 | 賣出小單的成交次數。  系統依照商品每筆成交的價格以及成交金額，判斷這筆成交是小單，中單，大單，或是特大單。如果是外盤成交，則為買進，內盤成交的話則為賣出。  詳細的定義請參考：https://www.xq.com.tw/台股逐筆功能行情端相關異動/。  欄位支援分鐘頻率(該分鐘區間的累計)，以及日頻率(當日開盤迄今的累計)。  如果是指數型商品的話，數值則是成分股的累計。  資料從2020年開始提供。 |

---
## 賣出小單量
**Path:** 資料欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 賣出小單量 (資料欄位) |
| 語法 | ``` Value1 = GetField("賣出小單量"); Value1 = GetField("AskVolume_S"); ``` |
| 欄位分類 | 量能 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 分鐘 日 |
| 支援商品 | 類股指數 台(股票) |
| 說明 | 賣出小單的成交量。  系統依照商品每筆成交的價格以及成交金額，判斷這筆成交是小單，中單，大單，或是特大單。如果是外盤成交，則為買進，內盤成交的話則為賣出。  詳細的定義請參考：https://www.xq.com.tw/台股逐筆功能行情端相關異動/。  欄位支援分鐘頻率(該分鐘區間的累計)，以及日頻率(當日開盤迄今的累計)。  如果是指數型商品的話，數值則是成分股的累計。  資料從2020年開始提供。 |

---
## 賣出小單金額
**Path:** 資料欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 賣出小單金額 (資料欄位) |
| 語法 | ``` Value1 = GetField("賣出小單金額"); Value1 = GetField("AskVolumePrice_S"); ``` |
| 欄位分類 | 量能 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 分鐘 日 |
| 支援商品 | 類股指數 台(股票) |
| 說明 | 賣出小單的成交金額。  系統依照商品每筆成交的價格以及成交金額，判斷這筆成交是小單，中單，大單，或是特大單。如果是外盤成交，則為買進，內盤成交的話則為賣出。  詳細的定義請參考：https://www.xq.com.tw/台股逐筆功能行情端相關異動/。  欄位支援分鐘頻率(該分鐘區間的累計)，以及日頻率(當日開盤迄今的累計)。  如果是指數型商品的話，數值則是成分股的累計。  資料從2020年開始提供。 |

---
## 賣出特大單成交次數
**Path:** 資料欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 賣出特大單成交次數 (資料欄位) |
| 語法 | ``` Value1 = GetField("賣出特大單成交次數"); Value1 = GetField("AskTickCount_XL"); ``` |
| 欄位分類 | 量能 |
| 單位 | 次 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 分鐘 日 |
| 支援商品 | 類股指數 台(股票) |
| 說明 | 賣出特大單的成交次數。  系統依照商品每筆成交的價格以及成交金額，判斷這筆成交是小單，中單，大單，或是特大單。如果是外盤成交，則為買進，內盤成交的話則為賣出。  詳細的定義請參考：https://www.xq.com.tw/台股逐筆功能行情端相關異動/。  欄位支援分鐘頻率(該分鐘區間的累計)，以及日頻率(當日開盤迄今的累計)。  如果是指數型商品的話，數值則是成分股的累計。  資料從2020年開始提供。 |

---
## 賣出特大單量
**Path:** 資料欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 賣出特大單量 (資料欄位) |
| 語法 | ``` Value1 = GetField("賣出特大單量"); Value1 = GetField("AskVolume_XL"); ``` |
| 欄位分類 | 量能 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 分鐘 日 |
| 支援商品 | 類股指數 台(股票) |
| 說明 | 賣出特大單的成交量。  系統依照商品每筆成交的價格以及成交金額，判斷這筆成交是小單，中單，大單，或是特大單。如果是外盤成交，則為買進，內盤成交的話則為賣出。  詳細的定義請參考：https://www.xq.com.tw/台股逐筆功能行情端相關異動/。  欄位支援分鐘頻率(該分鐘區間的累計)，以及日頻率(當日開盤迄今的累計)。  如果是指數型商品的話，數值則是成分股的累計。  資料從2020年開始提供。 |

---
## 賣出特大單金額
**Path:** 資料欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 賣出特大單金額 (資料欄位) |
| 語法 | ``` Value1 = GetField("賣出特大單金額"); Value1 = GetField("AskVolumePrice_XL"); ``` |
| 欄位分類 | 量能 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 分鐘 日 |
| 支援商品 | 類股指數 台(股票) |
| 說明 | 賣出特大單的成交金額。  系統依照商品每筆成交的價格以及成交金額，判斷這筆成交是小單，中單，大單，或是特大單。如果是外盤成交，則為買進，內盤成交的話則為賣出。  詳細的定義請參考：https://www.xq.com.tw/台股逐筆功能行情端相關異動/。  欄位支援分鐘頻率(該分鐘區間的累計)，以及日頻率(當日開盤迄今的累計)。  如果是指數型商品的話，數值則是成分股的累計。  資料從2020年開始提供。 |

---
## 量比
**Path:** 資料欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 量比 (資料欄位) |
| 語法 | ``` Value1 = GetField("量比"); Value1 = GetField("VolumeRatio"); ``` |
| 欄位分類 | 量能 |
| 單位 | 比值 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 分鐘 日 |
| 支援商品 | 類股指數 台(股票) 香港股票 大陸股票 |
| 說明 | 當日估計量與昨量的放大比例。  計算公式：估計量／昨量。  這個數值越大表示當日的成交量可能會放大，可以當成是交易熱絡度的一個指標。 |

---
## 開盤委買
**Path:** 資料欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 開盤委買 (資料欄位) |
| 語法 | ``` Value1 = GetField("開盤委買"); Value1 = GetField("DayOpenBid"); Value1 = GetField("期權開盤委買"); ``` |
| 欄位分類 | 量能 |
| 單位 | 大盤/類股/台股:張；期貨/選擇權:口 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 |
| 支援商品 | 台股 大盤 類股指數 期貨 選擇權 |
| 說明 | 開盤時委託買進的數量。  台股的數值是開盤第一筆成交前五檔委買的數量加總。  類股商品（細產業指標、主題指標）的計算方式則是成分股的開盤委買的加總。  大盤/期貨/選擇權的數值則是交易所公布的委買委賣資料的第一筆數值。大盤的數值會扣除權證。 |

---
## 開盤委賣
**Path:** 資料欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 開盤委賣 (資料欄位) |
| 語法 | ``` Value1 = GetField("開盤委賣"); Value1 = GetField("DayOpenAsk"); Value1 = GetField("期權開盤委賣"); ``` |
| 欄位分類 | 量能 |
| 單位 | 大盤/類股/台股:張；期貨/選擇權:口 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 |
| 支援商品 | 台股 大盤 類股指數 期貨 選擇權 |
| 說明 | 開盤時委託賣出的數量。  台股的數值是開盤第一筆成交前五檔委賣的數量加總。  類股商品（細產業指標、主題指標）的計算方式則是成分股的開盤委賣的加總。  大盤/期貨/選擇權的數值則是交易所公布的委買委賣資料的第一筆數值。大盤的數值會扣除權證。 |

---
## 開盤買筆
**Path:** 資料欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 開盤買筆 (資料欄位) |
| 語法 | ``` Value1 = GetField("開盤買筆"); Value1 = GetField("OpenBidTx"); ``` |
| 欄位分類 | 量能 |
| 單位 | 筆 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 |
| 支援商品 | 大盤 期貨 選擇權 |
| 說明 | 開盤時委託買進的筆數。  大盤的數值會扣除權證。 |

---
## 開盤賣筆
**Path:** 資料欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 開盤賣筆 (資料欄位) |
| 語法 | ``` Value1 = GetField("開盤賣筆"); Value1 = GetField("OpenAskTx"); ``` |
| 欄位分類 | 量能 |
| 單位 | 筆 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 日 |
| 支援商品 | 大盤 期貨 選擇權 |
| 說明 | 開盤時委託賣出的筆數。  大盤的數值會扣除權證。 |

---
## 下一次董監改選年
**Path:** 選股欄位 > 事件

|  |  |
| --- | --- |
| 欄位名稱 | 下一次董監改選年 (選股欄位) |
| 語法 | ``` Value1 = GetField("下一次董監改選年"); Value1 = GetField("NextElectionYearOfBoardOfDirectors"); ``` |
| 欄位分類 | 事件 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 單一 |
| 支援商品 | 台股 |
| 說明 | 回傳下一次董監改選年。  欄位格式為西元年，例如2016。 |

---
## 庫藏股結束日期
**Path:** 選股欄位 > 事件

|  |  |
| --- | --- |
| 欄位名稱 | 庫藏股結束日期 (選股欄位) |
| 語法 | ``` Value1 = GetField("庫藏股結束日期"); Value1 = GetField("Stockenddate"); ``` |
| 欄位分類 | 事件 |
| 單位 | 日期 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 單一 |
| 支援商品 | 台股 |
| 說明 | 最新一次庫藏股預計買回期間的結束日期。  欄位格式為西元年月日，例如20221101。  可以檢查GetFieldDate("庫藏股結束日期")是否等於0來判斷是否有資料。 |

---
## 庫藏股開始日期
**Path:** 選股欄位 > 事件

|  |  |
| --- | --- |
| 欄位名稱 | 庫藏股開始日期 (選股欄位) |
| 語法 | ``` Value1 = GetField("庫藏股開始日期"); Value1 = GetField("Stockstartdate"); ``` |
| 欄位分類 | 事件 |
| 單位 | 日期 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 單一 |
| 支援商品 | 台股 |
| 說明 | 最新一次庫藏股預計買回期間的開始日期。  欄位格式為西元年月日，例如20221101。  可以檢查GetFieldDate("庫藏股開始日期")是否等於0來判斷是否有資料。 |

---
## 新股上市日
**Path:** 選股欄位 > 事件

|  |  |
| --- | --- |
| 欄位名稱 | 新股上市日 (選股欄位) |
| 語法 | ``` Value1 = GetField("新股上市日"); Value1 = GetField("LaunchDayOfNewShares"); ``` |
| 欄位分類 | 事件 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 單一 |
| 支援商品 | 台股 |
| 說明 | 最近一次新股上市日期。  欄位格式為西元年月日，例如20221101。  系統依照最近一次公布的股東會，除權，現增，或是減資，回傳最接近的新股上市日。 |

---
## 日期
**Path:** 選股欄位 > 事件

|  |  |
| --- | --- |
| 欄位名稱 | 日期 (選股欄位) |
| 語法 | ``` Value1 = GetField("日期"); Value1 = GetField("Date"); ``` |
| 欄位分類 | 事件 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 還原日 還原週 還原月 季 半年 年 |
| 支援商品 | 全部 |
| 說明 | K棒資料的日期。欄位格式為西元年月日，例如20221101。  語法上也可以使用Date來取得同樣的數值。 |

---
## 最後過戶日期
**Path:** 選股欄位 > 事件

|  |  |
| --- | --- |
| 欄位名稱 | 最後過戶日期 (選股欄位) |
| 語法 | ``` Value1 = GetField("最後過戶日期"); Value1 = GetField("LastTransferDay"); ``` |
| 欄位分類 | 事件 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 單一 |
| 支援商品 | 台股 |
| 說明 | 最新一期的除權息最後過戶日日期，以西元年月日來表示，當日期為2023年6月18日，回傳值為20230618。 會依計算日期取得最新的除權息最後過戶日期。 資料來源：TEJ台灣經濟新報。 |

---
## 法說會日期
**Path:** 選股欄位 > 事件

|  |  |
| --- | --- |
| 欄位名稱 | 法說會日期 (選股欄位) |
| 語法 | ``` Value1 = GetField("法說會日期"); Value1 = GetField("InvestorConferenceDate"); ``` |
| 欄位分類 | 事件 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 單一 |
| 支援商品 | 台股 |
| 說明 | 最新一期法說會日期。  欄位格式為西元年月日，例如20221101。  資料來源為台灣證券交易所。  請注意：目前只紀錄各公司每季財報說明、業績說明會，以及於證交所所舉辦的法說會的日期。其餘如投資機構所邀請的法說會，如投資論壇、投資展望說明會、台股企業日等，則不紀錄於此欄位中。 |

---
## 減資新股上市日
**Path:** 選股欄位 > 事件

|  |  |
| --- | --- |
| 欄位名稱 | 減資新股上市日 (選股欄位) |
| 語法 | ``` Value1 = GetField("減資新股上市日"); Value1 = GetField("LaunchDayOfCapitalReduction"); ``` |
| 欄位分類 | 事件 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 單一 |
| 支援商品 | 台股 |
| 說明 | 最近一次減資的新股上市日。  欄位格式為西元年月日，例如20221101。  可以檢查GetFieldDate("減資新股上市日")是否等於0來判斷是否有減資的資料。 |

---
## 減資日期
**Path:** 選股欄位 > 事件

|  |  |
| --- | --- |
| 欄位名稱 | 減資日期 (選股欄位) |
| 語法 | ``` Value1 = GetField("減資日期"); Value1 = GetField("CapitalReductionDate"); ``` |
| 欄位分類 | 事件 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 單一 |
| 支援商品 | 台股 |
| 說明 | 最近一次減資日期日期。  欄位格式為西元年月日，例如20221101。  可以檢查GetFieldDate("減資日期")是否等於0來判斷是否有減資的資料。 |

---
## 減資最後過戶日
**Path:** 選股欄位 > 事件

|  |  |
| --- | --- |
| 欄位名稱 | 減資最後過戶日 (選股欄位) |
| 語法 | ``` Value1 = GetField("減資最後過戶日"); Value1 = GetField("LastTransferDateOfCapitalReduction"); ``` |
| 欄位分類 | 事件 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 單一 |
| 支援商品 | 台股 |
| 說明 | 最近一次減資的最後過戶日。  欄位格式為西元年月日，例如20221101。  可以檢查GetFieldDate("減資最後過戶日")是否等於0來判斷是否有減資的資料。 |

---
## 減資比例
**Path:** 選股欄位 > 事件

|  |  |
| --- | --- |
| 欄位名稱 | 減資比例 (選股欄位) |
| 語法 | ``` Value1 = GetField("減資比例"); Value1 = GetField("CapitalReductionRatio"); ``` |
| 欄位分類 | 事件 |
| 單位 | % |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 單一 |
| 支援商品 | 台股 |
| 說明 | 最近一次減資的比例。  單位是％，例如20代表減資20%。  可以檢查GetFieldDate("減資比例")是否等於0來判斷是否有減資的資料。 |

---
## 現增價格
**Path:** 選股欄位 > 事件

|  |  |
| --- | --- |
| 欄位名稱 | 現增價格 (選股欄位) |
| 語法 | ``` Value1 = GetField("現增價格"); Value1 = GetField("CashIncrementAmount"); ``` |
| 欄位分類 | 事件 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 單一 |
| 支援商品 | 台股 |
| 說明 | 最新一期現金增資的發行價格。  可以檢查GetFieldDate("現增價格")是否等於0來判斷是否有現金增資的資料。 |

---
## 現增新股上市日
**Path:** 選股欄位 > 事件

|  |  |
| --- | --- |
| 欄位名稱 | 現增新股上市日 (選股欄位) |
| 語法 | ``` Value1 = GetField("現增新股上市日"); Value1 = GetField("LaunchDayOfNewSharesCashIncrement"); ``` |
| 欄位分類 | 事件 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 單一 |
| 支援商品 | 台股 |
| 說明 | 最近一次現金增資的新股上市日。  欄位格式為西元年月日，例如20221101。  可以檢查GetFieldDate("現增新股上市日")是否等於0來判斷是否有現金增資的資料。 |

---
## 現增最後過戶日
**Path:** 選股欄位 > 事件

|  |  |
| --- | --- |
| 欄位名稱 | 現增最後過戶日 (選股欄位) |
| 語法 | ``` Value1 = GetField("現增最後過戶日"); Value1 = GetField("LastTransferDayOfCashIncrement"); ``` |
| 欄位分類 | 事件 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 單一 |
| 支援商品 | 台股 |
| 說明 | 最近一次現金增資的最後過戶日。  欄位格式為西元年月日，例如20221101。  可以檢查GetFieldDate("現增最後過戶日")是否等於0來判斷是否有現金增資的資料。 |

---
## 現增比率
**Path:** 選股欄位 > 事件

|  |  |
| --- | --- |
| 欄位名稱 | 現增比率 (選股欄位) |
| 語法 | ``` Value1 = GetField("現增比率"); Value1 = GetField("RatioofCashIncrement"); ``` |
| 欄位分類 | 事件 |
| 單位 | 股 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 單一 |
| 支援商品 | 台股 |
| 說明 | 最新一期現金增資內每千股可認購新股的股數。  例如：如果每千股可以認購20股的話，則數值為20。  可以檢查GetFieldDate("現增比率")是否等於0來判斷是否有現金增資的資料。 |

---
## 現增繳款日期
**Path:** 選股欄位 > 事件

|  |  |
| --- | --- |
| 欄位名稱 | 現增繳款日期 (選股欄位) |
| 語法 | ``` Value1 = GetField("現增繳款日期"); Value1 = GetField("DateofCashIncrement"); ``` |
| 欄位分類 | 事件 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 單一 |
| 支援商品 | 台股 |
| 說明 | 最近一次現金增資的最後繳款日。  欄位格式為西元年月日，例如20221101。  可以檢查GetFieldDate("現增繳款日期")是否等於0來判斷是否有現金增資的資料。 |

---
## 現增金額
**Path:** 選股欄位 > 事件

|  |  |
| --- | --- |
| 欄位名稱 | 現增金額 (選股欄位) |
| 語法 | ``` Value1 = GetField("現增金額"); Value1 = GetField("PriceofCashIncrement"); ``` |
| 欄位分類 | 事件 |
| 單位 | 億 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 單一 |
| 支援商品 | 台股 |
| 說明 | 最新一期現金增資的發行總金額。  可以檢查GetFieldDate("現增金額")是否等於0來判斷是否有現金增資的資料。 |

---
## 股利年度
**Path:** 選股欄位 > 事件

|  |  |
| --- | --- |
| 欄位名稱 | 股利年度 (選股欄位) |
| 語法 | ``` Value1 = GetField("股利年度"); Value1 = GetField("DividendYear"); ``` |
| 欄位分類 | 事件 |
| 單位 | 年 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 年 |
| 支援商品 | 台(股票) 美(股票) 美(ETF) |
| 說明 | 回傳完整的股利年度。  欄位格式為西元年，例如2018。 |

---
## 股東會日期
**Path:** 選股欄位 > 事件

|  |  |
| --- | --- |
| 欄位名稱 | 股東會日期 (選股欄位) |
| 語法 | ``` Value1 = GetField("股東會日期"); Value1 = GetField("DateofMeetingofShareHolders"); ``` |
| 欄位分類 | 事件 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 單一 |
| 支援商品 | 台股 |
| 說明 | 最近一次股東會的日期。  欄位格式為西元年月日，例如20221101。 |

---
## 董監事就任日期
**Path:** 選股欄位 > 事件

|  |  |
| --- | --- |
| 欄位名稱 | 董監事就任日期 (選股欄位) |
| 語法 | ``` Value1 = GetField("董監事就任日期"); Value1 = GetField("DateOfBoardOfDirectorInaugurate"); ``` |
| 欄位分類 | 事件 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 單一 |
| 支援商品 | 台股 |
| 說明 | 最新一期董監事就任日期。  欄位格式為西元年月日，例如20100101。 |

---
## 處置結束日期
**Path:** 選股欄位 > 事件

|  |  |
| --- | --- |
| 欄位名稱 | 處置結束日期 (選股欄位) |
| 語法 | ``` Value1 = GetField("處置結束日期"); Value1 = GetField("DispositionED"); ``` |
| 欄位分類 | 事件 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 單一 |
| 支援商品 | 台股 台(權證) 台(可轉債) 台(特別股) |
| 說明 | 最近一次處置期間的結束日期。  欄位格式為西元年月日，例如20221101。  請注意：這個日期可能是「未來」的日期(例如已經進入處置期間，可是尚未結束)，也可能是「過去」的日期(例如已經結束處置)。  可以檢查GetFieldDate("處置結束日期")是否等於0來判斷是否有資料。 |

---
## 處置開始日期
**Path:** 選股欄位 > 事件

|  |  |
| --- | --- |
| 欄位名稱 | 處置開始日期 (選股欄位) |
| 語法 | ``` Value1 = GetField("處置開始日期"); Value1 = GetField("DispositionSD"); ``` |
| 欄位分類 | 事件 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 單一 |
| 支援商品 | 台股 台(權證) 台(可轉債) 台(特別股) |
| 說明 | 最近一次處置期間的開始日期。  欄位格式為西元年月日，例如20221101。  請注意：這個日期可能是「未來」的日期(例如剛公佈處置，可是尚未進入處置期間)，也可能是「過去」的日期(例如已經進入處置，或是已經結束處置)。  可以檢查GetFieldDate("處置開始日期")是否等於0來判斷是否有資料。 |

---
## 融券最後回補日
**Path:** 選股欄位 > 事件

|  |  |
| --- | --- |
| 欄位名稱 | 融券最後回補日 (選股欄位) |
| 語法 | ``` Value1 = GetField("融券最後回補日"); Value1 = GetField("ShortSaleFinalRecallDate"); ``` |
| 欄位分類 | 事件 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 單一 |
| 支援商品 | 台股 |
| 說明 | 最近一次融券最後回補日的日期。  欄位格式為西元年月日，例如20221101。  系統依照最近一次公布的股東會，除權息，現增，或是減資，回傳最接近的融券最後回補日。 |

---
## 除息值
**Path:** 選股欄位 > 事件

|  |  |
| --- | --- |
| 欄位名稱 | 除息值 (選股欄位) |
| 語法 | ``` Value1 = GetField("除息值"); Value1 = GetField("ExDividendValue"); ``` |
| 欄位分類 | 事件 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 單一 |
| 支援商品 | 台股 美(股票) |
| 說明 | 最近一次公佈的除息值。  單位為元(每1股配發幾元現金)。  例如：如果每股配發0.5元現金的話，則數值為0.5。  請注意：這個數值可能是即將除息的數值(例如已經公佈要除息，可是尚未除息)，也可能是過去一次除息的數值(例如已經除息，可是下次除息日尚未確定)。  可以檢查GetFieldDate("除息值")是否等於0來判斷是否有資料。 |

---
## 除息年度
**Path:** 選股欄位 > 事件

|  |  |
| --- | --- |
| 欄位名稱 | 除息年度 (選股欄位) |
| 語法 | ``` Value1 = GetField("除息年度"); Value1 = GetField("ExDividendYear"); ``` |
| 欄位分類 | 事件 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 單一 |
| 支援商品 | 台(股票) 美(股票) |
| 說明 | 最近一次公佈的除息資料所對應的股息所屬年度。  欄位格式為西元年，例如2015。  可以檢查GetFieldDate("除息年度")是否等於0來判斷是否有資料。 |

---
## 除息日期
**Path:** 選股欄位 > 事件

|  |  |
| --- | --- |
| 欄位名稱 | 除息日期 (選股欄位) |
| 語法 | ``` Value1 = GetField("除息日期"); Value1 = GetField("ExDividendDate"); ``` |
| 欄位分類 | 事件 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 單一 |
| 支援商品 | 台股 美(股票) |
| 說明 | 最近一次公佈的除息日期。  欄位格式為西元年月日，例如20150618。  請注意：這個日期可能是「未來」的日期(例如剛公佈要除息，可是尚未除息)，也可能是「過去」的日期(例如已經除完息，可是尚未公佈下次除息的日期)。  可以檢查GetFieldDate("除息日期")是否等於0來判斷是否有資料。 |

---
## 除權值
**Path:** 選股欄位 > 事件

|  |  |
| --- | --- |
| 欄位名稱 | 除權值 (選股欄位) |
| 語法 | ``` Value1 = GetField("除權值"); Value1 = GetField("ExRightValue"); ``` |
| 欄位分類 | 事件 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 單一 |
| 支援商品 | 台股 美(股票) |
| 說明 | 最近一次公佈的除權值。  台股的話單位是元(每1股配發幾股股票)，美股的話則是除權(Split)的比例。  舉例而言，在台股，如果每股配發0.5股的話(每1000股配發500股)，那數值是0.5。  在美股，如果分割的比例是1:2的話(每1股變成0.5股)，則數值為0.5，如果是2:1的話(每1股變成2股)，則數值為2。  請注意：這個數值可能是即將除權的數值(例如已經公佈要除權，可是尚未除權)，也可能是過去一次除權的數值(例如已經除權，可是下次除權日尚未確定)。  可以檢查GetFieldDate("除權值")是否等於0來判斷是否有資料。 |

---
## 除權年度
**Path:** 選股欄位 > 事件

|  |  |
| --- | --- |
| 欄位名稱 | 除權年度 (選股欄位) |
| 語法 | ``` Value1 = GetField("除權年度"); Value1 = GetField("ExRightYear"); ``` |
| 欄位分類 | 事件 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 單一 |
| 支援商品 | 台(股票) 美(股票) |
| 說明 | 最近一次公佈的除權資料所對應的股利所屬年度。  欄位格式為西元年，例如2015。  可以檢查GetFieldDate("除權年度")是否等於0來判斷是否有資料。 |

---
## 除權息值
**Path:** 選股欄位 > 事件

|  |  |
| --- | --- |
| 欄位名稱 | 除權息值 (選股欄位) |
| 語法 | ``` Value1 = GetField("除權息值"); Value1 = GetField("ExDividendRightValue"); ``` |
| 欄位分類 | 事件 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 單一 |
| 支援商品 | 台股 |
| 說明 | 最近一次公佈的除權或是除息的數值。  如果股票在同一日除權+除息的話，則回傳配發的現金與配發的股票的加總，否則則回傳最近一次的除權值，或是除息值。  僅支援台股。  例如：如果股票同一日除權1元(每1000股配發100股)且除息0.5元的話，則數值為1.5。  可以檢查GetFieldDate("除權息值")是否等於0來判斷是否有資料。 |

---
## 除權息年度
**Path:** 選股欄位 > 事件

|  |  |
| --- | --- |
| 欄位名稱 | 除權息年度 (選股欄位) |
| 語法 | ``` Value1 = GetField("除權息年度"); Value1 = GetField("ExDividendRightYear"); ``` |
| 欄位分類 | 事件 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 單一 |
| 支援商品 | 台(股票) |
| 說明 | 最近一次公佈的除權或是除息資料所對應的股利或是股息所屬年度。  欄位格式為西元年，例如2015。  如果股票在同一日除權+除息的話，則回傳這個日期，否則則回傳最近一次的除權年度，或是除息年度。  可以檢查GetFieldDate("除權息年度")是否等於0來判斷是否有資料。 |

---
## 除權息日期
**Path:** 選股欄位 > 事件

|  |  |
| --- | --- |
| 欄位名稱 | 除權息日期 (選股欄位) |
| 語法 | ``` Value1 = GetField("除權息日期"); Value1 = GetField("ExDividendRightDate"); ``` |
| 欄位分類 | 事件 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 單一 |
| 支援商品 | 台股 |
| 說明 | 最近一次公佈的除權或是除息日期。  欄位格式為西元年月日，例如20150618。  如果股票在同一日除權+除息的話，則回傳這個日期，否則則回傳最近一次的除權日期，或是除息日期。  請注意：這個日期可能是「未來」的日期(例如剛公佈要除息/除權，可是尚未除息/除權)，也可能是「過去」的日期(例如已經除完息/除完權，可是尚未公佈下次除息/除權的日期)。  可以檢查GetFieldDate("除權息日期")是否等於0來判斷是否有資料。 |

---
## 除權日期
**Path:** 選股欄位 > 事件

|  |  |
| --- | --- |
| 欄位名稱 | 除權日期 (選股欄位) |
| 語法 | ``` Value1 = GetField("除權日期"); Value1 = GetField("ExRightDate"); ``` |
| 欄位分類 | 事件 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 單一 |
| 支援商品 | 台股 美(股票) |
| 說明 | 最近一次公佈的除權日期。  欄位格式為西元年月日，例如20150618。  請注意：這個日期可能是「未來」的日期(例如剛公佈要除權，可是尚未除權)，也可能是「過去」的日期(例如已經除完權，可是尚未公佈下次除權的日期)。  可以檢查GetFieldDate("除權日期")是否等於0來判斷是否有資料。 |

---
## Jensen
**Path:** 選股欄位 > 價格

|  |  |
| --- | --- |
| 欄位名稱 | Jensen (選股欄位) |
| 語法 | ``` Value1 = GetField("Jensen"); Value1 = GetField("Jensen"); ``` |
| 欄位分類 | 價格 |
| 單位 | % |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 |
| 支援商品 | 台股 美(ETF) |
| 說明 | Jensen指標用於衡量實際的投資回報超過基於其風險水平所預期的市場回報。  計算公式為：(商品一個月的平均報酬率 - 無風險利率 - Beta值 \* (大盤同期報酬率 - 無風險利率))，用來評估個股扣除市場風險後的報酬率。  Jensen指標大於0時表示超過預期，負值則表示低於預期。 |

---
## SHARPE
**Path:** 選股欄位 > 價格

|  |  |
| --- | --- |
| 欄位名稱 | SHARPE (選股欄位) |
| 語法 | ``` Value1 = GetField("SHARPE"); Value1 = GetField("SHARPE"); ``` |
| 欄位分類 | 價格 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 |
| 支援商品 | 台股 美(ETF) |
| 說明 | Sharpe指標衡量投資相對於其風險所帶來的超額回報率，即投資回報超過無風險利率部分的表現。  計算方式為: (商品過去一個月的平均報酬率 - 無風險利率) / 過去一個月的報酬率的標準差，用來衡量每單位風險所獲得的超額報酬。 |

---
## Treynor
**Path:** 選股欄位 > 價格

|  |  |
| --- | --- |
| 欄位名稱 | Treynor (選股欄位) |
| 語法 | ``` Value1 = GetField("Treynor"); Value1 = GetField("Treynor"); ``` |
| 欄位分類 | 價格 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 |
| 支援商品 | 台股 美(ETF) |
| 說明 | Treynor指標衡量投資相對於系統性風險的回報率。  計算公式為: (商品過去一個月的平均報酬率 - 無風險利率) / 商品的Beta值，用來衡量每單位系統風險所獲得的超額報酬。 |

---
## 上游股價指標
**Path:** 選股欄位 > 價格

|  |  |
| --- | --- |
| 欄位名稱 | 上游股價指標 (選股欄位) |
| 語法 | ``` Value1 = GetField("上游股價指標"); Value1 = GetField("UpStreamPriceIndex"); ``` |
| 欄位分類 | 價格 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 |
| 支援商品 | 台股 |
| 說明 | 利用供應商的股價波動計算股價指數。  從2004年1月1日為基期100後逐日計算。  上游股價指標的計算有包含國外股票，詳細的成份內容可以參考個股的"營運結構"。 |

---
## 下游股價指標
**Path:** 選股欄位 > 價格

|  |  |
| --- | --- |
| 欄位名稱 | 下游股價指標 (選股欄位) |
| 語法 | ``` Value1 = GetField("下游股價指標"); Value1 = GetField("DownStreamPriceIndex"); ``` |
| 欄位分類 | 價格 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 |
| 支援商品 | 台股 |
| 說明 | 利用客戶的股價波動計算股價指數。  從2004年1月1日為基期100後逐日計算。  下游股價指標的計算有包含國外股票，詳細的成份內容可以參考個股的"營運結構"。 |

---
## 參考價
**Path:** 選股欄位 > 價格

|  |  |
| --- | --- |
| 欄位名稱 | 參考價 (選股欄位) |
| 語法 | ``` Value1 = GetField("參考價"); Value1 = GetField("RefPrice"); ``` |
| 欄位分類 | 價格 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 |
| 支援商品 | 台股 |
| 說明 | 當日交易的參考價。一般而言是前一交易日的收盤價，如果股票商品有除權息的話，則是除權息後的參考價。  除權息的計算公式如下(實際數值以交易所公佈為主)： 1.除息參考價為股價－股息 2.除權參考價為股價／(1+配股率) 3.同時除權息參考價為 (股價-股息)／(1+配股率) |

---
## 同業股價指標
**Path:** 選股欄位 > 價格

|  |  |
| --- | --- |
| 欄位名稱 | 同業股價指標 (選股欄位) |
| 語法 | ``` Value1 = GetField("同業股價指標"); Value1 = GetField("CompetitorsPriceIndex"); ``` |
| 欄位分類 | 價格 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 |
| 支援商品 | 台股 |
| 說明 | 利用同業競爭公司的股價波動計算股價指數。  從2004年1月1日為基期100後逐日計算。  同業競爭公司詳細的成份內容可以參考個股的"營運結構"。 |

---
## 均價
**Path:** 選股欄位 > 價格

|  |  |
| --- | --- |
| 欄位名稱 | 均價 (選股欄位) |
| 語法 | ``` Value1 = GetField("均價"); Value1 = GetField("AvgPrice"); ``` |
| 欄位分類 | 價格 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 |
| 支援商品 | 台股 大盤 類股指數 香港股票 大陸股票 |
| 說明 | 每日的平均成交價。  計算方式是累加每日開盤後每筆成交的價格乘上數量，然後除以累計的成交數量，得出當日平均的成交價。  如果是分鐘頻率的話，數值為開盤到現在為止的成交均價。如果是日頻率的話則是當日的成交均價。 |

---
## 投資建議目標價
**Path:** 選股欄位 > 價格

|  |  |
| --- | --- |
| 欄位名稱 | 投資建議目標價 (選股欄位) |
| 語法 | ``` Value1 = GetField("投資建議目標價"); Value1 = GetField("AnalystPriceTargets"); ``` |
| 欄位分類 | 價格 |
| 單位 | 美元 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 單一 |
| 支援商品 | 美(股票) |
| 說明 | 市場機構對個股的股價預估平均值。 美股股票才適用本欄位。  每個市場機構會不定期的對股票提供評等，同時也會提供預估目標價。 基於所有機構所提供的預估目標價，採平均計算後，就會得到投資目標價欄位。  請注意：   1. 此欄位會不定期的更新 2. 並不是所有的股票都會有這個欄位 |

---
## 振幅
**Path:** 選股欄位 > 價格

|  |  |
| --- | --- |
| 欄位名稱 | 振幅 (選股欄位) |
| 語法 | ``` Value1 = GetField("振幅"); Value1 = GetField("Range(％)"); ``` |
| 欄位分類 | 價格 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 季 年 |
| 支援商品 | 台股 |
| 說明 | 當期股價的振幅。單位為%。  振幅計算公式 = (當期最高價 - 當期最低價) \* 100 / 參考價%。  週頻率以上為區間統計值。 |

---
## 收盤價
**Path:** 選股欄位 > 價格

|  |  |
| --- | --- |
| 欄位名稱 | 收盤價 (選股欄位) |
| 語法 | ``` Value1 = GetField("收盤價"); Value1 = GetField("Close"); ``` |
| 欄位分類 | 價格 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 還原日 還原週 還原月 季 半年 年 |
| 支援商品 | 台股 香港股票 大陸股票 美(股票) |
| 說明 | K棒資料的收盤價(最後一個成交價位)。  語法上也可以使用Close來取得同樣的數值。 |

---
## 最低價
**Path:** 選股欄位 > 價格

|  |  |
| --- | --- |
| 欄位名稱 | 最低價 (選股欄位) |
| 語法 | ``` Value1 = GetField("最低價"); Value1 = GetField("Low"); ``` |
| 欄位分類 | 價格 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 還原日 還原週 還原月 季 半年 年 |
| 支援商品 | 台股 香港股票 大陸股票 美(股票) |
| 說明 | K棒資料的最低成交價位。  語法上也可以使用Low來取得同樣的數值。 |

---
## 最高價
**Path:** 選股欄位 > 價格

|  |  |
| --- | --- |
| 欄位名稱 | 最高價 (選股欄位) |
| 語法 | ``` Value1 = GetField("最高價"); Value1 = GetField("High"); ``` |
| 欄位分類 | 價格 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 還原日 還原週 還原月 季 半年 年 |
| 支援商品 | 台股 香港股票 大陸股票 美(股票) |
| 說明 | K棒資料的最高成交價位。  語法上也可以使用High來取得同樣的數值。 |

---
## 月平均收益率
**Path:** 選股欄位 > 價格

|  |  |
| --- | --- |
| 欄位名稱 | 月平均收益率 (選股欄位) |
| 語法 | ``` Value1 = GetField("月平均收益率"); Value1 = GetField("AvgReturnM"); ``` |
| 欄位分類 | 價格 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 |
| 支援商品 | 台股 |
| 說明 | 最近21日的平均賺賠比例。  計算公式 = 近21日漲幅 / 21，其中近21日漲幅 = (收盤價 - 21日前收盤價) / 21日前收盤價 \* 100%。  使用還原資料計算。 |

---
## 本益比
**Path:** 選股欄位 > 價格

|  |  |
| --- | --- |
| 欄位名稱 | 本益比 (選股欄位) |
| 語法 | ``` Value1 = GetField("本益比"); Value1 = GetField("PERatio"); Value1 = GetField("PE"); ``` |
| 欄位分類 | 價格 |
| 單位 | 倍 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 |
| 支援商品 | 台股 美(股票) |
| 說明 | 股票的本益比。  計算公式為收盤價／每股稅後盈餘。  台股的每股稅後盈餘＝近四季稅後盈餘／發行參考股數。實際數值以交易所公佈值為準。  美股的每股稅後盈餘＝近四季本期稅後淨利加總／最新一季的總流通在外股數。  當每股稅後盈餘為0或負值時，則不計算本益比。 |

---
## 標準差
**Path:** 選股欄位 > 價格

|  |  |
| --- | --- |
| 欄位名稱 | 標準差 (選股欄位) |
| 語法 | ``` Value1 = GetField("標準差"); Value1 = GetField("StdDev"); ``` |
| 欄位分類 | 價格 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 |
| 支援商品 | 台股 |
| 說明 | 使用一年的每日漲跌幅，計算標準差數值，用來衡量股票報酬率的波動性。數值越小表示波動性越低。 |

---
## 波動率
**Path:** 選股欄位 > 價格

|  |  |
| --- | --- |
| 欄位名稱 | 波動率 (選股欄位) |
| 語法 | ``` Value1 = GetField("波動率"); Value1 = GetField("Volatility"); ``` |
| 欄位分類 | 價格 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 |
| 支援商品 | 台股 選擇權 |
| 說明 | 選擇權／權證商品標的股的歷史波動率。 單位是％，例如數值如果是23.12％，則回傳23.12。  計算方式是使用最近20天的股價變化，算出年化的歷史波動率，以此來衡量標的股股價的波動幅度。  除了選擇權／權證商品之外，有發行衍生性商品的台股或是指數商品也都支援此欄位。  週頻率以上為統計期間的期底值。 |

---
## 漲停價
**Path:** 選股欄位 > 價格

|  |  |
| --- | --- |
| 欄位名稱 | 漲停價 (選股欄位) |
| 語法 | ``` Value1 = GetField("漲停價"); Value1 = GetField("UpLimit"); ``` |
| 欄位分類 | 價格 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 |
| 支援商品 | 台股 |
| 說明 | 當日的漲停價。 |

---
## 漲跌幅
**Path:** 選股欄位 > 價格

|  |  |
| --- | --- |
| 欄位名稱 | 漲跌幅 (選股欄位) |
| 語法 | ``` Value1 = GetField("漲跌幅"); Value1 = GetField("Change(％)"); ``` |
| 欄位分類 | 價格 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 季 年 |
| 支援商品 | 台股 香港股票 大陸股票 美(股票) |
| 說明 | 當期股價的漲跌幅。單位是％。  漲跌幅計算方式為(當期收盤價 - 當期參考價)／當期參考價 \* 100%。  週頻率以上為區間統計值。 |

---
## 真實範圍
**Path:** 選股欄位 > 價格

|  |  |
| --- | --- |
| 欄位名稱 | 真實範圍 (選股欄位) |
| 語法 | ``` Value1 = GetField("真實範圍"); Value1 = GetField("TrueRange"); Value1 = GetField("TR"); ``` |
| 欄位分類 | 價格 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 季 年 |
| 支援商品 | 台股 |
| 說明 | 股價的真實範圍(TrueRange)，數值是考慮到跳空所推算出來的股價真正震盪幅度。  計算公式：MAX(最高價 ─ 最低價，最高價 ─ 昨收價，最低價 ─ 昨收價)。  如果今日的股價跳空，而且最低點高於昨收的話，真實範圍會涵蓋跳空的缺口(最高價─昨收價)，同理如果今日股價開低，而且最高點小於昨收的話，真實範圍也會涵蓋跳空的缺口(最低價─昨收價)。 |

---
## 真實範圍波幅
**Path:** 選股欄位 > 價格

|  |  |
| --- | --- |
| 欄位名稱 | 真實範圍波幅 (選股欄位) |
| 語法 | ``` Value1 = GetField("真實範圍波幅"); Value1 = GetField("TR％"); ``` |
| 欄位分類 | 價格 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 季 年 |
| 支援商品 | 台股 |
| 說明 | 真實範圍(TrueRange) / 昨收價 \* 100%。單位為%，假如真實範圍是5，昨收價是50，則真實範圍波幅為10%，回傳值為10。 |

---
## 股價淨值比
**Path:** 選股欄位 > 價格

|  |  |
| --- | --- |
| 欄位名稱 | 股價淨值比 (選股欄位) |
| 語法 | ``` Value1 = GetField("股價淨值比"); Value1 = GetField("PBRatio"); Value1 = GetField("PB"); ``` |
| 欄位分類 | 價格 |
| 單位 | 倍 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 季 年 |
| 支援商品 | 台股 美(股票) |
| 說明 | 計算公式：收盤價／每股淨值。  週頻率以上為統計期間的期底值。 |

---
## 貝他值
**Path:** 選股欄位 > 價格

|  |  |
| --- | --- |
| 欄位名稱 | 貝他值 (選股欄位) |
| 語法 | ``` Value1 = GetField("貝他值"); Value1 = GetField("Beta"); ``` |
| 欄位分類 | 價格 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 |
| 支援商品 | 台股 美(ETF) |
| 說明 | 個股的股價變動與市場大盤指數變動的相關性。 也就是當大盤平均上漲／下跌1%時，個股的漲跌預計將以 β% 的比例相應上漲或下跌。  貝他值>1時，顯示個股的股價波動程度將高過市場。  貝他值<1時，表示個股的股價波動程度會較市場平均值穩定。 |

---
## 跌停價
**Path:** 選股欄位 > 價格

|  |  |
| --- | --- |
| 欄位名稱 | 跌停價 (選股欄位) |
| 語法 | ``` Value1 = GetField("跌停價"); Value1 = GetField("DownLimit"); ``` |
| 欄位分類 | 價格 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 |
| 支援商品 | 台股 |
| 說明 | 當日的跌停價。 |

---
## 週平均收益率
**Path:** 選股欄位 > 價格

|  |  |
| --- | --- |
| 欄位名稱 | 週平均收益率 (選股欄位) |
| 語法 | ``` Value1 = GetField("週平均收益率"); Value1 = GetField("AvgReturnW"); ``` |
| 欄位分類 | 價格 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 |
| 支援商品 | 台股 |
| 說明 | 最近5日的平均賺賠比例。  計算公式 = 近5日漲幅 / 5，其中近5日漲幅 = (收盤價 - 5日前收盤價) / 5日前收盤價 \* 100%。  使用還原資料計算。 |

---
## 開盤價
**Path:** 選股欄位 > 價格

|  |  |
| --- | --- |
| 欄位名稱 | 開盤價 (選股欄位) |
| 語法 | ``` Value1 = GetField("開盤價"); Value1 = GetField("Open"); ``` |
| 欄位分類 | 價格 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 還原日 還原週 還原月 季 半年 年 |
| 支援商品 | 台股 香港股票 大陸股票 美(股票) |
| 說明 | K棒資料的開盤價(第一個成交價位)。  語法上也可以使用Open來取得同樣的數值。 |

---
## 高低差
**Path:** 選股欄位 > 價格

|  |  |
| --- | --- |
| 欄位名稱 | 高低差 (選股欄位) |
| 語法 | ``` Value1 = GetField("高低差"); Value1 = GetField("Range"); ``` |
| 欄位分類 | 價格 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 季 年 |
| 支援商品 | 台股 |
| 說明 | 當期的最高價 － 當期的最低價。 |

---
## 公司成立日期
**Path:** 選股欄位 > 基本

|  |  |
| --- | --- |
| 欄位名稱 | 公司成立日期 (選股欄位) |
| 語法 | ``` Value1 = GetField("公司成立日期"); Value1 = GetField("FoundingDate"); ``` |
| 欄位分類 | 基本 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 單一 |
| 支援商品 | 台股 美(ETF) |
| 說明 | 公司成立日期。 欄位格式為西元年月日，例如20100101。 |

---
## 公司掛牌日期
**Path:** 選股欄位 > 基本

|  |  |
| --- | --- |
| 欄位名稱 | 公司掛牌日期 (選股欄位) |
| 語法 | ``` Value1 = GetField("公司掛牌日期"); Value1 = GetField("ListingDate"); ``` |
| 欄位分類 | 基本 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 單一 |
| 支援商品 | 台股 |
| 說明 | 公司上櫃或是上市的日期。  欄位格式為西元年月日，例如20100101。 |

---
## 公司類別
**Path:** 選股欄位 > 基本

|  |  |
| --- | --- |
| 欄位名稱 | 公司類別 (選股欄位) |
| 語法 | ``` Value1 = GetField("公司類別"); Value1 = GetField("CompanySize"); ``` |
| 欄位分類 | 基本 |
| 格式 | 字串 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 月 |
| 支援商品 | 台股 |
| 說明 | 系統運用市值，將公司分為大型股、中型股、小型股。 公司類別數值會每月重新評估。 |

---
## 公司風格
**Path:** 選股欄位 > 基本

|  |  |
| --- | --- |
| 欄位名稱 | 公司風格 (選股欄位) |
| 語法 | ``` Value1 = GetField("公司風格"); Value1 = GetField("CompanyStyle"); ``` |
| 欄位分類 | 基本 |
| 格式 | 字串 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 |
| 支援商品 | 台股 |
| 說明 | 系統運用財務數值評價，將公司分為混合型、價值型、成長型。 公司風格數值會每季重新評估。 |

---
## 公積配股
**Path:** 選股欄位 > 基本

|  |  |
| --- | --- |
| 欄位名稱 | 公積配股 (選股欄位) |
| 語法 | ``` Value1 = GetField("公積配股"); Value1 = GetField("DividendFromCapitalReserve"); ``` |
| 欄位分類 | 基本 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 年 |
| 支援商品 | 台股 |
| 說明 | 股票股利內的公積配股。 (股票股利 = 盈餘配股 + 公積配股) |

---
## 員工人數
**Path:** 選股欄位 > 基本

|  |  |
| --- | --- |
| 欄位名稱 | 員工人數 (選股欄位) |
| 語法 | ``` Value1 = GetField("員工人數"); Value1 = GetField("Numemployees"); ``` |
| 欄位分類 | 基本 |
| 單位 | 人數 |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 美(股票) |
| 說明 | 來自年報的員工總人數。  註：美股僅支援年頻率。 |

---
## 員工配股率
**Path:** 選股欄位 > 基本

|  |  |
| --- | --- |
| 欄位名稱 | 員工配股率 (選股欄位) |
| 語法 | ``` Value1 = GetField("員工配股率"); Value1 = GetField("RatioOfEmployeeBonus－Stock"); ``` |
| 欄位分類 | 基本 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 年 |
| 支援商品 | 台股 |
| 說明 | 計算公式 : 員工分紅股數 / 已發行股數\*100%。 資料來源：台灣經濟新報。 |

---
## 填息天數
**Path:** 選股欄位 > 基本

|  |  |
| --- | --- |
| 欄位名稱 | 填息天數 (選股欄位) |
| 語法 | ``` Value1 = GetField("填息天數"); Value1 = GetField("DaysForCashDividendAdjustment"); ``` |
| 欄位分類 | 基本 |
| 單位 | 天 |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 單一 |
| 支援商品 | 台股 |
| 說明 | 填息天數，指的是在股票除息後，股價從除息參考價回升至除息前收盤價所需的天數。  單位是天，例如填息天數為1天的話，則回傳1。若尚未除息的話，則回傳-2。若尚未填息的話，則回傳-1。 |

---
## 填權天數
**Path:** 選股欄位 > 基本

|  |  |
| --- | --- |
| 欄位名稱 | 填權天數 (選股欄位) |
| 語法 | ``` Value1 = GetField("填權天數"); Value1 = GetField("DaysForStockDividendAdjustment"); ``` |
| 欄位分類 | 基本 |
| 單位 | 天 |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 單一 |
| 支援商品 | 台股 |
| 說明 | 填權天數，指的是在股票除權後，股價從除權參考價回升至除權前收盤價所需的天數。  單位是天，例如填權天數為1天的話，則回傳1。若尚未除權的話，則回傳-2。若尚未填權的話，則回傳-1。 |

---
## 投資建議評級
**Path:** 選股欄位 > 基本

|  |  |
| --- | --- |
| 欄位名稱 | 投資建議評級 (選股欄位) |
| 語法 | ``` Value1 = GetField("投資建議評級"); Value1 = GetField("RecommendationRating"); ``` |
| 欄位分類 | 基本 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 單一 |
| 支援商品 | 美(股票) |
| 說明 | 市場機構對個股的投資建議評等分數。  美股股票才適用本欄位。  每個市場機構會不定期的對股票提供評等，評等分數分成５級，分別是：1=強烈買進，2=買進(Outperform)，3＝中立, 4=賣出(Underperform), 5=強烈賣出。  基於所有機構所提供的評等分數，採平均計算後，就會得到投資建議評等分數。  這個分數介於1 ~ 5之間，依照上述的評等方式，可以對應出以下的關係： 1 <= x < 1.5：強烈買進 1.5 <= x < 2.5：買進(Outperform) 2.5 <= x < 3.5：中立 3.5 <= x < 4.5：賣出(Underperform) 4.5 <= x < 5：強烈賣出  請注意：   1. 此欄位會不定期的更新 2. 並不是所有的股票都會有這個欄位 |

---
## 新產能預計量產日期
**Path:** 選股欄位 > 基本

|  |  |
| --- | --- |
| 欄位名稱 | 新產能預計量產日期 (選股欄位) |
| 語法 | ``` Value1 = GetField("新產能預計量產日期"); Value1 = GetField("DateOfNewProductionCapacity"); ``` |
| 欄位分類 | 基本 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 單一 |
| 支援商品 | 台股 |
| 說明 | 最近可能增加產能的預估量產日期。  欄位格式為西元年月日，例如20221101。  新增產能的資料來源為各公司年報或是新聞媒體上所公開的資訊，例如公司新廠預計投入營運的日期。  可以檢查GetFieldDate("新產能預計量產日期")是否等於0來判斷該公司是否有此資料。 |

---
## 月營收
**Path:** 選股欄位 > 基本

|  |  |
| --- | --- |
| 欄位名稱 | 月營收 (選股欄位) |
| 語法 | ``` Value1 = GetField("月營收"); Value1 = GetField("Sales"); ``` |
| 欄位分類 | 基本 |
| 單位 | 億 |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 月 |
| 支援商品 | 台股 |
| 說明 | 台股上市櫃公司每月公告的營業收入數值。 欄位更新日期依照各家公司公佈的時間點而異。 依交易所規定，每月10號前需公佈上月營收。若截止日遇到假日，可順延至上班日再公布。 |

---
## 月營收年增率
**Path:** 選股欄位 > 基本

|  |  |
| --- | --- |
| 欄位名稱 | 月營收年增率 (選股欄位) |
| 語法 | ``` Value1 = GetField("月營收年增率"); Value1 = GetField("Sales－YoY"); ``` |
| 欄位分類 | 基本 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 月 |
| 支援商品 | 台股 |
| 說明 | 商品當月營收較去年同月營收的成長率。 計算公式 ＝(當月營收－去年同期) / 去年同期\*100%。  例如：今年三月營收100億、去年三月營收80億，月營收年增率為(100-80)/80\*100% = 25%，單位為%， 則回傳25。 |

---
## 月營收月增率
**Path:** 選股欄位 > 基本

|  |  |
| --- | --- |
| 欄位名稱 | 月營收月增率 (選股欄位) |
| 語法 | ``` Value1 = GetField("月營收月增率"); Value1 = GetField("Sales－MoM"); ``` |
| 欄位分類 | 基本 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 月 |
| 支援商品 | 台股 |
| 說明 | 商品當月營收較上一月營收的成長率。 計算公式＝(當月營收－上月營收) / 上月營收\*100%。  例如：今年三月營收100億、同年二月營收80億，月營收月增率為(100-80)/80\*100% = 25%，單位為％，則回傳25。 |

---
## 殖利率
**Path:** 選股欄位 > 基本

|  |  |
| --- | --- |
| 欄位名稱 | 殖利率 (選股欄位) |
| 語法 | ``` Value1 = GetField("殖利率"); Value1 = GetField("Yield"); ``` |
| 欄位分類 | 基本 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 |
| 支援商品 | 台股 美(股票) 美(特別股) 美(ETF) |
| 說明 | 股票的殖利率。單位是%，例如殖利率為5.6％的話，則回傳5.6。  殖利率計算的公式如下：  台股：(最近一個完整股利年度的現金股利＋股票股利)／收盤價＊100%  美股/ETF：近1年股利／收盤價＊100%。  美國特別股：(票面利率＊每股面額)／收盤價＊100%。 |

---
## 現金股利
**Path:** 選股欄位 > 基本

|  |  |
| --- | --- |
| 欄位名稱 | 現金股利 (選股欄位) |
| 語法 | ``` Value1 = GetField("現金股利"); Value1 = GetField("CashDividend"); Value1 = GetField("股息"); ``` |
| 欄位分類 | 基本 |
| 單位 | 台股:元；美股:元(美元) |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 年 |
| 支援商品 | 台股 美(股票) 美(特別股) 美(ETF) |
| 說明 | 現金配發的股利(股息)。  統計方式依照商品類型有些許差異：  台股: 回傳最近一個完整的股利年度所配發的所有現金股利。 美股: 最近12個月(1年)的現金股息的加總。 ETF: 最近12個月(1年)的現金股息的加總。 |

---
## 現金股利佔股利比重
**Path:** 選股欄位 > 基本

|  |  |
| --- | --- |
| 欄位名稱 | 現金股利佔股利比重 (選股欄位) |
| 語法 | ``` Value1 = GetField("現金股利佔股利比重"); Value1 = GetField("CashDividend／TotalDividend"); ``` |
| 欄位分類 | 基本 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 年 |
| 支援商品 | 台股 |
| 說明 | 計算公式：現金股利 / (現金股利+股票股利) \* 100%。 |

---
## 現金股利殖利率
**Path:** 選股欄位 > 基本

|  |  |
| --- | --- |
| 欄位名稱 | 現金股利殖利率 (選股欄位) |
| 語法 | ``` Value1 = GetField("現金股利殖利率"); Value1 = GetField("CashDividendYield"); ``` |
| 欄位分類 | 基本 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 |
| 支援商品 | 台股 |
| 說明 | 現金股利相對於股價的比例，用來衡量股票投資的回報。  計算公式依股票或是ETF有些許不同：  台股股票：最近一個完整股利年度的現金股利／收盤價 \* 100%  ETF：最近12個月股利／收盤價 \* 100%。 |

---
## 發行張數_張_
**Path:** 選股欄位 > 基本

|  |  |
| --- | --- |
| 欄位名稱 | 發行張數(張) (選股欄位) |
| 語法 | ``` Value1 = GetField("發行張數(張)"); Value1 = GetField("OutStandingShares"); ``` |
| 欄位分類 | 基本 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 |
| 支援商品 | 台股 |
| 說明 | 當期的發行張數。  資料來源為交易所。 |

---
## 發行張數_萬張_
**Path:** 選股欄位 > 基本

|  |  |
| --- | --- |
| 欄位名稱 | 發行張數(萬張) (選股欄位) |
| 語法 | ``` Value1 = GetField("發行張數(萬張)"); Value1 = GetField("OutStandingShares"); Value1 = GetField("發行張數"); ``` |
| 欄位分類 | 基本 |
| 單位 | 萬張 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 季 年 |
| 支援商品 | 台股 |
| 說明 | 當期的發行張數。單位是萬張。 |

---
## 盈餘配股
**Path:** 選股欄位 > 基本

|  |  |
| --- | --- |
| 欄位名稱 | 盈餘配股 (選股欄位) |
| 語法 | ``` Value1 = GetField("盈餘配股"); Value1 = GetField("DividendFromEarning"); ``` |
| 欄位分類 | 基本 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 年 |
| 支援商品 | 台股 |
| 說明 | 股票股利內的盈餘配股項目。 (股票股利 = 盈餘配股 + 公積配股) |

---
## 累計營收
**Path:** 選股欄位 > 基本

|  |  |
| --- | --- |
| 欄位名稱 | 累計營收 (選股欄位) |
| 語法 | ``` Value1 = GetField("累計營收"); Value1 = GetField("AccSales"); ``` |
| 欄位分類 | 基本 |
| 單位 | 億 |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 月 |
| 支援商品 | 台股 |
| 說明 | 年初迄今公告的月營收累計總金額。  例如, 一月月營收5億, 二月月營收, 5億, 三月月營收5億，三月的累計營收為15億。 |

---
## 累計營收年增率
**Path:** 選股欄位 > 基本

|  |  |
| --- | --- |
| 欄位名稱 | 累計營收年增率 (選股欄位) |
| 語法 | ``` Value1 = GetField("累計營收年增率"); Value1 = GetField("AccSales－YoY"); ``` |
| 欄位分類 | 基本 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 月 |
| 支援商品 | 台股 |
| 說明 | 商品今年以來累計營收較去年同期累計營收的成長率。 計算公式 = (今年以來累計營收 - 去年同期累計營收) / 去年同期累計營收 \* 100%。  例如：今年三月累計營收100億、去年三月累計營收80億，累計營收年增率為(100-80)/80\*100% = 25%，單位為％，則回傳25。 |

---
## 總市值_億_
**Path:** 選股欄位 > 基本

|  |  |
| --- | --- |
| 欄位名稱 | 總市值(億) (選股欄位) |
| 語法 | ``` Value1 = GetField("總市值(億)"); Value1 = GetField("MarketCapin100Million"); Value1 = GetField("總市值"); ``` |
| 欄位分類 | 基本 |
| 單位 | 億元 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 季 年 |
| 支援商品 | 台股 類股指數 |
| 說明 | 股票或是指數的市值，換算成億元。股票市值以發行股數 ＊ 收盤價來計算，指數則是成分股的市值加總。  週頻率以上為統計期間的期底值。 |

---
## 總市值_元_
**Path:** 選股欄位 > 基本

|  |  |
| --- | --- |
| 欄位名稱 | 總市值(元) (選股欄位) |
| 語法 | ``` Value1 = GetField("總市值(元)"); Value1 = GetField("TotalMarketValue"); ``` |
| 欄位分類 | 基本 |
| 單位 | 台股:元；美股:元(美元) |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 季 年 |
| 支援商品 | 台股 類股指數 美(股票) |
| 說明 | 股票或是指數的市值。股票市值以發行股數 ＊ 收盤價來計算，指數則是成分股的市值加總。單位為元。  週以上頻率，是取期底值。 |

---
## 總經理
**Path:** 選股欄位 > 基本

|  |  |
| --- | --- |
| 欄位名稱 | 總經理 (選股欄位) |
| 語法 | ``` Value1 = GetField("總經理"); Value1 = GetField("GeneralManager"); ``` |
| 欄位分類 | 基本 |
| 格式 | 字串 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 單一 |
| 支援商品 | 台股 |
| 說明 | 目前總經理的姓名。 |

---
## 股利合計
**Path:** 選股欄位 > 基本

|  |  |
| --- | --- |
| 欄位名稱 | 股利合計 (選股欄位) |
| 語法 | ``` Value1 = GetField("股利合計"); Value1 = GetField("TotalDividend"); ``` |
| 欄位分類 | 基本 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 年 |
| 支援商品 | 台股 |
| 說明 | 最近一個完整股利年度的現金股利＋股票股利。 |

---
## 股本_億_
**Path:** 選股欄位 > 基本

|  |  |
| --- | --- |
| 欄位名稱 | 股本(億) (選股欄位) |
| 語法 | ``` Value1 = GetField("股本(億)"); Value1 = GetField("Capital-Lastest"); Value1 = GetField("最新股本"); ``` |
| 欄位分類 | 基本 |
| 單位 | 億元 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 |
| 支援商品 | 台股 |
| 說明 | 股票的股本，換算成億元。 |

---
## 股本_元_
**Path:** 選股欄位 > 基本

|  |  |
| --- | --- |
| 欄位名稱 | 股本(元) (選股欄位) |
| 語法 | ``` Value1 = GetField("股本(元)"); Value1 = GetField("CurrentCapital"); ``` |
| 欄位分類 | 基本 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 |
| 支援商品 | 台股 |
| 說明 | 股票的股本，換算成元。 |

---
## 股票股利
**Path:** 選股欄位 > 基本

|  |  |
| --- | --- |
| 欄位名稱 | 股票股利 (選股欄位) |
| 語法 | ``` Value1 = GetField("股票股利"); Value1 = GetField("StockDividend"); ``` |
| 欄位分類 | 基本 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 年 |
| 支援商品 | 台股 |
| 說明 | 最近一個完整股利年度的股票股利。 |

---
## 股票股利佔股利比重
**Path:** 選股欄位 > 基本

|  |  |
| --- | --- |
| 欄位名稱 | 股票股利佔股利比重 (選股欄位) |
| 語法 | ``` Value1 = GetField("股票股利佔股利比重"); Value1 = GetField("StockDividend／TotalDividend"); ``` |
| 欄位分類 | 基本 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 年 |
| 支援商品 | 台股 |
| 說明 | 計算公式：股票股利 / (現金股利+股票股利) \*100%。 |

---
## 股票股利殖利率
**Path:** 選股欄位 > 基本

|  |  |
| --- | --- |
| 欄位名稱 | 股票股利殖利率 (選股欄位) |
| 語法 | ``` Value1 = GetField("股票股利殖利率"); Value1 = GetField("StockDividendYield"); ``` |
| 欄位分類 | 基本 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 |
| 支援商品 | 台股 |
| 說明 | 股票股利(配股)相對於股價的比例。  計算公式：(最近一年股票股利／收盤價) \* 100%。 |

---
## 董事長
**Path:** 選股欄位 > 基本

|  |  |
| --- | --- |
| 欄位名稱 | 董事長 (選股欄位) |
| 語法 | ``` Value1 = GetField("董事長"); Value1 = GetField("Chairman"); ``` |
| 欄位分類 | 基本 |
| 格式 | 字串 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 單一 |
| 支援商品 | 台股 |
| 說明 | 目前董事長的姓名。 |

---
## 財報股本_億_
**Path:** 選股欄位 > 基本

|  |  |
| --- | --- |
| 欄位名稱 | 財報股本(億) (選股欄位) |
| 語法 | ``` Value1 = GetField("財報股本(億)"); Value1 = GetField("CurrentCapitalin100Million"); Value1 = GetField("億元股本"); Value1 = GetField("加權平均股本"); ``` |
| 欄位分類 | 基本 |
| 單位 | 億元 |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 半年 年 |
| 支援商品 | 台股 |
| 說明 | 公司財報(季、年)上載明的股本，換算成億元。 |

---
## 主力買賣超張數
**Path:** 選股欄位 > 常用

|  |  |
| --- | --- |
| 欄位名稱 | 主力買賣超張數 (選股欄位) |
| 語法 | ``` Value1 = GetField("主力買賣超張數"); Value1 = GetField("LeaderDifference"); ``` |
| 欄位分類 | 常用 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 |
| 支援商品 | 台股 |
| 說明 | 主力買張 ─ 主力賣張。  系統每日統計券商分點對個股的買賣記錄，找出買超(買進張數─賣出張數)的前15大分點，加總這15個分點的買超張數，稱之為主力買張。同時也找出賣超(賣出張數─買進張數)的前15大分點，加總這15個分點的賣超張數，稱之為主力賣張。  主力買賣超，就是主力買張 ─ 主力賣張。  類股商品（細產業指標、主題指標）計算方式為成分股加總。  週頻率以上為期間累計值。 |

---
## 均價
**Path:** 選股欄位 > 常用

|  |  |
| --- | --- |
| 欄位名稱 | 均價 (選股欄位) |
| 語法 | ``` Value1 = GetField("均價"); Value1 = GetField("AvgPrice"); ``` |
| 欄位分類 | 常用 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 |
| 支援商品 | 台股 大盤 類股指數 香港股票 大陸股票 |
| 說明 | 每日的平均成交價。  計算方式是累加每日開盤後每筆成交的價格乘上數量，然後除以累計的成交數量，得出當日平均的成交價。  如果是分鐘頻率的話，數值為開盤到現在為止的成交均價。如果是日頻率的話則是當日的成交均價。 |

---
## 成交量
**Path:** 選股欄位 > 常用

|  |  |
| --- | --- |
| 欄位名稱 | 成交量 (選股欄位) |
| 語法 | ``` Value1 = GetField("成交量"); Value1 = GetField("Volume"); ``` |
| 欄位分類 | 常用 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 季 半年 年 |
| 支援商品 | 台股 香港股票 大陸股票 美(股票) |
| 說明 | K棒資料的成交量。  數值單位依照商品類型而異，例如台股的話成交量的單位是張，可是美股的話成交量的單位則是股。  語法上也可以使用Volume來取得同樣的數值。 |

---
## 成交金額_億_
**Path:** 選股欄位 > 常用

|  |  |
| --- | --- |
| 欄位名稱 | 成交金額(億) (選股欄位) |
| 語法 | ``` Value1 = GetField("成交金額(億)"); Value1 = GetField("TradeValue"); Value1 = GetField("成交金額"); ``` |
| 欄位分類 | 常用 |
| 單位 | 億元 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 季 年 |
| 支援商品 | 台股 類股指數 |
| 說明 | 成交的加總金額，換算成億。  週頻率以上為區間累計值。 |

---
## 收盤價
**Path:** 選股欄位 > 常用

|  |  |
| --- | --- |
| 欄位名稱 | 收盤價 (選股欄位) |
| 語法 | ``` Value1 = GetField("收盤價"); Value1 = GetField("Close"); ``` |
| 欄位分類 | 常用 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 還原日 還原週 還原月 季 半年 年 |
| 支援商品 | 台股 香港股票 大陸股票 美(股票) |
| 說明 | K棒資料的收盤價(最後一個成交價位)。  語法上也可以使用Close來取得同樣的數值。 |

---
## 最低價
**Path:** 選股欄位 > 常用

|  |  |
| --- | --- |
| 欄位名稱 | 最低價 (選股欄位) |
| 語法 | ``` Value1 = GetField("最低價"); Value1 = GetField("Low"); ``` |
| 欄位分類 | 常用 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 還原日 還原週 還原月 季 半年 年 |
| 支援商品 | 台股 香港股票 大陸股票 美(股票) |
| 說明 | K棒資料的最低成交價位。  語法上也可以使用Low來取得同樣的數值。 |

---
## 最高價
**Path:** 選股欄位 > 常用

|  |  |
| --- | --- |
| 欄位名稱 | 最高價 (選股欄位) |
| 語法 | ``` Value1 = GetField("最高價"); Value1 = GetField("High"); ``` |
| 欄位分類 | 常用 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 還原日 還原週 還原月 季 半年 年 |
| 支援商品 | 台股 香港股票 大陸股票 美(股票) |
| 說明 | K棒資料的最高成交價位。  語法上也可以使用High來取得同樣的數值。 |

---
## 月營收
**Path:** 選股欄位 > 常用

|  |  |
| --- | --- |
| 欄位名稱 | 月營收 (選股欄位) |
| 語法 | ``` Value1 = GetField("月營收"); Value1 = GetField("Sales"); ``` |
| 欄位分類 | 常用 |
| 單位 | 億 |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 月 |
| 支援商品 | 台股 |
| 說明 | 台股上市櫃公司每月公告的營業收入數值。 欄位更新日期依照各家公司公佈的時間點而異。 依交易所規定，每月10號前需公佈上月營收。若截止日遇到假日，可順延至上班日再公布。 |

---
## 本益比
**Path:** 選股欄位 > 常用

|  |  |
| --- | --- |
| 欄位名稱 | 本益比 (選股欄位) |
| 語法 | ``` Value1 = GetField("本益比"); Value1 = GetField("PERatio"); Value1 = GetField("PE"); ``` |
| 欄位分類 | 常用 |
| 單位 | 倍 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 |
| 支援商品 | 台股 美(股票) |
| 說明 | 股票的本益比。  計算公式為收盤價／每股稅後盈餘。  台股的每股稅後盈餘＝近四季稅後盈餘／發行參考股數。實際數值以交易所公佈值為準。  美股的每股稅後盈餘＝近四季本期稅後淨利加總／最新一季的總流通在外股數。  當每股稅後盈餘為0或負值時，則不計算本益比。 |

---
## 殖利率
**Path:** 選股欄位 > 常用

|  |  |
| --- | --- |
| 欄位名稱 | 殖利率 (選股欄位) |
| 語法 | ``` Value1 = GetField("殖利率"); Value1 = GetField("Yield"); ``` |
| 欄位分類 | 常用 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 |
| 支援商品 | 台股 美(股票) 美(特別股) 美(ETF) |
| 說明 | 股票的殖利率。單位是%，例如殖利率為5.6％的話，則回傳5.6。  殖利率計算的公式如下：  台股：(最近一個完整股利年度的現金股利＋股票股利)／收盤價＊100%  美股/ETF：近1年股利／收盤價＊100%。  美國特別股：(票面利率＊每股面額)／收盤價＊100%。 |

---
## 每股稅後淨利_元_
**Path:** 選股欄位 > 常用

|  |  |
| --- | --- |
| 欄位名稱 | 每股稅後淨利(元) (選股欄位) |
| 語法 | ``` Value1 = GetField("每股稅後淨利(元)"); Value1 = GetField("EPS"); Value1 = GetField("EarningPerShare"); ``` |
| 欄位分類 | 常用 |
| 單位 | 台股：元。美股：元(美元) |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 美(股票) |
| 說明 | 台股，是損益表上的「每股盈餘」項目。 美股，是損益表上的「基本每股盈餘(由淨利計算)」項目。 提供單季與年度資料。 |

---
## 法人買賣超張數
**Path:** 選股欄位 > 常用

|  |  |
| --- | --- |
| 欄位名稱 | 法人買賣超張數 (選股欄位) |
| 語法 | ``` Value1 = GetField("法人買賣超張數"); Value1 = GetField("InvestorDifference"); ``` |
| 欄位分類 | 常用 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 |
| 支援商品 | 台股 類股指數 |
| 說明 | 三大法人(外資、投信、自營商)合計買賣超張數。  台股商品的數值為法人買張 ─ 法人賣張。 類股商品（細產業指標、主題指標）的數值則是成分股的法人買賣超的加總。 週頻率以上為期間累計值。 |

---
## 漲跌幅
**Path:** 選股欄位 > 常用

|  |  |
| --- | --- |
| 欄位名稱 | 漲跌幅 (選股欄位) |
| 語法 | ``` Value1 = GetField("漲跌幅"); Value1 = GetField("Change(％)"); ``` |
| 欄位分類 | 常用 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 季 年 |
| 支援商品 | 台股 香港股票 大陸股票 美(股票) |
| 說明 | 當期股價的漲跌幅。單位是％。  漲跌幅計算方式為(當期收盤價 - 當期參考價)／當期參考價 \* 100%。  週頻率以上為區間統計值。 |

---
## 營業毛利率
**Path:** 選股欄位 > 常用

|  |  |
| --- | --- |
| 欄位名稱 | 營業毛利率 (選股欄位) |
| 語法 | ``` Value1 = GetField("營業毛利率"); Value1 = GetField("GrossMargin"); ``` |
| 欄位分類 | 常用 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 美(股票) |
| 說明 | 財務比率表上的「營業毛利率」欄位。 提供單季與年度資料。 不提供此欄位的公司，輸出值會以零表示。 單位是％，例如：營業毛利率為30%，則回傳30。 |

---
## 總市值_億_
**Path:** 選股欄位 > 常用

|  |  |
| --- | --- |
| 欄位名稱 | 總市值(億) (選股欄位) |
| 語法 | ``` Value1 = GetField("總市值(億)"); Value1 = GetField("MarketCapin100Million"); Value1 = GetField("總市值"); ``` |
| 欄位分類 | 常用 |
| 單位 | 億元 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 季 年 |
| 支援商品 | 台股 類股指數 |
| 說明 | 股票或是指數的市值，換算成億元。股票市值以發行股數 ＊ 收盤價來計算，指數則是成分股的市值加總。  週頻率以上為統計期間的期底值。 |

---
## 股價淨值比
**Path:** 選股欄位 > 常用

|  |  |
| --- | --- |
| 欄位名稱 | 股價淨值比 (選股欄位) |
| 語法 | ``` Value1 = GetField("股價淨值比"); Value1 = GetField("PBRatio"); Value1 = GetField("PB"); ``` |
| 欄位分類 | 常用 |
| 單位 | 倍 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 季 年 |
| 支援商品 | 台股 美(股票) |
| 說明 | 計算公式：收盤價／每股淨值。  週頻率以上為統計期間的期底值。 |

---
## 股東權益報酬率
**Path:** 選股欄位 > 常用

|  |  |
| --- | --- |
| 欄位名稱 | 股東權益報酬率 (選股欄位) |
| 語法 | ``` Value1 = GetField("股東權益報酬率"); Value1 = GetField("ROE"); Value1 = GetField("ReturnOnEquity"); ``` |
| 欄位分類 | 常用 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 美(股票) |
| 說明 | 台股，是財務比率表的「ROE(A)─稅後」項目。 美股，是財務比率表上的「股東權益報酬率」項目。 提供單季與年度資料。 單位為%，例如：股東權益報酬率為30%，則回傳30。 |

---
## 轉換價格
**Path:** 選股欄位 > 常用

|  |  |
| --- | --- |
| 欄位名稱 | 轉換價格 (選股欄位) |
| 語法 | ``` Value1 = GetField("轉換價格"); Value1 = GetField("ConversionPrice"); ``` |
| 欄位分類 | 常用 |
| 單位 |  |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 單一 |
| 支援商品 | 台(可轉債) |
| 說明 |  |

---
## 開盤價
**Path:** 選股欄位 > 常用

|  |  |
| --- | --- |
| 欄位名稱 | 開盤價 (選股欄位) |
| 語法 | ``` Value1 = GetField("開盤價"); Value1 = GetField("Open"); ``` |
| 欄位分類 | 常用 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 還原日 還原週 還原月 季 半年 年 |
| 支援商品 | 台股 香港股票 大陸股票 美(股票) |
| 說明 | K棒資料的開盤價(第一個成交價位)。  語法上也可以使用Open來取得同樣的數值。 |

---
## CB剩餘張數
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | CB剩餘張數 (選股欄位) |
| 語法 | ``` Value1 = GetField("CB剩餘張數"); Value1 = GetField("CBConversionAmount"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 週 月 季 半年 年 |
| 支援商品 | 台(可轉債) |
| 說明 | 目前流通在外的CB張數（就是還沒有轉換的）季底值，每週更新。 |

---
## ETF規模
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | ETF規模 (選股欄位) |
| 語法 | ``` Value1 = GetField("ETF規模"); Value1 = GetField("ETFSize"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 台股:元；美股:元(美元) |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 月 |
| 支援商品 | 台股 美(ETF) |
| 說明 | ETF的規模，單位是元/美元. |

---
## 主力平均買超成本
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 主力平均買超成本 (選股欄位) |
| 語法 | ``` Value1 = GetField("主力平均買超成本"); Value1 = GetField("AvgCostLeadertotalbuy"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 |
| 支援商品 | 台股 |
| 說明 | 前十五大買超券商的買超成本。 計算公式: 買超金額/買超張數。 |

---
## 主力平均賣超成本
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 主力平均賣超成本 (選股欄位) |
| 語法 | ``` Value1 = GetField("主力平均賣超成本"); Value1 = GetField("AvgCostLeadertotalSell"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 |
| 支援商品 | 台股 |
| 說明 | 前十五大賣超券商的賣超成本。 計算公式: 賣超金額/賣超張數。 |

---
## 主力成本
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 主力成本 (選股欄位) |
| 語法 | ``` Value1 = GetField("主力成本"); Value1 = GetField("LeaderCost"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 |
| 支援商品 | 台股 |
| 說明 | 系統估計的長期主力持股成本。  透過每日的主力買賣超以及買進賣出成本，系統推估出主力目前的持股成本。 |

---
## 主力持股
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 主力持股 (選股欄位) |
| 語法 | ``` Value1 = GetField("主力持股"); Value1 = GetField("LeaderSharesHeld"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 |
| 支援商品 | 台股 |
| 說明 | 系統估計的長期主力持股數量。  透過每日的主力買賣超，系統推估出主力目前的持股總數量。 |

---
## 主力買張
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 主力買張 (選股欄位) |
| 語法 | ``` Value1 = GetField("主力買張"); Value1 = GetField("Leadertotalbuy"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 |
| 支援商品 | 台股 |
| 說明 | 前十五大買超分點的買超張數加總。  系統每日統計券商分點對個股的買賣記錄，依照買超張數(買進張數 - 賣出張數)排序，找出前15大分點，然後把這15個分點對這檔個股的買超張數加總，計算出主力買張這個數值。  週頻率以上為期間累計值。 |

---
## 主力買賣超張數
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 主力買賣超張數 (選股欄位) |
| 語法 | ``` Value1 = GetField("主力買賣超張數"); Value1 = GetField("LeaderDifference"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 |
| 支援商品 | 台股 |
| 說明 | 主力買張 ─ 主力賣張。  系統每日統計券商分點對個股的買賣記錄，找出買超(買進張數─賣出張數)的前15大分點，加總這15個分點的買超張數，稱之為主力買張。同時也找出賣超(賣出張數─買進張數)的前15大分點，加總這15個分點的賣超張數，稱之為主力賣張。  主力買賣超，就是主力買張 ─ 主力賣張。  類股商品（細產業指標、主題指標）計算方式為成分股加總。  週頻率以上為期間累計值。 |

---
## 主力賣張
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 主力賣張 (選股欄位) |
| 語法 | ``` Value1 = GetField("主力賣張"); Value1 = GetField("Leadertotalsell"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 |
| 支援商品 | 台股 |
| 說明 | 前十五大賣超分點的賣超張數加總。  系統每日統計券商分點對個股的買賣記錄，依照賣超張數(賣出張數 - 買進張數)排序，找出前15大分點，然後把這15個分點對這檔個股的賣超張數加總，計算出主力賣張這個數值。  週頻率以上為期間累計值。 |

---
## 借券張數
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 借券張數 (選股欄位) |
| 語法 | ``` Value1 = GetField("借券張數"); Value1 = GetField("SBLBorrowing"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 |
| 支援商品 | 台股 |
| 說明 | 借券的張數。  週頻率以上為期間累計值。  借券數量係指當日投資人透過證交所借券中心或向證券商、證金公司借入之證券數量。這個數量並不一定等於借券放空的數量。 |

---
## 借券賣出張數
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 借券賣出張數 (選股欄位) |
| 語法 | ``` Value1 = GetField("借券賣出張數"); Value1 = GetField("SBUnits"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 |
| 支援商品 | 台股 |
| 說明 | 借券內實際賣出(放空)的張數。  週頻率以上為期間累計值。 |

---
## 借券賣出還券張數
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 借券賣出還券張數 (選股欄位) |
| 語法 | ``` Value1 = GetField("借券賣出還券張數"); Value1 = GetField("SBStcokDiff"); Value1 = GetField("借券賣出庫存異動張數"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 |
| 支援商品 | 台股 |
| 說明 | 償還先前借券賣出的張數。  週頻率以上為期間累計值。  請注意：這個欄位使用負值來代表償還的動作，所以如果要計算當日借券賣出餘額的異動的話，請使用借券賣出張數＋借券賣出還券張數。 |

---
## 借券賣出餘額張數
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 借券賣出餘額張數 (選股欄位) |
| 語法 | ``` Value1 = GetField("借券賣出餘額張數"); Value1 = GetField("SBRemain"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 |
| 支援商品 | 台股 |
| 說明 | 借券餘額張數，也就是到目前為止借券的淨張數。  借券賣出餘額約等於前一日的借券賣出餘額＋當日的借券賣出張數＋當日的借券賣出還券張數。  請注意：上面公式內會加上「借券賣出還券張數」的原因是因為這欄位是負值。  週頻率以上為統計期間的期底值。 |

---
## 借券餘額張數
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 借券餘額張數 (選股欄位) |
| 語法 | ``` Value1 = GetField("借券餘額張數"); Value1 = GetField("SBLbalance"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 |
| 支援商品 | 台股 |
| 說明 | 借券餘額張數，也就是到目前為止借券的淨張數。  借券餘額約等於前一日的借券餘額＋當日的借券張數─當日的還券張數。  借券餘額的增加，代表著有越來越多人準備放空。可是是否有實際放空行為，必須觀察「借券賣出餘額」欄位的變化。  週頻率以上為統計期間的期底值。 |

---
## 內部人持股
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 內部人持股 (選股欄位) |
| 語法 | ``` Value1 = GetField("內部人持股"); Value1 = GetField("InsiderHodings"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 股 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 |
| 支援商品 | 美(股票) |
| 說明 | 依美國證券交易委員會規定公告的「FORM 4 」所計算出的持股異動內部人的總持有股數。 美股股票才適用本欄位。  依照SEC的規定，公司內部人(包括董事、經理人與持股超過10%的股東)買賣公司股票後必須在2日內申報。  請注意：   1. 此欄位的日期是實際股票買賣的日期，可是因為申報時間，可能會延後公佈。 2. 由於內部人可能透過其他方式取得持股，例如Warrant的轉換等，所以這個欄位的數值未必會等於前一期的內部人持股加上當期的內部人的持股異動。 |

---
## 內部人持股張數
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 內部人持股張數 (選股欄位) |
| 語法 | ``` Value1 = GetField("內部人持股張數"); Value1 = GetField("Insidersharesheld"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 月 |
| 支援商品 | 台股 |
| 說明 | 內部人= 台灣經濟新報所提供的所有董監持股資料 – 大股東。不重複計算股數 (即剃除重複名單/法人名稱資料)。每月更新。依查詢頻率取期底值。 |

---
## 內部人持股比例
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 內部人持股比例 (選股欄位) |
| 語法 | ``` Value1 = GetField("內部人持股比例"); Value1 = GetField("Insidersharesheldratio"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 月 |
| 支援商品 | 台股 |
| 說明 | 公司內部人持股占公司股本的比例。  計算公式為內部人持股張數／公司股本＊100%。  內部人統計的範圍包含董監以及公司內部的經理人。  資料每月更新一次。 |

---
## 內部人持股異動
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 內部人持股異動 (選股欄位) |
| 語法 | ``` Value1 = GetField("內部人持股異動"); Value1 = GetField("InsiderHodingsDiff"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 股 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 |
| 支援商品 | 美(股票) |
| 說明 | 依美國證券交易委員會規定公告的「FORM 4 」所計算出的公司內部人總異動股數。正值表示買入總股數，負值表示賣出總股數。  如果是週／月頻率的話，則是當週／當月內每一日內部人總異動股數的加總。  美股股票才適用本欄位。  依照SEC的規定，公司內部人(包括董事、經理人與持股超過10%的股東)買賣公司股票後必須在2日內申報。請注意此欄位的日期是實際股票買賣的日期，可是因為申報時間，可能會延後公佈。 |

---
## 公積及其他佔股本比重
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 公積及其他佔股本比重 (選股欄位) |
| 語法 | ``` Value1 = GetField("公積及其他佔股本比重"); Value1 = GetField("CapitalReserveOther／Capital"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 年 |
| 支援商品 | 台股 |
| 說明 | 股本形成中，公積及其他來源所佔的比重。 計算公式 : 公積及其他來源／總股本金額 \* 100%。 |

---
## 分公司交易家數
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 分公司交易家數 (選股欄位) |
| 語法 | ``` Value1 = GetField("分公司交易家數"); Value1 = GetField("BranchesOfTraded"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 家 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 |
| 支援商品 | 台股 類股指數 |
| 說明 | 統計有買進或是賣出的券商分點家數。  類股商品（細產業指標、主題指標）計算方式為統計成份股內有買進或是賣出的分點的集合家數。例如成份股#1的交易分點為a, b, 成份股#2的交易分點為b, c, 則類股商品的交易家數則為3。 |

---
## 分公司淨買超金額家數
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 分公司淨買超金額家數 (選股欄位) |
| 語法 | ``` Value1 = GetField("分公司淨買超金額家數"); Value1 = GetField("BranchesOfNetBuyers"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 家 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 |
| 支援商品 | 台股 類股指數 |
| 說明 | 統計買超的券商分點家數。  買超的定義是買進金額 - 賣出金額 > 0。  類股商品（細產業指標、主題指標）計算方式為統計成份股內符合買超的分點的集合家數。例如成份股#1的買超分點為a, b, 成份股#2的買超分點為b, c, 則類股商品的買超家數則為3。 |

---
## 分公司淨賣超金額家數
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 分公司淨賣超金額家數 (選股欄位) |
| 語法 | ``` Value1 = GetField("分公司淨賣超金額家數"); Value1 = GetField("BranchesOfNetSellers"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 家 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 |
| 支援商品 | 台股 類股指數 |
| 說明 | 統計賣超的券商分點家數。  賣超的定義是賣出金額 - 買進金額 > 0。  類股商品（細產業指標、主題指標）計算方式為統計成份股內符合賣超的分點的集合家數。例如成份股#1的賣超分點為a, b, 成份股#2的賣超分點為b, c, 則類股商品的賣超家數則為3。 |

---
## 分公司買進家數
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 分公司買進家數 (選股欄位) |
| 語法 | ``` Value1 = GetField("分公司買進家數"); Value1 = GetField("LongBranches"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 家 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 |
| 支援商品 | 台股 |
| 說明 | 統計買進金額大於零的券商分點家數。  請注意：只要有買進的券商分點就會納入統計，與該分點是否買超無關。 |

---
## 分公司賣出家數
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 分公司賣出家數 (選股欄位) |
| 語法 | ``` Value1 = GetField("分公司賣出家數"); Value1 = GetField("ShortBranches"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 家 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 |
| 支援商品 | 台股 |
| 說明 | 統計賣出金額大於零的券商分點家數。  請注意：只要有賣出的券商分點就會納入統計，與該分點是否賣超無關。 |

---
## 券資比
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 券資比 (選股欄位) |
| 語法 | ``` Value1 = GetField("券資比"); Value1 = GetField("ShortSale／Margin"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 |
| 支援商品 | 台股 類股指數 |
| 說明 | 融券餘額與融資餘額的比例。計算公式：融券餘額張數 / 融資餘額張數 \* 100%。  單位是％，例如券資比為10%的話，則回傳10。  類股商品（細產業指標、主題指標）的融券餘額張數以及融資餘額張數均為成分股餘額張數的加總。 大盤商品的融券餘額張數／融資餘額張數為交易所公布值。  週頻率以上為統計期間的期底值。 |

---
## 吉尼係數
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 吉尼係數 (選股欄位) |
| 語法 | ``` Value1 = GetField("吉尼係數"); Value1 = GetField("GiniCoefficient"); ``` |
| 欄位分類 | 籌碼 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 |
| 支援商品 | 台股 |
| 說明 | 使用券商分點交易比重所推估出來的吉尼系統。  數值介於０與１之間，愈接近0表示集中度越低（也就是越平均），愈接近１則表示集中度越高（也就是越不平均）。  系統將分點對個股的進出數值，統計成兩個數列，縱軸是「券商家數累積百分比」，橫軸是「累加券商買賣金額佔所有券商買賣金額百分比」，然後作出羅倫茲曲線( Lorenz Curve )後計算出吉尼係數的數值。  當吉尼係數越大時，表示買賣金額集中在少數券商，而吉尼係數越小的話，則表示買賣金額平均在許多分點上。 |

---
## 地緣券商買賣超張數
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 地緣券商買賣超張數 (選股欄位) |
| 語法 | ``` Value1 = GetField("地緣券商買賣超張數"); Value1 = GetField("GBDifference"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 |
| 支援商品 | 台股 類股指數 |
| 說明 | 「地緣券商」的買進張數 － 賣出張數。  地緣券商為公司地址附近的分點券商(不包含外資券商)。  預設是公司地址周圍三公里內的券商，如果分點過多，則改為1公里內，如果找不到分點的話則擴充到10公里以內。  類股商品（細產業指標、主題指標）計算方式為成分股加總。  週頻率以上為期間累計值。 |

---
## 外資成本
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 外資成本 (選股欄位) |
| 語法 | ``` Value1 = GetField("外資成本"); Value1 = GetField("FBSCost"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 季 半年 年 |
| 支援商品 | 台股 |
| 說明 | 系統估算的外資持股成本。  計算方式是透過每日外資買張，賣張，以及當日的價格，估算出外資的長期持有成本。  週頻率以上為統計期間的期底值。 |

---
## 外資持股
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 外資持股 (選股欄位) |
| 語法 | ``` Value1 = GetField("外資持股"); Value1 = GetField("Fsharesheld"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 |
| 支援商品 | 台股 類股指數 |
| 說明 | 外資持有的張數。  類股商品（細產業指標、主題指標）的數值則是成分股的外資持有張數的加總。  請注意：外資持股的數值由交易所每日公布。影響外資持股因素，除集中市場外資淨買賣數外，另有非集中市場交易因素例如：海外存託憑證異動股數、海外可轉換公司債轉換股數、上市公司增減資或股東會、除權申報或ETF申購買回、投資人變更國籍等，所以未必會等於外資買賣超的累計數值。  週頻率以上為統計期間的期底值。 |

---
## 外資持股比例
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 外資持股比例 (選股欄位) |
| 語法 | ``` Value1 = GetField("外資持股比例"); Value1 = GetField("Fsharesheldratio"); Value1 = GetField("ExgFsharesheld／Capital"); Value1 = GetField("外資持股比重"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 |
| 支援商品 | 台股 |
| 說明 | 外資持股的比例。 單位為%，例如：持股比例若為20.5%，則回傳20.5。  請注意：持股比例的數值由交易所直接公布。  週頻率以上為統計期間的期底值。 |

---
## 外資買張
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 外資買張 (選股欄位) |
| 語法 | ``` Value1 = GetField("外資買張"); Value1 = GetField("Ftotalbuy"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 |
| 支援商品 | 台股 |
| 說明 | 外資買進張數。  週頻率以上為期間累計值。 |

---
## 外資買賣超
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 外資買賣超 (選股欄位) |
| 語法 | ``` Value1 = GetField("外資買賣超"); Value1 = GetField("Fdifference"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 |
| 支援商品 | 台股 類股指數 |
| 說明 | 外資買賣超。數值內容/單位依商品類型而異。  台股商品的數值為外資買張 ─ 外資賣張。 類股商品（細產業指標、主題指標）的數值則是成分股的外資買賣超的加總。 大盤商品的數值則是外資買進金額 ─ 外資賣出金額，單位是元。  週頻率以上為期間累計值。 |

---
## 外資賣張
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 外資賣張 (選股欄位) |
| 語法 | ``` Value1 = GetField("外資賣張"); Value1 = GetField("Ftotalsell"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 |
| 支援商品 | 台股 |
| 說明 | 外資賣出張數。  週頻率以上為期間累計值。 |

---
## 大戶持股人數
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 大戶持股人數 (選股欄位) |
| 語法 | ``` Value1 = GetField("大戶持股人數",param:=400); Value1 = GetField("Bigsharesheldpeople",param:=400); ``` |
| 參數範圍 | 1、5、10、15、20、30、40、50、100、200、400、600、800、1000 |
| 欄位分類 | 籌碼 |
| 單位 | 人 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 週 月 季 半年 年 |
| 支援商品 | 台股 類股指數 |
| 說明 | 由集保公司所提供的指定級距以上的持股人數資料。 每週更新一次。  集保公司每週末會公佈當週每檔股票不同持股級距的持股人數，張數等資料。  目前公佈的級距包含1張以下，1張～5張，5張～10張，10張～15張，15張～20張，20張～30張，30張～40張，40張～50張，50張～100張，100張～200張，200張～400張，400張～600張，600張～800張，800張～1000張，以及1000張以上，其中每張為1000股。  大戶持股人數，則是統計某個級距以上的持股人數的加總，單位為人。例如400張大戶持股人數為400～600張，600～800張，800～1000張，以及1000張以上這幾個級距的持股人數的加總。  語法上可以使用： value1 = GetField("大戶持股人數", param:=400); 來取得400張以上的大戶持股人數。  指數型商品(例如類股指數，細產業指標、主題指標)亦提供此欄位，計算方式則是依照成份股的數值進行加總。 |

---
## 大戶持股張數
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 大戶持股張數 (選股欄位) |
| 語法 | ``` Value1 = GetField("大戶持股張數",param:=400); Value1 = GetField("Bigsharesheld",param:=400); ``` |
| 參數範圍 | 1、5、10、15、20、30、40、50、100、200、400、600、800、1000 |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 週 月 季 半年 年 |
| 支援商品 | 台股 類股指數 |
| 說明 | 由集保公司所提供的指定級距以上的持股張數資料。 每週更新一次。  集保公司每週末會公佈當週每檔股票不同持股級距的持股人數，張數等資料。  目前公佈的級距包含1張以下，1張～5張，5張～10張，10張～15張，15張～20張，20張～30張，30張～40張，40張～50張，50張～100張，100張～200張，200張～400張，400張～600張，600張～800張，800張～1000張，以及1000張以上，其中每張為1000股。  大戶持股張數，則是統計某個級距以上的持股張數的加總，單位為張。例如400張大戶持股張數為400～600張，600～800張，800～1000張，以及1000張以上這幾個級距的持股張數的加總。  語法上可以使用： value1 = GetField("大戶持股張數", param:=400); 來取得400張以上的大戶持股張數。  指數型商品(例如類股指數，細產業指標、主題指標)亦提供此欄位，計算方式則是依照成份股的數值進行加總。 |

---
## 大戶持股比例
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 大戶持股比例 (選股欄位) |
| 語法 | ``` Value1 = GetField("大戶持股比例",param:=400); Value1 = GetField("Bigsharesheldratio",param:=400); ``` |
| 參數範圍 | 1、5、10、15、20、30、40、50、100、200、400、600、800、1000 |
| 欄位分類 | 籌碼 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 週 月 季 半年 年 |
| 支援商品 | 台股 類股指數 |
| 說明 | 由集保公司所提供的指定級距以上的持股比例資料。 每週更新一次。  集保公司每週末會公佈當週每檔股票不同持股級距的持股人數，張數等資料。  目前公佈的級距包含1張以下，1張～5張，5張～10張，10張～15張，15張～20張，20張～30張，30張～40張，40張～50張，50張～100張，100張～200張，200張～400張，400張～600張，600張～800張，800張～1000張，以及1000張以上，其中每張為1000股。  大戶持股比例，則是統計某個級距以上的持股人數/總持股人數的比例，單位為百分比。例如400張大戶持股比例為400～600張，600～800張，800～1000張，以及1000張以上這幾個級距的持股人數加總/總持股人數的比例。  語法上可以使用： value1 = GetField("大戶持股比例", param:=400); 來取得400張以上的大戶持股比例。  指數型商品(例如類股指數，細產業指標、主題指標)亦提供此欄位，計算方式則是依照成份股的數值進行加總。 |

---
## 官股券商買賣超張數
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 官股券商買賣超張數 (選股欄位) |
| 語法 | ``` Value1 = GetField("官股券商買賣超張數"); Value1 = GetField("OBDifference"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 |
| 支援商品 | 台股 類股指數 |
| 說明 | 「官股券商」買進張數 - 賣出張數。  官股券商名單為: 臺銀，土銀，合庫，兆豐，第一金，華南永昌，彰銀以及台企銀。  類股商品（細產業指標、主題指標）計算方式為成分股加總。  週頻率以上為期間累計值。  請注意：因為統計時是以股為單位，所以欄位數值可能會包含小數(未滿1張時)。 |

---
## 實戶買張
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 實戶買張 (選股欄位) |
| 語法 | ``` Value1 = GetField("實戶買張"); Value1 = GetField("operatortotalbuy"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 |
| 支援商品 | 台股 |
| 說明 | 單一價格成交值介於50萬到100萬之間的合計買進張數。  系統每日統計券商分點對個股的買賣記錄，把成交值介於50萬到100萬之間的買進張數加總起來，稱之為實戶買張。  請注意：交易所提供分點統計資料內，同一價格的買進成交會自動合併成一筆資料。所以成交值是依照合併後的資料來計算。  週頻率以上為期間累計值。 |

---
## 實戶買賣超張數
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 實戶買賣超張數 (選股欄位) |
| 語法 | ``` Value1 = GetField("實戶買賣超張數"); Value1 = GetField("operatordifference"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 |
| 支援商品 | 台股 |
| 說明 | 實戶買張 ─ 實戶賣張。  實戶買張是單一價格成交值介於50萬到100萬之間的合計買進張數，而實戶賣張則是同樣級距的合計賣出張數。  週頻率以上為期間累計值。 |

---
## 實戶賣張
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 實戶賣張 (選股欄位) |
| 語法 | ``` Value1 = GetField("實戶賣張"); Value1 = GetField("operatortotalSell"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 |
| 支援商品 | 台股 |
| 說明 | 單一價格成交值介於50萬到100萬之間的合計賣出張數。  系統每日統計券商分點對個股的買賣記錄，把成交值介於50萬到100萬之間的賣出張數加總起來，稱之為實戶賣張。  請注意：交易所提供分點統計資料內，同一價格的賣出成交會自動合併成一筆資料。所以成交值是依照合併後的資料來計算。  週頻率以上為期間累計值。 |

---
## 實質買盤比
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 實質買盤比 (選股欄位) |
| 語法 | ``` Value1 = GetField("實質買盤比"); Value1 = GetField("RealLongRatio"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 |
| 支援商品 | 台股 類股指數 |
| 說明 | 實質買進張數佔成交量的比例。  計算公式為：(成交量－融資買進張數－資券相抵張數－融券回補張數)／成交量 \* 100％。  指數類型商品則是統計所有成分股的資料加總。 |

---
## 實質賣盤比
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 實質賣盤比 (選股欄位) |
| 語法 | ``` Value1 = GetField("實質賣盤比"); Value1 = GetField("RealShortRatio"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 |
| 支援商品 | 台股 |
| 說明 | 實質賣出張數佔成交量的比例。  計算公式為：(成交量－融券賣出張數－資券相抵張數－融資賣出張數)／成交量 \* 100％。  指數類型商品則是統計所有成分股的資料加總。 |

---
## 庫藏股實際買回張數
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 庫藏股實際買回張數 (選股欄位) |
| 語法 | ``` Value1 = GetField("庫藏股實際買回張數"); Value1 = GetField("Stockbuyvalue"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 單一 |
| 支援商品 | 台股 |
| 說明 | 最新一次庫藏股公告的實際買回張數。  使用時可以搭配庫藏股開始日期，庫藏股結束日期，以及庫藏股預計買回張數等欄位。 |

---
## 庫藏股預計買回張數
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 庫藏股預計買回張數 (選股欄位) |
| 語法 | ``` Value1 = GetField("庫藏股預計買回張數"); Value1 = GetField("Stockestivalue"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 單一 |
| 支援商品 | 台股 |
| 說明 | 最新一次庫藏股公告的預計買回張數。  使用時可以搭配庫藏股開始日期以及庫藏股結束日期來確認此欄位的有效期間。 |

---
## 投信成本
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 投信成本 (選股欄位) |
| 語法 | ``` Value1 = GetField("投信成本"); Value1 = GetField("SBSCost"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 季 半年 年 |
| 支援商品 | 台股 |
| 說明 | 系統估算的投信持股成本。  計算方式是透過每日投信買張，賣張，以及當日的價格，估算出投信的長期持有成本。  週頻率以上為統計期間的期底值。 |

---
## 投信持股
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 投信持股 (選股欄位) |
| 語法 | ``` Value1 = GetField("投信持股"); Value1 = GetField("Ssharesheld"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 |
| 支援商品 | 台股 類股指數 |
| 說明 | 系統估計值，計算方式是累計長期的投信買賣超張數。  類股商品（細產業指標、主題指標）的數值則是成分股的投信持股的加總。  請注意：投信估計持股，除了依每日買賣超計算外，會利用投信投顧公會公告的基金季持股明細(或半年持股明細，視基金類型)進行個股投信的庫存數值調整（季末的持股值可能會與前一日會有較大的落差）。  週頻率以上為統計期間的期底值。 |

---
## 投信持股比例
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 投信持股比例 (選股欄位) |
| 語法 | ``` Value1 = GetField("投信持股比例"); Value1 = GetField("Ssharesheldratio"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 |
| 支援商品 | 台股 |
| 說明 | 系統估算值，計算公式為：投信持股/流通在外張數 \* 100%。  單位為%，例如：持股比例若為20.5%，則回傳20.5。  週頻率以上為統計期間的期底值。 |

---
## 投信買張
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 投信買張 (選股欄位) |
| 語法 | ``` Value1 = GetField("投信買張"); Value1 = GetField("Stotalbuy"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 |
| 支援商品 | 台股 |
| 說明 | 投信買進張數。  週頻率以上為期間累計值。 |

---
## 投信買賣超
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 投信買賣超 (選股欄位) |
| 語法 | ``` Value1 = GetField("投信買賣超"); Value1 = GetField("Sdifference"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 |
| 支援商品 | 台股 大盤 類股指數 |
| 說明 | 投信買賣超。數值內容/單位依商品類型而異。  台股商品的數值為投信買張 ─ 投信賣張。 類股商品（細產業指標、主題指標）的數值則是成分股的投信買賣超的加總。 大盤商品的數值則是投信買進金額 ─ 投信賣出金額，單位是元。  週頻率以上為期間累計值。 |

---
## 投信賣張
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 投信賣張 (選股欄位) |
| 語法 | ``` Value1 = GetField("投信賣張"); Value1 = GetField("Stotalsell"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 |
| 支援商品 | 台股 |
| 說明 | 投信賣出張數。  週頻率以上為期間累計值。 |

---
## 控盤者成本線
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 控盤者成本線 (選股欄位) |
| 語法 | ``` Value1 = GetField("控盤者成本線"); Value1 = GetField("CostLine"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 季 半年 年 |
| 支援商品 | 台股 |
| 說明 | 系統估計的控盤者的持股成本。  控盤者的資料是每日單一價格成交值大於100萬的買賣記錄。  系統依此資料，估計出控盤者的長期持有成本。 |

---
## 控盤者買張
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 控盤者買張 (選股欄位) |
| 語法 | ``` Value1 = GetField("控盤者買張"); Value1 = GetField("controllertotalbuy"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 |
| 支援商品 | 台股 |
| 說明 | 單一價格成交值大於100萬的合計買進張數。  系統每日統計券商分點對個股的買賣記錄，把成交值大於100萬的買進張數加總起來，稱之為控盤者買張。  請注意：交易所提供分點統計資料內，同一價格的買進成交會自動合併成一筆資料。所以成交值是依照合併後的資料來計算。  週頻率以上為期間累計值。 |

---
## 控盤者買賣超張數
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 控盤者買賣超張數 (選股欄位) |
| 語法 | ``` Value1 = GetField("控盤者買賣超張數"); Value1 = GetField("controllerdifference"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 |
| 支援商品 | 台股 |
| 說明 | 控盤者買張 ─ 控盤者賣張。  控盤者買張是單一價格成交值大於100萬之間的合計買進張數，而控盤者賣張則是同樣級距的合計賣出張數。  週頻率以上為期間累計值。 |

---
## 控盤者賣張
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 控盤者賣張 (選股欄位) |
| 語法 | ``` Value1 = GetField("控盤者賣張"); Value1 = GetField("controllertotalsell"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 |
| 支援商品 | 台股 |
| 說明 | 單一價格成交值大於100萬的合計賣出張數。  系統每日統計券商分點對個股的買賣記錄，把成交值大於100萬的賣出張數加總起來，稱之為控盤者賣張。  請注意：交易所提供分點統計資料內，同一價格的賣出成交會自動合併成一筆資料。所以成交值是依照合併後的資料來計算。  週頻率以上為期間累計值。 |

---
## 散戶持股人數
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 散戶持股人數 (選股欄位) |
| 語法 | ``` Value1 = GetField("散戶持股人數",param:=400); Value1 = GetField("Retailsharesheldpeople",param:=400); ``` |
| 參數範圍 | 1、5、10、15、20、30、40、50、100、200、400、600、800、1000 |
| 欄位分類 | 籌碼 |
| 單位 | 人 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 週 月 季 半年 年 |
| 支援商品 | 台股 類股指數 |
| 說明 | 由集保公司所提供的指定級距以下的持股比例資料。 每週更新一次。  集保公司每週末會公佈當週每檔股票不同持股級距的持股人數，張數等資料。  目前公佈的級距包含1張以下，1張～5張，5張～10張，10張～15張，15張～20張，20張～30張，30張～40張，40張～50張，50張～100張，100張～200張，200張～400張，400張～600張，600張～800張，800張～1000張，以及1000張以上，其中每張為1000股。  散戶持股人數，則是統計某個級距以下的持股人數的加總，單位為人。例如10張持股人數為1張以下，1張～5張，以及5張～10張這幾個級距的持股人數的加總。  語法上可以使用： value1 = GetField("散戶持股人數", param:=10); 來取得10張以下的散戶持股人數。  指數型商品(例如類股指數，細產業指標、主題指標)亦提供此欄位，計算方式則是依照成份股的數值進行加總。 |

---
## 散戶持股張數
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 散戶持股張數 (選股欄位) |
| 語法 | ``` Value1 = GetField("散戶持股張數",param:=400); Value1 = GetField("Retailsharesheld",param:=400); ``` |
| 參數範圍 | 1、5、10、15、20、30、40、50、100、200、400、600、800、1000 |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 週 月 季 半年 年 |
| 支援商品 | 台股 類股指數 |
| 說明 | 由集保公司所提供的指定級距以下的持股比例資料。 每週更新一次。  集保公司每週末會公佈當週每檔股票不同持股級距的持股人數，張數等資料。  目前公佈的級距包含1張以下，1張～5張，5張～10張，10張～15張，15張～20張，20張～30張，30張～40張，40張～50張，50張～100張，100張～200張，200張～400張，400張～600張，600張～800張，800張～1000張，以及1000張以上，其中每張為1000股。  散戶持股張數，則是統計某個級距以下的持股張數的加總，單位為張。例如10張持股張數為1張以下，1張～5張，以及5張～10張這幾個級距的持股張數的加總。  語法上可以使用： value1 = GetField("散戶持股張數", param:=10); 來取得10張以下的散戶持股張數。  指數型商品(例如類股指數，細產業指標、主題指標)亦提供此欄位，計算方式則是依照成份股的數值進行加總。 |

---
## 散戶持股比例
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 散戶持股比例 (選股欄位) |
| 語法 | ``` Value1 = GetField("散戶持股比例",param:=400); Value1 = GetField("Retailsharesheldratio",param:=400); ``` |
| 參數範圍 | 1、5、10、15、20、30、40、50、100、200、400、600、800、1000 |
| 欄位分類 | 籌碼 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 週 月 季 半年 年 |
| 支援商品 | 台股 類股指數 |
| 說明 | 由集保公司所提供的指定級距以下的持股比例資料。 每週更新一次。  集保公司每週末會公佈當週每檔股票不同持股級距的持股人數，張數等資料。  目前公佈的級距包含1張以下，1張～5張，5張～10張，10張～15張，15張～20張，20張～30張，30張～40張，40張～50張，50張～100張，100張～200張，200張～400張，400張～600張，600張～800張，800張～1000張，以及1000張以上，其中每張為1000股。  散戶持股比例，則是統計某個級距以下的持股人數/總持股人數的比例，單位為百分比。例如10張持股比例為1張以下，1張～5張，以及5張～10張這幾個級距的持股人數加總/總持股人數的比例。  語法上可以使用： value1 = GetField("散戶持股比例", param:=10); 來取得10張以下的散戶持股比例。  指數型商品(例如類股指數，細產業指標、主題指標)亦提供此欄位，計算方式則是依照成份股的數值進行加總。 |

---
## 散戶買張
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 散戶買張 (選股欄位) |
| 語法 | ``` Value1 = GetField("散戶買張"); Value1 = GetField("retailtotalbuy"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 |
| 支援商品 | 台股 |
| 說明 | 單一價格成交值小於50萬的合計買進張數。  系統每日統計券商分點對個股的買賣記錄，把成交值小於50萬的買進張數加總起來，稱之為散戶買張。  請注意：交易所提供分點統計資料內，同一價格的買進成交會自動合併成一筆資料。所以成交值是依照合併後的資料來計算。  週頻率以上為期間累計值。 |

---
## 散戶買賣超張數
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 散戶買賣超張數 (選股欄位) |
| 語法 | ``` Value1 = GetField("散戶買賣超張數"); Value1 = GetField("retaildifference"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 |
| 支援商品 | 台股 |
| 說明 | 散戶買張 ─ 散戶賣張。  散戶買張是單一價格成交值小於50萬的合計買進張數，而散戶賣張則是同樣級距的合計賣出張數。  週頻率以上為期間累計值。 |

---
## 散戶賣張
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 散戶賣張 (選股欄位) |
| 語法 | ``` Value1 = GetField("散戶賣張"); Value1 = GetField("retailtotalsell"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 |
| 支援商品 | 台股 |
| 說明 | 單一價格成交值小於50萬的合計賣出張數。  系統每日統計券商分點對個股的買賣記錄，把成交值小於50萬的賣出張數加總起來，稱之為散戶賣張。  請注意：交易所提供分點統計資料內，同一價格的賣出成交會自動合併成一筆資料。所以成交值是依照合併後的資料來計算。  週頻率以上為期間累計值。 |

---
## 機構持股
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 機構持股 (選股欄位) |
| 語法 | ``` Value1 = GetField("機構持股"); Value1 = GetField("13FHoldings"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 股 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 季 |
| 支援商品 | 美(股票) |
| 說明 | 依美國證券交易委員會規定每季公告的「SEC Form 13F」所計算出的投資機構總持有股數。  美股股票才適用本欄位。  依照SEC的規定，管理超過一億美元以上的機構投資人，需在每季結束後的45天內向美國證券交易委員會提供報告，載明所有持股的明細。  透過申報資料，系統會加總所有機構人對單一股票的總持股數量，每季提供一筆數值，更新時間約在當季結束後45日後陸續更新。 |

---
## 機構持股比重
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 機構持股比重 (選股欄位) |
| 語法 | ``` Value1 = GetField("機構持股比重"); Value1 = GetField("13FHoldingsRate"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | % |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 季 |
| 支援商品 | 美(股票) |
| 說明 | 依美國證券交易委員會規定每季公告的「SEC Form 13F」所計算出的投資機構總持有股數佔流通在外股數的比重。  單位為%，例如：欄位數值75%，則回傳75。  美股股票才適用本欄位。  依照SEC的規定，管理超過一億美元以上的機構投資人，需在每季結束後的45天內向美國證券交易委員會提供報告，載明所有持股的明細。  透過申報資料，系統會加總所有機構人對單一股票的總持股數量，每季提供一筆數值，更新時間約在當季結束後45日後陸續更新。 |

---
## 法人持股
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 法人持股 (選股欄位) |
| 語法 | ``` Value1 = GetField("法人持股"); Value1 = GetField("InvestorSharesHeld"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 |
| 支援商品 | 台股 |
| 說明 | 三大法人(外資、投信、自營商)合計估計持股張數。  計算公式是外資持股(交易所公布數值)＋投信持股(系統估計值)＋自營商持股(系統估計值)。  類股商品（細產業指標、主題指標）的數值則是成分股的法人持股的加總。  週頻率以上為統計期間的期底值。 |

---
## 法人持股比例
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 法人持股比例 (選股欄位) |
| 語法 | ``` Value1 = GetField("法人持股比例"); Value1 = GetField("InvestorSharesHeldRatio"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 |
| 支援商品 | 台股 |
| 說明 | 系統估算值，計算公式為：法人持股/流通在外張數 \* 100%。  單位為%，例如：持股比例若為20.5%，則回傳20.5。  週頻率以上為統計期間的期底值。 |

---
## 法人買張
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 法人買張 (選股欄位) |
| 語法 | ``` Value1 = GetField("法人買張"); Value1 = GetField("InvestorTotalBuy"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 |
| 支援商品 | 台股 |
| 說明 | 三大法人(外資、投信、自營商)合計買進張數。 計算公式：外資買張＋投信買張＋自營商買張。  週頻率以上為期間累計值。 |

---
## 法人買賣超張數
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 法人買賣超張數 (選股欄位) |
| 語法 | ``` Value1 = GetField("法人買賣超張數"); Value1 = GetField("InvestorDifference"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 |
| 支援商品 | 台股 類股指數 |
| 說明 | 三大法人(外資、投信、自營商)合計買賣超張數。  台股商品的數值為法人買張 ─ 法人賣張。 類股商品（細產業指標、主題指標）的數值則是成分股的法人買賣超的加總。 週頻率以上為期間累計值。 |

---
## 法人賣張
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 法人賣張 (選股欄位) |
| 語法 | ``` Value1 = GetField("法人賣張"); Value1 = GetField("investorTotalSell"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 |
| 支援商品 | 台股 |
| 說明 | 三大法人(外資、投信、自營商)合計賣出張數。 計算公式：外資賣張＋投信賣張＋自營商賣張。  週頻率以上為期間累計值。 |

---
## 現券償還張數
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 現券償還張數 (選股欄位) |
| 語法 | ``` Value1 = GetField("現券償還張數"); Value1 = GetField("ShortSalesRS"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 |
| 支援商品 | 台股 |
| 說明 | 以現股方式償還融券的張數。  現券償還指的是以現股來償還融券放空的部位。  週頻率以上為期間累計值。 |

---
## 現股當沖張數
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 現股當沖張數 (選股欄位) |
| 語法 | ``` Value1 = GetField("現股當沖張數"); Value1 = GetField("NormalDayTrades"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 |
| 支援商品 | 台股 類股指數 |
| 說明 | 現股當日沖銷的張數。  類股商品（細產業指標、主題指標）的數值為成分股現股當沖張數的加總。 大盤商品的數值為交易所公布值。  週頻率以上為期間累計值。 |

---
## 現股當沖買進金額
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 現股當沖買進金額 (選股欄位) |
| 語法 | ``` Value1 = GetField("現股當沖買進金額"); Value1 = GetField("DayTradeBValue"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 |
| 支援商品 | 台股 類股指數 |
| 說明 | 現股當日沖銷的總買進金額。  類股商品（細產業指標、主題指標）的數值為成分股現股當沖買進金額的加總。 大盤商品的數值為交易所公布值。  週頻率以上為期間累計值。 |

---
## 現股當沖賣出金額
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 現股當沖賣出金額 (選股欄位) |
| 語法 | ``` Value1 = GetField("現股當沖賣出金額"); Value1 = GetField("DayTradeSValue"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 |
| 支援商品 | 台股 類股指數 |
| 說明 | 現股當日沖銷的總賣出金額。  類股商品（細產業指標、主題指標）的數值為成分股現股當沖賣出金額的加總。 大盤商品的數值為交易所公布值。  週頻率以上為期間累計值。 |

---
## 現金償還張數
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 現金償還張數 (選股欄位) |
| 語法 | ``` Value1 = GetField("現金償還張數"); Value1 = GetField("POMRC"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 |
| 支援商品 | 台股 |
| 說明 | 以現金方式償還融資的張數。  現金償還指的是以補足現金把融資股票轉成現股。  週頻率以上為期間累計值。 |

---
## 現金增資佔股本比重
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 現金增資佔股本比重 (選股欄位) |
| 語法 | ``` Value1 = GetField("現金增資佔股本比重"); Value1 = GetField("CashIncrease／Capital"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 年 |
| 支援商品 | 台股 |
| 說明 | 股本形成中，現金增資所佔的比重。 計算公式 : 現金增資金額／總股本金額\*100%。 |

---
## 當日沖銷張數
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 當日沖銷張數 (選股欄位) |
| 語法 | ``` Value1 = GetField("當日沖銷張數"); Value1 = GetField("Daytradeshares"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 |
| 支援商品 | 台股 |
| 說明 | 當日沖銷的總張數。計算公式：現股當沖張數＋資券互抵張數。  大盤商品以及類股商品（細產業指標、主題指標）的數值為成分股當日沖銷張數的加總。  週頻率以上為期間累計值。 |

---
## 盈餘轉增資佔股本比重
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 盈餘轉增資佔股本比重 (選股欄位) |
| 語法 | ``` Value1 = GetField("盈餘轉增資佔股本比重"); Value1 = GetField("CaptialIncreaseFromEarning／Capital"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 年 |
| 支援商品 | 台股 |
| 說明 | 股本形成中，盈餘轉增資所佔的比重。 計算公式 : 盈餘轉增資金額／總股本金額\*100%。 |

---
## 籌碼鎖定率
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 籌碼鎖定率 (選股欄位) |
| 語法 | ``` Value1 = GetField("籌碼鎖定率"); Value1 = GetField("RatioOfMajorHolder"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 |
| 支援商品 | 台股 |
| 說明 | 計算公式: 主力券商買超張數／(發行張數－董監持股張數) \* 100%。 |

---
## 綜合前十大券商買賣超張數
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 綜合前十大券商買賣超張數 (選股欄位) |
| 語法 | ``` Value1 = GetField("綜合前十大券商買賣超張數"); Value1 = GetField("IB10Difference"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 |
| 支援商品 | 台股 類股指數 |
| 說明 | 「綜合前十大券商」買進張數 － 賣出張數。  綜合前十大券商名單為:統一、元富、台新、玉山、國泰、群益金鼎、凱基、富邦、元大、永豐金。  一般在看籌碼分析的時候，我們會去研究今天的買賣盤是來自那一類的力量，一般分為公司派，外資，基金公司，主力，實戶，業內，散戶及當沖客。但其實還有一股力量叫代操，代操分成兩類，一類是檯面上的，一類是檯面下的，檯面上的主要是投信及投顧，這個力量其實很龐大。  這些資金的進出場，沒有算在投信買賣超裡頭，如果我們想要知道代操資金的動向，必須另謀他法。  我們有觀察到，代操體系一般不會自己養大量研究員，還是會需要券商的研究資源來支援，所以他們下單的券商，絕大多數是能提供研究資源的大型券商法人部，而這些法人部的業績一般是算到券商總公司，所以我們才會把前十大券商總公司合計成為一個群組來觀察他們的買賣超情況。  更多資訊請參考：[籌碼大數據裡的“前十大券商總公司”釋疑](https://www.xq.com.tw/xstrader/%E7%B1%8C%E7%A2%BC%E5%A4%A7%E6%95%B8%E6%93%9A%E8%A3%A1%E7%9A%84%E5%89%8D%E5%8D%81%E5%A4%A7%E5%88%B8%E5%95%86%E7%B8%BD%E5%85%AC%E5%8F%B8%E9%87%8B%E7%96%91/)  類股商品（細產業指標、主題指標）計算方式為成分股加總。  週頻率以上為期間累計值。  請注意：因為統計時是以股為單位，所以欄位數值可能會包含小數(未滿1張時)。 |

---
## 總持股人數
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 總持股人數 (選股欄位) |
| 語法 | ``` Value1 = GetField("總持股人數"); Value1 = GetField("TotalSharesHeldPeople"); Value1 = GetField("總股東人數"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 人 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 週 月 季 半年 年 |
| 支援商品 | 台股 類股指數 |
| 說明 | 由集保公司所提供的總持股人數。 股票每週更新一次，可轉債(CB)則是每月更新一次。  指數型商品(例如類股指數，細產業指標、主題指標)亦提供此欄位，計算方式則是依照成份股的數值進行加總。 |

---
## 股票基金持有檔數
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 股票基金持有檔數 (選股欄位) |
| 語法 | ``` Value1 = GetField("股票基金持有檔數"); Value1 = GetField("HoldingByFunds"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 檔 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 季 |
| 支援商品 | 台股 |
| 說明 | 根據主管機關核准的境內外共同基金，每季公告的占基金淨資產價值1%以上持股資料做統計，計算持有該股票的基金檔數。  依照規定，各基金公司於每個季底(3/31, 6/30, 9/30, 12/31)後10個工作日內會公佈當季的持股，系統預計在次月20日時更新此資料，也就是說第一季的資料約在4/20日左右時更新，第二季的資料約在7/20日左右時更新，以下類推。  單位為持股檔數，例如Ａ股票在七月份公告的六月份基金持股資料共有10檔基金持有該商品，則回傳10。 |

---
## 自營商成本
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 自營商成本 (選股欄位) |
| 語法 | ``` Value1 = GetField("自營商成本"); Value1 = GetField("DBSCost"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 季 半年 年 |
| 支援商品 | 台股 |
| 說明 | 系統估算的自營商持股成本。  計算方式是透過每日自營商買張，賣張，以及當日的價格，估算出自營商的長期持有成本。  週頻率以上為統計期間的期底值。 |

---
## 自營商持股
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 自營商持股 (選股欄位) |
| 語法 | ``` Value1 = GetField("自營商持股"); Value1 = GetField("Dsharesheld"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 |
| 支援商品 | 台股 類股指數 |
| 說明 | 系統估計值，計算方式是累計長期的自營商買賣超張數。  類股商品（細產業指標、主題指標）的數值則是成分股的自營商持股的加總。  週頻率以上為統計期間的期底值。 |

---
## 自營商持股比例
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 自營商持股比例 (選股欄位) |
| 語法 | ``` Value1 = GetField("自營商持股比例"); Value1 = GetField("Dsharesheldratio"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 |
| 支援商品 | 台股 |
| 說明 | 系統估算值，計算公式為：自營商持股/流通在外張數 \* 100%。  單位為%，例如：持股比例若為20.5%，則回傳20.5。  週頻率以上為統計期間的期底值。 |

---
## 自營商自行買賣買張
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 自營商自行買賣買張 (選股欄位) |
| 語法 | ``` Value1 = GetField("自營商自行買賣買張"); Value1 = GetField("DSelftotalbuy"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 |
| 支援商品 | 台股 |
| 說明 | 自營商買進張數內歸屬於自行買賣的數量。  週頻率以上為期間累計值。 |

---
## 自營商自行買賣買賣超
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 自營商自行買賣買賣超 (選股欄位) |
| 語法 | ``` Value1 = GetField("自營商自行買賣買賣超"); Value1 = GetField("DSelfdifference"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 |
| 支援商品 | 台股 |
| 說明 | 自營商買賣超內歸屬於自行買賣的數量。數值內容/單位依商品類型而異。  台股商品的數值為自營商自行買賣買張 ─ 自營商自行買賣賣張。 大盤商品的數值則是自營商自行買賣買進金額 ─ 自營商自行買賣賣出金額，單位是元。  週頻率以上為期間累計值。 |

---
## 自營商自行買賣賣張
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 自營商自行買賣賣張 (選股欄位) |
| 語法 | ``` Value1 = GetField("自營商自行買賣賣張"); Value1 = GetField("DSelftotalsell"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 |
| 支援商品 | 台股 |
| 說明 | 自營商買進張數內歸屬於自行買賣的數量。  週頻率以上為期間累計值。 |

---
## 自營商買張
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 自營商買張 (選股欄位) |
| 語法 | ``` Value1 = GetField("自營商買張"); Value1 = GetField("Dtotalbuy"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 |
| 支援商品 | 台股 |
| 說明 | 自營商買進張數。  週頻率以上為期間累計值。 |

---
## 自營商買賣超
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 自營商買賣超 (選股欄位) |
| 語法 | ``` Value1 = GetField("自營商買賣超"); Value1 = GetField("Ddifference"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 |
| 支援商品 | 台股 類股指數 |
| 說明 | 自營商買賣超。數值內容/單位依商品類型而異。  台股商品的數值為自營商買張 ─ 自營商賣張。 類股商品（細產業指標、主題指標）的數值則是成分股的自營商買賣超的加總。 大盤商品的數值則是自營商買進金額 ─ 自營商賣出金額，單位是元。  週頻率以上為期間累計值。 |

---
## 自營商賣張
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 自營商賣張 (選股欄位) |
| 語法 | ``` Value1 = GetField("自營商賣張"); Value1 = GetField("Dtotalsell"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 |
| 支援商品 | 台股 |
| 說明 | 自營商賣出張數。  週頻率以上為期間累計值。 |

---
## 自營商避險買張
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 自營商避險買張 (選股欄位) |
| 語法 | ``` Value1 = GetField("自營商避險買張"); Value1 = GetField("DtotalHedgebuy"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 |
| 支援商品 | 台股 |
| 說明 | 自營商買進張數內歸屬於避險的數量。  週頻率以上為期間累計值。 |

---
## 自營商避險買賣超
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 自營商避險買賣超 (選股欄位) |
| 語法 | ``` Value1 = GetField("自營商避險買賣超"); Value1 = GetField("DHedgedifference"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 |
| 支援商品 | 台股 |
| 說明 | 自營商買賣超內歸屬於避險的數量。數值內容/單位依商品類型而異。  台股商品的數值為自營商避險買張 ─ 自營商避險賣張。 大盤商品的數值則是自營商避險買進金額 ─ 自營商避險賣出金額，單位是元。  週頻率以上為期間累計值。 |

---
## 自營商避險賣張
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 自營商避險賣張 (選股欄位) |
| 語法 | ``` Value1 = GetField("自營商避險賣張"); Value1 = GetField("DtotalHedgesell"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 |
| 支援商品 | 台股 |
| 說明 | 自營商買進張數內歸屬於避險的數量。  週頻率以上為期間累計值。 |

---
## 董監持股
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 董監持股 (選股欄位) |
| 語法 | ``` Value1 = GetField("董監持股"); Value1 = GetField("DirectorHeld"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 月 |
| 支援商品 | 台股 類股指數 |
| 說明 | 董事與監察人的持股張數。 類股與細產業商品計算方式為成分股加總。 |

---
## 董監持股佔股本比例
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 董監持股佔股本比例 (選股欄位) |
| 語法 | ``` Value1 = GetField("董監持股佔股本比例"); Value1 = GetField("DirectorHeld／Capital"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 |
| 支援商品 | 台股 |
| 說明 | 董監持股占公司股本的比例。  計算公式為董監持股張數／公司股本＊100%。  董監持股統計的範圍包含公司內的董事以及監察人。  資料每月更新一次。 |

---
## 董監質設比例
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 董監質設比例 (選股欄位) |
| 語法 | ``` Value1 = GetField("董監質設比例"); Value1 = GetField("DSmortgageratio"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 月 |
| 支援商品 | 台股 |
| 說明 | 董監持股內質設的比例。  計算公式：董監持股內目前質設的張數／董監持股＊100%。  資料每月更新一次。 |

---
## 融券使用率
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 融券使用率 (選股欄位) |
| 語法 | ``` Value1 = GetField("融券使用率"); Value1 = GetField("ShortSell％"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 |
| 支援商品 | 台股 |
| 說明 | 融券使用的比例。計算公式：融券餘額 / 融資限額 \* 100%。  單位是％，例如融券使用率為70%的話，則回傳70。  週頻率以上為統計期間的期底值。 |

---
## 融券增減張數
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 融券增減張數 (選股欄位) |
| 語法 | ``` Value1 = GetField("融券增減張數"); Value1 = GetField("ShortSaleNew"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 |
| 支援商品 | 台股 類股指數 |
| 說明 | 融券餘額(張數)與前一期的異動張數。  融券餘額張數為到目前為止以融券賣出的淨張數。  類股商品（細產業指標、主題指標）的數值為成分股融券增減張數的加總。  週頻率以上為期間累計值。 |

---
## 融券買進張數
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 融券買進張數 (選股欄位) |
| 語法 | ``` Value1 = GetField("融券買進張數"); Value1 = GetField("ShortSaleRecall"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 |
| 支援商品 | 台股 |
| 說明 | 融券買進的張數。  週頻率以上為期間累計值。 |

---
## 融券賣出張數
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 融券賣出張數 (選股欄位) |
| 語法 | ``` Value1 = GetField("融券賣出張數"); Value1 = GetField("ShortSales"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 |
| 支援商品 | 台股 |
| 說明 | 融券賣出的張數。  週頻率以上為期間累計值。 |

---
## 融券餘額佔股本比例
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 融券餘額佔股本比例 (選股欄位) |
| 語法 | ``` Value1 = GetField("融券餘額佔股本比例"); Value1 = GetField("Shortsaleremain／Capital"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 |
| 支援商品 | 台股 |
| 說明 | 計算公式 : 融券餘額張數／發行張數\*100%。 |

---
## 融券餘額張數
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 融券餘額張數 (選股欄位) |
| 語法 | ``` Value1 = GetField("融券餘額張數"); Value1 = GetField("ShortSaleRemain"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 還原日 還原週 還原月 |
| 支援商品 | 台股 類股指數 |
| 說明 | 融券餘額張數，也就是到目前為止以融券賣出的淨張數。  這個數值代表著還有多少融券張數尚未償還。  類股商品（細產業指標、主題指標）的數值為成分股融券餘額張數的加總。  週頻率以上為統計期間的期底值。 |

---
## 融資使用率
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 融資使用率 (選股欄位) |
| 語法 | ``` Value1 = GetField("融資使用率"); Value1 = GetField("pomusingratio"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 |
| 支援商品 | 台股 |
| 說明 | 融資使用的比例。計算公式：融資餘額／融資限額 \* 100%。  單位是％，例如融資使用率為70%的話，則回傳70。  週頻率以上為統計期間的期底值。 |

---
## 融資增減張數
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 融資增減張數 (選股欄位) |
| 語法 | ``` Value1 = GetField("融資增減張數"); Value1 = GetField("pomnew"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 |
| 支援商品 | 台股 類股指數 |
| 說明 | 融資餘額(張數)與前一期的異動張數。  融資餘額張數為到目前為止以融資買進的淨張數。  類股商品（細產業指標、主題指標）的數值為成分股融資增減張數的加總。  週頻率以上為期間累計值。 |

---
## 融資維持率
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 融資維持率 (選股欄位) |
| 語法 | ``` Value1 = GetField("融資維持率"); Value1 = GetField("Pomremainratio"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 季 半年 年 |
| 支援商品 | 台股 |
| 說明 | 系統估算的大盤或是個股的融資維持率，也就是股票市值與融資金額的比例。  計算公式:  個股融資維持率=收盤價／(融資成本線 \* 0.6) \* 100%  大盤融資維持率=所有融資股票市值／大盤融資餘額 \* 100%  依查詢頻率取期底值。  註：  個股融資成本=(昨日融資買進成本 \* (融資餘額-融資買進) + 收盤價 \* 融資買進) / 融資餘額 |

---
## 融資買進張數
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 融資買進張數 (選股欄位) |
| 語法 | ``` Value1 = GetField("融資買進張數"); Value1 = GetField("POMBuy"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 |
| 支援商品 | 台股 |
| 說明 | 融資買進的張數。  週頻率以上為期間累計值。 |

---
## 融資賣出張數
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 融資賣出張數 (選股欄位) |
| 語法 | ``` Value1 = GetField("融資賣出張數"); Value1 = GetField("POMSell"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 |
| 支援商品 | 台股 |
| 說明 | 融資買進的張數。  週頻率以上為期間累計值。 |

---
## 融資限額張數
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 融資限額張數 (選股欄位) |
| 語法 | ``` Value1 = GetField("融資限額張數"); Value1 = GetField("MarginLimit"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 |
| 支援商品 | 台股 |
| 說明 | 交易所公告的融資限額張數。 |

---
## 融資餘額佔股本比例
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 融資餘額佔股本比例 (選股欄位) |
| 語法 | ``` Value1 = GetField("融資餘額佔股本比例"); Value1 = GetField("Pomremain／Capital"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 |
| 支援商品 | 台股 |
| 說明 | 計算公式 : 融資餘額張數／發行張數\*100%。 |

---
## 融資餘額張數
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 融資餘額張數 (選股欄位) |
| 語法 | ``` Value1 = GetField("融資餘額張數"); Value1 = GetField("Pomremain"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 還原日 還原週 還原月 |
| 支援商品 | 台股 類股指數 |
| 說明 | 融資餘額張數，也就是到目前以融資買進的淨張數。  這個數值代表著還有多少融資張數尚未償還。  類股商品（細產業指標、主題指標）的數值為成分股融資餘額張數的加總。  週頻率以上為統計期間的期底值。 |

---
## 買家數
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 買家數 (選股欄位) |
| 語法 | ``` Value1 = GetField("買家數"); Value1 = GetField("NetLongBranches"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 家 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 |
| 支援商品 | 台股 類股指數 |
| 說明 | 統計買超張數大於零的券商家數。  同一個券商的所有分點加總統計，如果合計為買超，則算為1家。 |

---
## 買進公司家數
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 買進公司家數 (選股欄位) |
| 語法 | ``` Value1 = GetField("買進公司家數"); Value1 = GetField("LongSecurityFirms"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 家 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 |
| 支援商品 | 台股 |
| 說明 | 統計買進金額大於零的券商家數。  同一個券商的所有分點會合併統計，例如元大館前、元大忠孝都有買進個股，只會被計算為"元大"一家。 |

---
## 資券互抵張數
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 資券互抵張數 (選股欄位) |
| 語法 | ``` Value1 = GetField("資券互抵張數"); Value1 = GetField("MarginTrades"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 |
| 支援商品 | 台股 類股指數 |
| 說明 | 資券當沖(信用當沖)的張數。  資券當沖指的是在同一日有融資買進、融券賣出所相抵的張數。  類股商品（細產業指標、主題指標）的數值為成分股資券互抵張數的加總。 大盤商品的數值為交易所公布值。  週頻率以上為期間累計值。 |

---
## 賣出公司家數
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 賣出公司家數 (選股欄位) |
| 語法 | ``` Value1 = GetField("賣出公司家數"); Value1 = GetField("ShortSecurityFirms"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 家 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 |
| 支援商品 | 台股 |
| 說明 | 統計賣出金額大於零的券商家數。  同一個券商的所有分點會合併統計，例如元大館前、元大忠孝都有買進個股，只會被計算為"元大"一家。 |

---
## 賣家數
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 賣家數 (選股欄位) |
| 語法 | ``` Value1 = GetField("賣家數"); Value1 = GetField("NetShortBranches"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 家 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 |
| 支援商品 | 台股 類股指數 |
| 說明 | 統計賣超張數大於零的券商家數。  同一個券商的所有分點加總統計，如果合計為賣超，則算為1家。 |

---
## 週轉率
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 週轉率 (選股欄位) |
| 語法 | ``` Value1 = GetField("週轉率"); Value1 = GetField("TurnOverRatio"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 季 年 |
| 支援商品 | 台股 |
| 說明 | 股票交易量與流通股數的比例。  計算公式 : 當期成交量總和／發行張數 \* 100%。 |

---
## 還券張數
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 還券張數 (選股欄位) |
| 語法 | ``` Value1 = GetField("還券張數"); Value1 = GetField("SBLreturn"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 |
| 支援商品 | 台股 |
| 說明 | 償還先前借券的張數。  週頻率以上為期間累計值。 |

---
## 關聯券商買賣超張數
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 關聯券商買賣超張數 (選股欄位) |
| 語法 | ``` Value1 = GetField("關聯券商買賣超張數"); Value1 = GetField("CorrelationBrokerDifference"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 |
| 支援商品 | 台股 |
| 說明 | 「關聯券商」買進張數 － 賣出張數。  關聯券商是比對每週集保股權分散表內「千張大戶持股」的變化與當週主力分點的買賣超，來找出千張大戶所在的分點，這些分點就稱為這檔商品的關聯券商。  資料從2017/06開始提供。 |

---
## 關鍵券商買賣超張數
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 關鍵券商買賣超張數 (選股欄位) |
| 語法 | ``` Value1 = GetField("關鍵券商買賣超張數"); Value1 = GetField("KeyBrokerDifference"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 |
| 支援商品 | 台股 類股指數 |
| 說明 | 「關鍵券商」買進張數 － 賣出張數。  關鍵券商是個股實施庫藏股期間買超最大的券商分點，以此來追蹤大股東的進出動向。  類股商品（細產業指標、主題指標）計算方式為成分股加總。  請注意：因為統計時是以股為單位，所以欄位數值可能會包含小數(未滿1張時)。  資料從2013年開始提供。 |

---
## 集保張數
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 集保張數 (選股欄位) |
| 語法 | ``` Value1 = GetField("集保張數"); Value1 = GetField("SharesUnderCentralCustody"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | 萬張 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 週 |
| 支援商品 | 台股 |
| 說明 | 由集保公司所提供的集保張數，換算成萬張。 |

---
## 集保張數佔發行張數百分比
**Path:** 選股欄位 > 籌碼

|  |  |
| --- | --- |
| 欄位名稱 | 集保張數佔發行張數百分比 (選股欄位) |
| 語法 | ``` Value1 = GetField("集保張數佔發行張數百分比"); Value1 = GetField("SharesUnderCentralCustody％"); ``` |
| 欄位分類 | 籌碼 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 週 |
| 支援商品 | 台股 |
| 說明 | 計算公式 : 集保張數／發行張數 \*100%。 |

---
## 10年年化報酬率
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 10年年化報酬率 (選股欄位) |
| 語法 | ``` Value1 = GetField("10年年化報酬率"); Value1 = GetField("IRR10YAnnualized"); ``` |
| 欄位分類 | 財務 |
| 單位 | % |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 |
| 支援商品 | 台股 美(ETF) |
| 說明 | 近10年年化報酬率，計算方式包含配息，以及配息再投入。  公式是期間的累計報酬率然後年化。 僅支援台/美ETF商品。 |

---
## 1年夏普指數
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 1年夏普指數 (選股欄位) |
| 語法 | ``` Value1 = GetField("1年夏普指數"); Value1 = GetField("SHARPE_1Y"); ``` |
| 欄位分類 | 財務 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 月 |
| 支援商品 | 台股 |
| 說明 | Sharpe指標衡量投資相對於其風險所帶來的超額回報率，即投資回報超過無風險利率部分的表現。 計算方式為：(商品過去一年的平均月報酬率-無風險利率)/過去一年的月報酬率的標準差，用來衡量每單位風險所獲得的超額報酬。 |

---
## 1年年化報酬率
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 1年年化報酬率 (選股欄位) |
| 語法 | ``` Value1 = GetField("1年年化報酬率"); Value1 = GetField("IRR1YAnnualized"); ``` |
| 欄位分類 | 財務 |
| 單位 | % |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 |
| 支援商品 | 台股 美(ETF) |
| 說明 | 近1年年化報酬率，計算方式包含配息，以及配息再投入。  公式是期間的累計報酬率然後年化。 僅支援台/美ETF商品。 |

---
## 3年年化報酬率
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 3年年化報酬率 (選股欄位) |
| 語法 | ``` Value1 = GetField("3年年化報酬率"); Value1 = GetField("IRR3YAnnualized"); ``` |
| 欄位分類 | 財務 |
| 單位 | % |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 |
| 支援商品 | 台股 美(ETF) |
| 說明 | 近3年年化報酬率，計算方式包含配息，以及配息再投入。  公式是期間的累計報酬率然後年化。 僅支援台/美ETF商品。 |

---
## 5年年化報酬率
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 5年年化報酬率 (選股欄位) |
| 語法 | ``` Value1 = GetField("5年年化報酬率"); Value1 = GetField("IRR5YAnnualized"); ``` |
| 欄位分類 | 財務 |
| 單位 | % |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 |
| 支援商品 | 台股 美(ETF) |
| 說明 | 近5年年化報酬率，計算方式包含配息，以及配息再投入。  公式是期間的累計報酬率然後年化。 僅支援台/美ETF商品。 |

---
## EPS法說公佈值
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | EPS法說公佈值 (選股欄位) |
| 語法 | ``` Value1 = GetField("EPS法說公佈值"); Value1 = GetField("ReportedQuarterlyEPS"); ``` |
| 欄位分類 | 財務 |
| 單位 | 美元 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 季 |
| 支援商品 | 美(股票) |
| 說明 | 公司在財報法說會上發佈的季度EPS數值。 美股股票才適用本欄位。  公司在每季財報公佈前通常會召開法說會(Earning Call)，會議內會公佈當季的各項財報數值，也會給出對於未來的展望。  請注意：   1. Earning Call時所公佈的當期EPS，通常是Diluted EPS before non-recurring items, 為Non-GAAP計算。所以可能會跟財務報表內依GAAP會計原則計算的EPS會有不同。 2. SEC並沒有要求所有公司都要召開Earning Call，所以並非每一家公司都會有此欄位。 3. 此欄位的公佈時間與財報的EPS的公佈時間可能會不同。 |

---
## EPS預估值
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | EPS預估值 (選股欄位) |
| 語法 | ``` Value1 = GetField("EPS預估值"); Value1 = GetField("EstimateQuarterlyEPS"); ``` |
| 欄位分類 | 財務 |
| 單位 | 美元 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 季 |
| 支援商品 | 美(股票) |
| 說明 | 市場機構對個股的季度EPS預估值。 美股股票才適用本欄位。  請注意：   1. 這個數值是Diluted EPS before non-recurring items, 採Non-GAAP計算. 所以有可能與財務報表內依GAAP會計原則計算的EPS會有不同。 2. 市場機構並非會對每檔股票提供EPS的預估，所以並非每檔股票都會有此欄位。   如果想要知道這一筆欄位的對應財報年度的話，請使用GetFieldFiscalY("EPS預估值")，以及GetFieldFiscalQ("EPS預估值")這兩個函數。 |

---
## _存貨_應收帳款__營收
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | (存貨+應收帳款)／營收 (選股欄位) |
| 語法 | ``` Value1 = GetField("(存貨+應收帳款)／營收"); Value1 = GetField("(Inventory+AccoutReceivable)／Sales"); Value1 = GetField("(存貨+應收帳款)／季營收"); ``` |
| 欄位分類 | 財務 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 |
| 說明 | 計算公式: (存貨+應收帳款及票據)／營業收入淨額\*100%。 |

---
## 一年內到期長期負債
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 一年內到期長期負債 (選股欄位) |
| 語法 | ``` Value1 = GetField("一年內到期長期負債"); Value1 = GetField("CurrentOfLong－TermDebt"); ``` |
| 欄位分類 | 財務 |
| 單位 | 百萬 |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 |
| 說明 | 「資產負債表」中「流動負債」的「一年內到期長期負債」項目。 金融、壽險、產險股不適用此欄位。 |

---
## 企業價值
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 企業價值 (選股欄位) |
| 語法 | ``` Value1 = GetField("企業價值"); Value1 = GetField("EnterpriseValue"); ``` |
| 欄位分類 | 財務 |
| 單位 | 百萬 |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 |
| 說明 | 計算公式: 總市值 ＋ 負債總額 － 現金及約當現金。 |

---
## 企業價值營收比
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 企業價值營收比 (選股欄位) |
| 語法 | ``` Value1 = GetField("企業價值營收比"); Value1 = GetField("CorporateValue／Sales"); ``` |
| 欄位分類 | 財務 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 |
| 說明 | 計算方式: (總市值 + 負債總額－現金及約當現金) / 營業收入淨額 \*100%。 |

---
## 來自營運之現金流量
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 來自營運之現金流量 (選股欄位) |
| 語法 | ``` Value1 = GetField("來自營運之現金流量"); Value1 = GetField("CashFlow－Operating"); Value1 = GetField("CFO"); ``` |
| 欄位分類 | 財務 |
| 單位 | 台股：百萬。美股：百萬(美元) |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 美(股票) |
| 說明 | 現金流量表上的「來自營運之現金流量」欄位。 正值表示現金流入，負值表示現金流出。 提供單季與年度資料。 |

---
## 借款依存度
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 借款依存度 (選股欄位) |
| 語法 | ``` Value1 = GetField("借款依存度"); Value1 = GetField("Debt／Eqity ％"); ``` |
| 欄位分類 | 財務 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 |
| 說明 | 財務比率表上的「借款依存度」欄位。 提供單季與年度資料。 單位為%，例如：借款依存度為150%，則回傳150。 不提供此欄位的公司，輸出值會以零表示。 |

---
## 停業部門損益
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 停業部門損益 (選股欄位) |
| 語法 | ``` Value1 = GetField("停業部門損益"); Value1 = GetField("DiscontinuedDepartmentsGainLoss"); Value1 = GetField("停業單位損益"); ``` |
| 欄位分類 | 財務 |
| 單位 | 百萬 |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 |
| 說明 | 「損益表」中「停業單位損益」項目。 凡停業單位之稅後損益，及構成停業單位之資產或處分群組於按公允價值減出售成本衡量時或於處分時所認列之稅後利益或損失屬之。其為正數，表示停業單位利益之數；其為負數，表示停業單位損失之數。 |

---
## 兌換損失
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 兌換損失 (選股欄位) |
| 語法 | ``` Value1 = GetField("兌換損失"); Value1 = GetField("ExchangeLoss"); ``` |
| 欄位分類 | 財務 |
| 單位 | 百萬 |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 |
| 說明 | 「損益表」中「外幣兌換損失」項目。凡外幣資產或負債因匯率變動實際兌換、評價之損失屬之。 |

---
## 兌換盈益
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 兌換盈益 (選股欄位) |
| 語法 | ``` Value1 = GetField("兌換盈益"); Value1 = GetField("ForeignExchangeGains"); ``` |
| 欄位分類 | 財務 |
| 單位 | 百萬 |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 |
| 說明 | 「損益表」中「外幣兌換利益」項目。凡外幣資產或負債因匯率變動實際兌換或評價之利益屬之。 |

---
## 其他應付款
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 其他應付款 (選股欄位) |
| 語法 | ``` Value1 = GetField("其他應付款"); Value1 = GetField("OtherReceivable"); ``` |
| 欄位分類 | 財務 |
| 單位 | 百萬 |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 |
| 說明 | 「資產負債表」中負債項的「其他應付款」或「應付款項」項目。 |

---
## 其他應收款
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 其他應收款 (選股欄位) |
| 語法 | ``` Value1 = GetField("其他應收款"); Value1 = GetField("BadDebtAllOther"); ``` |
| 欄位分類 | 財務 |
| 單位 | 百萬 |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 |
| 說明 | 「資產負債表」中「流動資產」項內的「其他應收款」項目。 |

---
## 其他收入
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 其他收入 (選股欄位) |
| 語法 | ``` Value1 = GetField("其他收入"); Value1 = GetField("OtherNonOperatingIncome"); ``` |
| 欄位分類 | 財務 |
| 單位 | 百萬 |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 |
| 說明 | 「損益表」中「其他收入－其他」項目。 |

---
## 其他流動負債
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 其他流動負債 (選股欄位) |
| 語法 | ``` Value1 = GetField("其他流動負債"); Value1 = GetField("OtherCurrentLiabilities"); ``` |
| 欄位分類 | 財務 |
| 單位 | 百萬 |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 |
| 說明 | 「資產負債表」中「流動負債」的「其他流動負債」項目。 金融、壽險、產險股不適用此欄位。 |

---
## 其他流動資產
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 其他流動資產 (選股欄位) |
| 語法 | ``` Value1 = GetField("其他流動資產"); Value1 = GetField("OtherCurrentAssets"); ``` |
| 欄位分類 | 財務 |
| 單位 | 百萬 |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 |
| 說明 | 「資產負債表」中「流動資產」項內的「其他流動資產」項目。 金融、壽險、產險股不適用此欄位。 |

---
## 其他資產
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 其他資產 (選股欄位) |
| 語法 | ``` Value1 = GetField("其他資產"); Value1 = GetField("TotalOtherAssets"); Value1 = GetField("其他非流動資產"); ``` |
| 欄位分類 | 財務 |
| 單位 | 百萬 |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 |
| 說明 | 一般、證券股:「資產負債表」中的「其他非流動資產」項目。 金融、壽險、產險:「資產負債表」中的「其他資產」項目。 |

---
## 利息保障倍數
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 利息保障倍數 (選股欄位) |
| 語法 | ``` Value1 = GetField("利息保障倍數"); Value1 = GetField("TimesInterestEarne"); ``` |
| 欄位分類 | 財務 |
| 單位 | 次 |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 |
| 說明 | 財務比率表上的「利息保障倍數」欄位。 提供單季與年度資料。 不提供此欄位的公司，輸出值會以零表示。 |

---
## 利息支出
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 利息支出 (選股欄位) |
| 語法 | ``` Value1 = GetField("利息支出"); Value1 = GetField("InterestExpenses"); ``` |
| 欄位分類 | 財務 |
| 單位 | 百萬 |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 |
| 說明 | 損益表內的項目。 一般產業:「利息支出(不含租賃負債)」。凡非以投資為營業項目者，其舉借各種債務及金融負債所產生之利息等屬之。 金融股:「利息支出」。凡收受存款或舉借其他債務及金融負債所發生之各項利息費用等屬之。 壽險、產險、證券股不適用此欄位。 |

---
## 利息支出率
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 利息支出率 (選股欄位) |
| 語法 | ``` Value1 = GetField("利息支出率"); Value1 = GetField("InterestExpense(％)"); ``` |
| 欄位分類 | 財務 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 |
| 說明 | 財務比率表上的「利息支出率」欄位。 提供單季與年度資料。 只支援證券、期貨與金控個股。不提供此欄位的公司，輸出值會以零表示。 單位為%，例如：利息支出率為25%，則回傳25。 |

---
## 利息收入
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 利息收入 (選股欄位) |
| 語法 | ``` Value1 = GetField("利息收入"); Value1 = GetField("InterestIncome"); ``` |
| 欄位分類 | 財務 |
| 單位 | 百萬 |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 |
| 說明 | 「損益表」中「利息收入」項目。凡非以投資為營業項目者，其各種存款、貸出款項及投資金融資產所產生之利息收入屬之。 證券股不適用此欄位。 |

---
## 加權平均股數
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 加權平均股數 (選股欄位) |
| 語法 | ``` Value1 = GetField("加權平均股數"); Value1 = GetField("WeightedAverageCapitalStock"); ``` |
| 欄位分類 | 財務 |
| 單位 | 台股：百萬股。美股：百萬股 |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 美(股票) |
| 說明 | 「損益表」中「加權平均股數」項目。 加權平均股數是依股數流通期間佔會計期間的比例計算之股數。 |

---
## 員工平均營業額_千元_
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 員工平均營業額(千元) (選股欄位) |
| 語法 | ``` Value1 = GetField("員工平均營業額(千元)"); Value1 = GetField("SalesPerEmployee"); Value1 = GetField("每人營收"); ``` |
| 欄位分類 | 財務 |
| 單位 | 千元 |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 |
| 說明 | 財務比率表內的「每人營收」欄位。 計算公式 : 營業收入／平均員工人數 |

---
## 因子_QMJ安全
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 因子\_QMJ安全 (選股欄位) |
| 語法 | ``` Value1 = GetField("因子_QMJ安全"); Value1 = GetField("Factor_QMJ_SAFETY"); Value1 = GetField("因子_QMJ安全因子"); Value1 = GetField("Factor_QMJ_Safety Factor"); ``` |
| 欄位分類 | 財務 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 月 |
| 支援商品 | 台股 |
| 說明 | 從六個指標(貝他因子、負債比率、股東權益報酬率的波動度、特質波動度、Altman’s Z分數、以及Ohlson's O分數)個別找出每項指標中某檔股票在整體市場的排名，將這六個排名的標準化數值平均，即為該檔股票的綜合安全評分。 |

---
## 因子_QMJ獲利
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 因子\_QMJ獲利 (選股欄位) |
| 語法 | ``` Value1 = GetField("因子_QMJ獲利"); Value1 = GetField("Factor_QMJ_PROFIT"); Value1 = GetField("因子_QMJ獲利因子"); Value1 = GetField("Factor_QMJ_Profitability Factor"); ``` |
| 欄位分類 | 財務 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 月 |
| 支援商品 | 台股 |
| 說明 | 從六個指標(應計項目佔資產比率、自由現金流量佔總資產比率、毛利佔資產比率、毛利佔營收比率、資產報酬率、股東權益報酬率)個別找出每項指標中某檔股票在整體市場的排名，將這六個排名的標準化數值平均，即為該檔股票的綜合獲利能力評分。 |

---
## 因子_一年動能
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 因子\_一年動能 (選股欄位) |
| 語法 | ``` Value1 = GetField("因子_一年動能"); Value1 = GetField("Factor_MOM"); Value1 = GetField("因子_動能因子(以前11個月報酬為基準)"); Value1 = GetField("Factor_Momentum Factor"); ``` |
| 欄位分類 | 財務 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 月 |
| 支援商品 | 台股 |
| 說明 | 股票在過去1年內(不包含當月，共計11個月)的漲跌幅度。 |

---
## 因子_偏態係數
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 因子\_偏態係數 (選股欄位) |
| 語法 | ``` Value1 = GetField("因子_偏態係數"); Value1 = GetField("Factor_TSKEW"); Value1 = GetField("因子_偏態係數因子"); Value1 = GetField("Factor_Total Skewness Factor"); ``` |
| 欄位分類 | 財務 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 月 |
| 支援商品 | 台股 |
| 說明 | 日報酬率序列的三階主動差/日報酬率序列的二階主動差的1.5次方根。 |

---
## 因子_前一年獲利
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 因子\_前一年獲利 (選股欄位) |
| 語法 | ``` Value1 = GetField("因子_前一年獲利"); Value1 = GetField("Factor_PROFITABILITY_LAG1"); Value1 = GetField("因子_獲利因子(以前一年獲利為基準)"); Value1 = GetField("Factor_one year lagged profitability Factor"); ``` |
| 欄位分類 | 財務 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 月 |
| 支援商品 | 台股 |
| 說明 | 一年前的營業利益/三年前的帳面權益。 帳面權益=總資產-總負債+遞延所得稅-特別股股本。 |

---
## 因子_前三年獲利
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 因子\_前三年獲利 (選股欄位) |
| 語法 | ``` Value1 = GetField("因子_前三年獲利"); Value1 = GetField("Factor_PROFITABILITY_LAG3"); Value1 = GetField("因子_獲利因子(以前三年獲利為基準)"); Value1 = GetField("Factor_three year lagged  profitability Factor"); ``` |
| 欄位分類 | 財務 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 月 |
| 支援商品 | 台股 |
| 說明 | 三年前的營業利益/三年前的帳面權益。 帳面權益=總資產-總負債+遞延所得稅-特別股股本。 |

---
## 因子_前兩年獲利
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 因子\_前兩年獲利 (選股欄位) |
| 語法 | ``` Value1 = GetField("因子_前兩年獲利"); Value1 = GetField("Factor_PROFITABILITY_LAG2"); Value1 = GetField("因子_獲利因子(以前兩年獲利為基準)"); Value1 = GetField("Factor_two year lagged  profitability Factor"); ``` |
| 欄位分類 | 財務 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 月 |
| 支援商品 | 台股 |
| 說明 | 兩年前的營業利益/三年前的帳面權益。 帳面權益=總資產-總負債+遞延所得稅-特別股股本。 |

---
## 因子_半年動能因子
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 因子\_半年動能因子 (選股欄位) |
| 語法 | ``` Value1 = GetField("因子_半年動能因子"); Value1 = GetField("Factor_MOM_1Y"); Value1 = GetField("因子_動能因子(以前7~12月報酬為基準)"); Value1 = GetField("Factor_Momentum Factor"); ``` |
| 欄位分類 | 財務 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 月 |
| 支援商品 | 台股 |
| 說明 | 股票在過去1年至過去半年這段期間內(12個月前至7個月前)的漲跌幅度。 |

---
## 因子_實現損益偏態係數
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 因子\_實現損益偏態係數 (選股欄位) |
| 語法 | ``` Value1 = GetField("因子_實現損益偏態係數"); Value1 = GetField("Factor_RDSKEW"); Value1 = GetField("因子_偏態係數因子(以實現損益為基準)"); Value1 = GetField("Factor_Realized skewness Factor"); ``` |
| 欄位分類 | 財務 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 月 |
| 支援商品 | 台股 |
| 說明 | 偏態係數分子/偏態係數分母。 偏態係數分子=過去一個月對數日報酬率的平方加總，再開3/2次方根。 偏態係數分母=過去一個月對數日報酬率的三次方加總，並乘以上個月交易天數的平方根。 |

---
## 因子_帳面市值比
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 因子\_帳面市值比 (選股欄位) |
| 語法 | ``` Value1 = GetField("因子_帳面市值比"); Value1 = GetField("Factor_BM"); Value1 = GetField("因子_帳面市值比因子"); Value1 = GetField("Factor_Book-to-Market ratio Factor"); ``` |
| 欄位分類 | 財務 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 月 |
| 支援商品 | 台股 |
| 說明 | 自然對數(帳面價值/市場價值)。 帳面價值=股東權益總額+遞延所得稅。 市場價值=股票總市值。 |

---
## 因子_彩券型需求
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 因子\_彩券型需求 (選股欄位) |
| 語法 | ``` Value1 = GetField("因子_彩券型需求"); Value1 = GetField("Factor_MAX"); Value1 = GetField("因子_彩券型需求因子"); Value1 = GetField("Factor_Lottery Demand Factor"); ``` |
| 欄位分類 | 財務 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 月 |
| 支援商品 | 台股 |
| 說明 | 過去一個月中最大五筆日報酬率的簡單算數平均。 |

---
## 因子_成交值流動性
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 因子\_成交值流動性 (選股欄位) |
| 語法 | ``` Value1 = GetField("因子_成交值流動性"); Value1 = GetField("Factor_DVOL"); Value1 = GetField("因子_流動性因子(以成交值為基準)"); Value1 = GetField("Factor_Trading volume Factor"); ``` |
| 欄位分類 | 財務 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 月 |
| 支援商品 | 台股 |
| 說明 | 自然對數(過去一個月成交金額加總)。 |

---
## 因子_日週轉率流動性
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 因子\_日週轉率流動性 (選股欄位) |
| 語法 | ``` Value1 = GetField("因子_日週轉率流動性"); Value1 = GetField("Factor_TURNOVER_AVG"); Value1 = GetField("因子_流動性因子(以平均日週轉率為基準)"); Value1 = GetField("Factor_Trading volume Factor"); ``` |
| 欄位分類 | 財務 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 月 |
| 支援商品 | 台股 |
| 說明 | 過去一個月的日週轉率平均值。 日週轉率=日成交量/發行量。 |

---
## 因子_最高價動能
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 因子\_最高價動能 (選股欄位) |
| 語法 | ``` Value1 = GetField("因子_最高價動能"); Value1 = GetField("Factor_MOM_52W"); Value1 = GetField("因子_動能因子(以前52週高價為基準)"); Value1 = GetField("Factor_52-week High Momentum Factor"); ``` |
| 欄位分類 | 財務 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 月 |
| 支援商品 | 台股 |
| 說明 | 當前收盤價與過去52週內最高價的比值。 |

---
## 因子_月週轉率流動性
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 因子\_月週轉率流動性 (選股欄位) |
| 語法 | ``` Value1 = GetField("因子_月週轉率流動性"); Value1 = GetField("Factor_STDTURN"); Value1 = GetField("因子_流動性因子(以股票月週轉率標準差為基準)"); Value1 = GetField("Factor_Standard deviation of share turnover Factor"); ``` |
| 欄位分類 | 財務 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 月 |
| 支援商品 | 台股 |
| 說明 | 自然對數(標準差(股票過去36個月的月週轉率))。 月週轉率=月成交量/發行量。 |

---
## 因子_淨營運資產
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 因子\_淨營運資產 (選股欄位) |
| 語法 | ``` Value1 = GetField("因子_淨營運資產"); Value1 = GetField("Factor_NOA"); Value1 = GetField("因子_淨營運資產因子"); Value1 = GetField("Factor_Net Operating Assets Factor"); ``` |
| 欄位分類 | 財務 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 月 |
| 支援商品 | 台股 |
| 說明 | 淨營運資產/去年同期總資產。 淨營運資產=營運資產-營運負債。 營運資產=資產總額-現金及短期投資。 營運負債=資產總額-短期負債-長期負債-少數股權-特別股股本-普通股股本。 |

---
## 因子_潛在上檔報酬比
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 因子\_潛在上檔報酬比 (選股欄位) |
| 語法 | ``` Value1 = GetField("因子_潛在上檔報酬比"); Value1 = GetField("Factor_UPR"); Value1 = GetField("因子_潛在上檔報酬比率因子"); Value1 = GetField("Factor_Upside Potential Ratio Factor"); ``` |
| 欄位分類 | 財務 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 月 |
| 支援商品 | 台股 |
| 說明 | 將過去1年的日報酬率隨機抽樣1000筆，以正報酬率的加總與過去1年日交易天數的比值作為分子，以負報酬率的平方加總與過去1年日交易天數的比值平方根作為分母，得到的比率即為潛在上檔報酬比率因子。 |

---
## 因子_特質波動度
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 因子\_特質波動度 (選股欄位) |
| 語法 | ``` Value1 = GetField("因子_特質波動度"); Value1 = GetField("Factor_IVOL"); Value1 = GetField("因子_特質波動度因子"); Value1 = GetField("Factor_Idiosyncratic Factor"); ``` |
| 欄位分類 | 財務 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 月 |
| 支援商品 | 台股 |
| 說明 | 透過對歷史股票報酬與市場報酬進行Fama French三因子統計迴歸分析，並計算迴歸殘差值的標準差，即為特質波動度因子。 Fama French三因子是透過將市場上的股票分組後統計出的市場溢酬、規模溢酬、以及價值溢酬。 |

---
## 因子_現金流量波動度
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 因子\_現金流量波動度 (選股欄位) |
| 語法 | ``` Value1 = GetField("因子_現金流量波動度"); Value1 = GetField("Factor_VCF"); Value1 = GetField("因子_現金流量波動度因子"); Value1 = GetField("Factor_Cash flow volatility Factor"); ``` |
| 欄位分類 | 財務 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 月 |
| 支援商品 | 台股 |
| 說明 | 標準差(過去16季之營業現金流量/營業收入)。 |

---
## 因子_股票發行量
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 因子\_股票發行量 (選股欄位) |
| 語法 | ``` Value1 = GetField("因子_股票發行量"); Value1 = GetField("Factor_ISSUE"); Value1 = GetField("因子_股票發行量因子"); Value1 = GetField("Factor_Share issuance Factor"); ``` |
| 欄位分類 | 財務 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 月 |
| 支援商品 | 台股 |
| 說明 | 自然對數(當期股票面額/上季股票面額/前一季流通在外股數)。 |

---
## 因子_貝他值
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 因子\_貝他值 (選股欄位) |
| 語法 | ``` Value1 = GetField("因子_貝他值"); Value1 = GetField("Factor_BETA_1Y"); Value1 = GetField("因子_貝他因子"); Value1 = GetField("Factor_BETA_1year Factor"); ``` |
| 欄位分類 | 財務 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 月 |
| 支援商品 | 台股 |
| 說明 | 過去一年的日報酬率和市場報酬率之共變異數/市場報酬率之變異數。 股票所屬市場視其交易市場而定，上市公司=加權指數報酬率；上櫃公司=櫃買指數報酬率。此因子與「貝他值」不同，是由中興大學財金所提供之因子計算規則計算而得。 |

---
## 因子分數_QMJ安全
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 因子分數\_QMJ安全 (選股欄位) |
| 語法 | ``` Value1 = GetField("因子分數_QMJ安全"); Value1 = GetField("FactorScore_QMJ_SAFETY"); Value1 = GetField("因子分數_QMJ安全因子"); Value1 = GetField("FactorScore_QMJ_Safety Factor"); ``` |
| 欄位分類 | 財務 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 月 |
| 支援商品 | 台股 |
| 說明 | QMJ安全因子經過過濾因子極端值、流動性不足及交易天數不足的商品後的優化排名分數。 |

---
## 因子分數_QMJ獲利
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 因子分數\_QMJ獲利 (選股欄位) |
| 語法 | ``` Value1 = GetField("因子分數_QMJ獲利"); Value1 = GetField("FactorScore_QMJ_PROFIT"); Value1 = GetField("因子分數_QMJ獲利因子"); Value1 = GetField("FactorScore_QMJ_Profitability Factor"); ``` |
| 欄位分類 | 財務 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 月 |
| 支援商品 | 台股 |
| 說明 | QMJ獲利因子經過過濾因子極端值、流動性不足及交易天數不足的商品後的優化排名分數。 |

---
## 因子分數_一年動能
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 因子分數\_一年動能 (選股欄位) |
| 語法 | ``` Value1 = GetField("因子分數_一年動能"); Value1 = GetField("FactorScore_MOM"); Value1 = GetField("因子分數_動能因子(以前11個月報酬為基準)"); Value1 = GetField("FactorScore_Momentum Factor"); ``` |
| 欄位分類 | 財務 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 月 |
| 支援商品 | 台股 |
| 說明 | 動能因子(以前11個月報酬為基準)經過過濾因子極端值、流動性不足及交易天數不足的商品後的優化排名分數。 |

---
## 因子分數_偏態係數
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 因子分數\_偏態係數 (選股欄位) |
| 語法 | ``` Value1 = GetField("因子分數_偏態係數"); Value1 = GetField("FactorScore_TSKEW"); Value1 = GetField("因子分數_偏態係數因子"); Value1 = GetField("FactorScore_Total Skewness Factor"); ``` |
| 欄位分類 | 財務 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 月 |
| 支援商品 | 台股 |
| 說明 | 偏態係數因子經過過濾因子極端值、流動性不足及交易天數不足的商品後的優化排名分數。 |

---
## 因子分數_前一年獲利
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 因子分數\_前一年獲利 (選股欄位) |
| 語法 | ``` Value1 = GetField("因子分數_前一年獲利"); Value1 = GetField("FactorScore_PROFITABILITY_LAG1"); Value1 = GetField("因子分數_獲利因子(以前一年獲利為基準)"); Value1 = GetField("FactorScore_one year lagged profitability Factor"); ``` |
| 欄位分類 | 財務 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 月 |
| 支援商品 | 台股 |
| 說明 | 獲利因子(以前一年獲利為基準)經過過濾因子極端值、流動性不足及交易天數不足的商品後的優化排名分數。 |

---
## 因子分數_前三年獲利
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 因子分數\_前三年獲利 (選股欄位) |
| 語法 | ``` Value1 = GetField("因子分數_前三年獲利"); Value1 = GetField("FactorScore_PROFITABILITY_LAG3"); Value1 = GetField("因子分數_獲利因子(以前三年獲利為基準)"); Value1 = GetField("FactorScore_three year lagged  profitability Factor"); ``` |
| 欄位分類 | 財務 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 月 |
| 支援商品 | 台股 |
| 說明 | 獲利因子(以前三年獲利為基準)經過過濾因子極端值、流動性不足及交易天數不足的商品後的優化排名分數。 |

---
## 因子分數_前兩年獲利
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 因子分數\_前兩年獲利 (選股欄位) |
| 語法 | ``` Value1 = GetField("因子分數_前兩年獲利"); Value1 = GetField("FactorScore_PROFITABILITY_LAG2"); Value1 = GetField("因子分數_獲利因子(以前兩年獲利為基準)"); Value1 = GetField("FactorScore_two year lagged  profitability Factor"); ``` |
| 欄位分類 | 財務 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 月 |
| 支援商品 | 台股 |
| 說明 | 獲利因子(以前兩年獲利為基準)經過過濾因子極端值、流動性不足及交易天數不足的商品後的優化排名分數。 |

---
## 因子分數_半年動能因子
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 因子分數\_半年動能因子 (選股欄位) |
| 語法 | ``` Value1 = GetField("因子分數_半年動能因子"); Value1 = GetField("FactorScore_MOM_1Y"); Value1 = GetField("因子分數_動能因子(以前7~12月報酬為基準)"); Value1 = GetField("FactorScore_Momentum Factor"); ``` |
| 欄位分類 | 財務 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 月 |
| 支援商品 | 台股 |
| 說明 | 動能因子(以前7~12月報酬為基準)經過過濾因子極端值、流動性不足及交易天數不足的商品後的優化排名分數。 |

---
## 因子分數_實現損益偏態係數
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 因子分數\_實現損益偏態係數 (選股欄位) |
| 語法 | ``` Value1 = GetField("因子分數_實現損益偏態係數"); Value1 = GetField("FactorScore_RDSKEW"); Value1 = GetField("因子分數_偏態係數因子(以實現損益為基準)"); Value1 = GetField("FactorScore_Realized skewness Factor"); ``` |
| 欄位分類 | 財務 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 月 |
| 支援商品 | 台股 |
| 說明 | 偏態係數因子(以實現損益為基準)經過過濾因子極端值、流動性不足及交易天數不足的商品後的優化排名分數。 |

---
## 因子分數_帳面市值比
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 因子分數\_帳面市值比 (選股欄位) |
| 語法 | ``` Value1 = GetField("因子分數_帳面市值比"); Value1 = GetField("FactorScore_BM"); Value1 = GetField("因子分數_帳面市值比因子"); Value1 = GetField("FactorScore_Book-to-Market ratio Factor"); ``` |
| 欄位分類 | 財務 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 月 |
| 支援商品 | 台股 |
| 說明 | 帳面市值比因子經過過濾因子極端值、流動性不足及交易天數不足的商品後的優化排名分數。 |

---
## 因子分數_彩券型需求
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 因子分數\_彩券型需求 (選股欄位) |
| 語法 | ``` Value1 = GetField("因子分數_彩券型需求"); Value1 = GetField("FactorScore_MAX"); Value1 = GetField("因子分數_彩券型需求因子"); Value1 = GetField("FactorScore_Lottery Demand Factor"); ``` |
| 欄位分類 | 財務 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 月 |
| 支援商品 | 台股 |
| 說明 | 彩券型需求因子經過過濾因子極端值、流動性不足及交易天數不足的商品後的優化排名分數。 |

---
## 因子分數_成交值流動性
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 因子分數\_成交值流動性 (選股欄位) |
| 語法 | ``` Value1 = GetField("因子分數_成交值流動性"); Value1 = GetField("FactorScore_DVOL"); Value1 = GetField("因子分數_流動性因子(以成交值為基準)"); Value1 = GetField("FactorScore_Trading volume Factor"); ``` |
| 欄位分類 | 財務 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 月 |
| 支援商品 | 台股 |
| 說明 | 流動性因子(以成交值為基準)經過過濾因子極端值、流動性不足及交易天數不足的商品後的優化排名分數。 |

---
## 因子分數_日週轉率流動性
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 因子分數\_日週轉率流動性 (選股欄位) |
| 語法 | ``` Value1 = GetField("因子分數_日週轉率流動性"); Value1 = GetField("FactorScore_TURNOVER_AVG"); Value1 = GetField("因子分數_流動性因子(以平均日週轉率為基準)"); Value1 = GetField("FactorScore_Trading volume Factor"); ``` |
| 欄位分類 | 財務 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 月 |
| 支援商品 | 台股 |
| 說明 | 流動性因子(以平均日週轉率為基準)經過過濾因子極端值、流動性不足及交易天數不足的商品後的優化排名分數。 |

---
## 因子分數_最高價動能
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 因子分數\_最高價動能 (選股欄位) |
| 語法 | ``` Value1 = GetField("因子分數_最高價動能"); Value1 = GetField("FactorScore_MOM_52W"); Value1 = GetField("因子分數_動能因子(以前52週高價為基準)"); Value1 = GetField("FactorScore_52-week High Momentum Factor"); ``` |
| 欄位分類 | 財務 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 月 |
| 支援商品 | 台股 |
| 說明 | 動能因子(以前52週高價為基準)經過過濾因子極端值、流動性不足及交易天數不足的商品後的優化排名分數。 |

---
## 因子分數_月週轉率流動性
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 因子分數\_月週轉率流動性 (選股欄位) |
| 語法 | ``` Value1 = GetField("因子分數_月週轉率流動性"); Value1 = GetField("FactorScore_STDTURN"); Value1 = GetField("因子分數_流動性因子(以股票月週轉率標準差為基準)"); Value1 = GetField("FactorScore_Standard deviation of share turnover Factor"); ``` |
| 欄位分類 | 財務 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 月 |
| 支援商品 | 台股 |
| 說明 | 流動性因子(以股票月週轉率標準差為基準)經過過濾因子極端值、流動性不足及交易天數不足的商品後的優化排名分數。 |

---
## 因子分數_淨營運資產
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 因子分數\_淨營運資產 (選股欄位) |
| 語法 | ``` Value1 = GetField("因子分數_淨營運資產"); Value1 = GetField("FactorScore_NOA"); Value1 = GetField("因子分數_淨營運資產因子"); Value1 = GetField("FactorScore_Net Operating Assets Factor"); ``` |
| 欄位分類 | 財務 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 月 |
| 支援商品 | 台股 |
| 說明 | 淨營運資產因子經過過濾因子極端值、流動性不足及交易天數不足的商品後的優化排名分數。 |

---
## 因子分數_潛在上檔報酬比
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 因子分數\_潛在上檔報酬比 (選股欄位) |
| 語法 | ``` Value1 = GetField("因子分數_潛在上檔報酬比"); Value1 = GetField("FactorScore_UPR"); Value1 = GetField("因子分數_潛在上檔報酬比率因子"); Value1 = GetField("FactorScore_Upside Potential Ratio Factor"); ``` |
| 欄位分類 | 財務 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 月 |
| 支援商品 | 台股 |
| 說明 | 潛在上檔報酬比率因子經過過濾因子極端值、流動性不足及交易天數不足的商品後的優化排名分數。 |

---
## 因子分數_特質波動度
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 因子分數\_特質波動度 (選股欄位) |
| 語法 | ``` Value1 = GetField("因子分數_特質波動度"); Value1 = GetField("FactorScore_IVOL"); Value1 = GetField("因子分數_特質波動度因子"); Value1 = GetField("FactorScore_Idiosyncratic Factor"); ``` |
| 欄位分類 | 財務 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 月 |
| 支援商品 | 台股 |
| 說明 | 特質波動度因子經過過濾因子極端值、流動性不足及交易天數不足的商品後的優化排名分數。 |

---
## 因子分數_現金殖利率
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 因子分數\_現金殖利率 (選股欄位) |
| 語法 | ``` Value1 = GetField("因子分數_現金殖利率"); Value1 = GetField("FactorScore_DY"); Value1 = GetField("因子分數_現金殖利率因子"); Value1 = GetField("FactorScore_Dividend Yield"); ``` |
| 欄位分類 | 財務 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 月 |
| 支援商品 | 台股 |
| 說明 | 現金殖利率因子經過過濾因子極端值、流動性不足及交易天數不足的商品後的優化排名分數。 |

---
## 因子分數_現金流量波動度
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 因子分數\_現金流量波動度 (選股欄位) |
| 語法 | ``` Value1 = GetField("因子分數_現金流量波動度"); Value1 = GetField("FactorScore_VCF"); Value1 = GetField("因子分數_現金流量波動度因子"); Value1 = GetField("FactorScore_Cash flow volatility Factor"); ``` |
| 欄位分類 | 財務 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 月 |
| 支援商品 | 台股 |
| 說明 | 現金流量波動度因子經過過濾因子極端值、流動性不足及交易天數不足的商品後的優化排名分數。 |

---
## 因子分數_股東權益報酬率
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 因子分數\_股東權益報酬率 (選股欄位) |
| 語法 | ``` Value1 = GetField("因子分數_股東權益報酬率"); Value1 = GetField("FactorScore_ROE"); Value1 = GetField("因子分數_股東權益報酬率因子"); Value1 = GetField("FactorScore_Return on Equity"); ``` |
| 欄位分類 | 財務 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 月 |
| 支援商品 | 台股 |
| 說明 | 股東權益報酬率因子經過過濾因子極端值、流動性不足及交易天數不足的商品後的優化排名分數。 |

---
## 因子分數_股票發行量
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 因子分數\_股票發行量 (選股欄位) |
| 語法 | ``` Value1 = GetField("因子分數_股票發行量"); Value1 = GetField("FactorScore_ISSUE"); Value1 = GetField("因子分數_股票發行量因子"); Value1 = GetField("FactorScore_Share issuance Factor"); ``` |
| 欄位分類 | 財務 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 月 |
| 支援商品 | 台股 |
| 說明 | 股票發行量因子經過過濾因子極端值、流動性不足及交易天數不足的商品後的優化排名分數。 |

---
## 因子分數_貝他值
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 因子分數\_貝他值 (選股欄位) |
| 語法 | ``` Value1 = GetField("因子分數_貝他值"); Value1 = GetField("FactorScore_BETA_1Y"); Value1 = GetField("因子分數_貝他因子"); Value1 = GetField("FactorScore_BETA_1year Factor"); ``` |
| 欄位分類 | 財務 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 月 |
| 支援商品 | 台股 |
| 說明 | 貝他因子經過過濾因子極端值、流動性不足及交易天數不足的商品後的優化排名分數。 |

---
## 固定資產
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 固定資產 (選股欄位) |
| 語法 | ``` Value1 = GetField("固定資產"); Value1 = GetField("TotalFixedAssets"); Value1 = GetField("不動產廠房及設備"); ``` |
| 欄位分類 | 財務 |
| 單位 | 台股：百萬。美股：百萬(美元) |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 美(股票) |
| 說明 | 台股:「資產負債表」中的「不動產及設備淨額」項目。 美股: 「資產負債表」中的「財產，機器及設備淨額」項目。 |

---
## 固定資產報酬率
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 固定資產報酬率 (選股欄位) |
| 語法 | ``` Value1 = GetField("固定資產報酬率"); Value1 = GetField("ReturnOnFixedAsset"); ``` |
| 欄位分類 | 財務 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 |
| 說明 | 計算公式: 稅後淨利／固定資產\*100%。 取單期財報數值來計算。 |

---
## 固定資產成長率
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 固定資產成長率 (選股欄位) |
| 語法 | ``` Value1 = GetField("固定資產成長率"); Value1 = GetField("YOY％－FixedAssets"); Value1 = GetField("折舊性FA成長率"); ``` |
| 欄位分類 | 財務 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 |
| 說明 | 固定資產，是資產負債表中非流動資產項下的"不動產廠房及設備"。 固定資產成長率，表示當期"不動產廠房及設備"較去年同期的成長幅度。 提供季度與年度資料。 例如：今年第一季"不動產廠房及設備"為100億、去年第一季為80億，固定資產成長率為(100-80)/80\*100% = 25%，單位為%，則回傳25。 |

---
## 固定資產週轉率_次_
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 固定資產週轉率(次) (選股欄位) |
| 語法 | ``` Value1 = GetField("固定資產週轉率(次)"); Value1 = GetField("FixedAssetTurnover"); ``` |
| 欄位分類 | 財務 |
| 單位 | 次 |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 |
| 說明 | 財務比率表上的「固定資產週轉次數」欄位。 提供單季與年度資料。 不提供此欄位的公司，輸出值會以零表示。 |

---
## 外幣換算調整數
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 外幣換算調整數 (選股欄位) |
| 語法 | ``` Value1 = GetField("外幣換算調整數"); Value1 = GetField("AdjustmentForFX"); Value1 = GetField("國外機構報表換算差額"); ``` |
| 欄位分類 | 財務 |
| 單位 | 百萬 |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 |
| 說明 | 「資產負債表」股東權益項目中的「國外機構報表換算差額」。 金融、壽險、產險、證券股才適用此欄位。 |

---
## 外銷比率
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 外銷比率 (選股欄位) |
| 語法 | ``` Value1 = GetField("外銷比率"); Value1 = GetField("ExportRatio"); ``` |
| 欄位分類 | 財務 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 年 |
| 支援商品 | 台股 |
| 說明 | 外銷占營收的比例。 例如：公司年度總營收為100億，外銷地區的總營收為80億，外銷比率為80%，單位為％，則回傳80。 |

---
## 存貨
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 存貨 (選股欄位) |
| 語法 | ``` Value1 = GetField("存貨"); Value1 = GetField("Inventories"); ``` |
| 欄位分類 | 財務 |
| 單位 | 百萬 |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 |
| 說明 | 「資產負債表」中「流動資產」項內的「存貨」項目。 金融、壽險、產險、證券股不適用此欄位。 |

---
## 存貨及應收帳款_淨值
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 存貨及應收帳款／淨值 (選股欄位) |
| 語法 | ``` Value1 = GetField("存貨及應收帳款／淨值"); Value1 = GetField("Inventory"); ``` |
| 欄位分類 | 財務 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 |
| 說明 | 財務比率表上的「存貨及應收帳款/淨值」欄位。 提供單季與年度資料。 單位為%，例如：存貨及應收帳款/淨值為5%，則回傳5。 不提供此欄位的公司，輸出值會以零表示。 |

---
## 存貨營收比
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 存貨營收比 (選股欄位) |
| 語法 | ``` Value1 = GetField("存貨營收比"); Value1 = GetField("Inventory／Sales"); ``` |
| 欄位分類 | 財務 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 年 |
| 支援商品 | 台股 |
| 說明 | 計算公式: 存貨之商品及製成品 / 營業收入淨額\*100%。 |

---
## 存貨週轉率_次_
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 存貨週轉率(次) (選股欄位) |
| 語法 | ``` Value1 = GetField("存貨週轉率(次)"); Value1 = GetField("InventoryTurnover"); ``` |
| 欄位分類 | 財務 |
| 單位 | 次 |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 |
| 說明 | 財務比率表上的「存貨週轉率(次)」欄位。 提供單季與年度資料。 不提供此欄位的公司，輸出值會以零表示。 |

---
## 市值營收比
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 市值營收比 (選股欄位) |
| 語法 | ``` Value1 = GetField("市值營收比"); Value1 = GetField("MarketValue／Sales"); ``` |
| 欄位分類 | 財務 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 |
| 說明 | 計算公式: 市值 / 當期營業收入淨額。 |

---
## 市研率
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 市研率 (選股欄位) |
| 語法 | ``` Value1 = GetField("市研率"); Value1 = GetField("OperatingExpense－R＆D／MarketValue"); ``` |
| 欄位分類 | 財務 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 |
| 說明 | 計算公式: 研發費用 / 總市值\*100%。 |

---
## 常續性利益_稅後_
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 常續性利益(稅後) (選股欄位) |
| 語法 | ``` Value1 = GetField("常續性利益(稅後)"); Value1 = GetField("NetIncome－ExcDispo"); ``` |
| 欄位分類 | 財務 |
| 單位 | 百萬 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 |
| 說明 | 「損益表」中「常續性稅後淨利」項目。 |

---
## 平均售貨天數
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 平均售貨天數 (選股欄位) |
| 語法 | ``` Value1 = GetField("平均售貨天數"); Value1 = GetField("DaysInventoryOutstanding"); ``` |
| 欄位分類 | 財務 |
| 單位 | 天 |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 |
| 說明 | 單期財務比率表上的"平均售貨天數"欄位。 |

---
## 平均收帳天數
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 平均收帳天數 (選股欄位) |
| 語法 | ``` Value1 = GetField("平均收帳天數"); Value1 = GetField("DaysReceivablesOutstanding"); ``` |
| 欄位分類 | 財務 |
| 單位 | 天 |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 |
| 說明 | 財務比率表上的"平均收帳天數"欄位。 |

---
## 年報酬率
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 年報酬率 (選股欄位) |
| 語法 | ``` Value1 = GetField("年報酬率",param:=1); Value1 = GetField("YearlyReturnRate",param:=1); ``` |
| 參數範圍 | 1、3、5、10 |
| 欄位分類 | 財務 |
| 單位 | % |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 單一 |
| 支援商品 | 台股 美(ETF) |
| 說明 | 僅支援台/美ETF商品. 統計近N年報酬率, 計算方式包含配息, 以及配息再投入. 欄位提供參數, 可以傳入年度, 支援1,3,5,10，預設是 1。 (如欲計算股票的報酬率, 可以使用還原股價直接計算, 系統會提供內建函數) |

---
## 庫藏股票帳面值
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 庫藏股票帳面值 (選股欄位) |
| 語法 | ``` Value1 = GetField("庫藏股票帳面值"); Value1 = GetField("TreasuryStock"); ``` |
| 欄位分類 | 財務 |
| 單位 | 百萬 |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 |
| 說明 | 「資產負債表」股東權益項目中的「庫藏股票帳面值」。 |

---
## 應付商業本票
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 應付商業本票 (選股欄位) |
| 語法 | ``` Value1 = GetField("應付商業本票"); Value1 = GetField("BillsIssued"); ``` |
| 欄位分類 | 財務 |
| 單位 | 百萬 |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 |
| 說明 | 「資產負債表」中「流動負債」項內「應付商業本票∕承兌匯票」項目。 金融、壽險、產險股不適用此欄位。 |

---
## 應付帳款付現天數
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 應付帳款付現天數 (選股欄位) |
| 語法 | ``` Value1 = GetField("應付帳款付現天數"); Value1 = GetField("Days－A／P Turnover"); ``` |
| 欄位分類 | 財務 |
| 單位 | 天 |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 |
| 說明 | 財務比率表上的「應付帳款付現天數」欄位。 提供單季與年度資料。 不提供此欄位的公司，輸出值會以零表示。 |

---
## 應付帳款及票據
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 應付帳款及票據 (選股欄位) |
| 語法 | ``` Value1 = GetField("應付帳款及票據"); Value1 = GetField("AccountsPayable＆NotesPayable"); ``` |
| 欄位分類 | 財務 |
| 單位 | 百萬 |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 |
| 說明 | 「資產負債表」中「應付帳款及票據」項目。 金融股不適用此欄位。 |

---
## 應收帳款及票據
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 應收帳款及票據 (選股欄位) |
| 語法 | ``` Value1 = GetField("應收帳款及票據"); Value1 = GetField("AccountsReceivable＆NotesReceivable"); ``` |
| 欄位分類 | 財務 |
| 單位 | 百萬 |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 |
| 說明 | 「資產負債表」中「流動資產」項內的「應收帳款及票據」項目。 銀行與金控股不適用此欄位。 |

---
## 應收帳款週轉次數
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 應收帳款週轉次數 (選股欄位) |
| 語法 | ``` Value1 = GetField("應收帳款週轉次數"); Value1 = GetField("AccountsReceivablesTurnover"); ``` |
| 欄位分類 | 財務 |
| 單位 | 次 |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 |
| 說明 | 財務比率表上的"應收帳款週轉次"欄位。 |

---
## 應收帳款週轉率_次_
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 應收帳款週轉率(次) (選股欄位) |
| 語法 | ``` Value1 = GetField("應收帳款週轉率(次)"); Value1 = GetField("AccountsReceivableNotesReceivableTurnover"); ``` |
| 欄位分類 | 財務 |
| 單位 | 次 |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 |
| 說明 | 財務比率表上的「應收帳款週轉次」欄位。 提供單季與年度資料。 不提供此欄位的公司，輸出值會以零表示。 |

---
## 或有負債比率
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 或有負債比率 (選股欄位) |
| 語法 | ``` Value1 = GetField("或有負債比率"); Value1 = GetField("Contingent Liab／Equity"); ``` |
| 欄位分類 | 財務 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 年 |
| 支援商品 | 台股 |
| 說明 | 財務比率表上的「或有負債/淨值」欄位。 只提供年度資料。 單位為%，例如：或有負債比率為150%，則回傳150。 不提供此欄位的公司，輸出值會以零表示。 |

---
## 所得稅費用
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 所得稅費用 (選股欄位) |
| 語法 | ``` Value1 = GetField("所得稅費用"); Value1 = GetField("IncomeTaxExpense"); ``` |
| 欄位分類 | 財務 |
| 單位 | 百萬 |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 |
| 說明 | 「損益表」中「所得稅費用」項目。 凡本期稅前淨利（或淨損），依所得稅法等有關規定核算應認列之所得稅費用（或利益）屬之。 |

---
## 投資活動之現金流量
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 投資活動之現金流量 (選股欄位) |
| 語法 | ``` Value1 = GetField("投資活動之現金流量"); Value1 = GetField("CashFlowInvestment"); Value1 = GetField("CFI"); ``` |
| 欄位分類 | 財務 |
| 單位 | 台股：百萬。美股：百萬(美元) |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 美(股票) |
| 說明 | 現金流量表上的「投資活動之現金流量」欄位。 正值表示現金流入，負值表示現金流出。 提供單季與年度資料。 |

---
## 投資跌價損失
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 投資跌價損失 (選股欄位) |
| 語法 | ``` Value1 = GetField("投資跌價損失"); Value1 = GetField("LossonInvestmentRevaluation"); Value1 = GetField("金融資產減損損失"); ``` |
| 欄位分類 | 財務 |
| 單位 | 百萬 |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 |
| 說明 | 「損益表」中「金融資產減損損失」項目。 |

---
## 投資跌價損失回轉
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 投資跌價損失回轉 (選股欄位) |
| 語法 | ``` Value1 = GetField("投資跌價損失回轉"); Value1 = GetField("RevofLossonInvestment"); Value1 = GetField("金融資產減損迴轉利益"); ``` |
| 欄位分類 | 財務 |
| 單位 | 百萬 |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 |
| 說明 | 「損益表」中「金融資產減損迴轉利益」項目。 |

---
## 推銷費用
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 推銷費用 (選股欄位) |
| 語法 | ``` Value1 = GetField("推銷費用"); Value1 = GetField("OperatingExpense－Promotion"); ``` |
| 欄位分類 | 財務 |
| 單位 | 百萬 |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 |
| 說明 | 「損益表」中「推銷費用」項目。凡各項行銷費用屬之。 金融、證券股不適用此欄位。 |

---
## 普通股股本
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 普通股股本 (選股欄位) |
| 語法 | ``` Value1 = GetField("普通股股本"); Value1 = GetField("CapitalCommonStocks"); ``` |
| 欄位分類 | 財務 |
| 單位 | 台股：億元。美股：百萬(美元) |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 美(股票) |
| 說明 | 台股:「資產負債表」股東權益項目中的「普通股股本」。 美股:「資產負債表」股東權益項目中的「普通股股東權益」。 |

---
## 有息負債利率
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 有息負債利率 (選股欄位) |
| 語法 | ``` Value1 = GetField("有息負債利率"); Value1 = GetField("Int.Exp.／Debt"); ``` |
| 欄位分類 | 財務 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 |
| 說明 | 財務比率表上的「有息負債利率」欄位。 提供單季與年度資料。 例如：有息負債利率為1.5%，單位為%，則回傳1.5。 不提供此欄位的公司，輸出值會以零表示。 |

---
## 有效稅率
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 有效稅率 (選股欄位) |
| 語法 | ``` Value1 = GetField("有效稅率"); Value1 = GetField("Tax Rate"); ``` |
| 欄位分類 | 財務 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 |
| 說明 | 財務比率表上的「稅率(A)」欄位。 提供單季與年度資料。 若稅前淨利或所得稅費用為負或0、所得稅費用大於稅前淨利等特殊情況，資料會呈現為0。 |

---
## 未分配盈餘
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 未分配盈餘 (選股欄位) |
| 語法 | ``` Value1 = GetField("未分配盈餘"); Value1 = GetField("UnappropriatedRetainedEarnings"); ``` |
| 欄位分類 | 財務 |
| 單位 | 百萬 |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 |
| 說明 | 「資產負債表」股東權益項目中的「未分配盈餘」。 |

---
## 未完工程及預付款
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 未完工程及預付款 (選股欄位) |
| 語法 | ``` Value1 = GetField("未完工程及預付款"); Value1 = GetField("ConstructionProcess＆Prepayment"); Value1 = GetField("預付設備款"); ``` |
| 欄位分類 | 財務 |
| 單位 | 百萬 |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 |
| 說明 | 「資產負債表」中「非流動資產」項內的「預付設備款」項目。 金融、壽險、產險、證券股不適用此欄位。 |

---
## 本期稅後淨利
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 本期稅後淨利 (選股欄位) |
| 語法 | ``` Value1 = GetField("本期稅後淨利"); Value1 = GetField("NetIncome"); Value1 = GetField("歸屬母公司淨利(損)"); ``` |
| 欄位分類 | 財務 |
| 單位 | 台股：百萬。美股：百萬(美元) |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 美(股票) |
| 說明 | 「損益表」中「歸屬母公司淨利（損）」項目。 凡本期淨利（或淨損）歸屬於母公司業主之當期分攤數屬之。本期淨利（淨損）為繼續營業單位本期淨利（或淨損）加計停業單位損益之餘額屬之。 |

---
## 杜邦型ROA
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 杜邦型ROA (選股欄位) |
| 語法 | ``` Value1 = GetField("杜邦型ROA"); Value1 = GetField("ROA－DuPont?Analysis"); ``` |
| 欄位分類 | 財務 |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 |
| 說明 | 計算公式: 總資產週轉率(次) \* 稅後淨利率。 |

---
## 杜邦型ROE
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 杜邦型ROE (選股欄位) |
| 語法 | ``` Value1 = GetField("杜邦型ROE"); Value1 = GetField("ROE－DuPont?Analysis"); ``` |
| 欄位分類 | 財務 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 |
| 說明 | 計算公式: 總資產週轉率(次) \* 稅後淨利率 \* (資產總額 / 股東權益總額)。 |

---
## 每人營業利益
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 每人營業利益 (選股欄位) |
| 語法 | ``` Value1 = GetField("每人營業利益"); Value1 = GetField("OperatingIncomePerEmployee"); ``` |
| 欄位分類 | 財務 |
| 單位 | 千元 |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 |
| 說明 | 財務比率表上的「每人營業利益」欄位。 提供單季與年度資料。 不提供此欄位的公司，輸出值會以零表示。 |

---
## 每股流動淨資產
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 每股流動淨資產 (選股欄位) |
| 語法 | ``` Value1 = GetField("每股流動淨資產"); Value1 = GetField("NetCurrentAssetsPerShare"); ``` |
| 欄位分類 | 財務 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 |
| 說明 | 計算公式: (流動資產－全部負債) ／ 普通股股數。 |

---
## 每股淨值_元_
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 每股淨值(元) (選股欄位) |
| 語法 | ``` Value1 = GetField("每股淨值(元)"); Value1 = GetField("BookValuePerShare"); ``` |
| 欄位分類 | 財務 |
| 單位 | 台股：元。美股：元(美元) |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 美(股票) |
| 說明 | 台股，是財務比率表的「每股淨值(F)(TSE公告數)」項目。 美股，是財務比率表上的「每股淨值」項目。 提供單季與年度資料。 |

---
## 每股營業利益_元_
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 每股營業利益(元) (選股欄位) |
| 語法 | ``` Value1 = GetField("每股營業利益(元)"); Value1 = GetField("OperatingIncomePerShare"); ``` |
| 欄位分類 | 財務 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 |
| 說明 | 財務比率表上的「每股營業利益(元)」欄位。 提供單季與年度資料。 |

---
## 每股營業額_元_
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 每股營業額(元) (選股欄位) |
| 語法 | ``` Value1 = GetField("每股營業額(元)"); Value1 = GetField("SalesPerShare"); ``` |
| 欄位分類 | 財務 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 |
| 說明 | 財務比率表上的「每股營業額(元)」欄位。 提供單季與年度資料。 |

---
## 每股現金流量
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 每股現金流量 (選股欄位) |
| 語法 | ``` Value1 = GetField("每股現金流量"); Value1 = GetField("CashflowPerShare"); ``` |
| 欄位分類 | 財務 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 |
| 說明 | 財務比率表上的「每股現金流量」欄位。 提供單季與年度資料。 |

---
## 每股稅前淨利_元_
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 每股稅前淨利(元) (選股欄位) |
| 語法 | ``` Value1 = GetField("每股稅前淨利(元)"); Value1 = GetField("Pre_TaxIncomePerShare"); ``` |
| 欄位分類 | 財務 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 |
| 說明 | 財務比率表上的「每股稅前淨利(元)」欄位。 提供單季與年度資料。 |

---
## 每股稅後淨利_元_
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 每股稅後淨利(元) (選股欄位) |
| 語法 | ``` Value1 = GetField("每股稅後淨利(元)"); Value1 = GetField("EPS"); Value1 = GetField("EarningPerShare"); ``` |
| 欄位分類 | 財務 |
| 單位 | 台股：元。美股：元(美元) |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 美(股票) |
| 說明 | 台股，是損益表上的「每股盈餘」項目。 美股，是損益表上的「基本每股盈餘(由淨利計算)」項目。 提供單季與年度資料。 |

---
## 每股自由現金流量
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 每股自由現金流量 (選股欄位) |
| 語法 | ``` Value1 = GetField("每股自由現金流量"); Value1 = GetField("FCFFPerShare"); ``` |
| 欄位分類 | 財務 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 |
| 說明 | 計算公式：自由現金流量／加權平均股數 |

---
## 法定盈餘公積
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 法定盈餘公積 (選股欄位) |
| 語法 | ``` Value1 = GetField("法定盈餘公積"); Value1 = GetField("LegalReserve"); ``` |
| 欄位分類 | 財務 |
| 單位 | 百萬 |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 |
| 說明 | 「資產負債表」股東權益項目中的「法定盈餘公積」。 |

---
## 流動比率
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 流動比率 (選股欄位) |
| 語法 | ``` Value1 = GetField("流動比率"); Value1 = GetField("CurrentRatio"); ``` |
| 欄位分類 | 財務 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 美(股票) |
| 說明 | 財務比率表上的「流動比率」欄位。 提供季度與年度資料。 單位為%，例如：流動比率為250%，則回傳250。 不提供此欄位的公司，輸出值會以零表示。 |

---
## 流動負債
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 流動負債 (選股欄位) |
| 語法 | ``` Value1 = GetField("流動負債"); Value1 = GetField("TotalCurrentLiabilities"); ``` |
| 欄位分類 | 財務 |
| 單位 | 台股：百萬。美股：百萬(美元) |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 美(股票) |
| 說明 | 「資產負債表」中「流動負債」項目。凡將於一年或一營業週期內（以較長者為準）以流動資產償還或再轉為流動負債皆屬之。 台股中金融、壽險、產險股不適用此欄位。 |

---
## 流動資產
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 流動資產 (選股欄位) |
| 語法 | ``` Value1 = GetField("流動資產"); Value1 = GetField("TotalCurrentAssets"); ``` |
| 欄位分類 | 財務 |
| 單位 | 台股：百萬。美股：百萬(美元) |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 美(股票) |
| 說明 | 「資產負債表」中「流動資產」項目。凡現金及其他將於一年或一營業週期內（以較長者為準）變現、出售或耗用之資產皆屬之。 台股中金融、壽險、產險股不適用此欄位。 |

---
## 淨值成長率
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 淨值成長率 (選股欄位) |
| 語法 | ``` Value1 = GetField("淨值成長率"); Value1 = GetField("YOY％－TotalEquity"); ``` |
| 欄位分類 | 財務 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 |
| 說明 | 財務比率表上的「淨值成長率」欄位。表示當期股東權益總額較去年同期股東權益總額的成長幅度。 股東權益＝資產總額－負債總額，也稱為淨值或凈資產。 淨值成長率，提供季度與年度資料。 例如：今年第一季股東權益總額100億、去年第一季股東權益總額80億，淨值成長率為(100-80)/80\*100% = 25%，單位為%，則回傳25。 |

---
## 淨值自由現金流量比
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 淨值自由現金流量比 (選股欄位) |
| 語法 | ``` Value1 = GetField("淨值自由現金流量比"); Value1 = GetField("NetValue／FCF"); ``` |
| 欄位分類 | 財務 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 |
| 說明 | 計算公式: 股東權益總額 / 自由現金流量\*100%。 |

---
## 淨值週轉率
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 淨值週轉率 (選股欄位) |
| 語法 | ``` Value1 = GetField("淨值週轉率"); Value1 = GetField("EquityTurnover"); ``` |
| 欄位分類 | 財務 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 |
| 說明 | 財務比率表上的「淨值週轉率(次)」欄位。 提供單季與年度資料。 不提供此欄位的公司，輸出值會以零表示。 |

---
## 淨營業週期
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 淨營業週期 (選股欄位) |
| 語法 | ``` Value1 = GetField("淨營業週期"); Value1 = GetField("NetOperatingCycle"); ``` |
| 欄位分類 | 財務 |
| 單位 | 天 |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 |
| 說明 | 財務比率表上的"淨營業週期(日)"欄位。 |

---
## 無形資產
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 無形資產 (選股欄位) |
| 語法 | ``` Value1 = GetField("無形資產"); Value1 = GetField("IntangibleAssets"); ``` |
| 欄位分類 | 財務 |
| 單位 | 台股：百萬。美股：百萬(美元) |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 美(股票) |
| 說明 | 台股:「資產負債表」中「非流動資產」項內「其他無形資產淨額」項目。 美股:「資產負債表」中「非流動資產」項內「商譽與無形資產淨額」項目。 不提供此欄位的公司，輸出值會以零表示。 |

---
## 營收成長率
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 營收成長率 (選股欄位) |
| 語法 | ``` Value1 = GetField("營收成長率"); Value1 = GetField("YOY％－Sales"); ``` |
| 欄位分類 | 財務 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 |
| 說明 | 財務比率表上的「營收成長率」欄位。表示當期營收較去年同期營收的成長幅度。 提供單季與年度資料。 例如：今年第一季營收100億、去年第一季營收80億，營收成長率為(100-80)/80\*100% = 25%，單位為%，則回傳25。 |

---
## 營業利益
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 營業利益 (選股欄位) |
| 語法 | ``` Value1 = GetField("營業利益"); Value1 = GetField("OperatingIncome"); ``` |
| 欄位分類 | 財務 |
| 單位 | 台股：百萬。美股：百萬(美元) |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 美(股票) |
| 說明 | 「損益表」中「營業利益」項目。 |

---
## 營業利益成長率
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 營業利益成長率 (選股欄位) |
| 語法 | ``` Value1 = GetField("營業利益成長率"); Value1 = GetField("YOY％－OperatingIncome"); ``` |
| 欄位分類 | 財務 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 |
| 說明 | 財務比率表上的「營業利益成長率」欄位。表示當期營業利益較去年同期營業利益的成長幅度。 提供單季與年度資料。 例如：今年第一季營業利益100億、去年第一季營業利益80億，營業利益成長率為(100-80)/80\*100% = 25%，單位為%，則回傳25。 |

---
## 營業利益率
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 營業利益率 (選股欄位) |
| 語法 | ``` Value1 = GetField("營業利益率"); Value1 = GetField("OperatingProfitMargin"); ``` |
| 欄位分類 | 財務 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 美(股票) |
| 說明 | 財務比率表上的「營業利益率」欄位。 提供單季與年度資料。 不提供此欄位的公司，輸出值會以零表示。 單位為%，例如：營業利益率為30%，則回傳30。 |

---
## 營業外收入及支出
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 營業外收入及支出 (選股欄位) |
| 語法 | ``` Value1 = GetField("營業外收入及支出"); Value1 = GetField("NonOperatingIncome&Expense"); ``` |
| 欄位分類 | 財務 |
| 單位 | 百萬 |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 |
| 說明 | 「綜合損益表」財務報表項目。 |

---
## 營業外收入合計
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 營業外收入合計 (選股欄位) |
| 語法 | ``` Value1 = GetField("營業外收入合計"); Value1 = GetField("TotalNonOperatingIncome"); ``` |
| 欄位分類 | 財務 |
| 單位 | 百萬 |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 |
| 說明 | 「損益表」中「其他收入」項目。凡投資性不動產收入、租賃收入、代理收入及處分投資性不動產利益等屬之。 |

---
## 營業成本
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 營業成本 (選股欄位) |
| 語法 | ``` Value1 = GetField("營業成本"); Value1 = GetField("CostOfGoodsSold"); ``` |
| 欄位分類 | 財務 |
| 單位 | 台股：百萬。美股：百萬(美元) |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 美(股票) |
| 說明 | 凡銷售貨物、提供勞務及金融保險業務等各項成本皆屬之。 台股:「損益表」中的「營業成本」。 美股:「損益表」中「銷貨成本」項目。 台股中金融、證券股不適用此欄位。 |

---
## 營業收入淨額
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 營業收入淨額 (選股欄位) |
| 語法 | ``` Value1 = GetField("營業收入淨額"); Value1 = GetField("NetSales"); ``` |
| 欄位分類 | 財務 |
| 單位 | 台股：百萬。美股：百萬(美元) |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 美(股票) |
| 說明 | 損益表內的項目。凡銷售貨物、供應勞務及金融保險服務等所獲得之收入皆屬之。 台股: 一般產業:「營業收入淨額」 金融股:「利息淨收益+利息以外淨損益」 壽險股:「營業收入」 證券股:「收益合計」  美股:「營業收入總額」 |

---
## 營業毛利
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 營業毛利 (選股欄位) |
| 語法 | ``` Value1 = GetField("營業毛利"); Value1 = GetField("GrossProfit"); ``` |
| 欄位分類 | 財務 |
| 單位 | 台股：百萬。美股：百萬(美元) |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 美(股票) |
| 說明 | 「損益表」中「營業毛利」項目。 台股中金融、證券股不適用此欄位。 |

---
## 營業毛利率
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 營業毛利率 (選股欄位) |
| 語法 | ``` Value1 = GetField("營業毛利率"); Value1 = GetField("GrossMargin"); ``` |
| 欄位分類 | 財務 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 美(股票) |
| 說明 | 財務比率表上的「營業毛利率」欄位。 提供單季與年度資料。 不提供此欄位的公司，輸出值會以零表示。 單位是％，例如：營業毛利率為30%，則回傳30。 |

---
## 營業現金流量_營業利益
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 營業現金流量／營業利益 (選股欄位) |
| 語法 | ``` Value1 = GetField("營業現金流量／營業利益"); Value1 = GetField("CFO／OperatingIncome"); ``` |
| 欄位分類 | 財務 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 |
| 說明 | 計算公式: 營業現金流量／營業利益\*100%。 提供單季與年度資料。 單位為%，例如：數值為25%，則回傳25。 |

---
## 營業費用
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 營業費用 (選股欄位) |
| 語法 | ``` Value1 = GetField("營業費用"); Value1 = GetField("OperatingExpenses"); ``` |
| 欄位分類 | 財務 |
| 單位 | 百萬 |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 |
| 說明 | 「損益表」中「營業費用」項目。凡配合產銷營運所發生之各項營業費用皆屬之。 證券股不適用此欄位。 |

---
## 營業費用率
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 營業費用率 (選股欄位) |
| 語法 | ``` Value1 = GetField("營業費用率"); Value1 = GetField("Operating Exp. ％"); ``` |
| 欄位分類 | 財務 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 |
| 說明 | 財務比率表上的「營業費用率」欄位。 提供單季與年度資料。 單位為%，例如：營業費用率為15%，則回傳15。 |

---
## 營運資金
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 營運資金 (選股欄位) |
| 語法 | ``` Value1 = GetField("營運資金"); Value1 = GetField("WorkingCapital"); ``` |
| 欄位分類 | 財務 |
| 單位 | 台股:百萬；美股:百萬(美元) |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 |
| 說明 | 計算公式 : 流動資產 - 流動負債 |

---
## 特別盈餘公積
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 特別盈餘公積 (選股欄位) |
| 語法 | ``` Value1 = GetField("特別盈餘公積"); Value1 = GetField("AppropriatedRetainedEarnings"); ``` |
| 欄位分類 | 財務 |
| 單位 | 百萬 |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 |
| 說明 | 「資產負債表」股東權益項目中的「特別盈餘公積」。 |

---
## 特別股股本
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 特別股股本 (選股欄位) |
| 語法 | ``` Value1 = GetField("特別股股本"); Value1 = GetField("CapitalPreferredStocks"); ``` |
| 欄位分類 | 財務 |
| 單位 | 億元。 |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 |
| 說明 | 「資產負債表」股東權益項目中的「特別股股本」。 |

---
## 現金再投資_
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 現金再投資％ (選股欄位) |
| 語法 | ``` Value1 = GetField("現金再投資％"); Value1 = GetField("CashReinvestmentRatio"); ``` |
| 欄位分類 | 財務 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 年 |
| 支援商品 | 台股 |
| 說明 | 財務比率表上的"現金再投資%"欄位。 |

---
## 現金及約當現金
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 現金及約當現金 (選股欄位) |
| 語法 | ``` Value1 = GetField("現金及約當現金"); Value1 = GetField("CashCashEquivalent"); ``` |
| 欄位分類 | 財務 |
| 單位 | 百萬 |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 |
| 說明 | 「資產負債表」中「流動資產」項內的「現金及約當現金」項目。 |

---
## 現金派息比率
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 現金派息比率 (選股欄位) |
| 語法 | ``` Value1 = GetField("現金派息比率"); Value1 = GetField("CurrentPayoutRatio"); ``` |
| 欄位分類 | 財務 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 年 |
| 支援商品 | 台股 |
| 說明 | 計算公式: 現金股利 / 每股盈餘\*100%。 |

---
## 現金流量允當_
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 現金流量允當％ (選股欄位) |
| 語法 | ``` Value1 = GetField("現金流量允當％"); Value1 = GetField("CFAdequacyRatio"); ``` |
| 欄位分類 | 財務 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 年 |
| 支援商品 | 台股 |
| 說明 | 計算公式 : 最近5年度來自營運之現金流量/ 最近5年度(長期投資(新增)+固定資產(購置)+存貨(增)減+支付現金股利) \*100%。 只提供年度頻率資料。 單位為%，例如：欄位數值105%，則回傳105。 |

---
## 現金流量比率
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 現金流量比率 (選股欄位) |
| 語法 | ``` Value1 = GetField("現金流量比率"); Value1 = GetField("CFO ／CL ％"); ``` |
| 欄位分類 | 財務 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 |
| 說明 | 財務比率表上的「現金流量比率」欄位。 提供單季與年度資料。 例如：現金流量比率為30%，單位為%，則回傳30。 不提供此欄位的公司，輸出值會以零表示。 |

---
## 理財活動之現金流量
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 理財活動之現金流量 (選股欄位) |
| 語法 | ``` Value1 = GetField("理財活動之現金流量"); Value1 = GetField("CashFlowFinancing"); Value1 = GetField("CFF"); ``` |
| 欄位分類 | 財務 |
| 單位 | 台股：百萬。美股：百萬(美元) |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 美(股票) |
| 說明 | 現金流量表上的「籌資活動之現金流量」欄位。 正值表示現金流入，負值表示現金流出。 提供單季與年度資料。 |

---
## 用人費用率
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 用人費用率 (選股欄位) |
| 語法 | ``` Value1 = GetField("用人費用率"); Value1 = GetField("Employee Fee ％"); ``` |
| 欄位分類 | 財務 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 |
| 說明 | 財務比率表上的「用人費用率」欄位。 提供單季與年度資料。 單位為%，例如：用人費用率為15%，則回傳15。 |

---
## 當期財報截止日
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 當期財報截止日 (選股欄位) |
| 語法 | ``` Value1 = GetField("當期財報截止日"); Value1 = GetField("PeriodEndDate"); ``` |
| 欄位分類 | 財務 |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 美(股票) |
| 說明 | 回傳每個公司的財報截止日，格式為YYYYMMDD. 美股股票才支援此欄位。 |

---
## 盈餘成長係數
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 盈餘成長係數 (選股欄位) |
| 語法 | ``` Value1 = GetField("盈餘成長係數"); Value1 = GetField("PEG"); ``` |
| 欄位分類 | 財務 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 年 |
| 支援商品 | 台股 |
| 說明 | 計算公式: 本益比 / 盈餘成長率 \*100%，其中盈餘成長率 = (1-股利合計/EPS) \* ROE。 |

---
## 盈餘殖利率
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 盈餘殖利率 (選股欄位) |
| 語法 | ``` Value1 = GetField("盈餘殖利率"); Value1 = GetField("EarningYield"); ``` |
| 欄位分類 | 財務 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 |
| 說明 | 計算公式: 稅前息前淨利 / (總市值+負債總額)\*100%。 |

---
## 盈餘營收成長率比
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 盈餘營收成長率比 (選股欄位) |
| 語法 | ``` Value1 = GetField("盈餘營收成長率比"); Value1 = GetField("EarningGrowth／SalesGrowth"); ``` |
| 欄位分類 | 財務 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 |
| 說明 | 計算公式: 本期稅後淨利年成長率 / 營業收入淨額年成長率 \*100% 提供單季與年度資料。 單位為%，例如：欄位數值為25%，則回傳25。 |

---
## 短期借支
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 短期借支 (選股欄位) |
| 語法 | ``` Value1 = GetField("短期借支"); Value1 = GetField("LoanToOther－ShortTerm"); ``` |
| 欄位分類 | 財務 |
| 單位 | 百萬 |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 |
| 說明 | 「資產負債表」中「流動資產」項內的「資金貸予他人－流動」項目。 金融、壽險、產險、證券股不適用此欄位。 |

---
## 短期借款
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 短期借款 (選股欄位) |
| 語法 | ``` Value1 = GetField("短期借款"); Value1 = GetField("Short－TermBorrowing"); ``` |
| 欄位分類 | 財務 |
| 單位 | 百萬 |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 |
| 說明 | 「資產負債表」中「流動負債」項內「短期借款」項目。 金融、壽險、產險股不適用此欄位。 |

---
## 短期投資
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 短期投資 (選股欄位) |
| 語法 | ``` Value1 = GetField("短期投資"); Value1 = GetField("MarketableSecurity"); ``` |
| 欄位分類 | 財務 |
| 單位 | 百萬 |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 |
| 說明 | 「資產負債表」中「流動資產」項內的「"透過損益按公允價值衡量之金融資產－流動" + "透過其他綜合損益按公允價值衡量之金融資產－流動" + "按攤銷後成本衡量之金融資產－流動"」三個項目加總。 |

---
## 研發費用
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 研發費用 (選股欄位) |
| 語法 | ``` Value1 = GetField("研發費用"); Value1 = GetField("OperatingExpense－R＆D"); ``` |
| 欄位分類 | 財務 |
| 單位 | 百萬 |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 |
| 說明 | 「損益表」中「研究發展費」項目。 金融、壽險、產險、證券股不適用此欄位。 |

---
## 研發費用率
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 研發費用率 (選股欄位) |
| 語法 | ``` Value1 = GetField("研發費用率"); Value1 = GetField("R＆D ％"); ``` |
| 欄位分類 | 財務 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 |
| 說明 | 財務比率表上的「研究發展費用率」欄位。 提供單季與年度資料。 單位為%，例如：研發費用率為15%，則回傳15。 不提供此欄位的公司，輸出值會以零表示。 |

---
## 稀釋後每股淨利
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 稀釋後每股淨利 (選股欄位) |
| 語法 | ``` Value1 = GetField("稀釋後每股淨利"); Value1 = GetField("DilutedEPS"); ``` |
| 欄位分類 | 財務 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 美(股票) |
| 說明 | 損益表的"稀釋後每股盈餘"項目。  稀釋每股盈餘是假設所有潛在股份都被轉換成普通股，以此計算出的每股收益數字。它是用來衡量公司實際每股盈利可能降低的情況，因為潛在股份的發行會稀釋公司的盈利。 |

---
## 稅前息前折舊前淨利
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 稅前息前折舊前淨利 (選股欄位) |
| 語法 | ``` Value1 = GetField("稅前息前折舊前淨利"); Value1 = GetField("EBITDA"); ``` |
| 欄位分類 | 財務 |
| 單位 | 台股：百萬。美股：百萬(美元) |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 美(股票) |
| 說明 | 「損益表」中「稅前息前折舊前淨利」項目。 |

---
## 稅前息前淨利
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 稅前息前淨利 (選股欄位) |
| 語法 | ``` Value1 = GetField("稅前息前淨利"); Value1 = GetField("EBIT"); ``` |
| 欄位分類 | 財務 |
| 單位 | 百萬 |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 |
| 說明 | 「損益表」中「稅前息前淨利」項目。 |

---
## 稅前淨利
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 稅前淨利 (選股欄位) |
| 語法 | ``` Value1 = GetField("稅前淨利"); Value1 = GetField("Pre－TaxIncome"); ``` |
| 欄位分類 | 財務 |
| 單位 | 台股：百萬。美股：百萬(美元) |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 美(股票) |
| 說明 | 「損益表」中「稅前淨利」項目。 凡營業利益（或損失）加上營業外利益（或損失）後之餘額屬之。其為正數，表示本期稅前淨利之數；其為負數，表示本期稅前淨損之數。 |

---
## 稅前淨利成長率
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 稅前淨利成長率 (選股欄位) |
| 語法 | ``` Value1 = GetField("稅前淨利成長率"); Value1 = GetField("YOY％－Pre－TaxIncome"); ``` |
| 欄位分類 | 財務 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 |
| 說明 | 財務比率表上的「稅前淨利成長率」欄位。表示當期稅前淨利較去年同期稅前淨利的成長幅度。 提供單季與年度資料。 例如：稅前淨利成長率為25%，則回傳25。 例如：今年第一季稅前淨利100億、去年第一季稅前淨利80億，稅前淨利成長率為(100-80)/80\*100% = 25%，單位為%，則回傳25。 |

---
## 稅前淨利率
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 稅前淨利率 (選股欄位) |
| 語法 | ``` Value1 = GetField("稅前淨利率"); Value1 = GetField("Pre－TaxProfitMargin"); Value1 = GetField("Pre－TaxIncomeMargin"); ``` |
| 欄位分類 | 財務 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 美(股票) |
| 說明 | 財務比率表上的「稅前淨利率」欄位。 提供單季與年度資料。 單位為%，例如：稅前淨利率為30%，則回傳30。 |

---
## 稅後淨利成長率
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 稅後淨利成長率 (選股欄位) |
| 語法 | ``` Value1 = GetField("稅後淨利成長率"); Value1 = GetField("YOY％－NetIncome"); ``` |
| 欄位分類 | 財務 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 |
| 說明 | 財務比率表上的「稅後淨利成長率」欄位。表示當期稅後淨利較去年同期稅後淨利的成長幅度。 提供單季與年度資料。 例如：今年第一季稅後淨利100億、去年第一季稅後淨利80億，稅後淨利成長率為(100-80)/80\*100% = 25%，單位為%，則回傳25。 |

---
## 稅後淨利率
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 稅後淨利率 (選股欄位) |
| 語法 | ``` Value1 = GetField("稅後淨利率"); Value1 = GetField("ProfitMargin"); ``` |
| 欄位分類 | 財務 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 美(股票) |
| 說明 | 財務比率表上的「稅後淨利率」欄位。 提供單季與年度資料。 單位為%，例如：稅後淨利率為30%，則回傳30。 |

---
## 管理_銷售費用_營收
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 管理+銷售費用／營收 (選股欄位) |
| 語法 | ``` Value1 = GetField("管理+銷售費用／營收"); Value1 = GetField("OperatingExpense－ADM＆Promotion／Sales"); ``` |
| 欄位分類 | 財務 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 |
| 說明 | 計算公式 : (營業費用的推銷費用 + 管理費用) / 營業收入淨額\*100%。 |

---
## 管理費用
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 管理費用 (選股欄位) |
| 語法 | ``` Value1 = GetField("管理費用"); Value1 = GetField("OperatingExpense－ADM"); ``` |
| 欄位分類 | 財務 |
| 單位 | 百萬 |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 |
| 說明 | 「損益表」中「管理費用」項目。凡各項管理費用屬之。 金融、證券股不適用此欄位。 |

---
## 管理費用_季營收
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 管理費用／季營收 (選股欄位) |
| 語法 | ``` Value1 = GetField("管理費用／季營收"); Value1 = GetField("OperatingExpense－ADM／Sales"); ``` |
| 欄位分類 | 財務 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 |
| 支援商品 | 台股 |
| 說明 | 計算公式: 營業費用的管理費用/營業收入淨額\*100%。 提供單季與年度資料。 單位為%，例如：欄位數值5%，則回傳5。不提供此欄位的公司，輸出值會以零表示。 |

---
## 經常利益
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 經常利益 (選股欄位) |
| 語法 | ``` Value1 = GetField("經常利益"); Value1 = GetField("OrdinaryIncome"); Value1 = GetField("繼續營業單位損益"); ``` |
| 欄位分類 | 財務 |
| 單位 | 百萬 |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 |
| 說明 | 「損益表」中「繼續營業單位損益」項目。 凡本期繼續營業單位稅前淨利（或淨損）扣除所得稅費用（或利益）後之餘額屬之。其為正數，表示繼續營業單位本期稅後淨利之數；其為負數，表示繼續營業單位本期稅後淨損之數。 |

---
## 總流通在外股數
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 總流通在外股數 (選股欄位) |
| 語法 | ``` Value1 = GetField("總流通在外股數"); Value1 = GetField("TotalSharesOutstanding"); ``` |
| 欄位分類 | 財務 |
| 單位 | 股 |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 美(股票) |
| 說明 | 「資產負債表」中「流通在外股數」項目。 美股股票才適用本欄位。 |

---
## 總資產成長率
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 總資產成長率 (選股欄位) |
| 語法 | ``` Value1 = GetField("總資產成長率"); Value1 = GetField("YOY％－TotalAssets"); ``` |
| 欄位分類 | 財務 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 |
| 說明 | 財務比率表上的「總資產成長率」欄位。表示當期資產總額較去年同期資產總額的成長幅度。 提供季度與年度資料。 例如：今年第一季資產總額100億、去年第一季資產總額80億，總資產成長率為(100-80)/80\*100% = 25%，單位為%，則回傳25。 |

---
## 總資產週轉率_次_
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 總資產週轉率(次) (選股欄位) |
| 語法 | ``` Value1 = GetField("總資產週轉率(次)"); Value1 = GetField("TotalAssetTurnover"); ``` |
| 欄位分類 | 財務 |
| 單位 | 次 |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 |
| 說明 | 財務比率表上的「總資產週轉次數」欄位。 提供單季與年度資料。 不提供此欄位的公司，輸出值會以零表示。 |

---
## 聯屬公司間未實現銷貨
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 聯屬公司間未實現銷貨 (選股欄位) |
| 語法 | ``` Value1 = GetField("聯屬公司間未實現銷貨"); Value1 = GetField("UnearnedRelatedSale"); ``` |
| 欄位分類 | 財務 |
| 單位 | 百萬 |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 |
| 說明 | 「損益表」中「聯屬公司已（未）實現銷貨利益」項目。 不提供此欄位的公司，輸出值會以零表示。 |

---
## 股價自由現金流量比
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 股價自由現金流量比 (選股欄位) |
| 語法 | ``` Value1 = GetField("股價自由現金流量比"); Value1 = GetField("Price／FCF"); ``` |
| 欄位分類 | 財務 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 季 年 |
| 支援商品 | 台股 |
| 說明 | 計算公式: 股價 / 每股自由現金流量。 |

---
## 股利收入
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 股利收入 (選股欄位) |
| 語法 | ``` Value1 = GetField("股利收入"); Value1 = GetField("InvestmentIncome"); Value1 = GetField("投資收入／股利收入"); ``` |
| 欄位分類 | 財務 |
| 單位 | 百萬 |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 |
| 說明 | 「損益表」中「股利收入」項目。凡非以投資為營業項目者，其採用權益法之投資以外之金融資產獲配之現金股利屬之。 壽險、產險、證券股不適用此欄位。 |

---
## 股東權益報酬率
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 股東權益報酬率 (選股欄位) |
| 語法 | ``` Value1 = GetField("股東權益報酬率"); Value1 = GetField("ROE"); Value1 = GetField("ReturnOnEquity"); ``` |
| 欄位分類 | 財務 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 美(股票) |
| 說明 | 台股，是財務比率表的「ROE(A)─稅後」項目。 美股，是財務比率表上的「股東權益報酬率」項目。 提供單季與年度資料。 單位為%，例如：股東權益報酬率為30%，則回傳30。 |

---
## 股東權益總額
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 股東權益總額 (選股欄位) |
| 語法 | ``` Value1 = GetField("股東權益總額"); Value1 = GetField("TotalEquity"); ``` |
| 欄位分類 | 財務 |
| 單位 | 台股：百萬。美股：百萬(美元) |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 美(股票) |
| 說明 | 「資產負債表」財務報表項目。凡全部資產減除全部負債後之餘額者屬之，包括資本、資本公積、保留盈餘（累積虧損）、權益調整及非控制權益等。 |

---
## 背書保證佔淨值比
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 背書保證佔淨值比 (選股欄位) |
| 語法 | ``` Value1 = GetField("背書保證佔淨值比"); Value1 = GetField("％OfEndorsementsGuarantees"); ``` |
| 欄位分類 | 財務 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 月 |
| 支援商品 | 台股 |
| 說明 | 計算公式：背書保證餘額／股東權證總額\*100%。 |

---
## 背書保證餘額
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 背書保證餘額 (選股欄位) |
| 語法 | ``` Value1 = GetField("背書保證餘額"); Value1 = GetField("BalanceOfEndorsementsGuarantees"); ``` |
| 欄位分類 | 財務 |
| 單位 | 仟元 |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 月 |
| 支援商品 | 台股 |
| 說明 | 背書保證累計餘額。 資料來源為公開資訊觀測站。 |

---
## 自由現金流量
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 自由現金流量 (選股欄位) |
| 語法 | ``` Value1 = GetField("自由現金流量"); Value1 = GetField("FCFF"); ``` |
| 欄位分類 | 財務 |
| 單位 | 百萬 |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 |
| 說明 | 現金流量表中的「"來自營運之現金流量" + "購置不動產廠房設備（含預付）－CFI" + "支付利息－CFF"」。 正值表示現金流入，負值表示現金流出。 |

---
## 自由現金流量營收比
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 自由現金流量營收比 (選股欄位) |
| 語法 | ``` Value1 = GetField("自由現金流量營收比"); Value1 = GetField("FCF／Sales"); ``` |
| 欄位分類 | 財務 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 美(股票) |
| 說明 | 計算公式: 自由現金流量／營業收入淨額\*100%。 其中, 自由現金流量是依現金流量表中的「"來自營運之現金流量" + "購置不動產廠房設備（含預付）－CFI" + "支付利息－CFF"」計算之。正值表示現金流入，負值表示現金流出。 提供單季與年度資料。 單位為%，例如欄位數值為25%，則回傳25。 |

---
## 處分投資利得
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 處分投資利得 (選股欄位) |
| 語法 | ``` Value1 = GetField("處分投資利得"); Value1 = GetField("Gain－DisposalInvestment"); ``` |
| 欄位分類 | 財務 |
| 單位 | 百萬 |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 |
| 說明 | 「損益表」中「處分投資利益」項目。凡非以投資為營業項目者，處分金融資產（透過損益按公允價值衡量之金融資產除外）所獲之利益等屬之。 金融、壽險、產險、證券股不適用此欄位。 |

---
## 處分投資損失
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 處分投資損失 (選股欄位) |
| 語法 | ``` Value1 = GetField("處分投資損失"); Value1 = GetField("Loss－DisposalInvestment"); ``` |
| 欄位分類 | 財務 |
| 單位 | 百萬 |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 |
| 說明 | 「損益表」中「處分投資損失」項目。凡非以投資為營業項目者，處分金融資產（透過損益按公允價值衡量之金融資產除外）產生之損失等屬之。 金融、壽險、產險、證券股不適用此欄位。 |

---
## 處分資產利得
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 處分資產利得 (選股欄位) |
| 語法 | ``` Value1 = GetField("處分資產利得"); Value1 = GetField("GainsDisposalFixedAssets"); ``` |
| 欄位分類 | 財務 |
| 單位 | 百萬 |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 |
| 說明 | 「損益表」中「處分不動產、廠房及設備利益」項目。凡不動產、廠房及設備變賣、交換等所獲得之利益屬之。 |

---
## 處分資產損失
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 處分資產損失 (選股欄位) |
| 語法 | ``` Value1 = GetField("處分資產損失"); Value1 = GetField("Loss－DisposalFixedAssets"); ``` |
| 欄位分類 | 財務 |
| 單位 | 百萬 |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 |
| 說明 | 「損益表」中「處分不動產、廠房及設備損失」項目。凡不動產、廠房及設備變賣、交換等所產生之損失屬之。 |

---
## 誠信指標
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 誠信指標 (選股欄位) |
| 語法 | ``` Value1 = GetField("誠信指標"); Value1 = GetField("CreditIndicator"); ``` |
| 欄位分類 | 財務 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 |
| 說明 | = S0104327 |

---
## 負債及股東權益總額
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 負債及股東權益總額 (選股欄位) |
| 語法 | ``` Value1 = GetField("負債及股東權益總額"); Value1 = GetField("TotalLiabilities＆Equity"); ``` |
| 欄位分類 | 財務 |
| 單位 | 百萬 |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 |
| 說明 | 「資產負債表」財務報表項目。 |

---
## 負債對淨值比率
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 負債對淨值比率 (選股欄位) |
| 語法 | ``` Value1 = GetField("負債對淨值比率"); Value1 = GetField("DebtEquityRatio"); Value1 = GetField("總負債/總淨值"); ``` |
| 欄位分類 | 財務 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 |
| 說明 | 財務比率表上的「總負債/總淨值」欄位。 提供季度與年度資料。 單位為％，例如：負債對淨值比率為60%，則回傳60。 |

---
## 負債比率
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 負債比率 (選股欄位) |
| 語法 | ``` Value1 = GetField("負債比率"); Value1 = GetField("LiabilitiesRatio"); ``` |
| 欄位分類 | 財務 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 美(股票) |
| 說明 | 財務比率表上的「負債比率」欄位。 提供季度與年度資料。 單位為%，例如：負債比率為30%，則回傳30。 |

---
## 負債總額
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 負債總額 (選股欄位) |
| 語法 | ``` Value1 = GetField("負債總額"); Value1 = GetField("TotalLiabilities"); ``` |
| 欄位分類 | 財務 |
| 單位 | 台股：百萬。美股：百萬(美元) |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 美(股票) |
| 說明 | 「資產負債表」財務報表項目。凡過去交易或其他事項所發生之經濟義務，能以貨幣衡量，並將以提供勞務或支付經濟資源之方式償付者，包括流動負債、存款、匯款及金融債券、央行及同業融資、長期負債、其他負債等。 |

---
## 資本公積
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 資本公積 (選股欄位) |
| 語法 | ``` Value1 = GetField("資本公積"); Value1 = GetField("CapitalReserve"); ``` |
| 欄位分類 | 財務 |
| 單位 | 百萬 |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 |
| 說明 | 「資產負債表」股東權益項目中的「資本公積合計」。 |

---
## 資本支出營收比
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 資本支出營收比 (選股欄位) |
| 語法 | ``` Value1 = GetField("資本支出營收比"); Value1 = GetField("Capitalexpenditure／Sales"); ``` |
| 欄位分類 | 財務 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 |
| 說明 | 計算公式 : 現金流量表中 購置不動產廠房設備（含預付）/營業收入淨額\*100%。 |

---
## 資本支出金額
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 資本支出金額 (選股欄位) |
| 語法 | ``` Value1 = GetField("資本支出金額"); Value1 = GetField("CapitalExpenditure"); ``` |
| 欄位分類 | 財務 |
| 單位 | 百萬 |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 |
| 說明 | 現金流量表內的"購置不動產廠房設備（含預付）－CFI"項目。現金流量表以負值表示支出。 |

---
## 資產報酬率
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 資產報酬率 (選股欄位) |
| 語法 | ``` Value1 = GetField("資產報酬率"); Value1 = GetField("ROA"); Value1 = GetField("ReturnOnAssets"); ``` |
| 欄位分類 | 財務 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 美(股票) |
| 說明 | 台股，是財務比率表的「ROA(A)稅後息前」項目。 美股，是財務比率表上的「資產報酬率」項目。 提供單季與年度資料。 單位為%，例如：資產報酬率為30%，則回傳30。 |

---
## 資產總額
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 資產總額 (選股欄位) |
| 語法 | ``` Value1 = GetField("資產總額"); Value1 = GetField("TotalAssets"); ``` |
| 欄位分類 | 財務 |
| 單位 | 台股：百萬。美股：百萬(美元) |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 美(股票) |
| 說明 | 「資產負債表」財務報表項目。凡透過各種交易或其他事項所獲得或控制之經濟資源，能以貨幣衡量並預期未來能提供經濟效益者。 |

---
## 資金貸放佔淨值比
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 資金貸放佔淨值比 (選股欄位) |
| 語法 | ``` Value1 = GetField("資金貸放佔淨值比"); Value1 = GetField("％OfLoanBalance"); ``` |
| 欄位分類 | 財務 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 月 |
| 支援商品 | 台股 |
| 說明 | 計算公式：資金貸放餘額／股東權證總額\*100%。 |

---
## 資金貸放餘額
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 資金貸放餘額 (選股欄位) |
| 語法 | ``` Value1 = GetField("資金貸放餘額"); Value1 = GetField("LoanBalance"); ``` |
| 欄位分類 | 財務 |
| 單位 | 仟元 |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 月 |
| 支援商品 | 台股 |
| 說明 | 資金貸放餘額。 資料來源為公開資訊觀測站。 |

---
## 退休金準備
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 退休金準備 (選股欄位) |
| 語法 | ``` Value1 = GetField("退休金準備"); Value1 = GetField("AccruedPensionPay"); ``` |
| 欄位分類 | 財務 |
| 單位 | 百萬 |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 |
| 說明 | 「資產負債表」中「應計退休金負債」項目。 |

---
## 速動比率
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 速動比率 (選股欄位) |
| 語法 | ``` Value1 = GetField("速動比率"); Value1 = GetField("AcidTest"); ``` |
| 欄位分類 | 財務 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 美(股票) |
| 說明 | 財務比率表上的「速動比率」欄位。 提供季度與年度資料。 單位為%，例如：速動比率為150%，則回傳150。 不提供此欄位的公司，輸出值會以零表示。 |

---
## 遞延所得稅
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 遞延所得稅 (選股欄位) |
| 語法 | ``` Value1 = GetField("遞延所得稅"); Value1 = GetField("DeferredTax"); ``` |
| 欄位分類 | 財務 |
| 單位 | 百萬 |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 |
| 說明 | 「資產負債表」中「遞延所得稅負債」項目。 |

---
## 遞延貸項
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 遞延貸項 (選股欄位) |
| 語法 | ``` Value1 = GetField("遞延貸項"); Value1 = GetField("DeferredCreditBalances"); ``` |
| 欄位分類 | 財務 |
| 單位 | 百萬 |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 |
| 說明 | 「資產負債表」中「遞延貸項」項目。 |

---
## 遞延資產
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 遞延資產 (選股欄位) |
| 語法 | ``` Value1 = GetField("遞延資產"); Value1 = GetField("DeferredAssets"); ``` |
| 欄位分類 | 財務 |
| 單位 | 百萬 |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 |
| 說明 | 「資產負債表」中「非流動資產」項內的「遞延資產合計」項目。 金融、壽險、產險、證券股不適用此欄位。 |

---
## 銷售費用比
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 銷售費用比 (選股欄位) |
| 語法 | ``` Value1 = GetField("銷售費用比"); Value1 = GetField("SellingExpanseRatio／CostOfGoodsSold"); ``` |
| 欄位分類 | 財務 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 |
| 說明 | 計算公式: 銷售費用/營業成本 提供單季與年度資料。 單位為%，例如：欄位數值5%，則回傳5。不提供此欄位的公司，輸出值會以零表示。 |

---
## 長期投資
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 長期投資 (選股欄位) |
| 語法 | ``` Value1 = GetField("長期投資"); Value1 = GetField("Long－TermInvestment"); ``` |
| 欄位分類 | 財務 |
| 單位 | 台股:百萬；美股:百萬(美元) |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 美(股票) |
| 說明 | 資產負債表的4項資料加總  1.透過損益按公允價值衡量之金融資產－非流動  2.透過其他綜合損益按公允價值衡量之金融資產－非流動  3.按攤銷後成本衡量之金融資產－非流動  4.採權益法之長期股權投資 |

---
## 長期負債
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 長期負債 (選股欄位) |
| 語法 | ``` Value1 = GetField("長期負債"); Value1 = GetField("Long－TermLiabilities"); Value1 = GetField("非流動負債"); ``` |
| 欄位分類 | 財務 |
| 單位 | 台股：百萬。美股：百萬(美元) |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 美(股票) |
| 說明 | 台股:「資產負債表」中「非流動負債」項目。 美股:「資產負債表」中「非流動負債」項內「長期借款」項目。 不提供此欄位的公司，輸出值會以零表示。 |

---
## 長期資金適合率
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 長期資金適合率 (選股欄位) |
| 語法 | ``` Value1 = GetField("長期資金適合率"); Value1 = GetField("(L－TLiab.+SE)／FA％"); ``` |
| 欄位分類 | 財務 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 |
| 說明 | 財務比率表上的「長期資金適合率」欄位。 提供單季與年度資料。 單位為%，例如：長期資金適合率為160%，則回傳160。 不提供此欄位的公司，輸出值會以零表示。 |

---
## 長短期負債比率
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 長短期負債比率 (選股欄位) |
| 語法 | ``` Value1 = GetField("長短期負債比率"); Value1 = GetField("LongTermDebt/ShortTermDebt"); ``` |
| 欄位分類 | 財務 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 |
| 說明 | 計算公式: 流動負債／負債總額 \*100%。 |

---
## 預付費用及預付款
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 預付費用及預付款 (選股欄位) |
| 語法 | ``` Value1 = GetField("預付費用及預付款"); Value1 = GetField("Prepaid＆Advance"); ``` |
| 欄位分類 | 財務 |
| 單位 | 百萬 |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 |
| 說明 | 「資產負債表」中「流動資產」項內的「預付費用及預付款」項目。 金融股不適用此欄位。 |

---
## 預收款項
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 預收款項 (選股欄位) |
| 語法 | ``` Value1 = GetField("預收款項"); Value1 = GetField("AdvancesCustomers"); ``` |
| 欄位分類 | 財務 |
| 單位 | 百萬 |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 |
| 說明 | 「資產負債表」中「預收款項」項目。 金融股不適用此欄位。 |

---
## 預收股款
**Path:** 選股欄位 > 財務

|  |  |
| --- | --- |
| 欄位名稱 | 預收股款 (選股欄位) |
| 語法 | ``` Value1 = GetField("預收股款"); Value1 = GetField("Proceeds－NewIssued"); ``` |
| 欄位分類 | 財務 |
| 單位 | 百萬 |
| 格式 | 數值 |
| 支援腳本 | 指標 選股 警示 交易 函數 |
| 可用頻率 | 季 年 |
| 支援商品 | 台股 |
| 說明 | 「資產負債表」股東權益項目中的「預收股款」。 |

---
## 上漲量
**Path:** 選股欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 上漲量 (選股欄位) |
| 語法 | ``` Value1 = GetField("上漲量"); Value1 = GetField("UpVolume"); Value1 = GetField("上漲成交量"); ``` |
| 欄位分類 | 量能 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 |
| 支援商品 | 台股 類股指數 |
| 說明 | 這一筆K棒的所有成交內比前一價上漲的成交量的加總。  例如，這一筆K棒內有4筆成交，分別是30元2張，30元3張，31元4張，30元5張，而前一筆K棒的收盤價是29。那麼上漲量=6張，下跌量=5張。  計算方法如下：   * 第一筆成交30元比前一價29元上漲(上漲量=2張) * 第二筆成交30元跟前一價30元相同，不予計算 * 第三筆31元比前一價30元上漲(上漲量=2張+4張=6張) * 第四筆30元比前一價31元下跌(下跌量=5張) |

---
## 下跌量
**Path:** 選股欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 下跌量 (選股欄位) |
| 語法 | ``` Value1 = GetField("下跌量"); Value1 = GetField("DownVolume"); Value1 = GetField("下跌成交量"); ``` |
| 欄位分類 | 量能 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 |
| 支援商品 | 台股 類股指數 |
| 說明 | 這一筆K棒的所有成交內比前一價下跌的成交量的加總。  例如，這一筆K棒內有4筆成交，分別是30元2張，30元3張，31元4張，30元5張，而前一筆K棒的收盤價是29。那麼上漲量=6張，下跌量=5張。  計算方法如下：   * 第一筆成交30元比前一價29元上漲(上漲量=2張) * 第二筆成交30元跟前一價30元相同，不予計算 * 第三筆31元比前一價30元上漲(上漲量=2張+4張=6張) * 第四筆30元比前一價31元下跌(下跌量=5張) |

---
## 佔全市場成交量比
**Path:** 選股欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 佔全市場成交量比 (選股欄位) |
| 語法 | ``` Value1 = GetField("佔全市場成交量比"); Value1 = GetField("WeightOfMarket"); ``` |
| 欄位分類 | 量能 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 |
| 支援商品 | 台股 |
| 說明 | 計算公式：個股成交值 / (加權指數成交值+櫃買指數成交值) \* 100%。  單位為%，例如比值為1.5%時，回傳值為1.5。 |

---
## 佔大盤成交量比
**Path:** 選股欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 佔大盤成交量比 (選股欄位) |
| 語法 | ``` Value1 = GetField("佔大盤成交量比"); Value1 = GetField("WeightOfTSE"); ``` |
| 欄位分類 | 量能 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 |
| 支援商品 | 台股 |
| 說明 | 計算公式：個股成交值 / 對應大盤指數成交值 \* 100%。  單位為%，例如比值為1.5%時，回傳值為1.5。  上市股票的對應大盤指數為加權指數，上櫃股票的對應大盤指數為櫃買指數。 |

---
## 內外盤比
**Path:** 選股欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 內外盤比 (選股欄位) |
| 語法 | ``` Value1 = GetField("內外盤比"); Value1 = GetField("BidAskRatio"); ``` |
| 欄位分類 | 量能 |
| 單位 | ％ |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 |
| 支援商品 | 台股 |
| 說明 | 計算公式 : 內盤量 / 外盤量 \* 100%  數值越大表示「賣方」力道越強勁。 |

---
## 內盤均量
**Path:** 選股欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 內盤均量 (選股欄位) |
| 語法 | ``` Value1 = GetField("內盤均量"); Value1 = GetField("BidAvg"); ``` |
| 欄位分類 | 量能 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 |
| 支援商品 | 台股 |
| 說明 | 計算公式：內盤成交量／內盤成交次數。 |

---
## 內盤成交次數
**Path:** 選股欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 內盤成交次數 (選股欄位) |
| 語法 | ``` Value1 = GetField("內盤成交次數"); Value1 = GetField("TotalInTicks"); ``` |
| 欄位分類 | 量能 |
| 單位 | 次 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 |
| 支援商品 | 台股 香港股票 大陸股票 |
| 說明 | 這一筆K棒的所有成交內以內盤成交的次數。 |

---
## 內盤量
**Path:** 選股欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 內盤量 (選股欄位) |
| 語法 | ``` Value1 = GetField("內盤量"); Value1 = GetField("TradeVolumeAtBid"); ``` |
| 欄位分類 | 量能 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 |
| 支援商品 | 台股 類股指數 |
| 說明 | 這一筆K棒的所有成交內標記為內盤成交的加總數量。  系統會依照成交發生當時委託簿的最佳委買價格以及最佳委賣價格來判斷此筆是外盤成交或是內盤成交。如果成交的價格貼近委買價格的話，則標示為內盤成交，一般會視為主動「賣出」。  如果是指數類型商品的話，則是比前一價下跌的所有成交量的加總。 |

---
## 外盤均量
**Path:** 選股欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 外盤均量 (選股欄位) |
| 語法 | ``` Value1 = GetField("外盤均量"); Value1 = GetField("AskAvg"); ``` |
| 欄位分類 | 量能 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 |
| 支援商品 | 台股 |
| 說明 | 計算公式 : 外盤成交量／外盤成交次數。 |

---
## 外盤成交次數
**Path:** 選股欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 外盤成交次數 (選股欄位) |
| 語法 | ``` Value1 = GetField("外盤成交次數"); Value1 = GetField("TotalOutTicks"); ``` |
| 欄位分類 | 量能 |
| 單位 | 次 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 |
| 支援商品 | 台股 類股指數 香港股票 大陸股票 |
| 說明 | 這一筆K棒的所有成交內以外盤成交的次數。 |

---
## 外盤量
**Path:** 選股欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 外盤量 (選股欄位) |
| 語法 | ``` Value1 = GetField("外盤量"); Value1 = GetField("TradeVolumeAtAsk"); ``` |
| 欄位分類 | 量能 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 |
| 支援商品 | 台股 類股指數 |
| 說明 | 這一筆K棒的所有成交內被標記為外盤成交的加總數量。  系統會依照成交發生當時委託簿的最佳委買價格以及最佳委賣價格來判斷此筆是外盤成交或是內盤成交。如果成交的價格貼近委賣價格的話，則標示為外盤成交，一般會視為主動「買進」。  如果是指數類型商品的話，則是比前一價上漲的所有成交量的加總。 |

---
## 成交均量
**Path:** 選股欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 成交均量 (選股欄位) |
| 語法 | ``` Value1 = GetField("成交均量"); Value1 = GetField("AvgDealedShare"); ``` |
| 欄位分類 | 量能 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 |
| 支援商品 | 台股 |
| 說明 | 整體市場(扣除權證)的每筆成交平均張數。 計算公式：累計成交／累成交筆。 |

---
## 成交量
**Path:** 選股欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 成交量 (選股欄位) |
| 語法 | ``` Value1 = GetField("成交量"); Value1 = GetField("Volume"); ``` |
| 欄位分類 | 量能 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 季 半年 年 |
| 支援商品 | 台股 香港股票 大陸股票 美(股票) |
| 說明 | K棒資料的成交量。  數值單位依照商品類型而異，例如台股的話成交量的單位是張，可是美股的話成交量的單位則是股。  語法上也可以使用Volume來取得同樣的數值。 |

---
## 成交金額_億_
**Path:** 選股欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 成交金額(億) (選股欄位) |
| 語法 | ``` Value1 = GetField("成交金額(億)"); Value1 = GetField("TradeValue"); Value1 = GetField("成交金額"); ``` |
| 欄位分類 | 量能 |
| 單位 | 億元 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 季 年 |
| 支援商品 | 台股 類股指數 |
| 說明 | 成交的加總金額，換算成億。  週頻率以上為區間累計值。 |

---
## 成交金額_元_
**Path:** 選股欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 成交金額(元) (選股欄位) |
| 語法 | ``` Value1 = GetField("成交金額(元)"); Value1 = GetField("TradeValue"); ``` |
| 欄位分類 | 量能 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 季 年 |
| 支援商品 | 台股 類股指數 |
| 說明 | 成交的加總金額。  如果是個股的話，計算方式是統計每筆成交數量乘上成交價。  如果是大盤或是指數型商品的話，此欄位與成交量是相同的。  數值依照統計區間而易，如果是分鐘頻率的話，則是該分鐘統計區間的每筆成交金額的加總，如果是週月以上的話則是每日成交金額的加總。 |

---
## 收盤量
**Path:** 選股欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 收盤量 (選股欄位) |
| 語法 | ``` Value1 = GetField("收盤量"); Value1 = GetField("VolumeAtClose"); ``` |
| 欄位分類 | 量能 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 |
| 支援商品 | 台股 |
| 說明 | 每天最後一筆成交的成交量。 |

---
## 新聞正向分數
**Path:** 選股欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 新聞正向分數 (選股欄位) |
| 語法 | ``` Value1 = GetField("新聞正向分數"); Value1 = GetField("NewsPosScore"); ``` |
| 欄位分類 | 量能 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 |
| 支援商品 | 台(股票) |
| 說明 | 依據個股新聞內容，判斷該則新聞為正向，負向，或是中立(觀望)。  新聞正向分數則是正向的新聞則數。  請注意：   1. 並非每檔商品都會有數值， 2. 每日會定時更新數值，非交易日的資料會彙整到前一個交易日， 3. 資料從2019/1/1日開始提供。 |

---
## 新聞聲量分數
**Path:** 選股欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 新聞聲量分數 (選股欄位) |
| 語法 | ``` Value1 = GetField("新聞聲量分數"); Value1 = GetField("NewsVolScore"); ``` |
| 欄位分類 | 量能 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 |
| 支援商品 | 台(股票) |
| 說明 | 依據個股新聞內容，判斷該則新聞為正向，負向，或是中立(觀望)。  新聞聲量分數則是正向的新聞則數 + 負向的新聞則數 + 觀望的新聞則數。  請注意：   1. 並非每檔商品都會有數值， 2. 每日會定時更新數值，非交易日的資料會彙整到前一個交易日， 3. 資料從2019/1/1日開始提供。 |

---
## 新聞負向分數
**Path:** 選股欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 新聞負向分數 (選股欄位) |
| 語法 | ``` Value1 = GetField("新聞負向分數"); Value1 = GetField("NewsNegScore"); ``` |
| 欄位分類 | 量能 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 |
| 支援商品 | 台(股票) |
| 說明 | 依據個股新聞內容，判斷該則新聞為正向，負向，或是中立(觀望)。  新聞負向分數則是負向的新聞則數。  請注意：   1. 並非每檔商品都會有數值， 2. 每日會定時更新數值，非交易日的資料會彙整到前一個交易日， 3. 資料從2019/1/1日開始提供。 |

---
## 盤中整股成交量
**Path:** 選股欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 盤中整股成交量 (選股欄位) |
| 語法 | ``` Value1 = GetField("盤中整股成交量"); Value1 = GetField("BoardLotVolume"); ``` |
| 欄位分類 | 量能 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 |
| 支援商品 | 台股 |
| 說明 | 盤中整股成交量 = 成交量 - 盤中零股成交量 - [盤後量](https://xshelp.xq.com.tw/XSHelp/?HelpName=%E7%9B%A4%E5%BE%8C%E9%87%8F&group=TVOLUME)(盤後整股成交量) - [零股量](https://xshelp.xq.com.tw/XSHelp/?HelpName=%E9%9B%B6%E8%82%A1%E9%87%8F&group=FVOLUME)(盤後零股成交量) - [鉅額交易量](https://xshelp.xq.com.tw/XSHelp/?HelpName=%E9%89%85%E9%A1%8D%E4%BA%A4%E6%98%93%E9%87%8F&group=FVOLUME) |

---
## 盤中零股成交量
**Path:** 選股欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 盤中零股成交量 (選股欄位) |
| 語法 | ``` Value1 = GetField("盤中零股成交量"); Value1 = GetField("OddLotVolume"); ``` |
| 欄位分類 | 量能 |
| 單位 | 股 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 |
| 支援商品 | 台股 |
| 說明 | 盤中零股成交量 = 成交量 - 盤中整股成交量 - [盤後量](https://xshelp.xq.com.tw/XSHelp/?HelpName=%E7%9B%A4%E5%BE%8C%E9%87%8F&group=TVOLUME)(盤後整股成交量) - [零股量](https://xshelp.xq.com.tw/XSHelp/?HelpName=%E9%9B%B6%E8%82%A1%E9%87%8F&group=FVOLUME)(盤後零股成交量) - [鉅額交易量](https://xshelp.xq.com.tw/XSHelp/?HelpName=%E9%89%85%E9%A1%8D%E4%BA%A4%E6%98%93%E9%87%8F&group=FVOLUME) |

---
## 盤後量
**Path:** 選股欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 盤後量 (選股欄位) |
| 語法 | ``` Value1 = GetField("盤後量"); Value1 = GetField("OffHourVolume"); ``` |
| 欄位分類 | 量能 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 |
| 支援商品 | 台股 |
| 說明 | 盤後定價交易區間的成交張數。 |

---
## 總成交次數
**Path:** 選股欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 總成交次數 (選股欄位) |
| 語法 | ``` Value1 = GetField("總成交次數"); Value1 = GetField("TotalTicks"); ``` |
| 欄位分類 | 量能 |
| 單位 | 次 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 |
| 支援商品 | 台股 類股指數 |
| 說明 | 成交的總筆數。  資料來源為交易所盤中所傳送的成交明細的筆數。  如果分鐘頻率的話則為該分鐘統計區間內的成交明細筆數。 |

---
## 總成交筆數
**Path:** 選股欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 總成交筆數 (選股欄位) |
| 語法 | ``` Value1 = GetField("總成交筆數"); Value1 = GetField("TotalTransaction"); ``` |
| 欄位分類 | 量能 |
| 單位 | 筆 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 季 年 |
| 支援商品 | 台股 |
| 說明 | 交易時段中的成交總筆數。  週頻率以上為區間累計值。  本統計資訊含一般、零股、盤後定價，不含鉅額、拍賣、標購。 |

---
## 買進中單成交次數
**Path:** 選股欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 買進中單成交次數 (選股欄位) |
| 語法 | ``` Value1 = GetField("買進中單成交次數"); Value1 = GetField("BidTickCount_M"); ``` |
| 欄位分類 | 量能 |
| 單位 | 次 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 |
| 支援商品 | 類股指數 台(股票) |
| 說明 | 買進中單的成交次數。  系統依照商品每筆成交的價格以及成交金額，判斷這筆成交是小單，中單，大單，或是特大單。如果是外盤成交，則為買進，內盤成交的話則為賣出。  詳細的定義請參考：https://www.xq.com.tw/台股逐筆功能行情端相關異動/。  欄位支援分鐘頻率(該分鐘區間的累計)，以及日頻率(當日開盤迄今的累計)。  如果是指數型商品的話，數值則是成分股的累計。  資料從2020年開始提供。 |

---
## 買進中單量
**Path:** 選股欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 買進中單量 (選股欄位) |
| 語法 | ``` Value1 = GetField("買進中單量"); Value1 = GetField("BidVolume_M"); ``` |
| 欄位分類 | 量能 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 |
| 支援商品 | 類股指數 台(股票) |
| 說明 | 買進中單的成交量。  系統依照商品每筆成交的價格以及成交金額，判斷這筆成交是小單，中單，大單，或是特大單。如果是外盤成交，則為買進，內盤成交的話則為賣出。  詳細的定義請參考：https://www.xq.com.tw/台股逐筆功能行情端相關異動/。  欄位支援分鐘頻率(該分鐘區間的累計)，以及日頻率(當日開盤迄今的累計)。  如果是指數型商品的話，數值則是成分股的累計。  資料從2020年開始提供。 |

---
## 買進中單金額
**Path:** 選股欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 買進中單金額 (選股欄位) |
| 語法 | ``` Value1 = GetField("買進中單金額"); Value1 = GetField("BidVolumePrice_M"); ``` |
| 欄位分類 | 量能 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 |
| 支援商品 | 類股指數 台(股票) |
| 說明 | 買進中單的金額。  系統依照商品每筆成交的價格以及成交金額，判斷這筆成交是小單，中單，大單，或是特大單。如果是外盤成交，則為買進，內盤成交的話則為賣出。  詳細的定義請參考：https://www.xq.com.tw/台股逐筆功能行情端相關異動/。  欄位支援分鐘頻率(該分鐘區間的累計)，以及日頻率(當日開盤迄今的累計)。  如果是指數型商品的話，數值則是成分股的累計。  資料從2020年開始提供。 |

---
## 買進大單成交次數
**Path:** 選股欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 買進大單成交次數 (選股欄位) |
| 語法 | ``` Value1 = GetField("買進大單成交次數"); Value1 = GetField("BidTickCount_L"); ``` |
| 欄位分類 | 量能 |
| 單位 | 次 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 |
| 支援商品 | 類股指數 台(股票) |
| 說明 | 買進大單的成交次數。  系統依照商品每筆成交的價格以及成交金額，判斷這筆成交是小單，中單，大單，或是特大單。如果是外盤成交，則為買進，內盤成交的話則為賣出。  詳細的定義請參考：https://www.xq.com.tw/台股逐筆功能行情端相關異動/。  欄位支援分鐘頻率(該分鐘區間的累計)，以及日頻率(當日開盤迄今的累計)。  如果是指數型商品的話，數值則是成分股的累計。  資料從2020年開始提供。 |

---
## 買進大單量
**Path:** 選股欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 買進大單量 (選股欄位) |
| 語法 | ``` Value1 = GetField("買進大單量"); Value1 = GetField("BidVolume_L"); ``` |
| 欄位分類 | 量能 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 |
| 支援商品 | 類股指數 台(股票) |
| 說明 | 買進大單的成交量。  系統依照商品每筆成交的價格以及成交金額，判斷這筆成交是小單，中單，大單，或是特大單。如果是外盤成交，則為買進，內盤成交的話則為賣出。  詳細的定義請參考：https://www.xq.com.tw/台股逐筆功能行情端相關異動/。  欄位支援分鐘頻率(該分鐘區間的累計)，以及日頻率(當日開盤迄今的累計)。  如果是指數型商品的話，數值則是成分股的累計。  資料從2020年開始提供。 |

---
## 買進大單金額
**Path:** 選股欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 買進大單金額 (選股欄位) |
| 語法 | ``` Value1 = GetField("買進大單金額"); Value1 = GetField("BidVolumePrice_L"); ``` |
| 欄位分類 | 量能 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 |
| 支援商品 | 類股指數 台(股票) |
| 說明 | 買進大單的成交金額。  系統依照商品每筆成交的價格以及成交金額，判斷這筆成交是小單，中單，大單，或是特大單。如果是外盤成交，則為買進，內盤成交的話則為賣出。  詳細的定義請參考：https://www.xq.com.tw/台股逐筆功能行情端相關異動/。  欄位支援分鐘頻率(該分鐘區間的累計)，以及日頻率(當日開盤迄今的累計)。  如果是指數型商品的話，數值則是成分股的累計。  資料從2020年開始提供。 |

---
## 買進小單成交次數
**Path:** 選股欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 買進小單成交次數 (選股欄位) |
| 語法 | ``` Value1 = GetField("買進小單成交次數"); Value1 = GetField("BidTickCount_S"); ``` |
| 欄位分類 | 量能 |
| 單位 | 次 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 |
| 支援商品 | 類股指數 台(股票) |
| 說明 | 買進小單的成交次數。  系統依照商品每筆成交的價格以及成交金額，判斷這筆成交是小單，中單，大單，或是特大單。如果是外盤成交，則為買進，內盤成交的話則為賣出。  詳細的定義請參考：https://www.xq.com.tw/台股逐筆功能行情端相關異動/。  欄位支援分鐘頻率(該分鐘區間的累計)，以及日頻率(當日開盤迄今的累計)。  如果是指數型商品的話，數值則是成分股的累計。  資料從2020年開始提供。 |

---
## 買進小單量
**Path:** 選股欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 買進小單量 (選股欄位) |
| 語法 | ``` Value1 = GetField("買進小單量"); Value1 = GetField("BidVolume_S"); ``` |
| 欄位分類 | 量能 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 |
| 支援商品 | 類股指數 台(股票) |
| 說明 | 買進小單的成交量。  系統依照商品每筆成交的價格以及成交金額，判斷這筆成交是小單，中單，大單，或是特大單。如果是外盤成交，則為買進，內盤成交的話則為賣出。  詳細的定義請參考：https://www.xq.com.tw/台股逐筆功能行情端相關異動/。  欄位支援分鐘頻率(該分鐘區間的累計)，以及日頻率(當日開盤迄今的累計)。  如果是指數型商品的話，數值則是成分股的累計。  資料從2020年開始提供。 |

---
## 買進小單金額
**Path:** 選股欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 買進小單金額 (選股欄位) |
| 語法 | ``` Value1 = GetField("買進小單金額"); Value1 = GetField("BidVolumePrice_S"); ``` |
| 欄位分類 | 量能 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 |
| 支援商品 | 類股指數 台(股票) |
| 說明 | 買進小單的成交金額。  系統依照商品每筆成交的價格以及成交金額，判斷這筆成交是小單，中單，大單，或是特大單。如果是外盤成交，則為買進，內盤成交的話則為賣出。  詳細的定義請參考：https://www.xq.com.tw/台股逐筆功能行情端相關異動/。  欄位支援分鐘頻率(該分鐘區間的累計)，以及日頻率(當日開盤迄今的累計)。  如果是指數型商品的話，數值則是成分股的累計。  資料從2020年開始提供。 |

---
## 買進特大單成交次數
**Path:** 選股欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 買進特大單成交次數 (選股欄位) |
| 語法 | ``` Value1 = GetField("買進特大單成交次數"); Value1 = GetField("BidTickCount_XL"); ``` |
| 欄位分類 | 量能 |
| 單位 | 次 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 |
| 支援商品 | 類股指數 台(股票) |
| 說明 | 買進特大單的成交次數。  系統依照商品每筆成交的價格以及成交金額，判斷這筆成交是小單，中單，大單，或是特大單。如果是外盤成交，則為買進，內盤成交的話則為賣出。  詳細的定義請參考：https://www.xq.com.tw/台股逐筆功能行情端相關異動/。  欄位支援分鐘頻率(該分鐘區間的累計)，以及日頻率(當日開盤迄今的累計)。  如果是指數型商品的話，數值則是成分股的累計。  資料從2020年開始提供。 |

---
## 買進特大單量
**Path:** 選股欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 買進特大單量 (選股欄位) |
| 語法 | ``` Value1 = GetField("買進特大單量"); Value1 = GetField("BidVolume_XL"); ``` |
| 欄位分類 | 量能 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 |
| 支援商品 | 類股指數 台(股票) |
| 說明 | 買進特大單的成交量。  系統依照商品每筆成交的價格以及成交金額，判斷這筆成交是小單，中單，大單，或是特大單。如果是外盤成交，則為買進，內盤成交的話則為賣出。  詳細的定義請參考：https://www.xq.com.tw/台股逐筆功能行情端相關異動/。  欄位支援分鐘頻率(該分鐘區間的累計)，以及日頻率(當日開盤迄今的累計)。  如果是指數型商品的話，數值則是成分股的累計。  資料從2020年開始提供。 |

---
## 買進特大單金額
**Path:** 選股欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 買進特大單金額 (選股欄位) |
| 語法 | ``` Value1 = GetField("買進特大單金額"); Value1 = GetField("BidVolumePrice_XL"); ``` |
| 欄位分類 | 量能 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 |
| 支援商品 | 類股指數 台(股票) |
| 說明 | 買進特大單的成交金額。  系統依照商品每筆成交的價格以及成交金額，判斷這筆成交是小單，中單，大單，或是特大單。如果是外盤成交，則為買進，內盤成交的話則為賣出。  詳細的定義請參考：https://www.xq.com.tw/台股逐筆功能行情端相關異動/。  欄位支援分鐘頻率(該分鐘區間的累計)，以及日頻率(當日開盤迄今的累計)。  如果是指數型商品的話，數值則是成分股的累計。  資料從2020年開始提供。 |

---
## 賣出中單成交次數
**Path:** 選股欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 賣出中單成交次數 (選股欄位) |
| 語法 | ``` Value1 = GetField("賣出中單成交次數"); Value1 = GetField("AskTickCount_M"); ``` |
| 欄位分類 | 量能 |
| 單位 | 次 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 |
| 支援商品 | 類股指數 台(股票) |
| 說明 | 賣出中單的成交次數。  系統依照商品每筆成交的價格以及成交金額，判斷這筆成交是小單，中單，大單，或是特大單。如果是外盤成交，則為買進，內盤成交的話則為賣出。  詳細的定義請參考：https://www.xq.com.tw/台股逐筆功能行情端相關異動/。  欄位支援分鐘頻率(該分鐘區間的累計)，以及日頻率(當日開盤迄今的累計)。  如果是指數型商品的話，數值則是成分股的累計。  資料從2020年開始提供。 |

---
## 賣出中單量
**Path:** 選股欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 賣出中單量 (選股欄位) |
| 語法 | ``` Value1 = GetField("賣出中單量"); Value1 = GetField("AskVolume_M"); ``` |
| 欄位分類 | 量能 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 |
| 支援商品 | 類股指數 台(股票) |
| 說明 | 賣出中單的成交量。  系統依照商品每筆成交的價格以及成交金額，判斷這筆成交是小單，中單，大單，或是特大單。如果是外盤成交，則為買進，內盤成交的話則為賣出。  詳細的定義請參考：https://www.xq.com.tw/台股逐筆功能行情端相關異動/。  欄位支援分鐘頻率(該分鐘區間的累計)，以及日頻率(當日開盤迄今的累計)。  如果是指數型商品的話，數值則是成分股的累計。  資料從2020年開始提供。 |

---
## 賣出中單金額
**Path:** 選股欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 賣出中單金額 (選股欄位) |
| 語法 | ``` Value1 = GetField("賣出中單金額"); Value1 = GetField("AskVolumePrice_M"); ``` |
| 欄位分類 | 量能 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 |
| 支援商品 | 類股指數 台(股票) |
| 說明 | 賣出中單的成交金額。  系統依照商品每筆成交的價格以及成交金額，判斷這筆成交是小單，中單，大單，或是特大單。如果是外盤成交，則為買進，內盤成交的話則為賣出。  詳細的定義請參考：https://www.xq.com.tw/台股逐筆功能行情端相關異動/。  欄位支援分鐘頻率(該分鐘區間的累計)，以及日頻率(當日開盤迄今的累計)。  如果是指數型商品的話，數值則是成分股的累計。  資料從2020年開始提供。 |

---
## 賣出大單成交次數
**Path:** 選股欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 賣出大單成交次數 (選股欄位) |
| 語法 | ``` Value1 = GetField("賣出大單成交次數"); Value1 = GetField("AskTickCount_L"); ``` |
| 欄位分類 | 量能 |
| 單位 | 次 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 |
| 支援商品 | 類股指數 台(股票) |
| 說明 | 賣出大單的成交次數。  系統依照商品每筆成交的價格以及成交金額，判斷這筆成交是小單，中單，大單，或是特大單。如果是外盤成交，則為買進，內盤成交的話則為賣出。  詳細的定義請參考：https://www.xq.com.tw/台股逐筆功能行情端相關異動/。  欄位支援分鐘頻率(該分鐘區間的累計)，以及日頻率(當日開盤迄今的累計)。  如果是指數型商品的話，數值則是成分股的累計。  資料從2020年開始提供。 |

---
## 賣出大單量
**Path:** 選股欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 賣出大單量 (選股欄位) |
| 語法 | ``` Value1 = GetField("賣出大單量"); Value1 = GetField("AskVolume_L"); ``` |
| 欄位分類 | 量能 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 |
| 支援商品 | 類股指數 台(股票) |
| 說明 | 賣出大單的成交量。  系統依照商品每筆成交的價格以及成交金額，判斷這筆成交是小單，中單，大單，或是特大單。如果是外盤成交，則為買進，內盤成交的話則為賣出。  詳細的定義請參考：https://www.xq.com.tw/台股逐筆功能行情端相關異動/。  欄位支援分鐘頻率(該分鐘區間的累計)，以及日頻率(當日開盤迄今的累計)。  如果是指數型商品的話，數值則是成分股的累計。  資料從2020年開始提供。 |

---
## 賣出大單金額
**Path:** 選股欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 賣出大單金額 (選股欄位) |
| 語法 | ``` Value1 = GetField("賣出大單金額"); Value1 = GetField("AskVolumePrice_L"); ``` |
| 欄位分類 | 量能 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 |
| 支援商品 | 類股指數 台(股票) |
| 說明 | 賣出大單的成交金額。  系統依照商品每筆成交的價格以及成交金額，判斷這筆成交是小單，中單，大單，或是特大單。如果是外盤成交，則為買進，內盤成交的話則為賣出。  詳細的定義請參考：https://www.xq.com.tw/台股逐筆功能行情端相關異動/。  欄位支援分鐘頻率(該分鐘區間的累計)，以及日頻率(當日開盤迄今的累計)。  如果是指數型商品的話，數值則是成分股的累計。  資料從2020年開始提供。 |

---
## 賣出小單成交次數
**Path:** 選股欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 賣出小單成交次數 (選股欄位) |
| 語法 | ``` Value1 = GetField("賣出小單成交次數"); Value1 = GetField("AskTickCount_S"); ``` |
| 欄位分類 | 量能 |
| 單位 | 次 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 |
| 支援商品 | 類股指數 台(股票) |
| 說明 | 賣出小單的成交次數。  系統依照商品每筆成交的價格以及成交金額，判斷這筆成交是小單，中單，大單，或是特大單。如果是外盤成交，則為買進，內盤成交的話則為賣出。  詳細的定義請參考：https://www.xq.com.tw/台股逐筆功能行情端相關異動/。  欄位支援分鐘頻率(該分鐘區間的累計)，以及日頻率(當日開盤迄今的累計)。  如果是指數型商品的話，數值則是成分股的累計。  資料從2020年開始提供。 |

---
## 賣出小單量
**Path:** 選股欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 賣出小單量 (選股欄位) |
| 語法 | ``` Value1 = GetField("賣出小單量"); Value1 = GetField("AskVolume_S"); ``` |
| 欄位分類 | 量能 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 |
| 支援商品 | 類股指數 台(股票) |
| 說明 | 賣出小單的成交量。  系統依照商品每筆成交的價格以及成交金額，判斷這筆成交是小單，中單，大單，或是特大單。如果是外盤成交，則為買進，內盤成交的話則為賣出。  詳細的定義請參考：https://www.xq.com.tw/台股逐筆功能行情端相關異動/。  欄位支援分鐘頻率(該分鐘區間的累計)，以及日頻率(當日開盤迄今的累計)。  如果是指數型商品的話，數值則是成分股的累計。  資料從2020年開始提供。 |

---
## 賣出小單金額
**Path:** 選股欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 賣出小單金額 (選股欄位) |
| 語法 | ``` Value1 = GetField("賣出小單金額"); Value1 = GetField("AskVolumePrice_S"); ``` |
| 欄位分類 | 量能 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 |
| 支援商品 | 類股指數 台(股票) |
| 說明 | 賣出小單的成交金額。  系統依照商品每筆成交的價格以及成交金額，判斷這筆成交是小單，中單，大單，或是特大單。如果是外盤成交，則為買進，內盤成交的話則為賣出。  詳細的定義請參考：https://www.xq.com.tw/台股逐筆功能行情端相關異動/。  欄位支援分鐘頻率(該分鐘區間的累計)，以及日頻率(當日開盤迄今的累計)。  如果是指數型商品的話，數值則是成分股的累計。  資料從2020年開始提供。 |

---
## 賣出特大單成交次數
**Path:** 選股欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 賣出特大單成交次數 (選股欄位) |
| 語法 | ``` Value1 = GetField("賣出特大單成交次數"); Value1 = GetField("AskTickCount_XL"); ``` |
| 欄位分類 | 量能 |
| 單位 | 次 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 |
| 支援商品 | 類股指數 台(股票) |
| 說明 | 賣出特大單的成交次數。  系統依照商品每筆成交的價格以及成交金額，判斷這筆成交是小單，中單，大單，或是特大單。如果是外盤成交，則為買進，內盤成交的話則為賣出。  詳細的定義請參考：https://www.xq.com.tw/台股逐筆功能行情端相關異動/。  欄位支援分鐘頻率(該分鐘區間的累計)，以及日頻率(當日開盤迄今的累計)。  如果是指數型商品的話，數值則是成分股的累計。  資料從2020年開始提供。 |

---
## 賣出特大單量
**Path:** 選股欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 賣出特大單量 (選股欄位) |
| 語法 | ``` Value1 = GetField("賣出特大單量"); Value1 = GetField("AskVolume_XL"); ``` |
| 欄位分類 | 量能 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 |
| 支援商品 | 類股指數 台(股票) |
| 說明 | 賣出特大單的成交量。  系統依照商品每筆成交的價格以及成交金額，判斷這筆成交是小單，中單，大單，或是特大單。如果是外盤成交，則為買進，內盤成交的話則為賣出。  詳細的定義請參考：https://www.xq.com.tw/台股逐筆功能行情端相關異動/。  欄位支援分鐘頻率(該分鐘區間的累計)，以及日頻率(當日開盤迄今的累計)。  如果是指數型商品的話，數值則是成分股的累計。  資料從2020年開始提供。 |

---
## 賣出特大單金額
**Path:** 選股欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 賣出特大單金額 (選股欄位) |
| 語法 | ``` Value1 = GetField("賣出特大單金額"); Value1 = GetField("AskVolumePrice_XL"); ``` |
| 欄位分類 | 量能 |
| 單位 | 元 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 |
| 支援商品 | 類股指數 台(股票) |
| 說明 | 賣出特大單的成交金額。  系統依照商品每筆成交的價格以及成交金額，判斷這筆成交是小單，中單，大單，或是特大單。如果是外盤成交，則為買進，內盤成交的話則為賣出。  詳細的定義請參考：https://www.xq.com.tw/台股逐筆功能行情端相關異動/。  欄位支援分鐘頻率(該分鐘區間的累計)，以及日頻率(當日開盤迄今的累計)。  如果是指數型商品的話，數值則是成分股的累計。  資料從2020年開始提供。 |

---
## 鉅額交易量
**Path:** 選股欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 鉅額交易量 (選股欄位) |
| 語法 | ``` Value1 = GetField("鉅額交易量"); Value1 = GetField("BlockTrade"); ``` |
| 欄位分類 | 量能 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 |
| 支援商品 | 台股 |
| 說明 | 鉅額交易的成交量加總。  週頻率以上為區間累計值。 |

---
## 開盤委買
**Path:** 選股欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 開盤委買 (選股欄位) |
| 語法 | ``` Value1 = GetField("開盤委買"); Value1 = GetField("BidAtOpen"); Value1 = GetField("DayOpenBid"); ``` |
| 欄位分類 | 量能 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 |
| 支援商品 | 台股 類股指數 |
| 說明 | 開盤時委託買進的數量。  台股的數值是開盤第一筆成交前五檔委買的數量加總。  類股商品（細產業指標、主題指標）的計算方式則是成分股的開盤委買的加總。  大盤/期貨/選擇權的數值則是交易所公布的委買委賣資料的第一筆數值。大盤的數值會扣除權證。 |

---
## 開盤委賣
**Path:** 選股欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 開盤委賣 (選股欄位) |
| 語法 | ``` Value1 = GetField("開盤委賣"); Value1 = GetField("AskAtOpen"); Value1 = GetField("DayOpenAsk"); ``` |
| 欄位分類 | 量能 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 |
| 支援商品 | 台股 類股指數 |
| 說明 | 開盤時委託賣出的數量。  台股的數值是開盤第一筆成交前五檔委賣的數量加總。  類股商品（細產業指標、主題指標）的計算方式則是成分股的開盤委賣的加總。  大盤/期貨/選擇權的數值則是交易所公布的委買委賣資料的第一筆數值。大盤的數值會扣除權證。 |

---
## 開盤量
**Path:** 選股欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 開盤量 (選股欄位) |
| 語法 | ``` Value1 = GetField("開盤量"); Value1 = GetField("VolumeAtOpen"); ``` |
| 欄位分類 | 量能 |
| 單位 | 張 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 |
| 支援商品 | 台股 |
| 說明 | 每天第一筆成交的成交量。 |

---
## 零股量
**Path:** 選股欄位 > 量能

|  |  |
| --- | --- |
| 欄位名稱 | 零股量 (選股欄位) |
| 語法 | ``` Value1 = GetField("零股量"); Value1 = GetField("OddLot"); Value1 = GetField("盤後零股成交量"); ``` |
| 欄位分類 | 量能 |
| 單位 | 股 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 日 週 月 |
| 支援商品 | 台股 |
| 說明 | 盤後零股交易的成交股數  週頻率以上為區間累計值 |

