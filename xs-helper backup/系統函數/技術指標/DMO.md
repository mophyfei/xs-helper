# DMO -  (系統函數)（技術指標）

## 語法

計算DMO指標。  
回傳數值=DMO(期數)

## 說明

DMO指標（Directional Movement Oscillator）是利用DMI趨向指標指標中的正負DI線計算，以此二條線的差值做為新的指標線。

範例：

```xs
value1 = DMO(14);       //計算14期的DMO指標
plot1(value1, "DMO");
```
