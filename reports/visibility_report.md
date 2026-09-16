# Visibility report

- Thư mục nhãn: `dataset/labels/train`
- 20 ảnh, 27 skeleton, trung bình 14.41 khớp có v > 0 mỗi người
- Tổng: v=2 355 | v=1 34 | v=0 70

| # | Khớp | v=2 | v=1 | v=0 | %v=1 |
| ---: | --- | ---: | ---: | ---: | ---: |
| 0 | nose | 22 | 1 | 4 | 4% |
| 1 | left_eye | 20 | 2 | 5 | 7% |
| 2 | right_eye | 20 | 2 | 5 | 7% |
| 3 | left_ear | 16 | 1 | 10 | 4% |
| 4 | right_ear | 18 | 3 | 6 | 11% |
| 5 | left_shoulder | 27 | 0 | 0 | 0% |
| 6 | right_shoulder | 27 | 0 | 0 | 0% |
| 7 | left_elbow | 23 | 4 | 0 | 15% |
| 8 | right_elbow | 23 | 2 | 2 | 7% |
| 9 | left_wrist | 22 | 4 | 1 | 15% |
| 10 | right_wrist | 21 | 3 | 3 | 11% |
| 11 | left_hip | 23 | 3 | 1 | 11% |
| 12 | right_hip | 25 | 1 | 1 | 4% |
| 13 | left_knee | 18 | 2 | 7 | 7% |
| 14 | right_knee | 17 | 3 | 7 | 11% |
| 15 | left_ankle | 16 | 2 | 9 | 7% |
| 16 | right_ankle | 17 | 1 | 9 | 4% |

## Đọc bảng này thế nào

1. Khớp nào có **%v=1 cao**: khớp hay bị che. Cổ tay và hông thường là hai vị trí cần xem lại guideline trước khi kết luận.
2. Khớp nào có **v=0 cao bất thường**: mọi người đang dùng Outside ở chỗ đáng lẽ là Occluded. Đó là lỗi số 3 của slide 46, và nó xoá thẳng khớp đó khỏi bảng điểm OKS.
3. Khi so hai người: **lệch lớn = bất đồng về guideline**, không phải về bức ảnh. Sửa guideline trước, sửa nhãn sau.
