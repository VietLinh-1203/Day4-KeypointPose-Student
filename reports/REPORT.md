# Báo cáo Ngày 4 - Keypoint & Pose

Họ tên: ___Đỗ Nguyễn Việt Linh___   Nhóm: ______   Ngày: ___16/09/2026___

> Cách dùng: copy file này thành `reports/REPORT.md`. Điền bằng số liệu do công cụ sinh ra;
> không tự ước lượng hoặc sửa số trong file JSON.

## 1. Nhãn của tôi

<!-- Lấy số từ reports/visibility_report.md hoặc outputs/visibility_report.json sau Chặng 4.
Số ảnh phải là 20; số skeleton là tổng số người trong 20 ảnh. Thời gian trung bình = tổng
thời gian gán / 20. -->

| Chỉ số | Giá trị |
| --- | ---: |
| Số ảnh đã gán | 20 |
| Số skeleton | 29 |
| v=2 / v=1 / v=0 | v=2 333 / v=1 129 / v=0 31 |
| Thời gian trung bình mỗi ảnh | 14,15 phút |

Ba khớp có `%v=1` cao nhất (chép từ `reports/visibility_report.md`):

1. left_ear 62%
2. left_hip 41%
3. right_ear 38%

Chúng có đúng là những khớp bạn thấy khó gán nhất không? Nếu không, giải thích.

Những khớp này thường bị che bởi rất nhiều yếu tố: Người đội mũ, nghiêng mặt , quay đi, người khác che, ... Nên thường phải dự đoán vị trí các bộ phận này, nhưng cơ bản là cũng không quá khó để xác định.

## 2. Chấm với gold

<!-- Lấy hai cột từ outputs/eval_vs_gold.json: một lần ngay khi protected release mở và một
lần sau rework. Đếm số phần tử trong từng danh sách lỗi, không tự làm tròn. -->

| Chỉ số | Trước rework | Sau rework |
| --- | ---: | ---: |
| OKS trung bình | 0.9311 | 0.9311 |
| OKS@0.50 | 1.0 | 1.0 |
| OKS@0.75 | 1.0 | 1.0 |
| Lỗi `dao_trai_phai` | 1 | 1 |
| Lỗi `nham_nguoi` | 1 | 1 |
| Lỗi `xoa_khop_bi_che` | 0 | 0 |

**Tôi đã sửa gì giữa hai lần chạy** (ghi cụ thể: ảnh nào, người thứ mấy, khớp nào):

<!-- Mỗi dòng phải có: tên ảnh + người thứ mấy + keypoint + thao tác sửa. Không viết “đã sửa
lại một số lỗi”. -->

-
-
-

**Lỗi đảo trái/phải của tôi xảy ra ở ảnh nào?** Ảnh đó dễ hay khó? Nếu là ảnh dễ,
bạn nghĩ vì sao mình vẫn sai?

<!-- Nếu không có lỗi, ghi rõ “Không có lỗi đảo trái/phải trong toàn bộ 20 ảnh.” -->
Lỗi dao_trai_phai xuất hiện ở ảnh train_13.jpg, cụ thể:
+ gold_person: 1
+ your_person: 3
Ảnh này khá khó vì đối tượng người ở khá xa và không rõ ràng, dựa vào tư thế người áo kẻ xanh đen trong ảnh, keypoint mặc dù về bản chất là đúng hướng nhưng vì các keypoint rất gần nhau, dễ gây lỗi đảo trái/phải
## 3. Kiểm chéo

Bạn cùng nhóm: ______

Khớp lệch `%v=1` nhiều nhất giữa hai bảng đếm:

| Khớp | Bạn | Họ | Lệch | Nguyên nhân (guideline hay gán sai?) |
| --- | ---: | ---: | ---: | --- |
| | | | | |
| | | | | |

Luật mới đã bổ sung vào `GUIDELINE_MINI.md` sau khi thống nhất:

<!-- Viết một rule kiểm chứng được: điều kiện nhìn thấy/căn cứ vị trí → chọn v=1 hoặc v=0.
Không chỉ ghi “cẩn thận hơn khi gán”. -->


## 4. Model

<!-- Chép số từ outputs/eval_model.json sau Chặng 6. “Chênh” = sau fine-tune trừ baseline;
đây là quan sát trên tập test, không phải chất lượng sản phẩm. -->

