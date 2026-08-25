# 7. Business Data Model and Data Dictionary — Mô hình dữ liệu và từ điển dữ liệu

> **Volere v20 §7** · Đặc tả Yêu cầu HW-SNR-26
> **Trạng thái:** 🟡 Đang soạn
> **Phụ trách:** Kỹ sư phần mềm
> **Cập nhật:** 2026-08-25

## Mục đích phần này (theo Volere)

**7a** Business data model (thực thể dữ liệu và quan hệ) · **7b** Data dictionary (định nghĩa từng phần tử dữ liệu).

---

## 7a. Thực thể dữ liệu chính — bản nháp

```
   CHIẾN DỊCH ĐO
        │ 1..n
        ▼
   BUỔI ĐO ──────► ĐIỀU KIỆN MÔI TRƯỜNG (SVP, trạng thái biển)
        │ 1..n
        ▼
   BẢN GHI THÔ ──► METADATA (hiệu chuẩn, độ lợi, cấu hình thu)
        │           │
        │           └──► HỒ SƠ HIỆU CHUẨN ──► CHUỖI TRUY XUẤT
        ▼
   VỆT VỊ TRÍ (phao + trạm) ──► CỰ LY THEO THỜI GIAN
        │
        ▼
   KẾT QUẢ PHỔ ──► MỨC NGUỒN ──► BẢN GHI CHỮ KÝ ──► THƯ VIỆN CHỮ KÝ
```

## 7b. Từ điển dữ liệu

Đã có danh sách metadata bắt buộc tại `../SYSTEM-SPECIFICATION.md` §5.11.2 (C2).
**Cần chuyển thể thành từ điển dữ liệu đầy đủ:** mỗi trường có kiểu, đơn vị, miền giá trị,
bắt buộc/tuỳ chọn, nguồn sinh ra.

> Đây là đầu vào trực tiếp của **WP-2** (đặc tả định dạng dữ liệu). Hai việc nên làm cùng nhau.

