# 18. Open Issues — Vấn đề còn mở

> **Volere v20 §18** · Đặc tả Yêu cầu HW-SNR-26 · **Vòng xoắn 1 — PoC**
> **Trạng thái:** 🟢 Đủ để review
> **Phụ trách:** Chief Engineer
> **Cập nhật:** 2026-08-25

## Mục đích phần này (theo Volere)

Vấn đề đã nêu nhưng **chưa có lời giải**, có thể ảnh hưởng thành bại của vòng xoắn.

---

## Quyết định mở

| # | Vấn đề | Chặn | Mức |
|---|---|---|---|
| **QĐ-2** | **Ngưỡng YC-02** (nhiễu khi phao hoạt động thật) và sai số công bố YC-07 | **Tiêu chí đạt/không đạt của cả PoC** | 🔴 |
| **QĐ-1** | Dải tần công tác | Thuỷ âm, ADC, fs, thiết kế treo | 🔴 |
| **QĐ-3** | Phương án treo giảm chấn (≥2 để thử) | A1.4, A8 — rủi ro chi phối | 🔴 |
| **QĐ-4** | Neo cố định hay thả trôi | A8, RR-01, kế hoạch thử nghiệm | 🔴 |
| QĐ-5 | Thời lượng một đợt triển khai | A9, A5 | 🟡 |
| QĐ-6 | Mức hiệu chuẩn cần cho PoC | C3, chi phí | 🟡 |
| QĐ-7 | Thuật toán phát hiện sự kiện vòng 1 | A4.2 | 🟡 |
| QĐ-8 | Khu vực và cửa sổ thời gian thử nghiệm | Kế hoạch, RR-04, RR-08 | 🟡 |
| QĐ-16 | Định nghĩa chặt "sự kiện": bắt đầu/kết thúc, ngưỡng | A4.2, tiêu chí PoC-3 | 🟡 |
| QĐ-17 | Thang ưu tiên: H/M/L hay MoSCoW | Bản ghi yêu cầu | 🟢 chọn nhanh |

## Đã đóng

| # | Vấn đề | Kết luận |
|---|---|---|
| QĐ-0 | Nhiệm vụ chính | ✅ **Phao giám sát thuỷ âm thụ động** |
| QĐ-9 | Chuyển nấc PGA | ✅ Tự động — mục tiêu giám sát xuất hiện bất ngờ |
| QĐ-10 | Nhánh xử lý dải cao | ✅ Bỏ khỏi vòng 1 — một đường lấy mẫu duy nhất |
| QĐ-11 | Kênh nghe tương tự ↔ mã hoá | ✅ Bỏ cả hai khỏi vòng 1 — xung đột tự hết |
| QĐ-12 | Mô hình suy hao truyền âm | ✅ Không còn áp dụng — bỏ chuẩn hoá cự ly |
| QĐ-13 | Vị trí bảng thuật ngữ | ✅ Giữ ở Charter §11, các nơi khác trỏ sang |
| QĐ-14 | Ranh giới sản phẩm | ✅ Đặt rộng về phía con người ở PoC — xem `08-scope-of-product.md` |

