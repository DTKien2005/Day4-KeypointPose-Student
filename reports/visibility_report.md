# Visibility report

- Thư mục nhãn: `dataset\labels\train`
- 20 ảnh, 29 skeleton, trung bình 15.93 khớp có v > 0 mỗi người
- Tổng: v=2 345 | v=1 117 | v=0 31

| # | Khớp | v=2 | v=1 | v=0 | %v=1 |
| ---: | --- | ---: | ---: | ---: | ---: |
| 0 | nose | 24 | 5 | 0 | 17% |
| 1 | left_eye | 21 | 8 | 0 | 28% |
| 2 | right_eye | 22 | 7 | 0 | 24% |
| 3 | left_ear | 12 | 17 | 0 | 59% |
| 4 | right_ear | 15 | 14 | 0 | 48% |
| 5 | left_shoulder | 25 | 4 | 0 | 14% |
| 6 | right_shoulder | 28 | 1 | 0 | 3% |
| 7 | left_elbow | 23 | 6 | 0 | 21% |
| 8 | right_elbow | 27 | 2 | 0 | 7% |
| 9 | left_wrist | 19 | 10 | 0 | 34% |
| 10 | right_wrist | 18 | 10 | 1 | 34% |
| 11 | left_hip | 19 | 9 | 1 | 31% |
| 12 | right_hip | 23 | 5 | 1 | 17% |
| 13 | left_knee | 17 | 7 | 5 | 24% |
| 14 | right_knee | 20 | 4 | 5 | 14% |
| 15 | left_ankle | 16 | 4 | 9 | 14% |
| 16 | right_ankle | 16 | 4 | 9 | 14% |

## Đọc bảng này thế nào

1. Khớp nào có **%v=1 cao**: khớp hay bị che. Cổ tay và hông thường là hai vị trí cần xem lại guideline trước khi kết luận.
2. Khớp nào có **v=0 cao bất thường**: mọi người đang dùng Outside ở chỗ đáng lẽ là Occluded. Đó là lỗi số 3 của slide 46, và nó xoá thẳng khớp đó khỏi bảng điểm OKS.
3. Khi so hai người: **lệch lớn = bất đồng về guideline**, không phải về bức ảnh. Sửa guideline trước, sửa nhãn sau.
