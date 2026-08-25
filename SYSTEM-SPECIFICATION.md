# HW-SNR-26 — ĐẶC TẢ HỆ THỐNG (VÒNG XOẮN 1 — PoC)

**Phao Giám sát Thuỷ âm Thụ động · Cấu trúc · Sơ đồ khối · Đặc tả phân hệ**

| | |
|---|---|
| **Phiên bản** | 0.2 |
| **Ngày** | 2026-08-25 |
| **Phạm vi** | **Chỉ vòng xoắn 1 (PoC).** Không đặc tả sản phẩm cuối |
| **Tài liệu mẹ** | `PROJECT-CHARTER.md` v0.3 — đọc trước |
| **Đối tượng** | Kỹ sư thiết kế phân hệ |

> **Nguyên tắc bao trùm:** đây là đặc tả cho một PoC nhằm gỡ rủi ro, không phải cho một sản
> phẩm. Ở mọi chỗ có thể chọn giữa "đúng hơn" và "đơn giản hơn mà vẫn trả lời được câu hỏi
> PoC", **chọn đơn giản hơn**. Chỗ nào không được phép đơn giản hoá đều được đánh dấu rõ.

**Quy ước:** `[CHỐT]` · `[GIẢ ĐỊNH]` · `[MỞ]` · `[THAM CHIẾU]` (số của TMK-SAS) · `[TÍNH]` · `[V2+]`

---

## 1. CẤU TRÚC PHÂN RÃ

```
HW-SNR-26 Vòng 1 — Phao giám sát thuỷ âm thụ động
│
├─ SS-A  PHAO
│   ├─ A1  Cụm thu ..................................... 🔴 RỦI RO CAO NHẤT
│   │      ├─ A1.1  Thuỷ âm (mua ngoài)
│   │      ├─ A1.2  Tiền khuếch đại tại chỗ
│   │      ├─ A1.3  Cáp nhiễu thấp
│   │      └─ A1.4  ✱ CƠ CẤU TREO GIẢM CHẤN ✱  (tự thiết kế)
│   ├─ A2  Analog front-end
│   │      ├─ A2.1  Khuếch đại vi sai
│   │      ├─ A2.2  PGA
│   │      └─ A2.3  Lọc chống chồng phổ
│   ├─ A3  Số hoá  (ADC + đồng bộ giờ GNSS)
│   ├─ A4  Xử lý
│   │      ├─ A4.1  Phân tích phổ
│   │      ├─ A4.2  Phát hiện sự kiện
│   │      ├─ A4.3  Ghi dữ liệu thô
│   │      └─ A4.4  Đóng gói bản tin
│   ├─ A5  Lưu trữ
│   ├─ A6  GNSS + đồng bộ thời gian
│   ├─ A7  Vô tuyến  (chỉ kênh dữ liệu — không kênh nghe ở vòng 1)
│   ├─ A8  Kết cấu phao + neo ......................... 🔴 RỦI RO CAO
│   └─ A9  Nguồn điện
│
├─ SS-B  TRẠM ĐIỀU KHIỂN
│   ├─ B1  Máy tính
│   ├─ B2  Vô tuyến
│   └─ B3  Phần mềm  (hiển thị phổ · danh sách sự kiện · phân tích hậu kỳ)
│
└─ SS-C  LÕI XỬ LÝ VÀ HIỆU CHUẨN
    ├─ C1  Thư viện phân tích  (phổ · phát hiện · bù hiệu chuẩn)
    ├─ C2  Định dạng dữ liệu + metadata
    └─ C3  Hiệu chuẩn mức PoC
```

**A1.4 và A8 là cặp phân hệ quyết định vòng xoắn này.** Xem `PROJECT-CHARTER.md` §4.
Chúng phải do cùng một nhóm thiết kế.

---

## 2. SƠ ĐỒ KHỐI

### 2.1 Chuỗi tín hiệu

