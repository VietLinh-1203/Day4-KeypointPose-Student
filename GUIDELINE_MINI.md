# Mini guideline - nhóm: ______  |  người gán: ___Đỗ Nguyễn Việt Linh___  |  ngày: ___16/09/2026___

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
| Hông của người mặc quần áo dài | điểm tiếp xúc giữa quần và áo, hình dáng | điểm hông ở người nằm ở vị trí ngăn cách đấy, kết hợp với trạng thái trong ảnh như ngồi, nhảy, đứng,... để phán đoán |
<img width="873" height="665" alt="image" src="https://github.com/user-attachments/assets/d0c32eeb-2031-4a03-ba11-862fd1ddfb7b" />

| Tai bị tóc hoặc mũ bảo hiểm che một phần | phán đoán vị trí, tick Occluded | tai bị object khác che lấp, không nhìn thấy |
<img width="933" height="667" alt="image" src="https://github.com/user-attachments/assets/b1f89704-59ba-4c51-90fa-c056468245ec" />

| Người bị cắt ở mép ảnh (chỉ thấy từ hông trở lên) | tick Outside |  vì nó không còn trong khung hình để phán đoán nữa |
<img width="1187" height="666" alt="image" src="https://github.com/user-attachments/assets/9768fe1b-f581-4cd0-bd79-58ca60c5c756" />

| Cổ tay nằm sau tay lái / sau thân mình | phán đoán vị trí, tick Occluded | trong ảnh không thấy, bị che mất thì phán đoán hướng của tay trong ảnh rồi tick Occluded |
<img width="603" height="666" alt="image" src="https://github.com/user-attachments/assets/d4423518-6f5c-4be8-9f4d-7afc1b49b39e" />

| Hai người chồng lên nhau | vẫn tạo 2 keypoint riêng biệt | vì dù có chồng lên nhau vẫn sẽ nhận là 2 người riêng biệt, ta gắn keypoint như bình thường, người bị che thì phán đoán vị trí rồi tick Occluded cho bộ phận bị che|
<img width="442" height="667" alt="image" src="https://github.com/user-attachments/assets/e0fafbcf-86dd-40e7-9580-ea18f7e84de3" />

| Người nhỏ đến mức nào thì không gán nữa | nhỏ đến mức không còn phân biệt được bộ phận | lúc đấy sẽ rất khó để gắn keypoint, không thể phán đoán hoặc quá nhỏ để phán đoán, các keypoint sẽ bị chồng lên nhau |

Với mỗi luật, chèn **một ảnh mẫu** (screenshot từ CVAT) thay vì chỉ viết một câu.
Slide 12 nói rõ: khớp không có bề mặt nhìn thấy được thì phải có ảnh mẫu, không phải
một câu văn chung chung.

## 3. Ba ca mơ hồ đã gặp (bắt buộc, ghi ít nhất 3)

### Ca 1 - ảnh `___train_02.jpg___`, người thứ `_1__`, khớp `___left_shoulder/right_shoulder___`

- Mơ hồ ở chỗ nào: left_shoulder/right_shoulder nằm ngược chiều so với hai mắt - dấu hiệu đảo trái/phải
- Bạn quyết thế nào: set keypoint theo hướng nhìn, thân người thực tế có trong ảnh
- Vì sao: nếu để máy soi hình dáng thì sẽ gặp cảnh báo, nhưng trong thực tế có nhiều trạng thái, hình dáng cử chỉ khác nhau ở người, nên nhìn vào thực tế ảnh
- Nếu người khác quyết ngược lại thì model học sai cái gì: model đang có thuật toán riêng và mặc định nếu keypoint thực tế khác thuật toán thì nó sẽ cảnh báo, nên phải train model từ những điểm khác đấy để model tốt hơn

### Ca 2 - ảnh `___train_04.jpg___`, người thứ `_1__`, khớp `___left/right knee và left/right ankle___`

- Mơ hồ ở chỗ nào: có 4 khớp v=0 trong khi cả người nằm gọn giữa ảnh. Khớp không ra khỏi khung được thì phải là v=1 (bị che, vẫn đặt chấm), không phải v=0
- Bạn quyết thế nào: giữ nguyên quyết định đặt khớp v=0
- Vì sao: thuật toán train không giống như thực tế, trong ảnh 4 phần từ knee đến ankle thực tế bị cắt mép ảnh
- Nếu người khác quyết ngược lại thì model học sai cái gì: giống như Ca 1, ta nên train model để nó đánh giá các trường hợp thông minh hơn

### Ca 3 - ảnh `___train_11.jpg___`, người thứ `_1__`, khớp `___left/right knee và left/right ankle___`

- Mơ hồ ở chỗ nào: có 4 khớp v=0 trong khi cả người nằm gọn giữa ảnh. Khớp không ra khỏi khung được thì phải là v=1 (bị che, vẫn đặt chấm), không phải v=0
- Bạn quyết thế nào: giữ nguyên quyết định đặt khớp v=0
- Vì sao: thuật toán train không giống như thực tế, trong ảnh 4 phần từ knee đến ankle thực tế bị cắt mép ảnh
- Nếu người khác quyết ngược lại thì model học sai cái gì: giống như Ca 1 và 2, ta nên train model để nó đánh giá các trường hợp thông minh hơn

## 4. Sau khi so visibility report với bạn cùng nhóm

- Khớp lệch `%v=1` nhiều nhất: `______` (bạn `___%` / họ `___%`)
- Nguyên nhân là **guideline chưa rõ** hay **một trong hai bên gán sai**:
- Luật mới bổ sung vào mục 2 sau khi thống nhất:
