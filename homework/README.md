# Customer Order Entry（客戶訂單系統）
![Example1](https://github.com/user-attachments/assets/e08f9b89-7feb-424e-8ea1-8f4c8167cdd3)
## 📋 所有訂單一覽

---

### 🧾 訂單編號：O1001

- **下單日期**：2025-05-10  
- **顧客**：陳彥福（C001）  
- **負責員工**：黃子峻（E001）  
- **出貨地址**：台北市信義路五段10號（110）  
- **承諾出貨日**：2025-05-15  
- **狀態**：處理中  
- **付款方式**：信用卡（PM001）  
- **發票編號**：INV5001  
- **出貨資訊**：
  - S1001：商品 P001（無線滑鼠）數量 2，已完成，出貨方式：黑貓宅急便（SM01）
  - S1002：商品 P002（機械鍵盤）數量 1，已完成，出貨方式：7-11 超商取貨（SM02）  
- **訂單商品明細**：
  - P001：無線滑鼠 × 2（單價 NT$500）
  - P002：機械鍵盤 × 1（單價 NT$1200）

---

### 🧾 訂單編號：O1002

- **下單日期**：2025-05-11  
- **顧客**：羅文鍵（C002）  
- **負責員工**：楊祐宇（E002）  
- **出貨地址**：新北市中正南路88號（220）  
- **承諾出貨日**：2025-05-16  
- **狀態**：已出貨  
- **付款方式**：現金（PM002）  
- **發票編號**：INV5002  
- **出貨資訊**：
  - S1003：商品 P002（機械鍵盤）數量 3，處理中，出貨方式：黑貓宅急便（SM01）  
- **訂單商品明細**：
  - P002：機械鍵盤 × 3（單價 NT$1200）


## 🧾 客戶資料表（Customer）

| customerNo | customerName | customerStreet   | customerCity | customerState | customerZipCode | custTelNo   | custFaxNo  | DOB        | maritalStatus | creditRating |
|------------|--------------|------------------|--------------|----------------|------------------|-------------|------------|-------------|----------------|----------------|
| C001       | 陳彥福       | 中山北路100號    | 台北市       | 台北市         | 104              | 02-12345678 | 02-12345679 | 1985-03-21 | 單身           | A              |
| C002       | 羅文鍵       | 忠孝東路200號    | 新北市       | 新北市         | 220              | 02-87654321 | 02-87654322 | 1990-07-11 | 已婚           | B              |

---

## 🧑‍💼 員工資料表（Employee）

| employeeNo | title | firstName | lastName | address          | workTelExt | homeTelNo  | empEmailAddress     | socialSecurityNumber | DOB        | position   | sex | salary | dateStarted |
|------------|-------|-----------|----------|------------------|------------|-------------|----------------------|------------------------|-------------|-------------|------|--------|--------------|
| E001       | Mr.   | 子峻      | 黃       | 新店區民權路50號 | 301        | 02-29887766 | huang@example.com   | P123456789           | 1980-10-10 | Sales Rep | M    | 55000  | 2015-01-01   |
| E002       | Mr.   | 祐宇      | 楊       | 板橋區文化路120號 | 302        | 02-22776655 | yang@example.com    | F135678945           | 1988-05-23 | Manager   | M    | 70000  | 2017-08-15   |

---

## 🛒 訂單資料表（Order）

| orderNo | orderDate  | billingStreet     | billingCity | billingState | billingZipCode | promisedDate | status | customerNo | employeeNo |
|---------|-------------|--------------------|--------------|----------------|------------------|----------------|--------|--------------|-------------|
| O1001   | 2025-05-10  | 信義路五段10號      | 台北市       | 台北市         | 110              | 2025-05-15     | 處理中 | C001         | E001        |
| O1002   | 2025-05-11  | 中正南路88號        | 新北市       | 新北市         | 220              | 2025-05-16     | 已出貨 | C002         | E002        |

---

## 🧾 發票資料表（Invoice）

| invoiceNo | dateRaised | datePaid  | creditCardNo     | holdersName | expiryDate | orderNo | pMethodNo |
|-----------|-------------|-------------|----------------------|--------------|--------------|----------|------------|
| INV5001   | 2025-05-10  | 2025-05-12  | 1234-5678-9876-5432 | 陳彥福       | 2027-08     | O1001   | PM001     |
| INV5002   | 2025-05-11  | 2025-05-13  | 1111-2222-3333-4444 | 羅文鍵       | 2026-05     | O1002   | PM002     |

---

## 📦 訂單明細表（OrderDetail）

| orderNo | productNo | quantityOrdered |
|---------|------------|--------------------|
| O1001   | P001       | 2                  |
| O1001   | P002       | 1                  |
| O1002   | P002       | 3                  |

---

## 🛍️ 商品資料表（Product）

| productNo | productName | serialNo | unitPrice | quantityOnHand | reorderLevel | reorderQuantity | reorderLeadTime |
|-----------|----------------|-----------|-------------|-------------------|------------------|---------------------|------------------------|
| P001      | 無線滑鼠         | S001A     | 500         | 50                | 20               | 30                  | 7（天）                |
| P002      | 機械鍵盤         | S002B     | 1200        | 30                | 10               | 20                  | 5（天）                |

---

## 💳 付款方式表（PaymentMethod）

| pMethodNo | paymentMethod |
|-----------|------------------|
| PM001     | 信用卡             |
| PM002     | 現金               |

---

## 🚚 出貨資料表（Shipment）

| shipmentNo | quantity | shipmentDate | completeStatus | orderNo | productNo | employeeNo | sMethodNo |
|------------|----------|----------------|------------------|----------|-------------|--------------|--------------|
| S1001      | 2        | 2025-05-11     | 已完成           | O1001   | P001       | E001         | SM01         |
| S1002      | 1        | 2025-05-11     | 已完成           | O1001   | P002       | E001         | SM02         |
| S1003      | 3        | 2025-05-12     | 處理中           | O1002   | P002       | E002         | SM01         |

---

## 🚚 出貨方式表（ShipmentMethod）

| sMethodNo | shipmentMethod |
|------------|-------------------|
| SM01       | 黑貓宅急便            |
| SM02       | 7-11 超商取貨        |

# Inventory Control（庫存控制系統）
![Example2](https://github.com/user-attachments/assets/402ea277-bde4-4a2e-858d-7cd3d6894eb4)
## 📋 所有訂單一覽

---


### 🧾 採購單編號：PO1001

- **下單日期**：2025-04-01  
- **供應商**：台灣電子公司（S001）  
- **負責員工**：陳彥福（E001）  
- **承諾到貨日**：2025-04-05  
- **實際出貨日**：2025-04-04  
- **運費**：NT$500  
- **採購單說明**：風扇補貨  
- **關聯交易紀錄**：
  - `T0001`：商品 `P001`（高速風扇）進貨 50 台，已收貨 50 台，已售出 10 台，損耗 0 台  
- **商品明細**：
  - `P001`：高速風扇 × 50（單價 NT$1200）

---

### 🧾 採購單編號：PO1002

- **下單日期**：2025-04-10  
- **供應商**：安康醫材行（S002）  
- **負責員工**：羅文鍵（E002）  
- **承諾到貨日**：2025-04-12  
- **實際出貨日**：2025-04-11  
- **運費**：NT$200  
- **採購單說明**：體溫計緊急採購  
- **關聯交易紀錄**：
  - `T0002`：商品 `P002`（電子體溫計）進貨 30 支，已收貨 30 支，已售出 25 支，損耗 1 支  
- **商品明細**：
  - `P002`：電子體溫計 × 30（單價 NT$300）

## Employee 員工資料表
| employeeNo | title | firstName | middleName | lastName | address       | workTelExt | homeTelNo  | empEmailAddress                                   | socialSecurityNumber | DOB        | position | sex | salary | dateStarted |
| ---------- | ----- | --------- | ---------- | -------- | ------------- | ---------- | ---------- | ------------------------------------------------- | -------------------- | ---------- | -------- | --- | ------ | ----------- |
| E001       | Mr    | 彥福        |            | 陳        | 台北市信義區松仁路100號 | 1234       | 0223456789 | [yf.chen@company.com](mailto:yf.chen@company.com) | A123456789           | 1985-05-13 | 採購主管     | M   | 60000  | 2015-03-01  |
| E002       | Ms    | 文鍵        |            | 羅        | 新北市板橋區中山路200號 | 1235       | 0223456790 | [wj.luo@company.com](mailto:wj.luo@company.com)   | B234567890           | 1990-08-20 | 倉儲助理     | F   | 42000  | 2019-06-15  |

## Product 商品資料表
| productNo | productName | serialNo | unitPrice | quantityOnHand | reorderLevel | reorderQuantity | reorderLeadTime | categoryNo |
| --------- | ----------- | -------- | --------- | -------------- | ------------ | --------------- | --------------- | ---------- |
| P001      | 高速風扇        | SN123456 | 1200      | 30             | 10           | 50              | 7               | C01        |
| P002      | 電子體溫計       | SN654321 | 300       | 5              | 10           | 30              | 3               | C02        |

## ProductCategory 商品類別資料表
| categoryNo | categoryDescription |
| ---------- | ------------------- |
| C01        | 家電設備                |
| C02        | 醫療器材                |

## Supplier 供應商資料表
| supplierNo | supplierName | supplierStreet | supplierCity | supplierState | supplierZipCode | suppTelNo  | suppFaxNo  | suppEmailAddress                                    | suppWebAddress                                  | contactName | contactTelNo | contactFaxNo | contactEmailAddress                                             | paymentTerms |
| ---------- | ------------ | -------------- | ------------ | ------------- | --------------- | ---------- | ---------- | --------------------------------------------------- | ----------------------------------------------- | ----------- | ------------ | ------------ | --------------------------------------------------------------- | ------------ |
| S001       | 台灣電子公司       | 南京東路三段100號     | 台北市          | 台灣            | 104             | 0223451111 | 0223452222 | [sales@taiwanelec.com](mailto:sales@taiwanelec.com) | [www.taiwanelec.com](http://www.taiwanelec.com) | 林明志         | 0912123456   | 0223452222   | [mingchi.lin@taiwanelec.com](mailto:mingchi.lin@taiwanelec.com) | 月結30天        |
| S002       | 安康醫材行        | 忠孝西路一段45號      | 台中市          | 台灣            | 402             | 0423456789 | 0423456790 | [info@ankangmed.com](mailto:info@ankangmed.com)     | [www.ankangmed.com](http://www.ankangmed.com)   | 張惠雯         | 0933555666   | 0423456790   | [huiwen.chang@ankangmed.com](mailto:huiwen.chang@ankangmed.com) | 現金           |

## PurchaseOrder 採購單資料表
| purchaseOrderNo | purchaseOrderDescription | orderDate  | dateRequired | shippedDate | freightCharge | supplierNo | employeeNo |
| --------------- | ------------------------ | ---------- | ------------ | ----------- | ------------- | ---------- | ---------- |
| PO1001          | 風扇補貨                     | 2025-04-01 | 2025-04-05   | 2025-04-04  | 500           | S001       | E001       |
| PO1002          | 體溫計緊急採購                  | 2025-04-10 | 2025-04-12   | 2025-04-11  | 200           | S002       | E002       |

## Transaction 交易紀錄資料表
| transactionNo | transactionDate | transactionDescription | unitPrice | unitsOrdered | unitsReceived | unitsSold | unitsWastage | productNo | purchaseOrderNo |
| ------------- | --------------- | ---------------------- | --------- | ------------ | ------------- | --------- | ------------ | --------- | --------------- |
| T0001         | 2025-04-04      | 風扇進貨                   | 1200      | 50           | 50            | 10        | 0            | P001      | PO1001          |
| T0002         | 2025-04-11      | 體溫計進貨與銷售               | 300       | 30           | 30            | 25        | 1            | P002      | PO1002          |
