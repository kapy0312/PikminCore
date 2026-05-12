# 🌱 Pikmin Growth Center（皮克敏成長中心）

一個結合**班級經營加分制度**與**皮克敏養成樂趣**的互動式網頁系統。老師可以透過 Google 試算表即時管理各組分數，學生則能在網頁上看到各組專屬皮克敏的成長、進化，並進行趣味互動與製作專屬紀念照。

![Pikmin Theme](https://i.makeagif.com/media/11-07-2024/p-hi7V.gif)

---

## ✨ 功能亮點

### 1. 完整養成進化鏈
- **階段成長**：從「花盆」狀態開始，隨著分數（精華）餵食，進化為「基本皮克敏」，最終達成滿好感度後觸發「拿取禮物」事件
- **禮物倒數系統**：滿好感後進入倒數狀態（時間可自訂），結束後自動獲得隨機造型
- **多樣化造型**：支援紅、黃、藍、紫、白、岩石、羽翅、冰凍等 8 種顏色，每種顏色擁有專屬的造型池（如廚師帽、咖啡杯、泡菜等）

### 2. 豐富的感官互動
- **動態特效**：點擊皮克敏會觸發跳躍動畫，並噴發愛心淡入淡出特效
- **音效體驗**：內建背景音樂與 9 種隨機皮克敏叫聲（支援花盆狀態靜音）
- **環境氛圍**：玻璃擬態（Glassmorphism）UI 設計，搭配動態漂浮動畫

### 3. 🗺️ 冒險地圖系統（新功能）
- **即時定位**：各組的精華數量直接對應地圖上的 52 個點位，皮克敏圖示即時顯示於對應位置
- **縮放與拖曳**：支援滾輪縮放（以游標為中心）、滑鼠拖曳平移，以及行動裝置的雙指縮放與觸控平移
- **多組同點**：同一點位有多組時，皮克敏圖示自動橫向展開排列，點擊任一隻皆可查看該組詳情
- **路徑顯示**：可切換「流動虛線路徑」或「純點位」兩種呈現模式（`SHOW_PATH_LINES` 開關）
- **狀態標示**：禮物倒數中的組別在地圖標記上會顯示小禮物盒圖示

### 4. 自定義設定（Config）
核心參數說明：

| 設定項 | 來源 | 說明 |
|--------|------|------|
| `HeartCapacity` | 程式寫死（13） | 每顆愛心代表的精華數，固定為 13（對應地圖 52 點） |
| `GiftDurationSeconds` | Google Sheet 動態讀取 | 領取禮物所需的倒數秒數 |

> ⚠️ `HeartCapacity` 目前寫死為 **13**（總精華上限 52），與地圖 52 個點位對應。Google Sheet 中的 `HeartCapacity` 設定值將被忽略。

### 5. 📸 照片合成功能
- **成長紀念照**：點擊「製作明信片」按鈕，上傳班級活動照片（PNG / JPG）
- **三層合成**：系統自動將「背景框 + 上傳照片 + 當前皮克敏」進行三層影像合成
- **一鍵下載**：合成完成後可直接儲存，檔名自動帶入組別名稱與日期

---

## 🛠 技術架構

| 類別 | 技術 |
|------|------|
| 前端 | React 18 (UMD)、Tailwind CSS、HTM |
| 後端 | Google Apps Script (GAS) |
| 資料庫 | Google Sheets |
| 通訊 | Fetch API（CORS POST/GET） |
| 圖片合成 | HTML5 Canvas API |
| 地圖互動 | CSS Transform + 原生事件（wheel / touchmove） |

---

## 🚀 快速開始

### 第一步：準備 Google 試算表
1. 建立一個新的 Google 試算表
2. 點擊「擴充功能」>「Apps Script」
3. 將 `code.gs` 的內容貼入編輯器
4. 修改 `SPREADSHEET_ID` 為當前試算表的 ID
5. 執行 `initSetup` 函數，自動建立所有必要工作表（ConfigData、GroupData、PikminDictionary）

### 第二步：部署後端 API
1. 在 Apps Script 點擊「部署」>「新部署」
2. 類型選擇「網頁應用程式」
3. 「誰可以用有權存取」務必選擇「**所有人**」
4. 部署後複製產生的**網頁應用程式 URL**

### 第三步：設定前端網頁
1. 開啟 `index.html`
2. 找到 `GAS_DEPLOY_URL` 常數，替換為剛才複製的 URL
3. 確保資料夾結構包含 `assets/` 與 `audio/` 並放入對應圖檔與音檔

### 第四步：啟動網頁（重要）
> ⚠️ 請勿直接雙擊開啟 `index.html`！必須透過本地伺服器啟動，照片合成與地圖功能才能正常運作。

推薦方式：安裝 VS Code 擴充功能 **Live Server**，對 `index.html` 按右鍵選「Open with Live Server」，網址會變為 `http://127.0.0.1:5500/...`。

---

## 📂 資料夾結構

```
PikminCore/
│  index.html              # 主程式網頁
│  code.gs                 # GAS 後端邏輯
│  README.md
│
├─assets/                  # 皮克敏與造型圖檔
│      map.png             # 冒險地圖圖片（1024 × 682）
│      backPhoto.png       # 照片合成背景框
│      gift.png            # 禮物盒圖示
│      red_pot.png         # 紅色花盆
│      red_base.png        # 紅色皮克敏基本款
│      red_chef_hat.png    # 紅色皮克敏：廚師帽造型
│      red_cup.png         # 紅色皮克敏：咖啡杯造型
│      ... (各色圖檔依此類推)
│
└─audio/                   # 音效檔案
       Walking (Map Theme) - Pikmin Bloom OST (Audio).mp3
       皮克敏叫聲1.mp3
       皮克敏叫聲2.mp3
       ... (共 9 種叫聲)
```

---

## ⚙️ 可調整參數一覽

### 地圖系統

```js
// index.html 頂部的常數區
const SHOW_PATH_LINES = true;  // true = 流動虛線路徑，false = 純點位
const HEART_CAPACITY = 13;     // 每顆心代表的精華數（須配合地圖點位數）
const MAX_ENERGY = 52;         // 精華上限 = HEART_CAPACITY × 4（地圖共 52 點）
const MARKER_GAP = 42;         // 多組同點時，皮克敏橫向展開的間距（px）
```

> 若日後更換地圖並修改點位數量，需同步調整 `MAP_WAYPOINTS` 陣列與 `HEART_CAPACITY`、`MAX_ENERGY` 三個常數，使其保持一致（`HEART_CAPACITY × 4 = 點位總數`）。

### 照片合成

```js
const PHOTO_AREA = {
    x: 左上角X / 圖片總寬,
    y: 左上角Y / 圖片總高,
    w: (右下角X - 左上角X) / 圖片總寬,
    h: (右下角Y - 左上角Y) / 圖片總高,
};
const PIKMIN_SCALE = 0.22;          // 皮克敏佔背景圖寬度的比例
const PIKMIN_OFFSET = { x: 8, y: 8 }; // 皮克敏位置微調（px）
```

### 音量

```js
const VOL_BGM = 1.0;  // 背景音樂音量 (0.0 ~ 1.0)
const VOL_SFX = 0.8;  // 皮克敏叫聲音量 (0.0 ~ 1.0)
```

---

## 🔐 管理員小撇步

- **快速入口**：網頁右上角有一個透明隱藏按鈕，點擊可直通 Google 試算表後台進行數據手動微調
- **重置系統**：若要重新開始一學期的養成，在 GAS 執行 `initSetup` 即可清空所有資料
- **舊資料遷移警告**：若從舊版本（maxEnergy=20）升級，請先至 `GroupData` 工作表清除所有組別第 6 欄（`當前造型`）的 `GIFT_...` 內容，避免升級後出現立即進化的問題

---

## ⚠️ 注意事項

- 背景音樂需在使用者**第一次點擊頁面後**才會開始播放（瀏覽器 Autoplay 限制）
- 照片合成與地圖功能需透過 **http 本地伺服器**開啟（如 Live Server），直接用 `file://` 開啟會失敗
- 照片上傳僅支援 **PNG / JPG** 格式
- Google Sheet 的 `HeartCapacity` 設定目前無效，固定使用程式內的 `HEART_CAPACITY = 13`

---

**🌱 讓我們一起看著皮克敏茁壯成長吧！**