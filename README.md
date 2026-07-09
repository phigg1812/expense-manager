# Sổ theo dõi — Android App (Capacitor)

Đóng gói web app `www/index.html` thành app Android thật (.apk), chạy full màn hình,
có icon riêng, splash screen — không cần cài Android Studio để build, GitHub Actions
build APK giúp bạn hoàn toàn miễn phí.

## Cấu trúc project

```
expense-manager-app/
├── www/                      # Web app của bạn (đã copy từ bản PWA)
│   ├── index.html
│   ├── manifest.json
│   └── icon-*.png
├── assets/                   # Ảnh gốc để sinh icon app + splash screen
│   ├── icon.png
│   ├── icon-foreground.png
│   ├── icon-background.png
│   └── splash.png
├── capacitor.config.json     # Cấu hình tên app, package id, splash/status bar
├── package.json
└── .github/workflows/build-apk.yml   # Workflow build APK tự động
```

`android/` KHÔNG được commit — mỗi lần chạy, GitHub Actions tự tạo lại project Android
từ đầu (`npx cap add android`) rồi build, nên bạn không cần lo đồng bộ thủ công.

## Cách lấy file APK

1. Push toàn bộ project này lên nhánh `main` của repo
   `https://github.com/phigg1812/expense-manager` (ghi đè/thêm vào repo hiện có).
2. Vào tab **Actions** trên GitHub → chờ workflow **Build Android APK** chạy xong
   (khoảng 3-5 phút).
3. Mở lần chạy vừa xong → mục **Artifacts** ở cuối trang → tải
   **expense-manager-debug-apk** (file .zip chứa `app-debug.apk` bên trong).
4. Chuyển file `.apk` vào điện thoại Android (qua USB, Zalo, Google Drive...) → mở lên
   để cài. Android sẽ hỏi "Cho phép cài từ nguồn không xác định" → bấm Cho phép.

Cũng có thể tự chạy tay bất cứ lúc nào: vào tab **Actions** → chọn workflow → nút
**Run workflow**.

## Khi bạn sửa app (index.html)

- Cập nhật file trong `www/index.html`, commit & push — workflow tự chạy lại và ra
  bản APK mới, không cần làm lại các bước cấu hình.

## Đổi tên app / package id

Sửa trong `capacitor.config.json`:
- `"appName"`: tên hiển thị dưới icon trên điện thoại.
- `"appId"`: định danh gói app (kiểu `com.tencongty.tenapp`), chỉ nên đổi **trước khi
  phát hành lần đầu** — đổi sau này sẽ khiến app mới bị coi là app khác, không update
  được app cũ.

## Nếu muốn phát hành lên Google Play (bản release, có chữ ký)

Bản build hiện tại là **debug APK** — cài trực tiếp được nhưng chưa có chữ ký phát hành
chính thức, không nộp lên Google Play được. Nếu bạn cần bản release/ký để đăng Play
Store, báo lại — mình sẽ thêm bước tạo keystore + ký APK (`assembleRelease`) vào
workflow.
