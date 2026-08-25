# 6. The Scope of the Work — Phạm vi công việc

> **Volere v20 §6** · Đặc tả Yêu cầu HW-SNR-26
> **Trạng thái:** 🔴 Trống
> **Phụ trách:** BA / Kỹ sư hệ thống
> **Cập nhật:** 2026-08-25

## Mục đích phần này (theo Volere)

**6a** Current situation · **6b** Context of the work (sơ đồ ngữ cảnh: hệ thống lân cận + luồng dữ liệu vào/ra) · **6c** Work partitioning (danh sách **Business Use Case — BUC**) · **6d** Specifying a business use case.

---

## 6b. Sơ đồ ngữ cảnh — cần vẽ

Phải thể hiện: hệ thống HW-SNR-26 ở giữa, và mọi thực thể bên ngoài trao đổi dữ liệu với nó.
Ứng viên đã biết:

```
   [Tàu / vật thể cần đo]  ──âm thanh──►  ┌──────────────┐
   [Vệ tinh GNSS]          ──vị trí,     │              │
                              thời gian──►│  HW-SNR-26   │──►[Báo cáo chữ ký]
   [Trắc thủ]               ──lệnh──────►│              │──►[Thư viện chữ ký]
   [Phòng hiệu chuẩn]       ──hệ số─────►│              │──►[Hệ thống bên thứ ba]
   [Môi trường biển]        ──nhiễu─────►└──────────────┘
```

> Sơ đồ trên là **bản nháp**. Cần rà: còn thực thể ngoài nào chưa liệt kê?
> (Ví dụ: cơ quan cấp phép tần số? hệ thống lưu trữ của khách hàng?)

## 6c. Business Use Cases (BUC)

| BUC # | Tên | Sự kiện kích hoạt | Trạng thái |
|---|---|---|---|
| BUC-1 | Thực hiện một chiến dịch đo chữ ký tàu | Có yêu cầu đo | 🔴 chưa viết |
| BUC-2 | Hiệu chuẩn lại hệ thống định kỳ | Đến hạn hiệu chuẩn | 🔴 |
| BUC-3 | Tra cứu / đối chiếu thư viện chữ ký | Có nhu cầu phân tích | 🔴 |
| BUC-4 | Xác định vị trí nguồn sóng xung kích | Có sự kiện cần định vị | 🔴 tuỳ chọn |

> BUC mô tả **công việc nghiệp vụ**, chưa phải sản phẩm. Chu trình 6 pha P1–P6 ở
> `../PROJECT-CHARTER.md` §3.4 chính là nội dung của BUC-1 — chuyển thể sang mẫu BUC.

