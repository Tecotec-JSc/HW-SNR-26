# 16. Cultural Requirements — Yêu cầu văn hoá

> **Volere v20 §16** · Đặc tả Yêu cầu HW-SNR-26
> **Trạng thái:** 🔴 Trống
> **Phụ trách:** BA
> **Cập nhật:** 2026-08-25

## Mục đích phần này (theo Volere)

**16a** Cultural market requirements · **16b** Cultural diversity and inclusion.

---

## Nội dung

Với sản phẩm kỹ thuật dùng nội bộ, phần này thường mỏng — nhưng **không bỏ trống mà không rà**.

| Câu hỏi | Ghi chú |
|---|---|
| Ngôn ngữ giao diện và tài liệu | Tiếng Việt là mặc định; cân nhắc song ngữ nếu tính tới xuất khẩu |
| Đơn vị đo | Hệ SI; nhưng dB có nhiều mức quy chiếu khác nhau — **phải ghi rõ mức quy chiếu ở mọi nơi hiển thị** |
| Quy ước ngày giờ | Nên dùng UTC trong dữ liệu, giờ địa phương khi hiển thị |

> Mục "đơn vị đo" nghe nhỏ nhưng là nguồn sai sót thực tế: `dB re 1µPa` và `dB re 1V/µPa`
> khác nhau hoàn toàn. Giao diện nào hiển thị dB mà không ghi mức quy chiếu là lỗi.

