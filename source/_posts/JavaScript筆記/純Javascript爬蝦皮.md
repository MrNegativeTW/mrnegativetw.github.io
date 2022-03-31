---
title: 使用純 Javascript 當爬蟲把蝦皮商品資訊抓下來
date: 2022-03-31 10:00:00
tags:
categories:
- JavaScript 筆記
---
原本看到  BigGo 在高雄開 Android Developer 的缺，但投遞後說暫緩招募，問我要不要改應徵 React 工程師，答應後馬上就丟題目過來了，實作過程遇到一些很靠北的地方，所以紀錄一下。
<!--more-->

# 題目要求

這裡就不把題目全部打出來，大概就是

> 蝦皮某搜尋結果，用頁面抓爬的方式把商品連結、標題、價格等資訊抓下來，並存放到 Array 中，外加自動翻頁，限定使用原生 Javascript。

遇到這題目第一直覺就是找 API，但 API 爬蟲是加分題，所以只好照題目說的一個一個抓下來。

單筆資料格式須為：
```json
{
    "title": "",
    "price": 0
}
```

放進陣列後需長這樣 (如果我沒理解錯誤題目的話)：
```json
[
    {
    "title": "",
    "price": 0
    },
    {
    "title": "",
    "price": 0
    }
]
```

# 實作

依我的解決順序排序

## 爬資料

首先我們需要一個 Array

```javascript
let goodsArray = [];
```

接著把所有裝載商品的 `div` 選出來，一個商品佔用一個 `div`，並且全部商品放置在 `shopee-search-item-result__items` 這個 div.class 中

```javascript
let resultItems = document.querySelectorAll(
  "#main > div > div._193wCc > div > div > div.OQtnd7 > div > div.row.shopee-search-item-result__items > div"
);
```

接著就可以把每一個商品，也就是 `div` 中的資料取出來了，取出後結合並 push 至 array 中

```javascript
resultItems.forEach((element) => {
  let item = {};

  let title = element.querySelector(
    "div > a > div > div > div._2x8wqR > div._3GAFiR > div > div"
  ).innerText;

  let img = element.querySelector(
    "div > a > div > div > div._25_r8I.ggJllv > img"
  ).src;

  let price = element.querySelector(
    "div > a > div > div > div._2x8wqR > div._3_FVSo > div > span._3c5u7X"
  ).innerText;

  let link = element.querySelector("div > a").href;

  item["title"] = title;
  item["img"] = img;
  item["price"] = price;
  item["link"] = link;

  goodsArray.push(item);
  console.log(item);
});
```

最後最後直接把整個 array 印出來

```javascript
console.log(goodsArray);
```

在 console 大概中會長這樣

```json
(60) [{…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}]
```

順帶一提，在 F12 的頁面中，對想要的 `div` 右鍵 > 複製 > 複製 JS 路徑，就可以把整串 code 複製起來，像這樣：

```javascript
document.querySelector("#main")
```

## 自動翻頁

自動翻到最後一頁後總要讓程式停下來，原本想採用判斷「下一頁按鈕」，殊不知就算到了最後一頁蝦皮還是會顯示那個按鈕，只好改採用網址 `page` 參數判斷。

判斷過程類似 Two Pointer

1. `priviousPage` 起始值為 `-1`，畢竟在第一頁時沒有上一頁，並每次迴圈加 1。
2. `currentPage` 起始值為 `0`，對應第一頁，並於每次迴圈取得網址 `page` 參數。
3. 每次迴圈 `while` 都會比較兩個數值，當無條件加 1 的 `priviousPage` 遇上卡在最後一頁的 `currentPage` 時，表示到達最後一頁，自動終止。

```javascript
let priviousPage = -1;
let currentPage = 0;

while (priviousPage != currentPage) {

    // ...

    // Go to next page
    document.getElementsByClassName("shopee-icon-button--right")[0].click();
    const urlParams = new URLSearchParams(window.location.search);
    let currentPageInString = urlParams.get("page");
    currentPage = parseInt(currentPageInString);
    priviousPage += 1;
}
```




## 消失的 innerText

翻頁解決後又遇到了一個錯誤：讀不到資訊。

```
Uncaught TypeError: Cannot read properties of null (reading 'innerText')
    at <anonymous>:23:6
    at NodeList.forEach (<anonymous>)
    at <anonymous>:18:15
```

因為蝦皮有 Lazy Loading 的關係，當商品沒有在畫面上顯示時是不會載入相關資訊的，所以必須讓商品可以在頁面上被「看到一次」。

阿然後我跟這塊不熟，就直接暴力解，讓頁面自已往下捲，捲完後等 2 秒讓資訊載入，之後才繼續爬

```javascript
await new Promise((r) => setTimeout(r, 2000));
window.scroll({
    top: document.body.scrollHeight,
    behavior: "smooth",
});
await new Promise((r) => setTimeout(r, 2000));
let resultItems = document.querySelectorAll(
    "#main > div > div._193wCc > div > div > div.OQtnd7 > div > div.row.shopee-search-item-result__items > div"
);
```

這時又會發現一個問題，MBP 畫面太小，最右邊那一欄的商品資訊不會被載入，但這好解決，`CMD` + `-` 狂按直到瀏覽器跟你說嫑嫑，也就是縮放 25%。


# 最終程式碼

```javascript
let priviousPage = -1;
let currentPage = 0;

let goodsArray = [];

while (priviousPage != currentPage) {
  await new Promise((r) => setTimeout(r, 2000));
  window.scroll({
    top: document.body.scrollHeight,
    behavior: "smooth",
  });
  await new Promise((r) => setTimeout(r, 2000));

  // Get goods information
  let resultItems = document.querySelectorAll(
    "#main > div > div._193wCc > div > div > div.OQtnd7 > div > div.row.shopee-search-item-result__items > div"
  );

  resultItems.forEach((element) => {
    let item = {};

    let title = element.querySelector(
      "div > a > div > div > div._2x8wqR > div._3GAFiR > div > div"
    ).innerText;

    let img = element.querySelector(
      "div > a > div > div > div._25_r8I.ggJllv > img"
    ).src;

    let price = element.querySelector(
      "div > a > div > div > div._2x8wqR > div._3_FVSo > div > span._3c5u7X"
    ).innerText;

    let link = element.querySelector("div > a").href;

    item["title"] = title;
    item["img"] = img;
    item["price"] = price;
    item["link"] = link;

    goodsArray.push(item);
  });

  // Go to next page
  document.getElementsByClassName("shopee-icon-button--right")[0].click();
  const urlParams = new URLSearchParams(window.location.search);
  let currentPageInString = urlParams.get("page");
  currentPage = parseInt(currentPageInString);
  priviousPage += 1;
}

// Print items collected til this page.
console.log(goodsArray);
```

## 可以改進的地方

- 其實應該拆成很多 function 會比較容易閱讀或維護，但這是要在瀏覽器 console 執行的就不拆了
- 等 2 秒捲動在等 2 秒才爬真的很暴力，應該有偵測載入完成的方法