```
   Áp suất âm p(t) [µPa]        ◄── tín hiệu quan tâm + nhiễu môi trường
        │
        ▼
┌──────────────────────┐
│ A1.4 TREO GIẢM CHẤN  │  ✱ Chặn nhiễu cơ học từ phao truyền xuống
│      + neo/cáp       │  ✱ Không có khối này thì mọi khối sau vô nghĩa
└──────────┬───────────┘
           ▼
┌──────────────────────┐
│ A1.1 Thuỷ âm  PZT    │  Độ nhạy M [dB re 1V/µPa]
└──────────┬───────────┘  Nguồn tín hiệu kiểu tụ, trở kháng rất cao
           ▼
┌──────────────────────┐
│ A1.2 Tiền KĐ tại chỗ │  ◄── ĐẶT SÁT PHẦN TỬ. Nhiễu áp VÀ nhiễu dòng
└──────────┬───────────┘      đều quan trọng — xem §3.1
           ▼
┌──────────────────────┐
│ A1.3 Cáp nhiễu thấp  │  ◄── Cáp thường sinh nhiễu ma sát điện khi rung
└──────────┬───────────┘
           ▼
┌──────────────────────┐
│ A2.1 KĐ vi sai       │
└──────────┬───────────┘
           ▼
┌──────────────────────┐
│ A2.2 PGA             │  ◄── Giá trị độ lợi G PHẢI ghi vào metadata
└──────────┬───────────┘      kèm nhãn thời gian mỗi lần đổi nấc
           ▼
┌──────────────────────┐
│ A2.3 Lọc chống       │
│      chồng phổ       │
└──────────┬───────────┘
           ▼
┌──────────────────────┐
│ A3 ADC 16 bit        │  + nhãn thời gian từ PPS của GNSS
└──────────┬───────────┘
           │
     ┌─────┴─────┬──────────────────┐
     ▼           ▼                  ▼
┌─────────┐ ┌──────────┐    ┌──────────────┐
│ A4.3    │ │ A4.1     │    │ A4.2         │
│ Ghi thô │ │ Phân     │    │ Phát hiện    │
│ (ưu     │ │ tích phổ │    │ sự kiện      │
│  tiên   │ └────┬─────┘    └──────┬───────┘
│  cao    │      │                 │
│  nhất)  │      └────────┬────────┘
└────┬────┘               ▼
     │            ┌───────────────┐
     │            │ A4.4 Đóng gói │
     │            └───────┬───────┘
     │                    ▼
     │            ┌───────────────┐
     │            │ A7 Vô tuyến   │───► trạm, thời gian thực
     │            └───────────────┘
     │
     └──► A5 Lưu trữ ──► (sau thu hồi, qua Ethernet) ──► trạm

                            ▼
              ┌──────────────────────────┐
              │ C1.1 Bù hiệu chuẩn       │  − M − G + K
              └────────────┬─────────────┘
                           ▼
              ╔══════════════════════════════════╗
              ║  RL — Mức thu được [dB re 1µPa]  ║
              ║  + Danh sách sự kiện phát hiện   ║  ← SẢN PHẨM ĐẦU RA
              ╚══════════════════════════════════╝
```

### 2.2 Tốc độ dữ liệu

```
   ADC 16 bit, 1 kênh, fs = 42 kS/s  [THAM CHIẾU]        [TÍNH]
        │
        └──► 672 kbit/s = 84 kB/s = 302 MB/giờ = 2.4 GB / 8 giờ
                 │
       ┌─────────┴──────────┐
       ▼                    ▼
   ┌─────────┐      ┌───────────────────────┐
   │ Ghi thô │      │ Rút gọn: phổ + sự kiện│
   │ 84 kB/s │      │ ~vài trăm bit/s       │
   └─────────┘      └──────────┬────────────┘
                               ▼
                    ┌──────────────────────┐
                    │ Modem UHF            │
                    │ Nhu cầu << năng lực  │
                    └──────────────────────┘
```

**Ba kết luận `[TÍNH]`:**

1. **Vô tuyến dư băng thông rất lớn** cho dữ liệu đã xử lý.
2. **Vô tuyến hoàn toàn không đủ** cho dữ liệu thô → kiến trúc "lấy dữ liệu sau thu hồi" là
   bắt buộc, giống SONRAS. Đây là chỗ mượn kiến trúc hợp lý.
3. **Dung lượng lưu trữ không phải ràng buộc** — 2.4 GB / 8 giờ là nhỏ. Ràng buộc thật là
   **điện năng** và **độ tin cậy ghi**, không phải dung lượng.

> `[V2+]` Nếu sau này lên mảng nhiều phần tử, nhân các con số trên với số kênh. Lúc đó lưu trữ
> và điện năng **đều** trở thành ràng buộc thật.

---

## 3. NGÂN SÁCH THIẾT KẾ

### 3.1 Ngân sách nhiễu — khung

Đây là công cụ chính để trả lời PoC-1 và PoC-2. Phải lập **trước** khi chọn linh kiện.

