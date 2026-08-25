# HW-SNR-26 — ĐẶC TẢ HỆ THỐNG

**Cấu trúc · Sơ đồ khối · Yêu cầu · Đặc tả từng phân hệ**

| | |
|---|---|
| **Mã chương trình** | HW-SNR-26 |
| **Phiên bản** | 0.1 |
| **Ngày** | 2026-08-25 |
| **Tài liệu mẹ** | `PROJECT-CHARTER.md` — đọc trước tài liệu này |
| **Đối tượng** | Kỹ sư thiết kế phân hệ |
| **Trạng thái** | Bản thảo — **phụ thuộc QĐ-0 chưa chốt** |

> ### ⚠ ĐIỀU KIỆN TIÊN QUYẾT
>
> Tài liệu này viết theo **Hướng A — hệ đo chữ ký âm học** (xem Charter mục 1). Nếu Chief Engineer chốt Hướng B (giám sát thuỷ âm), các mục 3.1, 4, 5.1–5.3 và toàn bộ mục 6 phải viết lại.
>
> Các chỗ có khác biệt lớn giữa hai hướng được đánh dấu `⇄ HƯỚNG B`.

**Quy ước trạng thái:** `[CHỐT]` · `[GIẢ ĐỊNH]` · `[MỞ]` · `[THAM CHIẾU]` (số liệu của hệ tham chiếu TMK-SAS, không phải yêu cầu của ta) · `[TÍNH]` (giá trị suy ra bằng tính toán trong tài liệu này).

---

## 1. CẤU TRÚC PHÂN RÃ HỆ THỐNG

### 1.1 Cây phân rã

```
HW-SNR-26  ─ Hệ thống thu nhận, ghi và phân tích tín hiệu thuỷ âm
│
├─ SS-A  PHAO ĐO ................................ (đơn vị triển khai dưới/trên nước)
│   ├─ A1  Cụm thuỷ âm
│   │      ├─ A1.1  Phần tử thu (hydrophone)
│   │      ├─ A1.2  Tiền khuếch đại tích hợp
│   │      ├─ A1.3  Cáp thuỷ âm nhiễu thấp
│   │      └─ A1.4  Cơ cấu treo giảm chấn
│   ├─ A2  Khối analog front-end
│   │      ├─ A2.1  Khuếch đại vi sai đầu vào
│   │      ├─ A2.2  Khuếch đại lập trình được (PGA)
│   │      ├─ A2.3  Lọc chống chồng phổ
│   │      └─ A2.4  Bảo vệ quá áp
│   ├─ A3  Khối số hoá
│   │      ├─ A3.1  ADC dải cơ bản
│   │      ├─ A3.2  Nhánh dải cao (xem 5.3.4 — kiến trúc chưa chốt)
│   │      └─ A3.3  Đồng hồ lấy mẫu + đồng bộ PPS
│   ├─ A4  Khối xử lý
│   │      ├─ A4.1  Xử lý phổ thời gian thực
│   │      ├─ A4.2  Quản lý ghi dữ liệu thô
│   │      └─ A4.3  Đóng gói bản tin + mã hoá
│   ├─ A5  Lưu trữ
│   ├─ A6  GNSS + đồng bộ thời gian
│   ├─ A7  Liên lạc vô tuyến
│   │      ├─ A7.1  Modem dữ liệu
│   │      └─ A7.2  Kênh nghe tương tự
│   ├─ A8  Kết cấu phao
│   │      ├─ A8.1  Thân phao + độ nổi
│   │      ├─ A8.2  Cột ăng-ten
│   │      ├─ A8.3  Khoang kín điện tử
│   │      └─ A8.4  Cơ cấu neo / thả / thu hồi
│   └─ A9  Nguồn điện
│          ├─ A9.1  Ắc quy chính
│          ├─ A9.2  Chuyển đổi + phân phối
│          └─ A9.3  Nguồn dự phòng + đèn/phao định vị
│
├─ SS-B  TRẠM ĐIỀU KHIỂN (SCS) ................... (trên tàu / công trình biển)
│   ├─ B1  Máy tính trạm
│   ├─ B2  Modem vô tuyến + máy thu kênh nghe
│   ├─ B3  GNSS trạm
│   ├─ B4  Phần mềm trắc thủ
│   │      ├─ B4.1  CĐ-1 Giám sát thu thập
│   │      ├─ B4.2  CĐ-2 Phát lại
│   │      ├─ B4.3  CĐ-3 Định vị sự kiện (tuỳ chọn)
│   │      └─ B4.4  CĐ-4 Xử lý hậu kỳ
│   └─ B5  Lưu trữ + xuất báo cáo
│
└─ SS-C  NỀN TẢNG XỬ LÝ & HIỆU CHUẨN ............. (lõi IP, xuyên suốt A và B)
    ├─ C1  Thư viện xử lý tín hiệu
    │      ├─ C1.1  Bù hiệu chuẩn
    │      ├─ C1.2  Phân tích 1/3 octave
    │      ├─ C1.3  Phân tích băng hẹp (FFT)
    │      ├─ C1.4  Chuẩn hoá theo cự ly
    │      └─ C1.5  Trích đặc trưng / đối chiếu thư viện
    ├─ C2  Định dạng dữ liệu + metadata
    ├─ C3  Quy trình & hồ sơ hiệu chuẩn
    └─ C4  Tự kiểm tra (BIT)
```

### 1.2 Ma trận phân công

| Phân hệ | Chuyên môn chính | Phụ thuộc đầu vào |
|---|---|---|
| A1, A8 | Cơ khí + vật liệu biển | Độ sâu, trạng thái biển (QĐ-8) |
| A2, A3 | Điện tử tương tự + đo lường | Ngân sách nhiễu (WP-1) |
| A4, A5, C1 | Phần mềm nhúng + DSP | Định dạng dữ liệu (WP-2) |
| A6, A7 | RF + hệ thống | Cấp phép tần số (WP-4) |
| A9 | Điện tử công suất | Ngân sách điện năng (WP-4) |
| B1–B5 | Phần mềm ứng dụng | Định dạng dữ liệu (WP-2) |
| C3 | Đo lường / hiệu chuẩn | Chuỗi truy xuất (WP-3) |

---

## 2. SƠ ĐỒ KHỐI HỆ THỐNG

### 2.1 Mức hệ thống

```
                                    ╔═══════════════╗
                                    ║  VỆ TINH GNSS ║
                                    ╚═══════╤═══════╝
                        ┌───────────────────┴───────────────────┐
                        │ Vị trí + thời gian (PPS)              │
                        ▼                                       ▼
╔════════════════════════════════════════╗      ╔════════════════════════════════════╗
║          SS-A — PHAO ĐO                ║      ║   SS-B — TRẠM ĐIỀU KHIỂN (SCS)     ║
║                                        ║      ║                                    ║
║  ┌──────────────────────────────────┐  ║      ║  ┌──────────────────────────────┐  ║
║  │ A6  GNSS + đồng bộ thời gian     │  ║      ║  │ B3  GNSS trạm                │  ║
║  └───────────────┬──────────────────┘  ║      ║  └──────────────┬───────────────┘  ║
║                  │ PPS + NMEA          ║      ║                 │ vị trí trạm      ║
║  ┌───────┐  ┌────▼─────┐  ┌─────────┐  ║      ║  ┌──────────────▼───────────────┐  ║
║  │ A1    │  │ A2       │  │ A3      │  ║      ║  │ B1/B4  Máy tính + phần mềm   │  ║
║  │ Thuỷ  ├─►│ Analog   ├─►│ Số hoá  │  ║      ║  │        trắc thủ              │  ║
║  │ âm    │  │ front-end│  │         │  ║      ║  │  ┌────────────────────────┐  │  ║
║  └───────┘  └──────────┘  └────┬────┘  ║      ║  │  │ CĐ-1 Giám sát          │  │  ║
║                                │       ║      ║  │  │ CĐ-2 Phát lại          │  │  ║
║              ┌─────────────────▼─────┐ ║      ║  │  │ CĐ-3 Định vị sự kiện   │  │  ║
║              │ A4  Khối xử lý        │ ║      ║  │  │ CĐ-4 Xử lý hậu kỳ      │  │  ║
║              │  ├ phổ 1/3 octave     │ ║      ║  │  └────────────────────────┘  │  ║
║              │  ├ ghi thô            │ ║      ║  └───────▲──────────────┬───────┘  ║
║              │  └ đóng gói + mã hoá  │ ║      ║          │              │          ║
║              └───┬───────────────┬───┘ ║      ║  ┌───────┴──────────────▼───────┐  ║
║                  │               │     ║      ║  │ B5  Lưu trữ + báo cáo        │  ║
║      ┌───────────▼────┐   ┌──────▼───┐ ║      ║  └──────────────────────────────┘  ║
║      │ A5  Lưu trữ    │   │ A7 Vô    │ ║      ║  ┌──────────────────────────────┐  ║
║      │ (dữ liệu thô)  │   │ tuyến    │ ║      ║  │ B2  Modem + thu kênh nghe    │  ║
║      └───────┬────────┘   └────┬─────┘ ║      ║  └──────────────┬───────────────┘  ║
║              │                 │       ║      ║                 │                  ║
║  ┌───────────▼──────────────┐  │       ║      ╚═════════════════╪══════════════════╝
║  │ A9 Nguồn  │ A8 Kết cấu   │  │       ║                        │
║  └──────────────────────────┘  │       ║                        │
╚════════════════════════════════╪═══════╝                        │
                                 │                                │
                 ┌───────────────┴────────────────────────────────┘
                 │
    ═════════════╪═══════════════════════════════════════════════════
      A7.1 UHF   │  dữ liệu đã xử lý (lên) + lệnh cấu hình (xuống)
      A7.2 VHF   │  kênh nghe tương tự (lên)
    ═════════════╪═══════════════════════════════════════════════════
                 │
    ┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄
      SAU KHI THU HỒI PHAO:  A5 ──── Ethernet ────► B5
      Dữ liệu thô đầy đủ, phục vụ CĐ-4 phân tích băng hẹp
    ┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄
```

