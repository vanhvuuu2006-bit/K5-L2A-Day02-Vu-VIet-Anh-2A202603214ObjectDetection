# Báo cáo cá nhân — Ngày 2 Object Detection

Đổi tên file theo `MSSV_HoTen.md`, ví dụ `20241234_NguyenVanAn.md`. Điền kết quả thật, xóa dòng hướng dẫn khi nộp. Xem [quy định nộp](../../SUBMISSION.md).

## 1. Thông tin học viên

- Họ tên: Vũ Việt Anh
- MSSV: 2A202603214
- Email: vanhvuuu2006@gmail.com
- Lớp: K5-L2A
- Link repo bài làm: https://github.com/vanhvuuu2006-bit/K5-L2A-Day02-Vu-VIet-Anh-2A202603214ObjectDetection
- Link Colab đã chạy: https://colab.research.google.com/drive/1CDpd6-lJIpDd83zb-znxIff-amtkWQj6#scrollTo=6f78b880

## 2. Schema và mini guideline

Dẫn tới [GUIDELINE_MINI.md](../../GUIDELINE_MINI.md). Tóm tắt ba ca mơ hồ đã quyết định, cách áp dụng schema và phần cần giảng viên làm rõ.

## 3. Annotation và self-QC

- Số file nhãn train:
- Kết quả checker và lỗi đã xử lý:
- Danh sách ít nhất 10 ảnh đã rà bằng mắt:
- Lỗi bỏ sót/box/class phát hiện khi rà và cách sửa:
- Commit khóa nhãn ban đầu:

## 4. Gold QC và rework

| Chỉ số | Trước sửa | Sau sửa nếu có |
|---|---:|---:|
| TP |352 | |
| FP |194 | |
| FN |301 | |
| Precision |0.6447 | |
| Recall |0.5391 | |
| F1 |0.5872 | |
| mean_iou_of_matches |0.7834 | |
| passed |false | |

Dẫn tới `outputs/annotation_evaluation.json` trong repo. Nếu rework, ghi ảnh, lỗi đã kiểm, quyết định sửa/giữ và lý do. Không suy nguyên nhân chỉ từ FP/FN. Không có rework thì ghi rõ.

## 5. Kết quả YOLO

- Runtime, GPU/CPU, phiên bản Ultralytics và Torch:
- Các tham số thay đổi so với notebook nếu có và lý do:
- Đường dẫn best checkpoint trong phiên chạy:

| Chỉ số trên 10 ảnh eval | Giá trị |
|---|---:|
| Precision |0.45735154495224656 |
| Recall |0.24902482269503548 |
| mAP50 |0.2761970363279415 |
| mAP50–95 |0.17192449109655422 |

Đặt cả 2 folder `assets/eval_predictions/` và `assets/yolo26_runs/` (giải nén từ `day2_colab_outputs.zip`) cạnh báo cáo và dẫn link tương đối. Không chỉ ghi số; chỉ ra ít nhất một ảnh và vật thể cụ thể.

Trả lời bốn câu hỏi cuối notebook:

1. Lớp nào recall thấp nhất? Ảnh nào minh họa?
Lớp van là lớp có Recall thấp nhất trong tập dữ liệu này. `drive_060.jpg`. một chiếc xe tải cỡ lớn tiến sát camera và bị cắt mất thùng hàng, hệ thống gán nhãn tự động nhảy sang màu xanh ngọc (van) thay vì nhận diện đúng là truck. Sự nhập nhằng hình học giữa van và đầu cabin truck, cùng với việc lớp van có số lượng mẫu (sample size) ít nhất trong 4 lớp khiến mô hình rất dễ bỏ sót vật thể thực tế hoặc đoán nhầm sang lớp khác, dẫn đến tỷ lệ Recall (Khả năng tìm sót) của van bị kéo thấp nhất

2. Nếu mAP50 cao nhưng mAP50–95 thấp, cần kiểm điều gì trên box?
Khi chỉ số sụt giảm mạnh ở các ngưỡng IoU khắt khe (từ 0.50 đến 0.95), bạn cần thực hiện QA/QC (Kiểm định chất lượng) tập trung vào độ chính xác hình học của các bounding box thuộc tập dữ liệu với các điểm mấu chốt sau:
• Độ lỏng/chặt của Box (Loose/Tight Boxes): Kiểm tra xem người dán nhãn (annotator) hay mô hình có đang vẽ box quá rộng (thừa khoảng trống nền) hoặc quá khít (bị cắt mất gương xe, bánh xe hoặc các viền ngoài phương tiện) hay không. Một vài pixel lệch nhỏ ở các xe kích thước bé hoặc xe ở xa sẽ lập tức kéo tụt IoU xuống dưới 0.95.
• Quy ước dán nhãn vùng che khuất (Occlusion Handling): Kiểm tra lại quy định khi hai xe xếp nối đuôi chồng lên nhau (như hàng dài xe tại giao lộ trong ảnh). Người dán nhãn vẽ box bao quanh toàn bộ phần nhìn thấy hay vẽ ước lượng xuyên qua cả phần bị che. Sự không nhất quán này giữa tập Train và tập Eval khiến AI vẽ box bị lệch tâm.
• Hiện tượng dính box (Box Merging): Kiểm tra xem mô hình có xu hướng gộp 2 phương tiện chạy song song hoặc nối đuôi sát nhau thành một box lớn duy nhất hay không (thường gặp ở cụm xe con phía xa hoặc hàng xe sát dải phân cách).
• Box chứa cả bóng phản chiếu (Shadow/Reflection): Như ở ảnh mặt đường ướt sau mưa, cần kiểm tra xem các box dự đoán/nhãn chuẩn có vô tình bao cả bóng nước phản chiếu của xe dưới đường vào hay không, làm tăng kích thước chiều cao box một cách sai lệch.

