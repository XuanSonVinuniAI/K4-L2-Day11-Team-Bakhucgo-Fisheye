# Exit ticket

Đọc `docs/10-svm360-reading-vi.md` trước khi trả lời câu 1–2. Các câu về zone, `why`, rework, parking và sampling
đã nằm trong file tương ứng nên không hỏi lại ở đây.

1. **Một vật ở vùng seam giữa hai camera thật xuất hiện với hai box khác nhau: đó là lỗi `DUPLICATE` hay cần một quy tắc riêng? Vì sao?**
   - Đây **không phải là lỗi DUPLICATE**, mà bắt buộc cần một **quy tắc riêng (Cross-Camera / Seam Association Policy)**.
   - *Lý do:* Về mặt vật lý quang học, vùng đường may nối (seam) là vùng giao thoa giữa hai trường nhìn (FOV) của hai camera fisheye đặt tại hai vị trí khác nhau trên xe (ví dụ camera trước và camera hông). Trong không gian ảnh 2D của từng sensor, mỗi camera chụp được góc nhìn, độ méo và phần bề mặt khác nhau của cùng một chiếc xe hoặc người đi bộ. Do đó, việc mỗi camera có một 2D bounding box độc lập là hoàn toàn chính xác và cần thiết để huấn luyện mô hình 2D detector trên từng góc máy. Chỉ khi dữ liệu được chiếu lên không gian hợp nhất 3D hoặc Bird's Eye View (BEV), hệ thống mới cần thuật toán kết hợp hai box này lại thành một đối tượng duy nhất thông qua thông số hiệu chuẩn ngoại suy (extrinsic calibration). Nếu vội vàng xóa đi một box ở cấp độ ảnh 2D, mô hình sẽ bị thiếu dữ liệu huấn luyện cục bộ cho camera đó.

2. **Một vật đi qua nhiều frame trên cùng camera: khi nào giữ cùng track ID, khi nào thêm keyframe hoặc trạng thái Outside? Nêu bằng chứng sẽ cần trước khi nối track qua hai camera.**
   - *Giữ cùng track ID:* Khi đối tượng di chuyển liên tục trong tầm nhìn của camera, kể cả khi bị che khuất tạm thời trong thời gian ngắn (dưới 10–15 frame) rồi xuất hiện trở lại với quỹ đạo chuyển động có thể dự đoán được.
   - *Thêm keyframe:* Khi đối tượng có sự thay đổi đột ngột về quỹ đạo (quay đầu, rẽ ngoặt), tăng giảm tốc độ đột biến, hoặc hình dạng bounding box co giãn phi tuyến do di chuyển từ vùng tâm ít méo ra vùng rìa méo quang học cao của thấu kính fisheye.
   - *Đánh dấu trạng thái Outside:* Khi đối tượng di chuyển ra khỏi vòng kính hữu ích của thấu kính fisheye, vượt ra khỏi khung hình camera, hoặc bị vật cản lớn che khuất hoàn toàn trong thời gian dài mà không thể suy đoán vị trí.
   - *Bằng chứng bắt buộc trước khi nối track qua hai camera:*
     1. Đồng bộ thời gian phần cứng (hardware timestamp sync) giữa hai khung hình với độ lệch < 5ms.
     2. Tính liên tục về động học (kinematic continuity): vector vận tốc, hướng di chuyển và tọa độ 3D mặt đất tại vùng seam giữa hai camera phải khớp với mô hình chuyển động vật lý.
     3. Tính nhất quán về đặc trưng nhận dạng (Re-ID appearance feature): màu sắc, loại class và cấu trúc đối tượng phải tương thích sau khi đã bù trừ sai lệch ánh sáng giữa hai góc máy.

3. **Nhìn lại cả buổi: một chỗ bạn tin nhãn mình đúng nhưng reference hoặc người soát nghĩ khác (dẫn frame/`object_ref`), bạn đã xử lý thế nào, và nếu làm lại slice này bạn sẽ đổi gì trong cách làm?**
   - *Ca cụ thể:* Đối tượng xe hai bánh lớn Bike R3 ở tiền cảnh bên phải tại frame `adasind_295948.jpg`. Ban đầu khi gán nhãn, annotator cho rằng vật thể này nằm quá sát mép dưới và bị nắp capo xe ego che cắt ngang, nên nghiêng về việc coi đây là vùng ignore region. Tuy nhiên, teaching reference và QA độc lập chỉ ra rằng đây là phương tiện chuyển động có kích thước lớn (> 40px), nằm ngay phía trước đầu xe và quyết định trực tiếp đến cự ly phanh an toàn nên bắt buộc phải có box.
   - *Cách xử lý:* Nhóm đã tổ chức phân tích đối chiếu luật R01 (ngưỡng chiều cao) và R06/R09 (quy tắc ignore), mở escalation ticket `30_escalation_ticket.md` và ghi nhận quyết định trong `40_decision_log.csv`. Tại vòng P5 rework, nhóm đã thống nhất bổ sung box Bike bao trọn phần nhìn thấy ngoài phạm vi thân xe và gắn nhãn `truncated=true`.
   - *Nếu làm lại slice này:* Sẽ chủ động rà soát kỹ ranh giới nắp capo (`ego_body`) và vành kính (`lens_border`) ngay từ khâu tự soát (self-QC) ở P2, phóng to độ phân giải tối đa ở các vùng giáp ranh, và chủ động trao đổi sớm giữa annotator và QA để thống nhất ranh giới trước khi khóa bản nộp đầu tiên.
