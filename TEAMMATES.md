# Thành viên và phân vai — Day11 SVM 360 Fisheye

## 1. Thông tin nhóm

- Khóa/lớp: Khóa 4, lớp 2
- Tên nhóm: K4-L2-Day11-Team-Bakhucgo-Fisheye
- Repo Public: https://github.com/XuanSonVinuniAI/K4-L2-Day11-Team-Bakhucgo-Fisheye
- Máy giữ hồ sơ chính / người quản lý: Nguyễn Xuân Sơn
- Slice chung lấy từ mode.json: Không dùng slice chung ở P0; cả ba thành viên làm P0 trên hồ sơ cá nhân. P2 của Trương Văn Vượng dùng slice `B4-center`.
- Tên định danh vai A dùng cho --self: `vuong`.
- Kênh trao đổi nội bộ: Qua repo
- Đại diện nộp (vai C): Trương Đức Thành, 2A202602179
- Commit chốt bài: Chưa có commit chốt. HEAD hiện tại là f539e61b1988357b29aea77505683a5c85b3337d (commit đồng bộ project, không xác nhận là commit nộp cuối).

## 2. Ba vai chính

| Vai | Họ và tên | MSSV | Tên định danh trong mode | Trách nhiệm | Bằng chứng đóng góp |
|---|---|---|---|---|---|
| A · Gán nhãn | Trương Văn Vượng | Chưa rõ | vuong | Parking/C0/slice, self-QC, lock, rework |  |
| B · QA độc lập | Nguyễn Xuân Sơn | 2A202602155 | son | Review trước reference, finding QA, kiểm lại ca sửa | `submission/r2_qa/qa_review.md`, `qa_overlay.html`; QA B4-center có 7 nhận xét |
| C · Chẩn đoán & điều phối | Trương Đức Thành | 2A202602179 | thanh | Báo cáo, phân xử, kế hoạch, tích hợp, check và nộp |  |

Bảng này xác định vai của nhóm. Vòng QA tự sinh trong team.json thuộc quy trình nhiều hồ sơ của CLI; nhóm dùng một slice chung và quy trình A → B → C đã nêu trong hướng dẫn.

Thông tin cấu hình đã xác nhận: mode.json liệt kê các định danh `son`, `thanh`, `vuong`; phân công cá nhân ghi lần lượt là `B2-edge`, `B4-dense`, `B4-center`. Theo xác nhận của nhóm, `son` là Nguyễn Xuân Sơn (2A202602155, vai B), `thanh` là Trương Đức Thành (2A202602179, vai C), và `vuong` là Trương Văn Vượng (MSSV chưa rõ, vai A). P0 mỗi người thực hiện trên hồ sơ cá nhân; P2 của Vượng là `B4-center`. team.json ghi vòng QA `son → thanh`, `thanh → vuong`, `vuong → son` cho việc bàn giao review.

## 3. Bàn giao theo pha

| Mốc | Người giao → nhận | File / commit / mã khóa | Người nhận đã kiểm gì? | Trạng thái / vướng mắc |
|---|---|---|---|---|
| P0 · Chốt môi trường và vai | Mỗi thành viên tự làm trên hồ sơ cá nhân | Mỗi repo cá nhân: `submission/00_setup/`, `parking/annotations.xml`, `parking/observations.md` | Nhóm xác nhận cả ba người đã làm P0 riêng | Không có một slice chung ở P0; mỗi thành viên giữ hồ sơ cá nhân. |
| P2 · Khóa bản đầu | Trương Văn Vượng  → Nguyễn Xuân Sơn và Trương Đức Thành | Slice `B4-center`; `submission/r1_craft/annotations.xml`, `selfqc.md`, `lock.txt`; code CECC-18CC | Self-QC đánh dấu đủ 9 mục; | Bản khóa lúc 2026-09-28 20:32 +07; K12 được ghi là degrade/chưa vẽ polygon. |
| P3 · Chốt QA mù | Nguyễn Xuân Sơn (T022, vai B) → vai C/A | `submission/r2_qa/qa_review.md`, `qa_overlay.html`; mã khóa CECC-18CC; `findings.csv` | QA review có 7 nhận xét theo R01/R04/R08; | Có QA record cho B4-center. qa_review.md có 7 nhận xét, trong khi findings.csv hiện có 4 dòng r2_qa |
| P4 · Quyết định sửa | Trương Đức Thành (T024, vai C) → A/B | Findings, decision log và báo cáo chẩn đoán |
| P5 · Kiểm bản sửa | Trương Văn Vượng → Nguyễn Xuân Sơn → Trương Đức Thành | Bản rework, lock2, review kiểm lại, delta |
| P6 · Chốt nộp | Nguyễn Xuân Sơn, Trương Văn Vượng → Trương Đức Thành | Manifest, commit chốt, link repo |
## 4. Bất đồng và phối hợp

- Một ca đã phân xử: Chưa có bất đồng A/B nào được ghi nhận là đã phân xử. Tự soát parking ghi đã giảm từ 23 xuống 14 polyline, loại các nét thuộc lối xe chạy và nét trùng; bằng chứng ở `submission/parking/observations.md`.
- Ca còn mở: QA B4-center ghi 7 nhận xét, gồm missing ThreeWheeler, sai class ThreeWheeler/Car và Truck/Car, duplicate Pedestrian, thiếu Pedestrian và Bike, cùng `lens_border` thiếu/sai reason. Chờ đối chiếu ảnh gốc theo rule và cập nhật đủ findings/decision log; chưa có owner cá nhân được xác nhận.

## 5. Xác nhận trước khi nộp

- [x] A xác nhận nhãn và export đúng phiên bản: Trương Văn Vượng; P2 slice B4-center, lock code CECC-18CC.
- [x] B xác nhận đã QA độc lập trước reference và kiểm lại ca sửa: Nguyễn Xuân Sơn; QA review B4-center có trong hồ sơ; nhóm xác nhận kiểm lại sau rework.
- [x] C xác nhận báo cáo đúng bản khóa, các file đầy đủ và check exit 0: Trương Đức Thành xác nhận hoàn tất P6.
- [x] manifest.json tại commit chốt có failed_gates rỗng: Đã xác nhận hoàn tất P6 theo thông tin nhóm; manifest trong working tree hiện tại là bản cũ và cần đồng bộ.
- [x] Repo nhóm Public, ảnh và các bằng chứng mở được: Đã xác nhận hoàn tất P6 theo thông tin nhóm.
- [x] C đã push và gửi link repo nhóm + commit qua kênh lớp công bố: Đã xác nhận hoàn tất P6; SHA cuối cần cập nhật khi đồng bộ commit mới vào working tree.

Checklist P5/P6 được tích theo xác nhận của nhóm.
