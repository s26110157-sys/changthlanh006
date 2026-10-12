<!DOCTYPE html>
<html lang="zh-TW">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>🌸 可愛粉色泡泡迷因產生器 🌸</title>

  <!-- 引入 Tailwind CSS 工具庫（快速建構響應式排版） -->
  <script src="https://cdn.tailwindcss.com"></script>
  
  <!-- 引入 Google Fonts: Fredoka (標題可愛字體) & Noto Sans TC (中文內文字體) -->
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Fredoka:wght@400;600;700&family=Noto+Sans+TC:wght@400;700;900&display=swap" rel="stylesheet">
  
  <!-- 引入 Font Awesome 圖示庫 -->
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">

  <!-- 引入 html2canvas 程式庫：用於將 HTML 區塊轉為圖片下載 -->
  <script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js"></script>

  <style>
    /* 全域字體設定 */
    body {
      font-family: 'Fredoka', 'Noto Sans TC', sans-serif;
      background: linear-gradient(135deg, #ffd1dc 0%, #ffe4e1 50%, #f0fff0 100%);
      min-height: 100vh;
      overflow-x: hidden;
    }

    /* 可愛泡泡背景動畫效果 */
    .bubble-bg {
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      z-index: -1;
      overflow: hidden;
      pointer-events: none;
    }

    .bubble {
      position: absolute;
      bottom: -100px;
      background: rgba(255, 182, 193, 0.4);
      border-radius: 50%;
      box-shadow: inset 0 0 10px rgba(255, 255, 255, 0.6);
      animation: floatUp 12s infinite ease-in-out;
    }

    @keyframes floatUp {
      0% {
        transform: translateY(0) scale(0.8);
        opacity: 0;
      }
      20% {
        opacity: 0.7;
      }
      100% {
        transform: translateY(-120vh) scale(1.2);
        opacity: 0;
      }
    }

    /* 粉色玻璃擬態卡片容器 */
    .glass-card {
      background: rgba(255, 255, 255, 0.85);
      backdrop-filter: blur(10px);
      border: 3px solid rgba(255, 192, 203, 0.6);
      box-shadow: 0 10px 25px rgba(255, 182, 193, 0.3);
    }

    /* 可愛粉色按鈕懸停點擊效果 */
    .bubble-btn {
      background: linear-gradient(135deg, #ff8da1 0%, #ff6b8b 100%);
      transition: all 0.25s cubic-bezier(0.175, 0.885, 0.32, 1.275);
      box-shadow: 0 6px 15px rgba(255, 107, 139, 0.4);
    }
    .bubble-btn:hover {
      transform: translateY(-3px) scale(1.02);
      box-shadow: 0 8px 20px rgba(255, 107, 139, 0.6);
    }
    .bubble-btn:active {
      transform: translateY(1px) scale(0.98);
    }

    /* 自訂可愛風格控制項（滑桿 Range Slider） */
    input[type=range] {
      -webkit-appearance: none;
      width: 100%;
      background: #ffd6e0;
      height: 10px;
      border-radius: 10px;
      outline: none;
    }
    input[type=range]::-webkit-slider-thumb {
      -webkit-appearance: none;
      appearance: none;
      width: 22px;
      height: 22px;
      border-radius: 50%;
      background: #ff5e7e;
      cursor: pointer;
      box-shadow: 0 0 6px rgba(0, 0, 0, 0.2);
      border: 2px solid white;
      transition: transform 0.15s ease;
    }
    input[type=range]::-webkit-slider-thumb:hover {
      transform: scale(1.2);
    }

    /* 迷因文字效果：預設黑色重重輪廓描邊，確保在任何底圖上都看得到 */
    .meme-text {
      font-family: 'Fredoka', 'Noto Sans TC', sans-serif;
      font-weight: 900;
      text-transform: uppercase;
      word-break: break-word;
      line-height: 1.2;
      /* 重度黑陰影/描邊：上下左右多重陰影確保粗黑邊框 */
      text-shadow: 
        2px 2px 0 #000, -2px -2px 0 #000,
        2px -2px 0 #000, -2px 2px 0 #000,
        0px 3px 0 #000, 0px -3px 0 #000,
        3px 0px 0 #000, -3px 0px 0 #000,
        2px 3px 5px rgba(0,0,0,0.8);
      user-select: none;
    }
  </style>
</head>
<body class="p-3 sm:p-6 text-gray-800">

  <!-- 背景動態粉色泡泡 -->
  <div class="bubble-bg" id="bubbleContainer"></div>

  <!-- 主頁面容器 (響應式：手機版上下排列，電腦版左右雙欄) -->
  <div class="max-w-6xl mx-auto">
    
    <header class="text-center my-4 sm:my-6">
      <h1 class="text-3xl sm:text-5xl font-extrabold text-pink-600 drop-shadow-md tracking-wider flex items-center justify-center gap-2">
        <span>🫧</span>
        <span>粉色泡泡迷因產生器</span>
        <span>🫧</span>
      </h1>
      <p class="text-pink-500 font-medium text-sm sm:text-base mt-2">
        上傳你的圖片，製作專屬於你的可愛或爆笑迷因吧！✨
      </p>
    </header>

    <!-- 主要內容區塊：左側面板 + 右側預覽 -->
    <main class="grid grid-cols-1 lg:grid-cols-12 gap-6 items-start">

      <section class="lg:col-span-5 glass-card p-5 sm:p-6 rounded-3xl space-y-5">
        
        <!-- 區塊標題 -->
        <h2 class="text-xl font-bold text-pink-600 border-b-2 border-pink-200 pb-2 flex items-center gap-2">
          <i class="fa-solid fa-sliders"></i> 控制面板
        </h2>

        <!-- 1. 圖片上傳區塊 -->
        <div class="space-y-2">
          <label class="block text-sm font-bold text-gray-700">
            <i class="fa-solid fa-image text-pink-500 mr-1"></i> 上傳圖片底圖
          </label>
          <!-- 檔案選擇按鈕 (預防 CORS，使用 FileReader 讀取本機檔案) -->
          <label class="flex flex-col items-center justify-center w-full h-28 border-2 border-dashed border-pink-300 rounded-2xl cursor-pointer bg-pink-50 hover:bg-pink-100 transition-all text-center p-3">
            <i class="fa-solid fa-cloud-arrow-up text-3xl text-pink-400 mb-1"></i>
            <span class="text-xs sm:text-sm font-semibold text-pink-600">點擊上傳或拖放圖片到這裡</span>
            <span class="text-xs text-pink-400 mt-1">支援 PNG, JPG, GIF 格式</span>
            <input type="file" id="imageInput" accept="image/*" class="hidden" />
          </label>
        </div>

        <!-- 2. 上方與下方文字輸入欄位 -->
        <div class="space-y-4">
          <div>
            <label for="topTextInput" class="block text-sm font-bold text-gray-700 mb-1">
              <i class="fa-solid fa-heading text-pink-500 mr-1"></i> 上方文字 (Top Text)
            </label>
            <input 
              type="text" 
              id="topTextInput" 
              placeholder="輸入上方文字..." 
              value="當你在寫程式碼" 
              class="w-full px-4 py-2.5 rounded-xl border-2 border-pink-200 focus:border-pink-400 focus:outline-none bg-white/80 transition"
            />
          </div>

          <div>
            <label for="bottomTextInput" class="block text-sm font-bold text-gray-700 mb-1">
              <i class="fa-solid fa-font text-pink-500 mr-1"></i> 下方文字 (Bottom Text)
            </label>
            <input 
              type="text" 
              id="bottomTextInput" 
              placeholder="輸入下方文字..." 
              value="結果一次就順利執行！" 
              class="w-full px-4 py-2.5 rounded-xl border-2 border-pink-200 focus:border-pink-400 focus:outline-none bg-white/80 transition"
            />
          </div>
        </div>

        <div class="space-y-4 bg-pink-50/70 p-4 rounded-2xl border border-pink-100">
          
          <!-- 字體大小控制 -->
          <div>
            <div class="flex justify-between text-sm font-bold text-gray-700 mb-1">
              <span><i class="fa-solid fa-text-height text-pink-500 mr-1"></i> 字體大小</span>
              <span id="fontSizeValue" class="text-pink-600 font-extrabold">36px</span>
            </div>
            <input type="range" id="fontSizeSlider" min="16" max="72" value="36" />
          </div>

          <!-- 上方文字垂直位置 offset -->
          <div>
            <div class="flex justify-between text-sm font-bold text-gray-700 mb-1">
              <span><i class="fa-solid fa-arrows-up-down text-pink-500 mr-1"></i> 上方文字垂直位置</span>
              <span id="topPosValue" class="text-pink-600 font-extrabold">5%</span>
            </div>
            <input type="range" id="topPosSlider" min="0" max="40" value="5" />
          </div>

          <!-- 下方文字垂直位置 offset -->
          <div>
            <div class="flex justify-between text-sm font-bold text-gray-700 mb-1">
              <span><i class="fa-solid fa-arrows-up-down text-pink-500 mr-1"></i> 下方文字垂直位置</span>
              <span id="bottomPosValue" class="text-pink-600 font-extrabold">5%</span>
            </div>
            <input type="range" id="bottomPosSlider" min="0" max="40" value="5" />
          </div>

          <!-- 文字顏色選擇器 -->
          <div class="flex items-center justify-between pt-1">
            <label for="textColorPicker" class="text-sm font-bold text-gray-700">
              <i class="fa-solid fa-palette text-pink-500 mr-1"></i> 文字顏色
            </label>
            <div class="flex items-center gap-2">
              <input type="color" id="textColorPicker" value="#ffffff" class="w-8 h-8 rounded-full border-2 border-white shadow cursor-pointer" />
            </div>
          </div>
        </div>

        <!-- 3. 下載按鈕區塊 -->
        <button 
          id="downloadBtn" 
          class="bubble-btn w-full py-3.5 px-6 rounded-2xl text-white font-extrabold text-lg flex items-center justify-center gap-2 shadow-lg"
        >
          <i class="fa-solid fa-download"></i> 一鍵下載迷因 (.png)
        </button>

      </section>

      <section class="lg:col-span-7 glass-card p-4 sm:p-6 rounded-3xl flex flex-col items-center">
        
        <h2 class="text-xl font-bold text-pink-600 border-b-2 border-pink-200 pb-2 w-full mb-4 flex items-center gap-2">
          <i class="fa-solid fa-eye"></i> 即時預覽區
        </h2>

        <!-- 迷因圖片與文字容器（將會被 html2canvas 擷取） -->
        <div 
          id="memeContainer" 
          class="relative w-full max-w-lg bg-gray-100 rounded-2xl overflow-hidden shadow-inner flex items-center justify-center border-4 border-white"
          style="min-height: 320px;"
        >
          <!-- 底圖 (預設占位圖，上傳後更新) -->
          <img 
            id="memeImage" 
            src="https://placehold.co/600x600/ffb6c1/ffffff?text=Upload+Your+Image!" 
            alt="迷因底圖" 
            class="w-full h-auto object-contain block"
          />

          <!-- 上方文字圖層 -->
          <div 
            id="topTextDisplay" 
            class="meme-text absolute left-0 right-0 px-4 text-center text-white pointer-events-none"
            style="top: 5%; font-size: 36px;"
          >
            當你在寫程式碼
          </div>

          <!-- 下方文字圖層 -->
          <div 
            id="bottomTextDisplay" 
            class="meme-text absolute left-0 right-0 px-4 text-center text-white pointer-events-none"
            style="bottom: 5%; font-size: 36px;"
          >
            結果一次就順利執行！
          </div>
        </div>

        <p class="text-xs text-pink-400 mt-3 flex items-center gap-1">
          <i class="fa-solid fa-circle-info"></i> 調整左側數值可即時於畫面上呈現效果
        </p>
      </section>

    </main>

    <!-- 頁尾聲明 -->
    <footer class="text-center text-pink-400 text-xs py-6 mt-6">
      <p>✨ Cute Pink Meme Generator &copy; 2026 ✨</p>
    </footer>
  </div>

  <script>
    // ===== 1. DOM 元素選取 =====
    // 取得頁面中需要操作的 HTML 標籤元素
    const imageInput = document.getElementById('imageInput');
    const memeImage = document.getElementById('memeImage');
    
    const topTextInput = document.getElementById('topTextInput');
    const bottomTextInput = document.getElementById('bottomTextInput');
    const topTextDisplay = document.getElementById('topTextDisplay');
    const bottomTextDisplay = document.getElementById('bottomTextDisplay');

    const fontSizeSlider = document.getElementById('fontSizeSlider');
    const fontSizeValue = document.getElementById('fontSizeValue');
    
    const topPosSlider = document.getElementById('topPosSlider');
    const topPosValue = document.getElementById('topPosValue');
    
    const bottomPosSlider = document.getElementById('bottomPosSlider');
    const bottomPosValue = document.getElementById('bottomPosValue');

    const textColorPicker = document.getElementById('textColorPicker');
    
    const memeContainer = document.getElementById('memeContainer');
    const downloadBtn = document.getElementById('downloadBtn');

    // ===== 2. 背景可愛泡泡生成器 =====
    function createBubbles() {
      const container = document.getElementById('bubbleContainer');
      const bubbleCount = 15; // 生成泡泡數量

      for (let i = 0; i < bubbleCount; i++) {
        const bubble = document.createElement('div');
        bubble.classList.add('bubble');
        
        // 隨機設置泡泡的大小、位置與動畫延遲時間
        const size = Math.random() * 40 + 20; // 20px ~ 60px
        const left = Math.random() * 100; // 0% ~ 100%
        const delay = Math.random() * 8; // 0s ~ 8s
        const duration = Math.random() * 8 + 8; // 8s ~ 16s

        bubble.style.width = `${size}px`;
        bubble.style.height = `${size}px`;
        bubble.style.left = `${left}%`;
        bubble.style.animationDelay = `${delay}s`;
        bubble.style.animationDuration = `${duration}s`;

        container.appendChild(bubble);
      }
    }

    // ===== 3. 本機圖片上傳邏輯 (使用 FileReader 避開 CORS) =====
    imageInput.addEventListener('change', function (e) {
      const file = e.target.files[0];
      if (file) {
        // 使用 HTML5 FileReader API 來讀取本機圖檔
        const reader = new FileReader();
        
        reader.onload = function (event) {
          // 當圖片讀取完畢，將 Data URL 設為 <img> 的 src
          memeImage.src = event.target.result;
        };

        reader.readAsDataURL(file); // 將檔案轉為 Base64 Data URL 格式
      }
    });

    // ===== 4. 即時同步文字輸入 =====
    // 監聽「上方文字」輸入事件
    topTextInput.addEventListener('input', function () {
      topTextDisplay.textContent = this.value || '';
    });

    // 監聽「下方文字」輸入事件
    bottomTextInput.addEventListener('input', function () {
      bottomTextDisplay.textContent = this.value || '';
    });

    // ===== 5. 滑桿控制與樣式同步 =====
    // A. 字體大小控制
    fontSizeSlider.addEventListener('input', function () {
      const size = `${this.value}px`;
      fontSizeValue.textContent = size;
      topTextDisplay.style.fontSize = size;
      bottomTextDisplay.style.fontSize = size;
    });

    // B. 上方文字垂直位置控制 (Top Offset)
    topPosSlider.addEventListener('input', function () {
      const pos = `${this.value}%`;
      topPosValue.textContent = pos;
      topTextDisplay.style.top = pos;
    });

    // C. 下方文字垂直位置控制 (Bottom Offset)
    bottomPosSlider.addEventListener('input', function () {
      const pos = `${this.value}%`;
      bottomPosValue.textContent = pos;
      bottomTextDisplay.style.bottom = pos;
    });

    // D. 文字顏色切換
    textColorPicker.addEventListener('input', function () {
      topTextDisplay.style.color = this.value;
      bottomTextDisplay.style.color = this.value;
    });

    // ===== 6. 一鍵下載迷因圖檔 (html2canvas) =====
    downloadBtn.addEventListener('click', function () {
      // 更改按鈕狀態為處理中
      const originalBtnHTML = downloadBtn.innerHTML;
      downloadBtn.innerHTML = `<i class="fa-solid fa-spinner fa-spin"></i> 正在生成圖片中...`;
      downloadBtn.disabled = true;

      // 使用 html2canvas 將 #memeContainer 區塊轉為 canvas
      html2canvas(memeContainer, {
        useCORS: true, // 支援跨域圖片繪製
        scale: 2, // 提高輸出解析度 (清晰度 2x)
        backgroundColor: null // 保持背景透明或符合原始設定
      }).then(canvas => {
        // 將 canvas 轉為 PNG Data URL
        const imageURL = canvas.toDataURL('image/png');

        // 建立虛擬的 <a> 標籤觸發下載
        const link = document.createElement('a');
        link.href = imageURL;
        link.download = 'my-meme.png'; // 下載檔案名稱
        document.body.appendChild(link);
        link.click();
        document.body.removeChild(link);

        // 恢復按鈕狀態
        downloadBtn.innerHTML = originalBtnHTML;
        downloadBtn.disabled = false;
      }).catch(err => {
        console.error('下載失敗：', err);
        alert('生成圖片時發生錯誤，請稍後再試！');
        downloadBtn.innerHTML = originalBtnHTML;
        downloadBtn.disabled = false;
      });
    });

    // ===== 7. 頁面初始啟動 =====
    window.onload = function () {
      createBubbles(); // 產生動態粉色背景泡泡
    };
  </script>
</body>
</html>
