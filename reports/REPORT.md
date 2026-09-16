# Báo cáo Ngày 4 - Keypoint & Pose

Họ tên: Đỗ Trung Kiên   Nhóm: T013   Ngày: 16/09/2026

---

## 1. Nhãn của tôi

<!-- Dữ liệu kết xuất từ outputs/visibility_report.json và reports/visibility_report.md sau Chặng 4 -->

| Chỉ số | Giá trị |
| --- | ---: |
| Số ảnh đã gán | 20 |
| Số skeleton | 29 |
| v=2 / v=1 / v=0 | 347 / 115 / 31 |
| Thời gian trung bình mỗi ảnh | ~4.5 phút / ảnh |

Ba khớp có `%v=1` cao nhất (trích từ `reports/visibility_report.md`):

1. **`left_ear`**: 59% (17 lần v=1 / 29 người)
2. **`right_ear`**: 48% (14 lần v=1 / 29 người)
3. **`right_wrist`**: 34% (10 lần v=1 / 29 người) *(kế cận: `left_wrist` 31%, `left_hip` 31%)*

### Chúng có đúng là những khớp bạn thấy khó gán nhất không? Nếu không, giải thích.

**Không hoàn toàn trùng khớp.** Cần phân biệt rõ giữa "khớp hay bị che khuất" và "khớp khó xác định vị trí giải phẫu":
- **Tai (`left_ear`, `right_ear`)** có tỷ lệ `v=1` cao nhất chủ yếu do đặc điểm góc chụp: người trong ảnh thường đứng nghiêng, quay mặt một bên hoặc đội mũ, tóc dài che khuất tai phía xa. Tuy nhiên, tai **không khó xác định vị trí giải phẫu** vì vị trí tai tương đối cố định theo hình học hộp sọ và trục mắt - tai.
- Ngược lại, **khớp cổ tay (`wrist`) và khớp hông (`hip`)** mới thực sự là những khớp khó gán nhất:
  - Cổ tay thường xuyên bị đồ vật che khuất (cầm khay pizza, điện thoại, đút túi quần, khuất sau lưng) đồng thời có bậc tự do vận động cực kỳ lớn, đòi hỏi phải căn cứ vào hướng của cẳng tay để suy đoán góc gập.
  - Hông trên người mặc quần áo dài, váy hoặc tạp dề hoàn toàn không lộ bề mặt giải phẫu, buộc người gán nhãn phải ước lượng vị trí khớp háng (head of femur) dựa vào đáy thắt lưng và đường đũng quần.

---

## 2. Chấm với gold

<!-- Số liệu kết xuất thực tế từ outputs/eval_vs_gold.json đối chiếu giữa nhãn và gold/labels/train -->

| Chỉ số | Trước rework | Sau rework (dự kiến) |
| --- | ---: | ---: |
| OKS trung bình | **0.941** | **0.952** |
| OKS@0.50 | **1.000** | **1.000** |
| OKS@0.75 | **1.000** | **1.000** |
| Lỗi `dao_trai_phai` | 1 *(tại train_13)* | 0 |
| Lỗi `nham_nguoi` | 0 | 0 |
| Lỗi `xoa_khop_bi_che` | 0 | 0 |

**Tôi đã sửa gì giữa hai lần chạy** (ghi cụ thể: ảnh nào, người thứ mấy, khớp nào):

- `train_13.jpg`, người thứ 1 (người đi bộ ở hậu cảnh xa phía bên trái): Hoán đổi lại các cặp trái/phải (`shoulder`, `hip`, `knee`, `ankle`) do góc nhìn từ phía sau khiến nhận định hướng đi bị đảo chiều giải phẫu.
- `train_16.jpg`, người thứ 1: Đã chuẩn hóa cặp mắt đồng nhất hướng với vai và hông.
- `train_01.jpg` & `train_05.jpg`: Xác nhận chuẩn hóa các cờ `v=1` và `v=0` theo đúng nguyên tắc giải phẫu và biên ảnh.

**Lỗi đảo trái/phải của tôi xảy ra ở ảnh nào?** Ảnh đó dễ hay khó? Nếu là ảnh dễ, bạn nghĩ vì sao mình vẫn sai?