```
NGUỒN NHIỄU                              NHÓM        GHI CHÚ
──────────────────────────────────────────────────────────────────────────────
0. Nhiễu môi trường biển                 MỐC         Tất cả so với mốc này.
   (gió, sóng, giao thông tàu,                       PHẢI ĐO THỰC ĐỊA, không
    sinh vật)                                        lấy từ sách — xem §7 việc 4
──────────────────────────────────────────────────────────────────────────────
NHÓM I — NHIỄU NỀN TẢNG   ⭐ rủi ro chi phối của vòng xoắn 1
1. Nhiễu dòng chảy qua thuỷ âm           I           Tăng mạnh theo tốc độ dòng
2. Rung dây neo (strum)                  I           Phụ thuộc dòng + thiết kế neo
3. Chuyển động phao truyền qua cáp       I           A1.4 sinh ra để chặn cái này
4. Nhiễu ma sát điện của cáp             I           Chỉ xuất hiện khi cáp rung/uốn
──────────────────────────────────────────────────────────────────────────────
NHÓM II — NHIỄU ĐIỆN TỬ   (bài toán đã biết cách giải)
5. Nhiễu nhiệt của phần tử thu           II
6. Nhiễu áp tiền khuếch đại              II          ~3.3 nV/√Hz [THAM CHIẾU]
7. Nhiễu dòng tiền khuếch đại            II          ⚠ TRỘI Ở TẦN SỐ THẤP
8. Nhiễu lượng tử hoá ADC                II          Xem §3.2
9. Nhiễu nguồn / nhiễu số nội bộ         II          Do bố trí mạch quyết định
──────────────────────────────────────────────────────────────────────────────

YÊU CẦU:
   Nhóm II tổng            ≤  (0) − 10 dB        ← YC-01, chứng minh ở G2 (bàn)
   Nhóm I + Nhóm II tổng   ≤  (0) + ngưỡng YC-02 ← PoC-2, chứng minh ở G3 (biển)
```

> **Hai cảnh báo cho kỹ sư analog:**
>
> **(a)** Ở đầu dải tần thấp, với nguồn tín hiệu trở kháng rất cao như thuỷ âm PZT,
> **nhiễu dòng của tiền khuếch đại thường trội hơn nhiễu áp.** Chọn linh kiện chỉ theo chỉ
> tiêu nV/√Hz là sai lầm phổ biến. Phải xét cả hai, tại tần số thấp nhất của dải công tác.
>
> **(b)** Bộ chuyển đổi nguồn kiểu xung là nguồn nhiễu đáng kể. Tách nguồn analog khỏi nguồn
> số, cân nhắc ổn áp tuyến tính cho tầng đầu. Đây là một trong các nguyên nhân phổ biến nhất
> khiến không đạt YC-01.

### 3.2 Dải động và số bit

```
ADC 16 bit lý tưởng:  SNR = 6.02 × 16 + 1.76 = 98.1 dB      [TÍNH]
Thực tế (ENOB ~14.5): ≈ 89 dB                                [TÍNH]

Dải động cần phủ:  từ nhiễu nền biển yên tĩnh
                   đến nguồn ồn ở cự ly gần
                   khoảng cách = TBD  ◄── PHẢI CÓ SỐ TRONG NGÂN SÁCH NHIỄU

Nếu dải_động_cần > 89 dB  ⇒  bắt buộc PGA nhiều nấc
```

**Hệ quả vận hành:** khi PGA đổi nấc sẽ có gián đoạn. Với hệ **giám sát** (khác hệ đo),
mục tiêu xuất hiện bất ngờ nên **đổi nấc tự động là cần thiết** — không đặt cố định trước
được như hệ đo. Kéo theo hai yêu cầu bắt buộc:

- ghi lại mọi lần đổi nấc kèm nhãn thời gian (CN-09);
- phần mềm phân tích phải loại bỏ vùng chuyển tiếp.

### 3.3 Nhiễu nền tảng — cách định lượng ở vòng 1

Không cần mô hình lý thuyết phức tạp. Vòng 1 dùng **phép đo so sánh**:

```
   Đo A — TĨNH:   thuỷ âm thả từ tàu neo, biển lặng, cáp chùng, không phao
                  → cho nền nhiễu "tốt nhất có thể" của chuỗi thu

   Đo B — THẬT:   thuỷ âm treo trên phao, đúng cấu hình triển khai
                  → cho nền nhiễu thực tế

   ĐÓNG GÓP CỦA NỀN TẢNG  =  Đo B  −  Đo A        (theo từng dải tần)
```

> **Đây là phép đo trung tâm của PoC-2, và là kết quả quan trọng nhất của cả vòng xoắn 1.**
> Thiết kế kế hoạch thử nghiệm quanh phép đo này. Phải đo được **cả hai** trong cùng một
> chuyến biển, cùng điều kiện môi trường — nếu không thì hiệu số không có ý nghĩa.

Vòng 1 phải chạy phép đo này với **ít nhất hai cấu hình treo giảm chấn khác nhau** để có
cơ sở so sánh, không chỉ một.

### 3.4 Đồng bộ thời gian

```
   Δd = c × Δt,  c ≈ 1500 m/s        Δt = 1 ms ⇒ Δd = 1.5 m
```

Vòng 1 **không định vị nguồn**, nên yêu cầu đồng bộ lỏng. GNSS PPS (sai số cỡ chục ns) dư
sức. Nhãn thời gian ở vòng 1 chỉ cần đủ để:
- đối chiếu sự kiện phát hiện được với quan sát bên ngoài (mắt thường, AIS);
- ghép dữ liệu nhiều lần triển khai.

