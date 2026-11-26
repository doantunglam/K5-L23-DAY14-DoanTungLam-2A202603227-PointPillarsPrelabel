# Báo cáo Day 14 — Đoàn Tùng Lâm — 2A202603227

- Mã học viên dùng với `--student`: `227`
- `seed_tag` trong `frames_A.txt`: `393590736f`

## 1. Dự đoán trước sweep (bước 5b)

Ghi **trước** khi chạy `scripts/pp_sweep.py`, rồi commit và push ngay (lịch sử commit là bằng chứng thời điểm viết). Không sửa phần này sau khi có kết quả.

- Tham số tôi nghĩ giảm công sửa nhiều nhất: `score_thr` tăng từ 0.1 lên khoảng 0.4, kèm `min_points=5`. Tôi dự đoán `score_thr` đóng góp phần lớn mức giảm, còn `min_points` chỉ giảm thêm một ít.
- Vì sao: sau khi sửa tay tập A, cấu hình gốc (`score_thr=0.1`, `min_points=0`) cho 108 box, trong đó 56 box phải xóa, 3 box phải thêm và 8 box phải sửa, chi phí 23.8. Riêng phần xóa đã là 56 × 0.3 = 16.8, khoảng 70% tổng chi phí, còn phần thêm chỉ 3 × 1.0 = 3.0. Tức là model thừa box nhiều hơn hẳn sót box. Ở ngưỡng 0.1 model giữ cả các box điểm tin cậy thấp, và model gán nhiều Cyclist (17 box ở s01f45, 29 box trên cả 3 frame), nhiều khả năng là nhiễu trên bụi cây và lề đường. Tăng `score_thr` và đặt `min_points` sẽ loại phần lớn số box này, nên chi phí xóa giảm mạnh. Rủi ro tôi thấy: lọc quá tay sẽ làm mất cả box thật (xe ở xa, người đi bộ ít điểm), khi đó mỗi box thật bị mất phải thêm lại với giá 1.0 thay vì 0.3, nên tôi không đoán mức lọc cao nhất (0.5) là tốt nhất.

## 2. Tập A: sửa tay và tune

| Cấu hình | Xóa | Thêm | Sửa | Chi phí |
|---|---:|---:|---:|---:|
| Gốc | | | | |
| `best_config.json` | | | | |

- Tham số đã đổi trong `best_config.json`: ...
- Kết quả leave-one-out (dán các dòng `scripts/pp_sweep.py` in ra): ...
- Cấu hình có ổn định trên từng frame không? Vì sao tôi vẫn chọn (hoặc không chọn) nó: ...

## 3. Trước/sau (bước 8)

![so sánh](../../d14_<ID>/compare/<frame>.jpg)

- `scripts/pp_compare.py` báo giảm (hoặc tăng) ...% công sửa trên tập A.
- Dự đoán ở mục 1 đúng hay sai? Giải thích bằng số box xóa/thêm/sửa: ...

## 4. Tập B

Số đo từ `scripts/pp_prelabel.py --frames d14_<ID>/frames_B.txt --config d14_<ID>/best_config.json --gt d14_<ID>/ref_B.json`:

| Xóa | Thêm | Sửa | Chi phí |
|---:|---:|---:|---:|
| | | | |

- Loại box tốn công sửa nhất ở tập B: ...

## 5. Giới hạn

- Nêu ít nhất 2 giới hạn (ví dụ: chỉ 3 frame tune; model KITTI không có Truck/Bus; chi phí là công thức, chưa phải thời gian sửa thật): ...
