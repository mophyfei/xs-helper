# TW50MTM多空家數 (資料欄位)

## 語法

```xs
Value1 = GetField("TW50MTM多空家數");
Value1 = GetField("TW50MTMLongShortSecurities");
```

| 屬性 | 值 |
|---|---|
| 欄位分類 | 市場統計 |
| 單位 | 家 |
| 格式 | 數值 |
| 支援腳本 | 指標 警示 交易 函數 |
| 可用頻率 | 分鐘 日 |
| 支援商品 | 大盤 |

## 說明

統計台灣50成分股內，Momentum(10) > 0的家數。
Momentum(N)的公式 = 最近1分鐘的Close - N分鐘前的Close。也就是說這個欄位是最近1分鐘較10分鐘前上漲的家數。

僅支援TSE50.SJ 商品。

系統每分鐘會統計符合的家數，而腳本端則依照執行頻率(支援分鐘/日頻率)來取得當分鐘最後的符合家數。

建議撰寫方式：
value1 = getsymbolfield("TSE50.SJ","TW50MTM多空家數");
