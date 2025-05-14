# 👋 Group G01
## 👥 Members 成員
| Student ID 學號 | Name 姓名 | Hobby 興趣 | Work 分工 | 
|------------|---------------|---------------|--------|
| 41143236 | 陳彥福 | coding、動漫    | 資料表schema建置 |
| 41143241 | 黃子峻 | 🐼             | 蒐集資料與整理架構 |
| 41143245 | 楊祐宇 | 🕹️             | 蒐集資料與整理架構 |
| 41143259 | 羅文鍵 | 看動漫、coding  | 建置資料庫 編輯與審核 Github | 

# 🏨 [飯店管理系統](#-link)
## 🔸 應用情境

### 住宿管理系統

| 使用者     | 使用案例說明 |
|------------|---------------|
| 前台人員   | 查詢即時房況（空房、已預訂、維修中），以回應客戶查詢 |
| 客戶       | 線上預訂房間，選擇日期與房型並完成付款 |
| 客務人員   | 登記客戶入住，分配房號並製作房卡 |
| 房務人員   | 查看今日退房房間，更新房間清潔狀態為「待清潔」、「清潔中」、「已完成」 |
| 維修人員   | 接收報修通知（如：冷氣壞掉），並在維修完成後更新狀態 |
| 管理者     | 查看每日入住率、平均房價（ADR）、收益報表（RevPAR） |

### 餐廳管理系統

| 使用者     | 使用案例說明 |
|------------|---------------|
| 客人 | 透過 App 或櫃台預訂午餐或晚餐時段 |
| 餐廳接待 | 查看每日預約用餐人數，安排座位與排班 |
| 廚房人員 | 查看當日各時段點餐內容與總體用量（供餐預估） |
| 庫管人員 | 根據每日餐點消耗，自動更新食材庫存並提醒補貨 |
| 活動部門 | 登記宴會用餐需求（場地、桌數、菜單、時間） |
| 管理者 | 分析熱門菜色、每日供餐人數、毛利與成本比例報表 |

## 系統需求說明

### 住宿管理系統

1. 客房資料管理
    - 建立與管理房型、樓層、價格、設備等資訊

2. 房況即時查詢
    - 前台可查詢目前所有房間狀態（空房、預訂、入住、清潔、維修）

3. 線上預訂管理
    - 提供前台或客戶預訂房間之功能，可選日期與房型

4. 入住/退房流程
    - 記錄客人入住資料，辦理入住與退房作業，自動更新房況

5. 清潔狀態管理
    - 房務人員可標記房間狀態為待清潔、清潔中、完成清潔

6. 維修申請與追蹤
    - 前台或房務可通報設備異常，維修人員可接單與回報完成
      
7. 報表輸出
    - 產出入住率、房間使用統計、維修清潔紀錄報表（每日、每月）
      
### 餐廳管理系統

1. 菜單管理
    - 可設定各餐別(早餐、午餐、晚餐、宴會)菜單內容與價格

2. 餐點訂單與出菜
    - 廚房可根據訂單製作餐點並列出出餐明細

3. 餐飲報表
    - 出餐統計、熱門菜色排行、食材使用與成本報表

## 完整性限制
### 實體完整性（Entity Integrity）
主鍵不可為空值（NULL），並且其值在資料表中必須是唯一的。

| 資料表 (Table)	      | 主鍵欄位 (Primary Key Column(s))	| 說明 |
|---------------------|---------------------------------|------|
| Customer |           	CustomerID | 	        每位顧客都有唯一的識別編號 |
| Room |             	RoomID | 	            每個房間都有唯一的識別編號 |
| Booking |         	BookingID | 	        每筆訂房記錄都有唯一的識別編號 |
| Meal_Plan	|           MealPlanID | 	        每個餐飲方案都有唯一的識別碼 |
| Season |             	SeasonID | 	            每個季節定義都有唯一的識別碼 |
| Restaurant |         	RestaurantID |  	    每間餐廳都有唯一的識別碼 |
| Menu_Item	|           MenuItemID | 	        每個菜單項目都有唯一的識別編號 |
| Employee |            EmployeeID | 	        每位員工都有唯一的識別編號 |
| Room_Type	|           RoomTypeID |         	每種房型都有唯一的識別碼 |
| Meal_Plan_Menu | 	    MealPlanMenuID | 	    餐飲方案與菜單項目的關聯記錄有唯一識別碼 |
| Room_Season_Rate | 	RoomTypeID, SeasonID |  複合主鍵，定義特定房型在特定季節的唯一價格調整 |

