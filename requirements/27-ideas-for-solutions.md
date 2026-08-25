# 27. Ideas for Solutions — Ý tưởng giải pháp

> **Volere v20 §27** · Đặc tả Yêu cầu HW-SNR-26 · **Vòng xoắn 1 — PoC**
> **Trạng thái:** 🟡 Đang soạn
> **Phụ trách:** mọi thành viên
> **Cập nhật:** 2026-08-25

## Mục đích phần này (theo Volere)

Ý tưởng **giải pháp**, tách khỏi phần yêu cầu. Yêu cầu nói *cần gì*; phần này nói *có thể làm thế nào*.

---

## Ý tưởng cho cơ cấu treo giảm chấn (A1.4) — trọng tâm vòng 1

| # | Ý tưởng | Ghi chú |
|---|---|---|
| YT-01 | Phần tử đàn hồi giữa phao và cáp thuỷ âm | Cắt đường truyền rung cơ học |
| YT-02 | Khối lượng giảm chấn / tạ ổn định dưới thuỷ âm | Tăng quán tính, giảm dao động theo phao |
| YT-03 | Đoạn cáp chùng có chủ ý | Cắt đường truyền rung dọc cáp |
| YT-04 | Chắn dòng chảy quanh thuỷ âm | ⚠ Cân nhắc kỹ — có thể tự sinh nhiễu xoáy |
| YT-05 | Tách thuỷ âm khỏi phao bằng dây mềm dài | Đánh đổi với việc kiểm soát độ sâu |

> Vòng 1 phải chọn **ít nhất hai** trong số trên để thử và so sánh (RB-07).

## Ý tưởng khác

| # | Ý tưởng | Liên quan |
|---|---|---|
| YT-06 | Đo tĩnh và đo động trong cùng chuyến để lấy hiệu số | Phép đo trung tâm — đã nâng thành SPEC §3.3 |
| YT-07 | Ghi cấu hình treo vào metadata của file dữ liệu | Đã nâng thành yêu cầu bắt buộc — SPEC C2 |
| YT-08 | Hiệu chuẩn từng nấc PGA cho từng thiết bị | SPEC ĐT-C3-02 |
| YT-09 | Truyền trước phần dữ liệu quan trọng phòng mất phao | RR-05 |
| YT-10 | Dùng AIS để đối chiếu sự kiện phát hiện được | PoC-3 — cách xác nhận rẻ và khách quan |
| YT-11 | Bố trí tàu hợp tác chạy qua theo lịch | Phương án dự phòng cho RR-08 |

> **Quy tắc:** ý tưởng ở đây không phải cam kết thiết kế. Đưa vào thiết kế thì phải qua một
> quyết định có ghi chép, và thường sinh ra một atomic requirement mới.

