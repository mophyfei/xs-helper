# IsFirstCall -  (內建函數)（一般函數）

## 語法

回傳目前計算的K棒（currentbar）是否為事件的第一次洗價  
傳入事件字串：" "、"Bar"、"Date"、"Realtime"、"RealBar"  
回傳布林值=IsFirstCall

## 說明

isfirstcall(" ")：此次執行的第一次洗價

isfirstcall("Bar")：此根 Bar 的第一次洗價

isfirstcall("Date")：此交易日的第一次洗價

isfirstcall("Realtime")：此交易日進入即時洗價區間的第一次洗價

isfirstcall("RealBar")：此交易日進入即時洗價區間，首次產生成交事件後的第一次洗價(通常會與"Realtime"一致；若是在揭示未成交K棒時，遇到暫緩開盤或開盤後沒有成交事件，兩者就會產生差異)

詳細說明與範例請參考：

<https://www.xq.com.tw/learn/xspractice/isfirstcall/>