3. Lỗi class nào có thể liên quan guideline? Dữ kiện nào hỗ trợ nhận xét?
• Lỗi hệ thống: Mô hình dự đoán nhầm hoặc dao động (flip nhãn) giữa car và truck.
• Dữ kiện hỗ trợ từ hình ảnh & Guideline:
	• Guideline định nghĩa: car bao gồm cả "bán tải (pick-up) cỡ nhỏ dùng cho gia đình", còn truck là xe "có thùng xe tải riêng biệt".
	• Mâu thuẫn thực tế: Về mặt thị giác máy tính, bản chất xe bán tải luôn luôn có một cái thùng hở phía sau tách biệt với cabin (kích hoạt đặc trưng hình học của lớp truck). Mô hình AI không thể có dữ kiện về "mục đích sử dụng" (cho gia đình hay chở hàng) nếu chỉ nhìn ảnh. Định nghĩa dựa trên mục đích sử dụng thay vì hình dáng thị giác là nguyên nhân trực tiếp khiến mô hình phân loại sai.


4. Vì sao không dùng 10 ảnh eval để train? Vai trò validation của chúng trong notebook là gì?
• Chống học vẹt (Prevent Overfitting): Mục tiêu của mạng nơ-ron là học các đặc trưng tổng quát (generalization) của dòng xe chứ không phải học thuộc lòng từng bức ảnh cụ thể. Nếu đưa 10 ảnh này vào tập huấn luyện, mô hình sẽ tối ưu hóa trọng số (weights) để khớp hoàn toàn với các pixel của 10 ảnh này.
• Ngăn chặn rò rỉ dữ liệu (Data Leakage): Việc "biết trước đề thi" khiến các chỉ số kiểm thử không còn giá trị. Mô hình có thể đạt độ chính xác 100% trên 10 ảnh này nhưng khi triển khai thực tế trên một camera giao thông ở tuyến đường khác, hệ thống sẽ mất hoàn toàn khả năng nhận diện.
Trong quy trình chạy và lập trình notebook (như định kỳ đánh giá sau mỗi epoch huấn luyện), 10 ảnh này đóng vai trò như một Hệ thống giám sát động và phát hiện lỗi sớm:
1. Cung cấp mốc đo lường khách quan (Unbiased Benchmark): Giúp tính toán chỉ số mAP (mAP50, mAP50-95) thực tế của mô hình sau mỗi chu kỳ học để thuật toán quyết định khi nào nên dừng (Early Stopping), tránh việc mô hình càng học càng tệ trên dữ liệu thực tế.
2. Bộ lọc bẫy Edge Case (Ca mơ hồ): 10 ảnh này được chọn lọc cố ý vì chúng chứa đầy đủ các thử thách giao thông khó nhất: xe chạy ban đêm (khung hình 1, 4), xe bị khuất một phần (khung hình 3, 6), xe ở quá xa (ảnh có CAR 327 MANUAL), mặt đường phản chiếu (ảnh trời mưa). Vai trò của chúng là ép mô hình phải lộ ra các điểm yếu về mặt thị giác (như nhầm nhãn, lệch box) ngay trong notebook, giúp kỹ sư biết chính xác cần phải bổ sung thêm dữ liệu dạng nào (Data Augmentation) vào tập Train ở bước tiếp theo.


## 6. Khó khăn và cách xử lý

| Bước/thời điểm | Lỗi quan sát | Cách đã thử | Kết quả/còn thiếu |
|---|---|---|---|
| | | | |

## 7. Tự đánh giá theo rubric

Đối chiếu [RUBRIC.md](../../RUBRIC.md): phần nào đã có kết quả, phần nào còn thiếu hoặc chưa kiểm được. Không tự nhận hoàn thành bước chưa chạy.

## 8. Công cụ AI và tài liệu tham khảo

Ghi công cụ đã dùng, dùng ở bước nào và phần đã tự kiểm tra; không dùng thì ghi rõ. Liệt kê nguồn ngoài repo được sử dụng. Không ghi mật khẩu/token hoặc đính kèm gold train.