### 參照完整性（Referential Integrity）
外鍵欄位的值必須參照到其主參考資料表中已存在的主鍵值，或者外鍵值可以為空值（NULL），除非該外鍵欄位被定義為不可為空。

| 子資料表 (Child Table) | 外鍵欄位 (Foreign Key Column(s)) | 參照主資料表 (Parent Table) | 說明 |
|-----------------------|----------------------------------|----------------------------|-----|
| Booking | 	        CustomerID | 	Customer | 	    每筆訂房記錄必須關聯到一位已存在的顧客 |
| Booking | 	        RoomID | 	    Room | 	        每筆訂房記錄必須關聯到一個已存在的房間 |
| Booking | 	        MealPlanID | 	Meal_Plan | 	訂房記錄可以選擇一個已存在的餐飲方案 |
| Booking | 	        EmployeeID | 	Employee | 	    訂房記錄可以關聯到一位處理該訂單的員工  |
| Room | 	            RoomTypeID | 	Room_Type | 	每個房間必須歸屬於一個已存在的房型 |
| Employee | 	        RestaurantID | 	Restaurant | 	每位員工可以歸屬於一間已存在的餐廳 |
| Menu_Item	|           RestaurantID | 	Restaurant | 	每個菜單項目必須歸屬於一間已存在的餐廳 |
| Meal_Plan_Menu | 	    MealPlanID | 	Meal_Plan | 	此關聯記錄中的餐飲方案必須是已存在的餐飲方案 |
| Meal_Plan_Menu |     	MenuItemID | 	Menu_Item | 	此關聯記錄中包含的菜單項目必須是已存在的項目 |
| Room_Season_Rate | 	RoomTypeID | 	Room_Type | 	價格調整所適用的房型必須是已存在的房型 |
| Room_Season_Rate | 	SeasonID | 	    Season | 	    價格調整所適用的季節必須是已存在的季節定義 |

### 資料型態限制（Domain Constraints）

| 欄位 | 資料型態 | 限制與說明 |
|------|-----------|------------|
| CustomerID, RoomID 等           | int           | 整數，主鍵必須為正整數且唯一 |
| Name, RoomType 等               | varchar(n)    | 字元長度上限（例如 varchar(50)） |
| Phone                           | varchar(15)   | 最多 15 字元，建議符合電話格式 |
| Email                           | varchar(100)  | 建議符合 Email 格式 |
| 價格欄位如 BasePrice、FinalPrice | decimal(10,2) | 最大值 99999999.99，應為非負 |
| PriceAdjustmentPercent          | decimal(5,2)  | 例：20 表示 +20% |
| CheckInDate, CheckOutDate 等    | date          | 必須符合日期格式 yyyy/mm/dd |

### 業務邏輯完整性（Business Rules）

| 條件 | 限制說明 |
|------|----------|
| CheckOutDate > CheckInDate | 退房日需晚於入住日 |
| FinalPrice ≥ BasePrice     | 最終價格應包含基本價格與其他費用 |
| ROOM.RoomStatus            | 限定值如：空房、已預訂、維修中 |
| ROOM.RoomCleanStatus       | 限定值如：待清潔、清潔中、已完成 |
| SEASON.StartDate < EndDate | 季節開始日需早於結束日 |
| MEAL_PLAN_MENU 組合唯一     | 一道菜不能重複在同一方案中出現 |
| ROOM_SEASON_RATE 組合唯一   | 同一房型與季節不能重複設定價格 |

