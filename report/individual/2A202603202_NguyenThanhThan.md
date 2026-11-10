# Báo cáo cá nhân — Ngày 2 Object Detection

Đổi tên file theo `MSSV_HoTen.md`, ví dụ `20241234_NguyenVanAn.md`. Điền kết quả thật, xóa dòng hướng dẫn khi nộp. Xem [quy định nộp](../../SUBMISSION.md).

## 1. Thông tin học viên

- Họ tên: Nguyễn Thanh Thản
- MSSV: 2A202603202
- Email: npc12a1@gmail.com
- Lớp: L2A
- Link repo bài làm: https://github.com/thannt97/K5-L2A-DAY02-NguyenThanhThan-2A202603202-ObjectDetection
- Link Colab đã chạy: https://colab.research.google.com/drive/1Iay37gmo0vbMFSGg60Ds1RrE3VBa1RXP#scrollTo=8e715548

## 2. Schema và mini guideline

Dẫn tới [GUIDELINE_MINI.md](../../GUIDELINE_MINI.md). Tóm tắt ba ca mơ hồ đã quyết định, cách áp dụng schema và phần cần giảng viên làm rõ.

## 3. Annotation và self-QC

- Số file nhãn train: 50
- Kết quả checker và lỗi đã xử lý: box vượt biên ảnh, box trùng, box không sát
- Danh sách ít nhất 10 ảnh đã rà bằng mắt: drive_004, drive_002, drive_006, drive_0010, drive_005, drive_50, drive_40, drive_39, drive_15, drive_19
- Lỗi bỏ sót/box/class phát hiện khi rà và cách sửa: 
+ Box lỏng/Box bao rộng hơn vật thể, thừa khoảng trống/Siết box bám sát biên ngoài vật thể
+ Bỏ sót/Có xe trong ảnh nhưng không có box/Thêm box mới, gán đúng class
+ Khi rà ảnh drive_002 phát hiện nhiều xe chị chồng chéo không đủ nhận dạng nên đã bỏ gán nhãn
- Commit khóa nhãn ban đầu: b2ccfdf1be4ed28b817db1e1e0a4385945e50aa4

## 4. Gold QC và rework

| Chỉ số | Trước sửa | Sau sửa nếu có |
|---|---:|---:|
| TP | | |
| FP | | |
| FN | | |
| Precision |0.66 |0.7764 |
| Recall |0.5 |0.9453 |
| F1 |0.52 | 0.8094|
| mean_iou_of_matches | | 0.9574|
| passed |false | true|

Dẫn tới `outputs/annotation_evaluation.json` trong repo. Nếu rework, ghi ảnh, lỗi đã kiểm, quyết định sửa/giữ và lý do. Không suy nguyên nhân chỉ từ FP/FN. Không có rework thì ghi rõ.

## 5. Kết quả YOLO

- Runtime, GPU/CPU, phiên bản Ultralytics và Torch: 37 minutes
- Các tham số thay đổi so với notebook nếu có và lý do:
- Đường dẫn best checkpoint trong phiên chạy: "/content/Day2-Lab/outputs/yolo26_runs/vehicle_annotation/weights/best.pt"

| Chỉ số trên 10 ảnh eval | Giá trị |
|---|---:|
| Precision | 0.696 |
| Recall |0.3756 |
| mAP50 |0.567 |
| mAP50–95 |0.3667 |

Đặt cả 2 folder `assets/eval_predictions/` và `assets/yolo26_runs/` (giải nén từ `day2_colab_outputs.zip`) cạnh báo cáo và dẫn link tương đối. Không chỉ ghi số; chỉ ra ít nhất một ảnh và vật thể cụ thể.

Trả lời bốn câu hỏi cuối notebook:

1. Lớp nào recall thấp nhất? Ảnh nào minh họa? 
- lớp bus có recall thấp nhất
- ảnh driver_52 không phát hiện ra xe bus 
2. Nếu mAP50 cao nhưng mAP50–95 thấp, cần kiểm điều gì trên box?
- Box quá rộng: Bao gồm nhiều phần nền xung quanh vật thể.
- Box quá hẹp: Bị cắt mất một phần xe.
- Box lệch vị trí: Tâm hoặc cạnh Box không khớp với vật thể.
- Box không nhất quán: Cùng một loại xe nhưng cách vẽ Box khác nhau giữa các ảnh.
3. Lỗi class nào có thể liên quan guideline? Dữ kiện nào hỗ trợ nhận xét?
- Bus có kết quả rất thấp, nhưng chỉ có 2 instances nên chưa đủ dữ liệu để kết luận nguyên nhân.
- Van có Precision = 1.0 nhưng Recall chỉ 0.115. Mô hình ít dự đoán sai khi đã dự đoán van, nhưng bỏ sót rất nhiều xe van.
- Truck có 5 instances, vẫn là số lượng nhỏ để đánh giá độ ổn định.
4. Vì sao không dùng 10 ảnh eval để train? Vai trò validation của chúng trong notebook là gì?
- Có 10 ảnh eval với 113 instances.
- Đây là tập dữ liệu dùng để đánh giá khả năng tổng quát hóa của mô hình.
- Nếu đưa 10 ảnh này vào training, mô hình có thể học thuộc các ảnh đó. Khi đánh giá lại trên chính chúng, kết quả không còn phản ánh khách quan khả năng nhận diện trên dữ liệu chưa thấy.
- Notebook gọi tập dữ liệu là eval, nhưng log sử dụng quy trình val của Ultralytics. Nếu tập này được giữ riêng và không dùng để điều chỉnh mô hình, nó có vai trò như tập test độc lập. Nếu liên tục điều chỉnh mô hình dựa trên kết quả này, nó đang được sử dụng như validation.

## 6. Khó khăn và cách xử lý

| Bước/thời điểm | Lỗi quan sát | Cách đã thử | Kết quả/còn thiếu |
|---|---|---|---|
| gán nhãn những chiếc xe bị chồng chéo | không thể xác định rõ % đặc điểm chiếc xe | gán nhãn thử và kiểm tra lại recall và Precision | chỉ matches được 1 phần sau khi tính toán |

## 7. Tự đánh giá theo rubric

Đối chiếu [RUBRIC.md](../../RUBRIC.md): phần nào đã có kết quả, phần nào còn thiếu hoặc chưa kiểm được. Không tự nhận hoàn thành bước chưa chạy.

## 8. Công cụ AI và tài liệu tham khảo

Ghi công cụ đã dùng, dùng ở bước nào và phần đã tự kiểm tra; không dùng thì ghi rõ. Liệt kê nguồn ngoài repo được sử dụng. Không ghi mật khẩu/token hoặc đính kèm gold train.
- chatGPT: hướng dẫn cài và sử dụng cvat, cài model yolov7
- deepseek: tham khảo kết quả tìm kiếm nhận dạng các loại xe
- claude: hỗ trợ trả lời là tóm tắt việc cần làm trong project
