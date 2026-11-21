# Tự đánh giá — Trần Thọ Lâm · G02

## 1. Tự rà soát 4 cảnh

Nghỉ tay 1 phút, mở lại bài của mình, chọn 4 ảnh (2 S##, 2 D##), đọc như người lạ.

Loại lỗi: `seat_flip`, `wrong_class`, `guess`, `box`, `missing`, `extra`, `lr_flip` (trái/phải mặt), `flag` (thiếu cờ điểm), `ok`.

| Ảnh | Đối tượng | Loại lỗi | Ghi chú | Đã sửa? |
|---|---|---|---|---|
| `S07_rgb.png` | `object` (`box`) & `frame_meta` | `missing` | Ban đầu để `light=night` (sai quy định SVIRO phải là `unknown`) và bỏ sót thùng carton ở vùng tối ghế `rear_right` | Đã sửa |
| `S03_ir.png` | `child_seat` | `seat_flip` | Để nhầm giá trị mặc định của CVAT là `seat=driver` trên ảnh SVIRO hàng ghế sau | Đã sửa |
| `D01.png` | `hand` & `face` / `face13` | `box`, `flag` | Ban đầu vẽ `hand` ôm cả cánh tay, trùng `side=left` cho 2 tay và để `eye_right=unknown` nhưng chưa bật cờ `occluded` trên `face13` | Đã sửa |
| `D04.png` | `face` & `face13` | `guess` | Đã bật cờ che khuất cho cả 4 điểm mắt trên `face13` nhưng thuộc tính trên box `face` vẫn để `eye_left=open`, `eye_right=open` | Đã sửa |

## 2. Lỗi mình lặp lại nhiều nhất hôm nay

* **Lỗi:** Quên cập nhật các thuộc tính mặc định trên CVAT (như `seat=driver` trên ảnh SVIRO, `light=night` thay vì `unknown`) và lệch trạng thái giữa thuộc tính trên box `face` (`eye_left`/`eye_right`) với cờ `occluded`/`outside` trên bộ xương `face13`.
* **Vì sao mình mắc:** Do thao tác vẽ liên tiếp nhiều khung hình nên quen tay giữ giá trị mặc định đầu tiên trong danh sách dropdown của CVAT, và chưa đối chiếu chéo hai chiều giữa box `face` với từng điểm mốc của `face13` ngay lúc chấm.
* **Lần sau tránh bằng cách:** Gắn `frame_meta` ngay từ đầu cho toàn bộ khung hình theo từng nhóm ảnh (`S##` vs `D##`), kiểm tra lại thuộc tính ngay sau khi vẽ xong từng box, và chạy `cabin_check.py` kiểm tra sớm theo từng chặng nhỏ thay vì đợi xong hết cả 22 ảnh mới chạy.

## 3. Ghi chú về disagreement_log

* **Ghi chú thực tế (Cá nhân / Self-review):** Do thực hiện bài cá nhân và tự đối chiếu giữa hai phiên bản xuất dữ liệu (`export_final.zip` và `export_final_v2.zip`), ở dòng đối chiếu ảnh `D01.png` (`face13`), `D02.png` (`hand`, `face13`) và `D03.png` (`person`, `face`, `face13`), bản `v2` bị thiếu dữ liệu do thao tác xuất/sửa nhầm, nên phiên bản gốc `export_final.zip` là bản đúng theo Guideline Cabin v1.0.
* **Bài học rút ra:** Phải luôn kiểm tra số lượng đối tượng và chạy lại `cabin_check.py` ngay sau mỗi lần chỉnh sửa hoặc export lại từ CVAT để đảm bảo không vô tình làm mất nhãn hay skeleton đã hoàn thiện trước đó.