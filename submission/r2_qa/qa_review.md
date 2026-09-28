# QA review · B4-center

Mã khóa: CECC-18CC

| frame | object_ref | rule_id | nhận xét |
|---|---|---|---|
| adasind_270517.jpg | Missing | R01 | Vùng x≈167-269 y≈728-878 có ThreeWheeler cao ~150px (> ngưỡng H=40 R01) trong vùng hợp lệ nhưng chưa được vẽ box. |
| adasind_271039.jpg | L2 | R04 | Đối tượng x≈3 y≈822 w≈34 h≈60 là xe ba bánh (ThreeWheeler/auto-rickshaw) nhưng bị gán nhãn Car, cần đối chiếu R04. |
| adasind_271039.jpg | L7+L9 | R01 | Hai box L7 và L9 cùng bao phủ một người đi bộ tại vị trí x≈390-440 y≈808-937 (L7 nằm trọn trong L9), có dấu hiệu trùng lặp (DUPLICATE). |
| adasind_271039.jpg | Missing | R01 | Người đi bộ ở sát biên phải x≈992 y≈845 w≈34 h≈130 (> 40px) nhìn thấy rõ nhưng chưa có box Pedestrian. |
| adasind_295948.jpg | L2 | R04 | Xe tại x≈460 y≈892 w≈194 h≈190 là ô tô con/SUV chở người (Car) nhưng đang gán nhãn Truck theo R04. |
| adasind_295948.jpg | ignore_region | R08 | Thiếu 2 polygon lens_border ngoài vành kính; cả 4 polygon hiện đều mang thuộc tính ego_body, cần gán lại reason lens_border cho 2 góc. |
| adasind_295948.jpg | Missing | R01 | Xe hai bánh lớn ở tiền cảnh bên phải x≈570..1080 y≈605..1720 chưa có box Bike theo R01 và R03. |

Ghi finding r2_qa: cell=L_only, rule_id có giá trị, why để trống.
