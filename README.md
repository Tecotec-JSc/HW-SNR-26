# HW-SNR-26

**Hệ thống Thu nhận, Ghi và Phân tích Tín hiệu Thuỷ âm**
*Sound Registration and Analyzing System*

---

## ⚠ ĐỌC TRƯỚC TIÊN

Dự án đang chờ **QĐ-0** — chưa chốt nhiệm vụ chính của hệ thống:

| | **Hướng A — Đo chữ ký âm học** | **Hướng B — Giám sát thuỷ âm** |
|---|---|---|
| Câu hỏi trả lời | "Tàu của tôi ồn bao nhiêu dB?" | "Có mục tiêu nào trong khu vực không?" |
| Yêu cầu bậc nhất | Độ chính xác tuyệt đối | Độ nhạy phát hiện |
| Cảm biến | 1 thuỷ âm hiệu chuẩn chính xác | Mảng nhiều phần tử |
| Triển khai | Phao nổi, theo đợt đo | Đặt đáy, trực canh dài ngày |

Hai hướng dẫn tới **hai hệ thống khác nhau**. Toàn bộ tài liệu hiện tại viết theo
**Hướng A**, dựa trên hệ tham chiếu trong `reference/`.

**Chief Engineer cần chốt QĐ-0 trước khi bắt đầu thiết kế chi tiết.**
Lập luận đầy đủ: `PROJECT-CHARTER.md` §1.

---

## Tài liệu

| Tài liệu | Nội dung | Đọc khi nào |
|---|---|---|
| **`PROJECT-CHARTER.md`** | Điều lệ dự án: mục tiêu, CONOPS, phân rã hệ thống, lộ trình, rủi ro, quyết định mở | **Đọc đầu tiên** |
| **`SYSTEM-SPECIFICATION.md`** | Cấu trúc, sơ đồ khối, chuỗi tín hiệu, ngân sách thiết kế, đặc tả 13 phân hệ | Khi bắt đầu thiết kế |
| **`requirements/`** | Đặc tả yêu cầu theo Volere v20 — 27 mục + bản ghi yêu cầu nguyên tử | Khi viết hoặc rà yêu cầu |
| `reference/` | Tài liệu hệ tham chiếu và mẫu Volere | Tra cứu |

### Thứ tự đọc cho kỹ sư mới

```
1. README.md (file này)          — 5 phút, hiểu bối cảnh
2. PROJECT-CHARTER.md §0, §1     — 15 phút, hiểu vấn đề chưa chốt
3. PROJECT-CHARTER.md toàn bộ    — 1 giờ
4. SYSTEM-SPECIFICATION.md §1-§4 — 1 giờ, cấu trúc và ngân sách
5. Phần đặc tả phân hệ của mình  — SYSTEM-SPECIFICATION.md §5
6. requirements/README.md        — khi cần viết yêu cầu
```

---

## Cấu trúc kho

```
HW-SNR-26/
├── README.md                     ← file này
├── PROJECT-CHARTER.md            ← điều lệ dự án
├── SYSTEM-SPECIFICATION.md       ← đặc tả hệ thống
├── requirements/                 ← đặc tả yêu cầu Volere v20
│   ├── README.md                 ← hướng dẫn + tình trạng
│   ├── 01…27-*.md                ← 27 mục Volere
│   ├── atomic-requirements.csv   ← bản ghi yêu cầu (16 trường Volere)
│   └── _requirement-shell-template.md
└── reference/
    ├── 6. Catalog TMK-SAS.pdf              ← hệ tham chiếu Scanmatic
    ├── con-of diagram.jpg                  ← sơ đồ tổng thể hệ tham chiếu
    ├── Requirements Specification Template v20.doc  ← mẫu Volere
    ├── Volere Atomic Requirements Stationery.xls
    └── Volere Atomic Requirements example.xls
```

---

## Tình trạng

| Chỉ số | Giá trị |
|---|---|
| Yêu cầu đã ghi | 25 |
| Thiếu Fit Criterion | 1 (yêu cầu 2.0 — sai số công bố) |
| Xung đột yêu cầu chưa giải | 1 (kênh nghe tương tự ↔ mã hoá) |
| Quyết định mở | 15 (QĐ-0 … QĐ-15) |
| Mục Volere còn trống | 8 / 27 |
| Cổng hiện tại | **G1 — Đồng thuận & Đặc tả** |

### Ba việc cần làm ngay

| # | Việc | Ai | Chặn |
|---|---|---|---|
| 1 | **Chốt QĐ-0** — nhiệm vụ chính | Chief Engineer | toàn bộ thiết kế |
| 2 | Đọc **ISO 17208**, đối chiếu các mục còn trống | 1 kỹ sư | nhiều mục yêu cầu |
| 3 | **Ngân sách nhiễu + ngân sách sai số** (WP-1) | Kỹ sư analog + đo lường | YC-01, YC-13 |

Việc 2 và 3 **không** bị chặn bởi QĐ-0 — bắt đầu được ngay hôm nay.

---

## Hai con số cần thuộc

**1. Phương trình đo** — mọi kỹ sư trong dự án cần hiểu:

```
SL = SPL_đo + TL(r, f)        SPL_đo = V_ADC(dB) − M − G_tổng + K

SL   mức nguồn [dB re 1µPa @1m]      M  độ nhạy thuỷ âm  [dB re 1V/µPa]
TL   suy hao truyền âm [dB]          G  tổng độ lợi      [dB]
r    cự ly, tính từ GNSS hai đầu     K  hệ số hiệu chuẩn [dB]
```

Sai số của **mỗi** số hạng cộng thẳng vào sai số kết quả.

**2. Quan hệ thời gian ↔ cự ly** — với c ≈ 1500 m/s trong nước biển:

```
Δt = 1 ms  ⇒  Δd = 1.5 m
```

Đặt trần cho toàn bộ độ chính xác định vị của hệ thống.

---

## Quy ước trong tài liệu

| Ký hiệu | Ý nghĩa |
|---|---|
| `[CHỐT]` | Đã quyết định, đổi phải qua change request |
| `[GIẢ ĐỊNH]` | Đang dùng làm cơ sở thiết kế nhưng **chưa xác minh** |
| `[MỞ]` | Chưa quyết định — **không tự ý chọn thay**, báo Chief Engineer |
| `[THAM CHIẾU]` | Số liệu của hệ tham chiếu TMK-SAS, không phải yêu cầu của ta |
| `[TÍNH]` | Giá trị suy ra bằng tính toán, có trình bày trong tài liệu |

---

## Phạm vi kho tài liệu

Kho này chỉ chứa **nội dung kỹ thuật**. Không chứa: điều khoản thương mại và giá,
danh tính đối tác và vấn đề kiểm soát xuất khẩu, địa điểm triển khai cụ thể và dữ liệu
khảo sát, thông tin khách hàng và tiến trình phê duyệt.

Cần các thông tin trên để ra quyết định kỹ thuật → đề nghị qua Chief Engineer.
