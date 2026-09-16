# Báo cáo Ngày 4 - Keypoint & Pose

Họ tên: Nguyễn Hữu Huy   Nhóm: ______   Ngày: 2026-09-16

## 1. Nhãn của tôi

Số liệu lấy từ `reports/visibility_report.md` tại bản khoá nhãn (commit `17a6bca`, trước rework).
Sau rework, `reports/visibility_report.md` được sinh lại: 20 ảnh, 29 skeleton, v=2/v=1/v=0 = 379/34/80.

| Chỉ số | Giá trị |
| --- | ---: |
| Số ảnh đã gán | 20 |
| Số skeleton | 27 |
| v=2 / v=1 / v=0 | 355 / 34 / 70 |
| Thời gian trung bình mỗi ảnh | ______ |

Ba khớp có `%v=1` cao nhất (chép từ `reports/visibility_report.md`):

1. `left_elbow` - 15% (4/27)
2. `left_wrist` - 15% (4/27)
3. Đồng hạng 11% (3/27): `right_ear`, `right_wrist`, `left_hip`, `right_knee`

Chúng có đúng là những khớp bạn thấy khó gán nhất không? Nếu không, giải thích.

Chỉ đúng một phần. Khuỷu và cổ tay đúng là **hay bị che** (tay nắm tay lái xe máy ở `train_10`,
`train_15`; tay đi sau thân người kia ở `train_03`), nhưng vị trí vẫn suy ra được từ vai và bàn tay
nên không khó xác định. Khớp khó nhất thực ra là **vùng mặt khi người quay lưng/đội mũ bảo hiểm**
(`train_09`, `train_16`, `train_19`) và **cổ chân sau xe** (`train_15`). Chúng không xuất hiện trong
top `%v=1` chính vì tôi đã gắn `v=0` cho chúng: `left_ear` có 10 `v=0`, `left_ankle`/`right_ankle`
mỗi khớp 9 `v=0` - dấu hiệu của lỗi "xoá khớp bị che", sau đó gold xác nhận.

## 2. Chấm với gold

Số liệu từ `outputs/eval_vs_gold_before_rework.json` (lần chấm đầu) và `outputs/eval_vs_gold.json` (sau rework).

| Chỉ số | Trước rework | Sau rework |
| --- | ---: | ---: |
| OKS trung bình | 0.9229 | 0.92 |
| OKS@0.50 | 0.931 | 1.0 |
| OKS@0.75 | 0.8966 | 0.9655 |
| Lỗi `dao_trai_phai` | 0 | 0 |
| Lỗi `nham_nguoi` | 2 | 2 |
| Lỗi `xoa_khop_bi_che` | 5 | 5 |
| Người ghép được / gold | 27 / 29 | 29 / 29 |

**Tôi đã sửa gì giữa hai lần chạy** (ghi cụ thể: ảnh nào, người thứ mấy, khớp nào):

- `train_13`: bổ sung skeleton đủ 17 điểm cho **2 người ở hậu cảnh** (người nhỏ bên trái và người áo vàng) - hết lỗi `thieu_nguoi`, số người ghép được từ 27 lên 29.
- `train_04`, người bên phải (đội mũ bảo hiểm): chỉnh vị trí `right_elbow`, `right_wrist`, `right_hip`.
- `train_04`, người bên trái: chỉnh `left_elbow`, `left_wrist` (đổi sang `v=1` vì cổ tay nằm sau tay lái); `left_hip` nằm ngoài mép dưới ảnh nên đặt `v=0` (`check_pose_labels.py` báo lỗi toạ độ ngoài ảnh).
- `train_16`, người áo đỏ số 23: chỉnh `left_hip`, `left_knee`, `right_knee`, `left_ankle`, `right_ankle`, `left_wrist`, `right_elbow`.
- `train_16`, người áo trắng: chỉnh `right_elbow`, `right_hip`, `left_ankle`.

Kết quả: bổ sung đủ người giúp OKS@0.50 đạt 1.0 và OKS@0.75 tăng từ 0.8966 lên 0.9655; bài vẫn ở mức
"Xuất sắc" theo RUBRIC.

**Lỗi đảo trái/phải của tôi xảy ra ở ảnh nào?** Ảnh đó dễ hay khó? Nếu là ảnh dễ,
bạn nghĩ vì sao mình vẫn sai?

Không có lỗi đảo trái/phải trong toàn bộ 20 ảnh (`dao_trai_phai` = 0 ở cả hai lần chấm).
Lỗi nặng nhất của tôi là `nham_nguoi` ở `train_03`: hai người đứng sát nhau, cánh tay phải
của người 1 và người 2 nằm chồng lên thân nhau - ảnh không khó về pose nhưng khó về định danh.

