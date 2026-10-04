# 光室 · 瀏覽器修圖第一版

繁體中文圖片與影片工作台。靜態網站，無媒體上傳 API、無後端。

## 本機測試

```sh
python3 -m http.server 8080
```

開啟 http://localhost:8080 。GPU 摳圖需 HTTPS 或 localhost、新版 Chrome / Edge 與支援 WebGPU 的 GPU。首次摳圖下載 Transformers.js 與 RMBG-1.4 模型；檔案只在瀏覽器記憶體處理。

## GitHub Pages

在 Settings → Pages 選擇 Deploy from a branch，分支選 main，資料夾選 / (root)，儲存。若使用私有儲存庫，GitHub 方案必須支援私有儲存庫 Pages。網站可公開存取，請依需求確認 Pages 可見性；此網站不包含使用者媒體。

## 功能

- 可用：上傳、拖曳、貼上，裁剪、常用尺寸、自訂尺寸、比例、形狀蒙版、旋轉翻轉，文字樣式、色彩預設與滑桿，復原重做、重置、原圖對比、游標縮放、空白鍵平移、PNG/JPG/WebP 匯出。
- 可用：一般 2 倍 / 4 倍重採樣與可拖曳前後對比線。
- 試用：RMBG-1.4 WebGPU 圖片去背、主體保留、修補筆刷、背景色。模型載入與 GPU 運算失敗時保留原圖。取消會丟棄結果；目前 ONNX 的單次 GPU 運算需等待返回。
- 可用：MP4/WebM 輸入、播放、時間軸、保持比例且不超過 3840×2160 的一般放大、WebM 錄製，串接原片音轨。錄製依正常播放速度進行，需保持頁面前景；需驗證輸出音訊。
- 尚未完成：MI-GAN、U²-Net、Real-ESRGAN / Upscayl 三模型、影片 AI 逐格填補與去背、透明 WebM、MP4 匯出。介面明確停用未完成功能，不以一般縮放冒充 AI。

## 參考設計

- https://github.com/xiaowu89/skill-matting ：主體分割、逐格處理及合成流程。
- https://github.com/lxulxu/WatermarkRemover ：固定水印區域與逐格填補流程。

目前未複製桌面程式碼。桌面 Python / CUDA 模型不能直接在瀏覽器執行，指定模型仍需 ONNX 轉換、WebGPU 運算子相容性與影像品質驗證。

圖片以解碼後的像素編輯，不保留 EXIF。復原最多 12 步。第一版圖片上限 4,000 萬像素、單邊 8192，較大圖片的記憶體使用量仍可能超出裝置限制。
