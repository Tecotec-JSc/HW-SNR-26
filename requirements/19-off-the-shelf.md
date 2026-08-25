# 19. Off-the-Shelf Solutions — Giải pháp có sẵn

> **Volere v20 §19** · Đặc tả Yêu cầu HW-SNR-26
> **Trạng thái:** 🟡 Đang soạn
> **Phụ trách:** Kỹ sư hệ thống
> **Cập nhật:** 2026-08-25

## Mục đích phần này (theo Volere)

**19a** Ready-made products · **19b** Reusable components · **19c** Products that can be copied.

---

## 19a / 19c. Sản phẩm có sẵn và có thể học theo

| Sản phẩm | Quan hệ | Tài liệu |
|---|---|---|
| **Scanmatic TMK-SAS / SONRAS** | Hệ tham chiếu chính | `../reference/6. Catalog TMK-SAS.pdf` |

Phân tích đối chiếu đầy đủ tại `../PROJECT-CHARTER.md` §7.4.

> **Quyết định mua-hay-làm chưa được đặt ra một cách tường minh.** Volere §19 tồn tại chính
> để buộc câu hỏi này: có thể **mua** một hệ như TMK-SAS thay vì tự làm không? Nếu không thì
> vì sao — giá, tiếp cận, hay vì mục tiêu là **sở hữu lõi IP** (TC-3, TC-4)?
> Cần ghi lại lập luận này, nếu không nó sẽ bị hỏi lại ở mọi lần review.

## 19b. Thành phần tái sử dụng — ứng viên

| Thành phần | Ghi chú |
|---|---|
| Thuỷ âm + tiền khuếch đại thương mại | Charter §2.4 đã chốt: mua ngoài |
| Modem UHF thương mại | Không có lý do tự làm |
| Máy thu GNSS có PPS | Không có lý do tự làm |
| Thư viện dàn lọc 1/3 octave | Cần theo IEC 61260 — cân nhắc thư viện đã được kiểm chứng |
| Thư viện FFT | Dùng thư viện chuẩn |

> **Nguyên tắc:** chỉ tự làm phần tạo ra lõi IP (SS-C). Mọi thứ khác mua nếu mua được.

