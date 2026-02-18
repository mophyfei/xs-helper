# angleprice -  (系統函數)（日期相關）

## 語法

傳回「N期至今的角度的趨勢線價格」  
回傳數值=Angleprice(期數,角度)  
傳入二個參數:  
- 第一個參數是期數。  
- 第二個參數是角度。

## 說明

用前N期到現在的角度，運算出趨勢線的價格。

範例：

```xs
value1 = angleprice(5,0);
plot1(value1); //Value1為前五期的開盤價，因為第二個參數是0度，所以會等於前五期的開盤價。
```

更多的資訊，請參考[常用語法匯總](https://www.xq.com.tw/xstrader/%E5%B8%B8%E7%94%A8%E7%9A%84%E8%AA%9E%E6%B3%95%E5%8C%AF%E7%B8%BD/)
