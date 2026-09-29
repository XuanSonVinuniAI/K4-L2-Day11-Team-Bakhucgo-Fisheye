# Zone table (slice của bạn)

Lệnh `python3 lab11.py model` tự ghi bảng số (cùng cách đếm với `r1_craft/compare.md` và `model_compare.md`); chạy lại lệnh sẽ cập nhật bảng và giữ nguyên phần nhận xét. Bạn chỉ viết mục Nhận xét.

| Zone | n_ref | L missing | L spurious | M missing (`LR_noM` + `R_only`) | M thừa (`LM_noR` + `M_only`) | Lỗi L chính (`what`) |
|---|---:|---:|---:|---:|---:|---|
| center | 12 | 3 | 2 | 4 | 7 | MISSING (2) |
| mid | 5 | 2 | 0 | 2 | 4 | MISSING (2) |
| edge | 3 | 2 | 1 | 1 | 1 | WRONG_CLASS (1) |

## Nhận xét

- **Zone gãy nhiều nhất:**
  - Với người gán nhãn (L): Về số lượng tuyệt đối, zone `center` gãy nhiều nhất với 3 ca `missing` và 2 ca `spurious` trên tổng số 12 ref. Tuy nhiên, xét theo tỷ lệ, zone `edge` có tỷ lệ bỏ sót cao nhất (2/3 missing = 66.7%), chủ yếu là các vật thể bị méo quang học và sai class (`WRONG_CLASS`).
  - Với model (M): Model gãy nghiêm trọng nhất ở zone `center` với 4 ca missing và tới 7 box thừa (`spurious`), kế đến là zone `mid` (2 missing, 4 thừa). Tổng cộng model sinh ra 12 box thừa trên toàn slice.
- **Giả thuyết nguyên nhân và giới hạn slice:**
  - Vùng `center` có mật độ giao thông dày đặc, nhiều vật thể bị che khuất một phần (`occluded`) như cụm người đi bộ hay xe ba bánh cạnh ô tô, khiến annotator dễ bỏ sót hoặc vẽ trùng box (DUPLICATE), đồng thời model YOLO (huấn luyện trên ảnh thường) bị kích hoạt sai nhiều ở vùng này.
  - Vùng `edge` chịu méo hình cầu cực lớn từ thấu kính fisheye, các đường thẳng bị uốn cong mạnh khiến người gán nhãn dễ nhầm lẫn hình thái class (ThreeWheeler thành Car) và bỏ sót các vật sát rìa vòng kính.
  - **Giới hạn slice 3 frame:** Mẫu chỉ gồm 3 frame tĩnh rời rạc với 20 vật thể tham chiếu trong slice `B4-center`, chưa đủ độ đại diện thống kê cho mọi tình huống chuyển động, góc chiếu sáng và các ca biên của hệ thống camera SVM thực tế.
