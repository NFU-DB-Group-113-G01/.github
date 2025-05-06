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
-- ==================================================
-- 核心實體 (基於 ER 圖)
-- ==================================================

-- 餐廳表 (依據 ERD 新增)
CREATE TABLE Restaurant (
    RestaurantID INT AUTO_INCREMENT PRIMARY KEY,
    Name VARCHAR(100) NOT NULL,             -- 餐廳名稱
    -- DayOfWeek, OpenTime, CloseTime 若複雜，可能需要獨立的營業時間表
    -- 根據 ERD 屬性簡化:
    DayOfWeek VARCHAR(50),                  -- 例如: '週一至週五', '週末', '每日'
    OpenTime TIME,                          -- 開放時間
    CloseTime TIME,                         -- 關閉時間
    CreatedAt TIMESTAMP DEFAULT CURRENT_TIMESTAMP -- 建立時間
);

-- 員工表 (原 Staff 表)
CREATE TABLE Employee (
    EmployeeID INT AUTO_INCREMENT PRIMARY KEY,
    RestaurantID INT NULL,                  -- 外鍵，員工所屬餐廳 (若為中央人員可為 NULL)
    Username VARCHAR(50) NOT NULL UNIQUE,   -- 登入帳號 (ERD 屬性未顯示，為登入功能添加)
    PasswordHash VARCHAR(255) NOT NULL,     -- 密碼雜湊 (為登入功能添加)
    Name VARCHAR(100) NOT NULL,             -- 姓名 (來自 ERD)
    Position VARCHAR(50),                   -- 職位 (來自 ERD)
    Department VARCHAR(50),                 -- 部門 (來自 ERD)
    HireDate DATE,                          -- 到職日期 (來自 ERD)
    Phone VARCHAR(20),                      -- 電話 (來自 ERD)
    IsActive BOOLEAN DEFAULT TRUE,          -- 是否在職 (來自 ERD)
    CreatedAt TIMESTAMP DEFAULT CURRENT_TIMESTAMP, -- 建立時間
    FOREIGN KEY (RestaurantID) REFERENCES Restaurant(RestaurantID) -- 關聯到餐廳表
);

-- 顧客表
CREATE TABLE Customer (
    CustomerID INT AUTO_INCREMENT PRIMARY KEY,
    Name VARCHAR(100) NOT NULL,             -- 姓名 (來自 ERD, 整合姓氏名字)
    Phone VARCHAR(20) UNIQUE,               -- 電話 (來自 ERD)
    Email VARCHAR(100) UNIQUE,              -- 電子郵件 (來自 ERD)
    CreatedAt TIMESTAMP DEFAULT CURRENT_TIMESTAMP -- 建立時間
);

-- 房型表 (依據 ERD 修改)
CREATE TABLE RoomType (
    RoomTypeID INT AUTO_INCREMENT PRIMARY KEY,
    Name VARCHAR(50) NOT NULL UNIQUE,       -- 房型名稱 (來自 ERD)
    BedCount INT CHECK (BedCount > 0),      -- 床位數 (來自 ERD)
    Description TEXT NULL,                  -- 描述 (ERD 屬性未顯示，保留彈性)
    CreatedAt TIMESTAMP DEFAULT CURRENT_TIMESTAMP -- 建立時間
);

-- 房間表 (依據 ERD 修改)
CREATE TABLE Room (
    RoomID INT AUTO_INCREMENT PRIMARY KEY,
    RoomNumber VARCHAR(10) NOT NULL UNIQUE, -- 房號 (來自 ERD)
    RoomTypeID INT NOT NULL,                -- 房型外鍵 (來自 ERD)
    RoomStatus VARCHAR(20) NOT NULL DEFAULT 'Available', -- 房間狀態 (來自 ERD, 建議用 ENUM 約束: 'Available', 'Occupied', 'Booked', 'Cleaning', 'Maintenance', 'OutOfOrder')
    BasePrice DECIMAL(10, 2) NOT NULL CHECK (BasePrice >= 0), -- 基本價格 (來自 ERD)
    Floor INT NULL,                         -- 樓層 (ERD 屬性未顯示，保留彈性)
    FOREIGN KEY (RoomTypeID) REFERENCES RoomType(RoomTypeID) -- 關聯到房型表
);

