# 5. Relevant Facts and Assumptions — Sự kiện liên quan và giả định

> **Volere v20 §5** · Đặc tả Yêu cầu HW-SNR-26
> **Trạng thái:** 🟡 Đang soạn
> **Phụ trách:** Kỹ sư hệ thống
> **Cập nhật:** 2026-08-25

## Mục đích phần này (theo Volere)

**5a** Relevant facts (sự thật bên ngoài ảnh hưởng tới sản phẩm) · **5b** Business rules · **5c** Assumptions (điều ta đang cho là đúng mà **chưa kiểm chứng**).

---

## 5a. Sự kiện liên quan

| # | Sự kiện | Nguồn |
|---|---|---|
| SK-1 | Tốc độ âm trong nước biển ≈ 1500 m/s — quyết định quan hệ thời gian↔cự ly | Vật lý |
| SK-2 | ADC 16 bit cho SNR lý thuyết ≈ 98 dB, thực tế ≈ 89 dB | SPEC §4.2 |
| SK-3 | Dữ liệu thô 1 kênh @42 kS/s = 84 kB/s = 302 MB/giờ | SPEC §2.3 |
| SK-4 | Modem UHF dư băng thông ~1300× so với nhu cầu dữ liệu đã xử lý | SPEC §2.3 |
| SK-5 | PPS của GNSS phổ thông chính xác cỡ chục ns — dư 4 bậc so với yêu cầu 1 ms | SPEC §4.4 |

## 5c. Giả định — **danh sách kiểm soát rủi ro chính của dự án**

Mọi mục `[GIẢ ĐỊNH]` trong Charter và Spec phải xuất hiện ở đây.

| # | Giả định | Nếu sai thì sao | Kiểm chứng bằng | Trạng thái |
|---|---|---|---|---|
| GĐ-1 | Nhiệm vụ chính là **đo chữ ký** (Hướng A), không phải giám sát | Thiết kế sai toàn bộ | QĐ-0 | 🔴 **chưa kiểm chứng** |
| GĐ-2 | Dải tần công tác giống hệ tham chiếu (3 Hz–100 kHz) | Chọn sai ADC, thuỷ âm | QĐ-1 + đọc ISO 17208 | 🔴 |
| GĐ-3 | Cần dải trên 20 kHz | Thừa phần cứng, tốn điện | QĐ-10 | 🔴 |
| GĐ-4 | Cấu hình phao nổi phù hợp | Đổi toàn bộ cơ khí + đường truyền | QĐ-0 | 🟡 |
| GĐ-5 | GNSS thường (±5 m) đủ chính xác | Phải nâng lên RTK, đội chi phí | Sau khi có YC-13 | 🟡 |
| GĐ-6 | 16 bit đủ phủ dải động | Phải đổi ADC hoặc thêm nấc PGA | WP-1 | 🟡 |
| GĐ-7 | Nhiễu nền môi trường ở vùng đo nằm trong mức thiết kế | Giảm dải đo hữu ích | Đo thực địa | 🔴 |

> **Nguyên tắc Volere:** giả định chưa kiểm chứng là rủi ro chưa được đặt tên.
> Mỗi dòng 🔴 ở trên phải có người chịu trách nhiệm đóng trước cổng G1.