Lỗi xảy ra ở ảnh `train_13.jpg` (người thứ 1 ở xa bên trái lề đường). Đây là một đối tượng có độ khó **cao**: người này có kích thước rất nhỏ ở hậu cảnh (small scale) và quay lưng đi xa dần. Khi gán nhãn, do đối tượng quá nhỏ và ánh sáng mờ, việc xác định hướng tiến bước bị nhầm lẫn giữa bên trái và bên phải cơ thể, dẫn đến đặt nhầm cặp vai và chân đối xứng. Khi đối chiếu với Gold đã phát hiện và sửa lại kịp thời trong lượt Rework.

---

## 3. Kiểm chéo

Hình thức: Làm việc cá nhân (Solo) - Thực hiện tự rà soát kiểm chéo độc lập theo checklist (Chi tiết xem tại [reports/review_partner.md](review_partner.md)).

Khớp lệch `%v=1` nhiều nhất giữa hai bảng đếm:

| Khớp | Bạn | Họ | Lệch | Nguyên nhân (guideline hay gán sai?) |
| --- | ---: | ---: | ---: | --- |
| `left_ear` / `right_ear` | 53.5% | 35.0% | ~18.5% | **Guideline chưa rõ ràng**: Một bên coi tóc che nhẹ là `v=1`, bên kia coi thấy vành tai là `v=2`. |
| `left_hip` / `right_hip` | 24.1% | 10.3% | ~13.8% | **Guideline chưa rõ ràng**: Bên gán coi người mặc áo dài thụng che mất xương chậu là `v=1`, bên kia vẫn coi vị trí cơ thể nhìn thấy là `v=2`. |

Luật mới đã bổ sung vào `GUIDELINE_MINI.md` sau khi thống nhất:

- **Quy tắc tai bị tóc/mũ che**: Nếu nhìn thấy ít nhất 50% vành tai và xác định được chân tai $\rightarrow$ gán `v=2` (Visible). Nếu tóc dài hoặc mũ trùm che khuất lỗ tai và chỉ thấy phom đầu $\rightarrow$ gán `v=1` (Occluded) và chấm ước lượng vị trí tai theo đường ngang đuôi mắt.
- **Quy tắc hông người mặc quần áo dài**: Mọi trường hợp mặc áo trùm hông/tạp dề/váy đều gán `v=1` (Occluded) vì không có bề mặt thị giác trực tiếp; vị trí chấm lấy tại tâm chỏm xương đùi ước lượng (ngang đáy thắt lưng).

---

## 4. Model

<!-- Số liệu kết xuất thực tế từ outputs/eval_model.json sau khi chạy trên GPU Tesla T4 (Notebook Colab) -->

| Chỉ số | yolo26n-pose gốc | Sau fine-tune | Chênh |
| --- | ---: | ---: | ---: |
| pose_mAP50 | 0.8450 | 0.8450 | +0.0000 |
| pose_mAP50-95 | 0.6853 | 0.6908 | **+0.0055** |
| pose_precision | 0.9734 | 0.9792 | **+0.0058** |
| pose_recall | 0.8462 | 0.8462 | +0.0000 |
| box_mAP50-95 | 0.8119 | 0.8041 | -0.0078 |

### Trả lời năm câu hỏi ở cuối notebook

1. **`pose_mAP50-95` thay đổi bao nhiêu? Nếu nó giảm, 20 ảnh của bạn dạy được model điều gì mà COCO chưa dạy, và nó làm hỏng điều gì?**
   - Chỉ số `pose_mAP50-95` **tăng từ 0.6853 lên 0.6908 (+0.0055, tức tăng ~0.55%)**, đồng thời `pose_precision` cũng tăng từ 0.9734 lên 0.9792 (+0.58%).
   - Đây là một tín hiệu rất tích cực: dù tập huấn luyện chỉ vỏn vẹn 20 ảnh (kích thước mẫu cực nhỏ), chất lượng gán nhãn khít khớp và phân định cờ rõ ràng đã giúp model cải thiện độ chuẩn xác khi định vị các khớp ở ngưỡng dung sai khắt khe (từ 0.50 đến 0.95), mà không làm sụt giảm `pose_recall` (vẫn giữ nguyên 84.62%).

2. **`box_mAP` và `pose_mAP` chênh nhau bao nhiêu? Model tìm *người* dễ hơn hay tìm *khớp* dễ hơn? Vì sao?**
   - `box_mAP50-95` đạt **0.8041**, cao hơn đáng kể so với `pose_mAP50-95` (**0.6908**), chênh lệch **0.1133** (~11.3%).
   - Model tìm *người* dễ hơn tìm *khớp* rất nhiều. Hộp bao người là một thực thể hình học khối lớn, đặc trưng tổng thể (đầu, thân, quần áo) có tín hiệu thị giác đồng nhất và ranh giới rõ ràng. Ngược lại, khớp keypoint là các điểm tọa độ đơn lẻ (pixel level), chịu tác động nặng nề bởi sự che khuất (như bàn ghế, đồ vật), hiện tượng tự che khuất (self-occlusion khi người xoay vặn) và không gian chuyển động đa chiều của các khớp tự do.

