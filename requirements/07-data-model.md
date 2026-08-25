# 7. Business Data Model and Data Dictionary — Mô hình dữ liệu và từ điển dữ liệu

> **Volere v20 §7** · Đặc tả Yêu cầu HW-SNR-26 · **Vòng xoắn 1 — PoC**
> **Trạng thái:** 🟡 Đang soạn
> **Phụ trách:** Kỹ sư phần mềm
> **Cập nhật:** 2026-08-25

## Mục đích phần này (theo Volere)

**7a** Business data model · **7b** Data dictionary.

---

## 7a. Thực thể dữ liệu — vòng xoắn 1

```
   CHUYẾN THỬ NGHIỆM
        │ 1..n
        ▼
   PHIÊN ĐO ────► CẤU HÌNH TREO GIẢM CHẤN   ⭐ đặc thù vòng 1
        │    ────► TRẠNG THÁI (tĩnh / động)  ⭐ đặc thù vòng 1
        │    ────► ĐIỀU KIỆN MÔI TRƯỜNG
        │ 1..n
        ▼
   BẢN GHI THÔ ────► METADATA ────► HỒ SƠ HIỆU CHUẨN (M, K)
        │                    └────► VỆT ĐỘ LỢI G(t)
        │
        ├──► VỆT VỊ TRÍ GNSS
        │
        ▼
   KẾT QUẢ PHỔ ──► MỨC THU ĐƯỢC RL
        │
        ▼
   SỰ KIỆN PHÁT HIỆN ──► ĐỐI CHIẾU QUAN SÁT NGOÀI (mắt / AIS)
```

**Hai thực thể có sao ⭐ là đặc thù của vòng xoắn PoC** và sẽ biến mất ở vòng sau. Chúng tồn
tại vì vòng 1 là một **thí nghiệm so sánh**, không phải một hệ vận hành.

## 7b. Từ điển dữ liệu

Danh sách metadata bắt buộc: `../SYSTEM-SPECIFICATION.md` §4.11 (C2).
Cần chuyển thành từ điển đầy đủ — kiểu, đơn vị, miền giá trị, nguồn sinh ra. Đây là đầu vào
trực tiếp của việc đặc tả định dạng dữ liệu (Charter §9 việc 5).

