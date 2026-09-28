# Quan sát vạch ô đỗ

- Hai vạch `parking_line` đại diện đã vẽ (mô tả vị trí trong ảnh): Nhóm các vạch ngắn nghiêng ở tiền cảnh giữa-trái (y≈500–570, x≈50–460) là các vạch chia ô đỗ rõ nhất; và dãy vạch xa hơn về phía phải (y≈515–550, x≈550–835) cũng là vạch chia ô. Tổng cộng 14 polyline đã được vẽ sau khi soát và xóa các vạch không chia ô (ban đầu 23, đã sửa xuống 14).
- Một vạch/dấu sơn hoặc biên **không** vẽ, và vì sao: Dải đường xe chạy giữa hai dãy ô (lối di chuyển xe) không được gán nhãn `parking_line` vì đây là lối lưu thông, không phải vạch chia ô đỗ.
- Polygon `free_space` dừng ở đâu; có phần bị che nào không: Hai polygon `free_space` phủ vùng trống nhìn thấy được — polygon lớn (y≈511–683) bao vùng trống phía trước các ô giữa và phải; polygon nhỏ hơn (y≈494–520) bao vùng trống hàng ô gần hơn phía trái. Không có phần bị che rõ ràng trong ảnh này.
- Ca đã giải quyết: Ban đầu 23 polyline (vẽ quá nhiều vạch), sau khi soát lại còn 14 — đã xóa lối xe chạy và polyline trùng lặp. Không còn ca chưa chắc.
