# 👋 Group G01
## 👥 Members 成員
| Student ID 學號 | Name 姓名 | Hobby 興趣 |
|------------|---------------|---------------|
| 41143236 | 陳彥福 | coding、動漫 |
| 41143241 | 黃子峻 | |
| 41143245 | 楊祐宇 | |
| 41143259 | 羅文鍵 | 看動漫、coding |

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
1. ROOM.RoomStatus
   - 限定值如：空房、已預訂、維修中

2. CheckOutDate > CheckInDate
   - 退房日必須晚於入住日

3. RoomID、CustomerID
   - int 每個房間與客戶有唯一識別編號

4. Email、Phone
   - 需符合對應的 Email  或是 電話號碼 格式

5. BasePrice、FinalPrice
   - decimal(10,2) 最大值 99999999.99 應為正整數

6. Name、RoomType
   - 字元長度上限（例: varchar(50)）

## ER Diagram
### 圖片（點擊圖片🖼️並搭配建盤Ctrl⌨️與滑鼠滾輪🖱️可放大檢視圖片🔎）
![ER Diagram](https://github.com/user-attachments/assets/8832f638-0d00-46dd-b787-3cc9ed9f9bc1)

## SQL Schema
```SQL
CREATE TABLE Restaurant (
    RestaurantID INT AUTO_INCREMENT PRIMARY KEY,
    Name VARCHAR(255),
    DayOfWeek VARCHAR(50),
    OpenTime TIME,
    CloseTime TIME
);

CREATE TABLE Employee (
    EmployeeID INT AUTO_INCREMENT PRIMARY KEY,
    Name VARCHAR(255),
    Position VARCHAR(100),
    Department VARCHAR(100),
    HireDate DATE,
    Phone VARCHAR(50),
    IsActive BOOLEAN,
    RestaurantID INT,
    FOREIGN KEY (RestaurantID) REFERENCES Restaurant(RestaurantID)
);

CREATE TABLE Customer (
    CustomerID INT AUTO_INCREMENT PRIMARY KEY,
    Name VARCHAR(255),
    Phone VARCHAR(50),
    Email VARCHAR(255)
);

CREATE TABLE Room_Type (
    RoomTypeID INT AUTO_INCREMENT PRIMARY KEY,
    Name VARCHAR(100),
    BedCount INT
);

CREATE TABLE Room (
    RoomID INT AUTO_INCREMENT PRIMARY KEY,
    RoomTypeID INT,
    RoomNumber VARCHAR(50),
    RoomStatus VARCHAR(50),
    BasePrice DECIMAL(10, 2),
    FOREIGN KEY (RoomTypeID) REFERENCES Room_Type(RoomTypeID)
);

CREATE TABLE Season (
    SeasonID INT AUTO_INCREMENT PRIMARY KEY,
    Name VARCHAR(100),
    StartDate DATE,
    EndDate DATE,
    PriceAdjustmentPercent DECIMAL(5, 2)
);

CREATE TABLE Room_Season_Rate (
    RoomTypeID INT,
    SeasonID INT,
    AdjustedPrice DECIMAL(10, 2),
    PRIMARY KEY (RoomTypeID, SeasonID),
    FOREIGN KEY (RoomTypeID) REFERENCES Room_Type(RoomTypeID),
    FOREIGN KEY (SeasonID) REFERENCES Season(SeasonID)
);

CREATE TABLE Meal_Plan (
    MealPlanID INT AUTO_INCREMENT PRIMARY KEY,
    Name VARCHAR(100),
    ExtraCharge DECIMAL(10, 2)
);

CREATE TABLE Menu_Item (
    MenuItemID INT AUTO_INCREMENT PRIMARY KEY,
    RestaurantID INT,
    Name VARCHAR(100),
    Category VARCHAR(100),
    Price DECIMAL(10, 2),
    FOREIGN KEY (RestaurantID) REFERENCES Restaurant(RestaurantID)
);

CREATE TABLE Meal_Plan_Menu (
    MealPlanMenuID INT AUTO_INCREMENT PRIMARY KEY,
    MealPlanID INT,
    MenuItemID INT,
    FOREIGN KEY (MealPlanID) REFERENCES Meal_Plan(MealPlanID),
    FOREIGN KEY (MenuItemID) REFERENCES Menu_Item(MenuItemID)
);

CREATE TABLE Booking (
    BookingID INT AUTO_INCREMENT PRIMARY KEY,
    CustomerID INT,
    RoomID INT,
    MealPlanID INT,
    EmployeeID INT,
    CheckInDate DATE,
    CheckOutDate DATE,
    FinalPrice DECIMAL(10, 2),
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
('標準大床房', 1),
('豪華雙床房', 2);

INSERT INTO Season (Name, StartDate, EndDate, PriceAdjustmentPercent) VALUES
('旺季', '2025-06-01', '2025-08-31', 15.00),
('淡季', '2025-09-01', '2025-11-30', -10.00);

INSERT INTO Meal_Plan (Name, ExtraCharge) VALUES
('僅住房', 0.00),
('自助早餐', 350.00);

INSERT INTO Customer (Name, Phone, Email) VALUES
('楊祐宇', '0912345678', 'yuyu.yang@email.com'),
('黃子峻', '0922333444', 'tj.huang@email.com');

INSERT INTO Employee (RestaurantID, Name, Position, Department, HireDate, Phone, IsActive) VALUES
(1, '羅文鍵', '餐廳經理', '餐飲部', '2023-08-01', '0912000111', TRUE),
(NULL, '陳彥福', '前台接待員', '客房部', '2024-01-15', '0911111111', TRUE);

INSERT INTO Room (RoomTypeID, RoomNumber, RoomStatus, BasePrice) VALUES
(1, '101', 'Available', 2500.00),
(2, '205', 'Available', 3800.00),
(1, '102', 'Maintenance', 2600.00);

INSERT INTO Menu_Item (RestaurantID, Name, Category, Price) VALUES
(1, '柳橙汁', '飲品', 80.00),
(1, '可頌麵包', '烘焙坊', 60.00),
(1, '美式炒蛋', '早餐熱食', 150.00),
(1, '總匯三明治', '輕食', 220.00);

INSERT INTO Room_Season_Rate (RoomTypeID, SeasonID, AdjustedPrice) VALUES
(1, 1, 2875.00),
(1, 2, 2250.00),
(2, 1, 4370.00),
(2, 2, 3420.00);

INSERT INTO Meal_Plan_Menu (MealPlanID, MenuItemID) VALUES
(2, 1),
(2, 2),
(2, 3);

INSERT INTO Booking (CustomerID, RoomID, MealPlanID, EmployeeID, CheckInDate, CheckOutDate, FinalPrice) VALUES
(1, 2, 2, 2, '2025-06-10', '2025-06-12', 9440.00),
(2, 1, 1, 2, '2025-07-01', '2025-07-03', 5750.00);
```

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
