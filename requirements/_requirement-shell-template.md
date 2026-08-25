# Mẫu Atomic Requirement (Volere Shell)

Sao chép khối dưới đây cho mỗi yêu cầu mới. 16 trường theo đúng
`../reference/Volere Atomic Requirements Stationery.xls`.

Bản ghi chính thức nằm ở `atomic-requirements.csv`. File này chỉ là mẫu để soạn thảo
và để giải thích ý nghĩa từng trường.

---

```
┌─────────────────────────────────────────────────────────────────────┐
│ Requirement #:              │ Requirement Type:                     │
│ (số duy nhất, không tái sử  │ (9 Functional / 10 Look and Feel /     │
│  dụng số đã bỏ)             │  11 Usability / 12 Performance /        │
│                             │  13 Operational / 14 Maintainability /  │
│                             │  15 Security / 16 Cultural /            │
│                             │  17 Compliance)                         │
├─────────────────────────────┼───────────────────────────────────────┤
│ Product Use Case (PUC) #:   │ Related BUC:                          │
│ (PUC mà yêu cầu này thuộc)  │ (Business Use Case nguồn gốc)         │
├─────────────────────────────┴───────────────────────────────────────┤
│ Description:                                                        │
│   MỘT câu. Nói sản phẩm PHẢI LÀM GÌ, không nói làm thế nào.         │
│   Nếu cần chữ "và" để nối hai hành động → tách thành hai yêu cầu.   │
├─────────────────────────────────────────────────────────────────────┤
│ Rationale:                                                          │
│   VÌ SAO yêu cầu này quan trọng. Không có lý do chính đáng thì       │
│   nhiều khả năng đây không phải yêu cầu thật.                       │
├─────────────────────────────────────────────────────────────────────┤
│ Fit Criterion:                                                      │
│   ⭐ TRƯỜNG QUAN TRỌNG NHẤT. Phép đo cho phép kiểm chứng khách       │
│   quan rằng giải pháp đã thoả yêu cầu. Phải định lượng.             │
│   "Hệ thống phải nhanh" ✗                                           │
│   "Kết quả hiển thị trong vòng 1.5 s kể từ khi thu mẫu" ✓           │
│   Nếu không viết được fit criterion → yêu cầu còn mơ hồ, chưa xong. │
├──────────────────────────────┬──────────────────────────────────────┤
│ Customer Satisfaction: 1..5  │ Customer Dissatisfaction: 1..5       │
│ (lợi ích nếu ĐẠT)            │ (thiệt hại nếu KHÔNG đạt)            │
│  Sat thấp + Dissat cao = yêu cầu nền, bắt buộc nhưng không gây ấn   │
│  tượng. Sat cao + Dissat thấp = điểm tạo khác biệt.                 │
├──────────────────────────────┴──────────────────────────────────────┤
│ Priority:  H / M / L   (thống nhất một thang - xem QĐ-15)            │
├─────────────────────────────────────────────────────────────────────┤
│ Conflicts:                                                          │
│   Yêu cầu nào không thể cùng thoả với yêu cầu này.                  │
│   Ví dụ đang có: 9.0 (kênh nghe tương tự) ↔ 13.0 (mã hoá).          │
├─────────────────────────────────────────────────────────────────────┤
│ Originator:              │ Other Interested Stakeholders:           │
│ (ai nêu ra yêu cầu này)  │ (ai còn quan tâm tới nó)                 │
├──────────────────────────┴──────────────────────────────────────────┤
│ Other Related PUCs:                                                 │
│ Supporting Materials:  (tài liệu, tiêu chuẩn, phép đo tham chiếu)   │
│ History:               (ngày tạo, ngày sửa, lý do sửa)              │
└─────────────────────────────────────────────────────────────────────┘
```

---

## Ba lỗi hay gặp

**1. Yêu cầu không nguyên tử.**
"Sản phẩm phải ghi dữ liệu và truyền về trạm" → hai yêu cầu, kiểm thử riêng, có thể
ưu tiên khác nhau. Tách ra.

**2. Fit criterion không đo được.**
"Giao diện phải dễ dùng" không kiểm chứng được. Viết lại: "Trắc thủ chưa qua đào tạo
hoàn thành được một chu trình đo trong X phút với không quá Y lỗi thao tác."

**3. Mô tả lẫn giải pháp.**
"Sản phẩm phải dùng ADC 16 bit" là **giải pháp**, không phải yêu cầu. Yêu cầu thật là
"phải phủ dải động từ ... đến ...". Số bit là cách đạt được. Giải pháp thuộc §27.
