# Mini guideline — bài làm cá nhân

> Điền nội dung này trước khi bắt đầu gán nhãn. Giữ lại các quyết định sau khi self-QC.

## Định nghĩa lớp

| Lớp | Định nghĩa của bạn | Ví dụ/điểm phân biệt |
| --- | --- | --- |
| `car` |Xe ô tô con (xe du lịch) chở được tối đa khoảng 9 người, thường có 4 bánh, thiết kế nhỏ gọn dùng cho gia đình hoặc cá nhân. | Kích thước nhỏ hơn truck/bus/van; không có thùng hàng lớn; không chở quá nhiều người. |
| `truck` |Xe tải chuyên dùng để vận chuyển hàng hóa, có thùng hàng lớn hoặc đầu kéo, trọng tải và kích thước lớn hơn car/van. |Xe tải nhẹ, tải nặng, xe đầu kéo container, xe ben. Điểm phân biệt: có thùng chở hàng riêng biệt hoặc cấu trúc dành cho hàng hóa, cabin tách rời thùng. |
| `bus` |Xe khách chở nhiều người (thường trên 9 chỗ), kích thước lớn, thiết kế để vận chuyển hành khách công cộng hoặc theo tuyến. |Xe buýt thành phố, xe khách liên tỉnh, xe coach. Điểm phân biệt: nhiều ghế ngồi/đứng, cửa lên xuống lớn, thân xe dài và cao hơn car/van. |
| `van` |Xe thương mại nhỏ (xe van) có khoang chở khách và/hoặc hàng hóa kín, thường dùng để chở nhóm người nhỏ hoặc giao hàng nhẹ; kích thước trung gian giữa car và truck/bus. | Điểm phân biệt: thân xe hình hộp, khoang hàng kín liền cabin (không có thùng hàng riêng như truck), chở ít người hơn bus.|

## Quy tắc chung

- Box bám sát vật thể nghĩa là: Đường bao (bounding box) bao quanh vật thể sao cho sát với biên ngoài cùng của vật thể — không để khoảng trống thừa đáng kể giữa box và vật thể, đồng thời không cắt vào phần thân của vật thể. Box cần bao trọn các bộ phận nhìn thấy được (bánh xe, gương, thùng hàng...) và mép box trùng với mép ngoài của vật thể ở mức tốt nhất có thể.
- Vật thể bị che khuất: Vật thể bị một vật khác (hoặc bộ phận khác) chắn trước làm mất một phần tầm nhìn, không quan sát được toàn bộ hình dạng. Khi gán nhãn, vẫn vẽ box bao trọn phần nhìn thấy và ước lượng phần bị che khuất nếu có thể; ghi chú mức độ che khuất (nhẹ/trung bình/nặng) theo hướng dẫn bộ dữ liệu.
- Vật thể bị cắt bởi rìa ảnh: Vật thể nằm ở mép ảnh nên một phần của nó nằm ngoài khung hình, không hiển thị đầy đủ. Box chỉ bao phần nhìn thấy trong ảnh; cần đánh dấu (flag) là vật thể bị cắt rìa để tránh nhầm với vật thể nhỏ hoặc bị che khuất.
- Vật thể rất nhỏ/mơ hồ: Vật thể có kích thước quá nhỏ (ví dụ chiếm vài pixel) hoặc hình ảnh không rõ nét (mờ, nhòe, thiếu sáng, độ phân giải thấp) khiến việc nhận dạng và vẽ box không chắc chắn. Cần áp dụng ngưỡng tối thiểu (ví dụ diện tích box hoặc kích thước cạnh) theo hướng dẫn; nếu dưới ngưỡng thì bỏ qua (ignore) hoặc gán nhãn không xác định tùy quy ước bộ dữ liệu.

## Ghi lại quyết định cho ít nhất ba ca mơ hồ

| Ảnh | Tình huống | Quyết định cuối | Lý do theo schema |
| --- | --- | --- | --- |
| `drive_002` | nhiều xe chồng chéo| vẫn gán nhãn | vẫn nhận ra trên 50% đặc điểm xe |
| `drive_004` | xe bị mờ lóa do ánh đèn| không gán nhãn | bị lóa gần như không phân biệt được xe|
| `drive_024` | xe chỉ xuất hiện 1 phần đuôi xe trên ảnh | không gán nhãn | phần xuất hiện chỉ có 1 mẩu không đến 20% chiếc xe |
