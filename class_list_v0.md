
# Danh sách lớp v0 — Trần Thọ Lâm · G02

Viết tiếp từ slide 54 bài giảng. Mỗi lớp trả lời: **model cần nhận ra cái này để làm gì?**

## DMS (ít nhất 6 lớp)

| # | Lớp / thuộc tính | Giá trị | Để làm gì (chức năng DMS) | Nhìn thấy được trên IR? |
|---|---|---|---|---|
| 1 | `gaze_zone` | `road` / `mirror_l` / `mirror_r` / `mirror_c` / `cluster` / `center` / `lap` / `other` | Cảnh báo mất tập trung khi tài xế rời mắt khỏi mặt đường quá lâu | Có, dựa theo hướng quay đầu và vị trí đồng tử |
| 2 | `phone` (`state`) | `held` / `mounted` / `lap` | Phát hiện hành vi cầm hoặc sử dụng điện thoại gây xao nhãng khi lái xe | Có (nhận diện qua hình khối cạnh/màn hình điện thoại) |
| 3 | `hand` (`on_wheel`) | `yes` / `no` / `unknown` | Giám sát việc tài xế có giữ tay trên vô lăng hay đang buông tay làm việc khác | Có, trừ khi tay hạ thấp vào vùng tối sâu dưới đáy khung hình (như trên `D01`) |
| 4 | `seatbelt` (trên `person`) | `worn_correct` / `worn_incorrect` / `not_worn` / `unknown` | Cảnh báo tài xế chưa thắt dây an toàn hoặc thắt sai quy cách (luồn dưới nách/sau lưng) | Có, trừ khi áo tối màu trùng nền hoặc bị vật thể che khuất |
| 5 | `face13` (keypoints) | 13 điểm mốc (mắt, mũi, miệng) | Tính toán chính xác góc xoay đầu (head pose), độ mở mắt (EAR) và ngáp (MAR) | Có, trừ các điểm bị sống mũi hoặc góc nghiêng che khuất (như mắt phải trên `D01`) |
| 6 | `glasses` | `none` / `clear` / `sun` / `unknown` | Hỗ trợ hệ thống biết khi nào mắt bị kính râm che hoặc cần lọc nhiễu lóa hồng ngoại trên tròng kính | Có, dễ nhận ra qua gọng kính hoặc đốm phản xạ LED trên tròng |

## OMS (ít nhất 6 lớp)

| # | Lớp / thuộc tính | Giá trị | Để làm gì (chức năng OMS) | Nhìn thấy được trên IR? |
|---|---|---|---|---|
| 1 | `person` (`seat`) | `rear_left` / `rear_middle` / `rear_right` | Đếm số lượng và xác định vị trí từng hành khách trên hàng ghế sau để điều chỉnh túi khí và điều hòa | Có (nhận diện rõ hình khối cơ thể qua đèn chiếu hồng ngoại) |
| 2 | `seatbelt` (trên `person`) | `worn_correct` / `worn_incorrect` / `not_worn` / `unknown` | Nhắc nhở hành khách ngồi hàng ghế sau cài dây an toàn trước khi xe di chuyển | Hạn chế — dễ bị mất tương phản khi người mặc áo tối màu hoặc thiếu sáng (như trên `S07_ir`) |
| 3 | `child_seat` (`occupied`, `facing`) | `yes` / `no` / `unknown`; `forward` / `rear` / `unknown` | Kiểm tra ghế trẻ em có trẻ ngồi bên trong hay không để kích hoạt cảnh báo bỏ quên trẻ trên xe | Có (nhận diện rõ khung ghế và hướng quay của ghế qua hình dạng) |
| 4 | `infant_seat` (`occupied`, `facing`) | `yes` / `no` / `unknown`; `rear` / `forward` / `unknown` | Cảnh báo chống bỏ quên trẻ sơ sinh trong xe và hỗ trợ tự động ngắt túi khí nếu lắp sai quy chuẩn | Có (nhận diện qua kiểu dáng nôi xách tay đặc trưng) |
| 5 | `object` (`type`, `seat`) | `bag` / `box` / `phone` / `bottle` / `other` | Cảnh báo hành lý, đồ đạc bị bỏ quên trên ghế sau khi hành khách đã xuống xe | Có — trên `S07_ir` còn nhìn rõ thùng carton (`box`) hơn cả ảnh `S07_rgb` ban đêm |
| 6 | `camera_blocked` | `yes` / `no` | Cảnh báo hệ thống khi ống kính camera cabin bị che khuất, bám bẩn hoặc mất tầm nhìn | Có (nhận biết ngay khi toàn khung hình bị tối đen hoặc mờ đặc bất thường) |

## 2 câu hỏi leo thang

Câu hỏi bạn muốn hỏi người viết guideline (hoặc khách hàng) vì guideline v1.0 chưa trả lời. Mỗi câu kèm **tên khung** làm ví dụ.

1. **Câu hỏi:** Khi cánh tay tài xế hạ xuống vùng tối sát mép dưới ảnh IR khiến bàn tay bị hòa lẫn vào bóng tối (không thấy rõ ranh giới cổ tay và các ngón tay), có nên vẽ ước lượng box `hand` với `on_wheel=unknown` hay bỏ qua hoàn toàn?
   * **Khung ví dụ:** `D01.png`
   * **tạm gán thế nào và vì sao:** Chỉ vẽ box `hand` cho bàn tay thấy rõ từ cổ tay đến đầu ngón tay; phần cánh tay chìm vào vùng tối không phân biệt được bàn tay thì không vẽ box `hand` để tránh tạo nhãn suy đoán sai biên theo nguyên tắc *"không thấy thì không bịa"* của Guideline v1.0.
2. **Câu hỏi:** Với các ảnh SVIRO ban đêm (`S07_rgb.png`) mà một vật thể (thùng carton) chỉ lộ ra khi tự ý chỉnh tăng độ sáng (Brightness) trên công cụ gán nhãn chứ nhìn bằng mắt thường ở ảnh gốc gần như đen kịt, thì người gán có được phép chỉnh sáng để gán nhãn hay phải giữ nguyên ảnh gốc?
   * **Khung ví dụ:** `S07_rgb.png`
   * **tạm gán thế nào và vì sao:** Khi tăng độ sáng trên CVAT và xác nhận được hình khối thùng carton ở ghế `rear_right`, tôi tạm gán box `object` (`type=box`) để đồng bộ với vật thể hiện hữu trong cảnh; tuy nhiên cần làm rõ giới hạn chỉnh sáng để tránh việc mỗi annotator kéo sáng khác nhau dẫn đến lệch nhãn giữa RGB và IR.