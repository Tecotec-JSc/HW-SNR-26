# 5. Relevant Facts and Assumptions — Sự kiện liên quan và giả định

> **Volere v20 §5** · Đặc tả Yêu cầu HW-SNR-26 · **Vòng xoắn 1 — PoC**
> **Trạng thái:** 🟡 Đang soạn
> **Phụ trách:** Kỹ sư hệ thống
> **Cập nhật:** 2026-08-25

## Mục đích phần này (theo Volere)

**5a** Relevant facts · **5b** Business rules · **5c** Assumptions (chưa kiểm chứng).

---

## 5a. Sự kiện liên quan

| # | Sự kiện | Nguồn |
|---|---|---|
| SK-1 | Tốc độ âm trong nước biển ≈ 1500 m/s | Vật lý |
| SK-2 | ADC 16 bit: SNR lý thuyết ≈ 98 dB, thực tế ≈ 89 dB | SPEC §3.2 |
| SK-3 | Dữ liệu thô 1 kênh @42 kS/s = 84 kB/s = 302 MB/giờ | SPEC §2.2 |
| SK-4 | Modem UHF dư băng thông rất lớn so với nhu cầu dữ liệu đã xử lý | SPEC §2.2 |
| SK-5 | GNSS PPS chính xác cỡ chục ns — dư sức cho yêu cầu vòng 1 | SPEC §3.4 |
| SK-6 | Hệ tham chiếu SONRAS là phao **đo**, triển khai vài giờ, chọn được thời tiết | Catalog TMK-SAS |

> **SK-6 là sự kiện quan trọng nhất trong bảng này.** Nó giải thích vì sao tài liệu tham
> chiếu không bàn tới bài toán nhiễu nền tảng — và vì sao ta phải tự giải.

## 5c. Giả định — danh sách kiểm soát rủi ro

| # | Giả định | Nếu sai thì sao | Kiểm chứng | Trạng thái |
|---|---|---|---|---|
| GĐ-1 | Chế tạo được cơ cấu treo đủ giảm nhiễu nền tảng | **PoC thất bại** — tiêu chí dừng D-2 | Phép đo SPEC §3.3 | 🔴 **cốt lõi** |
| GĐ-2 | Nhiễu môi trường vùng thử nằm trong mức thiết kế | Giảm dải đo hữu ích | Đo thực địa sớm | 🔴 |
| GĐ-3 | Dải tần công tác đã chọn đúng với mục tiêu quan tâm | Nghe sai dải, không phát hiện được | QĐ-1 | 🔴 |
| GĐ-4 | 16 bit đủ phủ dải động | Phải đổi ADC hoặc thêm nấc PGA | Ngân sách nhiễu | 🟡 |
| GĐ-5 | GNSS thường đủ chính xác cho vòng 1 | Cần RTK | Đúng vì không chuẩn hoá cự ly | 🟢 |
| GĐ-6 | Có mục tiêu thật đi qua trong lúc thử nghiệm | **Không chứng minh được PoC-3** | Chọn vùng có giao thông; tàu hợp tác dự phòng | 🔴 RR-08 |
| GĐ-7 | Thuật toán phát hiện đơn giản là đủ cho vòng 1 | Phải làm phức tạp hơn | QĐ-7 | 🟡 |

> **GĐ-1 là giả định mà cả vòng xoắn đặt cược vào.** Nếu nó sai, tiêu chí dừng D-2 kích hoạt.
> Việc phát hiện điều đó ở vòng 1 — chứ không phải vòng 3 — chính là giá trị của mô hình xoắn ốc.