-- 季節表 (依據 ERD 新增)
CREATE TABLE Season (
    SeasonID INT AUTO_INCREMENT PRIMARY KEY,
    Name VARCHAR(50) NOT NULL UNIQUE,       -- 季節名稱 (來自 ERD, 例如: '旺季', '淡季', '假日')
    StartDate DATE NOT NULL,                -- 開始日期 (來自 ERD)
    EndDate DATE NOT NULL,                  -- 結束日期 (來自 ERD)
    PriceAdjustmentPercent DECIMAL(5, 2) DEFAULT 0.00, -- 價格調整百分比 (來自 ERD, 例如: 20.00 代表 +20%, -10.00 代表 -10%)
    CONSTRAINT chk_season_dates CHECK (EndDate > StartDate) -- 檢查結束日期需晚於開始日期
);

-- 房間季節價格表 (依據 ERD 新增)
-- 連接 RoomType 和 Season 以定義特定期間的價格
CREATE TABLE RoomSeasonRate (
    RoomTypeID INT NOT NULL,
    SeasonID INT NOT NULL,
    AdjustedPrice DECIMAL(10, 2) NOT NULL CHECK (AdjustedPrice >= 0), -- 調整後價格 (來自 ERD, 此價格適用於此房型在此季節期間)
    PRIMARY KEY (RoomTypeID, SeasonID),     -- 複合主鍵
    FOREIGN KEY (RoomTypeID) REFERENCES RoomType(RoomTypeID) ON DELETE CASCADE, -- 關聯到房型表
    FOREIGN KEY (SeasonID) REFERENCES Season(SeasonID) ON DELETE CASCADE       -- 關聯到季節表
);

-- 餐飲方案表 (依據 ERD 新增)
CREATE TABLE MealPlan (
    MealPlanID INT AUTO_INCREMENT PRIMARY KEY,
    Name VARCHAR(100) NOT NULL UNIQUE,      -- 方案名稱 (來自 ERD, 例如: '僅住房', '含早餐', '半食宿')
    ExtraCharge DECIMAL(10, 2) NOT NULL DEFAULT 0.00 -- 額外費用 (來自 ERD, 此方案的附加費用，可能是每天或每次住宿)
);

-- 菜單項目表 (依據 ERD 修改)
CREATE TABLE MenuItem (
    MenuItemID INT AUTO_INCREMENT PRIMARY KEY,
    RestaurantID INT NOT NULL,              -- 所屬餐廳外鍵 (來自 ERD)
    Name VARCHAR(100) NOT NULL,             -- 項目名稱 (來自 ERD)
    Category VARCHAR(50),                   -- 分類 (來自 ERD)
    Price DECIMAL(10, 2) NOT NULL CHECK (Price >= 0), -- 價格 (來自 ERD)
    IsAvailable BOOLEAN DEFAULT TRUE,       -- 是否供應 (ERD 未顯示，但實務上常用)
    FOREIGN KEY (RestaurantID) REFERENCES Restaurant(RestaurantID) -- 關聯到餐廳表
);

-- 餐飲方案菜單表 (依據 ERD 新增)
-- 作為 MealPlan 和 MenuItem 之間的多對多關聯表
CREATE TABLE MealPlanMenu (
    MealPlanMenuID INT AUTO_INCREMENT PRIMARY KEY, -- 代理主鍵 (如 ERD 所示)
    MealPlanID INT NOT NULL,
    MenuItemID INT NOT NULL,
    -- 可考慮增加份數或餐點類型 (如前菜/主菜) 欄位
    UNIQUE (MealPlanID, MenuItemID),        -- 確保同一項目不被重複加入同一方案
    FOREIGN KEY (MealPlanID) REFERENCES MealPlan(MealPlanID) ON DELETE CASCADE, -- 關聯到餐飲方案表
    FOREIGN KEY (MenuItemID) REFERENCES MenuItem(MenuItemID) ON DELETE CASCADE  -- 關聯到菜單項目表
);