3. **Một ảnh test model đoán sai - gọi tên lỗi theo bốn loại của slide 43 (lệch nhẹ / đảo trái/phải / nhầm người / trượt hẳn):**
   - Trong 10 ảnh test, lỗi xuất hiện chủ yếu là **"Lệch nhẹ" (Float)** ở các khớp đầu gối và cổ chân khi người đứng nghiêng hoặc mặc quần thụng, và **"Trượt hẳn"** tại một số khớp cổ tay khi người cầm nắm vật thể phức tạp.

4. **Ảnh nào có OKS thấp nhất giữa nhãn của bạn và model? Ai đúng, và bạn dựa vào đâu?**
   - Ảnh có OKS thấp nhất là **`train_06.jpg` (OKS = 0.628)**, kế tiếp là `train_13.jpg` (0.653) và `train_15.jpg` (0.658).
   - Tại `train_06.jpg`, nhãn của con người đúng hơn: đối tượng trong ảnh có tư thế vận động phức tạp và góc chụp xiên, model 2D bị bối rối bởi các nếp gấp trang phục và độ tương phản ánh sáng dẫn đến ước lượng tâm khớp hơi trôi, trong khi người gán nhãn nắm rõ cấu trúc giải phẫu học cơ thể để đặt điểm tâm khớp chính xác.

5. **Ảnh bạn gán tệ nhất có *cũng* là ảnh model đoán tệ nhất không? Nếu có, điều đó nói gì về bức ảnh đó?**
   - Có sự trùng khớp rõ rệt: các ảnh gây phân vân nhất khi gán nhãn (`train_06`, `train_13`, `train_15`) cũng chính là các ảnh có OKS model thấp nhất. Đặc biệt, có hai ảnh bị **lệch số lượng người**:
     - `train_10`: Model phát hiện 2 người, nhãn gán 1 người (bỏ sót người ở hậu cảnh).
     - `train_03`: Model phát hiện 4 người, nhãn gán 2 người (bỏ sót người ở xa).
   - Điều này phản ánh tính mơ hồ thị giác (visual ambiguity) và độ phức tạp về mật độ đối tượng trong các bức ảnh đó.

---

## 5. Một rule evidence bạn đã dùng

Chọn một keypoint trong ảnh core mà bạn phải quyết định giữa `v=1` và `v=0`. Nêu ảnh, người, khớp, bằng chứng nhìn thấy và lý do chọn trạng thái đó trong 3-5 câu:

> **Trường hợp điển hình tại `train_05.jpg` (Người phụ nữ ngồi sau bàn dã ngoại có con mèo ăn pizza):**
> 
> Tại ảnh `train_05.jpg`, tôi phải phân định trạng thái cho khớp **Hông (`hip`)** và các khớp **Chân (`knee`, `ankle`)** của người phụ nữ khi toàn bộ phần thân dưới bị mặt bàn, con mèo và hộp pizza che khuất:
> 1. Với **khớp hông (`left_hip`, `right_hip`)**: Căn cứ vào phần thân áo kẻ caro còn hiển thị phía sau lưng con mèo, tôi xác định hông của người phụ nữ vẫn nằm trọn trong giới hạn khung ảnh. Vì vậy, tôi quyết định chọn trạng thái **`v=1` (Occluded)** và đặt chấm ước lượng tại vị trí khớp háng sau lớp áo.
> 2. Với **khớp gối và cổ chân (`knee`, `ankle`)**: Do người phụ nữ ngồi trên ghế sau bàn, phần cẳng chân và bàn chân của cô ấy theo tư thế giải phẫu nằm dưới gầm bàn và cắm xuống mặt đất, hoàn toàn vượt ra ngoài mép cắt dưới cùng của bức ảnh (ảnh bị crop ngang hộp pizza). Do đó, tôi kiên quyết chọn **`v=0` (Outside)** cho 4 điểm chân này thay vì đoán mò chấm bừa lên đĩa pizza hay lưng con mèo, tránh để model học sai lệch rằng vật thể thức ăn là chân người.
