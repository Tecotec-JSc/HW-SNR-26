# 12. Performance Requirements — Yêu cầu hiệu năng

> **Volere v20 §12** · Đặc tả Yêu cầu HW-SNR-26
> **Trạng thái:** 🟡 Đang soạn
> **Phụ trách:** Kỹ sư hệ thống
> **Cập nhật:** 2026-08-25

## Mục đích phần này (theo Volere)

**12a** Speed and latency · **12b** Safety-critical · **12c** Precision or accuracy · **12d** Reliability and availability · **12e** Robustness/fault-tolerance · **12f** Capacity · **12g** Scalability/extensibility · **12h** Longevity.

---

## Nguồn

13 yêu cầu hiệu năng YC-01…YC-13 tại `../PROJECT-CHARTER.md` §6.1, đã nạp vào
`atomic-requirements.csv`.

## Ánh xạ sang phân nhóm Volere

| Volere | Yêu cầu hiện có | Khoảng trống |
|---|---|---|
| 12a Tốc độ / độ trễ | YC-06 (chu kỳ cập nhật 1.5 s) | Độ trễ kênh nghe chưa đặt |
| 12b An toàn | — | **Trống.** UD-2 (cự ly an toàn tàu quét mìn) là ứng dụng an toàn — cần rà |
| 12c Độ chính xác | **YC-01, YC-13**, YC-05, YC-07, YC-08 | YC-13 **chưa có số** |
| 12d Độ tin cậy / sẵn sàng | — | **Trống.** Tỷ lệ buổi đo thành công? MTBF? |
| 12e Chịu lỗi | CN-12 (BIT) | Hành vi khi mất liên lạc giữa chừng chưa định nghĩa |
| 12f Dung lượng | YC-11 (thời gian hoạt động) | Số phao tối đa cùng lúc (PUC-5) chưa đặt |
| 12g Mở rộng | TC-3 | Chưa viết thành yêu cầu có fit criterion |
| 12h Tuổi thọ | — | **Trống.** Vòng đời thiết kế bao lâu? |

> ⚠ **Bốn nhóm trống (12b, 12d, 12g, 12h)** — Volere đưa ra các nhóm này chính vì chúng hay bị
> bỏ sót. Với một hệ thống đo dùng cho quyết định an toàn, **12b không được để trống.**

## Ưu tiên tuyệt đối

**YC-01** (nhiễu nội tại ≥10 dB dưới ngưỡng môi trường) là ràng buộc bậc nhất — xem
`../SYSTEM-SPECIFICATION.md` §4.1.
**YC-13** (sai số công bố) là yêu cầu bị thiếu nghiêm trọng nhất — chặn QĐ-2.