-- 預訂表 (依據 ERD 修改)
CREATE TABLE Booking (
    BookingID INT AUTO_INCREMENT PRIMARY KEY,
    CustomerID INT NOT NULL,                -- 顧客外鍵 (來自 ERD)
    RoomID INT NOT NULL,                    -- 房間外鍵 (來自 ERD)
    EmployeeID INT NULL,                    -- 處理員工外鍵 ('Handled by' 關係, 線上預訂可為 NULL)
    MealPlanID INT NULL,                    -- 餐飲方案外鍵 (來自 ERD, 可選)
    CheckInDate DATE NOT NULL,              -- 入住日期 (來自 ERD)
    CheckOutDate DATE NOT NULL,             -- 退房日期 (來自 ERD)
    FinalPrice DECIMAL(10, 2) NOT NULL CHECK (FinalPrice >= 0), -- 最終價格 (來自 ERD, 根據房型、季節、方案等計算得出)
    BookingStatus VARCHAR(20) NOT NULL DEFAULT 'Confirmed', -- 預訂狀態 (ERD 未顯示，但實務上必要，例如: 'Confirmed', 'CheckedIn', 'CheckedOut', 'Cancelled')
    BookingTime TIMESTAMP DEFAULT CURRENT_TIMESTAMP, -- 預訂時間 (ERD 未顯示，保留用於追蹤)
    FOREIGN KEY (CustomerID) REFERENCES Customer(CustomerID), -- 關聯到顧客表
    FOREIGN KEY (RoomID) REFERENCES Room(RoomID),          -- 關聯到房間表
    FOREIGN KEY (EmployeeID) REFERENCES Employee(EmployeeID), -- 關聯到員工表
    FOREIGN KEY (MealPlanID) REFERENCES MealPlan(MealPlanID), -- 關聯到餐飲方案表
    CONSTRAINT chk_booking_dates CHECK (CheckOutDate > CheckInDate) -- 檢查退房日需晚於入住日
);

-- ==================================================
-- 輔助/日誌記錄表 (由需求或關係推斷)
-- 這些表可能對於完整功能是必要的，但在提供的 ER 圖中未明確作為獨立實體繪製
-- ==================================================

-- 清潔日誌表 (由 'Clean' 關係推斷)
CREATE TABLE CleaningLog (
    LogID INT AUTO_INCREMENT PRIMARY KEY,
    RoomID INT NOT NULL,                    -- 房間外鍵
    EmployeeID INT NULL,                    -- 清潔人員外鍵
    Status VARCHAR(20) NOT NULL DEFAULT 'Pending', -- 狀態 (例如: 'Pending', 'InProgress', 'Completed')
    RequestedAt TIMESTAMP DEFAULT CURRENT_TIMESTAMP, -- 要求清潔時間
    CompletedAt TIMESTAMP NULL,             -- 完成清潔時間
    Notes TEXT,                             -- 備註
    FOREIGN KEY (RoomID) REFERENCES Room(RoomID), -- 關聯到房間表
    FOREIGN KEY (EmployeeID) REFERENCES Employee(EmployeeID) -- 關聯到員工表
);

