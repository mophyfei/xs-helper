# MTM -  (系統函數)（技術指標）

## 語法

計算收盤價的變動量。  
回傳數值=MTM(期數)

## 說明

以收盤價計算目前K棒與N根K棒之前的差值。

計算公式：

MTM = 當期收盤價 - 前N期收盤價

範例：

```xs
value1 = MTM(10);       //計算收盤價10期的運動量指標
plot1(value1, "MTM");
```

與[Momentum函數](api?a=momentum&b=sys)相同。
