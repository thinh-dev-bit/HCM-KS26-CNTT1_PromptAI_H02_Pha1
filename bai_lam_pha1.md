# BÀI LÀM PHA 1 

- **Họ tên:** Trịnh Phú Thịnh
- **Lớp:** HCM-KS26-CNTT1
- **Link repository Pha1:** 

> Pha 1 (70 phút đầu, KHÔNG dùng AI). Viết xong, kéo thả file này vào repository Pha1 trước mốc giờ giám thị công bố.

## 4.1. Bảo vệ dữ liệu và chống ảo giác

### a) Ghi chú đã ẩn danh hoá

**GHI CHU DA AN DANH**

```text
Khán giả: [Ten_Khan_Gia]
Số điện thoại: [SDT_Khan_Gia]
Số CCCD: [CCCD_Ghan_Gia]
Email: [Email_Khan_Gia]
Thông tin đặt vé: Đặt 3 vé gói SVT trận bán kết Cúp Sao Vàng ngày 18/12/2026 cho 2 người lớn và 1 trẻ em 9 tuổi. 
Tình trạng thanh toán: Khán giả đã chuyển khoản đặt cọc 1.500.000 đồng. 
Yêu cầu của khán giả: Khán giả yêu cầu 3 ghế ngồi cạnh nhau gần lối đi.

```

### b) Hai câu lệnh chống ảo giác

**CAU LENH**

```text
Yêu cầu:
- Chỉ đưa ra các câu trả lời dựa trên file đính kèm đã được cung cấp. Nếu gặp phải 1 câu hỏi mà không có thông tin tham khảo trong file thì phải phản hồi lại cho khách hàng: "Vui lòng đợi xác minh..." và ngay lập tức báo về bộ phận vận hành để bổ sung thông tin chứ không tự cải biên, tạo mới câu trả lời cho khách hàng.
- Mỗi phản hồi, câu trả lời đều phải ghi rõ câu trả lời lấy từ mục hoặc điều khoản nào trong file ở cuối phản hồi, câu trả lời.
```

## 4.2. Trích xuất tin nhắn đặt vé ra JSON

**PROMPT**

```text
# Prompt

## C - Context:
Bạn là nhân viên mới của Ban tổ chức giải bóng đá Cúp Sao Vàng, phụ trách hỗ trợ khán giả mua vé các trận đấu. 

## R - Request:
Trách nhiệm của bạn là xử lý thông tin khán giả, tin nhắn đặt vé, báo giá và khiếu nại một cách an toàn, chính xác.

## E - Explanation:
BẢNG MÃ GÓI
SVT – Sân Vận Động: vé trận đấu
FZ – Fan Zone: khu cổ vũ
VIP – Khán đài VIP

Phản hồi đưa ra phải tuân thủ QUY TẮC CHUẨN HOÁ (5 quy tắc):
1. ma_goi lấy theo bảng mã gói.
2. ngay_su_kien dạng YYYY-MM-DD; tin nhắn không ghi năm thì lấy năm 2026; không có ngày cụ thể thì null.
3. Khách không nhắc trẻ em thì tre_em là 0 và mọi khách là người lớn.
4. loai_ve chỉ nhận "thuong", "vip" hoặc null.
5. ghi_chu chỉ ghi yêu cầu đặc biệt về chỗ ngồi, hỗ trợ di chuyển hoặc ăn uống; không có thì null.

## A - Action:
Thực hiện chuyển TN1, TN2, TN3 thành một mảng JSON 3 đối tượng, đúng thứ tự, đúng 6 trường như tin nhắn mẫu

<vi-du>

Đặt giúp mình 2 vé VIP trận chung kết ngày 15/11/2026, 2 người lớn và 1 bé. Cần chỗ ngồi cạnh lối đi.
Kết quả đúng:
{"ma_goi": "VIP", "ngay_su_kien": "2026-11-15", "nguoi_lon": 2, "tre_em": 1, "loai_ve": "vip", "ghi_chu": "chỗ ngồi cạnh lối đi"}

Tin nhắn mẫu 2:
Cho mình một vé Fan Zone, ngày nào cũng được, mình đi một mình.
Kết quả đúng:
{"ma_goi": "FZ", "ngay_su_kien": null, "nguoi_lon": 1, "tre_em": 0, "loai_ve": "thuong", "ghi_chu": null}
</vi-du>

## T - Tone:
Giọng điệu chuyên nghiệp, chuẩn mực

## E - Extras:
Đầu ra phải là một mảng JSON 3 đối tượng, đúng thứ tự, đúng 6 trường yêu cầu, không chào hỏi, không giải thích.

# Dũ liệu cần xử lý:
<tin-nhan>

TN1: Mình muốn đặt gói Sân Vận Động trận tứ kết ngày 20/12 nha, 2 người lớn với 1 bé 8 tuổi, vé thường.

TN2: Cho 3 người đi Fan Zone trận ngày 05/01/2027 ạ. Một người cần hỗ trợ xe lăn.

TN3: Trận chung kết Cúp Sao Vàng tháng 12 còn vé không ạ? Nhà em 4 người lớn, có một người khiếm thị cần hỗ trợ tìm chỗ, lấy vé VIP.

</tin-nhan>
```