### 2.2 Chuỗi tín hiệu chi tiết — từ áp suất âm đến mức nguồn

```
   Áp suất âm p(t)  [µPa]
        │
        ▼
┌───────────────────┐
│ A1.1 Phần tử thu  │  Độ nhạy M  [dB re 1V/µPa]     → điện áp
│      PZT          │  Nguồn tín hiệu kiểu tụ điện
└─────────┬─────────┘
          ▼
┌───────────────────┐
│ A1.2 Tiền KĐ      │  Trở kháng vào rất cao
│      (tại chỗ)    │  Nhiễu ~3.3 nV/√Hz            ◄── ĐIỂM QUYẾT ĐỊNH YC-01
└─────────┬─────────┘      Đặt càng gần phần tử càng tốt
          ▼
┌───────────────────┐
│ A1.3 Cáp          │  Nhiễu ma sát điện (triboelectric)
└─────────┬─────────┘  ◄── Cáp phải là loại nhiễu thấp chuyên dụng
          ▼
┌───────────────────┐
│ A2.1 KĐ vi sai    │  Khử nhiễu đồng pha
└─────────┬─────────┘
          ▼
┌───────────────────┐
│ A2.2 PGA          │  Độ lợi lập trình được, bước rời rạc
└─────────┬─────────┘  ◄── Giá trị độ lợi PHẢI ghi vào metadata
          ▼
┌───────────────────┐
│ A2.3 Lọc chống    │  Bậc đủ để suy hao tại fs/2
│      chồng phổ    │
└─────────┬─────────┘
          ▼
┌───────────────────┐
│ A3.1 ADC 16 bit   │  fs = 42 kS/s [THAM CHIẾU]
└─────────┬─────────┘
          ▼
   ╔══════════════════════════════════════════╗
   ║  Mẫu số + nhãn thời gian GNSS + độ lợi   ║
   ╚══════════════════════════════════════════╝
          │
          ├──────────────────────────────┐
          ▼                              ▼
┌───────────────────┐          ┌───────────────────┐
│ A5 Ghi thô        │          │ A4.1 Xử lý phổ    │
│ (giữ nguyên vẹn)  │          │ 1/3 octave        │
└─────────┬─────────┘          └─────────┬─────────┘
          │                              ▼
          │                    ┌───────────────────┐
          │                    │ A7.1 Truyền UHF   │
          │                    └─────────┬─────────┘
          │ (sau thu hồi)                │ (thời gian thực)
          └──────────────┬───────────────┘
                         ▼
   ╔═══════════════════════════════════════════╗
   ║        SS-C  CHUỖI XỬ LÝ ĐO LƯỜNG         ║
   ╚═══════════════════════════════════════════╝
                         │
                         ▼
              ┌─────────────────────┐
              │ C1.1 Bù hiệu chuẩn  │  − M − G(độ lợi) + hệ số hiệu chuẩn
              └──────────┬──────────┘  ◄── BƯỚC TẠO RA TÍNH TRUY XUẤT
                         ▼
              ┌─────────────────────┐
              │ C1.2/C1.3 Phân tích │  1/3 octave (thời gian thực)
              │ phổ                 │  FFT băng hẹp (hậu kỳ)
              └──────────┬──────────┘
                         ▼
              ┌─────────────────────┐
              │ Mức áp suất âm tại  │  [dB re 1µPa]
              │ vị trí thuỷ âm      │
              └──────────┬──────────┘
                         ▼
              ┌─────────────────────┐   ◄── cự ly r(t) từ GNSS hai đầu
              │ C1.4 Chuẩn hoá      │   ◄── mô hình suy hao TL(r,f)
              │      theo cự ly     │   ◄── profile tốc độ âm
              └──────────┬──────────┘
                         ▼
   ╔═══════════════════════════════════════════╗
   ║   MỨC NGUỒN  SL  [dB re 1µPa @ 1m]        ║  ← SẢN PHẨM CUỐI
   ╚═══════════════════════════════════════════╝
```

**Phương trình đo cơ bản** — mọi kỹ sư trong dự án cần thuộc:

```
SL = SPL_đo  +  TL(r, f)

SPL_đo = V_ADC(dB)  −  M  −  G_tổng  +  K_hiệu_chuẩn

Trong đó:
  SL          Mức nguồn                      [dB re 1µPa @ 1m]
  SPL_đo      Mức áp suất âm tại thuỷ âm     [dB re 1µPa]
  TL(r,f)     Suy hao truyền âm               [dB]
  M           Độ nhạy thuỷ âm                 [dB re 1V/µPa]  ← từ hồ sơ hiệu chuẩn
  G_tổng      Tổng độ lợi chuỗi analog        [dB]            ← từ metadata
  K           Hệ số hiệu chuẩn hệ thống       [dB]            ← từ hồ sơ hiệu chuẩn
```

> **Ý nghĩa kỹ thuật của phương trình này:** sai số của **mỗi** số hạng cộng trực tiếp vào sai số của SL. Đây là lý do WP-1 (ngân sách sai số) phải làm trước, và là lý do metadata phải mang theo `G_tổng` — nếu PGA đổi độ lợi giữa chừng mà không ghi lại, toàn bộ đoạn dữ liệu đó **không dùng được**.

### 2.3 Sơ đồ luồng dữ liệu và ràng buộc băng thông

```
                          TỐC ĐỘ DỮ LIỆU  [TÍNH]

   A3 ADC ──────────► 42 000 mẫu/s × 16 bit = 672 kbit/s = 84 kB/s
                                                    │
              ┌─────────────────────────────────────┤
              ▼                                     ▼
   ┌────────────────────┐              ┌──────────────────────────┐
   │ Ghi thô vào A5     │              │ Rút gọn thành 1/3 octave │
   │ 84 kB/s            │              │ ~48 dải × 2 byte / 1.5 s │
   │ = 302 MB/giờ       │              │ ≈ 64 B/s ≈ 512 bit/s     │
   │ = 2.4 GB / 8 giờ   │              └────────────┬─────────────┘
   └────────────────────┘                           ▼
                                        ┌──────────────────────────┐
                                        │ A7.1 Modem UHF           │
                                        │ Nhu cầu: ~0.5 kbit/s     │
                                        │ Năng lực điển hình:      │
                                        │   9.6 – 115 kbit/s       │
                                        │ ⇒ DƯ THỪA LỚN            │
                                        └──────────────────────────┘

   TỶ SỐ RÚT GỌN:  672 kbit/s ÷ 0.5 kbit/s ≈ 1300 : 1
```

**Ba kết luận thiết kế rút ra:**

