# Báo cáo cá nhân — Ngày 2 Object Detection

Đổi tên file theo `MSSV_HoTen.md`, ví dụ `20241234_NguyenVanAn.md`. Điền kết quả thật, xóa dòng hướng dẫn khi nộp. Xem [quy định nộp](../../SUBMISSION.md).

## 1. Thông tin học viên

- Họ tên: NGUYỄN HỒNG SƠN
- MSSV: 03098
- Email: 0917077890son@gmail.com
- Lớp: 2A
- Link repo bài làm: https://github.com/Shiru-Shiru/K5-L2A-DAY02-NguyenHongSon-03098-ObjectDetection.git
- Link Colab đã chạy: https://colab.research.google.com/github/Shiru-Shiru/K5-L2A-DAY02-NguyenHongSon-03098-ObjectDetection/blob/main/notebooks/day2_annotation_to_yolo.ipynb

## 2. Schema và mini guideline

Dẫn tới [GUIDELINE_MINI.md](../../GUIDELINE_MINI.md). Tóm tắt ba ca mơ hồ đã quyết định, cách áp dụng schema và phần cần giảng viên làm rõ.

## 3. Annotation và self-QC

- Số file nhãn train: 50
- Kết quả checker và lỗi đã xử lý: các xe bị che chưa label
- Danh sách ít nhất 10 ảnh đã rà bằng mắt: drive_001, drive_002, drive_003, drive_004, drive_005, drive_006, drive_007, drive_008, drive_009, drive_010.
- Lỗi bỏ sót/box/class phát hiện khi rà và cách sửa: 2
- Commit khóa nhãn ban đầu: Labeled Data

## 4. Gold QC và rework

| Chỉ số | Trước sửa | Sau sửa nếu có |
|---|---:|---:|
| TP | 442 | 442 |
| FP | 490 | 490 |
| FN | 211 | 211 |
| Precision | 0.4742 | 0.4742 |
| Recall | 0.6769 | 0.6769 |
| F1 | 0.5577 | 0.5577 |
| mean_iou_of_matches | 0.8055 | 0.8055 |
| passed | true | true |

Dẫn tới `outputs/annotation_evaluation.json` trong repo. Nếu rework, ghi ảnh, lỗi đã kiểm, quyết định sửa/giữ và lý do. Không suy nguyên nhân chỉ từ FP/FN. Không có rework thì ghi rõ.

## 5. Kết quả YOLO

- Runtime, GPU/CPU, phiên bản Ultralytics và Torch: 
 - GPU: Tesla T4 (14913MiB), Ultralytics: 8.4.208, Torch: 2.14.1+cu130 (CUDA:0)
- Các tham số thay đổi so với notebook nếu có và lý do: Không thay đổi.
- Đường dẫn best checkpoint trong phiên chạy: /content/Day2-Lab/outputs/yolo26_runs/vehicle_annotation/weights/best.pt

| Chỉ số trên 10 ảnh eval | Giá trị |
|---|---:|
| Precision | 0.411 |
| Recall | 0.230 |
| mAP50 | 0.272 |
| mAP50–95 | 0.202 |

Đặt cả 2 folder `assets/eval_predictions/` và `assets/yolo26_runs/` (giải nén từ `day2_colab_outputs.zip`) cạnh báo cáo và dẫn link tương đối. Không chỉ ghi số; chỉ ra ít nhất một ảnh và vật thể cụ thể.

Trả lời bốn câu hỏi cuối notebook:

1. Lớp nào recall thấp nhất? Ảnh nào minh họa?
2. Nếu mAP50 cao nhưng mAP50–95 thấp, cần kiểm điều gì trên box?
3. Lỗi class nào có thể liên quan guideline? Dữ kiện nào hỗ trợ nhận xét?
4. Vì sao không dùng 10 ảnh eval để train? Vai trò validation của chúng trong notebook là gì?

**1. Lớp nào có recall thấp nhất? Chỉ ra một ảnh eval minh họa.**
- **Lớp có recall thấp nhất:** Lớp `bus` có recall bằng 0 (Precision cũng bằng 0 và mAP50 cực kỳ thấp, chỉ đạt 0.00786).
- **Ảnh minh họa:** Trong ảnh `drive_053.jpg`, có thể thấy chiếc xe buýt rất lớn ở giữa đường nhưng mô hình dự đoán sai lệch hoặc gán nhãn chồng chéo thành `car`, dẫn đến không phát hiện đúng thực tế đối tượng `bus` nào.

**2. Nếu mAP50 cao nhưng mAP50-95 thấp, box đang gặp vấn đề gì?**
- Khi mAP50 cao nhưng mAP50-95 thấp, điều này có nghĩa là mô hình định vị được đúng vùng có đối tượng, nhưng độ chính xác của các đường biên bounding box đang gặp vấn đề.
- Các cạnh của bounding box dự đoán có thể bị lệch, quá rộng hoặc quá hẹp so với nhãn gốc (Ground Truth), làm cho chỉ số IoU giảm sâu khiến hiệu năng sụt giảm nghiêm trọng khi tăng ngưỡng kiểm tra từ 0.5 lên 0.95.

**3. Có lỗi truck/bus/van/car nào liên quan tới guideline không?**
- Có lỗi gán nhãn lẫn lộn rất rõ ràng liên quan đến định nghĩa loại phương tiện trong guideline:
  - Ở các ảnh khác như `drive_056.jpg` hay `drive_058.jpg`, các dòng xe van bị nhận diện sai thành `car`, cho thấy ranh giới phân loại giữa xe con (`car`) và các phương tiện vận tải lớn hơn (`van`) chưa được mô hình phân biệt tốt theo đúng guideline.

**4. Vì sao không dùng 10 ảnh eval để train dù chúng có nhãn?**
- **Tránh hiện tượng Overfitting:** Nếu đưa 10 ảnh eval này vào tập huấn luyện, mô hình sẽ học thuộc lòng các đặc trưng của chúng.
- **Đảm bảo đánh giá khách quan:** Tập eval đóng vai trò là thước đo độc lập để kiểm chứng khả năng tổng quát hóa của mô hình trên các dữ liệu mới mà nó chưa từng thấy trong quá trình tối ưu hóa trọng số.

## 6. Khó khăn và cách xử lý

| Bước/thời điểm | Lỗi quan sát | Cách đã thử | Kết quả/còn thiếu |
|---|---|---|---|
| | | | |

## 7. Tự đánh giá theo rubric

Đối chiếu [RUBRIC.md](../../RUBRIC.md): phần nào đã có kết quả, phần nào còn thiếu hoặc chưa kiểm được. Không tự nhận hoàn thành bước chưa chạy.
Đã chạy xong và có kết quả toàn bộ.
## 8. Công cụ AI và tài liệu tham khảo

Ghi công cụ đã dùng, dùng ở bước nào và phần đã tự kiểm tra; không dùng thì ghi rõ. Liệt kê nguồn ngoài repo được sử dụng. Không ghi mật khẩu/token hoặc đính kèm gold train.
