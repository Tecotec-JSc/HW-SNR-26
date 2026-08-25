# 18. Open Issues — Vấn đề còn mở

> **Volere v20 §18** · Đặc tả Yêu cầu HW-SNR-26
> **Trạng thái:** 🟢 Đủ để review
> **Phụ trách:** Chief Engineer
> **Cập nhật:** 2026-08-25

## Mục đích phần này (theo Volere)

Vấn đề đã được nêu ra nhưng **chưa có lời giải**, và có thể ảnh hưởng tới thành bại của dự án. Volere yêu cầu ghi lại để chúng không bị quên.

---

## Danh mục quyết định mở

Nguồn: `../PROJECT-CHARTER.md` §11 và `../SYSTEM-SPECIFICATION.md` §7.

| # | Vấn đề | Chặn | Mức |
|---|---|---|---|
| **QĐ-0** | **Nhiệm vụ chính: đo chữ ký (A) hay giám sát (B)?** | **Toàn bộ thiết kế** | 🔴 tối cao |
| QĐ-1 | Dải tần công tác | Chọn thuỷ âm, ADC, fs | 🔴 |
| QĐ-2 | Sai số tổng của mức nguồn công bố (YC-13) | Toàn bộ ngân sách sai số, tiêu chí nghiệm thu | 🔴 cao |
| QĐ-3 | Có làm chế độ định vị đa phao không | Số phao, yêu cầu đồng bộ | 🟡 |
| QĐ-4 | Phương án đường truyền | L1.7, kiến trúc SS-B | 🟡 |
| QĐ-5 | Thời gian hoạt động mục tiêu | Thiết kế nguồn, kết cấu phao | 🟡 |
| QĐ-6 | Chu kỳ và phương pháp hiệu chuẩn | C3, chi phí vận hành | 🟡 |
| QĐ-7 | 16 bit có đủ dải động không | A3 | 🟡 |
| QĐ-8 | Trạng thái biển và điều kiện môi trường thiết kế | A8, kế hoạch thử nghiệm | 🟡 |
| QĐ-9 | PGA: chuyển nấc tự động hay cố định | A2.2, quy trình đo | 🟡 |
| QĐ-10 | Kiến trúc nhánh dải cao (PA-1 hay PA-2) | A2, A3, A5, CĐ-4 | 🟡 |
| QĐ-11 | Kênh nghe: giữ tương tự hay chuyển số | A7.2 ↔ CN-06 xung đột | 🟡 |
| QĐ-12 | Mô hình suy hao truyền âm | Toàn bộ độ chính xác đầu ra | 🟡 |

## Vấn đề phát sinh từ việc áp Volere

| # | Vấn đề | Mức |
|---|---|---|
| QĐ-13 | Bảng thuật ngữ đặt ở Charter hay ở `04-terminology.md`? Volere đòi một nguồn duy nhất | 🟡 |
| QĐ-14 | Ranh giới sản phẩm (§8a): lập lộ trình chạy tàu, dựng báo cáo, quản lý thư viện — trong hay ngoài sản phẩm? | 🔴 cao |
| QĐ-15 | Thang ưu tiên dùng H/M/L hay MoSCoW? | 🟢 thấp — chọn nhanh |

