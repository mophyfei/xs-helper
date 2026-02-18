# EPS法說公佈值 (選股欄位)

## 語法

```xs
Value1 = GetField("EPS法說公佈值");
Value1 = GetField("ReportedQuarterlyEPS");
```

| 屬性 | 值 |
|---|---|
| 欄位分類 | 財務 |
| 單位 | 美元 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 季 |
| 支援商品 | 美(股票) |

## 說明

公司在財報法說會上發佈的季度EPS數值。
美股股票才適用本欄位。

公司在每季財報公佈前通常會召開法說會(Earning Call)，會議內會公佈當季的各項財報數值，也會給出對於未來的展望。

請注意：

1. Earning Call時所公佈的當期EPS，通常是Diluted EPS before non-recurring items, 為Non-GAAP計算。所以可能會跟財務報表內依GAAP會計原則計算的EPS會有不同。
2. SEC並沒有要求所有公司都要召開Earning Call，所以並非每一家公司都會有此欄位。
3. 此欄位的公佈時間與財報的EPS的公佈時間可能會不同。
