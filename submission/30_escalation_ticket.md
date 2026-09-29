# Escalation ticket

## Ticket 1

- **Frame:** adasind_295948.jpg
- **Ảnh chụp:** submission/screenshots/review_overlay.png
- **Expected impact:** Phương tiện hai bánh (Bike R3) kích thước rất lớn ở tiền cảnh bên phải (tọa độ x≈570..1080 y≈605..1720) nằm giao thoa sát mép vùng thân xe ego và rìa vòng kính fisheye. Việc thiếu quy định rõ ràng khiến annotator phân vân giữa việc vẽ box hay coi toàn bộ là ignore scope, gây sai lệch lớn (missing P0) trong quá trình huấn luyện và đánh giá mô hình tránh va chạm của xe tự hành.
- **Owner:** guideline
- **Recommendation:** Guideline Ops cần bổ sung điều khoản hướng dẫn cho các ca giao thoa biên: nếu vật thể lớn nằm ở tiền cảnh bị cắt bởi thân xe ego nhưng phần nhìn thấy còn lại vẫn đủ nhận diện và cao > 40px thì bắt buộc vẽ box bao quanh phần nhìn thấy đó, gắn cờ `truncated=true` và không mở rộng polygon `ego_body` đè lên vật thể chuyển động.
