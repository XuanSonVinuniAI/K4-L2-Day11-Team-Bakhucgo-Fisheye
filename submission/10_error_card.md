# Error analysis card

## Zone × block

| zone | block | what | count |
|---|---|---|---:|
| center | B4 | DUPLICATE | 1 |
| center | B4 | IGNORE_SCOPE | 1 |
| center | B4 | MISSING | 6 |
| center | B4 | SPURIOUS | 8 |
| center | B4 | WRONG_CLASS | 2 |
| center | C0 | ATTRIBUTE | 1 |
| center | C0 | DUPLICATE | 1 |
| center | C0 | MISSING | 1 |
| center | C0 | SPURIOUS | 1 |
| edge | B4 | MISSING | 3 |
| edge | B4 | SPURIOUS | 1 |
| edge | B4 | WRONG_CLASS | 2 |
| mid | B4 | IGNORE_SCOPE | 2 |
| mid | B4 | MISSING | 4 |
| mid | B4 | SPURIOUS | 4 |
| mid | C0 | MISSING | 1 |
| mid | C0 | WRONG_CLASS | 1 |
| unknown | B4 | IGNORE_SCOPE | 1 |
| unknown | B4 | MISSING | 3 |

## Top defects
- MISSING: 18 (ví dụ frame adasind_019560.jpg)
- SPURIOUS: 14 (ví dụ frame adasind_019560.jpg)
- WRONG_CLASS: 5 (ví dụ frame adasind_019560.jpg)

## Phân tích của bạn

Hai bảng trên do `python3 lab11.py card` tính từ `findings.csv`; chạy lại lệnh sẽ cập nhật bảng và giữ nguyên mục này. Viết cho lỗi nổi bật nhất, dẫn frame/`object_ref`.

- **Nguyên nhân khả dĩ (`why`) và vì sao bạn nghĩ vậy:**
  - Lỗi nổi bật nhất là **MISSING** (18 ca) và **SPURIOUS** (14 ca), tập trung chủ yếu ở block B4 zone `center` và `mid`.
  - Với **MISSING**: Nguyên nhân xuất phát từ lỗi người gán nhãn (`E1_annotator_error`) khi vùng trung tâm có mật độ đối tượng cao, các xe và người đi bộ đứng che khuất nhau (`occluded`), dẫn đến việc người gán nhãn lướt qua các vật thể nhỏ (ví dụ ThreeWheeler R4 tại `adasind_270517.jpg` hay Pedestrian R4 tại `adasind_271039.jpg`). Ở vùng rìa mép, độ méo fisheye uốn cong vật thể làm mất đi hình dạng thông thường cũng là nguyên nhân gây sót.
  - Với **SPURIOUS**: Đa phần box thừa sinh ra từ model YOLO26m đóng băng (`E4_model_domain`, 12 box trên slice B4). Model gốc huấn luyện trên tập dữ liệu ảnh phối cảnh thông thường (rectilinear), khi đưa vào ảnh fisheye với hiện tượng méo góc cực lớn, độ tương phản ánh sáng mạnh và bóng râm ở mặt đường đã kích hoạt các detection giả. Ngoài ra ở phía người gán nhãn có 1 ca `DUPLICATE` (L7 chồng lên L9 tại `adasind_271039.jpg`).
  - Với **WRONG_CLASS** (5 ca): Hiện tượng biến dạng quang học làm xe ba bánh auto-rickshaw ở góc nghiêng bị bẹp và kéo dài, khiến annotator dễ nhầm thành ô tô con (`Car` tại L2 `adasind_271039.jpg`) hoặc nhầm xe SUV chở người thành xe tải (`Truck` tại L2 `adasind_295948.jpg`).
- **Cách sửa và ai nhận việc (`owner`):**
  - `annotator`: Thực hiện tự soát theo quy trình checklist 9 bước, zoom chi tiết các cụm vật thể đông đúc ở zone center để phát hiện vật che khuất; xóa box trùng lặp L7; sửa lại class xe ba bánh L2 và ô tô L2 trong CVAT; kiểm tra toàn bộ rìa ảnh để bổ sung box thiếu trước khi nộp.
  - `ai_team`: Cần thu thập tập dữ liệu camera fisheye để fine-tune lại backbone nhận diện đối tượng; áp dụng post-processing mask theo vòng kính (`lens_border`) và loại bỏ box kích thước bất thường ở vùng biên ảnh.
  - `guideline`: Cập nhật tài liệu minh họa phân biệt ThreeWheeler và Car dưới các góc méo fisheye điển hình.
- **Bằng chứng (ảnh trong `screenshots/`, dòng findings, rule):**
  - Ảnh minh họa: `submission/screenshots/review_overlay.png` và `submission/screenshots/distortion_case.png` cho thấy rõ biến dạng và vùng nhãn trùng lặp.
  - Dòng findings: `r1_craft,B4-center,adasind_271039.jpg,L7` (DUPLICATE R01), `r1_craft,B4-center,adasind_271039.jpg,L2+R9` (WRONG_CLASS R04), `r1_craft,B4-center,adasind_270517.jpg,R4` (MISSING R01).
  - Quy tắc: Tuân thủ R01 (ngưỡng H=40 và không vẽ trùng), R04 (phân loại xe đặc thù: auto-rickshaw là ThreeWheeler, SUV là Car), R06 & R08 (quản lý vùng ignore và bảo toàn lens_border).
