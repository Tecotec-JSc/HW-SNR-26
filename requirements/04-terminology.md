# 4. Naming Conventions and Terminology — Quy ước đặt tên và thuật ngữ

> **Volere v20 §4** · Đặc tả Yêu cầu HW-SNR-26 · **Vòng xoắn 1 — PoC**
> **Trạng thái:** 🟡 Đang soạn
> **Phụ trách:** Kỹ sư hệ thống
> **Cập nhật:** 2026-08-25

## Mục đích phần này (theo Volere)

**4a** Glossary — nguồn định nghĩa **duy nhất** cho mọi thuật ngữ.

---

## 4a. Bảng thuật ngữ

Bảng đầy đủ: `../PROJECT-CHARTER.md` §11.

### Thuật ngữ phải định nghĩa chặt trước G1

| Thuật ngữ | Vì sao |
|---|---|
| **RL — Mức thu được** | Sản phẩm đầu ra. `dB re 1 µPa`. **Không phải mức nguồn** |
| **SL — Mức nguồn** | `dB re 1 µPa @ 1m`. **Hệ này KHÔNG tính được** vì không biết cự ly |
| **Nhiễu nền tảng** | Nhiễu do phao/neo/cáp/dòng chảy sinh ra — Nhóm I trong ngân sách nhiễu |
| **Nhiễu môi trường** | Nhiễu của biển: gió, sóng, tàu, sinh vật — là MỐC so sánh |
| **Nhiễu nội tại** | Nhiễu điện tử — Nhóm II |
| **Sự kiện** | Một lần phát hiện. Cần định nghĩa: bắt đầu/kết thúc thế nào, ngưỡng nào |
| **Đo tĩnh / đo động** | Hai trạng thái của phép đo so sánh ở SPEC §3.3 |

> ⚠ **Ba loại nhiễu trên rất dễ bị dùng lẫn nhau trong hội thoại hằng ngày.** Toàn bộ tiêu
> chí đạt/không đạt của PoC dựa trên việc phân biệt được chúng. Đây là lý do §4 tồn tại.

