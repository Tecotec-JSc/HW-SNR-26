# HW-SNR-26 — ĐIỀU LỆ DỰ ÁN (PROJECT CHARTER)

**Hệ thống Thu nhận, Ghi và Phân tích Tín hiệu Thuỷ âm**
*Underwater Acoustic Registration and Analysis System*

| | |
|---|---|
| **Mã chương trình** | HW-SNR-26 |
| **Phiên bản tài liệu** | 0.1 — Bản thảo đầu tiên |
| **Ngày** | 2026-08-25 |
| **Trạng thái** | Đang soạn thảo — chờ Chief Engineer phê duyệt |
| **Người đọc đầu tiên** | Chief Engineer |
| **Đối tượng** | Kỹ sư tham gia dự án từ ngày đầu tiên |
| **Phân loại** | Nội bộ — kỹ thuật |

---

## 0. CÁCH ĐỌC TÀI LIỆU NÀY

Đây là tài liệu **khởi động dự án**, không phải đặc tả kỹ thuật (specification) và cũng không phải thiết kế chi tiết. Mục tiêu của nó là trả lời bốn câu hỏi mà một kỹ sư mới vào dự án cần biết trong ngày đầu tiên:

1. Chúng ta đang xây cái gì, và nó dùng để làm gì? → Mục 2, 3
2. Hệ thống được chia thành những khối nào, ai làm khối nào? → Mục 4, 5
3. Cái gì đã chốt, cái gì còn đang mở? → Mục 6, 11
4. Tôi bắt đầu từ đâu? → Mục 8, 9

**Quy ước trong tài liệu:**

| Ký hiệu | Ý nghĩa |
|---|---|
| `[CHỐT]` | Đã quyết định, không thay đổi nếu không qua change request |
| `[GIẢ ĐỊNH]` | Đang dùng làm cơ sở thiết kế nhưng **chưa được xác minh** — phải kiểm chứng trước khi khoá thiết kế |
| `[MỞ]` | Chưa có quyết định. Không được tự ý chọn thay — ghi vào mục 11 và báo Chief Engineer |
| `TBD` | Chưa có số liệu |

> **Cảnh báo dành cho kỹ sư:** phần lớn các con số trong tài liệu này hiện đang ở trạng thái `[GIẢ ĐỊNH]`. Đây là tình trạng bình thường của một charter ở phiên bản 0.1. Điều **không** bình thường là để nguyên trạng thái đó khi bước vào giai đoạn thiết kế chi tiết. Xem mục 6.4.

---

## 1. BỐI CẢNH

Việt Nam hiện chưa tự chủ được năng lực giám sát thuỷ âm thụ động. Cụ thể, bốn khoảng trống đã được xác định:

1. Không có năng lực sản xuất phao thuỷ âm (sonobuoy) trong nước.
2. Không có sonar mảng kéo thụ động (towed array) cho tàu mặt nước.
3. Không có mạng lưới đo và lưu trữ profile tốc độ âm (SVP) cấp quốc gia.
4. Năng lực phần mềm xử lý tín hiệu thuỷ âm còn ở giai đoạn sơ khai.

Trong bốn khoảng trống này, **khoảng trống số 4 là điểm tựa chiến lược của chương trình HW-SNR-26**. Lý do mang tính kỹ thuật chứ không phải khẩu hiệu:

- Rào cản lớn nhất của việc nội địa hoá sonar nằm ở **gốm áp điện (PZT)** và **bo mạch xử lý FPGA** — đây là các hạng mục bị kiểm soát xuất khẩu và cần chuỗi cung ứng chuyên biệt.
- Ngược lại, **cửa sổ âm học** (polyurethane) và **vỏ chịu áp** (Ti/Al gia công CNC) là các hạng mục tiếp cận được ngay trong nước.
- Năng lực tính toán (GPU, FPGA thương mại) không bị kiểm soát và giá thành giảm ~50% mỗi 2 năm; trong khi giá cảm biến độ nhạy cao thì không giảm.

Kết luận thiết kế rút ra từ ba điểm trên được trình bày ở mục 5.1.

---

## 2. MỤC TIÊU CHƯƠNG TRÌNH

### 2.1 Tuyên bố mục tiêu

> Xây dựng một hệ thống **thu nhận – ghi – phân tích – hiển thị** tín hiệu thuỷ âm dưới biển, trong đó phần lõi xử lý tín hiệu và thuật toán thuộc sở hữu của Tecotec, có khả năng tái sử dụng sang các cấu hình cảm biến khác nhau.

### 2.2 Tiêu chí thành công

Chương trình được coi là thành công khi đạt đồng thời cả bốn tiêu chí:

| # | Tiêu chí | Cách đo |
|---|---|---|
| TC-1 | Hệ thống phát hiện và bám được mục tiêu hợp tác trong điều kiện biển thực | Thử nghiệm biển có bên thứ ba chứng kiến |
| TC-2 | Nhiễu nội tại hệ thống không phải là yếu tố giới hạn hiệu năng | Đo theo YC-01 (mục 6.1) |
| TC-3 | Chuỗi xử lý tín hiệu chạy được trên **ít nhất hai** cấu hình mảng cảm biến khác nhau | Kiểm thử hồi quy trên dữ liệu của cả hai cấu hình |
| TC-4 | Toàn bộ mã nguồn xử lý, tài liệu thiết kế và bộ dữ liệu hiệu chuẩn nằm trong repo này | Kiểm tra kho tài liệu |