-- 維修日誌表 (由 RoomStatus='Maintenance' 推斷)
CREATE TABLE MaintenanceLog (
    LogID INT AUTO_INCREMENT PRIMARY KEY,
    RoomID INT NOT NULL,                    -- 房間外鍵
    ReportedByEmployeeID INT NULL,          -- 回報人員外鍵
    AssignedToEmployeeID INT NULL,          -- 維修人員外鍵
    IssueDescription TEXT NOT NULL,         -- 問題描述
    Status VARCHAR(20) NOT NULL DEFAULT 'Reported', -- 狀態 (例如: 'Reported', 'InProgress', 'Completed')
    ReportedAt TIMESTAMP DEFAULT CURRENT_TIMESTAMP, -- 回報時間
    CompletedAt TIMESTAMP NULL,             -- 完成時間
    Notes TEXT,                             -- 備註
    FOREIGN KEY (RoomID) REFERENCES Room(RoomID), -- 關聯到房間表
    FOREIGN KEY (ReportedByEmployeeID) REFERENCES Employee(EmployeeID), -- 關聯到回報員工
    FOREIGN KEY (AssignedToEmployeeID) REFERENCES Employee(EmployeeID) -- 關聯到維修員工
);

-- ==================================================
-- 餐廳銷售點 (POS) 相關表 (ERD 未包含，但餐廳模組可能需要)
-- ==================================================

-- 餐廳訂單表 (用於獨立的餐廳點餐)
CREATE TABLE RestaurantOrder (
    OrderID INT AUTO_INCREMENT PRIMARY KEY,
    RestaurantID INT NOT NULL,              -- 發生訂單的餐廳外鍵
    CustomerID INT NULL,                    -- 顧客外鍵 (如果是住客)
    BookingID INT NULL,                     -- 預訂外鍵 (如果掛房帳)
    TableNumber VARCHAR(10),                -- 桌號
    OrderTime TIMESTAMP DEFAULT CURRENT_TIMESTAMP, -- 下單時間
    TotalAmount DECIMAL(10, 2) DEFAULT 0.00, -- 總金額
    OrderStatus VARCHAR(20) NOT NULL DEFAULT 'Pending', -- 訂單狀態 (例如: 'Pending', 'Preparing', 'Served', 'Paid')
    EmployeeID INT NULL,                    -- 點餐/服務員工外鍵
    PaymentMethod VARCHAR(50),              -- 付款方式
    PaymentTime TIMESTAMP NULL,             -- 付款時間
    FOREIGN KEY (RestaurantID) REFERENCES Restaurant(RestaurantID), -- 關聯到餐廳表
    FOREIGN KEY (CustomerID) REFERENCES Customer(CustomerID),       -- 關聯到顧客表
    FOREIGN KEY (BookingID) REFERENCES Booking(BookingID),          -- 關聯到預訂表
    FOREIGN KEY (EmployeeID) REFERENCES Employee(EmployeeID)        -- 關聯到員工表
);

-- 餐廳訂單明細表 (記錄訂單中的每個項目)
CREATE TABLE RestaurantOrderItem (
    OrderItemID INT AUTO_INCREMENT PRIMARY KEY,
    OrderID INT NOT NULL,                   -- 訂單主檔外鍵
    MenuItemID INT NOT NULL,                -- 菜單項目外鍵
    Quantity INT NOT NULL CHECK (Quantity > 0), -- 數量
    UnitPrice DECIMAL(10, 2) NOT NULL,      -- 下單時的單價
    ItemStatus VARCHAR(20) NOT NULL DEFAULT 'Ordered', -- 項目狀態 (例如: 'Ordered', 'Preparing', 'Served')
    Notes TEXT,                             -- 特殊要求 (例如: '去冰', '不加洋蔥')
    FOREIGN KEY (OrderID) REFERENCES RestaurantOrder(OrderID) ON DELETE CASCADE, -- 關聯到訂單主檔 (主檔刪除時明細也刪除)
    FOREIGN KEY (MenuItemID) REFERENCES MenuItem(MenuItemID)     -- 關聯到菜單項目表
);
```

## SQL 範例資料
```SQL
-- 新增一家餐廳
INSERT INTO Restaurant (Name, DayOfWeek, OpenTime, CloseTime) VALUES
('雲苑廳', '每日', '07:00:00', '22:00:00');