## ER Diagram
### 圖片（點擊圖片🖼️並搭配鍵盤Ctrl⌨️與滑鼠滾輪🖱️可放大檢視圖片🔎）
![ER Diagram](https://github.com/user-attachments/assets/8832f638-0d00-46dd-b787-3cc9ed9f9bc1)

## SQL Schema
```SQL
CREATE TABLE Restaurant (
    RestaurantID INT AUTO_INCREMENT PRIMARY KEY,
    Name VARCHAR(255) NOT NULL,
    DayOfWeek VARCHAR(50) NOT NULL,
    OpenTime TIME NOT NULL,
    CloseTime TIME NOT NULL,
    CHECK (OpenTime < CloseTime)
);

CREATE TABLE Employee (
    EmployeeID INT AUTO_INCREMENT PRIMARY KEY,
    Name VARCHAR(255) NOT NULL,
    Position VARCHAR(100) NOT NULL,
    Department VARCHAR(100) NOT NULL,
    HireDate DATE NOT NULL,
    Phone VARCHAR(50) NOT NULL,
    IsActive BOOLEAN NOT NULL,
    RestaurantID INT,
    FOREIGN KEY (RestaurantID) REFERENCES Restaurant(RestaurantID) ON DELETE SET NULL
);

CREATE TABLE Customer (
    CustomerID INT AUTO_INCREMENT PRIMARY KEY,
    Name VARCHAR(255) NOT NULL,
    Phone VARCHAR(50) NOT NULL,
    Email VARCHAR(255) NOT NULL UNIQUE
);

CREATE TABLE Room_Type (
    RoomTypeID INT AUTO_INCREMENT PRIMARY KEY,
    Name VARCHAR(100) NOT NULL,
    BedCount INT NOT NULL CHECK (BedCount > 0)
);

CREATE TABLE Room (
    RoomID INT AUTO_INCREMENT PRIMARY KEY,
    RoomTypeID INT NOT NULL,
    RoomNumber VARCHAR(50) NOT NULL UNIQUE,
    RoomStatus VARCHAR(50) NOT NULL,
    BasePrice DECIMAL(10, 2) NOT NULL CHECK (BasePrice >= 0),
    FOREIGN KEY (RoomTypeID) REFERENCES Room_Type(RoomTypeID)
);

CREATE TABLE Season (
    SeasonID INT AUTO_INCREMENT PRIMARY KEY,
    Name VARCHAR(100) NOT NULL,
    StartDate DATE NOT NULL,
    EndDate DATE NOT NULL,
    PriceAdjustmentPercent DECIMAL(5, 2) NOT NULL,
    CHECK (StartDate < EndDate),
    CHECK (PriceAdjustmentPercent >= 0)
);

CREATE TABLE Room_Season_Rate (
    RoomTypeID INT NOT NULL,
    SeasonID INT NOT NULL,
    AdjustedPrice DECIMAL(10, 2) NOT NULL CHECK (AdjustedPrice >= 0),
    PRIMARY KEY (RoomTypeID, SeasonID),
    FOREIGN KEY (RoomTypeID) REFERENCES Room_Type(RoomTypeID),
    FOREIGN KEY (SeasonID) REFERENCES Season(SeasonID)
);

CREATE TABLE Meal_Plan (
    MealPlanID INT AUTO_INCREMENT PRIMARY KEY,
    Name VARCHAR(100) NOT NULL,
    ExtraCharge DECIMAL(10, 2) NOT NULL CHECK (ExtraCharge >= 0)
);

CREATE TABLE Menu_Item (
    MenuItemID INT AUTO_INCREMENT PRIMARY KEY,
    RestaurantID INT NOT NULL,
    Name VARCHAR(100) NOT NULL,
    Category VARCHAR(100) NOT NULL,
    Price DECIMAL(10, 2) NOT NULL CHECK (Price >= 0),
    FOREIGN KEY (RestaurantID) REFERENCES Restaurant(RestaurantID)
);

CREATE TABLE Meal_Plan_Menu (
    MealPlanMenuID INT AUTO_INCREMENT PRIMARY KEY,
    MealPlanID INT NOT NULL,
    MenuItemID INT NOT NULL,
    FOREIGN KEY (MealPlanID) REFERENCES Meal_Plan(MealPlanID),
    FOREIGN KEY (MenuItemID) REFERENCES Menu_Item(MenuItemID)
);

CREATE TABLE Booking (
    BookingID INT AUTO_INCREMENT PRIMARY KEY,
    CustomerID INT NOT NULL,
    RoomID INT NOT NULL,
    MealPlanID INT NOT NULL,
    EmployeeID INT NOT NULL,
    CheckInDate DATE NOT NULL,
    CheckOutDate DATE NOT NULL,
    FinalPrice DECIMAL(10, 2) NOT NULL CHECK (FinalPrice >= 0),
    CHECK (CheckOutDate > CheckInDate),
    FOREIGN KEY (CustomerID) REFERENCES Customer(CustomerID),
    FOREIGN KEY (RoomID) REFERENCES Room(RoomID),
    FOREIGN KEY (MealPlanID) REFERENCES Meal_Plan(MealPlanID),
    FOREIGN KEY (EmployeeID) REFERENCES Employee(EmployeeID)
);
```

## SQL 範例資料
```SQL
INSERT INTO Restaurant (Name, DayOfWeek, OpenTime, CloseTime) VALUES
('雲苑廳', '每日', '07:00:00', '22:00:00');

INSERT INTO Room_Type (Name, BedCount) VALUES
('單人房', 1),        -- RoomTypeID 1
('單床雙人房', 1),    -- RoomTypeID 2
('雙床雙人房', 2);    -- RoomTypeID 3
('雙床三人房', 2);    -- RoomTypeID 4
('標準三人房', 3);    -- RoomTypeID 5
('雙床四人房', 2);    -- RoomTypeID 6
('四床四人房', 4);    -- RoomTypeID 7

INSERT INTO Season (Name, StartDate, EndDate, PriceAdjustmentPercent) VALUES
('旺季(暑假)', '2025-06-01', '2025-08-31', 100.00),    -- SeasonID 1
('旺季(寒假)', '2025-01-01', '2025-03-31', 100.00),    -- SeasonID 2
('平時', '2025-09-01', '2025-12-31', 0.00),            -- SeasonID 3
('淡季', '2025-04-01', '2025-05-31', -20.00);          -- SeasonID 4

INSERT INTO Meal_Plan (Name, ExtraCharge) VALUES
('僅住房', 0.00),
('自助早餐', 350.00);

INSERT INTO Customer (Name, Phone, Email) VALUES
('楊祐宇', '0912345678', 'yuyu.yang@email.com'),
('黃子峻', '0922333444', 'tj.huang@email.com');

INSERT INTO Employee (RestaurantID, Name, Position, Department, HireDate, Phone, IsActive) VALUES
(1, '羅文鍵', '餐廳經理', '餐飲部', '2023-08-01', '0987654321', TRUE),
(NULL, '陳彥福', '前台接待員', '客房部', '2024-01-15', '0911111111', TRUE);

INSERT INTO Room (RoomTypeID, RoomNumber, RoomStatus, BasePrice) VALUES
(1, '101', 'Available', 2500.00),
(2, '205', 'Available', 3800.00),
(1, '102', 'Maintenance', 2600.00);

INSERT INTO Menu_Item (RestaurantID, Name, Category, Price) VALUES
(1, '檸檬汁', '飲品', 80.00),
(1, '夸頌', '烘焙坊', 60.00),
(1, '巧克力卡拉雞腿堡', '速食', 60.00),
(1, '美味炒蛋', '早餐熱食', 150.00),
(1, '總匯三明治', '輕食', 220.00);
(1, '長崎素食沙拉', '素食', 280.00);

INSERT INTO Room_Season_Rate (RoomTypeID, SeasonID, AdjustedPrice) VALUES
(1, 1, 4000.00),
(1, 2, 4000.00),
(1, 3, 2000.00),
(1, 4, 1800.00),
(2, 1, 4000.00),
(2, 2, 4000.00),
(2, 3, 2000.00),
(2, 4, 1800.00);

INSERT INTO Meal_Plan_Menu (MealPlanID, MenuItemID) VALUES
(2, 1),
(2, 2),
(2, 3);

INSERT INTO Booking (CustomerID, RoomID, MealPlanID, EmployeeID, CheckInDate, CheckOutDate, FinalPrice) VALUES
(1, 2, 2, 2, '2025-06-10', '2025-06-12', 9440.00),
(2, 1, 1, 2, '2025-07-01', '2025-07-03', 5750.00);
```

## 系統運作流程
1. 系統檢查 customer 表是否有顧客 沒有的話就直接插入添加
2. 根據顧客在網站的操作 新增 booking
3. clean 會表示打掃人員要打掃的房間
4. Meal_Plan 表示是否有要用餐與價格類別
5. Room_Type 表示各種房型 包括房型與床數
6. Season 表示季節 可依時段調整房型價格
7. 前台人員可透過介面調整與得知訂房資訊

## Homework 📖
- [20250507](https://github.com/NFU-DB-Group-113-G01/.github/blob/main/homework/README.md)


## 待辦事項
 - [x] 新增成員
 - [x] 題目
 - [x] 應用情境與使用案例
 - [x] 系統需求說明
 - [x] 完整性限制
 - [x] ER Diagram及詳細說明
 - [x] 資料庫Schema


## 🔗 Link
- [簡報](https://www.canva.com/design/DAGj9Gu7cDI/K8RuwSeM_7NDCD4SlW22tg/view?utm_content=DAGj9Gu7cDI&utm_campaign=designshare&utm_medium=link2&utm_source=uniquelinks&utlId=h9883e3e557)
