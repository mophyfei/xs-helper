# SetPosition -  (內建函數)（交易函數）

## 語法

調整交易策略至指定的部位。  
SetPosition(目標部位)  
SetPosition(目標部位，委託價格)  
SetPosition(目標部位，label:="指令標記")  
SetPosition(目標部位，委託價格，label:="指令標記")

## 說明

SetPosition函數的第一個參數是目標部位(Position)，代表交易策略預期持有的部位。第二個參數是此次交易的委託價格，如果不傳的話則會使用策略的預設買進/賣出價格。

部位(Position)是XS自動交易語法內最重要的觀念，在自動交易策略執行過程內，每個商品會有一個Position的數值，這個數值可以是0(空手)、大於0(多單)、或是小於0(空單)。

當腳本希望作買進的動作時，腳本可以透過SetPosition(或是其他交易函數)，把目標部位變大，例如如果目前Position是0的話，腳本可以呼叫SetPosition(1)，表示希望買進1張。同樣的，如果腳本希望作賣出的動作時，腳本可以透過SetPosition(或是其他函數)，把目標部位變小。

當腳本呼叫SetPosition(或是其他交易函數)時，系統會比對目前的Position以及新的目標部位的差異，然後送出對應的委託單，如果這些委託單完全成交的話，商品的淨成交部位就會跟目標部位是一樣的。

**範例#1**

```xs
SetPosition(1);
```

把腳本的部位變成1，委託價格使用策略預設的買賣價格。如果原先的Position是0的話，這個指令會買進1張，如果原先的Position是2的話，這個指令會賣出1張。如果原先的Position是1的話，這個指令會檢視目前委託的執行情形，可能會送出改價的委託(如果原先委託尚未成交，且這一次的委託價格跟上一筆委託單的委託價格不一樣的話)，或是不做任何動作。詳細的執行邏輯請參考底下「[交易指令的執行方式](api?a=SetPosition&b=bif#TradeOrderAnchor)」的說明。

**範例#2**

```xs
SetPosition(1, Close);
```

把腳本的部位變成1，委託價格使用目前的收盤價。SetPosition的第二個參數是委託價格，可以傳入一個固定數值(例如100.0)，或是其他的數值運算(例如Close, Close+1.0, 等)。

**範例#3**

```xs
SetPosition(-1);
```

把腳本的部位變成-1，委託價格使用策略預設的買賣價格。如果原先的Position是0的話，這個指令會賣出1張，如果原先的Position是-2的話，這個指令會買進1張。

**範例#4**

```xs
SetPosition(-1, MARKET);
```

把腳本的部位變成-1，委託價格使用市價。系統會依照帳號類型來決定市價單該如何傳送，如果是證券帳號的話，會傳送市價委託，如果是期貨帳號的話，則會傳送範圍市價委託。

**範例#5**

```xs
SetPosition(Position+1, AddSpread(Close, 1));
```

把腳本的部位變成目前的Position再加1張，所以不管原先的Position是多少，這個指令都會買進1張。委託的價格則是目前收盤價再往上加一檔。

[Position](api?a=Position&b=bif)是一個內建的欄位，腳本可以透過這個欄位取得目前的部位數值。
[AddSpread](api?a=AddSpread&b=bif)函數可以用來計算加減檔數後的價格。

除了SetPosition語法可以改變腳本部位之外，系統同時還提供[Buy](api?a=Buy&b=bif)、[Sell](api?a=Sell&b=bif)、[Short](api?a=Short&b=bif)、[Cover](api?a=Cover&b=bif)這幾種語法，請參考相關說明。

**範例#6**

```xs
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

```xs
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

```xs
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

```xs
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

```xs
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