**Ghi chú về TC-3:** đây không phải yêu cầu trang trí. Nếu chuỗi xử lý chỉ chạy được với đúng một mảng phần cứng cụ thể, thì sản phẩm của chương trình là *một hệ thống*, không phải *một nền tảng* — và toàn bộ luận điểm ở mục 1 sụp đổ. Mọi quyết định thiết kế mâu thuẫn với TC-3 phải được escalate.

### 2.3 Phi mục tiêu (Non-goals)

Ghi rõ để tránh phình phạm vi:

- **Không** phát triển gốm áp điện hoặc phần tử thuỷ âm từ đầu. Mua ngoài.
- **Không** phát triển nguồn phát chủ động công suất lớn trong giai đoạn 1.
- **Không** xây dựng hệ thống chỉ huy – điều khiển (C2) cấp chiến dịch. Hệ thống này là **cảm biến và phân tích**, có giao diện xuất dữ liệu để tích hợp lên C2 của khách hàng.
- **Không** tối ưu cho vùng nước sâu trong giai đoạn 1. Xem mục 3.4.

---

## 3. CONOPS — KHÁI NIỆM VẬN HÀNH

### 3.1 Bài toán vận hành

Hệ thống được triển khai để **phát hiện, ghi nhận, phân loại và định hướng** các nguồn âm dưới nước trong một khu vực biển quan tâm, đồng thời **tích luỹ cơ sở dữ liệu chữ ký âm học** của khu vực đó theo thời gian.

Điểm thứ hai quan trọng ngang điểm thứ nhất. Một hệ thống sonar không có thư viện chữ ký âm của chính vùng biển nó hoạt động thì chỉ phát hiện được "có cái gì đó", không phân loại được "cái đó là gì". Cơ sở dữ liệu tích luỹ được là tài sản dài hạn của chương trình.

### 3.2 Kiến trúc vận hành: đa tĩnh (multistatic), không thuần thụ động

`[CHỐT]` Hệ thống được thiết kế theo mô hình **đa tĩnh**: nguồn phát và bộ thu tách rời nhau.

```
        Nguồn âm (do bên vận hành cung cấp
        hoặc nguồn cơ hội trong môi trường)
                    │
                    │  sóng âm lan truyền
                    ▼
    ┌───────────────────────────────────┐
    │   Mục tiêu — phản xạ / bức xạ     │
    └───────────────────────────────────┘
                    │
                    ▼
    ┌───────────────────────────────────┐
    │   MẢNG THU HW-SNR-26 (đặt đáy)    │  ← phạm vi chương trình
    │   + chuỗi xử lý tín hiệu           │
    └───────────────────────────────────┘
                    │
                    ▼
    ┌───────────────────────────────────┐
    │   TRẠM ĐIỀU KHIỂN / PHÂN TÍCH      │  ← phạm vi chương trình
    └───────────────────────────────────┘
```

**Vì sao chọn đa tĩnh thay vì thuần thụ động:**

| | Thuần thụ động | Đa tĩnh |
|---|---|---|
| Phụ thuộc mức ồn mục tiêu | Cao — mục tiêu êm thì không thấy | Thấp hơn — có năng lượng chiếu xạ |
| Độ lợi hình học | Không | Có — hình học phân tán cho độ lợi phát hiện |
| Độ phức tạp đồng bộ | Thấp | Cao — cần đồng bộ thời gian/vị trí chặt |
| Phạm vi bộc lộ kỹ thuật | Toàn bộ chuỗi | Chỉ dải tần thu |

Đánh đổi phải chấp nhận: **yêu cầu đồng bộ thời gian và định vị trở thành ràng buộc thiết kế bậc nhất**, không phải chi tiết triển khai. Xem YC-05, YC-06.

### 3.3 Chu trình vận hành

Một chu kỳ nhiệm vụ điển hình gồm sáu pha:

| Pha | Hoạt động | Đầu ra | Ghi chú kỹ thuật |
|---|---|---|---|
| **P1 — Khảo sát** | Đo địa hình đáy phân giải cao, đo SVP tại chỗ | Bản đồ đáy + profile tốc độ âm | Không dùng số liệu độ sâu từ nguồn thứ cấp. Sai số SVP truyền thẳng vào sai số định hướng |
| **P2 — Triển khai** | Hạ mảng thu xuống đáy, cố định, đấu nối | Mảng hoạt động, vị trí đã ghi nhận | Cần thợ lặn có chứng chỉ ở độ sâu công tác. Xem RR-04 |
| **P3 — Hiệu chuẩn tại chỗ** | Phát nguồn chuẩn đã biết, đo đáp ứng từng phần tử | Ma trận hiệu chuẩn mảng | **Không được bỏ pha này.** Vị trí phần tử thực tế luôn lệch so với thiết kế |
| **P4 — Thu nhận** | Ghi liên tục, giám sát thời gian thực | Dữ liệu thô + dữ liệu đã xử lý | Ghi thô **luôn luôn** được giữ, kể cả khi xử lý thời gian thực đã cho kết quả |
| **P5 — Phân tích** | Xử lý hậu kỳ, phân loại, cập nhật thư viện chữ ký | Báo cáo + bản ghi thư viện | Đây là pha tạo ra tài sản dài hạn |
| **P6 — Thu hồi** | Trục vớt mảng, kiểm tra tình trạng | Thiết bị + nhật ký tình trạng | Nguồn dự phòng phải đủ để phát tín hiệu định vị khi thu hồi |

### 3.4 Môi trường vận hành

