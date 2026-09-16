# Mini guideline - nhóm: T013  |  người gán: Đỗ Trung Kiên  |  ngày: 16/09/2026

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
| Hông của người mặc quần áo dài | Chọn `v=1` (Occluded), đặt chấm ước lượng tại tâm chỏm xương đùi (head of femur) ngang đường đáy thắt lưng / đáy đũng quần. | Quần áo dài hoặc tạp dề che mất mốc xương hông nhìn thấy được, nhưng cơ thể vẫn nằm trong khung hình nên phải ước lượng giải phẫu. |
| Tai bị tóc hoặc mũ bảo hiểm che một phần | Nếu thấy >=50% vành tai: `v=2`. Nếu bị tóc dài/mũ trùm che hoàn toàn: `v=1` và chấm ước lượng ngang đuôi mắt. | Vị trí tai gắn cố định vào hộp sọ, hoàn toàn suy luận được từ trục mắt-tai ngay cả khi bị tóc che phủ. |
| Người bị cắt ở mép ảnh (chỉ thấy từ hông trở lên) | Khớp nào nằm phía ngoài đường biên ảnh: chọn `v=0` (Outside), không đặt chấm. Khớp nào còn trong ảnh: gán bình thường (`v=2` hoặc `v=1`). | Không được phỏng đoán tọa độ nằm ngoài không gian bức ảnh để tránh làm model học ảo (hallucination). |
| Cổ tay nằm sau tay lái / sau thân mình / đĩa bánh | Đặt chấm ước lượng theo trục kéo dài của cẳng tay và chọn `v=1` (Occluded). | Hướng đi của xương quay và xương trụ (cẳng tay) chỉ ra chính xác vị trí cổ tay nằm ngay sau vật cản. |
| Hai người chồng lên nhau | Làm xong trọn vẹn 17 điểm của người đứng trước rồi mới gán người đứng sau; các khớp của người sau bị người trước che chọn `v=1`. | Tránh nhầm lẫn nối xương giữa hai cơ thể khác nhau (`nham_nguoi`). |
| Người nhỏ đến mức nào thì không gán nữa | Gán tất cả mọi người có thể phân biệt được ít nhất phần đầu và thân (chiều cao bbox > 30 pixel). | Bộ dữ liệu core đã được lọc đảm bảo mọi đối tượng đều đủ kích thước gán nhãn. |

## 3. Ba ca mơ hồ đã gặp (bắt buộc, ghi ít nhất 3)

### Ca 1 - ảnh `train_05.jpg`, người thứ `1`, khớp `left_knee`, `right_knee`, `left_ankle`, `right_ankle`

- Mơ hồ ở chỗ nào: Người phụ nữ ngồi sau bàn dã ngoại, có con mèo ăn pizza trên bàn che khuất toàn bộ phần thân dưới. Khay pizza và mép bàn nằm sát cạnh dưới của ảnh.
- Bạn quyết thế nào: Đánh dấu 4 điểm chân là `v=0` (Outside), không kéo chấm đặt lên đĩa pizza hay lưng mèo.
- Vì sao: Theo tư thế ngồi, cẳng chân và bàn chân chúc xuống đất dưới gầm bàn, đã hoàn toàn nằm ngoài mép ảnh. 
- Nếu người khác quyết ngược lại thì model học sai cái gì: Nếu cố tình chấm lên đĩa pizza hoặc con mèo, model sẽ học sai rằng thức ăn hoặc động vật là chân người (lỗi False Positive / Float).

### Ca 2 - ảnh `train_01.jpg`, người thứ `2`, khớp `left_wrist` (tay cầm pizza)

- Mơ hồ ở chỗ nào: Bàn tay và cổ tay của người đàn ông nâng khay bánh pizza lớn, cổ tay bị viền khay bánh và topping che mất ranh giới rõ ràng.
- Bạn quyết thế nào: Chọn `v=1` (Occluded), chấm ước lượng vào tâm khớp cổ tay ngay dưới viền khay bánh theo hướng cẳng tay đưa lên.
- Vì sao: Khớp cổ tay chắc chắn còn nằm trong bức ảnh và có bằng chứng định vị từ cẳng tay áo sọc.
- Nếu người khác quyết ngược lại thì model học sai cái gì: Nếu đánh `v=0` (Outside) sẽ làm mất một điểm giám sát hợp lệ; nếu đánh `v=2` thì model sẽ coi bề mặt chiếc bánh pizza là cổ tay người.

### Ca 3 - ảnh `train_16.jpg`, người thứ `1`, khớp `left_shoulder` / `right_shoulder`

- Mơ hồ ở chỗ nào: Đối tượng đứng nghiêng xoay người, phần mặt hướng về phía máy ảnh nhưng vai và thân trên lại vặn chéo tạo cảm giác phân vân giữa hướng nhìn của người quan sát và hướng cơ thể đối tượng.
- Bạn quyết thế nào: Kiên quyết lấy hệ quy chiếu là cơ thể của người trong ảnh: vai phải nằm ở phía bên trái bức ảnh, vai trái nằm ở phía bên phải bức ảnh.
- Vì sao: Tuân thủ tuyệt đối quy tắc giải phẫu sinh học thay vì quán tính thị giác từ góc nhìn màn hình.
- Nếu người khác quyết ngược lại thì model học sai cái gì: Model sẽ bị học lỗi đảo trái/phải (`dao_trai_phai`), khi dữ liệu được áp dụng kỹ thuật lật ảnh ngang (horizontal flip augmentation), model sẽ bị dạy sai gấp đôi và không thể hội tụ chính xác.

## 4. Sau khi so visibility report với bạn cùng nhóm

- Khớp lệch `%v=1` nhiều nhất: `left_ear` / `right_ear` (bạn `53.5%` / họ `35.0%`)
- Nguyên nhân là **guideline chưa rõ** hay **một trong hai bên gán sai**: **Guideline chưa rõ**: Một bên coi tóc che một phần là `v=1`, trong khi bên kia chỉ cần nhìn thấy thoáng qua vành tai là đánh `v=2`.
- Luật mới bổ sung vào mục 2 sau khi thống nhất: Quy định rõ ngưỡng định lượng: Nếu diện tích tai lộ ra >= 50% thì đánh `v=2`; nếu tóc che mất lỗ tai và trên 50% diện tích thì bắt buộc đánh `v=1` kèm chấm ước lượng.
