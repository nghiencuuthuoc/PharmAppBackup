# Hướng dẫn sử dụng PharmApp Backup Tool (GUI) v1.3

PharmApp Backup Tool (GUI) là ứng dụng sao lưu thư mục theo cơ chế **incremental** (chỉ copy file mới hoặc file cập nhật), hỗ trợ **Profiles** (hồ sơ cấu hình) và nhiều **Jobs** (tuyến sao lưu Source → Target) trong mỗi profile. Ứng dụng phù hợp cho sao lưu dữ liệu PharmSolu/QA/R&D lên ổ cứng nội bộ hoặc ổ cứng rời, có **log + báo cáo** rõ ràng theo từng lần chạy.

---

## Tải về và chạy chương trình

Bạn có thể tải file EXE tại GitHub (dán vào trình duyệt hoặc gắn link trên website của bạn):

```text
https://github.com/nghiencuuthuoc/PharmAppBackup/blob/main/PharmAppBackup_v1.3.exe
```

Lưu ý khi tải từ GitHub:

* Nếu trình duyệt mở trang xem file trên GitHub, hãy chọn **Download** (hoặc **Raw/Download raw file** tùy giao diện GitHub) để tải về.
* Trên Windows có thể xuất hiện cảnh báo SmartScreen/Defender cho file EXE mới phát hành. Hãy kiểm tra đúng nguồn tải và bấm **More info → Run anyway** nếu bạn tin tưởng nguồn.

Chạy chương trình:

1. Tải `PharmAppBackup_v1.3.exe`
2. Double-click để chạy (không cần cài đặt)

---

## Tổng quan giao diện (như hình bạn đã chạy thử)

Giao diện gồm các khối chính:

1. **Profiles** (dòng trên cùng): chọn profile, tạo mới, lưu, xóa, export/import
2. **Jobs (Source → Target)**: nhập Source, Target, Note; quản lý danh sách jobs
3. **Profile default settings**: cấu hình mặc định cho profile (interval, exclude, verify)
4. **Profile default file groups**: chọn nhóm file (All groups hoặc chọn chi tiết)
5. **Action buttons**: Start/Stop Auto Backup, Run Selected Job, Run All Enabled Jobs
6. **Log**: hiển thị nhật ký chạy theo thời gian thực
7. **Footer donate**: liên kết website, Zalo và nút Donate

---

## Khái niệm quan trọng

### 1) Profile là gì?

**Profile** là “bộ cấu hình” để bạn lưu nhiều tuyến sao lưu theo từng mục đích, ví dụ:

* “QC to D” (QC data → ổ D)
* “COA to NAS”
* “PharmSolu backup weekly”

Mỗi profile có:

* Nhiều **Jobs**
* Cấu hình mặc định: interval, groups, exclude, verify…

### 2) Job là gì?

**Job** là 1 tuyến sao lưu:

* **Source**: thư mục nguồn
* **Target**: thư mục đích
* **Enabled**: bật/tắt job
* **Note**: ghi chú

### 3) Override là gì?

**Override (Yes/No)** cho phép mỗi Job có cấu hình riêng (khác với cấu hình mặc định của profile):

* Groups: All groups hay chọn nhóm cụ thể
* Exclude dirs / exclude patterns
* Verify sau copy

Khi **Override = Yes**: job dùng cấu hình riêng của job.
Khi **Override = No**: job dùng cấu hình mặc định của profile.

---

## Thiết lập Profile (mặc định)

### Interval (minutes)

* Dùng cho chế độ **Auto Backup**
* Ví dụ: `60` nghĩa là cứ mỗi 60 phút chạy 1 vòng các job đang Enabled

### Verify (size) after copy

* Khi bật, sau khi copy sẽ kiểm tra nhanh theo **kích thước file** (size check).
* Khuyến nghị bật khi bạn sao lưu dữ liệu quan trọng; có thể làm chậm nhẹ với lượng file lớn.

### Exclude dirs (comma)

Danh sách thư mục con sẽ bỏ qua, cách nhau bằng dấu phẩy. Mặc định thường gồm:

* `.git, __pycache__, node_modules, venv, .idea, .vscode, ...`

### Exclude patterns (comma)

Lọc file theo mẫu (glob), ví dụ:

* `~$*.docx, ~$*.xlsx, *.tmp, *.part, *.swp, *.bak`

---

## Thiết lập nhóm file (File groups)

Bạn có thể chọn:

* **All groups**: sao lưu tất cả loại file
* Hoặc bỏ chọn All groups để bật từng nhóm:

  * Documents, Archives, Audio, Video, Pictures
  * **Graphics (Corel/Adobe)**: PSD/AI/CDR/INDD… (phù hợp nhóm thiết kế)
  * **Stats (Minitab/SAS/JMP)**: MTW/MPJ/SAS/JMP… (phù hợp nhóm thống kê)
  * Code, Others