1. **Đường truyền vô tuyến không phải nút thắt** cho dữ liệu đã xử lý — dư băng thông rất lớn. Có thể cân nhắc truyền thêm dữ liệu (ví dụ phổ băng hẹp rút gọn, hoặc ảnh chụp nhanh phổ) mà không lo nghẽn.
2. **Đường truyền vô tuyến hoàn toàn không đủ** cho dữ liệu thô (cần 672 kbit/s liên tục, chưa kể chi phí giao thức và biên độ dự phòng suy hao). Kiến trúc "lấy dữ liệu thô sau thu hồi" của hệ tham chiếu là bắt buộc, không phải lựa chọn.
3. **Dung lượng lưu trữ không phải ràng buộc.** 2.4 GB cho 8 giờ là nhỏ so với thẻ nhớ công nghiệp hiện nay. Có thể tăng tần số lấy mẫu hoặc số kênh mà không lo về dung lượng — **ràng buộc thật là điện năng và băng thông ghi, không phải dung lượng**.

`⇄ HƯỚNG B` Nếu chuyển sang mảng nhiều phần tử, con số ở mục 1 nhân với số kênh. Với 64 kênh: 43 Mbit/s liên tục, 19 GB/giờ — lúc đó lưu trữ **trở thành** ràng buộc và bài toán đường truyền đổi hoàn toàn.

---

## 3. YÊU CẦU HỆ THỐNG

### 3.1 Phân cấp yêu cầu

```
   TC-1..TC-5      Tiêu chí thành công (Charter mục 2.3)
        │
        ▼
   YC-01..YC-13    Yêu cầu hiệu năng cấp hệ thống
   CN-01..CN-14    Yêu cầu chức năng cấp hệ thống
        │
        ▼
   ĐT-xx           Đặc tả cấp phân hệ (tài liệu này, mục 5)
```

### 3.2 Ma trận truy vết yêu cầu → phân hệ

| Yêu cầu | Nội dung tóm tắt | Phân hệ chịu trách nhiệm | Phân hệ liên quan |
|---|---|---|---|
| YC-01 | Nhiễu nội tại ≥10 dB dưới ngưỡng môi trường | **A2** | A1, A3, A9 |
| YC-02 | Dải tần phân tích trực tiếp | A1, A2, A3 | C1 |
| YC-03 | Dải tần phân tích lọc số | A3 | C1 |
| YC-04 | Dải động | **A2** | A3 |
| YC-05 | Độ chính xác gán nhãn thời gian | **A6** | A3, A4 |
| YC-06 | Chu kỳ cập nhật về trạm | A4 | A7 |
| YC-07 | Phân giải băng hẹp hậu kỳ | **C1.3** | A5 |
| YC-08 | Độ phân giải ADC | A3 | — |
| YC-09 | Tần số lấy mẫu | A3 | A4, A5 |
| YC-10 | Dải tần kênh nghe | A7.2 | — |
| YC-11 | Thời gian hoạt động | **A9** | A4, A7 |
| YC-12 | Thời gian nguồn dự phòng | A9.3 | — |
| YC-13 | Sai số tổng mức nguồn công bố | **C3** | A1, A2, A6, C1 |
| CN-01 | Ghi liên tục không mất mẫu | A4.2, A5 | — |
| CN-02 | Kênh nghe thời gian thực | A7.2 | B2 |
| CN-03 | Phân tích + hiển thị 1/3 octave | A4.1, C1.2 | B4.1 |
| CN-04 | Nhật ký GNSS | A6 | A5 |
| CN-05 | Tính cự ly, chuẩn hoá | **C1.4** | A6, B3 |
| CN-06 | Mã hoá lưu trữ và truyền | A4.3, A5 | B2 |
| CN-07 | Cấu hình từ xa | A7.1, A4 | B4 |
| CN-08 | Báo cáo trạng thái định kỳ | A4.3 | B4.1 |
| CN-09 | Nguồn dự phòng định vị | A9.3 | A8 |
| CN-10 | Lấy dữ liệu thô sau thu hồi | A5 | B5 |
| CN-11 | Phát lại | B4.2 | B5 |
| CN-12 | Tự kiểm tra BIT | **C4** | tất cả |
| CN-13 | Định vị đa phao | B4.3 | A6 |
| CN-14 | Xuất dữ liệu chuẩn | B5 | C2 |

### 3.3 Yêu cầu giao diện cấp hệ thống

| ID | Giao diện | Giữa | Loại | Đặc tả |
|---|---|---|---|---|
| GD-01 | Thuỷ âm → Front-end | A1 → A2 | Tương tự | Cáp nhiễu thấp; xem 5.1.4 |
| GD-02 | Front-end → ADC | A2 → A3 | Tương tự | Vi sai, mức phù hợp dải vào ADC |
| GD-03 | ADC → Xử lý | A3 → A4 | Số | Nối tiếp tốc độ cao (SPI/LVDS) |
| GD-04 | GNSS → Xử lý | A6 → A4 | Số + xung | NMEA + PPS |
| GD-05 | Xử lý → Lưu trữ | A4 → A5 | Số | Ghi khối, có kiểm tra toàn vẹn |
| GD-06 | Phao ↔ Trạm (dữ liệu) | A7.1 ↔ B2 | RF số | UHF, hai chiều, mã hoá |
| GD-07 | Phao → Trạm (nghe) | A7.2 → B2 | RF tương tự | VHF, một chiều |
| GD-08 | Phao → Trạm (thu hồi) | A5 → B5 | Số | Ethernet, chỉ khi phao đã lên tàu |
| GD-09 | Nguồn | A9 → tất cả | Điện | Xem 5.9 |
| GD-10 | Dữ liệu → bên thứ ba | B5 → ngoài | File | Định dạng theo C2 |

---

## 4. NGÂN SÁCH THIẾT KẾ SƠ BỘ

Các con số dưới đây là **`[TÍNH]` sơ bộ để định hướng**, không thay thế WP-1.

### 4.1 Ngân sách nhiễu — khung tính

```
Nguồn nhiễu                          Đóng góp        Ghi chú
─────────────────────────────────────────────────────────────────────────
1. Nhiễu môi trường biển             THAM CHIẾU      Mốc so sánh — hệ thống
   (Knudsen / Wenz, phụ thuộc                        phải nằm dưới mốc này
    trạng thái biển và giao thông)                   ít nhất 10 dB

2. Nhiễu nhiệt phần tử thu           TBD             Phụ thuộc điện dung
                                                     và điện trở tổn hao

3. Nhiễu áp tiền khuếch đại          ~3.3 nV/√Hz     [THAM CHIẾU]
                                                     Thường trội ở dải cao

4. Nhiễu dòng tiền khuếch đại        TBD             Trội ở dải thấp với
                                                     nguồn trở kháng cao
                                                     ◄ QUAN TRỌNG ở 3 Hz

5. Nhiễu cáp (ma sát điện)           TBD             Chỉ xuất hiện khi cáp
                                                     rung/uốn ◄ xem 5.1.4

6. Nhiễu lượng tử hoá ADC            [TÍNH] xem 4.2

7. Nhiễu nguồn / nhiễu số nội bộ     TBD             Do bố trí mạch quyết định
─────────────────────────────────────────────────────────────────────────
YÊU CẦU: tổng (2..7)  ≤  (1) − 10 dB          ← YC-01
```

> **Lưu ý cho kỹ sư analog:** ở đầu dải tần thấp (3–20 Hz) với nguồn tín hiệu trở kháng rất cao như thuỷ âm PZT, **nhiễu dòng của tiền khuếch đại thường trội hơn nhiễu áp**. Chọn linh kiện chỉ theo chỉ tiêu nV/√Hz là sai lầm phổ biến. Phải xét cả hai, và xét tại tần số thấp nhất của dải công tác.

### 4.2 Dải động và số bit

```
ADC 16 bit lý tưởng:
   SNR = 6.02 × 16 + 1.76  =  98.1 dB          [TÍNH]
   Thực tế (ENOB ~14.5 bit): ≈ 89 dB           [TÍNH]

Dải động cần phủ (YC-04):
   Từ: nhiễu nền biển yên tĩnh
   Đến: vật thử rất ồn ở cự ly gần
   Khoảng cách ước tính: TBD  ◄── PHẢI CÓ SỐ TRONG WP-1

Nếu  dải_động_cần  >  89 dB  ⇒  bắt buộc dùng PGA nhiều nấc
                                (không thể phủ bằng một độ lợi cố định)
```

