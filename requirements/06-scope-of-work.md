# 6. The Scope of the Work — Phạm vi công việc

> **Volere v20 §6** · Đặc tả Yêu cầu HW-SNR-26 · **Vòng xoắn 1 — PoC**
> **Trạng thái:** 🟡 Đang soạn
> **Phụ trách:** Kỹ sư hệ thống
> **Cập nhật:** 2026-08-25

## Mục đích phần này (theo Volere)

**6a** Current situation · **6b** Context of the work · **6c** Work partitioning (BUC) · **6d** Specifying a business use case.

---

## 6b. Sơ đồ ngữ cảnh — vòng xoắn 1

```
   [Nguồn âm trong khu vực]  ──âm thanh──►┌───────────────┐
   (tàu, sinh vật, môi trường)            │               │
                                          │  HW-SNR-26    │──►[Dữ liệu thô + metadata]
   [Vệ tinh GNSS] ──vị trí, thời gian────►│   Vòng 1      │──►[Danh sách sự kiện]
                                          │               │──►[Phổ nhiễu nền]
   [Đội kỹ thuật] ──thả/thu/cấu hình─────►│               │
                                          └───────────────┘
   [Phòng hiệu chuẩn] ──hệ số M, K───────►      ▲
                                                │
   [Môi trường biển] ──nhiễu, dòng chảy, sóng───┘
```

**Đầu ra "Phổ nhiễu nền" là sản phẩm đặc thù của vòng 1** — nó không phải sản phẩm của hệ
thống cuối, mà là bằng chứng để phán quyết PoC-1 và PoC-2.

## 6c. Business Use Cases — vòng xoắn 1

| BUC # | Tên | Trạng thái |
|---|---|---|
| **BUC-1** | Thực hiện một chuyến thử nghiệm biển: đo tĩnh, đo động, ≥2 cấu hình treo | 🔴 cần viết chi tiết |
| BUC-2 | Phân tích dữ liệu sau chuyến, so sánh cấu hình treo | 🔴 |
| BUC-3 | Hiệu chuẩn và kiểm tra hệ thống trước/sau chuyến | 🔴 |

> **BUC-1 là use case trung tâm và cần được viết chi tiết trước chuyến biển đầu tiên.**
> Một chuyến biển tốn kém; nếu quy trình không được nghĩ trước, sẽ về tay không hoặc thiếu
> dữ liệu đối chứng. Xem `../PROJECT-CHARTER.md` §9 việc 9.

