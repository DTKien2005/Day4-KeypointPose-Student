# Reviewer Checklist & Partner Review (Hình thức Solo / Tự rà soát)

Người gán: Đỗ Trung Kiên   |   Người kiểm: Đỗ Trung Kiên (Tự kiểm chéo độc lập)   |   Nhóm: T013   |   Ngày: 16/09/2026

---

## 1. Bảng kiểm Reviewer Checklist

| # | Mục kiểm | Đạt? | Ghi chú / Minh chứng ảnh |
| ---: | --- | :---: | --- |
| 1 | Mọi người trong ảnh đều có đủ 17 điểm, không ai bị thiếu | ☑ Đạt | 20 ảnh, 29 skeleton đều đủ 17 điểm theo chuẩn COCO-17 |
| 2 | Bật đường nối: không có xương nào cắt chéo ở vai hoặc hông | ☑ Đạt | Đã kiểm tra qua `tools/visualize_pose.py`, các đường nối thân song song |
| 3 | Không có xương nào kéo dài sang một cơ thể khác | ☑ Đạt | Không có lỗi nhầm người (`nham_nguoi = 0`) |
| 4 | Khớp bị che dùng `v = 1` **và có chấm**, không phải `v = 0` | ☑ Đạt | Đã chuẩn hóa tại các ca như hông (`train_05`), cổ tay (`train_01`) |
| 5 | `v = 0` chỉ xuất hiện ở khớp thật sự ra ngoài mép ảnh | ☑ Đạt | Chân người phụ nữ (`train_05`) và chân các đối tượng bị cắt mép |
| 6 | Không có dấu hiệu dùng `Hidden` (điểm `v = 2` nằm ở chỗ vô lý) | ☑ Đạt | Không sử dụng phím `h` trong toàn bộ quá trình gán nhãn |
| 7 | Export đúng **COCO Keypoints 1.0**: mảng `keypoints` có 51 số mỗi người | ☑ Đạt | File `annotations/coco_keypoints/person_keypoints_default.json` hợp lệ |
| 8 | Bản YOLO Pose: mỗi dòng 56 số, `kpt_shape: [17, 3]` | ☑ Đạt | 20 file txt trong `dataset/labels/train/` đều đúng 56 số/dòng |
| 9 | Visibility report đã nộp, và các chỉ số đã được thống kê | ☑ Đạt | Đã xuất `outputs/visibility_report.json` và `reports/visibility_report.md` |
| 10 | Mọi ca không rõ đều được ghi trong `GUIDELINE_MINI.md` | ☑ Đạt | Đã ghi đủ quy tắc nhóm và 3 ca mơ hồ cụ thể |
| 11 | `check_pose_labels.py` chạy 0 lỗi | ☑ Đạt | Đã chạy kiểm tra và đạt kết quả `ĐẠT định dạng` |

---

## 2. Các lỗi tìm được trong quá trình rà soát độc lập

| Ảnh | Người thứ | Khớp | Lỗi gì | Sửa thế nào |
| --- | ---: | --- | --- | --- |
| `train_16.jpg` | 1 | `left_eye`, `right_eye` | Mắt bị ngược chiều so với Vai và Hông (dấu hiệu đảo trái/phải) | Hoán đổi lại vị trí 2 mắt: `left_eye` sang bên trái, `right_eye` sang bên phải đồng nhất với vai. |
| `train_01.jpg` | 2 | `left_wrist` | Cổ tay bị khay bánh pizza che khuất nhưng lỡ để `v=0` (Outside) | Chuyển thành `v=1` (Occluded) và đặt chấm ước lượng ngay dưới viền khay bánh theo hướng cẳng tay. |
| `train_05.jpg` | 1 | `left_knee`, `right_knee`, `left_ankle`, `right_ankle` | Đặt chấm lên đĩa bánh pizza và lưng mèo khi chân thực tế ở dưới gầm bàn | Chuyển cả 4 điểm chân thành `v=0` (Outside) vì chân đã vượt ra ngoài mép dưới bức ảnh. |

---

## 3. Hai câu kết luận

1. **Lỗi lặp đi lặp lại nhiều nhất của bài này**: Phân vân giữa `v=1` (bị vật thể che nhưng còn trong khung) và `v=0` (bị góc chụp cắt đứt ra ngoài mép ảnh).
2. **Nó là lỗi thao tác hay lỗi guideline chưa rõ**: Chủ yếu là lỗi **guideline ban đầu chưa nêu rõ quy ước về các trường hợp đối tượng bị cắt ngang thân bởi đồ vật và mép ảnh**, sau khi bổ sung quy tắc cụ thể vào `GUIDELINE_MINI.md` thì lỗi này đã được khắc phục hoàn toàn.
