# 3. Constraints — Ràng buộc

> **Volere v20 §3** · Đặc tả Yêu cầu HW-SNR-26
> **Trạng thái:** 🟡 Đang soạn
> **Phụ trách:** Chief Engineer
> **Cập nhật:** 2026-08-25

## Mục đích phần này (theo Volere)

**3a** Solution constraints (giải pháp bị bắt buộc phải thế nào) · **3b** Implementation environment of the current system · **3c** Partner/collaborative applications · **3d** Off-the-shelf software · **3e** Anticipated workplace environment · **3f** Schedule · **3g** Budget · **3h** Enterprise constraints.

---

## 3a. Ràng buộc giải pháp

| ID | Ràng buộc | Nguồn | Trạng thái |
|---|---|---|---|
| RB-01 | Kiến trúc hai phân hệ: phao đo + trạm điều khiển | Hệ tham chiếu TMK-SAS | 🟡 giả định |
| RB-02 | Không tự phát triển gốm áp điện / phần tử thu — mua ngoài | Charter §2.4 | 🟢 chốt |
| RB-03 | Lõi xử lý phải độc lập cấu hình phần cứng | Charter §5.1, TC-3 | 🟢 chốt |
| RB-04 | Dữ liệu thô chỉ lấy được sau khi thu hồi phao | Ràng buộc băng thông — SPEC §2.3 | 🟢 chốt (vật lý) |

## 3e. Môi trường làm việc dự kiến

Trắc thủ làm việc **trên tàu đang chạy**: rung, lắc, nắng gắt, tay ướt, có thể đeo găng.
Ảnh hưởng trực tiếp tới §10 (Look and Feel) và §11 (Usability) — **không được thiết kế giao
diện như phần mềm desktop văn phòng.**

## 3f / 3g. Tiến độ và ngân sách

> Không ghi trong repo này — xem `../PROJECT-CHARTER.md` §14.
> Lưu ý kỹ thuật: Charter §8 nêu rõ **chưa lập được mốc tiến độ đáng tin** cho tới khi
> QĐ-0 chốt và WP-1 xong.

## Việc cần làm
| Việc | Ai | Hạn |
|---|---|---|
| Chốt RB-01 (phụ thuộc QĐ-0) | Chief Engineer | **chặn mọi việc** |
| Liệt kê 3d — phần mềm/thư viện bên thứ ba dự kiến dùng | Kỹ sư phần mềm | trước G1 |

