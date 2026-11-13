# 解壓口袋 iOS 原生版

此交付內容是 SwiftUI / Xcode 原始碼專案，**不是已簽章的 IPA，不能直接在 iPhone 安裝**。目前尚未完成 Apple 平台編譯或實機驗證。

## 壓縮功能（1.1.0）

新增多選檔案、整個資料夾建立 ZIP、自訂名稱、中文路徑、同名不覆蓋、取消、儲存及分享。資料以 64 KB 分段讀寫，來源不會被刪除。照片與影片可能無法顯著縮小。

## 已實作

- 從「檔案」或其他 App 開啟未加密 ZIP。
- 64 KB 分段解壓、直接寫入磁碟，沒有原網頁版的 80 MB / 200 MB 限制。
- 依宣告的解壓容量預先檢查可用空間；寫入失敗會清除暫存，不顯示未完成結果。
- CRC32 與檔案長度檢查，拒絕越界路徑、符號連結與大小寫重複路徑。
- 保留資料夾、中文檔名、相同 ZIP 再次匯入使用不同資料夾。
- 解壓進度與取消、Quick Look 檔案預覽、單檔分享、整個資料夾匯出。
- 「檔案 → 我的 iPhone → 解壓口袋 → 解壓檔案」持久儲存。
- 無登入、無伺服器、無瀏覽器。未加密 ZIP 支援 Store / Deflate；RAR、7Z、密碼 ZIP 未實作。
- iOS 16 以上，iPhone / iPad。iOS 背景執行時間有限，大檔解壓請保持 App 前景開啟。

## 開啟與安裝

1. Mac 安裝 Xcode 15 以上，開啟 `PocketUnzip.xcodeproj`。
2. 等待 Swift Package Manager 解析 ZIPFoundation 0.9.20。
3. 選取 PocketUnzip target，在 Signing & Capabilities 選擇你的 Apple 開發團隊。
4. 確認 Bundle ID `com.grost9999.pocketunzip` 可用，必要時改為你帳號下的唯一識別碼。
5. 連接 iPhone，選取裝置並執行；或使用 Product → Archive，上傳 App Store Connect / TestFlight。

TestFlight 需可用的 Apple Developer 會員、對應 App Store Connect App、簽章與佈建設定。本包未包含任何憑證或金鑰，也未替你建立 Apple App 記錄。

## 驗證

已附 XCTest：201 MiB 解壓、中文資料夾、同名 ZIP 不覆蓋、路徑穿越、CRC 損壞、取消清理。GitHub Actions workflow 可在 macOS 上執行測試並產生未簽章裝置 archive；未簽章 archive 不能直接安裝。

目前製作環境為 Linux，無 Swift / Xcode：僅檢查專案結構、plist、fixture 與測試配置；上述 XCTest 尚未實際執行。第一次 macOS 建置通過之前不應視為可安裝版。未設定自動 TestFlight 上傳，以免誤用其他 App 的簽章與帳號。

## 依賴

ZIPFoundation 0.9.20（MIT）：https://github.com/weichsel/ZIPFoundation/tree/0.9.20 。透過 Swift Package Manager 取得；請保留依賴套件原授權。
