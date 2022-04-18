---
title: Python 3 筆記 - 將後端數值傳送到 Flask 顯示
date: 2018-09-11 10:00:00
tags:
categories:
- Python 3 筆記
---
弄了幾天的問題終於在一份簡報中看到解決辦法了，所以就來做個筆記吧
<!--more-->

# 重點程式碼
這是我最近的一段程式碼，主要是想讓在 Python 裡處理完的 `ans` 可以透過 Flask 顯示在前端，但怎麼找就是找不到如何把資料傳送到前端(Flask)，直到最近在一份教學簡報裡看到類似寫法，這個困繞我3天的問題才解決，首先來看看程式碼吧
```python
# main.py
ans = random.choice(qu)
return render_template('DiffToChoice.html', form=form, ans=ans)
```

```html
# DiffToChoice.html
{% if ans %}
  {{ ans }}
{% else %}
  嗯...
{% endif %}
```
# 解釋
首先我們看到 `main.py`
`ans` 是用來儲存 `random` 選出來的東西，而如何讓這個 `ans`  Flask 中顯示，就是本篇的重點
1. 首先，在 `return render_template` 後面加入 `ans=ans`，也就是讓前端的 `ans` 等於後端的 `ans` 數值，這樣一來就能夠在 Flask 中合理使用這個數值了
2. 再來看到前端，使用方法很簡單，簡單一句 ` { { ans }} ` 就可以了，因為剛剛已經將前端的 `ans` 指定為後端的 `ans`，所以在前端可直接使用這個數值

*第二點中的 `ans`，前方空格請忽略，因為 Hexo 引擎關係會將此段變不見