## 4.3. Báo giá vé bằng Chain-of-Thought

### a) Prompt

**PROMPT**

```text
# Prompt

## C - Context:
Bạn là nhân viên mới của Ban tổ chức giải bóng đá Cúp Sao Vàng, phụ trách hỗ trợ khán giả mua vé các trận đấu. 

## R - Request:
Trách nhiệm của bạn là xử lý thông tin khán giả, tin nhắn đặt vé, báo giá và khiếu nại một cách an toàn, chính xác.

## E - Explanation:
<bang-gia>
BẢNG GIÁ VÉ (đồng/người lớn)
SVT – Sân Vận Động: 1.200.000
FZ – Fan Zone: 800.000
VIP – Khán đài VIP: 2.500.000
</bang-gia>

<quy-tac>
QUY TẮC TÍNH GIÁ
1. Trẻ em từ 5 đến 11 tuổi tính 70% giá người lớn. Trẻ dưới 5 tuổi miễn phí.
2. Nhóm từ 5 người lớn trở lên được giảm 5% trên tiền vé (tiền người lớn + tiền trẻ em).
3. Phụ thu khu ghế ưu tiên: 300.000 đồng/người. Phụ thu không được giảm giá.
4. Tổng tiền = tiền vé sau giảm + phụ thu
</quy-tac>

## A - Action:
Thực hiện tính toán suy luận từng bước theo thứ tự tiền người lớn → tiền trẻ em → tiền vé → giảm giá nhóm → phụ thu → tổng tiền, mỗi bước ghi phép tính cụ thể

## T - Tone:
Giọng điệu chuyên nghiệp, chuẩn mực

## E - Extras:
Dòng cuối có dạng [RESULT: <tổng tiền> đồng].

# Dữ liệu cần xử lý:
<yeu-cau-bao-gia>

Nhóm cổ động viên đi xem trận bán kết, mua gói SVT, gồm 5 người lớn, 2 trẻ em (8 tuổi và 10 tuổi) và 1 bé 3 tuổi. Có 1 người lớn chọn khu ghế ưu tiên.

</yeu-cau-bao-gia>
```

## 4.4. Dây chuyền xử lý khiếu nại (Prompt Chaining)

### a) Prompt Trạm 1

**PROMPT**