**Đây chính là lý do kỹ thuật vì sao hệ tham chiếu dùng "khuếch đại điều khiển được".** Không phải tiện ích — mà là bắt buộc do dải động vật lý vượt quá dải động của ADC.

**Hệ quả vận hành:** khi PGA đổi nấc giữa buổi đo sẽ có gián đoạn. Cần quyết định: đổi tự động (tiện, nhưng phải ghi log chặt và xử lý vùng chuyển tiếp) hay đặt cố định trước mỗi lần đo (an toàn hơn cho đo lường, kém linh hoạt hơn). → **QĐ-9** (mục 7).

### 4.3 Ảnh hưởng của sai số vị trí GNSS lên kết quả đo

Sai số cự ly truyền vào mức nguồn qua số hạng suy hao. Với mô hình lan truyền cầu (TL = 20·log₁₀ r):

| Cự ly thực | Sai số vị trí | Sai số mức nguồn `[TÍNH]` |
|---|---|---|
| 50 m | ±5 m | ±0.83 dB |
| 100 m | ±5 m | ±0.42 dB |
| 200 m | ±5 m | ±0.21 dB |
| 50 m | ±0.5 m (RTK) | ±0.09 dB |
| 100 m | ±0.5 m (RTK) | ±0.04 dB |

**Ba kết luận:**

1. Sai số vị trí **quan trọng hơn ở cự ly gần**. Nếu quy trình đo yêu cầu chạy sát phao, sai số GNSS thường sẽ trở thành một trong các số hạng lớn nhất.
2. **GNSS thường (±5 m) có thể đủ** nếu YC-13 đặt ở mức ±1 dB trở lên và cự ly đo ≥100 m. Nếu YC-13 chặt hơn, phải cân nhắc RTK.
3. Không thể quyết định cấp chính xác GNSS trước khi có YC-13. → **phụ thuộc QĐ-2**.

### 4.4 Quan hệ đồng bộ thời gian ↔ độ chính xác định vị (chế độ CN-13)

```
   Δd  =  c × Δt          với c ≈ 1500 m/s trong nước biển

   Δt = 1 ms   ⇒  Δd = 1.5 m      [THAM CHIẾU — mức của TMK-SAS]
   Δt = 100 µs ⇒  Δd = 0.15 m
   Δt = 1 µs   ⇒  Δd = 1.5 mm
```

Xung PPS của máy thu GNSS phổ thông có sai số cỡ vài chục nano-giây — **tốt hơn yêu cầu 1 ms khoảng 4 bậc**. Nghĩa là: nút thắt của độ chính xác định vị **không nằm ở GNSS** mà nằm ở:

- độ trễ và độ ổn định của chuỗi analog (A2) và ADC (A3),
- độ chính xác gán nhãn mẫu trong phần mềm (A4),
- và quan trọng nhất: **sai số của mô hình tốc độ âm** — nếu c sai 1%, cự ly sai 1%.

> Đây là một cạm bẫy điển hình: đội thiết kế đầu tư vào GNSS chính xác cao rồi mất độ chính xác ở khâu gán nhãn mẫu trong phần mềm. Phải kiểm soát toàn chuỗi.

### 4.5 Ngân sách điện năng — khung

| Khối | Ước tính | Trạng thái |
|---|---|---|
| A1.2 Tiền khuếch đại | TBD | `[MỞ]` |
| A2 Analog front-end | TBD | `[MỞ]` |
| A3 ADC | TBD | `[MỞ]` |
| A4 Xử lý | TBD — **thường là khối tiêu thụ lớn nhất** | `[MỞ]` |
| A5 Lưu trữ | TBD | `[MỞ]` |
| A6 GNSS | TBD | `[MỞ]` |
| A7.1 Modem UHF (phát) | TBD — theo chu kỳ, không liên tục | `[MỞ]` |
| A7.2 Kênh nghe VHF (phát) | TBD — **chỉ khi trắc thủ kích hoạt** | `[MỞ]` |
| **Tổng** | **TBD** → quyết định dung lượng ắc quy và YC-11 | `[MỞ]` |

**Lưu ý thiết kế:** kênh nghe VHF phát tương tự liên tục sẽ tốn điện đáng kể. Nếu để bật suốt buổi đo, ngân sách điện năng đổi hoàn toàn so với khi chỉ bật theo yêu cầu. → cần làm rõ trong WP-4.

---

## 5. ĐẶC TẢ TỪNG PHÂN HỆ

### 5.1 A1 — CỤM THUỶ ÂM

#### 5.1.1 Chức năng
Chuyển đổi áp suất âm trong nước thành tín hiệu điện, khuếch đại tại chỗ, truyền về khối front-end với suy giảm chất lượng tối thiểu.

#### 5.1.2 Sơ đồ khối
```
   Nước ──► ┌──────────┐   ┌──────────┐   ┌──────────┐   ┌──────────┐
            │ A1.1     │──►│ A1.2     │──►│ A1.3     │──►│ tới A2   │
            │ Phần tử  │   │ Tiền KĐ  │   │ Cáp      │   │          │
            │ PZT      │   │ tại chỗ  │   │ nhiễu    │   │          │
            └────▲─────┘   └──────────┘   │ thấp     │   └──────────┘
                 │                         └──────────┘
            ┌────┴─────┐
            │ A1.4     │  Cơ cấu treo giảm chấn
            │ Giảm chấn│  ◄── tách chuyển động phao khỏi phần tử thu
            └──────────┘
```

#### 5.1.3 Đặc tả

| ID | Tham số | Giá trị | Trạng thái |
|---|---|---|---|
| ĐT-A1-01 | Vật liệu phần tử | PZT-5A hoặc PZT-5H | `[GIẢ ĐỊNH]` |
| ĐT-A1-02 | Số phần tử | 1 | `[GIẢ ĐỊNH]` ⇄ HƯỚNG B: mảng |
| ĐT-A1-03 | Tính định hướng | Vô hướng (omnidirectional) | `[CHỐT]` cho Hướng A |
| ĐT-A1-04 | Dải tần | Theo YC-02/YC-03 | `[MỞ]` |
| ĐT-A1-05 | Độ nhạy M | TBD — điển hình −180…−200 dB re 1V/µPa | `[MỞ]` |
| ĐT-A1-06 | Độ phẳng đáp ứng trong dải | TBD — ảnh hưởng trực tiếp YC-13 | `[MỞ]` |
| ĐT-A1-07 | Độ sâu làm việc | TBD | `[MỞ]` |
| ĐT-A1-08 | Hiệu chuẩn | Truy xuất được về chuẩn quốc gia | `[CHỐT]` |
| ĐT-A1-09 | Chu kỳ hiệu chuẩn lại | TBD | `[MỞ]` → QĐ-6 |

#### 5.1.4 Lưu ý thiết kế bắt buộc

**(a) Nhiễu do chuyển động — vấn đề đặc trưng của cấu hình phao nổi.**
Phao nổi nhấp nhô theo sóng. Nếu phần tử thu gắn cứng với phao, chuyển động này truyền xuống và sinh nhiễu ở đúng dải tần thấp mà ta quan tâm (một số ứng dụng cần tới 3 Hz — trùng dải sóng biển). Bắt buộc phải có **cơ cấu treo mềm (A1.4)** để tách cơ học phần tử thu khỏi thân phao.

Đây là hạng mục hay bị bỏ sót trong thiết kế lần đầu và rất tốn kém để sửa sau khi đã ra biển.

**(b) Nhiễu ma sát điện của cáp.**
Cáp nối thuỷ âm sinh điện tích khi bị uốn hoặc rung. Với nguồn tín hiệu trở kháng cao, hiệu ứng này tạo nhiễu đáng kể. Bắt buộc dùng **cáp thuỷ âm chuyên dụng nhiễu thấp**, không dùng cáp đồng trục thông thường.

**(c) Đặt tiền khuếch đại càng gần phần tử càng tốt.**
Phần tử PZT là nguồn trở kháng rất cao; mọi điện dung ký sinh trên đường dây trước tầng khuếch đại đều làm suy giảm tín hiệu và tăng nhiễu. Tiền khuếch đại nên nằm ngay trong cụm thuỷ âm.

