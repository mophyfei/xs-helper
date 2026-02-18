# Combination -  (內建函數)（數學函數）

## 語法

計算從集合個數M內取出N個元素的可能組合數目  
回傳數值 = Combination(集合個數M, 欲取出的個數N)

## 說明

計算從N個不同數字的集合內取出M個不同數字的[可能組合個數](https://zh.wikipedia.org/wiki/%E7%BB%84%E5%90%88%E6%95%B0%E5%AD%A6)。

範例

```xs
Value1 = Combination(3, 2); // Value1 = 3
```

假設母集合有三個數字 A, B, C, 則取出任意兩個數字的可能組合數 = (A,B), (B,C), (A,C) 共三種。

請參考 [Permutation函數](api?a=permutation&b=bif)
