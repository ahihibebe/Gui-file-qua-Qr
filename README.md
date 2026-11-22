# 📤 QR Send

Ứng dụng Android **gửi và nhận file qua mã QR** bằng Wi-Fi. Không cần internet, không giới hạn dung lượng, không cần cài app bên máy kia — chỉ cần trình duyệt.

## ✨ Tính năng

### 📤 Gửi file
- Chọn một hoặc nhiều file (mọi loại), bấm **Bắt đầu chia sẻ**.
- App hiện mã QR. Máy khác cùng Wi-Fi quét QR, mở trang danh sách file và bấm **Tải về**.
- Hiển thị số lượt tải, tốc độ, thanh tiến độ từng file.
- Đường dẫn có mã ngẫu nhiên 8 ký tự nên người khác trong mạng không đoán được.

### 📥 Nhận file
- Máy khác quét QR, chọn (hoặc kéo thả) file rồi gửi sang điện thoại, có thanh tiến độ.
- **Tuỳ chọn nơi lưu file**: bấm *Chọn thư mục* hoặc dùng mặc định `Download/QRSend`.
- Lưu ý: Android 11+ không cho chọn thẳng thư mục Download gốc, hãy chọn/tạo thư mục con.

### 🍿 Ủng hộ
- Tab *Gói Bim Bim* với mã QR ngân hàng MB Bank và nút sao chép số tài khoản.

## ⚙️ Kỹ thuật

| Yêu cầu | Cách làm |
|---|---|
| Không nạp cả file vào RAM | Truyền/ghi từng mảnh 256KB trực tiếp qua socket |
| Tải tiếp khi đứt mạng | Hỗ trợ HTTP `Range` / `If-Range` (206 Partial Content) |
| Chạy ngầm ổn định | Foreground Service + thông báo + `WakeLock` + `WifiLock` |
| Nhận file | HTTP `PUT` ghi thẳng ra bộ nhớ (SAF / MediaStore) |

- Ngôn ngữ: Kotlin, giao diện dựng bằng code (không dùng AndroidX).
- Thư viện duy nhất: [ZXing core](https://github.com/zxing/zxing) để tạo mã QR.
- Yêu cầu: Android 8.0 (API 26) trở lên.

## 🔨 Build APK bằng GitHub Actions

1. Tạo file `.github/workflows/build-apk.yml` trong repo (dùng file `build-apk.yml` đi kèm).
2. Upload `QRSend.zip` (hoặc toàn bộ mã nguồn đã giải nén) lên thư mục gốc của repo.
3. Vào tab **Actions** → **Build APK** → **Run workflow**.
4. Chạy xong, file APK release có ở **hai nơi**:
   - Mục **Releases** của repo (bên phải trang chính): tải trực tiếp `QRSend-vX.Y.Z.apk`.
   - Mục **Artifacts** của lần chạy: `QRSend-APK` (dạng zip).

APK được ký bằng keystore cố định `app/qrsend.keystore` nên các bản sau **cài đè** được lên bản trước.

## 📱 Cách dùng nhanh

1. Kết nối điện thoại vào Wi-Fi (hoặc bật điểm phát sóng cho máy kia kết nối).
2. Chọn tab **Gửi file** hoặc **Nhận file** rồi bấm bắt đầu.
3. Dùng máy kia quét mã QR bằng camera và làm theo trang web hiện ra.
4. Xong thì bấm **Dừng** (hoặc nút *Dừng chia sẻ* ở thông báo).

> Một lúc chỉ chạy một chế độ (gửi hoặc nhận).

## 🖼️ Đổi mã QR donate

Thay ảnh `app/src/main/res/drawable-nodpi/donate_qr.png` bằng ảnh QR của bạn rồi build lại.

## 🗂️ Cấu trúc

```
QRSend/
├── .github/workflows/build-apk.yml
├── app/
│   ├── qrsend.keystore
│   └── src/main/
│       ├── AndroidManifest.xml
│       ├── java/vn/qrsend/app/
│       │   ├── MainActivity.kt     # giao diện 3 tab
│       │   ├── HttpFileServer.kt   # server HTTP, streaming, Range, upload
│       │   ├── ShareService.kt     # Foreground Service + khoá giữ máy thức
│       │   ├── Receive.kt          # nơi lưu file nhận về
│       │   ├── ShareState.kt       # trạng thái dùng chung
│       │   └── Util.kt             # QR, mạng, định dạng
│       └── res/
└── build.gradle.kts
```

## 📄 Phiên bản

- **1.1.0**: thêm tab Nhận file và tuỳ chọn nơi lưu; dùng ảnh QR donate riêng.
- **1.0.0**: gửi file qua QR, streaming, Range, Foreground Service, tab ủng hộ.