> `[V2+]` Yêu cầu đồng bộ chỉ trở nên chặt khi làm định vị đa phao hoặc mảng định hướng.

### 3.5 Ngân sách điện năng

| Khối | Ước tính | Ghi chú |
|---|---|---|
| A1.2 Tiền khuếch đại | TBD | Chạy liên tục |
| A2 Analog front-end | TBD | Chạy liên tục |
| A3 ADC | TBD | Chạy liên tục |
| A4 Xử lý | TBD | **Thường là khối lớn nhất** |
| A5 Lưu trữ | TBD | Ghi liên tục |
| A6 GNSS | TBD | Có thể giảm chu kỳ để tiết kiệm |
| A7 Vô tuyến | TBD | Chỉ phát theo chu kỳ, không liên tục |

**Vòng 1 không tối ưu điện năng.** Chỉ cần đủ cho một đợt triển khai (QĐ-5). Nếu cần, chấp
nhận ắc quy to và nặng — vòng 1 không có ràng buộc kích thước. Tối ưu điện năng là bài toán
của `[V2+]` khi làm trực canh dài ngày.

---

## 4. ĐẶC TẢ PHÂN HỆ

### 4.1 A1 — CỤM THU  🔴

#### A1.4 — Cơ cấu treo giảm chấn ✱

**Đây là hạng mục thiết kế quan trọng nhất của vòng xoắn 1.**

| ID | Tham số | Yêu cầu | Trạng thái |
|---|---|---|---|
| ĐT-A1.4-01 | Tách cơ học thuỷ âm khỏi thân phao | Bắt buộc | `[CHỐT]` |
| ĐT-A1.4-02 | Số phương án phải thử ở vòng 1 | ≥ 2 | `[CHỐT]` |
| ĐT-A1.4-03 | Thay đổi được cấu hình ngoài hiện trường | Nên có | `[GIẢ ĐỊNH]` |
| ĐT-A1.4-04 | Dải tần cần cách ly | Theo QĐ-1, ưu tiên dải thấp | `[MỞ]` |
| ĐT-A1.4-05 | Chịu được lực khi thả và thu hồi | Bắt buộc | `[CHỐT]` |

**Hướng kỹ thuật cần khảo sát** (chưa chọn — thuộc QĐ-3):
- phần tử đàn hồi giữa phao và cáp thuỷ âm, giảm truyền rung;
- khối lượng giảm chấn / tạ ổn định phía dưới thuỷ âm;
- đoạn cáp chùng có chủ ý để cắt đường truyền rung;
- chắn dòng chảy quanh thuỷ âm — cân nhắc kỹ, vì có thể tự sinh nhiễu xoáy.

> **Lưu ý:** ĐT-A1.4-03 (đổi cấu hình ngoài hiện trường) không phải tiện nghi. Một chuyến
> biển tốn kém; nếu phải về xưởng mới đổi được cấu hình treo thì vòng 1 chỉ thử được một
> phương án mỗi chuyến, và ĐT-A1.4-02 không đạt.

#### A1.1–A1.3 — Thuỷ âm, tiền khuếch đại, cáp

| ID | Tham số | Yêu cầu | Trạng thái |
|---|---|---|---|
| ĐT-A1-01 | Thuỷ âm | Mua ngoài, không tự chế tạo | `[CHỐT]` |
| ĐT-A1-02 | Số phần tử | 1 (vòng 1) | `[CHỐT]` |
| ĐT-A1-03 | Tính định hướng | Vô hướng | `[CHỐT]` |
| ĐT-A1-04 | Độ nhạy M | TBD — điển hình −180…−200 dB re 1V/µPa | `[MỞ]` |
| ĐT-A1-05 | Dải tần | Theo QĐ-1 | `[MỞ]` |
| ĐT-A1-06 | Tiền khuếch đại | Đặt sát phần tử; xét cả nhiễu áp và nhiễu dòng | `[CHỐT]` |
| ĐT-A1-07 | Cáp | Loại nhiễu thấp chuyên dụng cho thuỷ âm | `[CHỐT]` |
| ĐT-A1-08 | Hiệu chuẩn | Đủ để công bố mức tin cậy được (mức PoC) | `[CHỐT]` |

> `[V2+]` Mảng nhiều phần tử, định hướng, mảng quang loại bỏ điện tử đầu ướt.

### 4.2 A2 — ANALOG FRONT-END

