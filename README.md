# SEL WebAR 互動體驗專案

這是一個使用 WebAR 技術的社會情緒學習（SEL）互動網頁專案，透過擴增實境和動畫效果，幫助使用者探索和理解自己的情緒。

## 📁 專案結構

```
/
├── index.html                 # 首頁（5個主題選單）
├── sel-1-ar.html             # 第1張卡片 AR 版本
├── sel-1-direct.html         # 第1張卡片直接觀看版本
├── sel-2-ar.html             # 第2張卡片 AR 版本
├── sel-2-direct.html         # 第2張卡片直接觀看版本
├── sel-3-ar.html             # 第3張卡片 AR 版本
├── sel-3-direct.html         # 第3張卡片直接觀看版本
├── sel-4-ar.html             # 第4張卡片 AR 版本
├── sel-4-direct.html         # 第4張卡片直接觀看版本
├── sel-5-ar.html             # 第5張卡片 AR 版本
├── sel-5-direct.html         # 第5張卡片直接觀看版本
├── targets-1.mind            # 第1張圖片的特徵檔
├── targets-2.mind            # 第2張圖片的特徵檔
├── targets-3.mind            # 第3張圖片的特徵檔
├── targets-4.mind            # 第4張圖片的特徵檔
├── targets-5.mind            # 第5張圖片的特徵檔
├── sel-1.png                 # 第1張插畫圖片
├── sel-2.png                 # 第2張插畫圖片
├── sel-3.png                 # 第3張插畫圖片
├── sel-4.png                 # 第4張插畫圖片
└── sel-5.png                 # 第5張插畫圖片
```

## 🎯 五個 SEL 主題

1. **被看不見的壓力球** - 情緒覺察（Recognizing Emotions）
2. **面具女孩** - 自我接納（Self-acceptance）
3. **情緒植物園** - 情緒管理（Emotion Regulation）
4. **兩條路的十字路口** - 決策與價值（Decision Making）
5. **黑夜裡的小燈塔** - 求助與支持（Seeking Help）

## 🛠️ 技術實作

### 1. 生成 .mind 特徵檔案

使用 Mind AR Compiler 工具：https://hiukim.github.io/mind-ar-js-doc/tools/compile/

**步驟：**
1. 前往上述網址
2. 上傳對應的插畫圖片（sel-1.png ~ sel-5.png）
3. 調整參數：
   - Image Width: 建議設定為 0.5（可根據實際使用調整）
   - Max Track: 1（一次追蹤一個目標）
   - Filter Min CF: 0.0001（提高識別敏感度）
   - Warm-up Tolerance: 5（增加初始識別容忍度）
4. 點擊 "Compile" 生成 .mind 檔案
5. 下載並命名為 targets-1.mind ~ targets-5.mind

### 2. 動畫實現方式

#### AR 版本（使用 Mind-AR + Three.js）
```javascript
// 使用 Three.js 創建 3D 氣泡
const bubbleGeometry = new THREE.SphereGeometry(0.1, 32, 32);
const bubbleMaterial = new THREE.MeshPhongMaterial({
    color: 0xffffff,
    transparent: true,
    opacity: 0.6
});

// 動畫循環
function animate() {
    // 漂浮效果
    bubble.position.y = Math.sin(time) * 0.1;
    // 旋轉效果
    bubble.rotation.y = time * 0.5;
}
```

#### 直接觀看版本（純 CSS/JS 動畫）
```css
/* CSS 動畫 */
@keyframes float {
    0%, 100% { transform: translateY(0px); }
    50% { transform: translateY(-20px); }
}

/* 氣泡破裂動畫 */
@keyframes pop {
    0% { transform: scale(1); opacity: 1; }
    50% { transform: scale(1.5); }
    100% { transform: scale(0); opacity: 0; }
}
```

### 3. QR Code 生成（選用）

如果需要生成 QR Code，可以使用 qrcode.js：

```html
<script src="https://cdn.jsdelivr.net/npm/qrcodejs@1.0.0/qrcode.min.js"></script>
```

```javascript
// 生成 QR Code
new QRCode(document.getElementById("qrcode"), {
    text: window.location.href.replace('-ar.html', '-direct.html'),
    width: 128,
    height: 128
});
```

## 📱 部署到 GitHub Pages

1. 創建新的 GitHub repository
2. 上傳所有檔案
3. 在 Settings → Pages 中啟用 GitHub Pages
4. 選擇 main branch 和 root 目錄
5. 等待部署完成，訪問 `https://[username].github.io/[repository-name]/`

## 🎨 客製化其他卡片

以第1張卡為模板，實作其他卡片的步驟：

### 第3張卡片「情緒植物園」範例：

1. **修改 HTML 結構**：
```javascript
// 創建5株植物而不是氣泡
const plants = ['怒', '喜', '悲', '焦', '平靜'];
const plantColors = ['#ff4444', '#44ff44', '#4444ff', '#ffaa44', '#44ffaa'];
```

2. **修改互動邏輯**：
```javascript
// 點擊植物顯示調節方法
const emotionTips = {
    '怒': '離開現場、深呼吸',
    '焦': '把事情寫下排序',
    '悲': '跟人說一句心事'
};
```

### 第4張卡片「兩條路的十字路口」範例：

1. **創建路徑動畫**：
```javascript
// 使用 Canvas 或 SVG 繪製兩條路徑
const pathA = document.createElementNS('http://www.w3.org/2000/svg', 'path');
pathA.setAttribute('d', 'M100,200 Q200,100 300,150');
```

2. **物件互動**：
```javascript
const items = ['書本', '手機', '心', '好友', '夢想'];
// 點擊後顯示象徵意義
```

### 第5張卡片「黑夜裡的小燈塔」範例：

1. **海浪動畫**：
```css
@keyframes wave {
    0% { transform: translateX(0) translateY(0); }
    50% { transform: translateX(-25px) translateY(-10px); }
    100% { transform: translateX(0) translateY(0); }
}
```

2. **燈塔光線效果**：
```javascript
// 點擊後光線增強
lighthouse.addEventListener('click', () => {
    lightBeam.style.opacity = '1';
    lightBeam.style.width = '200px';
});
```

## 🔧 常見問題解決

### AR 掃描失敗
1. 確認相機權限已開啟
2. 確保光線充足
3. 圖片平放且無反光
4. 嘗試調整距離（建議 30-50 公分）

### 動畫卡頓
1. 減少同時顯示的動畫元素
2. 使用 CSS transform 而非 position
3. 開啟硬體加速：`transform: translateZ(0)`

### 手機瀏覽器相容性
- 建議使用 Chrome、Safari 最新版本
- iOS 需要 Safari 14.3+
- Android 需要 Chrome 79+

## 📚 相關資源

- [Mind-AR 官方文件](https://hiukim.github.io/mind-ar-js-doc/)
- [Three.js 文件](https://threejs.org/docs/)
- [WebAR 最佳實踐](https://developers.google.com/ar/develop/webxr/best-practices)

## 📝 授權

此專案僅供教育用途使用。

## 🤝 貢獻

歡迎提出 Issue 或 Pull Request 來改進這個專案！

---

**注意事項：**
1. 請確保所有圖片檔案已正確命名並放置在正確位置
2. .mind 檔案需要使用您自己的插畫圖片生成
3. 建議在實機測試 AR 功能
4. 可根據需求調整動畫效果和互動方式
