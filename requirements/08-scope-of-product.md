# 8. The Scope of the Product — Phạm vi sản phẩm

> **Volere v20 §8** · Đặc tả Yêu cầu HW-SNR-26
> **Trạng thái:** 🟡 Đang soạn
> **Phụ trách:** BA / Kỹ sư hệ thống
> **Cập nhật:** 2026-08-25

## Mục đích phần này (theo Volere)

**8a** Product boundary (ranh giới sản phẩm — cái gì tự động hoá, cái gì để con người làm) · **8b** Product Use Case table (**PUC**) · **8c** Individual product use cases.

---

## 8b. Bảng Product Use Case

Bốn chế độ vận hành CĐ-1…CĐ-4 (`../PROJECT-CHARTER.md` §3.5) ánh xạ gần như trực tiếp
thành PUC. Đây là **trục đánh số mà mọi atomic requirement sẽ tham chiếu tới** (cột
*Product Use Case Number* trong `atomic-requirements.csv`).

| PUC # | Tên | Actor | BUC liên quan | Trạng thái |
|---|---|---|---|---|
| PUC-1 | Cấu hình và triển khai phao | Kỹ thuật viên triển khai | BUC-1 | 🔴 chưa viết |
| PUC-2 | Giám sát thu thập thời gian thực (CĐ-1) | Trắc thủ | BUC-1 | 🟡 |
| PUC-3 | Nghe kênh âm thanh trực tiếp | Trắc thủ | BUC-1 | 🟡 |
| PUC-4 | Phát lại dữ liệu đã ghi (CĐ-2) | Trắc thủ / Nhà phân tích | BUC-1 | 🟡 |
| PUC-5 | Định vị sự kiện đa phao (CĐ-3) | Nhà phân tích | BUC-4 | 🔴 tuỳ chọn |
| PUC-6 | Xử lý hậu kỳ dữ liệu thô (CĐ-4) | Nhà phân tích | BUC-1 | 🟡 |
| PUC-7 | Thu hồi phao và trích xuất dữ liệu | Kỹ thuật viên triển khai | BUC-1 | 🔴 |
| PUC-8 | Hiệu chuẩn và kiểm tra hệ thống | Kỹ sư hiệu chuẩn | BUC-2 | 🔴 |
| PUC-9 | Quản lý thư viện chữ ký | Nhà phân tích | BUC-3 | 🔴 |

> **Quy tắc đánh số:** PUC number ở đây là khoá liên kết cho toàn bộ đặc tả.
> Không đổi số đã cấp; nếu bỏ một PUC thì để trống số đó, không tái sử dụng.

## 8a. Ranh giới sản phẩm — câu hỏi chưa trả lời

- Việc **lập kế hoạch lộ trình chạy tàu** có nằm trong sản phẩm không, hay do người dùng tự làm ngoài?
- Việc **dựng báo cáo cuối** là sản phẩm sinh tự động hay nhà phân tích tự soạn?
- **Quản lý thư viện chữ ký** là chức năng của sản phẩm này hay một hệ thống riêng?

> Ba câu hỏi trên quyết định khối lượng công việc phần mềm. Cần Chief Engineer trả lời trước G1.