| Tham số | Giá trị thiết kế | Trạng thái |
|---|---|---|
| Chế độ triển khai | Đặt đáy (bottom-mounted), cố định | `[CHỐT]` |
| Độ sâu công tác | 20 – 30 m | `[CHỐT]` |
| Giới hạn dưới | ~10 m — dưới mức này mảng phá mặt nước | `[CHỐT]` |
| Giới hạn trên | ~30 – 50 m — trên mức này khó bố trí thợ lặn có chứng chỉ | `[GIẢ ĐỊNH]` |
| Đặc điểm đáy | Nước nông, đáy cứng / rạn | `[GIẢ ĐỊNH]` |
| Nhiễu môi trường | Vùng biển nhiệt đới, nhiễu gió mùa, mật độ giao thông tàu cao | `[GIẢ ĐỊNH]` |
| Nhiệt độ nước | TBD | `[MỞ]` |
| Dòng chảy thiết kế | TBD — ảnh hưởng trực tiếp tới rung mảng và nhiễu dòng | `[MỞ]` |

> **Ràng buộc quan trọng — vòng lặp "con gà và quả trứng":** việc chọn địa điểm triển khai và việc chốt đặc tả kỹ thuật phụ thuộc lẫn nhau. Đặc tả cần biết môi trường; chọn địa điểm cần biết hệ thống chịu được gì. Cách giải quyết đã thống nhất: **lặp** — thiết kế sơ bộ → khảo sát thực địa → hiệu chỉnh đặc tả — chứ không tuần tự. Kỹ sư không nên chờ "chốt địa điểm" mới bắt đầu thiết kế.

### 3.5 Vai trò người dùng

| Vai trò | Nhiệm vụ | Giao diện cần |
|---|---|---|
| Kỹ thuật viên triển khai | Hạ/thu mảng, đấu nối, kiểm tra sơ bộ | Công cụ chẩn đoán tại chỗ, đơn giản, chạy được trên laptop |
| Trắc thủ (operator) | Giám sát thời gian thực, nghe, đánh dấu sự kiện | Màn hình thời gian thực: waterfall, kênh nghe, cảnh báo |
| Nhà phân tích | Xử lý hậu kỳ, phân loại, dựng thư viện chữ ký | Công cụ phân tích ngoại tuyến, truy cập dữ liệu thô |
| Kỹ sư hệ thống | Hiệu chuẩn, chẩn đoán lỗi, cập nhật cấu hình | Truy cập tham số cấp thấp, nhật ký hệ thống |

---

## 4. PHÂN RÃ HỆ THỐNG

### 4.1 Kiến trúc mức đỉnh (L0)

Hệ thống HW-SNR-26 gồm **hai phân hệ triển khai** và **một phân hệ nền tảng**:

```
HW-SNR-26
│
├── SS-A  PHÂN HỆ THU DƯỚI NƯỚC  (Wet End)
│         Mảng thuỷ âm + tiền khuếch đại + số hoá + vỏ chịu áp
│
├── SS-B  TRẠM ĐIỀU KHIỂN & PHÂN TÍCH  (Dry End)
│         Máy tính xử lý + phần mềm + giao diện trắc thủ + lưu trữ
│
└── SS-C  NỀN TẢNG XỬ LÝ TÍN HIỆU  (Core IP — xuyên suốt cả hai)
          Thư viện thuật toán: beamforming, phát hiện, phân loại
```

`[CHỐT]` **SS-C được phát triển và quản lý như một sản phẩm độc lập**, không phải như một thư mục con của SS-B. Đây là hệ quả trực tiếp của tiêu chí TC-3 và là tài sản cốt lõi của chương trình.

### 4.2 Phân rã mức 1 (L1)

| Mã | Phân hệ | Thuộc | Nội dung |
|---|---|---|---|
| L1.1 | Mảng đầu dò thu | SS-A | Phần tử thuỷ âm, khung mảng, vật liệu chắn (baffle) |
| L1.2 | Tiền khuếch đại & Analog Front-End | SS-A | Preamp nhiễu thấp, VGA, lọc thông dải, ADC |
| L1.3 | Số hoá & đồng bộ | SS-A | FPGA thu nhận, gán nhãn thời gian, phân phối xung nhịp |
| L1.4 | Vỏ chịu áp & cơ khí | SS-A | Ống chịu áp, cửa sổ âm học, đế đáy, chống rung |
| L1.5 | Cáp & đầu nối | SS-A/B | Cáp umbilical, đầu nối ướt/khô, đấu nối trong bo |
| L1.6 | Nguồn & phân phối | SS-A/B | DC-DC cách ly, ắc quy, quản lý nguồn, nguồn dự phòng |
| L1.7 | Truyền dẫn dữ liệu | SS-A/B | Đường truyền từ mảng lên trạm |
| L1.8 | Máy tính xử lý | SS-B | CPU/GPU, bộ nhớ, lưu trữ |
| L1.9 | Phần mềm trắc thủ (HMI) | SS-B | Hiển thị thời gian thực, kênh nghe, cảnh báo, ghi nhật ký |
| L1.10 | Công cụ phân tích hậu kỳ | SS-B | Xử lý ngoại tuyến, quản lý thư viện chữ ký |
| L1.11 | Chuỗi xử lý tín hiệu | SS-C | Beamforming, phát hiện, ước lượng hướng, phân loại |
| L1.12 | Hiệu chuẩn & tự kiểm tra | SS-C | Quy trình hiệu chuẩn, BIT, ma trận hiệu chuẩn mảng |

### 4.3 Chi tiết các phân hệ then chốt

#### L1.1 — Mảng đầu dò thu