-- 新增員工 (假設 RestaurantID 1 存在)
INSERT INTO Employee (RestaurantID, Username, PasswordHash, Name, Position, Department, HireDate, Phone, IsActive) VALUES
(1, 'manager01', '雜湊後的密碼1', '羅文鍵', '餐廳經理', '餐飲部', '2023-08-01', '0912000111', TRUE),
(NULL, 'frontdesk01', '雜湊後的密碼2', '陳彥福', '前台接待員', '客房部', '2024-01-15', '0911111111', TRUE);

-- 新增房型
INSERT INTO RoomType (Name, BedCount, Description) VALUES
('標準大床房', 1, '舒適的房間，配備一張特大雙人床'),
('豪華雙床房', 2, '寬敞的房間，配備兩張單人床，享市景');

-- 新增房間 (假設 RoomTypeIDs 1 和 2 存在)
INSERT INTO Room (RoomNumber, RoomTypeID, RoomStatus, BasePrice, Floor) VALUES
('101', 1, 'Available', 2500.00, 1),
('205', 2, 'Available', 3800.00, 2);

-- 新增季節
INSERT INTO Season (Name, StartDate, EndDate, PriceAdjustmentPercent) VALUES
('旺季', '2025-06-01', '2025-08-31', 15.00), -- +15%
('淡季', '2025-09-01', '2025-11-30', -10.00); -- -10%

-- 新增房間季節價格 (假設 RoomTypeIDs 1, 2 和 SeasonIDs 1, 2 存在)
INSERT INTO RoomSeasonRate (RoomTypeID, SeasonID, AdjustedPrice) VALUES
(1, 1, 2875.00), -- 標準大床房於旺季價格 (2500 * 1.15)
(1, 2, 2250.00), -- 標準大床房於淡季價格 (2500 * 0.90)
(2, 1, 4370.00), -- 豪華雙床房於旺季價格 (3800 * 1.15)
(2, 2, 3420.00); -- 豪華雙床房於淡季價格 (3800 * 0.90)

-- 新增餐飲方案
INSERT INTO MealPlan (Name, ExtraCharge) VALUES
('僅住房', 0.00),
('自助早餐', 350.00);

-- 新增菜單項目 (假設 RestaurantID 1 存在)
INSERT INTO MenuItem (RestaurantID, Name, Category, Price, IsAvailable) VALUES
(1, '柳橙汁', '飲品', 80.00, TRUE),
(1, '可頌麵包', '烘焙坊', 60.00, TRUE),
(1, '美式炒蛋', '早餐熱食', 150.00, TRUE);

-- 將菜單項目連結到餐飲方案 (假設 MealPlanID 2 = 自助早餐, MenuItemIDs 1,2,3 存在)
INSERT INTO MealPlanMenu (MealPlanID, MenuItemID) VALUES
(2, 1), -- 柳橙汁 包含在 自助早餐 方案
(2, 2), -- 可頌麵包 包含在 自助早餐 方案
(2, 3); -- 美式炒蛋 包含在 自助早餐 方案

-- 新增顧客
INSERT INTO Customer (Name, Phone, Email) VALUES
('楊祐宇', '0912345678', 'yuyu.yang@email.com');

-- 新增預訂 (假設 CustomerID 1, RoomID 2 (豪華雙床房), EmployeeID 2 (前台), MealPlanID 2 (早餐) 存在)
-- 預訂期間為旺季 (6月10-12日)，價格應反映 RoomSeasonRate + MealPlan 費用
-- 這裡需要應用程式邏輯計算 FinalPrice。假設計算結果如下：
-- 調整後房價 = 4370.00。餐飲方案 = 350.00/晚。共 2 晚。總價 = (4370.00 + 350.00) * 2 = 9440.00
INSERT INTO Booking (CustomerID, RoomID, EmployeeID, MealPlanID, CheckInDate, CheckOutDate, FinalPrice, BookingStatus) VALUES
(1, 2, 2, 2, '2025-06-10', '2025-06-12', 9440.00, 'Confirmed');
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