| ID | Tham số | Yêu cầu | Trạng thái |
|---|---|---|---|
| ĐT-A2-01 | Mật độ nhiễu áp quy về đầu vào | ≤ 3.3 nV/√Hz | `[THAM CHIẾU]` |
| ĐT-A2-02 | Mật độ nhiễu dòng quy về đầu vào | TBD — **quan trọng ở tần số thấp** | `[MỞ]` |
| ĐT-A2-03 | Trở kháng vào | Cao, phù hợp nguồn kiểu tụ | `[CHỐT]` |
| ĐT-A2-04 | Dải điều chỉnh độ lợi | ~40 dB | `[THAM CHIẾU]` |
| ĐT-A2-05 | Chuyển nấc độ lợi | **Tự động** — xem §3.2 | `[CHỐT]` |
| ĐT-A2-06 | Sai số độ lợi từng nấc | TBD — cộng thẳng vào YC-07 | `[MỞ]` |
| ĐT-A2-07 | Ghi log mỗi lần đổi nấc | Bắt buộc | `[CHỐT]` CN-09 |
| ĐT-A2-08 | Lọc chống chồng phổ | Bậc theo fs | `[MỞ]` |

### 4.3 A3 — SỐ HOÁ

| ID | Tham số | Yêu cầu | Trạng thái |
|---|---|---|---|
| ĐT-A3-01 | Độ phân giải | 16 bit | `[GIẢ ĐỊNH]` |
| ĐT-A3-02 | ENOB | TBD — **chỉ tiêu thật, không phải số bit danh nghĩa** | `[MỞ]` |
| ĐT-A3-03 | Tần số lấy mẫu | Theo QĐ-1 | `[MỞ]` |
| ĐT-A3-04 | Nhãn thời gian | Từ PPS của GNSS | `[CHỐT]` |

> **Đơn giản hoá so với phiên bản trước:** bỏ nhánh xử lý dải cao (20–100 kHz) khỏi vòng 1.
> Vòng 1 dùng **một đường lấy mẫu duy nhất**. Việc có cần dải cao hay không phụ thuộc mục
> tiêu quan tâm, và câu hỏi đó thuộc QĐ-1. Loại bỏ nhánh này cũng đóng luôn QĐ-10 của
> phiên bản trước.

### 4.4 A4 — XỬ LÝ

| ID | Tham số | Yêu cầu | Trạng thái |
|---|---|---|---|
| ĐT-A4-01 | Ghi thô không mất mẫu | **Ưu tiên cao nhất trong A4** | `[CHỐT]` CN-01 |
| ĐT-A4-02 | Phân tích phổ | Dải 1/3 octave và/hoặc phổ băng hẹp | `[MỞ]` |
| ĐT-A4-03 | Phát hiện sự kiện | Thuật toán vòng 1 theo QĐ-7 | `[MỞ]` |
| ĐT-A4-04 | Chu kỳ báo cáo về trạm | TBD | `[MỞ]` |
| ĐT-A4-05 | Nền tảng tính toán | TBD | `[MỞ]` |

**Về thuật toán phát hiện (QĐ-7):** vòng 1 nên bắt đầu bằng phương pháp đơn giản nhất chạy
được — phát hiện theo ngưỡng năng lượng trong dải tần quan tâm, có ước lượng nền thích nghi.
Mục tiêu của vòng 1 là chứng minh **nghe được**, không phải chứng minh **phân loại giỏi**.

> `[V2+]` DEMON để trích tần số quay chân vịt, phân loại tự động, học máy — tất cả cần
> dữ liệu thật, mà dữ liệu thật chính là sản phẩm của vòng 1.

**Nguyên tắc ưu tiên tài nguyên:** nếu thiếu tài nguyên tính toán, chấp nhận trễ hoặc bỏ chu
kỳ báo cáo — **không bao giờ** chấp nhận mất mẫu dữ liệu thô. Dữ liệu thô mất là mất vĩnh viễn.

### 4.5 A5 — LƯU TRỮ

| ID | Tham số | Yêu cầu | Trạng thái |
|---|---|---|---|
| ĐT-A5-01 | Tốc độ ghi | ~84 kB/s (1 kênh) | `[TÍNH]` |
| ĐT-A5-02 | Dung lượng | Theo QĐ-5 + hệ số dự phòng | `[MỞ]` |
| ĐT-A5-03 | Cấp linh kiện | Công nghiệp, chịu rung và nhiệt | `[CHỐT]` |
| ĐT-A5-04 | Toàn vẹn dữ liệu | Có kiểm tra, phát hiện được hỏng | `[CHỐT]` |
| ĐT-A5-05 | Trích xuất sau thu hồi | Ethernet | `[CHỐT]` CN-06 |

> `[V2+]` Mã hoá dữ liệu lưu trữ. Vòng 1 bỏ vì không gỡ rủi ro nào của PoC.

### 4.6 A6 — GNSS

| ID | Tham số | Yêu cầu | Trạng thái |
|---|---|---|---|
| ĐT-A6-01 | Ngõ ra PPS | Bắt buộc | `[CHỐT]` |
| ĐT-A6-02 | Sai số vị trí | GNSS thường là đủ cho vòng 1 | `[GIẢ ĐỊNH]` |
| ĐT-A6-03 | Lưu vệt vị trí trong phao | Bắt buộc | `[CHỐT]` CN-04 |