| Hạng mục | Lựa chọn cơ sở | Trạng thái |
|---|---|---|
| Vật liệu phần tử | PZT-5A hoặc PZT-5H (Navy Type II/VI) | `[GIẢ ĐỊNH]` |
| Phương án thay thế | PMN-PT đơn tinh thể — độ nhạy cao hơn, giá và nguồn cung khó hơn | Nghiên cứu |
| Kết cấu | Ống chứa dầu, bù áp | `[GIẢ ĐỊNH]` |
| Số phần tử | TBD — phụ thuộc quyết định ở mục 5.1 | `[MỞ]` |
| Hình học mảng | TBD — tuyến tính / phẳng / thể tích | `[MỞ]` |

**Lý do chọn ống chứa dầu bù áp:** dầu vừa cân bằng áp suất trong/ngoài (cho phép vỏ mỏng, nhẹ), vừa giữ mảng ở trạng thái nổi trung tính, vừa là môi trường truyền âm có trở kháng gần nước biển. Ba chức năng trong một giải pháp.

**Cảnh báo kỹ thuật:** phương án mảng quang (fibre-optic towed array, kiểu FOTA) loại bỏ hoàn toàn điện tử ở đầu ướt — ưu điểm rất lớn về độ tin cậy và nhiễu. Đây là hướng công nghệ dẫn đầu nhưng **không** chọn cho giai đoạn 1 vì rủi ro triển khai. Ghi nhận là hướng nâng cấp tương lai, không phải phương án bị loại vĩnh viễn.

#### L1.2 — Analog Front-End

Đây là phân hệ quyết định việc hệ thống có đạt YC-01 hay không.

| Khối | Linh kiện tham chiếu | Chỉ tiêu then chốt |
|---|---|---|
| Tiền khuếch đại | ADA4625-1 (JFET) hoặc tương đương | Mật độ nhiễu ~3.3 nV/√Hz |
| Khuếch đại thay đổi độ lợi | HMC960 hoặc tương đương | Dải điều chỉnh ~40 dB |
| ADC | LTC2325-16 / AD7961 / ADS8688 | 16-bit SAR |
| Lọc | Thông dải, chống chồng phổ | Tần số cắt theo dải công tác |

**Nguyên tắc thiết kế bắt buộc:** dải động của chuỗi analog phải phủ **từ mức nhiễu nền của biển yên tĩnh đến mức tín hiệu của nguồn thử cường độ cao**. Không được tối ưu cho một đầu và bão hoà ở đầu kia. Đây là lý do cần VGA lập trình được thay vì độ lợi cố định.

#### L1.4 — Vỏ chịu áp & cơ khí

| Hạng mục | Lựa chọn cơ sở |
|---|---|
| Vật liệu vỏ | Titan Grade 5 (Ti-6Al-4V) hoặc đồng thau hàng hải |
| Cửa sổ âm học | Polyurethane, độ cứng Shore A 60–90 |
| Đầu nối ướt | Chuẩn công nghiệp: SubConn hoặc SEACON |
| Thành phần chịu lực cáp | Kevlar / aramid |

Ở độ sâu công tác 20–30 m, áp suất **không** phải là ràng buộc thiết kế khắc nghiệt. Ràng buộc thực sự là **chống ăn mòn, chống hà bám (biofouling) và độ kín dài hạn**. Cần thiết kế cho chu kỳ ngâm nhiều tháng, không phải cho một chuyến đi biển.

#### L1.11 — Chuỗi xử lý tín hiệu (lõi IP)

```
Dữ liệu thô N kênh
        │
        ▼
[Hiệu chuẩn & bù đáp ứng phần tử]   ← dùng ma trận từ P3
        │
        ▼
[Tạo búp sóng — beamforming]         ← delay-and-sum → MVDR thích nghi
        │
        ▼
[Phân tích phổ / thời gian-tần số]   ← FFT, waterfall, phân tích dải octave
        │
        ▼
[Trích xuất đặc trưng]               ← bao gồm giải điều chế đường bao (DEMON)
        │
        ▼
[Phát hiện & ước lượng hướng]
        │
        ▼
[Bám mục tiêu]                        ← lọc Kalman
        │
        ▼
[Phân loại — đối chiếu thư viện chữ ký]
```

**Ghi chú về DEMON:** kỹ thuật giải điều chế đường bao cho phép trích xuất tần số quay chân vịt từ tín hiệu băng rộng — cơ sở để phân loại loại tàu chứ không chỉ phát hiện có tàu. Bước FFT của đường bao có độ phức tạp O(N log N), đủ nhẹ để chạy trên vi điều khiển; nghĩa là **có thể triển khai một nhánh phát hiện tiêu thụ điện năng rất thấp ngay tại đầu ướt** nếu cần chế độ trực canh dài ngày. Đây là lựa chọn kiến trúc cần cân nhắc sớm, không phải tối ưu hoá muộn.

---

## 5. TRIẾT LÝ THIẾT KẾ

### 5.1 "Cảm biến rẻ, bộ não thông minh"

`[GIẢ ĐỊNH — cần quyết định chính thức, xem mục 11]`

Đây là lựa chọn kiến trúc lớn nhất của chương trình và cần Chief Engineer chốt trước khi bắt đầu thiết kế chi tiết L1.1.

| | Hướng truyền thống | Hướng "cảm biến rẻ, não thông minh" |
|---|---|---|
| Số phần tử | 10 – 50 | 100 – 1000 |
| Độ nhạy mỗi phần tử | Cao — đắt, khó mua, dễ vướng kiểm soát xuất khẩu | Trung bình — rẻ, nguồn cung thương mại |
| Bù đắp bằng | Chất lượng cảm biến | Độ lợi mảng + xử lý thích nghi |
| Chi phí dồn vào | Phần cứng | Tính toán |
| Khả năng mở rộng | Hạn chế | Theo thế hệ GPU |
| Rủi ro kiểm soát xuất khẩu | Cao | Thấp |