| Chỉ số | yolo26n-pose gốc | Sau fine-tune | Chênh |
| --- | ---: | ---: | ---: |
| pose_mAP50 | 0.845 | 0.845 | 0.0 |
| pose_mAP50-95 | 0.6853 | 0.6908 | +0.0055 |
| pose_precision | 0.9734 | 0.9792 | +0.0058 |
| pose_recall | 0.8462 | 0.8462 | 0.0 |
| box_mAP50-95 | 0.8119 | 0.8041 | -0.0078 |

### Trả lời năm câu hỏi ở cuối notebook

> Mỗi câu cần trỏ tới ảnh/chỉ số cụ thể. Một con số thấp không tự chứng minh nhãn sai;
> kiểm lại bằng bằng chứng thị giác và kết quả gold.

1. `pose_mAP50-95` thay đổi bao nhiêu? Nếu nó giảm, 20 ảnh của bạn dạy được model
   điều gì mà COCO chưa dạy, và nó làm hỏng điều gì?
Tăng nhẹ +0.0055. Fine-tune giúp đặt khớp chính xác hơn một chút, nhưng recall không đổi → model không học thêm cách tìm người mới, chỉ tinh chỉnh vị trí khớp. Đổi lại, box_mAP50-95 giảm 0.0078 — có dấu hiệu overfit nhẹ.

3. `box_mAP` và `pose_mAP` chênh nhau bao nhiêu? Model tìm *người* dễ hơn hay tìm
   *khớp* dễ hơn? Vì sao?
Chênh 0.1133 (0.8041 vs 0.6908). Model tìm người dễ hơn tìm khớp — vì box chỉ cần 1 vùng chữ nhật, còn pose phải định vị 17 điểm nhỏ, dễ bị che khuất (cổ tay, mắt cá, đầu gối).

5. Một ảnh test model đoán sai - gọi tên lỗi theo bốn loại của slide 43
   (lệch nhẹ / đảo trái/phải / nhầm người / trượt hẳn):
train_13.jpg — lỗi đảo trái/phải: finding ghi "Đổi lại toàn bộ cặp trái/phải thì OKS tăng hẳn", OKS chỉ 0.7948 (thấp nhì dataset). Ngoài ra train_04.jpg có lỗi nhầm người (left_wrist gán sang người khác, OKS 0.8207).

7. Ảnh nào có OKS thấp nhất giữa nhãn của bạn và model? Ai đúng, và bạn dựa vào đâu?
train_13.jpg, gold_person 1, your_person 3, OKS = 0.7948. Gold đúng — thuật toán phát hiện đảo trái/phải skeleton. Ảnh có 3 người nên dễ nhầm thứ tự và hướng cơ thể.

9. Ảnh bạn gán tệ nhất có *cũng* là ảnh model đoán tệ nhất không? Nếu có, điều đó
   nói gì về bức ảnh đó?
Không xác định được từ file hiện có (file chỉ so nhãn bạn vs gold, không so model vs gold). Nếu trùng, ảnh đó khó với cả người lẫn máy; nếu không, lỗi nằm ở phía nhãn của bạn.

## 5. Một rule evidence bạn đã dùng

Chọn một keypoint trong ảnh core mà bạn phải quyết định giữa `v=1` và `v=0`. Nêu ảnh, người,
khớp, bằng chứng nhìn thấy và lý do chọn trạng thái đó trong 3-5 câu.

<!-- Cấu trúc gợi ý: (1) train_XX + người thứ mấy + keypoint; (2) căn cứ thị giác như phần cơ
thể liền kề, trang phục hoặc vật che; (3) vì sao khớp còn trong khung (v=1) hay đã ra khỏi
khung (v=0). -->
train_01.jpg, người thứ 2, khớp right_wrist — chọn v=1.
Cổ tay phải còn trong khung nhưng bị che một phần bởi cơ thể liền kề. Theo luật lớp: khớp bị che mà còn trong khung → giữ v=1 và vẫn đặt chấm (có thể nội suy từ khuỷu tay + hướng cẳng tay). Sai số 28 px = 1.2× dung sai, chấp nhận được, không trừ OKS.
