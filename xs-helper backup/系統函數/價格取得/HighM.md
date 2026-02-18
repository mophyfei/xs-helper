# HighM -  (系統函數)（價格取得）

## 語法

取得月線的最高價。  
僅限使用於月線以下之頻率。  
回傳數值=HighM(期別)  
傳入一個參數:  
- 第一個參數是期別，和序列引用定義相同，0表當期、1表前一期...依此類推。

## 說明

當使用頻率小於月線時，用HighM可以找到某期的月最高價。

範例：

```xs
plot1(HighM(0)); //繪製當月最高價的連線
plot2(HighM(1)); //繪製前一月最高價的連線
```

相關的函數包含:

* [OpenD](api?a=opend&b=sys), [OpenW](api?a=openw&b=sys), [OpenM](api?a=openm&b=sys), [OpenQ](api?a=openq&b=sys), [OpenH](api?a=openh&b=sys), [OpenY](api?a=openy&b=sys)
* [HighD](api?a=highd&b=sys), [HighW](api?a=highw&b=sys), [HighM](api?a=highm&b=sys), [HighQ](api?a=highq&b=sys), [HighH](api?a=highh&b=sys), [HighY](api?a-highy&b=sys)
* [LowD](api?a=lowd&b=sys), [LowW](api?a=loww&b=sys), [LowM](api?a=lowm&b=sys), [LowQ](api?a=lowq&b=sys), [LowH](api?a=lowh&b=sys), [LowY](api?a=lowy&b=sys)
* [CloseD](api?a=closed&b=sys), [CloseW](api?a=closew&b=sys), [CloseM](api?a=closem&b=sys), [CloseQ](api?a=closeq&b=sys), [CloseH](api?a=closeh&b=sys), [CloseY](api?a=closey&b=sys)
