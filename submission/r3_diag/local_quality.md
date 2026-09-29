# Đối chiếu chất lượng cục bộ — rectangle

Teaching reference, không phải gold set đã phê duyệt; không có điểm đạt tự động.
Nguồn: export r1_craft đã khóa SHA256 `cecc18ccae1899474e36e159857144f8e01bcb802d49abe66f6483a31ac3e170`; slice `B4-center`.
Ghép hình học greedy một-một theo IoU ≥ 0.50, rồi so class; H ≥ 40 px.
Box trái nằm chủ yếu trong ignore_region reference không tính. Polygon, polyline, track không được chấm.
Đây là phép tính offline của lab, không phải báo cáo hay kết quả tương đương CVAT Premium.

Frame được tính: adasind_270517.jpg, adasind_271039.jpg, adasind_295948.jpg. Frame thiếu trong export: không.
TP=13; FP=3; FN=7; số lần đối chiếu=21; mean IoU của TP=0.868.

| Chỉ số | Micro | Macro | Nhãn thấp nhất |
|---|---:|---:|---:|
| accuracy | 0.619 | 0.905 | 0.857 |
| precision | 0.812 | 0.827 | 0.500 |
| recall | 0.650 | 0.686 | 0.250 |
| jaccard | 0.565 | 0.542 | 0.250 |
| dice | 0.722 | 0.687 | 0.400 |

| Nhãn | TP | FP | FN | Accuracy | Precision | Recall | Jaccard | Dice |
|---|---:|---:|---:|---:|---:|---:|---:|---:|
| Bike | 2 | 0 | 1 | 0.952 | 1.000 | 0.667 | 0.667 | 0.800 |
| Car | 4 | 1 | 1 | 0.905 | 0.800 | 0.800 | 0.667 | 0.800 |
| Pedestrian | 5 | 1 | 2 | 0.857 | 0.833 | 0.714 | 0.625 | 0.769 |
| ThreeWheeler | 1 | 0 | 3 | 0.857 | 1.000 | 0.250 | 0.250 | 0.400 |
| Truck | 1 | 1 | 0 | 0.952 | 0.500 | 1.000 | 0.500 | 0.667 |

| Frame | TP | FP | FN | Accuracy | Precision | Recall |
|---|---:|---:|---:|---:|---:|---:|
| adasind_270517.jpg | 6 | 0 | 1 | 0.857 | 1.000 | 0.857 |
| adasind_271039.jpg | 6 | 2 | 4 | 0.545 | 0.750 | 0.600 |
| adasind_295948.jpg | 1 | 1 | 2 | 0.333 | 0.500 | 0.333 |

Confusion matrix: hàng = teaching reference; cột = export đã khóa.
`<missing>` là thiếu box; `<extra>` là box thừa. Xem `local_quality_confusion.csv`.

| Reference \ Export | Bike | Car | Pedestrian | ThreeWheeler | Truck | <missing> |
|---|---:|---:|---:|---:|---:|---:|
| Bike | 2 | 0 | 0 | 0 | 0 | 1 |
| Car | 0 | 4 | 0 | 0 | 1 | 0 |
| Pedestrian | 0 | 0 | 5 | 0 | 0 | 2 |
| ThreeWheeler | 0 | 1 | 0 | 1 | 0 | 2 |
| Truck | 0 | 0 | 0 | 0 | 1 | 0 |
| <extra> | 0 | 0 | 1 | 0 | 0 | 0 |

Chi tiết xung đột trong `local_quality_conflicts.csv`; dữ liệu máy đọc trong `local_quality.json`.
Mismatching label đóng góp một FP cho class vẽ và một FN cho class reference; attribute khác được báo riêng.
Micro accuracy đếm mỗi cặp ghép sai class là một lần đối chiếu; Jaccard đếm cả FP và FN.
Macro/worst bỏ nhãn không xuất hiện ở cả hai phía; chỉ số không có mẫu là N/A.
