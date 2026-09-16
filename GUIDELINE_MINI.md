# Mini guideline - nhóm: ______  |  người gán: Nguyễn Hữu Huy  |  ngày: 2026-09-16

> Điền file này **trong lúc** gán nhãn, không phải sau khi xong. Mỗi lần bạn dừng lại
> hơn 10 giây để phân vân, đó là một dòng phải ghi vào đây.

## 1. Luật bắt buộc (đã thống nhất cả lớp - không sửa)

- Bộ 17 điểm COCO, đúng tên, đúng thứ tự. Lấy từ file `.SVG` chung.
- Mọi người trong ảnh đều có **đủ 17 điểm**. Điểm không dùng được thì gắn cờ, không xoá.
- Trái/phải tính theo **cơ thể người**, không theo bức ảnh.
- Bị che, còn trong khung -> `v = 1`, **vẫn đặt chấm** ở vị trí ước lượng.
- Ra ngoài mép ảnh -> `v = 0`, **không** đặt chấm.
- Không dùng `Hidden` (`h`) - nó không được lưu vào file.

## 2. Luật của nhóm bạn (phải điền)

| Tình huống | Luật nhóm bạn chọn | Vì sao |
| --- | --- | --- |
| Hông của người mặc quần áo dài | Đặt chấm ở chỗ đùi nối vào thân: ngang đường cạp quần, lùi vào trong khoảng 1/4 bề rộng hông tính từ mép ngoài. Thấy rõ đường quần/eo -> `v=2`; bị áo khoác dài, xe hoặc người khác che -> `v=1` và vẫn đặt chấm | Hông không có bề mặt nhìn thấy trên người mặc quần áo, nên phải có một mốc giải phẫu chung. Không bao giờ để `v=0` khi thân người còn trong khung |
| Tai bị tóc hoặc mũ bảo hiểm che một phần | `v=1`, chấm ở vị trí tai ước lượng trên mũ: ngang tầm mắt, phía sau góc hàm. Chỉ `v=2` khi thấy vành tai | Đầu còn trong khung thì tai còn trong khung. Để `v=0` là xoá khớp khỏi OKS (lỗi `xoa_khop_bi_che`) |
| Người bị cắt ở mép ảnh (chỉ thấy từ hông trở lên) | Khớp nằm **ngoài** mép ảnh -> `v=0`, không chấm. Khớp nằm sát mép nhưng vẫn thấy -> `v=2`. Khớp trong khung nhưng bị vật che -> `v=1` | `v=0` chỉ dành cho "ra ngoài khung". Trước khi bấm `o`, kiểm lại: nếu kéo dài xương đùi/cẳng chân mà khớp vẫn rơi trong ảnh thì phải là `v=1` |
| Cổ tay nằm sau tay lái / sau thân mình | `v=1`, chấm ở cuối cẳng tay kéo dài từ khuỷu, trước chỗ bàn tay nắm tay lái | Cổ tay còn trong khung, vị trí suy ra được từ khuỷu và bàn tay nhìn thấy |
| Hai người chồng lên nhau | Làm xong hẳn một người mới sang người kế tiếp. Với tay đi qua thân người kia: đi dọc vai -> khuỷu -> cổ tay, chuỗi phải liền mạch trên **một** cánh tay; khớp bị che dùng `v=1` | Tránh lỗi `nham_nguoi` (chấm rơi sang cơ thể bên cạnh) |
| Người nhỏ đến mức nào thì không gán nữa | Bộ ảnh core đã chọn sao cho mọi người đều đủ lớn: **gán tất cả mọi người**, kể cả người mờ/nhỏ ở hậu cảnh | Gold có skeleton cho người ở hậu cảnh; bỏ qua là lỗi `thieu_nguoi` (OKS = 0 cho người đó) |

Ảnh mẫu (trích từ ảnh core):

| Tình huống | Ảnh mẫu | Ghi chú |
| --- | --- | --- |
| Hông bị che | ![](reports/guideline_img/hip_train_10.jpg) | `train_10`: hông sau kính chắn gió xe máy -> `v=1`, chấm ước lượng |
| Tai dưới mũ bảo hiểm | ![](reports/guideline_img/ear_helmet_train_04.jpg) | `train_04` người đội mũ bên phải: tai `v=1` |
| Bị cắt ở mép ảnh | ![](reports/guideline_img/edge_cut_train_04.jpg) | `train_04` người bên trái: gối/cổ chân ra ngoài mép dưới -> `v=0` |
| Cổ tay sau tay lái | ![](reports/guideline_img/wrist_handlebar_train_10.jpg) | `train_10`: bàn tay nắm tay lái, cổ tay trong tay áo -> `v=1` |
| Hai người chồng lên nhau | ![](reports/guideline_img/overlap_train_03.jpg) | `train_03`: tay phải của hai người đan vào nhau |
| Người ở hậu cảnh | ![](reports/guideline_img/background_train_13.jpg) | `train_13`: hai người mờ phía sau vẫn phải gán |

## 3. Ba ca mơ hồ đã gặp (bắt buộc, ghi ít nhất 3)

### Ca 1 - ảnh `train_09`, người thứ `1`, khớp `nose`, `left_eye`, `right_eye`

