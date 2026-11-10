# Mini guideline — bài làm cá nhân

> Điền nội dung này trước khi bắt đầu gán nhãn. Giữ lại các quyết định sau khi self-QC.

## Định nghĩa lớp

| Lớp | Định nghĩa của bạn | Ví dụ/điểm phân biệt |
| --- | --- | --- |
| `car` | Xe cá nhân hoặc xe gia đình, thường có từ 4 đến 7 chỗ ngồi. | **Ví dụ:** Xe taxi.<br>**Phân biệt:** Kích thước nhỏ đến trung bình, gầm thấp, cửa sau mở ngang. |
| `truck` | Xe cơ giới chuyên dụng được thiết kế chủ yếu để vận chuyển hàng hóa. | **Ví dụ:** Xe tải thùng, xe ben, xe bán tải.<br>**Phân biệt:** Có thùng chứa hàng, phần cabin lái thường tách biệt rõ ràng với phần thùng chứa hàng phía sau. |
| `bus` | Xe thương mại kích thước lớn, được thiết kế chuyên dụng để chở số lượng lớn hành khách. | **Ví dụ:** Xe buýt công cộng, xe khách giường nằm, xe buýt trường học.<br>**Phân biệt:** Thân xe rất dài, trần cao, đặc trưng bởi dãy cửa sổ kính lớn chạy dọc hai bên hông xe. |
| `van` | Xe cỡ trung bình có thiết kế dạng hộp kín, có thể dùng để chở hành khách hoặc chở hàng hóa cỡ nhỏ. Khoang lái và khoang sau liền khối. | **Ví dụ:** Minivan, xe giao hàng, xe cứu thương.<br>**Phân biệt:** Thân hình hộp, thường trang bị cửa lùa bên hông. Nếu là van chở hàng sẽ bịt kín cửa sổ khoang sau. |

## Quy tắc chung

- **Box bám sát vật thể nghĩa là:** Các cạnh của bounding box phải tiếp xúc khít với các điểm ảnh ngoài cùng của vật thể. Tránh việc để thừa quá nhiều khoảng trống bên trong khung, nhưng đồng thời tuyệt đối không được vẽ cắt mất vào các bộ phận của vật thể.
- **Vật thể bị che khuất (Occlusion):** Chỉ vẽ box bao quanh phần vật thể mà mắt người thực sự nhìn thấy được. Nếu vật thể bị che khuất phần lớn dẫn đến việc mất quá nhiều đặc điểm nhận dạng, có thể hỏi lab coach.
- **Vật thể bị cắt bởi rìa ảnh (Truncation):** Vẽ box kéo dài sát đến mép của bức ảnh cho phần vật thể xuất hiện trong khung hình. Không vẽ box tràn ra bên ngoài rìa ảnh hoặc tự suy đoán để vẽ thêm phần bị cắt mất.
- **Vật thể rất nhỏ/mơ hồ:** Bỏ qua không gán nhãn nếu vật thể có kích thước quá nhỏ.

## Ghi lại quyết định cho ít nhất ba ca mơ hồ

| Ảnh | Tình huống | Quyết định cuối | Lý do theo schema |
| --- | --- | --- | --- |
| `drive_040.jpg` | Có 1 xe bị xe bus che chỉ nhìn thấy phần nóc. | Vẽ box bao quanh phần nhìn thấy. | Vẫn đủ khả năng nhận dạng loại xe. |
| `drive_035.jpg` | Nhiều xe đang đỗ bị che sau bến xe bus. | Bỏ qua | Không đủ đặc điểm nhận dạng. |
| `drive_038.jpg` | Chỉ hiện phần đầu xe ở mép ảnh. | Bỏ qua | Không đủ đặc điểm nhận dạng |