---

### 5.2 A2 — KHỐI ANALOG FRONT-END

#### 5.2.1 Chức năng
Khuếch đại tín hiệu tới mức phù hợp dải vào ADC, với độ lợi điều chỉnh được để phủ dải động yêu cầu, đồng thời giới hạn băng thông chống chồng phổ.

**Đây là phân hệ quyết định YC-01 và YC-04.**

#### 5.2.2 Sơ đồ khối
```
  từ A1 ──►┌─────────┐  ┌─────────┐  ┌─────────┐  ┌─────────┐
           │ A2.4    │─►│ A2.1    │─►│ A2.2    │─►│ A2.3    │──► tới A3
           │ Bảo vệ  │  │ KĐ vi   │  │ PGA     │  │ Lọc     │
           │ quá áp  │  │ sai     │  │         │  │ chống   │
           └─────────┘  └─────────┘  └────▲────┘  │ chồng   │
                                          │       │ phổ     │
                                    điều khiển     └─────────┘
                                    độ lợi từ A4
                                          │
                                          ▼
                              ┌───────────────────────┐
                              │ Giá trị độ lợi PHẢI   │
                              │ ghi vào metadata      │ ◄── BẮT BUỘC
                              │ cùng nhãn thời gian   │
                              └───────────────────────┘
```

#### 5.2.3 Đặc tả

| ID | Tham số | Giá trị | Trạng thái |
|---|---|---|---|
| ĐT-A2-01 | Mật độ nhiễu áp quy về đầu vào | ≤ 3.3 nV/√Hz | `[THAM CHIẾU]` |
| ĐT-A2-02 | Mật độ nhiễu dòng quy về đầu vào | TBD — **quan trọng ở tần số thấp** | `[MỞ]` |
| ĐT-A2-03 | Trở kháng vào | Cao — phù hợp nguồn kiểu tụ | `[CHỐT]` |
| ĐT-A2-04 | Dải điều chỉnh độ lợi | ~40 dB | `[THAM CHIẾU]` |
| ĐT-A2-05 | Bước điều chỉnh độ lợi | TBD — nên là bước rời rạc đã hiệu chuẩn | `[MỞ]` |
| ĐT-A2-06 | Sai số độ lợi từng nấc | TBD — **cộng thẳng vào YC-13** | `[MỞ]` |
| ĐT-A2-07 | Bậc lọc chống chồng phổ | TBD — theo fs và yêu cầu suy hao | `[MỞ]` |
| ĐT-A2-08 | Độ phẳng đáp ứng trong dải | TBD | `[MỞ]` |
| ĐT-A2-09 | Méo hài tổng | TBD | `[MỞ]` |

#### 5.2.4 Lưu ý thiết kế bắt buộc

**Sai số độ lợi là sai số đo.** Nếu PGA công bố 20 dB nhưng thực tế 20.3 dB, toàn bộ phép đo lệch 0.3 dB. Hai cách xử lý — chọn một, ghi vào thiết kế:
- Hiệu chuẩn từng nấc độ lợi của từng thiết bị, lưu vào hồ sơ hiệu chuẩn (chính xác hơn, tốn công);
- Dùng linh kiện có sai số độ lợi đủ nhỏ so với YC-13 (đơn giản hơn, đắt hơn).

**Không được để việc chuyển nấc độ lợi diễn ra âm thầm.** Mỗi lần đổi phải ghi log kèm nhãn thời gian; phần mềm phân tích phải xử lý được vùng chuyển tiếp (thường loại bỏ vài chục ms quanh thời điểm chuyển).

---

### 5.3 A3 — KHỐI SỐ HOÁ

#### 5.3.1 Chức năng
Chuyển tín hiệu tương tự thành mẫu số, gán nhãn thời gian chính xác theo GNSS.

#### 5.3.2 Sơ đồ khối
```
   từ A2 ──►┌──────────────┐
            │ A3.1 ADC     │──► mẫu dải cơ bản ──┐
            │ dải cơ bản   │                     │
            └──────▲───────┘                     │
                   │                             ├──► tới A4
            ┌──────┴───────┐                     │
            │ A3.3 Đồng hồ │                     │
            │ lấy mẫu      │◄── PPS từ A6        │
            │ + đồng bộ    │                     │
            └──────────────┘                     │
                                                 │
   từ A2 ──►┌──────────────┐                     │
            │ A3.2 Nhánh   │──► dữ liệu dải cao ─┘
            │ dải cao      │
            │ ⚠ KIẾN TRÚC  │
            │ CHƯA CHỐT    │
            └──────────────┘
```

#### 5.3.3 Đặc tả

| ID | Tham số | Giá trị | Trạng thái |
|---|---|---|---|
| ĐT-A3-01 | Độ phân giải | 16 bit | `[THAM CHIẾU]` → QĐ-7 |
| ĐT-A3-02 | Số bit hiệu dụng (ENOB) | TBD — **chỉ tiêu thật, không phải số bit danh nghĩa** | `[MỞ]` |
| ĐT-A3-03 | Tần số lấy mẫu dải cơ bản | 42 kS/s | `[THAM CHIẾU]` → QĐ-1 |
| ĐT-A3-04 | Kiến trúc ADC | SAR hoặc Sigma-Delta | `[MỞ]` |
| ĐT-A3-05 | Sai số nhãn thời gian mẫu | < 1 ms | `[THAM CHIẾU]` → YC-05 |
| ĐT-A3-06 | Độ ổn định đồng hồ lấy mẫu (jitter) | TBD | `[MỞ]` |

#### 5.3.4 ⚠ Vấn đề kiến trúc chưa giải — nhánh dải cao

Tài liệu tham chiếu nêu: lấy mẫu liên tục **42 kS/s** cho dải **3 Hz – 20 kHz**, và dải **20 – 100 kHz** được "phân tích bằng lọc số".

**Hai điều này không tự nhất quán nếu chỉ có một đường lấy mẫu.** Theo định lý lấy mẫu, 42 kS/s chỉ biểu diễn được tới ~21 kHz. Muốn xử lý số dải tới 100 kHz cần tốc độ lấy mẫu ≥200 kS/s.

Do đó hệ tham chiếu phải dùng một trong hai kiến trúc — tài liệu công bố không nói rõ:

| Phương án | Mô tả | Ưu | Nhược |
|---|---|---|---|
| **PA-1** | ADC thứ hai chạy ≥200 kS/s cho dải cao; ghi thô chỉ dải cơ bản | Giữ được dạng sóng dải cao, xử lý linh hoạt | Tốn điện, tốn dữ liệu |
| **PA-2** | Dàn lọc analog + tách đường bao, rồi số hoá chậm các mức dải | Rất tiết kiệm điện và dữ liệu | Mất dạng sóng — **không phân tích lại được** |

> **Đây là quyết định kiến trúc cần chốt sớm (QĐ-10).** Nó ảnh hưởng tới A2, A3, A5 và tới việc chế độ CĐ-4 có phân tích lại được dải cao hay không. Cần trả lời trước: **các ứng dụng UD-1…UD-6 có thực sự cần dải tới 100 kHz không?** Nếu chữ ký tàu chủ yếu nằm dưới 20 kHz, PA-2 là đủ và rẻ hơn nhiều.

---

### 5.4 A4 — KHỐI XỬ LÝ

#### 5.4.1 Chức năng
Chạy phân tích phổ thời gian thực, quản lý ghi dữ liệu thô không mất mẫu, đóng gói và mã hoá bản tin, nhận và thực thi lệnh cấu hình.

#### 5.4.2 Sơ đồ khối
```
   từ A3 ──►┌────────────────────────────────────────────┐
            │                A4                          │
            │  ┌──────────────┐    ┌──────────────────┐  │
            │  │ A4.2 Ghi dữ  │───►│ tới A5 lưu trữ   │  │
            │  │ liệu thô     │    └──────────────────┘  │
            │  │ (ưu tiên cao)│                          │
            │  └──────────────┘                          │
            │  ┌──────────────┐    ┌──────────────────┐  │
            │  │ A4.1 Phân    │───►│ A4.3 Đóng gói    │──┼──► tới A7.1
            │  │ tích 1/3     │    │ + mã hoá         │  │
            │  │ octave       │    └──────────────────┘  │
            │  └──────────────┘                          │
            │  ┌──────────────────────────────────────┐  │
            │  │ Nhận lệnh cấu hình ◄─────────────────┼──┼─── từ A7.1
            │  │ Điều khiển độ lợi A2                 │  │
            │  │ Giám sát sức khoẻ (BIT)              │  │
            │  └──────────────────────────────────────┘  │
            └────────────────────────────────────────────┘
```