Luận điểm: độ lợi mảng tăng theo số phần tử, và năng lực tính toán rẻ đi theo thời gian trong khi cảm biến độ nhạy cao thì không. Với vị thế của Việt Nam trong chuỗi cung ứng, dồn giá trị vào lớp xử lý là lựa chọn phòng thủ được.

**Phản biện phải trả lời trước khi chốt:** mảng nhiều phần tử làm tăng khối lượng dữ liệu, độ phức tạp cáp, số điểm hỏng cơ khí, và độ khó hiệu chuẩn — tất cả đều ở đầu ướt, nơi khó sửa nhất. Chi phí không biến mất, nó **dịch chuyển** từ BOM sang tích hợp và bảo trì. Cần một bài toán đánh đổi định lượng, không phải một khẩu hiệu.

### 5.2 Nguyên tắc mô-đun hoá

`[CHỐT]` Ba nguyên tắc bắt buộc, xuất phát từ TC-3:

1. **Chuỗi xử lý không được biết cấu hình phần cứng cụ thể.** Hình học mảng, số kênh, tần số lấy mẫu đều là **tham số cấu hình**, không phải hằng số biên dịch.
2. **Ranh giới giữa các khối là giao diện dữ liệu có phiên bản**, không phải lời gọi hàm trực tiếp.
3. **Mọi thuật toán phải chạy được ngoại tuyến trên file dữ liệu đã ghi.** Nếu một thuật toán chỉ chạy được khi có phần cứng thật đang cắm, nó không kiểm thử được và không tái sử dụng được.

Nguyên tắc 3 có hệ quả thực tế: **định dạng file dữ liệu thô là một hạng mục thiết kế cần được đặc tả sớm**, không phải thứ để sau. Xem WP-2.

---

## 6. YÊU CẦU KỸ THUẬT

### 6.1 Yêu cầu hiệu năng

| ID | Yêu cầu | Giá trị | Trạng thái | Phương pháp xác minh |
|---|---|---|---|---|
| **YC-01** | Nhiễu nội tại toàn hệ thống phải thấp hơn tỷ số tín/tạp do thuỷ âm cảm nhận (so với nhiễu nền môi trường) **ít nhất 10 dB** | ≥ 10 dB | `[CHỐT]` | Đo trong bể tiêu âm + đo tại chỗ khi biển yên |
| YC-02 | Dải tần công tác | 100 Hz – 4.5 kHz | `[CHỐT]` | Quét đáp ứng tần số |
| YC-03 | Dải tần thu nhận (mở rộng, để phân tích) | Mục tiêu 5 Hz – 100 kHz | `[GIẢ ĐỊNH]` | Quét đáp ứng tần số |
| YC-04 | Cự ly phát hiện tham chiếu | ~5 km với nguồn mức 115–125 dB | `[GIẢ ĐỊNH]` | Thử nghiệm biển với mục tiêu hợp tác |
| YC-05 | Sai số đồng bộ thời gian giữa các kênh | TBD | `[MỞ]` | Đo bằng tín hiệu chuẩn dùng chung |
| YC-06 | Sai số định vị phần tử mảng sau triển khai | TBD | `[MỞ]` | Hiệu chuẩn tại chỗ pha P3 |
| YC-07 | Thời gian hoạt động liên tục | TBD | `[MỞ]` | Thử nghiệm ngâm dài ngày |
| YC-08 | Độ phân giải ADC | 16 bit | `[GIẢ ĐỊNH]` | Kiểm tra thiết kế |

> **YC-01 là yêu cầu ràng buộc bậc nhất của chương trình.** Diễn giải cho kỹ sư: hệ thống không được phép là yếu tố giới hạn hiệu năng — giới hạn phải đến từ môi trường biển. Mọi lựa chọn linh kiện trong L1.2, mọi quyết định về nối đất, che chắn, bố trí nguồn đều phải quy chiếu về yêu cầu này. Nếu một thiết kế không chứng minh được YC-01 bằng ngân sách nhiễu (noise budget) có số liệu, thiết kế đó chưa hoàn thành.

### 6.2 Yêu cầu chức năng

| ID | Yêu cầu | Trạng thái |
|---|---|---|
| CN-01 | Ghi liên tục tín hiệu thuỷ âm ra file, không mất mẫu | `[CHỐT]` |
| CN-02 | Kênh nghe thời gian thực (có thể giới hạn băng thông) | `[CHỐT]` |
| CN-03 | Phân tích theo dải tần và hiển thị waterfall thời gian thực | `[CHỐT]` |
| CN-04 | Ghi nhật ký vị trí GPS xuyên suốt quá trình vận hành | `[CHỐT]` |
| CN-05 | Tính cự ly tại trạm điều khiển để chuẩn hoá dữ liệu âm học | `[CHỐT]` |
| CN-06 | Mã hoá dữ liệu thô và dữ liệu đã xử lý khi lưu trữ | `[CHỐT]` |
| CN-07 | Cấu hình từ xa các tham số thu nhận | `[GIẢ ĐỊNH]` |
| CN-08 | Nguồn dự phòng đủ để phát tín hiệu định vị phục vụ thu hồi | `[CHỐT]` |
| CN-09 | Tự kiểm tra tích hợp (BIT) và báo trạng thái sức khoẻ | `[CHỐT]` |
| CN-10 | Xuất dữ liệu theo giao diện chuẩn để tích hợp lên hệ thống bên thứ ba | `[GIẢ ĐỊNH]` |

### 6.3 Yêu cầu phi chức năng