**Vì sao GNSS thường là đủ ở vòng 1:** ta không chuẩn hoá theo cự ly (không có bước đó nữa),
nên sai số vị trí không truyền vào kết quả đo như ở hệ đo chữ ký. Vị trí ở đây dùng để biết
phao ở đâu và phao có trôi không, chứ không dùng để tính mức nguồn.

> `[V2+]` RTK chỉ cần khi làm định vị nguồn đa phao.

### 4.7 A7 — VÔ TUYẾN

| ID | Tham số | Yêu cầu | Trạng thái |
|---|---|---|---|
| ĐT-A7-01 | Kênh dữ liệu | Một kênh, hai chiều | `[CHỐT]` |
| ĐT-A7-02 | Băng thông cần | Vài trăm bit/s | `[TÍNH]` |
| ĐT-A7-03 | Cự ly làm việc | TBD | `[MỞ]` |
| ĐT-A7-04 | Cấp phép tần số | **Bắt buộc làm rõ sớm** | `[MỞ]` RR-06 |

> **Bỏ khỏi vòng 1:** kênh nghe tương tự VHF. Nó không gỡ rủi ro PoC nào, tốn điện, và từng
> gây xung đột với yêu cầu mã hoá. `[V2+]`

### 4.8 A8 — KẾT CẤU PHAO VÀ NEO  🔴

| ID | Tham số | Yêu cầu | Trạng thái |
|---|---|---|---|
| ĐT-A8-01 | Neo cố định hay thả trôi | Theo QĐ-4 | `[MỞ]` 🔴 |
| ĐT-A8-02 | Độ nổi và ổn định | TBD | `[MỞ]` |
| ĐT-A8-03 | Giảm truyền rung xuống cụm thu | **Bắt buộc — cùng bài toán với A1.4** | `[CHỐT]` |
| ĐT-A8-04 | Thả và thu hồi bởi đội nhỏ trên tàu | Bắt buộc | `[CHỐT]` |
| ĐT-A8-05 | Phương tiện định vị khi thu hồi | Bắt buộc | `[CHỐT]` CN-07 |
| ĐT-A8-06 | Chống ăn mòn nước biển | Bắt buộc | `[CHỐT]` |
| ĐT-A8-07 | Trạng thái biển thiết kế | Vòng 1 chọn cửa sổ thời tiết | `[GIẢ ĐỊNH]` |

**Đánh đổi trung tâm của A8** — ba yêu cầu kéo ngược nhau, phải thiết kế cùng lúc:

```
   Cột ăng-ten cao  ──►  liên lạc xa hơn
          │
          └──────────►  phao lắc nhiều hơn  ──►  nhiễu nền tảng tăng (RR-01)

   Neo chặt  ──►  giữ đúng vị trí   ──►  nhưng dây neo rung (strum) → nhiễu
   Thả trôi  ──►  ít nhiễu dây neo  ──►  nhưng trôi khỏi khu vực, khó thu hồi
```

> QĐ-4 (neo hay trôi) vì thế **không phải chi tiết triển khai** mà là quyết định kiến trúc
> ảnh hưởng trực tiếp tới rủi ro chi phối của cả vòng xoắn.

### 4.9 A9 — NGUỒN

| ID | Tham số | Yêu cầu | Trạng thái |
|---|---|---|---|
| ĐT-A9-01 | Thời lượng | Theo QĐ-5 | `[MỞ]` |
| ĐT-A9-02 | Nguồn dự phòng định vị thu hồi | Bắt buộc | `[CHỐT]` CN-07 |
| ĐT-A9-03 | **Cách ly nhiễu nguồn khỏi chuỗi analog** | **Bắt buộc** | `[CHỐT]` |
| ĐT-A9-04 | Báo dung lượng còn lại về trạm | Bắt buộc | `[CHỐT]` |
| ĐT-A9-05 | Kích thước, khối lượng | **Không ràng buộc ở vòng 1** | `[CHỐT]` |

### 4.10 SS-B — TRẠM ĐIỀU KHIỂN

Vòng 1 giữ tối giản. Ba chức năng:

| Chức năng | Nội dung | Trạng thái |
|---|---|---|
| Giám sát thời gian thực | Hiển thị phổ, danh sách sự kiện, trạng thái sức khoẻ phao | `[CHỐT]` |
| Phân tích hậu kỳ | Đọc dữ liệu thô, phân tích phổ, rà sự kiện | `[CHỐT]` |
| Trích xuất dữ liệu | Lấy dữ liệu từ phao sau thu hồi | `[CHỐT]` |

**Nền tảng:** laptop thường là đủ cho vòng 1. Không cần máy tính chuyên dụng, không cần
giao diện đẹp. `[V2+]` cho giao diện dùng được ngoài nắng, chống nước, đào tạo người dùng.