#### 5.4.3 Đặc tả

| ID | Tham số | Giá trị | Trạng thái |
|---|---|---|---|
| ĐT-A4-01 | Chu kỳ cập nhật kết quả | 1.5 s | `[THAM CHIẾU]` → YC-06 |
| ĐT-A4-02 | Số dải 1/3 octave | ~48 dải cho 3 Hz–100 kHz | `[TÍNH]` |
| ĐT-A4-03 | Bảo đảm không mất mẫu khi ghi | Bắt buộc | `[CHỐT]` CN-01 |
| ĐT-A4-04 | Mã hoá bản tin truyền | Bắt buộc | `[CHỐT]` CN-06 |
| ĐT-A4-05 | Công suất tiêu thụ | TBD — thường là khối lớn nhất | `[MỞ]` |
| ĐT-A4-06 | Nền tảng tính toán | TBD — vi xử lý / DSP / FPGA | `[MỞ]` |

#### 5.4.4 Lưu ý thiết kế

**Ghi dữ liệu thô phải có mức ưu tiên cao hơn phân tích thời gian thực.** Nếu tài nguyên thiếu, chấp nhận trễ hoặc bỏ một chu kỳ cập nhật hiển thị — **không bao giờ** chấp nhận mất mẫu dữ liệu thô. Dữ liệu thô mất là mất vĩnh viễn; một chu kỳ hiển thị trễ thì trắc thủ vẫn làm việc được.

**Phân tích 1/3 octave nên theo chuẩn dàn lọc quốc tế** (xem mục 8) để kết quả so sánh được với thiết bị khác và với số liệu công bố của bên thứ ba.

---

### 5.5 A5 — LƯU TRỮ

| ID | Tham số | Giá trị | Trạng thái |
|---|---|---|---|
| ĐT-A5-01 | Tốc độ ghi liên tục | ~84 kB/s (dải cơ bản, 1 kênh) | `[TÍNH]` |
| ĐT-A5-02 | Dung lượng cho 8 giờ | ~2.4 GB | `[TÍNH]` |
| ĐT-A5-03 | Dung lượng thiết kế | TBD — theo YC-11 + hệ số dự phòng | `[MỞ]` |
| ĐT-A5-04 | Mã hoá dữ liệu lưu | Bắt buộc | `[CHỐT]` CN-06 |
| ĐT-A5-05 | Chịu rung, chịu nhiệt | Cấp công nghiệp | `[CHỐT]` |
| ĐT-A5-06 | Giao diện lấy dữ liệu | Ethernet, sau thu hồi | `[CHỐT]` CN-10 |
| ĐT-A5-07 | Toàn vẹn dữ liệu | Có kiểm tra, phát hiện được hỏng | `[CHỐT]` |

**Lưu ý:** dung lượng **không** phải ràng buộc (xem 2.3). Ràng buộc thật là độ tin cậy ghi và khả năng chịu môi trường. Ưu tiên linh kiện cấp công nghiệp hơn là dung lượng lớn.

---

### 5.6 A6 — GNSS VÀ ĐỒNG BỘ THỜI GIAN

| ID | Tham số | Giá trị | Trạng thái |
|---|---|---|---|
| ĐT-A6-01 | Sai số vị trí | TBD — xem phân tích 4.3 | `[MỞ]` → phụ thuộc QĐ-2 |
| ĐT-A6-02 | Có ngõ ra PPS | Bắt buộc | `[CHỐT]` |
| ĐT-A6-03 | Sai số PPS | Cỡ chục ns với máy thu phổ thông | `[THAM CHIẾU]` |
| ĐT-A6-04 | Tần suất ghi vệt vị trí | TBD | `[MỞ]` |
| ĐT-A6-05 | Lưu vệt vị trí trong phao | Bắt buộc | `[CHỐT]` CN-04 |
| ĐT-A6-06 | Cấp RTK | TBD | `[MỞ]` → phụ thuộc QĐ-2 |

**Lưu ý:** vệt vị trí phải được lưu **trong phao**, không chỉ truyền về trạm — nếu mất liên lạc giữa chừng, dữ liệu vị trí vẫn còn để chuẩn hoá về sau.

---

### 5.7 A7 — LIÊN LẠC VÔ TUYẾN

#### 5.7.1 Đặc tả

| ID | Tham số | A7.1 Kênh dữ liệu | A7.2 Kênh nghe |
|---|---|---|---|
| ĐT-A7-01 | Dải tần | UHF `[THAM CHIẾU]` | VHF `[THAM CHIẾU]` |
| ĐT-A7-02 | Chiều truyền | Hai chiều | Một chiều (phao → trạm) |
| ĐT-A7-03 | Loại | Số | Tương tự |
| ĐT-A7-04 | Băng thông tín hiệu | ~0.5 kbit/s cần `[TÍNH]` | ~100 Hz – 3 kHz `[THAM CHIẾU]` |
| ĐT-A7-05 | Cự ly làm việc | TBD | TBD |
| ĐT-A7-06 | Mã hoá | Bắt buộc `[CHỐT]` | TBD — tương tự khó mã hoá |
| ĐT-A7-07 | Cấp phép tần số | **Bắt buộc làm rõ** | **Bắt buộc làm rõ** |

#### 5.7.2 Vấn đề mở

**(a) Kênh nghe tương tự và yêu cầu mã hoá mâu thuẫn nhau.** CN-06 yêu cầu mã hoá; một kênh thoại tương tự thì không mã hoá được theo cách thông thường. Ba lối ra: chấp nhận kênh nghe không mã hoá (và ghi rõ hạn chế này), chuyển kênh nghe sang dạng số (tốn băng thông hơn nhưng mã hoá được), hoặc bỏ kênh nghe. → **QĐ-11**.

**(b) Cấp phép tần số** phải làm rõ sớm. Đây là ràng buộc pháp lý, không phải kỹ thuật, và có thể mất nhiều thời gian hơn phần thiết kế.

---

### 5.8 A8 — KẾT CẤU PHAO

| ID | Tham số | Giá trị | Trạng thái |
|---|---|---|---|
| ĐT-A8-01 | Độ nổi dự trữ | TBD | `[MỞ]` |
| ĐT-A8-02 | Ổn định trong trạng thái biển thiết kế | TBD | `[MỞ]` → QĐ-8 |
| ĐT-A8-03 | Chiều cao cột ăng-ten | TBD — ảnh hưởng cự ly liên lạc | `[MỞ]` |
| ĐT-A8-04 | Cấp bảo vệ khoang điện tử | TBD | `[MỞ]` |
| ĐT-A8-05 | Chống ăn mòn nước biển | Bắt buộc | `[CHỐT]` |
| ĐT-A8-06 | Cơ cấu thả / thu hồi | Thao tác được bởi đội nhỏ trên tàu | `[CHỐT]` |
| ĐT-A8-07 | Đèn / phương tiện định vị khi thu hồi | Bắt buộc | `[CHỐT]` CN-09 |
| ĐT-A8-08 | Giảm chấn cụm thuỷ âm | Bắt buộc — xem 5.1.4(a) | `[CHỐT]` |
| ĐT-A8-09 | Khối lượng, kích thước vận chuyển | Theo kiện K-1 | `[MỞ]` |

**Ràng buộc thiết kế chi phối:** cột ăng-ten cao thì liên lạc xa hơn nhưng phao lắc nhiều hơn, mà lắc thì sinh nhiễu chuyển động xuống cụm thuỷ âm. Ba yêu cầu ĐT-A8-02, ĐT-A8-03 và ĐT-A8-08 phải được thiết kế **cùng nhau**, không tách rời.

---

### 5.9 A9 — NGUỒN ĐIỆN

