# Permutation -  (內建函數)（數學函數）

## 語法

計算從集合個數M內取出N個元素的可能排列個數  
回傳數值 = Permutation(集合個數M, 欲取出的個數N)

## 說明

計算從N個不同數字的集合內取出M個不同數字的[可能排列個數](https://zh.wikipedia.org/wiki/%E7%BB%84%E5%90%88%E6%95%B0%E5%AD%A6)。

範例:

```xs
Value1 = Permutation(3, 2);  // Value1 = 6
```

假設母集合有三個數字 A, B, C, 則取出任意兩個不同數字的可能排列方式 = (A,B), (A,C), (B,A), (B,C), (C,A), (C,B) 共六種。

請參考 [Combination函數](api?a=combination&b=bif)
