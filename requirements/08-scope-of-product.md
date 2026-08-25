# 8. The Scope of the Product — Phạm vi sản phẩm

> **Volere v20 §8** · Đặc tả Yêu cầu HW-SNR-26 · **Vòng xoắn 1 — PoC**
> **Trạng thái:** 🟡 Đang soạn
> **Phụ trách:** Kỹ sư hệ thống
> **Cập nhật:** 2026-08-25

## Mục đích phần này (theo Volere)

**8a** Product boundary · **8b** Product Use Case table (PUC) · **8c** Individual PUCs.

---

## 8b. Product Use Case — vòng xoắn 1

Trục đánh số mà mọi atomic requirement tham chiếu tới.

| PUC # | Tên | Actor | BUC | Trạng thái |
|---|---|---|---|---|
| PUC-1 | Cấu hình và thả phao | Kỹ sư field | BUC-1 | 🟡 |
| PUC-2 | Thu nhận và ghi liên tục | (tự động) | BUC-1 | 🟡 |
| PUC-3 | Phát hiện sự kiện và báo về trạm | (tự động) | BUC-1 | 🟡 |
| PUC-4 | Giám sát thời gian thực tại trạm | Kỹ sư field | BUC-1 | 🟡 |
| PUC-5 | Thu hồi phao và trích xuất dữ liệu | Kỹ sư field | BUC-1 | 🟡 |
| PUC-6 | Phân tích hậu kỳ và so sánh cấu hình treo | Kỹ sư phân tích | BUC-2 | 🟡 |
| PUC-7 | Hiệu chuẩn và tự kiểm tra | Kỹ sư hiệu chuẩn | BUC-3 | 🟡 |

**Đã bỏ so với phiên bản trước:** nghe kênh âm thanh trực tiếp, phát lại đồng bộ, định vị
đa phao, quản lý thư viện chữ ký. Tất cả `[V2+]`.

## 8a. Ranh giới sản phẩm ở vòng 1

**Trong sản phẩm:** thu, ghi, phân tích phổ, phát hiện sự kiện, báo cáo trạng thái, trích
xuất dữ liệu, phân tích hậu kỳ cơ bản.

**Ngoài sản phẩm, do người làm:** lập kế hoạch chuyến biển, chọn vị trí thả, đối chiếu sự
kiện với quan sát bên ngoài, phán quyết đạt/không đạt, viết báo cáo vòng xoắn.

> Ở một PoC, ranh giới nên đặt **rộng về phía con người**. Tự động hoá thứ chưa hiểu rõ là
> cách chắc chắn để tốn thời gian mà không gỡ được rủi ro nào.

