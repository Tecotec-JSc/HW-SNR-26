# 3. Constraints — Ràng buộc

> **Volere v20 §3** · Đặc tả Yêu cầu HW-SNR-26 · **Vòng xoắn 1 — PoC**
> **Trạng thái:** 🟡 Đang soạn
> **Phụ trách:** Chief Engineer
> **Cập nhật:** 2026-08-25

## Mục đích phần này (theo Volere)

**3a** Solution constraints · **3b** Implementation environment · **3c** Partner applications · **3d** Off-the-shelf software · **3e** Workplace environment · **3f** Schedule · **3g** Budget · **3h** Enterprise constraints.

---

## 3a. Ràng buộc giải pháp

| ID | Ràng buộc | Nguồn | Trạng thái |
|---|---|---|---|
| RB-01 | Kiến trúc phao + trạm điều khiển, mượn từ hệ tham chiếu | SONRAS | 🟢 |
| RB-02 | Thuỷ âm mua ngoài, không tự chế tạo | Charter §2.3 | 🟢 |
| RB-03 | **Thụ động** — không phát bất kỳ tín hiệu âm nào | Định nghĩa nhiệm vụ | 🟢 |
| RB-04 | Một thuỷ âm ở vòng 1, không phải mảng | Charter §2.3 | 🟢 |
| RB-05 | Chuỗi xử lý phải chạy được ngoại tuyến trên file đã ghi | SPEC ĐT-C1-04 | 🟢 |
| RB-06 | Dữ liệu thô chỉ lấy được sau khi thu hồi phao | Ràng buộc băng thông vật lý | 🟢 |
| RB-07 | Phải thử được ≥2 cấu hình treo giảm chấn trong vòng 1 | PoC-2 | 🟢 |

## 3e. Môi trường làm việc

Đội kỹ thuật làm việc **trên tàu**: rung, lắc, nắng, thời gian trên biển hạn chế và tốn kém.

**Hệ quả thiết kế quan trọng:** phải đổi được cấu hình treo **ngoài hiện trường**. Nếu phải
về xưởng mới đổi được thì mỗi chuyến chỉ thử được một cấu hình, và RB-07 không đạt trong
một chuyến biển. Xem SPEC ĐT-A1.4-03.

## 3f / 3g. Tiến độ và ngân sách

Không ghi trong repo này. Ràng buộc kỹ thuật liên quan: **số chuyến biển là nguồn lực khan
hiếm nhất của vòng 1** — kế hoạch thử nghiệm phải tối đa hoá thông tin thu được mỗi chuyến.