| ID | Tham số | Giá trị | Trạng thái |
|---|---|---|---|
| ĐT-A9-01 | Thời gian cấp nguồn chính | TBD | `[MỞ]` → YC-11 |
| ĐT-A9-02 | Thời gian nguồn dự phòng định vị | TBD | `[MỞ]` → YC-12 |
| ĐT-A9-03 | Loại ắc quy | TBD | `[MỞ]` |
| ĐT-A9-04 | Cách ly nhiễu nguồn khỏi chuỗi analog | **Bắt buộc** | `[CHỐT]` |
| ĐT-A9-05 | Chỉ báo dung lượng còn lại | Báo về trạm | `[CHỐT]` |
| ĐT-A9-06 | An toàn vận chuyển | Theo quy định vận chuyển pin | `[CHỐT]` |

**Lưu ý bắt buộc:** bộ chuyển đổi nguồn kiểu xung là nguồn nhiễu đáng kể đối với chuỗi analog nhiễu thấp. Phải tách nguồn analog và nguồn số, cân nhắc ổn áp tuyến tính cho tầng đầu vào, và đưa nhiễu nguồn vào ngân sách nhiễu WP-1. **Đây là một trong các nguyên nhân phổ biến nhất khiến không đạt YC-01.**

---

### 5.10 SS-B — TRẠM ĐIỀU KHIỂN

#### 5.10.1 Sơ đồ khối
```
   ┌────────────────────────────────────────────────────────┐
   │                    SS-B  SCS                           │
   │  ┌──────────┐   ┌──────────────────────────────────┐   │
   │  │ B2 Modem │──►│ B4 Phần mềm trắc thủ             │   │
   │  │ + thu    │   │                                  │   │
   │  │ kênh     │   │  B4.1 CĐ-1 Giám sát thu thập     │   │
   │  │ nghe     │◄──│       ├ hiển thị 1/3 octave       │   │
   │  └──────────┘   │       ├ quy về mức phổ thực       │   │
   │  ┌──────────┐   │       └ kích hoạt kênh nghe       │   │
   │  │ B3 GNSS  │──►│  B4.2 CĐ-2 Phát lại              │   │
   │  │ trạm     │   │  B4.3 CĐ-3 Định vị sự kiện (opt) │   │
   │  └──────────┘   │  B4.4 CĐ-4 Xử lý hậu kỳ          │   │
   │                 └────────────┬─────────────────────┘   │
   │  ┌──────────┐   ┌────────────▼─────────────────────┐   │
   │  │ B1 Máy   │   │ B5 Lưu trữ + xuất báo cáo        │   │
   │  │ tính     │   └──────────────────────────────────┘   │
   │  └──────────┘             ▲                            │
   └───────────────────────────┼────────────────────────────┘
                               │ Ethernet (sau thu hồi phao)
                               └──── từ A5
```

#### 5.10.2 Đặc tả phần mềm theo chế độ

| Chế độ | Chức năng bắt buộc | Trạng thái |
|---|---|---|
| **CĐ-1 Giám sát** | Hiển thị dải 1/3 octave theo chu kỳ cập nhật; quy về mức phổ thực; hiển thị vị trí và cự ly tức thời; kích hoạt kênh nghe; hiển thị trạng thái sức khoẻ phao; đánh dấu sự kiện | `[CHỐT]` |
| **CĐ-2 Phát lại** | Phát lại dữ liệu đã xử lý đồng bộ với tín hiệu tương tự đã ghi | `[CHỐT]` |
| **CĐ-3 Định vị sự kiện** | Nhận sự kiện gán nhãn thời gian từ nhiều phao; giải bài toán định vị | `[GIẢ ĐỊNH]` → QĐ-3 |
| **CĐ-4 Hậu kỳ** | Đọc dữ liệu thô; phân tích băng hẹp; chuẩn hoá cự ly; xuất báo cáo chữ ký | `[CHỐT]` |

#### 5.10.3 Đặc tả nền tảng

| ID | Tham số | Giá trị | Trạng thái |
|---|---|---|---|
| ĐT-B-01 | Loại máy tính | Laptop cấp bền, dùng được trên tàu | `[GIẢ ĐỊNH]` |
| ĐT-B-02 | Nguồn điện | Hoạt động được từ nguồn tàu và từ pin | `[GIẢ ĐỊNH]` |
| ĐT-B-03 | Hiển thị ngoài trời | Đọc được dưới ánh nắng | `[GIẢ ĐỊNH]` |
| ĐT-B-04 | Lưu trữ trạm | Đủ cho toàn bộ dữ liệu một chiến dịch đo | `[MỞ]` |
| ĐT-B-05 | Định dạng xuất | Theo C2 | `[MỞ]` |

---

### 5.11 SS-C — NỀN TẢNG XỬ LÝ VÀ HIỆU CHUẨN

#### 5.11.1 C1 — Thư viện xử lý tín hiệu

| ID | Khối | Đặc tả | Trạng thái |
|---|---|---|---|
| ĐT-C1-01 | C1.1 Bù hiệu chuẩn | Áp dụng M, G, K theo phương trình mục 2.2 | `[CHỐT]` |
| ĐT-C1-02 | C1.2 Phân tích 1/3 octave | Theo chuẩn dàn lọc quốc tế | `[CHỐT]` |
| ĐT-C1-03 | C1.3 Phân tích băng hẹp | Phân giải 1.5 Hz `[THAM CHIẾU]` | → YC-07 |
| ĐT-C1-04 | Chiều dài cửa sổ FFT tương ứng | ≈0.67 s ⇒ ~28 000 mẫu @42 kS/s ⇒ FFT 32768 điểm | `[TÍNH]` |
| ĐT-C1-05 | C1.4 Chuẩn hoá cự ly | Mô hình TL(r,f) + vệt GNSS + SVP | `[CHỐT]` |
| ĐT-C1-06 | Mô hình suy hao truyền âm | TBD — **nguồn sai số lớn ở nước nông** | `[MỞ]` |
| ĐT-C1-07 | C1.5 Thư viện chữ ký | Định dạng bản ghi, tiêu chí đối chiếu | `[MỞ]` |
| ĐT-C1-08 | Tính nhất quán hai chuỗi xử lý | Chuỗi trong phao và chuỗi trên trạm phải cho **cùng kết quả** trên cùng dữ liệu | `[CHỐT]` |

> **ĐT-C1-08 là một yêu cầu kiểm thử, không phải mong muốn.** Phải có bộ dữ liệu kiểm chuẩn và bài kiểm thử hồi quy tự động so sánh hai chuỗi. Nếu hai chuỗi lệch nhau, mọi kết quả giám sát thời gian thực đều mất giá trị đối chứng.

#### 5.11.2 C2 — Định dạng dữ liệu và metadata

Metadata **bắt buộc** đi kèm mỗi file dữ liệu thô:

| Trường | Nội dung | Bắt buộc |
|---|---|---|
| Định danh thiết bị | Số hiệu phao, số hiệu thuỷ âm | ✔ |
| Hồ sơ hiệu chuẩn | M, K, ngày hiệu chuẩn, đơn vị hiệu chuẩn, số chứng chỉ | ✔ |
| Cấu hình thu | fs, độ phân giải, dải tần | ✔ |
| Vệt độ lợi | G theo thời gian, mọi lần chuyển nấc | ✔ |
| Vệt GNSS | Vị trí phao theo thời gian | ✔ |
| Nhãn thời gian | Gốc thời gian GNSS | ✔ |
| Điều kiện môi trường | SVP, trạng thái biển, nhiệt độ nếu có | Nên có |
| Cấu hình cảm biến | Số kênh, hình học (cho Hướng B) | ✔ |

> **Nguyên tắc:** một file dữ liệu thô phải **tự mô tả đầy đủ** — người phân tích 3 năm sau, không có mặt lúc đo, vẫn phải quy được ra mức nguồn có truy xuất. Nếu phải tra cứu một bảng ở nơi khác mới hiểu được file, định dạng đó chưa đạt.

#### 5.11.3 C3 — Hiệu chuẩn và truy xuất chuẩn

| ID | Tham số | Giá trị | Trạng thái |
|---|---|---|---|
| ĐT-C3-01 | Phương pháp hiệu chuẩn thuỷ âm | TBD — so sánh hoặc tương hỗ | `[MỞ]` → WP-3 |
| ĐT-C3-02 | Chuỗi truy xuất | Về chuẩn quốc gia / quốc tế | `[CHỐT]` TC-4 |
| ĐT-C3-03 | Chu kỳ hiệu chuẩn lại | TBD | `[MỞ]` → QĐ-6 |
| ĐT-C3-04 | Hiệu chuẩn chuỗi điện (độ lợi A2) | Từng nấc, từng thiết bị | `[GIẢ ĐỊNH]` |
| ĐT-C3-05 | Ngân sách độ không đảm bảo đo | **Bắt buộc — là đầu ra chính của WP-1** | `[MỞ]` → YC-13 |

