# 27. Ideas for Solutions — Ý tưởng giải pháp

> **Volere v20 §27** · Đặc tả Yêu cầu HW-SNR-26
> **Trạng thái:** 🟡 Đang soạn
> **Phụ trách:** mọi thành viên
> **Cập nhật:** 2026-08-25

## Mục đích phần này (theo Volere)

Nơi ghi ý tưởng **giải pháp** để chúng không lẫn vào phần yêu cầu. Volere tách bạch: yêu cầu nói *cần gì*, phần này nói *có thể làm thế nào*.

---

## Ý tưởng đã ghi nhận

| # | Ý tưởng | Liên quan |
|---|---|---|
| YT-1 | Dùng cơ cấu treo mềm tách chuyển động phao khỏi thuỷ âm | SPEC §5.1.4(a) |
| YT-2 | Hiệu chuẩn từng nấc PGA cho từng thiết bị, lưu vào hồ sơ | SPEC §5.2.4 |
| YT-3 | Phép thử tự kiểm chứng: đo cùng nguồn ở nhiều cự ly, kiểm tra mức nguồn hội tụ | SPEC §6 |
| YT-4 | Dùng dàn lọc analog + tách đường bao cho dải cao thay vì ADC tốc độ cao | SPEC §5.3.4 PA-2 |
| YT-5 | Truyền trước một phần dữ liệu quan trọng phòng trường hợp mất phao | RR-09 |
| YT-6 | Tận dụng dư băng thông UHF để truyền thêm phổ băng hẹp rút gọn | SPEC §2.3 |

> **Quy tắc:** ý tưởng ở đây **không phải** cam kết thiết kế. Đưa vào thiết kế thì phải
> qua một quyết định có ghi chép, và thường sinh ra một atomic requirement mới.

