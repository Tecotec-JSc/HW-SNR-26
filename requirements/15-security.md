# 15. Security Requirements — Yêu cầu an ninh

> **Volere v20 §15** · Đặc tả Yêu cầu HW-SNR-26
> **Trạng thái:** 🟡 Đang soạn
> **Phụ trách:** Chief Engineer
> **Cập nhật:** 2026-08-25

## Mục đích phần này (theo Volere)

**15a** Access · **15b** Integrity · **15c** Privacy · **15d** Audit · **15e** Immunity.

---

## Đã có

| Volere | Yêu cầu | Nguồn |
|---|---|---|
| 15b/15c | CN-06 — mã hoá dữ liệu lưu trong phao **và** dữ liệu truyền | Charter §6.2 |
| 15b | Toàn vẹn dữ liệu, phát hiện được hỏng (ĐT-A5-07) | SPEC §5.5 |

## Khoảng trống

| Mục | Câu hỏi | Mức độ |
|---|---|---|
| 15a | Ai được truy cập dữ liệu? Có phân quyền không? | 🔴 chưa xét |
| 15d | Có cần nhật ký kiểm toán (ai xem/sửa/xuất dữ liệu gì, khi nào)? | 🔴 chưa xét |
| 15e | Chống can thiệp vật lý khi phao trôi nổi không người trông? | 🔴 chưa xét |
| — | Nếu mất phao, dữ liệu có bị đọc được không? | 🟡 CN-06 giảm nhẹ |

> ⚠ **Xung đột đã biết:** CN-02 (kênh nghe tương tự) không mã hoá được → mâu thuẫn CN-06.
> Xem QĐ-11 và `09-functional.md`.

> ⚠ **15e — miễn nhiễm:** một phao đo thả trên biển là tài sản không người trông coi.
> Rủi ro không chỉ là mất thiết bị mà là **mất dữ liệu** và **lộ cấu hình hệ thống**.
> Cần quyết định mức bảo vệ trước khi thiết kế A5.

