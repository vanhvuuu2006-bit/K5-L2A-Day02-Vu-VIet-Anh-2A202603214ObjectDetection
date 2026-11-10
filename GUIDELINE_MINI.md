# Mini guideline — bài làm cá nhân

> Điền nội dung này trước khi bắt đầu gán nhãn. Giữ lại các quyết định sau khi self-QC.

## Định nghĩa lớp

| Lớp | Định nghĩa của bạn | Ví dụ/điểm phân biệt |
| --- | --- | --- |
| `car` |Các loại xe ô tô con từ 4 đến 7 chỗ chở người. |Xe sedan, hatchback, SUV, bán tải (pick-up) cỡ nhỏ dùng cho gia đình |
| `truck` |Các loại xe tải chở hàng hóa, có thùng xe tải riêng biệt. |Xe tải 2 trục trở lên, xe container, xe ben, xe tải chở hàng hạng nặng. |
| `bus` |Xe khách lớn hoặc xe buýt công cộng dùng để chở nhiều người  |Xe buýt nội đô, xe khách giường nằm, xe đưa đón học sinh/công nhân cỡ lớn. |
| `van` |Xe tải nhỏ dạng hộp vuông hoặc xe khách mini từ 9 đến 16 chỗ. |xe tải van chở hàng đô thị |

## Quy tắc chung

- Box bám sát vật thể nghĩa là: Khung bao (bounding box) phải khít tối đa vào rìa ngoài cùng của phương tiện (gồm cả gương, bánh xe), không được vẽ quá rộng thừa nhiều nền trắng hoặc quá hẹp làm cắt mất một phần xe
- Vật thể bị che khuất: Nếu xe bị che khuất một phần (bởi cây cối, cột đèn hoặc xe khác) nhưng vẫn nhận diện được loại xe dựa trên phần còn lại, vẫn phải vẽ box bao phủ toàn bộ vùng diện tích ước tính của xe đó
- Vật thể bị cắt bởi rìa ảnh: Vẽ khung bao ôm sát phần phương tiện còn xuất hiện trong ảnh, rìa của box sẽ trùng khít với đường biên của khung ảnh
- Vật thể rất nhỏ/mơ hồ: Bỏ qua (không gán nhãn) nếu phương tiện ở quá xa, kích thước nhỏ hơn 15x15 pixel hoặc bị nhòe mờ đến mức mắt người không thể phân biệt chính xác đó là loại xe nào 

## Ghi lại quyết định cho ít nhất ba ca mơ hồ

| Ảnh | Tình huống | Quyết định cuối | Lý do theo schema |
| --- | --- | --- | --- |
| `drive_003` |Chiếc xe màu trắng dáng hộp(car 3) di chuyển từ xa trong điều kiện tầm nhìn bị giảm do khoảng cách phối cảnh |nhãn van |Hệ thống phải trích xuất các đặc trưng hình học cốt lõi (đuôi vuông phẳng, tỷ lệ chiều cao/chiều rộng) và so sánh trực tiếp với các xe dòng car xung quanh để vượt qua sự mơ hồ về kích thước do khoảng cách xa gây ra |
| `drive_029` |Nhận diện ô tô ở khoảng cách cực xa bị suy giảm độ phân giải trầm trọng và bị hàng rào che khuất một phần. |nhãn car |Sử dụng tư duy logic về làn đường, hướng dòng chảy giao thông và kích thước tương đối với các phương tiện lân cận để bù đắp cho sự thiếu hụt nghiêm trọng về chi tiết hình học của vật thể. |
| `drive_041` |Nhận diện phương tiện lai giữa xe đô thị nhỏ và xe công trình, trong điều kiện nhiễu bóng phản chiếu từ mặt đường sũng nước |nhãn truck | bỏ qua các đặc trưng nhiễu ở đầu cabin (giống van/car) và hiện tượng phản chiếu mặt đường, tập trung quét cấu trúc hình học của thùng xe độc lập phía sau để phân loại đúng nhóm xe tải. |
