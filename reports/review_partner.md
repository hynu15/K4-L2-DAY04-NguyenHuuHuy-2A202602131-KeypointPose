# Review bài bạn cùng nhóm

Người gán: Ngọc Hiếu   Người kiểm: Nguyễn Hữu Huy   Ngày: 2026-09-16

Bài được kiểm: `person_keypoints_default.json` (bản trước rework), export COCO Keypoints 1.0,
chuyển sang YOLO Pose bằng `tools/coco_kp_to_yolo_pose.py`.

Đã chạy:

```bash
python3 tools/coco_kp_to_yolo_pose.py --coco "<bài của họ>/annotations/person_keypoints_default.json" \
    --out "<bài của họ>/dataset/labels/train"
python3 tools/check_pose_labels.py --images dataset/images/train --labels "<bài của họ>/dataset/labels/train"
python3 tools/visualize_pose.py --images dataset/images/train --labels "<bài của họ>/dataset/labels/train" \
    --out "<bài của họ>/outputs/vis_train"
python3 tools/visibility_report.py --labels dataset/labels/train \
    --compare "<bài của họ>/dataset/labels/train" --markdown reports/visibility_compare.md
```

Kết quả nhanh: 20/20 file, 31 skeleton, `v=2` 419 | `v=1` 90 | `v=0` 18. 0 lỗi định dạng, 5 cảnh báo.

## Reviewer checklist

| | Mục kiểm | Đạt? | Ghi chú / ảnh nào |
| --- | --- | --- | --- |
| 1 | Mọi người trong ảnh đều có đủ 17 điểm, không ai bị thiếu | ☑ | Mọi annotation đủ 51 số |
| 2 | Bật đường nối: không có xương nào cắt chéo ở vai hoặc hông | ☐ | Thân đúng; nhưng mắt/tai bị đảo so với vai ở `train_02` người 1, `train_16` người 2 |
| 3 | Không có xương nào kéo dài sang một cơ thể khác | ☑ | |
| 4 | Khớp bị che dùng `v = 1` **và có chấm**, không phải `v = 0` | ☑ | 90 khớp `v=1`, không có dấu hiệu xoá khớp bị che |
| 5 | `v = 0` chỉ xuất hiện ở khớp thật sự ra ngoài mép ảnh | ☑ | Cảnh báo ở `train_04` người 1 là chân bị cắt ở mép dưới ảnh - `v=0` đúng |
| 6 | Không có dấu hiệu dùng `Hidden` (điểm `v = 2` nằm ở chỗ vô lý) | ☑ | |
| 7 | Export đúng **COCO Keypoints 1.0**: mảng `keypoints` có 51 số mỗi người | ☑ | |
| 8 | Bản YOLO Pose: mỗi dòng 56 số, `kpt_shape: [17, 3]` | ☑ | |
| 9 | Visibility report đã nộp, và hai bảng đã được đặt cạnh nhau | ☑ | `reports/visibility_compare.md` |
| 10 | Mọi ca không rõ đều được ghi trong `GUIDELINE_MINI.md` | ☐ | Chưa đối chiếu được: chưa nhận `GUIDELINE_MINI.md` của Ngọc Hiếu |
| 11 | `check_pose_labels.py` chạy 0 lỗi | ☑ | 0 lỗi, 5 cảnh báo |

## Lỗi tìm được

| Ảnh | Người thứ | Khớp | Lỗi gì | Sửa thế nào |
| --- | ---: | --- | --- | --- |
| train_03 | 2 | cả skeleton | Thừa người: skeleton gán lên **con búp bê** nằm dưới đất (box ~ góc dưới giữa ảnh) | Xoá skeleton này |
| train_10 | 2 | cả skeleton | Thừa người: skeleton nhỏ ở vùng gương chiếu hậu / tay lái bên trái, không có người nào ở đó | Xoá skeleton này |
| train_10 | 1 | nose, left_eye, right_eye, left_ear, right_ear, vai, khuỷu, cổ tay | Cờ sai: gần như toàn bộ khớp để `v=1` dù mặt và hai tay nhìn rõ | Bỏ tick Occluded cho các khớp nhìn thấy (`v=2`) |
| train_02 | 1 | left_eye/right_eye, left_ear/right_ear | Nghi đảo trái/phải ở đầu: thứ tự mắt/tai ngược chiều với vai (script cảnh báo) | Người quay mặt về phía camera → mắt trái nằm bên phải ảnh; đổi chỗ hai mắt, hai tai |
| train_16 | 2 (áo đỏ số 23) | left_eye/right_eye | Nghi đảo trái/phải ở mắt: left_eye nằm bên phải right_eye trong khi người quay lưng (vai trái bên trái ảnh) | Đổi chỗ hai mắt; mắt gần như không thấy nên để `v=1` |

## Hai câu kết luận

- Lỗi lặp đi lặp lại nhiều nhất của bài này: **gán thừa skeleton lên vật không phải người** (búp bê, gương xe),
  và đảo trái/phải ở vùng đầu khi người quay nghiêng.
- Nó là lỗi **thao tác** (thừa skeleton, đảo mắt) cộng với **guideline chưa rõ** về cờ vùng mặt: `%v=1` của
  left_ear/left_eye/nose lệch 19-32 điểm giữa hai bài - nhóm cần thống nhất khi nào mắt/tai tính là "bị che".
