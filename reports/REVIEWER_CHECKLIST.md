# Reviewer checklist - điền khi kiểm bài người khác

Người gán: ______   Người kiểm: Nguyễn Hữu Huy   Ngày: 2026-09-16

Chạy trước khi soi bằng mắt:

```bash
python3 tools/check_pose_labels.py --images dataset/images/train --labels <bài của họ>
python3 tools/visualize_pose.py --images dataset/images/train --labels <bài của họ> --out /tmp/vis_review
python3 tools/visibility_report.py --labels dataset/labels/train --compare <bài của họ>
```

Bài được kiểm: bản export COCO Keypoints 1.0 của bạn cùng nhóm (trước rework), đã chuyển sang YOLO Pose
bằng `tools/coco_kp_to_yolo_pose.py`. Kết quả `check_pose_labels.py`: 20/20 file, 31 skeleton,
`v=2` 419 | `v=1` 90 | `v=0` 18, **0 lỗi**, 5 cảnh báo.

| | Mục kiểm | Đạt? | Ghi chú / ảnh nào |
| --- | --- | --- | --- |
| 1 | Mọi người trong ảnh đều có đủ 17 điểm, không ai bị thiếu | ☑ | Mọi annotation đủ 51 số; `train_13` có đủ 3 người |
| 2 | Bật đường nối: không có xương nào cắt chéo ở vai hoặc hông | ☐ | Thân đúng, nhưng mắt/tai ngược chiều vai ở `train_02` người 1 và `train_16` người 2 |
| 3 | Không có xương nào kéo dài sang một cơ thể khác | ☑ | |
| 4 | Khớp bị che dùng `v = 1` **và có chấm**, không phải `v = 0` | ☑ | 90 khớp `v=1`, không có dấu hiệu xoá khớp bị che |
| 5 | `v = 0` chỉ xuất hiện ở khớp thật sự ra ngoài mép ảnh | ☑ | Cảnh báo ở `train_04` người 1 là chân bị cắt ở mép dưới ảnh - `v=0` đúng |
| 6 | Không có dấu hiệu dùng `Hidden` (điểm `v = 2` nằm ở chỗ vô lý) | ☑ | |
| 7 | Export đúng **COCO Keypoints 1.0**: mảng `keypoints` có 51 số mỗi người | ☑ | |
| 8 | Bản YOLO Pose: mỗi dòng 56 số, `kpt_shape: [17, 3]` | ☑ | `data.yaml`: `kpt_shape: [17, 3]` |
| 9 | Visibility report đã nộp, và hai bảng đã được đặt cạnh nhau | ☑ | `reports/visibility_compare.md` |
| 10 | Mọi ca không rõ đều được ghi trong `GUIDELINE_MINI.md` | ☐ | Chưa nhận được `GUIDELINE_MINI.md` của bạn cùng nhóm |
| 11 | `check_pose_labels.py` chạy 0 lỗi | ☑ | 0 lỗi, 5 cảnh báo |

## Lỗi tìm được

Chép sang `reports/review_partner.md`. Mỗi dòng một lỗi, đủ bốn cột - người sửa phải
mở đúng chỗ đó được mà không cần hỏi lại.

| Ảnh | Người thứ | Khớp | Lỗi gì | Sửa thế nào |
| --- | ---: | --- | --- | --- |
| train_03 | 2 | cả skeleton | Thừa người: skeleton gán lên **con búp bê** nằm dưới đất | Xoá skeleton này |
| train_10 | 2 | cả skeleton | Thừa người: skeleton nhỏ ở vùng gương chiếu hậu / tay lái bên trái, không có người | Xoá skeleton này |
| train_10 | 1 | nose, left_eye, right_eye, left_ear, right_ear, vai, khuỷu, cổ tay | Cờ sai: gần như toàn bộ khớp để `v=1` dù mặt và hai tay nhìn rõ | Bỏ tick Occluded cho các khớp nhìn thấy (`v=2`) |
| train_02 | 1 | left_eye/right_eye, left_ear/right_ear | Nghi đảo trái/phải ở đầu: thứ tự mắt/tai ngược chiều với vai | Người quay mặt về camera → mắt trái nằm bên phải ảnh; đổi chỗ hai mắt, hai tai |
| train_16 | 2 (áo đỏ số 23) | left_eye/right_eye | Nghi đảo trái/phải ở mắt: người quay lưng nhưng left_eye nằm bên phải right_eye | Đổi chỗ hai mắt; mắt gần như không thấy nên để `v=1` |

## Hai câu kết luận

- Lỗi lặp đi lặp lại nhiều nhất của bài này: **gán thừa skeleton lên vật không phải người** (búp bê ở
  `train_03`, gương xe ở `train_10`), và đảo trái/phải ở vùng đầu khi người quay nghiêng.
- Nó là lỗi **thao tác** hay lỗi **guideline chưa rõ**? Thừa skeleton và đảo mắt là lỗi **thao tác**;
  lệch cờ vùng mặt (`%v=1` của `left_ear` 35% so với 4% của tôi) là do **guideline chưa rõ** về
  mắt/tai của người quay lưng hoặc đội mũ bảo hiểm - đã bổ sung luật vào `GUIDELINE_MINI.md`.