### 4.11 SS-C — LÕI XỬ LÝ VÀ HIỆU CHUẨN

#### C1 — Thư viện phân tích

| ID | Yêu cầu | Trạng thái |
|---|---|---|
| ĐT-C1-01 | Bù hiệu chuẩn: RL = V_ADC − M − G + K | `[CHỐT]` |
| ĐT-C1-02 | Phân tích phổ | `[MỞ]` theo QĐ-1 |
| ĐT-C1-03 | Phát hiện sự kiện | `[MỞ]` theo QĐ-7 |
| ĐT-C1-04 | **Chạy được ngoại tuyến trên file đã ghi** | `[CHỐT]` |
| ĐT-C1-05 | Xử lý đúng vùng chuyển nấc độ lợi | `[CHỐT]` |

> **ĐT-C1-04 là nguyên tắc không được vi phạm.** Thuật toán chỉ chạy được khi có phần cứng
> thật đang cắm thì không kiểm thử được, không so sánh được giữa các cấu hình treo, và không
> tái sử dụng được ở vòng sau. Toàn bộ giá trị phân tích của vòng 1 nằm ở khả năng chạy đi
> chạy lại trên dữ liệu đã thu.

#### C2 — Định dạng dữ liệu và metadata

Metadata bắt buộc trong mỗi file dữ liệu thô:

| Trường | Bắt buộc |
|---|---|
| Định danh phao, định danh thuỷ âm | ✔ |
| Hồ sơ hiệu chuẩn: M, K, ngày hiệu chuẩn | ✔ |
| Cấu hình thu: fs, độ phân giải, dải tần | ✔ |
| **Vệt độ lợi G theo thời gian, mọi lần đổi nấc** | ✔ |
| Vệt vị trí GNSS | ✔ |
| Gốc thời gian GNSS | ✔ |
| **Cấu hình treo giảm chấn đang dùng** | ✔ ⭐ |
| Điều kiện môi trường: trạng thái biển, dòng chảy nếu có | Nên có |

> **Trường "cấu hình treo giảm chấn" là đặc thù của vòng xoắn này.** Vòng 1 thử nhiều phương
> án treo; nếu file dữ liệu không ghi lại đang dùng phương án nào thì không so sánh được, và
> phép đo trung tâm ở §3.3 mất ý nghĩa.

**Nguyên tắc:** file dữ liệu phải **tự mô tả đầy đủ** — người phân tích không có mặt lúc đo
vẫn phải quy được ra mức thu được.

#### C3 — Hiệu chuẩn mức PoC

| ID | Yêu cầu | Trạng thái |
|---|---|---|
| ĐT-C3-01 | Có hồ sơ hiệu chuẩn thuỷ âm hợp lệ | `[CHỐT]` |
| ĐT-C3-02 | Hiệu chuẩn độ lợi từng nấc PGA | `[GIẢ ĐỊNH]` |
| ĐT-C3-03 | Kiểm tra hiệu chuẩn trước và sau mỗi chuyến biển | `[CHỐT]` |

> **Hạ yêu cầu có chủ ý so với phiên bản trước:** vòng 1 **không** đòi chuỗi truy xuất đầy đủ
> về chuẩn quốc gia. Chỉ cần hiệu chuẩn đủ để mức công bố đáng tin và so sánh được giữa các
> lần đo. Truy xuất chuẩn đầy đủ là `[V2+]` — nó cần thiết khi bán dịch vụ đo, không cần
> thiết để chứng minh nghe được.
>
> ĐT-C3-03 (kiểm tra trước và sau chuyến) thì **không** được bỏ: nếu hiệu chuẩn trôi giữa
> chuyến mà không biết, toàn bộ dữ liệu chuyến đó mất giá trị so sánh.

---

## 5. MA TRẬN XÁC MINH

| Mệnh đề / Yêu cầu | Phương pháp | Cổng |
|---|---|---|
| YC-01 nhiễu điện tử | Đo trong bể tiêu âm hoặc thùng nước yên tĩnh | G2 |
| Dải động | Đo hai đầu dải: không bão hoà, không chìm dưới nhiễu | G2 |
| CN-01 không mất mẫu | Ghi dài, kiểm đếm số mẫu | G2 |
| C1 chạy ngoại tuyến | Chạy chuỗi phân tích trên dữ liệu mô phỏng và dữ liệu thật | G2 |
| **PoC-1** | Đo nền nhiễu tại chỗ, so với nhiễu môi trường | G3 |
| **PoC-2** | **Phép đo so sánh tĩnh–động ở §3.3, ≥2 cấu hình treo** | **G3** ⭐ |
| **PoC-3** | Phát hiện tàu đi qua, đối chiếu quan sát mắt hoặc AIS | G3 |
| **PoC-4** | Trích xuất trọn vẹn sau thu hồi, chạy được phân tích | G3 |
| CN-08 BIT | Gây lỗi có chủ đích, kiểm tra phát hiện được | G2 |