## 3. Kiểm chéo

Bạn cùng nhóm: ______

Khớp lệch `%v=1` nhiều nhất giữa hai bảng đếm (`reports/visibility_compare.md`):

| Khớp | Bạn | Họ | Lệch | Nguyên nhân (guideline hay gán sai?) |
| --- | ---: | ---: | ---: | --- |
| `left_ear` | 4% | 35% | 32 | Guideline chưa rõ: tai dưới mũ bảo hiểm / người quay lưng. Tôi dùng `v=0`, họ dùng `v=1`. Gold xác nhận phía tôi sai |
| `left_eye` | 7% | 29% | 22 | Cùng nguyên nhân: mắt của người quay lưng (`train_09`, `train_16`) tôi để `v=0` |
| `nose` | 4% | 23% | 19 | Cùng nguyên nhân; gold báo `xoa_khop_bi_che` ở `train_16` người 2 `nose` |

Tổng hai bài: tôi `v=2/v=1/v=0` = 355/34/70 trên 27 skeleton; bạn cùng nhóm 419/90/18 trên 31 skeleton.
Chi tiết lỗi tìm được trong bài bạn cùng nhóm ở `reports/review_partner.md`.

Luật mới đã bổ sung vào `GUIDELINE_MINI.md` sau khi thống nhất:

- Đầu còn trong khung ảnh thì mọi khớp mặt (`nose`, `left_eye`, `right_eye`, `left_ear`, `right_ear`)
  là `v=1` hoặc `v=2`, **không bao giờ `v=0`** - kể cả khi người quay lưng hoặc đội mũ bảo hiểm;
  chấm ở mặt trước của đầu, ngang tầm tai.
- Nhìn thấy giày/bàn chân trong ảnh thì cổ chân không thể là `v=0`; bị xe che thì `v=1`.

## 4. Model

<!-- Chép số từ outputs/eval_model.json sau Chặng 6. “Chênh” = sau fine-tune trừ baseline;
đây là quan sát trên tập test, không phải chất lượng sản phẩm. -->

| Chỉ số | yolo26n-pose gốc | Sau fine-tune | Chênh |
| --- | ---: | ---: | ---: |
| pose_mAP50 | | | |
| pose_mAP50-95 | | | |
| pose_precision | | | |
| pose_recall | | | |
| box_mAP50-95 | | | |

### Trả lời năm câu hỏi ở cuối notebook

> Mỗi câu cần trỏ tới ảnh/chỉ số cụ thể. Một con số thấp không tự chứng minh nhãn sai;
> kiểm lại bằng bằng chứng thị giác và kết quả gold.

1. `pose_mAP50-95` thay đổi bao nhiêu? Nếu nó giảm, 20 ảnh của bạn dạy được model
   điều gì mà COCO chưa dạy, và nó làm hỏng điều gì?

2. `box_mAP` và `pose_mAP` chênh nhau bao nhiêu? Model tìm *người* dễ hơn hay tìm
   *khớp* dễ hơn? Vì sao?

3. Một ảnh test model đoán sai - gọi tên lỗi theo bốn loại của slide 43
   (lệch nhẹ / đảo trái/phải / nhầm người / trượt hẳn):

4. Ảnh nào có OKS thấp nhất giữa nhãn của bạn và model? Ai đúng, và bạn dựa vào đâu?

5. Ảnh bạn gán tệ nhất có *cũng* là ảnh model đoán tệ nhất không? Nếu có, điều đó
   nói gì về bức ảnh đó?

## 5. Một rule evidence bạn đã dùng

`train_15`, người thứ 2 (người đội mũ bảo hiểm đen đứng sau xe mô tô, bên trái ảnh), khớp
`left_ankle` và `right_ankle`. Thân xe che gần hết cẳng chân, nhưng **đôi giày vẫn nhìn thấy rõ**
ở dưới gầm xe, gần mép dưới ảnh, và ống quần nối liền từ gối xuống giày. Vì bàn chân nằm trong ảnh
nên cổ chân chắc chắn còn trong khung; thứ che nó là chiếc xe chứ không phải mép ảnh. Do đó
trạng thái đúng là `v=1` với chấm đặt ngay trên cổ giày. Lần gán đầu tôi để `v=0` và gold báo hai lỗi
`xoa_khop_bi_che` ở đúng hai khớp này - bằng chứng cho thấy luật "thấy giày thì cổ chân không thể là `v=0`" là cần thiết.
