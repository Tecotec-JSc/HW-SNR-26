# 13. Operational and Environmental Requirements — Yêu cầu vận hành và môi trường

> **Volere v20 §13** · Đặc tả Yêu cầu HW-SNR-26 · **Vòng xoắn 1 — PoC**
> **Trạng thái:** 🟡 Đang soạn
> **Phụ trách:** Kỹ sư hệ thống
> **Cập nhật:** 2026-08-25

## Mục đích phần này (theo Volere)

**13a** Physical environment · **13b** Wider environment · **13c** Interfacing with adjacent systems · **13d** Productization · **13e** Release · **13f** Backwards compatibility.

---

## 13a. Môi trường vật lý

| Tham số | Vòng 1 | Trạng thái |
|---|---|---|
| Chế độ triển khai | Phao nổi; neo hay trôi chưa chốt | 🔴 QĐ-4 |
| Thời lượng một đợt | Giờ tới ngày, không phải tuần | 🟡 QĐ-5 |
| Trạng thái biển | **Chọn cửa sổ thời tiết thuận lợi** | 🟢 chủ ý |
| Vùng thử nghiệm | Cần có giao thông tàu để có mục tiêu | 🔴 QĐ-8 |
| Dòng chảy | TBD — **ảnh hưởng trực tiếp nhiễu nền tảng** | 🔴 |

> **Dòng chảy là tham số môi trường quan trọng nhất của vòng này**, vì nó sinh ra cả nhiễu
> dòng chảy lẫn rung dây neo. Cần số liệu vùng thử trước khi chốt QĐ-4.

## 13c. Giao diện với hệ thống lân cận

| Hệ thống | Giao diện | Trạng thái |
|---|---|---|
| GNSS | Thu tín hiệu + PPS | 🟢 |
| Nguồn quan sát ngoài (mắt / AIS) | Đối chiếu thủ công để xác nhận PoC-3 | 🟡 |
| Phòng hiệu chuẩn | Hồ sơ M, K | 🟡 |

## 13d / 13e / 13f

⬛ Không áp dụng ở vòng 1 — không có sản phẩm bàn giao, không có bản phát hành,
không có phiên bản trước để tương thích ngược.

