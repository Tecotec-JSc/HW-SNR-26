# 9. Functional Requirements — Yêu cầu chức năng

> **Volere v20 §9** · Đặc tả Yêu cầu HW-SNR-26 · **Vòng xoắn 1 — PoC**
> **Trạng thái:** 🟡 Đang soạn
> **Phụ trách:** Kỹ sư hệ thống
> **Cập nhật:** 2026-08-25

## Mục đích phần này (theo Volere)

Điều sản phẩm **phải làm**. Mỗi yêu cầu là một atomic requirement trong `atomic-requirements.csv` theo Volere shell 16 trường.

---

## Nguồn

10 yêu cầu chức năng CN-01…CN-10 tại `../PROJECT-CHARTER.md` §6.2, đã nạp vào
`atomic-requirements.csv`.

## Đã bỏ khỏi vòng 1

| Yêu cầu cũ | Vì sao bỏ |
|---|---|
| Kênh nghe tương tự thời gian thực | Không gỡ rủi ro PoC nào; tốn điện; từng xung đột với mã hoá |
| Mã hoá dữ liệu | Không gỡ rủi ro PoC nào |
| Cấu hình từ xa | Vòng 1 cấu hình trước khi thả |
| Phát lại đồng bộ dữ liệu + âm thanh | Phân tích hậu kỳ là đủ |
| Định vị nguồn đa phao | Cần nhiều phao |
| Xuất chuẩn cho bên thứ ba | Vòng 1 không bàn giao cho ai |
| **Chuẩn hoá theo cự ly** | **Không áp dụng được** — giám sát thụ động không biết cự ly |

> **Việc bỏ kênh nghe tương tự đã đóng luôn xung đột yêu cầu** ghi nhận ở phiên bản trước
> (kênh tương tự ↔ mã hoá). Hiện `atomic-requirements.csv` **không còn xung đột nào chưa giải**.

## Việc cần làm để chuyển 🟢

| Việc | Ghi chú |
|---|---|
| Viết fit criterion đo được cho mọi yêu cầu | Còn 1 dòng trống — yêu cầu 2.0 |
| Định nghĩa chặt "sự kiện" | Bắt đầu/kết thúc thế nào, ngưỡng nào — xem §4 |
| Rà tính nguyên tử | Tách yêu cầu nào còn gộp nhiều hành động |

