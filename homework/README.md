# Customer Order Entry（客戶訂單系統）
## Customer（客戶）
| CustomerID | CustomerName       | Street   | City   | Country | Zip | Phone      | Fax        | SinceDate | CreditRating |
|------------|--------------------|----------|--------|---------|-----|-------------|-------------|------------|----------------|
| C001       | 宏昌科技股份有限公司 | 中山北路 | 台北市 | 台灣    | 104 | 02-11223344 | 02-33445566 | 1985-07-01 | A+             |
| C002       | 翔宇文具店           | 中正路   | 新竹市 | 台灣    | 300 | 03-22334455 | 03-55667788 | 1990-03-15 | B              |

## Order（訂單）
| OrderID | OrderDate  | ShipAddress   | ShipCity | ShipCountry | ShipZip | ShipDate   | CustomerID | EmployeeID |
|---------|------------|----------------|----------|-------------|----------|-------------|--------------|-------------|
| O001    | 2025-04-10 | 中山路5號       | 台北市   | 台灣        | 104      | 2025-04-15 | C001        | E002        |
| O002    | 2025-04-11 | 建國路22號      | 新竹市   | 台灣        | 300      | 2025-04-18 | C002        | E001        |

## OrderDetail（訂單明細）
| OrderID | ProductID | Quantity |
|---------|-----------|----------|
| O001    | P001      | 100      |
| O002    | P002      | 50       |

## Invoice（發票）
| InvoiceID | InvoiceDate | ShipDate   | CardNumber        | CardHolder | ExpiryDate | OrderID | PaymentMethodID |
|-----------|--------------|-------------|---------------------|--------------|-------------|----------|-------------------|
| I001      | 2025-04-10   | 2025-04-11  | 1234567890123456   | 楊祐宇         | 2026-04     | O001     | PM01              |
| I002      | 2025-04-11   | 2025-04-12  | 9876543210987654   | 陳彥福         | 2026-05     | O002     | PM02              |

## PaymentMethod（付款方式）
| PaymentMethodID | MethodName |
|------------------|-------------|
| PM01             | 信用卡       |
| PM02             | 現金         |

## Shipment（出貨紀錄）
| ShipmentID | Quantity | ShipDate   | Status   | OrderID | ProductID | EmployeeID | ShipmentMethodID |
|------------|----------|-------------|-----------|----------|------------|-------------|---------------------|
| SHP001     | 100      | 2025-04-12  | 已出貨    | O001     | P001       | E002        | SM01                |
| SHP002     | 50       | 2025-04-14  | 配送中    | O002     | P002       | E001        | SM02                |
## ShipmentMethod（出貨方式）
| ShipmentMethodID | MethodName |
|-------------------|-------------|
| SM01              | 宅配         |
| SM02              | 超商取貨     |

# Inventory Control（庫存控制系統）
## ProductCategory（產品分類表）
| CategoryID | CategoryName |
|------------|--------------|
| C001       | 電子元件     |
| C002       | 辦公用品     |

## Product（產品表）
| ProductID | ProductName  | SupplierPart# | UnitCost | QuantityOnHand | ReorderPoint | TargetLevel | LeadTime | CategoryID |
|-----------|--------------|----------------|----------|----------------|---------------|--------------|----------|-------------|
| P001      | 電阻 10KΩ     | S1001           | 0.1      | 1000           | 200           | 500          | 3 days   | C001        |
| P002      | A4 影印紙     | S2002           | 3.5      | 300            | 100           | 200          | 5 days   | C002        |

## Supplier（供應商）
| SupplierID | SupplierName       | Address        | City   | Country | Zip | Phone        | Email              | ContactPerson | PaymentTerms |
|------------|--------------------|----------------|--------|---------|-----|---------------|---------------------|----------------|----------------|
| S001       | 電子通股份有限公司 | 科技路一段88號 | 台北市 | 台灣    | 100 | 02-12345678   | contact@eleco.com  | 羅文鍵         | Net 30         |
| S002       | 文具王企業社       | 文具街77巷3號  | 台中市 | 台灣    | 403 | 04-87654321   | sales@officepro.tw | 黃子峻         | Net 15         |

## Employee（員工）
| EmployeeID | Name   |
|------------|--------|
| E001       | 陳彥福 |
| E002       | 楊祐宇 |

## PurchaseOrder（採購單）
| PO_ID  | Description    | OrderDate   | ExpectedDate | ReceivedDate | Tax | SupplierID | EmployeeID |
|--------|----------------|-------------|--------------|---------------|-----|-------------|-------------|
| PO001  | 電阻補貨單     | 2025-04-01  | 2025-04-05   | 2025-04-04    | 50  | S001       | E001        |
| PO002  | 影印紙採購     | 2025-04-03  | 2025-04-08   | 2025-04-07    | 30  | S002       | E002        |

## Transaction（庫存交易紀錄）
| TransID | Date       | Description      | Cost | QuantityReceived | QtyOnHand | QtyUsed | QtyScrapped | ProductID | PO_ID  |
|---------|------------|------------------|------|------------------|-----------|----------|--------------|-----------|--------|
| T001    | 2025-04-04 | 收到電阻貨品     | 0.1  | 500              | 500       | 0        | 0            | P001      | PO001  |
| T002    | 2025-04-06 | 生產用掉電阻     | 0.1  | 0                | 0         | 100      | 5            | P001      | PO001  |
| T003    | 2025-04-07 | 收到影印紙       | 3.5  | 200              | 200       | 0        | 0            | P002      | PO002  |
