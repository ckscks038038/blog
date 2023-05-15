---
title: 'REST_API'
date: 2023-04-11T22:06:56+08:00
draft: false
---

## 前言：

最近工作上遇到主管提出，希望把重複的操作變成 API 服務，用以簡化一些不必要且重複的操作。在經過討論後決定使用 REST API 作為 service access interface，畢竟它很主流且易讀易懂，很適合作為新專案的 starting choice. 於是想說來寫一下什麼是 REST API, 要如何使用它。

## REST API:

API 的全名為 Application Programming Interface，簡單說 API 就是電腦與電腦間溝通的媒介，而 REST (Representaional State Transfer)則是在 Internet 上溝通的一種標準，且他是`loose set of rules`，也就是 REST 並沒有明確的規範，只有簡單且大範圍的規則。

舉例來說，當使用者送出一個 Request: `https://api.com/v2/comet`，我們稱之為 URL(Uniform Resource Locator or 全球資源定位器，通常稱為“網址”)，其中前半段`https://api.com`是 domain name，`/v2/comet`則是其用來分別該 request 中所需 resource 的 path。

一個 Request 中會有 Request URL, Request Method, Status Code，其中 Request Method 是動詞，常見的有`POST`, `GET`, `PUT`, `DELETE`，分別具有 CREATE, READ, UPDATE 以及 DELETE 的功能。

REST API 規範了 URL 的格式應該使用**名詞**作為 path，用以分辨使用者需要的 resource 為何。

Request: `https://api.com/v1/comet`

python3 -m tools.remote_ping -d /dev/ttyAP1 -i 1 --deviceVendorName DELTA --deviceModelName M30A-230 --deviceType INVERTER --parserVersion 1 --rtport 32775 -r 19200 -m rtu

python3 -m tools.remote_ping -d 192.168.16.201 -i 73 --deviceVendorName SUNGROW --deviceModelName PC4-PRO-SUNGROW-LOGGER --deviceType METERHEADER --parserVersion 1 --rtport 32551 -m tcp

python3 -m tools.remote_ping -d /dev/ttyAP1 -t holding -i 1 -a 220 --rtport 34541 -m rtu -l 7

query method: get

address_ping:
query param

python3 -m tools.remote_ping -d /dev/ttyAP1 -t holding -i 1 -a 220 --rtport 34541 -m rtu -l 7
Request: https://api.com/v1/address_ping?rtport=34541&d=/dev/ttyAP1&m=rtu&t=holding&a=220&i=1&l=7