| ID | Yêu cầu | Ghi chú |
|---|---|---|
| PC-01 | Toàn bộ dữ liệu thô được giữ lại, không ghi đè | Dữ liệu thô là tài sản, không phải sản phẩm phụ |
| PC-02 | Thiết kế cho chu kỳ ngâm nhiều tháng | Chống hà bám, chống ăn mòn |
| PC-03 | Triển khai được bằng đội nhỏ, thiết bị đóng thùng vận chuyển | Theo mô hình bộ kit container hoá |
| PC-04 | Tài liệu và mã nguồn quản lý phiên bản trong repo này | Theo TC-4 |

### 6.4 Tình trạng trưởng thành của tập yêu cầu

Thống kê trạng thái hiện tại — đây là chỉ số sức khoẻ của chương trình, cần theo dõi:

| Trạng thái | Số lượng | Đánh giá |
|---|---|---|
| `[CHỐT]` | 13 | |
| `[GIẢ ĐỊNH]` | 6 | Phải chuyển thành `[CHỐT]` hoặc bị bác bỏ trước cổng G2 |
| `[MỞ]` / TBD | 6 | **Rủi ro** — trong đó YC-05, YC-06 là ràng buộc bậc nhất của kiến trúc đa tĩnh nhưng chưa có số |

> Nhận định thẳng thắn cho Chief Engineer: **một kiến trúc đa tĩnh mà chưa có ngân sách sai số cho đồng bộ thời gian (YC-05) và định vị phần tử (YC-06) thì chưa đủ điều kiện bước vào thiết kế chi tiết.** Hai con số này quyết định độ chính xác định hướng của toàn hệ thống. Đề nghị ưu tiên WP-1 trước mọi việc khác.

---

## 7. HỆ THỐNG THAM CHIẾU

Chương trình sử dụng **SONRAS (Scanmatic, Na Uy)** làm hệ quy chiếu về cấu trúc chức năng — không phải để sao chép, mà để đối chiếu xem hệ thống của chúng ta thiếu chức năng gì so với một sản phẩm thương mại đã trưởng thành.

### 7.1 SONRAS — tóm tắt

Hệ thống hoàn chỉnh để thu nhận, ghi/lưu, nghe, phân tích và hiển thị tín hiệu thuỷ âm trên biển. Gồm hai phân hệ:

- **Phao SONRAS** — đơn vị tự hành, nuôi bằng ắc quy, chứa điện tử thu nhận và xử lý, định vị GPS; hoạt động độc lập tới ~8 giờ, có nguồn dự phòng ~24 giờ để phát vị trí phục vụ thu hồi.
- **Trạm điều khiển (SCS)** — đặt trên tàu, liên lạc với phao qua modem vô tuyến, điện tử đóng trong Pelicase, máy tính chạy phần mềm chuyên dụng, định vị GPS; cho phép cấu hình từ xa và giám sát dữ liệu thời gian thực.

Chuỗi cảm biến dùng tiền khuếch đại nhiễu thấp kết hợp ADC độ phân giải cao và khuếch đại lập trình được, phủ dải động từ nhiễu nền tới vật thử cường độ cao. Dải tần tiêu chuẩn 5 Hz – 100 kHz. Ghi liên tục ra file wave, có kênh nghe thời gian thực giới hạn băng thông, phân tích theo dải tần, mã hoá và lưu trữ cả dữ liệu thô lẫn đã xử lý. Vị trí GPS được ghi suốt quá trình; trạm điều khiển tính cự ly để chuẩn hoá dữ liệu âm học. Bộ kit triển khai đóng thùng gồm phao, điện tử trạm điều khiển, cụm thuỷ âm, phụ kiện và máy tính.

### 7.2 Đối chiếu HW-SNR-26 với SONRAS

| Khía cạnh | SONRAS | HW-SNR-26 | Nhận xét |
|---|---|---|---|
| Chế độ triển khai | Phao nổi tự hành, thả từ tàu | Đặt đáy, cố định | **Khác biệt cơ bản.** Ta đánh đổi tính cơ động lấy thời gian trực canh dài |
| Thời gian hoạt động | ~8 giờ | Mục tiêu nhiều tháng | Hệ quả của lựa chọn trên. Kéo theo bài toán nguồn và hà bám |
| Dải tần | 5 Hz – 100 kHz | Công tác 100 Hz – 4.5 kHz | Ta hẹp hơn nhiều — cần rà lại xem có bỏ sót ứng dụng không |
| Số kênh | Không công bố | 100 – 1000 (nếu chốt 5.1) | Ta đặt cược vào độ lợi mảng |
| Liên kết dữ liệu | Modem vô tuyến | TBD `[MỞ]` | Đặt đáy nên không dùng được RF trực tiếp — đây là vấn đề chưa giải |
| Xử lý | Ghi + phân tích dải tần | Beamforming thích nghi + phân loại | Ta tham vọng hơn ở lớp xử lý — đúng với định hướng ở mục 1 |
| Kiến trúc | Thụ động | Đa tĩnh | Ta cần đồng bộ chặt, SONRAS thì không |
| Mã hoá lưu trữ | Có | Có (CN-06) | Ngang bằng |
| Đóng gói triển khai | Kit container hoá | Kit container hoá (PC-03) | Học trực tiếp |

**Ba bài học rút ra từ đối chiếu:**

