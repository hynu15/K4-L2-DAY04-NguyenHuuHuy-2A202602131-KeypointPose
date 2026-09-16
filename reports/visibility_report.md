# Visibility report

- Thư mục nhãn: `dataset/labels/train`
- 20 ảnh, 29 skeleton, trung bình 14.28 khớp có v > 0 mỗi người
- Tổng: v=2 379 | v=1 35 | v=0 79

| # | Khớp | v=2 | v=1 | v=0 | %v=1 |
| ---: | --- | ---: | ---: | ---: | ---: |
| 0 | nose | 24 | 1 | 4 | 3% |
| 1 | left_eye | 21 | 2 | 6 | 7% |
| 2 | right_eye | 22 | 2 | 5 | 7% |
| 3 | left_ear | 16 | 1 | 12 | 3% |
| 4 | right_ear | 20 | 3 | 6 | 10% |
| 5 | left_shoulder | 29 | 0 | 0 | 0% |
| 6 | right_shoulder | 29 | 0 | 0 | 0% |
| 7 | left_elbow | 23 | 4 | 2 | 14% |
| 8 | right_elbow | 25 | 2 | 2 | 7% |
| 9 | left_wrist | 21 | 5 | 3 | 17% |
| 10 | right_wrist | 23 | 3 | 3 | 10% |
| 11 | left_hip | 25 | 3 | 1 | 10% |
| 12 | right_hip | 27 | 1 | 1 | 3% |
| 13 | left_knee | 20 | 2 | 7 | 7% |
| 14 | right_knee | 19 | 3 | 7 | 10% |
| 15 | left_ankle | 17 | 2 | 10 | 7% |
| 16 | right_ankle | 18 | 1 | 10 | 3% |

## Đọc bảng này thế nào

1. Khớp nào có **%v=1 cao**: khớp hay bị che. Cổ tay và hông thường là hai vị trí cần xem lại guideline trước khi kết luận.
2. Khớp nào có **v=0 cao bất thường**: mọi người đang dùng Outside ở chỗ đáng lẽ là Occluded. Đó là lỗi số 3 của slide 46, và nó xoá thẳng khớp đó khỏi bảng điểm OKS.
3. Khi so hai người: **lệch lớn = bất đồng về guideline**, không phải về bức ảnh. Sửa guideline trước, sửa nhãn sau.
