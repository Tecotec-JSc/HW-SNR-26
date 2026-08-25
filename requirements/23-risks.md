# 23. Risks — Rủi ro

> **Volere v20 §23** · Đặc tả Yêu cầu HW-SNR-26 · **Vòng xoắn 1 — PoC**
> **Trạng thái:** 🟢 Đủ để review
> **Phụ trách:** Chief Engineer
> **Cập nhật:** 2026-08-25

## Mục đích phần này (theo Volere)

Rủi ro của vòng xoắn, kèm mức và biện pháp gỡ.

---

## Rủi ro vòng xoắn 1

| ID | Rủi ro | Mức | Gỡ bằng cách nào |
|---|---|---|---|
| **RR-01** | **Nhiễu do chuyển động phao / dòng chảy / dây neo che mất tín hiệu** | 🔴 | Thiết kế treo giảm chấn; thử ≥2 phương án; phép đo so sánh tĩnh–động |
| RR-02 | Nhiễu điện tử không đạt YC-01 | 🟡 | Ngân sách nhiễu **trước** khi chọn linh kiện |
| RR-03 | Không đặt được ngưỡng YC-02 → không phán quyết được PoC | 🔴 | QĐ-2, đóng trước G1 |
| RR-04 | Nhiễu môi trường vùng thử cao hơn dự kiến | 🟡 | Đo nhiễu nền thực địa sớm |
| RR-05 | Mất phao khi thu hồi → mất toàn bộ dữ liệu thô | 🟡 | CN-07; cân nhắc truyền trước phần dữ liệu quan trọng |
| RR-06 | Cấp phép tần số chậm hơn tiến độ | 🟡 | Làm rõ thủ tục ngay tuần đầu |
| RR-07 | Không có tàu / thời tiết phù hợp | 🟡 | Đặt lịch sớm, chuẩn bị cửa sổ dự phòng |
| RR-08 | **Không có mục tiêu thật đi qua khi thử → không chứng minh được PoC-3** | 🟡 | Chọn vùng có giao thông tàu; tàu hợp tác làm phương án dự phòng |
| RR-09 | Chỉ thử được một cấu hình treo mỗi chuyến biển | 🟡 | Thiết kế cho phép đổi cấu hình ngoài hiện trường (ĐT-A1.4-03) |

> **RR-01 là rủi ro chi phối và là lý do tồn tại của vòng xoắn 1.** Nếu chỉ có ngân sách để
> gỡ một rủi ro, gỡ rủi ro này. Xem `../PROJECT-CHARTER.md` §4.

> **RR-08 hay bị bỏ sót.** Thả phao ở nơi vắng sẽ thu được rất nhiều dữ liệu đẹp mà không
> chứng minh được gì.

