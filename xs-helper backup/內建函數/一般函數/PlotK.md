# PlotK -  (內建函數)（一般函數）

## 語法

在腳本運算的橫軸位置上畫出K棒。  
PlotK(序列編號, vOpen, vHigh, vLow, vClose)  
PlotK(序列編號, vOpen, vHigh, vLow, vClose, "序列名稱")

## 說明

序列編號是1~999的數值，與目前XS Plot的序列編號相同。
序列名稱是非必需的參數，如果不傳的話，預設的序列名稱為"Plot"+序列編號，例如”Plot2”。
vOpen, vHigh, vLow, vClose 對應的是K棒的開高低收。

平均K線 (Heikin-Ashi) 範例：

```xs
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