1. **Vấn đề liên kết dữ liệu của ta chưa được giải.** SONRAS nổi nên dùng được modem vô tuyến. Ta đặt đáy nên phải chọn: cáp lên phao mặt nước, cáp vào bờ, hay modem thuỷ âm băng hẹp. Ba phương án này có hệ quả hoàn toàn khác nhau lên toàn bộ kiến trúc. **Đây là hạng mục `[MỞ]` nghiêm trọng nhất chưa được nêu trong danh sách yêu cầu** — bổ sung vào mục 11.
2. **Việc ghi liên tục ra định dạng file chuẩn kèm nhật ký GPS và tính cự ly để chuẩn hoá** là mô hình đã được kiểm chứng thương mại. Không cần sáng tạo lại — áp dụng thẳng.
3. **Dải động phủ từ nhiễu nền tới nguồn mạnh** được SONRAS nêu như một đặc tính bán hàng. Với ta nó là YC-01. Cùng một vấn đề vật lý.

---

## 8. LỘ TRÌNH VÀ CỔNG KIỂM SOÁT

Chương trình chia bốn giai đoạn, mỗi giai đoạn kết thúc bằng một cổng có tiêu chí thoát rõ ràng.

| GĐ | Tên | Mục tiêu | Cổng thoát |
|---|---|---|---|
| **G1** | Đồng thuận & Đặc tả | Chốt kiến trúc, đóng các mục `[MỞ]` bậc nhất, lập ngân sách nhiễu và ngân sách sai số | **Cổng G1:** đặc tả kỹ thuật được Chief Engineer duyệt; YC-05, YC-06 có số; quyết định 5.1 đã chốt |
| **G2** | Chứng minh | Nguyên mẫu chuỗi thu + chuỗi xử lý; kiểm chứng YC-01 trong phòng thí nghiệm | **Cổng G2:** YC-01 đạt trên bàn thí nghiệm; chuỗi xử lý chạy trên dữ liệu ghi sẵn |
| **G3** | Thử nghiệm biển | Triển khai thực địa, hiệu chuẩn tại chỗ, thu dữ liệu thật | **Cổng G3:** phát hiện và bám được mục tiêu hợp tác, có bên thứ ba chứng kiến |
| **G4** | Đóng gói & Nhân bản | Chuẩn hoá tài liệu, quy trình sản xuất, chứng minh TC-3 trên cấu hình thứ hai | **Cổng G4:** hệ thống thứ hai chạy trên cùng lõi xử lý |

**Nguyên tắc cổng:** không giải ngân nguồn lực của giai đoạn sau khi cổng của giai đoạn trước chưa đóng. Mốc thời gian tổng thể đang ở mức `[GIẢ ĐỊNH]` khoảng 18 tháng cho G1–G3; con số này chưa được kiểm chứng bằng WBS chi tiết và **không nên dùng để cam kết với bên ngoài** cho đến khi WP-1 hoàn thành.

---

## 9. GÓI CÔNG VIỆC KHỞI ĐỘNG

Dành cho kỹ sư bắt đầu từ hôm nay. Bốn gói này chạy song song được.

### WP-1 — Ngân sách sai số và ngân sách nhiễu `ƯU TIÊN CAO NHẤT`
**Đầu ra:** hai bảng tính có số liệu.
- Ngân sách nhiễu: từ nhiễu nhiệt phần tử → preamp → ADC → nhiễu số hoá, chứng minh YC-01 khả thi hoặc chỉ ra khối nào không đạt.
- Ngân sách sai số: đồng bộ thời gian + định vị phần tử → sai số định hướng cuối cùng. Đây là căn cứ để đặt số cho YC-05, YC-06.

**Vì sao ưu tiên:** hai bảng này quyết định kiến trúc. Làm sau khi đã chọn linh kiện thì đã muộn.

### WP-2 — Đặc tả định dạng dữ liệu và khung xử lý ngoại tuyến
**Đầu ra:** đặc tả định dạng file dữ liệu thô (metadata: hình học mảng, tần số lấy mẫu, nhãn thời gian, GPS, ma trận hiệu chuẩn) + khung chương trình đọc file và chạy được một chuỗi xử lý tối thiểu.

**Vì sao sớm:** theo nguyên tắc 3 ở mục 5.2, toàn bộ phát triển thuật toán về sau phụ thuộc vào việc có khung này. Có thể làm ngay với dữ liệu mô phỏng, không cần chờ phần cứng.

### WP-3 — Nghiên cứu đánh đổi cho quyết định 5.1
**Đầu ra:** báo cáo định lượng so sánh hai hướng kiến trúc mảng, đủ căn cứ để Chief Engineer chốt. Phải bao gồm: độ lợi mảng đạt được, khối lượng dữ liệu, số điểm hỏng, độ khó hiệu chuẩn, và ước tính chi phí tích hợp — không chỉ chi phí linh kiện.

### WP-4 — Giải bài toán liên kết dữ liệu
**Đầu ra:** so sánh ba phương án (cáp lên phao mặt nước / cáp vào bờ / modem thuỷ âm) theo băng thông, độ tin cậy, khả năng bị phát hiện, chi phí triển khai và thu hồi.

**Vì sao gấp:** đây là hạng mục `[MỞ]` được phát hiện muộn (mục 7.2) và nó ảnh hưởng tới cả SS-A lẫn SS-B.

---

## 10. RỦI RO KỸ THUẬT

