# 12. Performance Requirements — Yêu cầu hiệu năng

> **Volere v20 §12** · Đặc tả Yêu cầu HW-SNR-26 · **Vòng xoắn 1 — PoC**
> **Trạng thái:** 🟡 Đang soạn
> **Phụ trách:** Kỹ sư hệ thống
> **Cập nhật:** 2026-08-25

## Mục đích phần này (theo Volere)

**12a** Speed/latency · **12b** Safety-critical · **12c** Precision/accuracy · **12d** Reliability/availability · **12e** Robustness · **12f** Capacity · **12g** Scalability · **12h** Longevity.

---

## Nguồn

7 yêu cầu hiệu năng YC-01…YC-07 tại `../PROJECT-CHARTER.md` §6.1.

## Ánh xạ sang phân nhóm Volere

| Volere | Yêu cầu | Ghi chú |
|---|---|---|
| 12c Độ chính xác | **YC-01, YC-02**, YC-05, YC-07 | Nhóm quan trọng nhất của vòng này |
| 12f Dung lượng | YC-06 (thời lượng triển khai) | |
| 12a Tốc độ | — | Vòng 1 không có yêu cầu thời gian thực chặt |
| 12b An toàn | — | Không áp dụng ở vòng 1 |
| 12d Độ tin cậy | — | `[V2+]` — vòng 1 chấp nhận hỏng và sửa |
| 12e Chịu lỗi | CN-08 (tự kiểm tra) | Đủ cho vòng 1 |
| 12g Mở rộng | — | `[V2+]` |
| 12h Tuổi thọ | — | `[V2+]` |

> **Việc để trống 12a, 12b, 12d, 12g, 12h là có chủ ý ở một PoC**, khác với để trống vì quên.
> Ghi rõ như trên để lần review sau không phải hỏi lại.

## Hai yêu cầu chi phối

**YC-01** — nhiễu điện tử ≥10 dB dưới nhiễu môi trường. Chứng minh trên bàn ở G2.
Ngân sách nhiễu Nhóm II: `../SYSTEM-SPECIFICATION.md` §3.1.

**YC-02** — nhiễu khi phao hoạt động thật. Chứng minh ngoài biển ở G3 bằng phép đo so sánh
tĩnh–động (`../SYSTEM-SPECIFICATION.md` §3.3).

> ⚠ **YC-02 chưa có ngưỡng.** Đây là khoảng trống nghiêm trọng nhất của cả đặc tả: không có
> ngưỡng thì không phán quyết được PoC đạt hay không đạt. **Chặn bởi QĐ-2, phải đóng trước G1.**