Khuyến nghị vận hành:

* Profile “Full backup”: All groups
* Profile “QC/Docs”: chỉ Documents + Archives
* Profile “Design”: Pictures + Graphics
* Profile “Stats”: Stats + Documents

---

## Quản lý Jobs (Source → Target)

### 1) Thêm Job

1. Điền **Source** (hoặc Browse)
2. Điền **Target** (hoặc Browse)
3. Điền **Note** (khuyến nghị)
4. Bấm **Add Job**

### 2) Sửa Job

1. Click chọn job trong bảng
2. Chỉnh Source/Target/Note
3. Bấm **Update Job**

### 3) Xóa Job

* Chọn job → **Remove Job**

### 4) Bật/tắt Job

* Chọn job → **Toggle Enabled**
* Enabled = No: job sẽ không chạy trong Run All / Auto

---

## Chỉnh Override theo từng Job

1. Chọn 1 job trong bảng
2. Bấm **Edit Override…**
3. Bật **Enable override for this job**
4. Chỉnh các mục:

   * All groups hoặc chọn group cụ thể
   * Exclude dirs / Exclude patterns
   * Verify
5. Save

Khuyến nghị:

* Chỉ dùng override khi bạn cần “ngoại lệ” cho một job (ví dụ job này chỉ copy Documents, job khác copy All groups).

---

## Chạy backup thủ công

### Run Selected Job

* Chạy đúng **1 job** đang chọn
* Phù hợp test nhanh hoặc xử lý tình huống

### Run All Enabled Jobs

* Chạy **tuần tự** tất cả jobs có Enabled = Yes trong profile
* Phù hợp chạy thủ công cuối ngày/đầu tuần

---

## Auto Backup (chạy định kỳ)

### Start Auto Backup

* Chạy vòng lặp: cứ mỗi `Interval (minutes)` chạy lại toàn bộ Enabled jobs
* Jobs chạy tuần tự để giảm rủi ro “đè I/O” lên ổ đĩa

### Stop Auto Backup

* Dừng vòng lặp (có thể chờ job đang chạy kết thúc tùy thời điểm bấm)

Khuyến nghị khi dùng ổ cứng rời:

* Cắm ổ đích trước khi Start Auto
* Nếu ổ đích tạm mất kết nối, app sẽ chờ ổ đích xuất hiện lại (tùy cấu hình/logic chờ), sau đó tiếp tục.

---

## Log & báo cáo (rất quan trọng)

Mỗi lần chạy **mỗi job** sẽ tạo bộ artifact trong **Target folder**:

1. **Log theo phiên**
   `backup_YYYYMMDD_HHMMSS.log`

   * Ghi chi tiết file nào “new/updated/skip/error”

2. **Summary JSON**
   `backup_summary_YYYYMMDD_HHMMSS.json`

   * Thông tin tổng hợp: số file scan/copy/skip/errors, thời gian chạy, source/target, profile, note…

3. **History CSV (append)**
   `backup_history.csv`

   * Tự động append mỗi lần job chạy → dễ mở Excel để thống kê theo thời gian

---

## Lưu và chia sẻ Profiles

* **Save**: lưu profile hiện tại
* **Export**: xuất toàn bộ profiles ra JSON để backup hoặc chuyển máy
* **Import**: nhập profiles từ JSON

Vị trí lưu mặc định (để EXE one-file chạy ổn, không phụ thuộc thư mục cài đặt):

* Windows (thường):
  `%APPDATA%\PharmAppBackup\backup_profiles.json`
  hoặc `%LOCALAPPDATA%\PharmAppBackup\backup_profiles.json` (tùy máy)

---

## Các quy tắc an toàn (để tránh lỗi nghiêm trọng)

Ứng dụng có chặn cấu hình nguy hiểm:

* **Target trùng Source**
* **Target nằm bên trong Source** (dễ gây backup đệ quy vô hạn)

Khuyến nghị thêm:

* Không đặt Target vào các thư mục hệ thống nhạy cảm
* Không dùng Target là chính thư mục đang sync cloud nếu bạn chưa kiểm soát (OneDrive/Google Drive…) vì có thể tạo xung đột file khi đồng bộ.

---

## Troubleshooting nhanh

**1) Không thấy file log trong Target**

* Log chỉ tạo khi bạn bấm Run/Start và job bắt đầu chạy.
* Kiểm tra bạn có quyền ghi vào Target không.

**2) File bị “locked/permission”**

* Ứng dụng có retry + atomic copy, nhưng nếu file đang mở bởi chương trình khác, vẫn có thể lỗi.
* Đóng file đang mở hoặc chạy lại job.

**3) Target là ổ cứng rời**

* Cắm lại ổ, đảm bảo đúng ký tự ổ (D/E/F…).
* Kiểm tra quyền ghi và lỗi filesystem.

---
