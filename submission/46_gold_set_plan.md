# Đề xuất gold set theo camera — tình huống giả lập

**Đầu bài:** 50.000 frame từ bốn camera SVM, ngân sách chọn 200 frame để review/gold. Đây là tình huống trên slide,
**không phải** 50.000 frame có trong repo. Phân bổ đúng 200 ở `45_sampling_plan.csv` cho bốn camera, mỗi camera có
normal và hard slice. “Gold set” ở đây là **kế hoạch tạo** reference sau kiểm chứng, không phải teaching reference
ADASIND hoặc nhãn bạn vừa vẽ. Nếu cần, dùng `notebooks/day11-svm360-colab.ipynb` để thử tổng phân bổ; notebook
không làm thay phần lý do.

| camera_id | Hard case cần chọn | Vì sao dễ sai | Annotation space / calibration cần giữ | Cách review trước khi gọi là gold |
|---|---|---|---|---|
| front | Đèn pha rọi ngược chiều ban đêm, trời mưa ướt mặt đường phản quang, cụm xe hai bánh đan xen ở ngã tư. | Méo quang học tâm thấu kính kết hợp độ lóa sáng che mờ đường biên; các đối tượng occluded đứng sát nhau dễ bị gộp nhầm thành 1 box. | Giữ nguyên ảnh raw fisheye 1080×1920 px, lưu trữ ma trận nội suy intrinsic và hệ số méo thấu kính Kannala-Brandt; không nắn phẳng ảnh trước khi gán nhãn. | 3 annotator gán nhãn độc lập (triple-blind), chạy thuật toán đối chiếu IoU ≥ 0.7 và họp trọng tài (arbitration) với Lead Data Ops giải quyết 100% ca bất đồng. |
| rear | Phương tiện bám đuôi siêu gần (< 1.5m), người đi bộ cúi nhặt đồ sát cản sau, gờ giảm tốc cao tạo rung lắc góc camera. | Góc nhìn dốc xuống khiến phần trên của xe sau bị cắt (truncated) mạnh; bóng xe ego đè lên chướng ngại vật; rung lắc làm trôi góc extrinsic. | Không gian fisheye gốc, gắn cờ góc roll/pitch của camera đuôi, bảo toàn polygon `ego_body` cốp sau xe. | Kiểm tra chéo giữa camera sau và cảm biến siêu âm (USS) khoảng cách gần; thẩm định bởi chuyên gia an toàn ADAS trước khi đóng băng ground truth. |
| left | Xe máy lách qua khe hẹp bên trái khi xe rẽ, đối tượng di chuyển nhanh từ vùng rìa vào vùng đường may nối (seam trước-trái). | Méo hình cầu cực đại ở rìa ảnh kéo dẹt box; tốc độ tương đối cao gây mờ chuyển động (motion blur); hiện tượng vật thể xuất hiện trên cả 2 camera liền kề. | Hệ tọa độ fisheye trái kèm ma trận ngoại suy extrinsic liên kết với camera trước; giữ thông số đồng bộ thời gian (hardware sync timestamp). | Soát đồng thời 2 khung nhìn camera trước và camera trái trên cùng timestamp; đảm bảo tính liên tục của đối tượng và đồng nhất class/track ID. |
| right | Người đi bộ và xe đạp bước xuống từ vỉa hè cao bên phụ, chướng ngại vật tĩnh (thùng rác, cọc tiêu) khuất một phần dưới gương. | Điểm mù gương chiếu hậu; ánh sáng chênh lệch giữa lòng đường và bóng cây vỉa hè; nhầm lẫn class người đi bộ với xe máy dắt bộ. | Giữ nguyên tỷ lệ pixel không nén; lưu trữ metadata độ cao lắp đặt camera gương phụ và góc nghiêng chúc xuống. | Dual-review độc lập giữa 2 QA cấp cao; xác minh lại bằng video sequence liền trước và liền sau 10 frame để khẳng định chính xác class. |

- **Khi nào cần refresh gold set (đổi camera, calibration hoặc rule):**
  1. Khi thay đổi phần cứng camera: nâng cấp cảm biến, thay đổi ống kính fisheye, thay đổi góc FOV hoặc dịch chuyển vị trí lắp đặt trên thân xe.
  2. Khi hiệu chuẩn lại hệ số nội/ngoại suy (re-calibration): thay đổi ma trận intrinsic/extrinsic dẫn đến bản đồ méo và ánh xạ Bird's Eye View (BEV) thay đổi.
  3. Khi có phiên bản cập nhật guideline gán nhãn (`rules_version` bump, ví dụ v1.0.0 lên v1.1.0): mở rộng định nghĩa class, thay đổi ngưỡng chiều cao H=40, hoặc cập nhật chính sách phân tách rider/bike và vùng ignore.
  4. Theo định kỳ kiểm định chất lượng dữ liệu: phát hiện hiện tượng trôi dữ liệu (data drift) theo mùa thời tiết hoặc khu vực địa lý mới.

- **Một ca seam/cross-camera cần policy và evidence trước khi ghép hai box:**
  - *Tình huống:* Một phương tiện hoặc người đi bộ di chuyển qua góc xe (ví dụ góc trước-trái), xuất hiện đồng thời trên cả camera Trước (`front`) và camera Trái (`left`) tại cùng một thời điểm timestamp.
  - *Policy:* Không được tự động coi sự xuất hiện ở cả hai camera là lỗi trùng lặp box (DUPLICATE). Trong không gian 2D của từng camera fisheye, mỗi camera phải vẽ một box độc lập ôm trọn phần nhìn thấy của vật trên camera đó. Khi ghép vào không gian 3D/BEV toàn cảnh 360°, hệ thống chỉ hợp nhất thành 1 vật thể duy nhất nếu có đủ bằng chứng.
  - *Evidence bắt buộc trước khi ghép:*
    1. Timestamp phần cứng đồng bộ tuyệt đối (độ lệch jitter < 5ms).
    2. Chiếu ngược (back-projection) hình học: vết tiếp đất (ground contact point) hoặc tâm 3D bounding box của 2 camera chiếu xuống mặt đường phải cách nhau dưới sai số cho phép (< 0.3m).
    3. Tính nhất quán về phân loại đối tượng (class label và vector đặc trưng ngoại quan appearance feature tương đồng).

- **Vì sao peer agreement hoặc quality report trên ảnh một camera chưa chứng minh gold set đúng cho cả bốn camera:**
  - Đánh giá trên một camera đơn lẻ chỉ đo lường được tính nhất quán nội bộ (internal consistency) của các đối tượng 2D trong khung nhìn hạn hẹp đó, hoàn toàn bỏ qua các lỗi không gian 3D của hệ thống vòm:
    1. Không kiểm tra được lỗi đồng bộ thời gian giữa 4 camera quanh xe.
    2. Không phát hiện được sự đứt gãy hoặc nhân đôi vật thể tại 4 đường may nối (seams).
    3. Không đánh giá được độ chính xác của phép chiếu Bird's Eye View (BEV) và đo cự ly thực tế (depth estimation).
    4. Góc nhìn, độ cao lắp đặt và đặc tính chiếu sáng ở 4 hướng xe rất khác nhau (camera trước chịu đèn xe đối diện, camera sau chịu chênh sáng lớn, hai camera hông bị ảnh hưởng bởi bóng xe), nên độ tin cậy trên 1 camera trước không thể đảm bảo chất lượng cho 3 camera còn lại.
