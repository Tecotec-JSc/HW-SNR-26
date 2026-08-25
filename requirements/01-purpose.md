# 1. The Purpose of the Project — Mục đích dự án

> **Volere v20 §1** · Đặc tả Yêu cầu HW-SNR-26 · **Vòng xoắn 1 — PoC**
> **Trạng thái:** 🟢 Đủ để review
> **Phụ trách:** Chief Engineer
> **Cập nhật:** 2026-08-25

## Mục đích phần này (theo Volere)

**1a** Bối cảnh nghiệp vụ · **1b** Mục tiêu dự án, phát biểu dưới dạng đo được.

---

## 1a. Bối cảnh

Đơn vị chưa có năng lực chế tạo phao giám sát thuỷ âm thụ động. Trước khi đầu tư vào một
hệ thống hoàn chỉnh, cần chứng minh được mệnh đề nền tảng: **có chế tạo được một phao thu
đủ yên tĩnh để nghe được tín hiệu quan tâm trong điều kiện biển thật hay không.**

Kiến trúc tham chiếu là Scanmatic TMK-SAS / SONRAS (`../reference/`) — mượn **kiến trúc**
(phao + trạm điều khiển + GNSS + vô tuyến), **không** mượn nhiệm vụ. Xem `../PROJECT-CHARTER.md` §1.1.

## 1b. Mục tiêu vòng xoắn 1

| # | Mục tiêu | Fit criterion | Trạng thái |
|---|---|---|---|
| **PoC-1** | Chuỗi thu có nhiễu nội tại thấp hơn nhiễu môi trường | YC-01: chênh lệch ≥ 10 dB, đo tại chỗ | 🟢 |
| **PoC-2** | Phao giữ được thuỷ âm đủ yên tĩnh trong điều kiện biển thật | YC-02: hiệu số đo tĩnh–động không vượt ngưỡng | 🔴 **chưa có ngưỡng — QĐ-2** |
| **PoC-3** | Phát hiện được mục tiêu thật trong dữ liệu thật | Phát hiện tàu đi qua, đối chiếu quan sát mắt hoặc AIS | 🟢 |
| **PoC-4** | Dữ liệu lấy về được và phân tích được | Trích xuất trọn vẹn sau thu hồi, chạy được chuỗi phân tích | 🟢 |

> **PoC-2 là mệnh đề khó nhất** và là lý do tồn tại của vòng xoắn này. Nó chưa có fit
> criterion định lượng — đây là khoảng trống nghiêm trọng nhất của cả đặc tả.

## Không thuộc mục đích vòng 1

Đo chữ ký âm học có truy xuất chuẩn · định hướng và định vị nguồn · phân loại tự động ·
trực canh dài ngày · sản phẩm hoá. Xem `../PROJECT-CHARTER.md` §2.3.

