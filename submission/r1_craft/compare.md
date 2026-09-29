# So sánh L với R

Chỉ số L/R là thứ tự box cao ≥ H=40 trong từng frame, theo thứ tự XML; bắt đầu từ 1.
Box L trong ignore_region được báo IGNORE_SCOPE, không tính SPURIOUS.

## adasind_270517.jpg
- R4 center MISSING
## adasind_271039.jpg
- L8 center IGNORE_SCOPE
- L2+R9 edge WRONG_CLASS
- L7 center SPURIOUS
- R4 edge MISSING
- R6 mid MISSING
- R10 center MISSING
## adasind_295948.jpg
- L3 mid IGNORE_SCOPE
- L4 mid IGNORE_SCOPE
- L2+R1 center WRONG_CLASS
- R3 mid MISSING

## Theo zone
| zone | n_ref | matched | missing | spurious |
|---|---|---|---|---|
| center | 12 | 9 | 3 | 2 |
| mid | 5 | 3 | 2 | 0 |
| edge | 3 | 1 | 2 | 1 |
