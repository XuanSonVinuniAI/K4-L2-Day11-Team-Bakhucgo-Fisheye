# Kế hoạch review từ lỗi quan sát được

Từ `findings.csv` và `zone_table.md`, chọn **hai lát cắt của bài ADASIND một camera** cần review trước. Bảng này
giải thích dữ liệu thật bạn vừa làm; nó không thay cho kế hoạch bốn camera giả lập ở `45_sampling_plan.csv`.

| Lát cắt / frame | Số ca và loại lỗi | Vì sao review trước | Bằng chứng cần giữ |
|---|---|---|---|
| adasind_271039.jpg (Mật độ giao thông cao, cụm người đi bộ và méo rìa) | 4 ca: 1 WRONG_CLASS (ThreeWheeler thành Car L2), 1 DUPLICATE (L7 đè lên L9), 2 MISSING (người đi bộ sát mép phải và ThreeWheeler nhỏ) | Vùng trung tâm có nhiều đối tượng đứng che khuất lẫn nhau khiến annotator dễ bỏ sót hoặc vẽ trùng; đồng thời méo biên ảnh làm thay đổi hình dáng xe ba bánh dễ dẫn đến nhầm class. | Báo cáo `r3_diag/local_quality_conflicts.csv`, `qa_overlay.html`, ảnh `submission/screenshots/review_overlay.png` và các dòng findings L2, L7, R4. |
| adasind_295948.jpg (Giao thoa thân xe ego_body và vật thể tiền cảnh) | 3 ca: 1 IGNORE_SCOPE (thiếu lens_border và nhầm thuộc tính ego_body), 1 WRONG_CLASS (SUV thành Truck L2), 1 MISSING (xe hai bánh lớn tiền cảnh) | Khu vực tiếp giáp giữa nắp capo xe ego (`ego_body`) và vành kính (`lens_border`) có nguy cơ sai lệch phạm vi ignore cao nhất; vật thể lớn ở tiền cảnh quyết định trực tiếp khoảng cách phanh khẩn cấp. | Ảnh `submission/screenshots/distortion_case.png`, ticket `30_escalation_ticket.md`, và các dòng findings ignore_region, L2, R3. |

**Giới hạn của kết luận từ ba frame ADASIND:** Dữ liệu chỉ gồm 3 frame tĩnh trích xuất từ 1 camera fisheye đơn phía trước xe. Bộ dữ liệu không có thông số nội/ngoại suy (intrinsic/extrinsic), không có chuỗi frame liên tục theo thời gian để kiểm tra tracking, và không phản ánh được vùng mù cũng như vùng giao thoa (seam) giữa 4 góc nhìn của hệ thống SVM hoàn chỉnh.

## Chuyển sang kế hoạch bốn camera giả lập

- **Cách soát độ phủ của 200 frame ở `45_sampling_plan.csv`:** Phân bổ ngân sách 200 frame cân đối qua 4 camera (front, rear, left, right) với tỷ lệ phân tầng giữa ca thường (normal) và ca khó (hard). Để tránh tình trạng chọn các frame liên tiếp trong cùng một cảnh (dẫn đến trùng lặp dữ liệu và sai lệch thống kê), cần thiết lập quy tắc trích xuất cách quãng (stride tối thiểu 30–50 frame giữa 2 mẫu trong cùng clip) hoặc lấy mẫu theo các kịch bản môi trường phân tán (nắng gắt, ngược sáng, trời mưa, ban đêm, đường đông đúc).
- **Vì sao kế hoạch chỉ giúp tìm ca cần soi, chưa đo được tỷ lệ lỗi:** Phương pháp lấy mẫu phân tầng ca khó (adversarial/hard-negative sampling) tập trung ưu tiên tìm kiếm các kịch bản rủi ro cao ở vùng seam, vùng rìa méo và các tình huống dễ gây lỗi mô hình. Do đây không phải là phép lấy mẫu ngẫu nhiên độc lập (I.I.D) từ toàn bộ 50.000 frame, nên tỷ lệ lỗi đo được trên 200 frame này chỉ đóng vai trò chẩn đoán chất lượng biên, không thể ngoại suy thành tỷ lệ lỗi trung bình (defect rate) của toàn hệ thống sản xuất.