| ID | Rủi ro | Ảnh hưởng | Hướng xử lý |
|---|---|---|---|
| RR-01 | Nguồn cung gốm áp điện (PZT) bị hạn chế hoặc vướng kiểm soát xuất khẩu | Chặn L1.1 | Xác định tối thiểu hai nhà cung cấp ở hai khu vực pháp lý khác nhau trước cổng G1 |
| RR-02 | Bo mạch xử lý FPGA khó tiếp cận | Chặn L1.3 | Đánh giá phương án COTS thương mại thay cho bo mạch chuyên dụng quốc phòng |
| RR-03 | Không đạt YC-01 do nhiễu nội tại | Hỏng luận điểm hiệu năng | WP-1 phát hiện sớm; dự phòng: cải thiện che chắn/nối đất, tách nguồn |
| RR-04 | Không có thợ lặn chứng chỉ ở độ sâu công tác | Chặn pha P2, P6 | Xác nhận nguồn lực lặn trước khi chốt độ sâu triển khai |
| RR-05 | Hà bám làm suy giảm đáp ứng âm học theo thời gian | Suy giảm hiệu năng dài hạn | Thử nghiệm ngâm dài ngày; nghiên cứu lớp phủ chống bám |
| RR-06 | Sai số SVP làm hỏng độ chính xác định hướng | Sai số hệ thống | Đo SVP tại chỗ, không dùng số liệu thứ cấp; đưa SVP thành tham số vận hành |
| RR-07 | Liên kết dữ liệu chưa có phương án | Rủi ro kiến trúc | WP-4 |
| RR-08 | Chuỗi xử lý bị gắn chặt vào một cấu hình phần cứng | Mất TC-3, mất giá trị nền tảng | Thực thi nghiêm nguyên tắc mục 5.2; kiểm thử trên dữ liệu mô phỏng của cấu hình thứ hai từ sớm |

---

## 11. DANH MỤC QUYẾT ĐỊNH CÒN MỞ

Danh sách này là **phần quan trọng nhất của tài liệu đối với Chief Engineer**. Không kỹ sư nào được tự quyết các mục dưới đây.

| # | Quyết định cần đưa ra | Chặn việc gì | Đề xuất |
|---|---|---|---|
| QĐ-1 | Chốt hướng kiến trúc mảng (mục 5.1) | Toàn bộ L1.1, L1.2, L1.3 | Chờ WP-3 |
| QĐ-2 | Giá trị YC-05 (sai số đồng bộ thời gian) | Thiết kế L1.3 | Chờ WP-1 |
| QĐ-3 | Giá trị YC-06 (sai số định vị phần tử) | Quy trình hiệu chuẩn P3 | Chờ WP-1 |
| QĐ-4 | Phương án liên kết dữ liệu | L1.7, kiến trúc SS-B | Chờ WP-4 |
| QĐ-5 | Số phần tử và hình học mảng | L1.1, L1.4 | Sau QĐ-1 |
| QĐ-6 | Thời gian hoạt động liên tục mục tiêu (YC-07) | Thiết kế nguồn L1.6 | Cần đầu vào từ khái niệm vận hành của người dùng |
| QĐ-7 | Dải tần thu nhận mở rộng có cần thiết không (YC-03) | Chọn ADC, tần số lấy mẫu | Cần rà soát ứng dụng |
| QĐ-8 | Nhiệt độ nước và dòng chảy thiết kế | L1.4, phân tích rung | Cần dữ liệu khảo sát |

---

## 12. THUẬT NGỮ

| Viết tắt | Tiếng Anh | Giải thích |
|---|---|---|
| ADC | Analog-to-Digital Converter | Bộ chuyển đổi tương tự – số |
| BIT | Built-In Test | Tự kiểm tra tích hợp |
| CONOPS | Concept of Operations | Khái niệm vận hành |
| DEMON | Detection of Envelope Modulation On Noise | Giải điều chế đường bao trên nhiễu — trích tần số quay chân vịt |
| DoA | Direction of Arrival | Hướng tới của tín hiệu |
| FPGA | Field-Programmable Gate Array | Mạch logic khả trình |
| HMI | Human-Machine Interface | Giao diện người – máy |
| MVDR | Minimum Variance Distortionless Response | Thuật toán tạo búp sóng thích nghi |
| PZT | Lead Zirconate Titanate | Gốm áp điện |
| PMN-PT | — | Vật liệu áp điện đơn tinh thể, độ nhạy cao |
| SVP | Sound Velocity Profile | Profile tốc độ âm theo độ sâu |
| VGA | Variable Gain Amplifier | Khuếch đại độ lợi thay đổi được |
| Baffle | — | Vật liệu chắn âm, định hướng đáp ứng mảng |
| Beamforming | — | Tạo búp sóng — tổ hợp tín hiệu nhiều phần tử để định hướng |
| Biofouling | — | Hà bám, sinh vật bám vào bề mặt ngâm nước |
| Multistatic | — | Đa tĩnh — nguồn phát và bộ thu tách rời |

---

## 13. LỊCH SỬ SỬA ĐỔI

| Phiên bản | Ngày | Nội dung | Người thực hiện |
|---|---|---|---|
| 0.1 | 2026-08-25 | Bản thảo đầu tiên. Thiết lập CONOPS, phân rã hệ thống, tập yêu cầu ban đầu, danh mục quyết định mở | — |

---

## 14. GHI CHÚ VỀ PHẠM VI TÀI LIỆU

Tài liệu này cố ý **chỉ chứa nội dung kỹ thuật**. Các nội dung sau **không** thuộc phạm vi tài liệu này và được quản lý riêng theo kênh có kiểm soát truy cập:

- Điều khoản thương mại, giá, cấu trúc chi phí và phương án tài chính
- Danh tính đối tác, cấu trúc pháp lý và các vấn đề kiểm soát xuất khẩu
- Địa điểm triển khai cụ thể và dữ liệu khảo sát thực địa
- Thông tin về khách hàng, tiến trình phê duyệt và quan hệ đối tác

Kỹ sư cần các thông tin trên để ra quyết định kỹ thuật, hãy đề nghị qua Chief Engineer thay vì đưa vào tài liệu này hoặc vào repo.
