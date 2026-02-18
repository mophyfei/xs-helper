# SetInputName -  (內建函數)（一般函數）

## 語法

設定輸入參數(Input)的顯示名稱  
SetInputName(序號, 顯示名稱)  
SetInputName1(顯示名稱)

## 說明

在XS語法內可以使用[Input語法](api?a=input)來設定腳本輸入的參數。

```xs
Input: Length(10);


Plot1(Average(Close, Length));
```

例如上面範例內定義了一個輸入參數，名稱為Length，初始值為10。在腳本內可以直接使用**Length**這個變數，而在腳本執行時則可以利用參數設定畫面來動態修改**Length**的數值，以便讓程式的設計更有彈性。

![Input設定](https://www.xq.com.tw/xstrader/wp-content/uploads/2015/09/InputWithName.png)

如果希望在設定畫面上可以看到中文名稱，而不是英文的變數名稱的話，則可以使用SetInputName這個函數。

```xs
Input: Length(10);


SetInputName(1, "天期");
Plot1(Average(Close, Length));
```

SetInputName必須傳入兩個參數

* 第一個參數是參數的序號，從1開始，
* 第二個參數是參數的顯示名稱

以下是指標設定的畫面，標示處內可以看到透過SetInputName所指定的參數的名稱

![Input設定2](https://www.xq.com.tw/xstrader/wp-content/uploads/2015/09/Input_%E8%A8%AD%E5%AE%9A.png)

SetInputField指令也可以在函數名稱之後直接加上序號，例如SetInputName1, SetInputName2等。如果函數名稱內就包含序號的話，則就不需要傳入序號參數。

上面的範例可以改寫成:

```xs
Input: Length(10);


SetInputName1("天期");
Plot1(Average(Close, Length));
```

在XQ 5.60版之後，為了讓這個動作更簡單，使用者可以直接在Input語法內指定輸入參數的顯示名稱，上面的範例可以改寫成:

```xs
Input: Length(10, "天期");


Plot1(Average(Close, Length));
```

新的Input的語法可以讓程式變的更短，而且由於顯示名稱跟Input可以寫在同一行內，使用者不需要再去記憶每個Input的序號，建議大家以後直接使用Input語法來指定輸入參數的顯示名稱。
