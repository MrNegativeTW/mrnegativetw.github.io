---
title: Python 3 筆記 - WTForms 下拉式選單元件
date: 2018-09-10 10:00:00
tags:
categories:
- Python 3 筆記
- WTForms
---
前幾天在試著用 WTForms 做些東西，然後需要用到下拉選單，
<!--more-->
# 重點程式碼

## Python 端
撰寫在欲用的 class 裡面
```Python
number = SelectField('Label 名稱', choices=[('值','顯示名稱'), ('值','顯示名稱')])
```

## 網頁端
撰寫在欲用的 template 裡面
```
{{ form.hidden_tag() }}
{{ form.number.label(class="form-control-label") }}
{{ form.number(class="form-control") }}
```

# 解釋

## Python 端
`number` 將後面那一坨設定好的全部指定到這上面
`SelectField` 下拉選單
`choices` 這個就是本次重點了，每一個括號都代表著一個項目，每一項目使用 `,` 隔開

## 網頁端
使用方法跟其他類型的一樣
這一行用來呼出 Python 端所設定的 Label 名稱
```
{{ form.number.label(class="form-control-label") }}
```
這一行用來呼出 Python 端所設定的整個下拉選單
```
{{ form.number(class="form-control") }}
```