```text
# Prompt

## C - Context:
Bạn là nhân viên mới của Ban tổ chức giải bóng đá Cúp Sao Vàng, phụ trách hỗ trợ khán giả mua vé các trận đấu. 

## R - Request:
Trách nhiệm của bạn là xử lý thông tin khán giả, tin nhắn đặt vé, báo giá và khiếu nại một cách an toàn, chính xác.

## E - Explanation:
<chinh-sach-den-bu>
CHÍNH SÁCH ĐỀN BÙ CỦA BAN TỔ CHỨC CÚP SAO VÀNG

- tran_gio: trận đấu bắt đầu trễ trên 2 giờ do ban tổ chức: tặng voucher 200.000 đồng/khách.
- sai_khu_vuc: khán giả bị xếp sang khu vực thấp hơn quyền lợi đã cam kết: hoàn 500.000 đồng/khách cho mỗi hiệp bị ảnh hưởng.
- nhan_vien: khiếu nại về thái độ nhân viên: tặng voucher 300.000 đồng/khách.
- khac: chuyển bộ phận Chăm sóc khán giả xem xét, chưa đề xuất mức đền bù.
- Tổng mức đền bù trên 1.500.000 đồng phải được Trưởng ban Chăm sóc khán giả duyệt trước khi gửi khách.
<chinh-sach-den-bu>

## A - Action:
Đọc email, trả về chỉ một đối tượng JSON gồm đúng 4 trường:
{"loai_su_co": một trong "tran_gio" | "sai_khu_vuc" | "nhan_vien" | "khac", "so_khach": số nguyên, "so_hiep_bi_anh_huong": số nguyên hoặc null, "tom_tat": "không quá 20 từ"}

## T - Tone:
Giọng điệu chuyên nghiệp, chuẩn mực

## E - Extras:
Đầu ra phải là một file JSON đáp ứng đủ các yêu cầu

# Dũ liệu cần xử lý:
<emial-khieu-nai>

Tôi là khán giả mua vé gói SVT trận Cúp Sao Vàng ngày 02/11/2026, đi 2 người lớn. Ban tổ chức cam kết khu khán đài có mái che nhưng trong 2 hiệp đầu chúng tôi bị xếp sang khu không mái, trời mưa và ghế ướt. Nhân viên soát vé rất nhiệt tình, tôi không có gì phàn nàn về nhân viên. Tôi đề nghị ban tổ chức giải quyết thỏa đáng.


</email-khieu-nai>

```

### b) Hai điều cần kiểm tra ở JSON đầu ra của Trạm 1

1. Cấu trúc và kiểu dữ liệu hợp lệ

2. Số liệu khớp với email gốc và nhất quán với loại sự cố

### c) Câu lệnh viết tiếp

**CAU LENH**

```text
Dò lại nội dung tin nhắn bị đứt gãy và tiếp tục giũ giọng điệu chuyên nghiệp, chuẩn mực. Hãy tiếp tục viết tiếp nội dung của tin nhắn phản hồi từ "…Ban tổ chức Cúp Sao Vàng xin hoàn lại Quý khán giả khoản" cho đến hết nội dung phản hồi.
```

## 4.5. Prompt chấm bài theo Rubric

**PROMPT**

```text
(Viết prompt vào đây – xoá dòng này)
```

## 4.6. Hoàn thiện System Prompt

> Phần Vai trò và Chuẩn đầu ra đã cho sẵn. Viết tiếp phần Quy tắc ràng buộc và Xử lý ngoại lệ.

```text
[Vai trò]
Bạn là "Trợ lý Cúp Sao Vàng", nhân viên tư vấn vé và dịch vụ trên website của giải bóng đá Cúp Sao Vàng, giúp khán giả chọn và đặt một trong 3 gói: SVT (Sân Vận Động – vé trận đấu), FZ (Fan Zone – khu cổ vũ), VIP (Khán đài VIP).

[Quy tắc ràng buộc]
1. (Viết quy tắc 1 – xoá dòng này)
2.
3.

[Chuẩn đầu ra]
Trả lời bằng tiếng Việt, lịch sự, tối đa 120 từ. Giá vé trình bày dạng gạch đầu dòng. Kết thúc bằng một câu hỏi về nhu cầu tiếp theo của khách.

[Xử lý ngoại lệ]
- Khách hỏi ngoài phạm vi: (Viết cách trả lời – xoá dòng này)
- Khách cung cấp thiếu thông tin:
```
