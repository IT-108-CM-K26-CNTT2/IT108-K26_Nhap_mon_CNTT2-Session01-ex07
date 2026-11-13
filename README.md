# Bài 7: Thiết kế Quy trình Che mờ (Data Masking) Khách VIP
## 3. Ràng buộc kỹ thuật**

\- Phải loại bỏ 100% PII trước khi gửi sang AI công cộng.

\- Không được làm mất ngữ cảnh xưng呼 của AI. Sử dụng nhãn thay thế Placeholders.

## 4. Yêu cầu đầu ra**

### Phần A (Masking Table):** Lập bảng 3 cột (*\*Thông tin PII thật | Nhãn che mờ Placeholder | Loại dữ liệu\**).

|Thông tin PII thật|Nhãn che mờ Placeholder|Loại dữ liệu|
|-----|-----|-----|
|Phạm Minh C|[TÊN_KH]|Họ tên|
|0988.111.222|[SĐT_KH]|Số điện thoại|
|minhc@gmail.com|[EMAIL_KH]|Email|
|Căn hộ 502 Chung cư Sunrise, Q7|[ĐỊA_CHỈ_KH]|Địa chỉ|

### Phần B (Masked Input):** Viết lại văn bản khiếu nại đã được thay thế 100% nhãn che mờ.

```
Tôi là [TÊN_KH] (SĐT: [SĐT_KH], Email: [EMAIL_KH], ở [ĐỊA_CHỈ_KH]). Ngày 20/09 tôi mua 2kg thịt bò Wagyu giá 4.500.000 VNĐ tại MartX District 7 nhưng bị giao chậm 3 tiếng làm hỏng tiệc gia đình. Tôi yêu cầu bồi thường 100%!
```

### Phần C (Prompt gửi AI):** Viết Prompt yêu cầu AI soạn thư xin lỗi dựa trên Masked Input.

```
Vai trò: Chuyên viên CSKH MartX.
Nhiệm vụ: Soạn thư xin lỗi gửi khách hàng VIP dựa trên nội dung khiếu nại đã che mờ:
"""
Tôi là [TÊN_KH] (SĐT: [SĐT_KH], Email: [EMAIL_KH], ở [ĐỊA_CHỈ_KH]). Ngày 20/09 tôi mua 2kg thịt bò Wagyu giá 4.500.000 VNĐ tại MartX District 7 nhưng bị giao chậm 3 tiếng làm hỏng tiệc gia đình. Tôi yêu cầu bồi thường 100%!
"""

Yêu cầu:
1. Giọng văn chân thành, lịch sự, đồng cảm. Xưng hô: "MartX" - "Quý khách", gọi khách là "Quý khách [TÊN_KH]".
2. Giữ NGUYÊN VĂN mọi nhãn trong dấu [ ], tuyệt đối không tự bịa thông tin.
3. Nêu đúng sự việc: ngày 20/09, 2kg thịt bò Wagyu, 4.500.000 VNĐ, MartX District 7, giao chậm 3 tiếng.
4. Không hứa bồi thường 100%; chỉ nêu đang xem xét phương án thỏa đáng và sẽ liên hệ lại qua [SĐT_KH] hoặc [EMAIL_KH] trong 24 giờ. Không hỏi thêm thông tin cá nhân.
5. Độ dài 150-200 từ, cấu trúc gồm: Tiêu đề, lời chào, xin lỗi, hướng xử lý, lời kết, chữ ký "Bộ phận Chăm sóc khách hàng MartX".

```

### Phần D (Văn bản De-mask):** Trình bày kết quả thư xin lỗi cuối cùng sau khi thay lại thông tin thật.
```
Gửi tới: minhc@gmail.com
Tiêu đề: Lời xin lỗi chân thành từ MartX về đơn hàng ngày 20/09

Kính gửi Quý khách Phạm Minh C,

MartX xin chân thành gửi lời xin lỗi đến Quý khách. Chúng tôi được biết đơn hàng 2kg thịt bò Wagyu trị giá 4.500.000 VNĐ mua tại MartX District 7 vào ngày 20/09 đã bị giao chậm 3 tiếng, làm ảnh hưởng đến buổi tiệc gia đình của Quý khách. Đây là trải nghiệm không xứng đáng với sự tin tưởng mà Quý khách dành cho chúng tôi.

MartX đang kiểm tra nguyên nhân chậm trễ và xem xét phương án bồi thường thỏa đáng cho trường hợp này. Chúng tôi sẽ liên hệ lại với Quý khách qua số điện thoại 0988.111.222 hoặc email này trong vòng 24 giờ tới.

Một lần nữa, MartX xin lỗi Quý khách vì sự bất tiện này và rất mong có cơ hội phục vụ Quý khách tốt hơn.

Trân trọng,
Bộ phận Chăm sóc khách hàng MartX
```
