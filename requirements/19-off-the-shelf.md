# 19. Off-the-Shelf Solutions — Giải pháp có sẵn

> **Volere v20 §19** · Đặc tả Yêu cầu HW-SNR-26 · **Vòng xoắn 1 — PoC**
> **Trạng thái:** 🟡 Đang soạn
> **Phụ trách:** Kỹ sư hệ thống
> **Cập nhật:** 2026-08-25

## Mục đích phần này (theo Volere)

**19a** Ready-made products · **19b** Reusable components · **19c** Products that can be copied.

---

## 19a / 19c. Sản phẩm có sẵn

| Sản phẩm | Quan hệ | Tài liệu |
|---|---|---|
| **Scanmatic TMK-SAS / SONRAS** | **Kiến trúc** tham chiếu, không phải nhiệm vụ tham chiếu | `../reference/` |

Đối chiếu chi tiết: `../PROJECT-CHARTER.md` §1.1.

> **Câu hỏi mua-hay-làm ở vòng 1 có câu trả lời khác thường:** vòng 1 không tạo ra sản phẩm
> để bán, mà tạo ra **kiến thức** — cụ thể là biết được có chế tạo được phao đủ yên tĩnh hay
> không, và cấu hình treo nào hiệu quả. Kiến thức đó không mua được. Đó là lý do vòng 1 tự làm.
>
> Câu hỏi mua-hay-làm cho **hệ thống hoàn chỉnh** chỉ nên đặt ra sau khi có kết quả vòng 1.

## 19b. Thành phần mua ngoài ở vòng 1

| Thành phần | Quyết định |
|---|---|
| Thuỷ âm + tiền khuếch đại | **Mua** — RB-02 |
| Modem UHF | **Mua** |
| Máy thu GNSS có PPS | **Mua** |
| ADC / bo mạch thu nhận | **Mua** nếu có sẵn loại đạt yêu cầu nhiễu |
| Máy tính trạm | **Mua** — laptop thường |
| Thư viện FFT, dàn lọc octave | **Dùng thư viện đã kiểm chứng** |
| **Cơ cấu treo giảm chấn** | **TỰ LÀM** — đây là nội dung nghiên cứu của vòng xoắn |
| **Thân phao + neo** | **TỰ LÀM** — gắn với bài toán trên |

> **Nguyên tắc vòng 1:** mua mọi thứ mua được, dồn toàn bộ công sức thiết kế vào A1.4 và A8 —
> hai phân hệ quyết định thành bại. Tự làm bo mạch thu nhận khi có thể mua là tự tạo thêm rủi
> ro không cần thiết.

