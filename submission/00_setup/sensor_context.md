# Sensor context

## Thông tin slice

- **Slice được giao:** B2-edge (người thực hiện: son)
- **3 frame trong slice:** adasind_069450.jpg · adasind_082170.jpg · adasind_102750.jpg

## Thông tin camera

- **Rig:** Camera fisheye đơn gắn trên xe ego (bộ dữ liệu ADASIND), góc nhìn ~180°, ảnh dọc 1080×1920 px.
- **Vị trí lắp:** Camera hướng về phía trước/trên xe; ảnh bao quát toàn cảnh đường xung quanh xe.
- **Vòng kính (lens circle):** Hình tròn chiếm phần lớn khung hình; vành đen ở 4 góc ngoài vòng là vùng mù quang học — đã import sẵn 2 polygon `ignore_region` (reason: `lens_border`) mỗi frame.
- **ego_body:** Thân xe ego (capo/mũi xe) xuất hiện ở vùng giữa-dưới frame; phải vẽ polygon `ignore_region` (reason: `ego_body`) ở các frame nhìn thấy thân xe.

## Đặc điểm slice B2-edge

- **Loại vùng:** Edge zone — đối tượng nằm ở rìa/biên ảnh fisheye, gần vùng méo quang cao.
- **Méo hình:** Vật thể ở rìa bị kéo dài và cong đáng kể; box phải bao phần **nhìn thấy trên ảnh gốc**, không nắn thẳng.
- **Thuộc tính edge_zone:** Các box ở rìa có attribute `edge_zone=true` (đã prefill, cần soát lại).

## Giới hạn một camera

- **Chỉ một góc nhìn:** Bộ dữ liệu ADASIND chỉ có một camera fisheye; không đại diện đủ 4 góc SVM (front/rear/left/right).
- **Không có calibration:** Không có thông số intrinsic/extrinsic → không thể tính khoảng cách thật, không ghép box giữa các camera hay gán track ID chung.
- **Không có timestamp đồng bộ:** 3 frame là ảnh rời, không phải video liên tục; không suy luận chuyển động giữa frame.
- **Vùng chồng (seam) không xác định:** Với một camera đơn, không biết vật nào xuất hiện đồng thời ở camera khác.