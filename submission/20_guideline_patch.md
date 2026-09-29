# Guideline patch

- **Rule mới đề xuất:** R04-B — Quy tắc nhận diện phương tiện giao thông bị méo quang học trên ảnh fisheye:
  1. Với xe ba bánh chở khách (auto-rickshaw / e-rickshaw) nằm ở vùng bán kính trung gian (`mid`) và vùng rìa (`edge`): Nếu nhìn thấy rõ bánh trước đơn, tay lái hoặc kết cấu mui vải vát chéo, bắt buộc gán nhãn `ThreeWheeler`, không được gán nhãn `Car` hay `Bus` dù góc nghiêng làm thân xe trông giống ô tô con.
  2. Với các dòng xe thể thao đa dụng (SUV), xe crossover hoặc van chở người: Bắt buộc gán nhãn `Car` theo R04, cấm gán nhãn `Truck` trừ khi xe có thùng chở hàng tách rời lộ thiên phía sau.
  3. Bổ sung hướng dẫn xử lý đối tượng bị cắt bởi nắp capo xe ego: Nếu vật thể bị che >50% bởi thân xe ego (`ego_body`), coi là ignore scope theo R09; nếu che <50%, vẽ box ôm trọn phần nhìn thấy ngoài phạm vi polygon thân xe.
- **Áp dụng cho:** Class `ThreeWheeler`, `Car`, `Truck`, và polygon `ignore_region` (reason `ego_body`).
- **Vì sao luật hiện tại (`docs/02-rules-vi.md`) không đủ:** Luật R04 phiên bản 1.0.0 chỉ liệt kê tên phương tiện tương ứng nhưng chưa định lượng dấu hiệu nhận biết khi bị biến dạng phối cảnh fisheye (đặc biệt khi quan sát từ một camera fisheye đơn với góc nhìn chếch xuống). Điều này dẫn đến sự không thống nhất giữa annotator và QA (ví dụ ca L2 ở frame `adasind_271039.jpg` và `adasind_295948.jpg`).
- **`rules_version` mới:** v1.1.0 (nâng cấp từ v1.0.0).
- **Hiệu lực từ:** Vòng chẩn đoán và sửa đổi (P4 / P5 rework).
