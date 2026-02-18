# 因子_QMJ獲利 (選股欄位)

## 語法

```xs
Value1 = GetField("因子_QMJ獲利");
Value1 = GetField("Factor_QMJ_PROFIT");
Value1 = GetField("因子_QMJ獲利因子");
Value1 = GetField("Factor_QMJ_Profitability Factor");
```

| 屬性 | 值 |
|---|---|
| 欄位分類 | 財務 |
| 格式 | 數值 |
| 支援腳本 | 選股 函數 |
| 可用頻率 | 月 |
| 支援商品 | 台股 |

## 說明

從六個指標(應計項目佔資產比率、自由現金流量佔總資產比率、毛利佔資產比率、毛利佔營收比率、資產報酬率、股東權益報酬率)個別找出每項指標中某檔股票在整體市場的排名，將這六個排名的標準化數值平均，即為該檔股票的綜合獲利能力評分。