> **Phép thử quyết định của vòng xoắn 1 là PoC-2 tại G3.** Toàn bộ kế hoạch thử nghiệm biển
> nên được thiết kế quanh nó: đo tĩnh và đo động trong cùng chuyến, cùng điều kiện, với ít
> nhất hai cấu hình treo, và ghi đầy đủ cấu hình vào metadata.

---

## 6. QUYẾT ĐỊNH MỞ

| # | Quyết định | Chặn | Mức |
|---|---|---|---|
| **QĐ-2** | Ngưỡng YC-02 (nhiễu khi hoạt động thật) và sai số công bố YC-07 | **Tiêu chí đạt/không đạt của cả PoC** | 🔴 |
| **QĐ-1** | Dải tần công tác | Thuỷ âm, ADC, fs, thiết kế treo | 🔴 |
| **QĐ-3** | Phương án treo giảm chấn (≥2 để thử) | A1.4, A8 | 🔴 |
| **QĐ-4** | Neo cố định hay thả trôi | A8, RR-01, kế hoạch thử | 🔴 |
| QĐ-5 | Thời lượng một đợt triển khai | A9, A5 | 🟡 |
| QĐ-6 | Mức hiệu chuẩn cần cho PoC | C3, chi phí | 🟡 |
| QĐ-7 | Thuật toán phát hiện sự kiện vòng 1 | A4.2 | 🟡 |
| QĐ-8 | Khu vực và cửa sổ thời gian thử nghiệm | Kế hoạch, RR-04, RR-08 | 🟡 |

**Đã đóng ở phiên bản này:** QĐ-0 (nhiệm vụ — phao giám sát thụ động) · QĐ-9 (chuyển nấc PGA
— tự động, do đặc thù giám sát) · QĐ-10 (nhánh dải cao — bỏ khỏi vòng 1) · QĐ-11 (kênh nghe
vs mã hoá — bỏ cả hai khỏi vòng 1) · QĐ-12 (mô hình truyền âm — không còn áp dụng vì bỏ
chuẩn hoá cự ly).

---

## 7. VIỆC BẮT ĐẦU NGAY

| # | Việc | Ai | Đầu ra |
|---|---|---|---|
| 1 | Đặt số cho **YC-02** | Chief Engineer + đo lường | Ngưỡng + lập luận |
| 2 | **Ngân sách nhiễu** nhóm II | Kỹ sư analog | Bảng tính, kết luận YC-01 |
| 3 | **Thiết kế ≥2 phương án treo giảm chấn** | Kỹ sư cơ khí | Bản vẽ + luận cứ |
| 4 | **Đo nhiễu nền vùng thử nghiệm** | Kỹ sư field | Phổ nhiễu nền thực tế |
| 5 | Đặc tả định dạng dữ liệu + khung phân tích ngoại tuyến | Kỹ sư phần mềm | Đặc tả + code chạy được |
| 6 | Chốt QĐ-1 (dải tần) | Chief Engineer | Dải tần + lý do |
| 7 | Chốt QĐ-4 (neo hay trôi) | Chief Engineer + cơ khí | Quyết định + lý do |
| 8 | Cấp phép tần số | Kỹ sư hệ thống | Thủ tục, thời gian |
| 9 | Kế hoạch thử nghiệm quanh phép đo §3.3, bảo đảm có mục tiêu (RR-08) | Kỹ sư field | Kế hoạch chuyến biển |

**Việc 1, 2, 3 là đường găng.**

**Việc 4 nên làm sớm nhất** — mọi ngưỡng trong YC-01 và YC-02 đều tham chiếu về nhiễu môi
trường thật. Làm bằng thiết bị đi mượn cũng được; không cần chờ phần cứng của mình.

---

## 8. LỊCH SỬ SỬA ĐỔI

| Phiên bản | Ngày | Nội dung |
|---|---|---|
| 0.1 | 2026-08-25 | Bản đầu, theo hướng hệ đo chữ ký âm học |
| **0.2** | **2026-08-25** | **Tái phạm vi về vòng xoắn 1 PoC — phao giám sát thụ động.** Bỏ chuẩn hoá cự ly và mức nguồn; đầu ra nay là mức thu được RL + danh sách sự kiện. Nâng **nhiễu nền tảng** thành nhóm riêng trong ngân sách nhiễu và thêm phép đo so sánh tĩnh–động (§3.3) làm phép thử trung tâm. Nâng A1.4 (treo giảm chấn) thành hạng mục thiết kế bậc nhất. Bỏ khỏi vòng 1: nhánh dải cao, kênh nghe tương tự, mã hoá, truy xuất chuẩn đầy đủ, định vị đa phao. Đóng QĐ-9, QĐ-10, QĐ-11, QĐ-12. Thêm trường metadata "cấu hình treo giảm chấn" |