![](reports/guideline_img/back_head_train_09.jpg)

- Mơ hồ ở chỗ nào: người lái xe máy quay lưng hoàn toàn về camera, chỉ thấy gáy và mũ bảo hiểm. Mặt không có một điểm nào nhìn thấy.
- Bạn quyết thế nào: lúc gán để `v=0`. Chấm gold báo lỗi `xoa_khop_bi_che` ở `left_eye`, nên luật nhóm chốt lại là `v=1`, chấm ở phía trước mũ, ngang tầm tai.
- Vì sao: đầu nằm gọn trong khung, nên mắt/mũi chỉ **bị che bởi chính đầu**, không ra ngoài mép ảnh. Theo luật lớp, đó là `v=1`.
- Nếu người khác quyết ngược lại thì model học sai cái gì: với `v=0`, model coi mặt "không tồn tại" mỗi khi người quay lưng, nên sẽ không dự đoán được vị trí đầu cho người đi xe nhìn từ phía sau.

### Ca 2 - ảnh `train_15`, người thứ `2` (người đội mũ đen, bên trái), khớp `left_ankle`, `right_ankle`

![](reports/guideline_img/ankle_bike_train_15.jpg)

- Mơ hồ ở chỗ nào: chân bị thân xe mô tô che gần hết, chỉ thấy giày ở sát mép dưới xe. Dễ tưởng chân "không có".
- Bạn quyết thế nào: lúc gán để `v=0`. Chấm gold báo lỗi `xoa_khop_bi_che` ở cả hai cổ chân, nên luật nhóm chốt lại là `v=1`, chấm ngay trên đôi giày nhìn thấy.
- Vì sao: đôi giày nằm trong ảnh, nên cổ chân chắc chắn còn trong khung. Vật che là chiếc xe, không phải mép ảnh.
- Nếu người khác quyết ngược lại thì model học sai cái gì: model học rằng người đứng sau xe không có chân. Đây là cảnh rất phổ biến trong bộ ảnh (xe máy, xe đạp).

### Ca 3 - ảnh `train_03`, người thứ `1` và `2`, khớp `right_elbow`, `right_wrist`

![](reports/guideline_img/overlap_train_03.jpg)

- Mơ hồ ở chỗ nào: hai người đứng sát nhau, cánh tay của người này đi ngang qua thân người kia. Khuỷu và cổ tay của hai người nằm cách nhau chỉ vài chục pixel.
- Bạn quyết thế nào: lúc gán đặt chấm theo "cánh tay gần nhất". Gold báo `nham_nguoi` ở `right_elbow` (người 1) và `right_wrist` (người 2). Luật nhóm chốt lại: lần theo từng cánh tay từ vai xuống, dựa vào màu tay áo (áo khoác da vs áo sẫm màu).
- Vì sao: màu và chất liệu tay áo là bằng chứng liền mạch duy nhất để biết cánh tay thuộc người nào.
- Nếu người khác quyết ngược lại thì model học sai cái gì: xương cánh tay nối sang người khác, model học các chi "bay" giữa hai người đứng gần nhau. Đây là lỗi nặng hơn lệch nhẹ.

### Ca 4 - ảnh `train_13`, người ở hậu cảnh

![](reports/guideline_img/background_train_13.jpg)

- Mơ hồ ở chỗ nào: hai người phía sau bị mờ (xoá phông), nhỏ hơn người chính nhiều.
- Bạn quyết thế nào: lúc gán bỏ qua. Gold báo 2 lỗi `thieu_nguoi`. Rework: gán đủ 17 điểm cho cả hai người.
- Vì sao: GUIDE nói mọi người trong bộ ảnh core đều đủ lớn để gán; ảnh mờ vẫn đủ để thấy vai, hông, gối.
- Nếu người khác quyết ngược lại thì model học sai cái gì: model học rằng người mờ ở hậu cảnh không phải là người, nên bỏ sót người trong ảnh có độ sâu trường ảnh nông.

## 4. Sau khi so visibility report với bạn cùng nhóm

- Khớp lệch `%v=1` nhiều nhất: `left_ear` (bạn `4%` / họ `35%`); tiếp theo `left_eye` (`7%` / `29%`), `nose` (`4%` / `23%`).
- Nguyên nhân là **guideline chưa rõ** hay **một trong hai bên gán sai**: **guideline chưa rõ** cho vùng mặt khi người quay lưng hoặc đội mũ bảo hiểm. Tôi dùng `v=0` (coi như "không có"), bạn cùng nhóm dùng `v=1`. Kết quả chấm gold xác nhận cách của bạn cùng nhóm đúng: tôi bị 5 lỗi `xoa_khop_bi_che` và 3 lỗi thiếu khớp, phần lớn ở mặt (`nose`, `left_eye`, `right_eye`, `right_ear`).
- Luật mới bổ sung vào mục 2 sau khi thống nhất: **Đầu còn trong khung thì mọi khớp mặt (mũi, mắt, tai) là `v=1` hoặc `v=2`, không bao giờ `v=0`** - kể cả khi quay lưng hoặc đội mũ. Chấm mũi/mắt ở mặt trước của đầu, ngang tầm tai. Tương tự cho chân: thấy giày thì cổ chân không thể là `v=0`.