#### 5.11.4 C4 — Tự kiểm tra (BIT)

| Hạng mục kiểm tra | Khi nào | Phát hiện được gì |
|---|---|---|
| Bơm tín hiệu chuẩn vào chuỗi analog | Khởi động + định kỳ | Trôi độ lợi, hỏng tầng khuếch đại |
| Kiểm tra dải nhiễu nền | Trước mỗi buổi đo | Nhiễu bất thường, chạm mát, hỏng cáp |
| Kiểm tra khoá GNSS và PPS | Liên tục | Mất đồng bộ thời gian |
| Kiểm tra ghi dữ liệu | Liên tục | Mất mẫu, lỗi lưu trữ |
| Kiểm tra dung lượng nguồn | Liên tục | Cạn nguồn ngoài dự kiến |
| Kiểm tra đường truyền | Liên tục | Suy giảm liên lạc |

---

## 6. MA TRẬN XÁC MINH

| Yêu cầu | Phương pháp | Giai đoạn |
|---|---|---|
| YC-01 nhiễu nội tại | Đo trong bể tiêu âm + đo tại chỗ biển yên | G2 → G3 |
| YC-02/03 dải tần | Quét đáp ứng tần số với nguồn chuẩn | G2 |
| YC-04 dải động | Đo hai đầu dải, kiểm tra không bão hoà / không chìm dưới nhiễu | G2 |
| YC-05 nhãn thời gian | So với nguồn thời gian chuẩn độc lập | G2 |
| YC-06 chu kỳ cập nhật | Đo trên đường truyền thật | G2 |
| YC-07 phân giải băng hẹp | Phân tích tín hiệu hai tông sát nhau | G2 |
| YC-11/12 thời gian hoạt động | Thử nghiệm phóng điện đủ chu kỳ | G2 |
| YC-13 sai số mức nguồn | **Đo đối chứng với thiết bị đã hiệu chuẩn** | G3 |
| CN-01 không mất mẫu | Ghi dài ngày, kiểm đếm mẫu | G2 |
| CN-05 chuẩn hoá cự ly | Đo cùng nguồn ở nhiều cự ly, kiểm tra SL hội tụ | **G3 — phép thử quan trọng nhất** |
| CN-12 BIT | Gây lỗi có chủ đích, kiểm tra phát hiện được | G2 |
| TC-3 tính nền tảng | Chạy chuỗi xử lý trên cấu hình cảm biến thứ hai | G4 |

> **Phép thử quyết định của toàn chương trình là CN-05 ở G3:** đo cùng một nguồn âm ở nhiều cự ly khác nhau, rồi kiểm tra xem mức nguồn tính ra có **hội tụ về cùng một giá trị** không. Nếu có — chuỗi đo và mô hình truyền âm đúng. Nếu không — sai ở mô hình TL, ở hiệu chuẩn, hoặc ở cự ly. Đây là phép thử tự kiểm chứng, nên thiết kế vào kế hoạch thử nghiệm ngay từ đầu.

---

## 7. QUYẾT ĐỊNH MỞ PHÁT SINH TỪ TÀI LIỆU NÀY

Bổ sung vào danh mục ở Charter mục 11:

| # | Quyết định | Chặn việc gì | Đề xuất |
|---|---|---|---|
| **QĐ-9** | Chuyển nấc độ lợi PGA: tự động hay đặt cố định trước mỗi lần đo? | A2.2, A4, quy trình đo | Cố định cho phép đo chuẩn; tự động cho khảo sát |
| **QĐ-10** | Kiến trúc nhánh dải cao: PA-1 (ADC thứ hai) hay PA-2 (dàn lọc analog)? | A2, A3, A5, chế độ CĐ-4 | Trả lời trước: có ứng dụng nào thực sự cần >20 kHz không? |
| **QĐ-11** | Kênh nghe: giữ tương tự (không mã hoá được) hay chuyển số? | A7.2, CN-06 | Làm rõ yêu cầu bảo mật thực tế trước |
| **QĐ-12** | Mô hình suy hao truyền âm dùng cho C1.4 | Toàn bộ độ chính xác đầu ra | Bắt đầu bằng mô hình đơn giản, hiệu chỉnh bằng đo đối chứng nhiều cự ly |

---

## 8. TIÊU CHUẨN THAM CHIẾU

Danh sách khởi đầu — cần rà soát và bổ sung trong G1.

| Lĩnh vực | Tiêu chuẩn | Vai trò |
|---|---|---|
| Hiệu chuẩn thuỷ âm | IEC 60565 | Phương pháp hiệu chuẩn, cơ sở cho C3 |
| Dàn lọc octave | IEC 61260 | Định nghĩa dải 1/3 octave, cơ sở cho C1.2 |
| Đo tiếng ồn tàu | ISO 17208 (các phần) | **Quan trọng nhất cho Hướng A** — quy trình đo và công bố chữ ký tàu |
| Đo tiếng ồn tàu (Bắc Mỹ) | ANSI/ASA S12.64 | Phương án so sánh với ISO 17208 |
| Thuật ngữ âm học dưới nước | ISO 18405 | Thống nhất đại lượng và ký hiệu |
| Độ không đảm bảo đo | Hướng dẫn GUM | Phương pháp lập ngân sách sai số, cơ sở cho YC-13 |

> **ISO 17208 cần được đọc sớm và đọc kỹ.** Nếu chương trình đi theo Hướng A, tiêu chuẩn này định nghĩa gần như toàn bộ quy trình đo, cách bố trí thuỷ âm, cách chạy tàu, cách xử lý và cách công bố kết quả. Rất nhiều mục `[MỞ]` trong tài liệu này có thể đã có câu trả lời sẵn ở đó. **Giao một kỹ sư đọc và báo cáo lại trong tuần đầu tiên.**

---

## 9. VIỆC CẦN LÀM NGAY

| # | Việc | Ai | Đầu ra | Chặn bởi |
|---|---|---|---|---|
| 1 | Đọc ISO 17208, đối chiếu với các mục `[MỞ]` | 1 kỹ sư | Báo cáo: mục nào tiêu chuẩn đã trả lời | — |
| 2 | Ngân sách nhiễu (WP-1) | Kỹ sư analog | Bảng tính, kết luận YC-01 khả thi hay không | — |
| 3 | Ngân sách sai số (WP-1) | Kỹ sư đo lường | Bảng tính, đề xuất giá trị YC-13 | Việc 1 |
| 4 | Đặc tả định dạng dữ liệu (WP-2) | Kỹ sư phần mềm | Đặc tả + khung đọc file chạy được | — |
| 5 | Khảo sát chuỗi hiệu chuẩn (WP-3) | Kỹ sư đo lường | Phương án truy xuất chuẩn khả thi | — |
| 6 | Trả lời QĐ-10 (dải cao có cần không) | Kỹ sư hệ thống | Phân tích nhu cầu theo UD-1…UD-6 | Việc 1 |
| 7 | Ngân sách điện năng (WP-4) | Kỹ sư hệ thống | Bảng tính → YC-11, YC-12 | Việc 2 |
| 8 | Làm rõ cấp phép tần số | Kỹ sư hệ thống | Thủ tục, thời gian, ràng buộc | — |

**Việc 1, 2, 4, 5, 8 không bị chặn bởi QĐ-0 và bắt đầu được ngay hôm nay.**

---

## 10. LỊCH SỬ SỬA ĐỔI

| Phiên bản | Ngày | Nội dung |
|---|---|---|
| 0.1 | 2026-08-25 | Bản đầu. Cấu trúc phân rã 3 cấp, sơ đồ khối hệ thống và chuỗi tín hiệu, phương trình đo, ngân sách thiết kế sơ bộ, đặc tả 13 phân hệ, ma trận truy vết và ma trận xác minh, 4 quyết định mở phát sinh (QĐ-9…QĐ-12) |

---

**Phạm vi tài liệu:** như Charter mục 14 — chỉ nội dung kỹ thuật. Không chứa điều khoản thương mại, danh tính đối tác, địa điểm triển khai cụ thể hay thông tin khách hàng.
