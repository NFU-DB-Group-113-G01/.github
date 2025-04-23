# 👋 Group G01
## 👥 Members 成員
1. 41143236 陳彥福 興趣：coding、動漫
2. 41143241 黃子峻
3. 41143245 楊祐宇
4. 41143259 羅文鍵 興趣：看動漫、coding

# 🏨 [飯店管理系統]
## Canva
https://www.canva.com/design/DAGj9Gu7cDI/K8RuwSeM_7NDCD4SlW22tg/view?utm_content=DAGj9Gu7cDI&utm_campaign=designshare&utm_medium=link2&utm_source=uniquelinks&utlId=h9883e3e557
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
### 圖片

![mermaid-diagram-2025-04-08-205351](https://github.com/user-attachments/assets/ae7d8aae-525e-4091-9b82-d68ddc1afda4)

### 程式碼
```erDiagram
erDiagram
    CUSTOMER {
        int CustomerID PK "顧客唯一編號"
        varchar(50) Name "顧客姓名"
        varchar(15) Phone "聯絡電話"
        varchar(100) Email "電子郵件"
    }
    ROOM {
        int RoomID PK "房間唯一編號"
        varchar(30) RoomType "房型（單人房、雙人房等）"
        varchar(30) RoomStatus "房間狀態（空房、已預訂、維修中等）"
        varchar(30) RoomCleanStatus "房間清潔狀態（待清潔、清潔中、已完成等）"
        decimal(10，2) BasePrice "房間基本價格"
    }
    BOOKING {
        int BookingID PK "訂房編號"
        int CustomerID FK "對應的顧客"
        int RoomID FK "訂到的房間"
        date CheckInDate "入住日期"
        date CheckOutDate "退房日期"
        int MealPlanID FK "選擇的餐食方案"
        decimal(10，2) FinalPrice "最終價格（含房價 + 餐食 + 季節調整）"
    }
    MEAL_PLAN {
        int MealPlanID PK "方案 ID"
        varchar(50) Name "名稱（如「一泊一食」、「無餐」）"
        decimal(10，2) ExtraCharge "加購費用"
    }
    SEASON {
        int SeasonID PK "季節 ID"
        varchar(30) Name "季節名稱（如：暑假）"
        date StartDate "開始日期"
        date EndDate "結束日期"
        decimal(5，2) PriceAdjustmentPercent "調整比例（例：20 表示 +20%）" 
    }
    ROOM_SEASON_RATE {
        int RoomID FK "房間 ID"
        int SeasonID FK "季節 ID"
        decimal(10，2) AdjustedPrice "該房型於該季節的特別價格"
        %% Composite PK: (RoomID, SeasonID)
    }
    RESTAURANT {
        int RestaurantID PK "餐廳 ID"
        varchar(50) Name "餐廳名稱"
        varchar(20) OpenHours "營業時間（例如 06:30–10:00）"
    }
    MENU_ITEM {
        int MenuItemID PK "菜色 ID"
        int RestaurantID FK "所屬餐廳"
        varchar(50) Name "菜名（如牛排）"
        varchar(30) Category "類別（如主餐、甜點）"
        decimal(10，2) Price "價格" 
    }
    MEAL_PLAN_MENU {
        int MealPlanID FK "餐食方案 ID"
        int MenuItemID FK "包含的菜色 ID"
        %% Composite PK: (MealPlanID, MenuItemID)
    }
    EMPLOYEE {
        int EmployeeID PK "員工唯一編號"
        varchar(50) Name "員工姓名"
        varchar(30) Position "職位（例如：櫃檯、清潔人員、廚師）"
        varchar(30) Department "部門（例如：前台、房務、餐廳）"
        date HireDate "入職日期"
        varchar(15) Phone "聯絡電話"
        bool IsActive "是否在職 (TRUE/FALSE)"
    }

    %% Relationships (關係)
    CUSTOMER ||--o{ BOOKING : "訂房"
    ROOM ||--o{ BOOKING : "被預訂"
    MEAL_PLAN ||--o{ BOOKING : "選擇"
    ROOM ||--o{ ROOM_SEASON_RATE : "設定季節價格"
    SEASON ||--o{ ROOM_SEASON_RATE : "影響房間價格"
    RESTAURANT ||--o{ MENU_ITEM : "提供"
    MEAL_PLAN ||--o{ MEAL_PLAN_MENU : "包含"
    MENU_ITEM ||--o{ MEAL_PLAN_MENU : "屬於"
```


## 代辦事項
 - [x] 新增成員
 - [ ] 自我介紹 (?
 - [x] 題目
 - [x] 應用情境與使用案例
 - [x] 系統需求說明
 - [x] 完整性限制
 - [x] ER Diagram及詳細說明


## 🔗 Link
[ER Diagram 圖](https://mermaid.live/)

