# 26. Waiting Room — Phòng chờ

> **Volere v20 §26** · Đặc tả Yêu cầu HW-SNR-26 · **Vòng xoắn 1 — PoC**
> **Trạng thái:** 🟢 Đủ để review
> **Phụ trách:** Chief Engineer
> **Cập nhật:** 2026-08-25

## Mục đích phần này (theo Volere)

Yêu cầu đã đề xuất nhưng **hoãn**, không đưa vào vòng xoắn này.

---

## Đang chờ

| # | Nội dung | Vì sao hoãn | Vòng dự kiến |
|---|---|---|---|
| PC-01 | Định hướng nguồn âm (bearing / DOA) | Cần mảng nhiều phần tử | 3 |
| PC-02 | Định vị nguồn bằng nhiều phao | Cần nhiều phao + đồng bộ chặt | 3 |
| PC-03 | Phân loại mục tiêu tự động, DEMON, học máy | Cần dữ liệu thật — sản phẩm của vòng 1 | 2–3 |
| PC-04 | Trực canh dài ngày (tuần/tháng) | Bài toán nguồn điện + hà bám | 2 |
| PC-05 | Mã hoá dữ liệu lưu và truyền | Không gỡ rủi ro PoC nào | 2 |
| PC-06 | Kênh nghe tương tự thời gian thực | Không gỡ rủi ro PoC nào; tốn điện | 2 |
| PC-07 | Truy xuất chuẩn đo lường đầy đủ | Cần khi bán dịch vụ đo, không cần để chứng minh nghe được | 2–4 |
| PC-08 | Cấu hình từ xa tham số thu nhận | Vòng 1 cấu hình trước khi thả | 2 |
| PC-09 | Xuất dữ liệu chuẩn cho hệ thống bên thứ ba | Vòng 1 không bàn giao | 2–4 |
| PC-10 | Nhánh xử lý dải tần cao (trên ~20 kHz) | Chưa rõ có mục tiêu nào cần | 2 |
| PC-11 | Chịu trạng thái biển khắc nghiệt | Vòng 1 chọn cửa sổ thời tiết | 2 |
| PC-12 | Sản phẩm hoá: vỏ, tài liệu, đào tạo, hỗ trợ | PoC không bàn giao | 4 |
| PC-13 | Mảng quang loại bỏ điện tử đầu ướt | Rủi ro triển khai cao | 3–4 |
| PC-14 | Kiến trúc "cảm biến rẻ, bộ não thông minh" | Thuộc bài toán mảng | 3 |
| PC-15 | **Hệ đo chữ ký âm học có chuẩn hoá cự ly** | **Nhiệm vụ khác** — không phải hướng phát triển của sản phẩm này | — |

> **PC-15 khác các mục còn lại:** nó không phải tính năng bị hoãn mà là **một sản phẩm khác**.
> Nếu sau này có nhu cầu, nó sẽ là một chương trình riêng dùng chung một số thành phần
> (chuỗi thu, hiệu chuẩn, định dạng dữ liệu), không phải vòng xoắn tiếp theo của chương trình này.

## Công dụng của phòng chờ

Khi ai đó đề xuất "hay là thêm beamforming vào vòng 1", câu trả lời là *"đã ở PC-01, xem lại
ở vòng 3"* — thay vì tranh luận lại từ đầu hoặc âm thầm phình phạm vi.

