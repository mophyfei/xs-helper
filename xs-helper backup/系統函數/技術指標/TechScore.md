# TechScore -  (系統函數)（技術指標）

## 語法

計算多空判斷分數。  
回傳數值=TechScore

## 說明

利用多種指標計算多空分數，綜合研判後續走勢。使用的指標包括：Arron指標、隨機漫步指標、順勢指標、CMO錢德動量擺動指標、RSI、MACD、MTM、KD、DMI、AR、ACC、TRIX、SAR及均線計14種。

多空判斷分數會介於0~14之間，12以上是超買區、3以下是超賣區。

範例：

```xs
value1 = TechScore;       //計算多空判斷分數
plot1(value1, "多空指標");
```
