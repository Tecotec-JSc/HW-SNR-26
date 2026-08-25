# HW-SNR-26 — ĐIỀU LỆ DỰ ÁN (PROJECT CHARTER)

**Hệ thống Thu nhận, Ghi và Phân tích Tín hiệu Thuỷ âm**
*Sound Registration and Analyzing System*

| | |
|---|---|
| **Mã chương trình** | HW-SNR-26 |
| **Phiên bản tài liệu** | 0.2 — Bản thảo |
| **Ngày** | 2026-08-25 |
| **Trạng thái** | Đang soạn thảo — chờ Chief Engineer phê duyệt |
| **Người đọc đầu tiên** | Chief Engineer |
| **Đối tượng** | Kỹ sư tham gia dự án từ ngày đầu tiên |
| **Hệ tham chiếu** | Scanmatic TMK-SAS / SONRAS (Na Uy) — tài liệu trong `reference/` |
| **Phân loại** | Nội bộ — kỹ thuật |

---

## 0. CÁCH ĐỌC TÀI LIỆU NÀY

Đây là tài liệu **khởi động dự án**, không phải đặc tả kỹ thuật (specification) và cũng không phải thiết kế chi tiết. Mục tiêu: trả lời bốn câu hỏi mà một kỹ sư mới vào dự án cần biết trong ngày đầu tiên.

1. Chúng ta đang xây cái gì, dùng để làm gì? → Mục 2, 3
2. Hệ thống chia thành những khối nào? → Mục 4, 5
3. Cái gì đã chốt, cái gì còn mở? → Mục 6, 11
4. Tôi bắt đầu từ đâu? → Mục 9

**Quy ước:**

| Ký hiệu | Ý nghĩa |
|---|---|
| `[CHỐT]` | Đã quyết định, không đổi nếu không qua change request |
| `[GIẢ ĐỊNH]` | Đang dùng làm cơ sở thiết kế nhưng **chưa xác minh** — phải kiểm chứng trước khi khoá thiết kế |
| `[MỞ]` | Chưa có quyết định. **Không được tự ý chọn thay** — ghi vào mục 11, báo Chief Engineer |
| `[THAM CHIẾU]` | Số liệu lấy từ hệ tham chiếu TMK-SAS, dùng làm mốc so sánh — **không phải** yêu cầu của ta |
| TBD | Chưa có số liệu |

> **Cảnh báo:** phần lớn con số trong tài liệu này đang ở trạng thái `[GIẢ ĐỊNH]` hoặc `[THAM CHIẾU]`. Đó là bình thường ở phiên bản 0.2. Điều **không** bình thường là để nguyên như vậy khi bước vào thiết kế chi tiết. Xem mục 6.5.

---

## 1. VẤN ĐỀ CẦN GIẢI QUYẾT TRƯỚC TIÊN

> ### ⚠ QĐ-0 — CHƯA CHỐT NHIỆM VỤ CHÍNH CỦA HỆ THỐNG
>
> Tài liệu tham chiếu được cung cấp (`reference/6. Catalog TMK-SAS.pdf`) mô tả một hệ thống **đo đạc chữ ký âm học** — đo tiếng ồn của chính tàu/công trình của mình. Trong khi đó, bối cảnh chương trình còn có hướng **giám sát thuỷ âm** — phát hiện mục tiêu của bên khác.
>
> **Đây là hai nhiệm vụ khác nhau, dẫn tới hai hệ thống khác nhau.** Bảng dưới cho thấy mức độ khác biệt:

| Khía cạnh | **Hướng A — Đo chữ ký âm học** | **Hướng B — Giám sát thuỷ âm** |
|---|---|---|
| Câu hỏi trả lời | "Tàu của tôi ồn bao nhiêu dB?" | "Có mục tiêu nào trong khu vực không?" |
| Mục tiêu đo | Hợp tác, biết trước, đi theo lộ trình định sẵn | Không hợp tác, không biết trước |
| Cự ly làm việc | Gần, đã biết (tính được từ GPS hai đầu) | Xa, phải ước lượng |
| Yêu cầu bậc nhất | **Độ chính xác tuyệt đối** của phép đo | **Độ nhạy phát hiện** ở tỷ số tín/tạp thấp |
| Kết quả đầu ra | Mức phổ đã chuẩn hoá theo cự ly (dB re 1µPa @1m) | Phát hiện / hướng / phân loại |
| Số phần tử thu | Ít — thường 1 thuỷ âm hiệu chuẩn chính xác | Nhiều — mảng, cần độ lợi mảng |
| Beamforming | Không cần | Bắt buộc |
| Hiệu chuẩn | **Then chốt** — phải truy xuất được về chuẩn quốc gia | Quan trọng cho định hướng, không cần độ chính xác tuyệt đối |
| Triển khai | Phao nổi, cơ động, theo từng đợt đo | Đặt đáy, cố định, trực canh dài ngày |
| Nguồn điện | Giờ | Tháng |
| Đường truyền | Vô tuyến UHF/VHF (phao nổi) | Chưa giải được — xem QĐ-4 |

**Vì sao phải chốt trước mọi việc khác:** hai hướng này khác nhau ở lớp yêu cầu bậc nhất, không phải ở chi tiết triển khai. Chọn sai thì thiết kế cảm biến, kiến trúc xử lý, cơ khí, nguồn điện và quy trình hiệu chuẩn đều sai theo.

**Đề xuất của người soạn thảo — Hướng A trước, Hướng B sau.** Bốn lý do kỹ thuật và thương mại:

1. Hướng A có **hệ tham chiếu đã trưởng thành** (TMK-SAS) với đặc tả công khai đủ chi tiết để đối chiếu từng chỉ tiêu. Hướng B không có.
2. Hướng A là bài toán **đo lường** — nơi năng lực đã được kiểm chứng của đơn vị nằm ở đó (hiệu chuẩn thuỷ âm, hệ chuẩn, thử nghiệm). Hướng B đòi hỏi năng lực xử lý mảng chưa có.
3. Hướng A tạo ra **tài sản dùng lại được cho Hướng B**: chuỗi thu nhiễu thấp, hiệu chuẩn truy xuất được, định dạng dữ liệu, công cụ phân tích phổ. Chiều ngược lại không đúng.
4. Hướng A có **đường ra thị trường ngắn hơn**: đo chữ ký tàu, xác định cự ly an toàn cho tàu quét mìn, kiểm tra công trình ngầm — đều là dịch vụ đo bán được ngay, không cần chờ một chương trình dài hạn.

Phần còn lại của tài liệu này được viết theo **Hướng A**, và đánh dấu rõ những chỗ Hướng B sẽ khác. Nếu Chief Engineer chốt Hướng B, các mục 3, 4, 6 phải viết lại.

---

## 2. MỤC TIÊU CHƯƠNG TRÌNH

### 2.1 Tuyên bố mục tiêu (theo Hướng A)

> Xây dựng hệ thống hoàn chỉnh để **thu nhận – ghi – phân tích – hiển thị** tín hiệu thuỷ âm dưới biển, phục vụ đo và lập bản đồ chữ ký âm học của tàu và công trình biển, trong đó **phần lõi xử lý tín hiệu và quy trình hiệu chuẩn thuộc sở hữu của đơn vị** và tái sử dụng được sang các cấu hình cảm biến khác.

### 2.2 Nhóm ứng dụng mục tiêu

Kế thừa từ hệ tham chiếu, có điều chỉnh theo bối cảnh trong nước:

| # | Ứng dụng | Ghi chú |
|---|---|---|
| UD-1 | Lập bản đồ chữ ký tiếng ồn của tàu | Ứng dụng chính |
| UD-2 | Xác định **cự ly an toàn** cho tàu quét mìn | Liên hệ trực tiếp với công việc mô phỏng ngòi nổ ảnh hưởng đang triển khai |
| UD-3 | Đo tàu cá trong quá trình dò tìm và đánh bắt | Ứng dụng dân sự |
| UD-4 | Phát hiện hư hỏng công trình ngầm dưới đáy biển | Giám sát tình trạng thiết bị |
| UD-5 | Ghi nhận tiếng ồn sinh học, gồm âm thanh động vật biển | Ứng dụng môi trường / nghiên cứu |
| UD-6 | Xác định vị trí nguồn sóng xung kích | Cần nhiều trạm — xem mục 4.4 |

> **Ghi chú cho Chief Engineer:** UD-2 đáng chú ý về mặt chiến lược. Nó nối trực tiếp vào công việc mô phỏng ngòi nổ ảnh hưởng (kênh từ trường) đã đang chạy. Hai hệ thống đo hai kênh vật lý khác nhau (âm và từ) của cùng một bài toán: tàu gây ra trường gì, ở cự ly nào thì an toàn. Nếu định vị chương trình theo hướng này, hai việc dùng chung được khung đo, khung hiệu chuẩn và khung báo cáo.

### 2.3 Tiêu chí thành công

| # | Tiêu chí | Cách đo |
|---|---|---|
| TC-1 | Đo được mức phổ chữ ký của một tàu thật, kết quả có sai số công bố được | Đo đối chứng với thiết bị đã hiệu chuẩn |
| TC-2 | Nhiễu nội tại hệ thống không phải yếu tố giới hạn hiệu năng | Đo theo YC-01 |
| TC-3 | Chuỗi xử lý chạy được trên **ít nhất hai** cấu hình cảm biến khác nhau | Kiểm thử hồi quy trên dữ liệu cả hai cấu hình |
| TC-4 | Kết quả đo **truy xuất được** về chuẩn đo lường | Hồ sơ hiệu chuẩn có chuỗi truy xuất |
| TC-5 | Toàn bộ mã nguồn, tài liệu thiết kế, bộ dữ liệu hiệu chuẩn nằm trong repo này | Kiểm tra kho tài liệu |

**Ghi chú về TC-3:** nếu chuỗi xử lý chỉ chạy được với đúng một cấu hình phần cứng, sản phẩm của chương trình là *một hệ thống*, không phải *một nền tảng*. Mọi quyết định thiết kế mâu thuẫn với TC-3 phải được escalate.

**Ghi chú về TC-4:** với Hướng A, đây là tiêu chí **phân biệt sản phẩm**. Một phép đo âm học không truy xuất được về chuẩn thì không dùng để nghiệm thu hay để ra quyết định an toàn được — nó chỉ là một con số. Đây cũng là chỗ tạo ra giá trị giữ lại cao nhất cho đơn vị.

### 2.4 Phi mục tiêu

- **Không** phát triển gốm áp điện hay phần tử thuỷ âm từ đầu. Mua ngoài.
- **Không** phát triển nguồn phát chủ động công suất lớn trong giai đoạn 1.
- **Không** xây hệ thống chỉ huy – điều khiển cấp chiến dịch. Hệ này là **đo và phân tích**, có giao diện xuất dữ liệu.
- **Không** làm hệ giám sát trực canh dài ngày trong giai đoạn 1 (đó là Hướng B).

---

## 3. CONOPS — KHÁI NIỆM VẬN HÀNH

### 3.1 Sơ đồ tổng thể

Kiến trúc hai phân hệ, kế thừa từ hệ tham chiếu. Xem `reference/con-of diagram.jpg`.

```
                      ┌─────────────┐
                      │  VỆ TINH    │
                      │    GNSS     │
                      └──────┬──────┘
              ┌──────────────┴──────────────┐
              │ tín hiệu định vị + thời gian │
              ▼                              ▼
┌──────────────────────────┐      ┌──────────────────────────┐
│      PHAO ĐO (SS-A)      │      │  TRẠM ĐIỀU KHIỂN (SS-B)  │
│                          │      │                          │
│  ┌────────────────────┐  │      │   ┌──────────────────┐   │
│  │ Thu GNSS           │  │      │   │ Thu GNSS         │   │
│  └────────────────────┘  │      │   └──────────────────┘   │
│  ┌────────────────────┐  │◄────►│   ┌──────────────────┐   │
│  │ Modem vô tuyến     │  │ UHF  │   │ Modem vô tuyến   │   │
│  │ + kênh thoại       │  │ VHF  │   │ + kênh nghe      │   │
│  └────────────────────┘  │      │   └──────────────────┘   │
│  ┌────────────────────┐  │      │   ┌──────────────────┐   │
│  │ Bộ xử lý           │  │      │   │ Máy tính + phần  │   │
│  │ Lưu trữ + FFT      │  │      │   │ mềm trắc thủ     │   │
│  └────────▲───────────┘  │      │   └──────────────────┘   │
│  ┌────────┴───────────┐  │      │                          │
│  │ Tiền khuếch đại    │  │      │  Đặt trên tàu hoặc       │
│  └────────▲───────────┘  │      │  công trình biển         │
│  ┌────────┴───────────┐  │      └──────────────────────────┘
│  │ KĐ thuỷ âm         │  │
│  └────────▲───────────┘  │
│           │              │
│      ┌────┴────┐         │
│      │ Thuỷ âm │         │
│      └─────────┘         │
└──────────────────────────┘
```

### 3.2 Nguyên lý vận hành

Phao ghi âm từ biển, phân tích tại chỗ và truyền **kết quả đã xử lý** về trạm điều khiển theo thời gian thực. **Dữ liệu thô** được lưu trong phao và chỉ lấy ra sau khi thu hồi, qua kết nối tốc độ cao.

Đây là một quyết định kiến trúc quan trọng và cần hiểu đúng: **đường truyền vô tuyến không đủ băng thông để tải dữ liệu thô**, nên hệ thống chia đôi luồng dữ liệu:

| Luồng | Nội dung | Đường đi | Thời điểm |
|---|---|---|---|
| Thời gian thực | Dữ liệu dải 1/3 octave đã rút gọn + trạng thái | Vô tuyến UHF | Trong lúc đo |
| Kênh nghe | Tín hiệu tương tự băng hẹp | Vô tuyến VHF | Trong lúc đo, khi trắc thủ kích hoạt |
| Dữ liệu thô | Toàn bộ mẫu chưa xử lý | Bộ nhớ trong phao → Ethernet | Sau khi thu hồi |

**Hệ quả cho kỹ sư phần mềm:** phải viết **hai** chuỗi xử lý — một chạy trong phao dưới ràng buộc điện năng và thời gian thực, một chạy trên trạm khi phân tích hậu kỳ không bị ràng buộc. Hai chuỗi này phải cho **kết quả nhất quán** trên cùng một tập dữ liệu. Đây là một yêu cầu kiểm thử, không phải mong muốn.

### 3.3 Chuẩn hoá theo cự ly — cơ chế cốt lõi của Hướng A

Cả phao và trạm đều có định vị GNSS, và **toàn bộ vệt vị trí được lưu trong phao**. Trạm dùng hai vệt vị trí để tính **cự ly tương đối theo thời gian**, rồi dùng cự ly đó để chuẩn hoá số liệu tiếng ồn về mức nguồn.

Không có bước này thì phép đo vô nghĩa: cùng một con tàu đo ở 50 m và 200 m cho hai con số khác nhau. Chữ ký âm học chỉ có ý nghĩa khi quy về mức nguồn chuẩn.

```
Mức đo được tại thuỷ âm  ──┐
                           ├──► Mức nguồn (dB re 1µPa @ 1m)
Cự ly tính từ GNSS       ──┤
                           │
Mô hình suy hao truyền âm ─┘
```

> **Điểm cần chú ý:** mô hình suy hao truyền âm là **giả định**, không phải phép đo. Ở vùng nước nông, suy hao lệch khỏi mô hình lan truyền cầu rất nhiều. Sai số của mô hình này truyền thẳng vào kết quả cuối. Cần đưa vào ngân sách sai số (WP-1) và cần đo profile tốc độ âm tại chỗ để hiệu chỉnh.

### 3.4 Chu trình đo

| Pha | Hoạt động | Đầu ra |
|---|---|---|
| **P1 — Chuẩn bị** | Kiểm tra hiệu chuẩn thuỷ âm, nạp nguồn, kiểm tra đường truyền, thống nhất lộ trình chạy của tàu đo | Hồ sơ tiền nhiệm vụ, checklist đã ký |
| **P2 — Thả phao** | Thả phao xuống vị trí, xác nhận GNSS, xác nhận liên lạc với trạm | Phao hoạt động, vị trí đã ghi |
| **P3 — Đo tại chỗ nền** | Ghi nhiễu nền khi chưa có mục tiêu | Mức nhiễu nền — **bắt buộc**, là mốc so sánh |
| **P4 — Chạy đo** | Tàu chạy theo lộ trình định sẵn qua phao; ghi liên tục; trắc thủ giám sát | Dữ liệu thô + dữ liệu 1/3 octave + vệt GNSS |
| **P5 — Thu hồi** | Trục vớt phao, kiểm tra tình trạng | Thiết bị + nhật ký tình trạng |
| **P6 — Hậu kỳ** | Lấy dữ liệu thô qua Ethernet, phân tích băng hẹp, dựng báo cáo | Báo cáo chữ ký + bản ghi thư viện |

**P3 không được bỏ.** Nếu không có mức nhiễu nền của chính buổi đo đó, không biết được kết quả đo có bị nhiễu môi trường chi phối hay không — và không phát hiện được trường hợp mục tiêu êm hơn nhiễu nền.

### 3.5 Chế độ vận hành của trạm điều khiển

Kế thừa bốn chế độ của hệ tham chiếu:

| Chế độ | Nội dung | Trạng thái |
|---|---|---|
| **CĐ-1 Giám sát thu thập** | Hiển thị dữ liệu 1/3 octave theo chu kỳ cập nhật, quy về mức phổ thực; kích hoạt kênh nghe | `[CHỐT]` |
| **CĐ-2 Phát lại** | Phát lại dữ liệu đã xử lý cùng tín hiệu tương tự đã ghi | `[CHỐT]` |
| **CĐ-3 Định vị sự kiện** | Nhận sự kiện có gán nhãn thời gian từ **nhiều phao** để định vị nguồn âm | `[GIẢ ĐỊNH]` — tuỳ chọn |
| **CĐ-4 Xử lý hậu kỳ** | Phân tích lại dữ liệu thô với phân giải băng hẹp | `[CHỐT]` |

### 3.6 Môi trường vận hành

| Tham số | Giá trị | Trạng thái |
|---|---|---|
| Chế độ triển khai | Phao nổi, thả từ tàu, theo đợt đo | `[GIẢ ĐỊNH]` — theo Hướng A |
| Thời gian một đợt đo | Giờ, không phải tháng | `[GIẢ ĐỊNH]` |
| Vùng nước | Nước nông ven bờ | `[GIẢ ĐỊNH]` |
| Nhiễu môi trường | Biển nhiệt đới, nhiễu gió mùa, mật độ giao thông tàu cao | `[GIẢ ĐỊNH]` |
| Trạng thái biển thiết kế | TBD | `[MỞ]` |
| Nhiệt độ nước | TBD | `[MỞ]` |

### 3.7 Vai trò người dùng

| Vai trò | Nhiệm vụ | Giao diện cần |
|---|---|---|
| Kỹ thuật viên triển khai | Thả/thu phao, kiểm tra sơ bộ | Công cụ chẩn đoán tại chỗ, chạy trên laptop |
| Trắc thủ | Giám sát thời gian thực, nghe, đánh dấu sự kiện | Màn hình phổ thời gian thực, kênh nghe, cảnh báo |
| Nhà phân tích đo lường | Xử lý hậu kỳ, chuẩn hoá, lập báo cáo chữ ký | Công cụ phân tích ngoại tuyến, truy cập dữ liệu thô |
| Kỹ sư hiệu chuẩn | Hiệu chuẩn thuỷ âm, duy trì chuỗi truy xuất | Quy trình + hồ sơ hiệu chuẩn |

---

## 4. PHÂN RÃ HỆ THỐNG

### 4.1 Mức đỉnh (L0)

```
HW-SNR-26
│
├── SS-A  PHAO ĐO
│         Thuỷ âm + chuỗi khuếch đại + số hoá + xử lý + lưu trữ + vô tuyến + GNSS
│
├── SS-B  TRẠM ĐIỀU KHIỂN
│         Máy tính + phần mềm trắc thủ + vô tuyến + GNSS + lưu trữ
│
└── SS-C  NỀN TẢNG XỬ LÝ & HIỆU CHUẨN  (lõi IP — xuyên suốt cả hai)
          Thư viện thuật toán, quy trình và hồ sơ hiệu chuẩn
```

`[CHỐT]` **SS-C được phát triển và quản lý như một sản phẩm độc lập**, không phải thư mục con của SS-B. Đây là hệ quả trực tiếp của TC-3 và TC-4, và là tài sản cốt lõi của chương trình.

### 4.2 Mức 1 (L1)

| Mã | Phân hệ | Thuộc | Nội dung |
|---|---|---|---|
| L1.1 | Cụm thuỷ âm | SS-A | Phần tử thu, cáp, cơ cấu treo, chống rung |
| L1.2 | Khuếch đại thuỷ âm & tiền khuếch đại | SS-A | Preamp nhiễu thấp, khuếch đại điều khiển được |
| L1.3 | Số hoá | SS-A | Lọc chống chồng phổ, ADC, đồng hồ lấy mẫu |
| L1.4 | Bộ xử lý trong phao | SS-A | Phân tích dải tần, quản lý lưu trữ, đóng gói bản tin |
| L1.5 | Lưu trữ trong phao | SS-A | Bộ nhớ dữ liệu thô, mã hoá |
| L1.6 | GNSS & đồng bộ thời gian | SS-A/B | Định vị, gán nhãn thời gian |
| L1.7 | Liên lạc vô tuyến | SS-A/B | Modem dữ liệu, kênh nghe tương tự |
| L1.8 | Kết cấu phao & cơ khí | SS-A | Thân phao, độ nổi, ổn định, chống nước |
| L1.9 | Nguồn điện | SS-A | Ắc quy, quản lý nguồn, nguồn dự phòng định vị |
| L1.10 | Máy tính trạm | SS-B | Phần cứng tính toán, lưu trữ |
| L1.11 | Phần mềm trắc thủ (HMI) | SS-B | 4 chế độ vận hành ở mục 3.5 |
| L1.12 | Chuỗi xử lý tín hiệu | SS-C | Phân tích phổ, chuẩn hoá, trích đặc trưng |
| L1.13 | Hiệu chuẩn & truy xuất chuẩn | SS-C | Quy trình, hồ sơ, chuỗi truy xuất, tự kiểm tra |

### 4.3 Chi tiết các phân hệ then chốt

#### L1.1 + L1.2 — Cụm thuỷ âm và chuỗi analog

Đây là phân hệ quyết định hệ thống có đạt YC-01 và TC-4 hay không.

| Hạng mục | Lựa chọn cơ sở | Trạng thái |
|---|---|---|
| Vật liệu phần tử | PZT-5A hoặc PZT-5H | `[GIẢ ĐỊNH]` |
| Số phần tử | 1 (Hướng A) — mảng nhiều phần tử là Hướng B | `[GIẢ ĐỊNH]` |
| Tiền khuếch đại | Loại JFET nhiễu thấp; tham chiếu ~3.3 nV/√Hz | `[GIẢ ĐỊNH]` |
| Khuếch đại điều khiển được | Dải điều chỉnh ~40 dB | `[GIẢ ĐỊNH]` |
| Hiệu chuẩn | Truy xuất được về chuẩn quốc gia | `[CHỐT]` |

**Nguyên tắc thiết kế bắt buộc:** dải động của chuỗi analog phải phủ **từ mức nhiễu nền biển yên tĩnh đến mức vật thử rất ồn**. Không được tối ưu một đầu rồi bão hoà đầu kia. Đây là lý do cần khuếch đại lập trình được thay vì độ lợi cố định — và cũng là lý do hệ tham chiếu nêu đặc tính này lên đầu bảng thông số.

#### L1.3 — Số hoá

| Tham số | `[THAM CHIẾU]` TMK-SAS | Yêu cầu HW-SNR-26 |
|---|---|---|
| Độ phân giải | 16 bit | `[GIẢ ĐỊNH]` 16 bit — xem QĐ-7 |
| Tần số lấy mẫu | 42 kS/s | `[MỞ]` — phụ thuộc dải tần chốt ở YC-02 |
| Dải phân tích trực tiếp | 3 Hz – 20 kHz | `[MỞ]` |
| Dải phân tích bằng lọc số | 20 kHz – 100 kHz | `[MỞ]` |

#### L1.4 — Bộ xử lý trong phao

Ràng buộc thiết kế: chạy phân tích phổ liên tục, ghi song song dữ liệu thô, đóng gói bản tin định kỳ, tất cả dưới ngân sách điện năng của ắc quy.

Hệ tham chiếu cập nhật trạm mỗi **1.5 s** với dữ liệu 1/3 octave trên toàn dải. Con số này là mốc so sánh hợp lý cho chu kỳ cập nhật của ta.

#### L1.7 — Liên lạc vô tuyến

| Kênh | Chức năng | `[THAM CHIẾU]` TMK-SAS |
|---|---|---|
| Kênh dữ liệu | Lệnh cấu hình xuống, kết quả + trạng thái lên | Modem UHF |
| Kênh nghe | Truyền tín hiệu tương tự băng hẹp cho trắc thủ nghe | Vô tuyến VHF, ~100 Hz – 3 kHz |

**Cả hai chiều đều phải mã hoá** (CN-06). Hệ tham chiếu mã hoá cả dữ liệu lưu trong phao lẫn dữ liệu truyền qua vô tuyến.

#### L1.12 — Chuỗi xử lý tín hiệu

```
Dữ liệu thô
      │
      ▼
[Bù đáp ứng thuỷ âm]         ← dùng hồ sơ hiệu chuẩn — bước tạo ra tính truy xuất
      │
      ▼
[Phân tích phổ]              ← 1/3 octave thời gian thực; băng hẹp khi hậu kỳ
      │
      ▼
[Quy về mức phổ thực]
      │
      ▼
[Chuẩn hoá theo cự ly]       ← dùng vệt GNSS hai đầu + mô hình truyền âm
      │
      ▼
[Mức nguồn — dB re 1µPa @1m]
      │
      ▼
[Đối chiếu / lưu thư viện chữ ký]
```

**Nhánh mở rộng (Hướng B):** nếu chuyển sang giám sát, chèn thêm khối *tạo búp sóng* sau bước bù hiệu chuẩn, và thay khối chuẩn hoá cự ly bằng khối *ước lượng hướng + bám mục tiêu*. Kiến trúc phần mềm nên chừa sẵn chỗ cho hai khối này thay vì phải viết lại.

### 4.4 Cấu hình nhiều phao — định vị nguồn

`[GIẢ ĐỊNH]` Chế độ tuỳ chọn CĐ-3 yêu cầu **nhiều phao** cùng gán nhãn thời gian cho một sự kiện, rồi trạm dùng chênh lệch thời gian đến để định vị nguồn (đa phương vị / multilateration).

Hệ tham chiếu công bố độ chính xác gán nhãn thời gian **tốt hơn 1 ms** theo giờ GNSS.

> **Đây là con số quan trọng nhất mà kỹ sư cần hiểu ý nghĩa.** Với tốc độ âm trong nước ~1500 m/s, sai số thời gian 1 ms tương đương sai số cự ly **~1.5 m**. Nếu muốn định vị chính xác hơn, phải siết sai số đồng bộ chặt hơn tương ứng. Quan hệ này là tuyến tính và không tránh được — nó đặt trần cho toàn bộ độ chính xác định vị của hệ thống.

---

## 5. TRIẾT LÝ THIẾT KẾ

### 5.1 Nguyên tắc mô-đun hoá

`[CHỐT]` Ba nguyên tắc bắt buộc, xuất phát từ TC-3:

1. **Chuỗi xử lý không được biết cấu hình phần cứng cụ thể.** Số kênh, tần số lấy mẫu, hệ số hiệu chuẩn, hình học cảm biến đều là **tham số cấu hình**, không phải hằng số biên dịch.
2. **Ranh giới giữa các khối là giao diện dữ liệu có phiên bản**, không phải lời gọi hàm trực tiếp.
3. **Mọi thuật toán phải chạy được ngoại tuyến trên file dữ liệu đã ghi.** Thuật toán chỉ chạy được khi có phần cứng thật đang cắm thì không kiểm thử được và không tái sử dụng được.

Nguyên tắc 3 kéo theo: **định dạng file dữ liệu thô là hạng mục thiết kế phải đặc tả sớm**, không phải việc để sau. Xem WP-2.

### 5.2 Hiệu chuẩn là tính năng, không phải thủ tục

`[CHỐT]` Với Hướng A, tính truy xuất chuẩn (TC-4) là thứ phân biệt sản phẩm này với một máy ghi âm dưới nước. Hệ quả thiết kế:

- Hồ sơ hiệu chuẩn đi **cùng dữ liệu**, nhúng trong metadata của file, không nằm rời trong một thư mục khác.
- Mỗi kết quả đo phải truy ngược được ra: thuỷ âm nào, hiệu chuẩn ngày nào, hệ số bao nhiêu, chuỗi truy xuất tới chuẩn nào.
- Hệ thống phải có **tự kiểm tra** (BIT) phát hiện được trường hợp đáp ứng trôi khỏi hồ sơ hiệu chuẩn.

Nếu ba điều trên không được thiết kế từ đầu mà chắp vá về sau, chi phí sẽ rất lớn và tính truy xuất thường không khôi phục được.

### 5.3 Ghi chú cho Hướng B — "Cảm biến rẻ, bộ não thông minh"

Nếu chuyển sang Hướng B, có một lựa chọn kiến trúc lớn cần cân nhắc: thay vì ít phần tử độ nhạy cao (đắt, khó mua, dễ vướng kiểm soát xuất khẩu), dùng **nhiều phần tử thương mại rẻ** và bù bằng độ lợi mảng cùng xử lý thích nghi.

Luận điểm: năng lực tính toán rẻ đi theo thời gian, cảm biến độ nhạy cao thì không.

**Phản biện phải trả lời trước khi chốt:** mảng nhiều phần tử làm tăng khối lượng dữ liệu, độ phức tạp cáp, số điểm hỏng cơ khí và độ khó hiệu chuẩn — tất cả ở đầu ướt, nơi khó sửa nhất. Chi phí không biến mất, nó **dịch chuyển** từ BOM sang tích hợp và bảo trì. Cần bài toán đánh đổi định lượng, không phải khẩu hiệu.

Ghi ở đây để không mất dấu; chưa thuộc phạm vi giai đoạn 1.

---

## 6. YÊU CẦU KỸ THUẬT

### 6.1 Yêu cầu hiệu năng

| ID | Yêu cầu | Giá trị | Trạng thái | Xác minh |
|---|---|---|---|---|
| **YC-01** | Nhiễu nội tại toàn hệ thống thấp hơn tỷ số tín/tạp do thuỷ âm cảm nhận (so với nhiễu nền môi trường) **ít nhất 10 dB** | ≥ 10 dB | `[CHỐT]` | Bể tiêu âm + đo tại chỗ khi biển yên |
| YC-02 | Dải tần phân tích trực tiếp | `[THAM CHIẾU]` 3 Hz – 20 kHz | `[MỞ]` | Quét đáp ứng tần số |
| YC-03 | Dải tần phân tích bằng lọc số | `[THAM CHIẾU]` 20 – 100 kHz | `[MỞ]` | Quét đáp ứng tần số |
| YC-04 | Dải động | Từ nhiễu nền tới vật thử rất ồn | `[CHỐT]` | Đo hai đầu dải |
| YC-05 | Độ chính xác gán nhãn thời gian theo GNSS | `[THAM CHIẾU]` < 1 ms | `[MỞ]` | So với nguồn thời gian chuẩn |
| YC-06 | Chu kỳ cập nhật dữ liệu về trạm | `[THAM CHIẾU]` 1.5 s | `[MỞ]` | Đo trên đường truyền |
| YC-07 | Phân giải phân tích băng hẹp khi hậu kỳ | `[THAM CHIẾU]` 1.5 Hz | `[MỞ]` | Kiểm tra trên tín hiệu chuẩn |
| YC-08 | Độ phân giải ADC | `[THAM CHIẾU]` 16 bit | `[GIẢ ĐỊNH]` | Kiểm tra thiết kế |
| YC-09 | Tần số lấy mẫu | `[THAM CHIẾU]` 42 kS/s | `[MỞ]` | Kiểm tra thiết kế |
| YC-10 | Dải tần kênh nghe | `[THAM CHIẾU]` ~100 Hz – 3 kHz | `[GIẢ ĐỊNH]` | Nghe kiểm chứng |
| YC-11 | Thời gian hoạt động của phao trong một đợt đo | TBD | `[MỞ]` | Thử nghiệm phóng điện |
| YC-12 | Thời gian nguồn dự phòng phát tín hiệu định vị | TBD | `[MỞ]` | Thử nghiệm phóng điện |
| YC-13 | Sai số tổng của mức nguồn công bố | TBD — **cần có để đạt TC-1** | `[MỞ]` | Đo đối chứng |

> **YC-01 là ràng buộc bậc nhất.** Diễn giải: hệ thống không được là yếu tố giới hạn hiệu năng — giới hạn phải đến từ môi trường biển. Mọi lựa chọn linh kiện trong L1.2, mọi quyết định nối đất, che chắn, bố trí nguồn đều phải quy chiếu về đây. Thiết kế nào không chứng minh được YC-01 bằng **ngân sách nhiễu có số liệu** thì chưa hoàn thành.

> **YC-13 là yêu cầu bị thiếu nghiêm trọng nhất.** Toàn bộ Hướng A đứng trên khả năng công bố một con số kèm sai số. Hiện chưa ai đặt ra con số đó. Không thể thiết kế ngược từ một yêu cầu chưa tồn tại — xem QĐ-2.

### 6.2 Yêu cầu chức năng

| ID | Yêu cầu | Trạng thái |
|---|---|---|
| CN-01 | Ghi liên tục tín hiệu ra file, không mất mẫu | `[CHỐT]` |
| CN-02 | Kênh nghe thời gian thực (có thể giới hạn băng thông) | `[CHỐT]` |
| CN-03 | Phân tích dải 1/3 octave và hiển thị thời gian thực | `[CHỐT]` |
| CN-04 | Ghi nhật ký vị trí GNSS xuyên suốt, lưu trong phao | `[CHỐT]` |
| CN-05 | Tính cự ly tương đối tại trạm để chuẩn hoá dữ liệu | `[CHỐT]` |
| CN-06 | Mã hoá dữ liệu lưu trong phao **và** dữ liệu truyền vô tuyến | `[CHỐT]` |
| CN-07 | Cấu hình từ xa tham số thu nhận, từ trạm xuống phao | `[CHỐT]` |
| CN-08 | Báo cáo trạng thái và kết quả đo về trạm định kỳ | `[CHỐT]` |
| CN-09 | Nguồn dự phòng phát tín hiệu định vị phục vụ thu hồi | `[CHỐT]` |
| CN-10 | Lấy dữ liệu thô sau thu hồi qua kết nối tốc độ cao | `[CHỐT]` |
| CN-11 | Phát lại dữ liệu đã xử lý kèm tín hiệu tương tự đã ghi | `[CHỐT]` |
| CN-12 | Tự kiểm tra tích hợp (BIT), báo trạng thái sức khoẻ | `[CHỐT]` |
| CN-13 | Định vị nguồn từ nhiều phao bằng chênh lệch thời gian đến | `[GIẢ ĐỊNH]` — tuỳ chọn |
| CN-14 | Xuất dữ liệu theo giao diện chuẩn để tích hợp bên thứ ba | `[GIẢ ĐỊNH]` |

### 6.3 Yêu cầu phi chức năng

| ID | Yêu cầu | Ghi chú |
|---|---|---|
| PC-01 | Dữ liệu thô được giữ lại, không ghi đè | Dữ liệu thô là tài sản, không phải sản phẩm phụ |
| PC-02 | Hồ sơ hiệu chuẩn nhúng trong metadata của dữ liệu | Theo mục 5.2 |
| PC-03 | Triển khai được bằng đội nhỏ, thiết bị đóng thùng vận chuyển | Theo mô hình bộ kit container hoá |
| PC-04 | Tài liệu và mã nguồn quản lý phiên bản trong repo này | Theo TC-5 |

### 6.4 Bộ kit triển khai

Kế thừa mô hình đóng gói của hệ tham chiếu — toàn bộ hệ thống vận chuyển được thành các kiện độc lập:

| Kiện | Nội dung |
|---|---|
| K-1 | Phao đo |
| K-2 | Điện tử trạm điều khiển (thùng chống va đập) |
| K-3 | Cụm thuỷ âm (thùng chống va đập) |
| K-4 | Phụ kiện, cáp, đồ gá |
| K-5 | Máy tính |

### 6.5 Tình trạng trưởng thành của tập yêu cầu

| Trạng thái | Số lượng | Đánh giá |
|---|---|---|
| `[CHỐT]` | 19 | Chủ yếu là yêu cầu chức năng — phần dễ |
| `[GIẢ ĐỊNH]` | 6 | Phải chuyển thành `[CHỐT]` hoặc bị bác bỏ trước cổng G1 |
| `[MỞ]` / TBD | 10 | **Rủi ro** — hầu hết nằm ở nhóm hiệu năng |

> **Nhận định thẳng thắn cho Chief Engineer:** tập yêu cầu hiện đang mạnh ở phần *hệ thống làm gì* và yếu ở phần *hệ thống phải tốt đến mức nào*. 10/13 yêu cầu hiệu năng chưa có số của riêng ta — đang mượn số của hệ tham chiếu. Mượn số để định hướng thì được; mượn số để thiết kế thì không, vì ta không biết các con số đó được chọn theo ràng buộc nào của họ.
>
> **Ba việc phải làm trước khi bước vào thiết kế chi tiết:** chốt QĐ-0 (nhiệm vụ), đặt YC-13 (sai số công bố), hoàn thành WP-1 (ngân sách nhiễu và sai số).

---

## 7. HỆ THAM CHIẾU — TMK-SAS / SONRAS

Tài liệu gốc trong thư mục `reference/`. Dùng làm hệ quy chiếu để đối chiếu chức năng — **không phải để sao chép**.

### 7.1 Tóm tắt

Scanmatic TMK-SAS (Sound Registration and Analyzing System) là hệ thống hoàn chỉnh để thu nhận, lưu trữ và phân tích âm thanh dưới nước. Gồm hai phần chính: một **phao** định vị GPS chạy ắc quy, chứa toàn bộ điện tử để khuếch đại tín hiệu từ thuỷ âm, chuyển đổi A/D, lưu trữ, phân tích theo dải tần và truyền dữ liệu về trạm; và một **trạm điều khiển (SCS)** nền PC định vị GPS đặt trên tàu hoặc công trình biển, để cấu hình phao và hiển thị dữ liệu nhận được.

### 7.2 Thông số công bố

| Hạng mục | Giá trị |
|---|---|
| Lấy mẫu liên tục | 42 kS/s |
| Dải phân tích trực tiếp | 3 Hz – 20 kHz |
| Dải phân tích bằng lọc số | 20 kHz – 100 kHz |
| Chu kỳ cập nhật về trạm | 1.5 s, dữ liệu 1/3 octave toàn dải 3 Hz – 100 kHz |
| ADC | 16 bit |
| Chuỗi analog | Tiền khuếch đại thuỷ âm nhiễu thấp + khuếch đại điều khiển được |
| Dải động | Từ mức nhiễu nền tới vật thử rất ồn |
| Gán nhãn thời gian sự kiện (tuỳ chọn) | Tốt hơn 1 ms theo giờ GPS |
| Kênh nghe | Vô tuyến VHF, ~100 Hz – 3 kHz |
| Đường dữ liệu | Modem UHF |
| Lưu trữ trong phao | CompactFlash, dữ liệu thô + đã xử lý |
| Lấy dữ liệu sau nhiệm vụ | Ethernet, sau khi thu hồi phao |
| Phân giải băng hẹp khi hậu kỳ | 1.5 Hz, dải 3 Hz – 20 kHz |
| Mã hoá | Cả dữ liệu lưu lẫn dữ liệu truyền |
| Định vị | GPS ở cả phao và trạm; lưu vệt vị trí trong phao |

### 7.3 Nhóm ứng dụng công bố

- Lập bản đồ chữ ký tiếng ồn cho tàu và công trình biển
  - Tàu quét mìn — để bảo đảm cự ly an toàn
  - Tàu cá — trong quá trình dò tìm và đánh bắt
  - Công trình đáy biển — để phát hiện hư hỏng
- Tiếng ồn sinh học trong đại dương, gồm âm thanh động vật có vú
- Xác định nguồn sóng xung kích

### 7.4 Đối chiếu với HW-SNR-26

| Khía cạnh | TMK-SAS | HW-SNR-26 | Nhận xét |
|---|---|---|---|
| Nhiệm vụ | Đo chữ ký âm học | `[MỞ]` — xem QĐ-0 | **Khác biệt cần chốt trước tiên** |
| Kiến trúc | Phao + trạm | Phao + trạm (Hướng A) | Kế thừa trực tiếp |
| Số phần tử thu | 1 thuỷ âm | 1 (Hướng A) / mảng (Hướng B) | Phụ thuộc QĐ-0 |
| Beamforming | Không | Không (A) / Có (B) | Phụ thuộc QĐ-0 |
| Xử lý trong phao | Phân tích dải tần | Tương đương | Kế thừa |
| Xử lý hậu kỳ | FFT băng hẹp 1.5 Hz | Tương đương hoặc tốt hơn | Kế thừa |
| Chuẩn hoá cự ly | Có, từ vệt GPS | Có (CN-05) | Kế thừa — cơ chế cốt lõi |
| Truy xuất chuẩn đo lường | Không nêu rõ | **Có (TC-4)** | **Điểm ta chủ động làm hơn** |
| Mã hoá | Có | Có (CN-06) | Ngang bằng |
| Định vị đa phao | Tuỳ chọn | Tuỳ chọn (CN-13) | Kế thừa |
| Đóng gói | Kit container hoá | Kit container hoá (PC-03) | Kế thừa |

**Ba nhận xét rút ra:**

1. **Kiến trúc hai phân hệ đã được kiểm chứng thương mại — không cần sáng tạo lại.** Việc chia luồng dữ liệu (kết quả đã xử lý đi vô tuyến thời gian thực; dữ liệu thô lấy sau khi thu hồi) là cách giải quyết đúng cho ràng buộc băng thông, và nên áp dụng thẳng.

2. **Điểm ta có thể làm hơn là tính truy xuất chuẩn đo lường.** Tài liệu tham chiếu không nêu chuỗi truy xuất. Với bối cảnh trong nước — nơi kết quả đo cần dùng để nghiệm thu và ra quyết định an toàn — đây vừa là yêu cầu bắt buộc, vừa là chỗ tạo giá trị giữ lại cao nhất cho đơn vị. Xem TC-4, mục 5.2.

3. **Con số 1 ms phải được hiểu đúng.** Nó không phải một thông số marketing mà là trần độ chính xác định vị: 1 ms ≈ 1.5 m cự ly. Mọi yêu cầu định vị chặt hơn đều quy về việc siết đồng bộ thời gian.

---

## 8. LỘ TRÌNH VÀ CỔNG KIỂM SOÁT

| GĐ | Tên | Mục tiêu | Cổng thoát |
|---|---|---|---|
| **G1** | Đồng thuận & Đặc tả | Chốt QĐ-0, đóng các mục `[MỞ]` bậc nhất, lập ngân sách nhiễu và ngân sách sai số | Đặc tả được Chief Engineer duyệt; QĐ-0 đã chốt; YC-13 có số |
| **G2** | Chứng minh | Nguyên mẫu chuỗi thu + chuỗi xử lý; kiểm chứng YC-01 trong phòng thí nghiệm; hiệu chuẩn truy xuất được | YC-01 đạt trên bàn thí nghiệm; chuỗi xử lý chạy trên dữ liệu ghi sẵn; có hồ sơ hiệu chuẩn |
| **G3** | Thử nghiệm biển | Triển khai thực địa, đo tàu thật, đo đối chứng | Đo được chữ ký một tàu thật, sai số nằm trong YC-13 |
| **G4** | Đóng gói & Nhân bản | Chuẩn hoá tài liệu, quy trình, chứng minh TC-3 trên cấu hình thứ hai | Hệ thứ hai chạy trên cùng lõi xử lý |

**Nguyên tắc cổng:** không giải ngân nguồn lực giai đoạn sau khi cổng giai đoạn trước chưa đóng.

**Về tiến độ:** chưa lập được mốc thời gian đáng tin cho tới khi QĐ-0 được chốt và WP-1 hoàn thành. Không nên cam kết tiến độ với bên ngoài trước thời điểm đó.

---

## 9. GÓI CÔNG VIỆC KHỞI ĐỘNG

Dành cho kỹ sư bắt đầu từ hôm nay. WP-1 và WP-2 chạy song song được và **không** bị chặn bởi QĐ-0.

### WP-0 — Chốt nhiệm vụ chính `CHẶN MỌI VIỆC KHÁC`
**Người thực hiện:** Chief Engineer, không phải kỹ sư.
**Đầu ra:** quyết định Hướng A hay Hướng B, ghi vào mục 11 và cập nhật tài liệu này lên v1.0.

### WP-1 — Ngân sách nhiễu và ngân sách sai số `ƯU TIÊN CAO NHẤT`
**Đầu ra:** hai bảng tính có số liệu.
- **Ngân sách nhiễu:** từ nhiễu nhiệt phần tử → preamp → ADC → nhiễu lượng tử hoá, chứng minh YC-01 khả thi hoặc chỉ ra khối nào không đạt.
- **Ngân sách sai số:** hiệu chuẩn thuỷ âm + sai số cự ly GNSS + sai số mô hình truyền âm → sai số tổng của mức nguồn công bố. Đây là căn cứ để đặt YC-13.

**Vì sao ưu tiên:** hai bảng này quyết định kiến trúc. Làm sau khi đã chọn linh kiện thì đã muộn. Phần lớn nội dung **không** phụ thuộc QĐ-0 — chuỗi analog và bài toán nhiễu giống nhau ở cả hai hướng.

### WP-2 — Đặc tả định dạng dữ liệu và khung xử lý ngoại tuyến
**Đầu ra:** đặc tả định dạng file dữ liệu thô (metadata bắt buộc: hệ số hiệu chuẩn, tần số lấy mẫu, nhãn thời gian, vệt GNSS, cấu hình cảm biến) + khung chương trình đọc file và chạy một chuỗi xử lý tối thiểu.

**Vì sao sớm:** theo nguyên tắc 3 mục 5.1, toàn bộ phát triển thuật toán về sau phụ thuộc vào khung này. Làm ngay được với dữ liệu mô phỏng, không cần chờ phần cứng và không cần chờ QĐ-0.

### WP-3 — Khảo sát chuỗi hiệu chuẩn thuỷ âm
**Đầu ra:** phương án bảo đảm tính truy xuất chuẩn cho phép đo — hiệu chuẩn ở đâu, theo phương pháp nào, chu kỳ bao lâu, chi phí bao nhiêu, ai công nhận kết quả.

**Vì sao quan trọng:** TC-4 là điểm phân biệt sản phẩm. Nếu không có đường truy xuất khả thi, phải biết điều đó **trước** khi thiết kế chứ không phải sau khi đã đo xong.

### WP-4 — Nghiên cứu đường truyền và ngân sách điện năng
**Đầu ra:** phương án cho kênh dữ liệu và kênh nghe (băng thông, cự ly làm việc, công suất tiêu thụ, cấp phép tần số) + ngân sách điện năng của phao dẫn tới số cho YC-11, YC-12.

**Lưu ý:** nếu QĐ-0 chốt Hướng B (đặt đáy), gói này đổi bản chất hoàn toàn — vô tuyến không dùng được dưới nước, phải chọn giữa cáp lên phao mặt nước, cáp vào bờ, hoặc modem thuỷ âm băng hẹp. Đây là lý do WP-4 xếp sau WP-1 và WP-2.

---

## 10. RỦI RO KỸ THUẬT

| ID | Rủi ro | Ảnh hưởng | Hướng xử lý |
|---|---|---|---|
| RR-01 | QĐ-0 chưa chốt, đội bắt đầu thiết kế theo giả định khác nhau | Lãng phí công sức, thiết kế không khớp | WP-0, ưu tiên tuyệt đối |
| RR-02 | Không đạt YC-01 do nhiễu nội tại | Hỏng luận điểm hiệu năng | WP-1 phát hiện sớm; dự phòng: cải thiện che chắn, nối đất, tách nguồn |
| RR-03 | Không có đường hiệu chuẩn truy xuất được | Mất TC-4, mất điểm phân biệt sản phẩm | WP-3 |
| RR-04 | Sai số mô hình truyền âm ở nước nông lớn hơn dự kiến | Sai số mức nguồn vượt YC-13 | Đo profile tốc độ âm tại chỗ; đo đối chứng nhiều cự ly |
| RR-05 | Nguồn cung gốm áp điện hạn chế hoặc vướng kiểm soát xuất khẩu | Chặn L1.1 | Xác định tối thiểu hai nhà cung cấp ở hai khu vực pháp lý trước G1 |
| RR-06 | Cấp phép tần số vô tuyến cho kênh dữ liệu và kênh nghe | Chặn L1.7 | Làm rõ thủ tục trong WP-4 |
| RR-07 | Chuỗi xử lý bị gắn chặt vào một cấu hình phần cứng | Mất TC-3, mất giá trị nền tảng | Thực thi nghiêm nguyên tắc mục 5.1; kiểm thử trên dữ liệu cấu hình thứ hai từ sớm |
| RR-08 | Nhiễu nền môi trường cao hơn mức thiết kế (giao thông tàu, gió mùa) | Giảm dải đo hữu ích | Đo nhiễu nền thực tế sớm (pha P3); có thể phải chọn cửa sổ thời gian đo |
| RR-09 | Mất phao khi thu hồi | Mất thiết bị và toàn bộ dữ liệu thô | CN-09 nguồn dự phòng định vị; cân nhắc truyền trước một phần dữ liệu quan trọng |

---

## 11. DANH MỤC QUYẾT ĐỊNH CÒN MỞ

**Đây là phần quan trọng nhất của tài liệu đối với Chief Engineer.** Không kỹ sư nào được tự quyết các mục dưới đây.

| # | Quyết định | Chặn việc gì | Đề xuất |
|---|---|---|---|
| **QĐ-0** | **Nhiệm vụ chính: Hướng A (đo chữ ký) hay Hướng B (giám sát)?** | **Toàn bộ thiết kế** | **Hướng A trước — xem lập luận mục 1** |
| QĐ-1 | Dải tần công tác (YC-02, YC-03) | Chọn thuỷ âm, ADC, tần số lấy mẫu | Bám hệ tham chiếu trừ khi có lý do khác |
| QĐ-2 | Sai số tổng của mức nguồn công bố (YC-13) | Toàn bộ ngân sách sai số, tiêu chí nghiệm thu G3 | Chờ WP-1; **phải có trước G1** |
| QĐ-3 | Có làm chế độ định vị đa phao (CN-13) không | Số phao cần chế tạo, yêu cầu đồng bộ YC-05 | Quyết định theo nhu cầu ứng dụng UD-6 |
| QĐ-4 | Phương án đường truyền | L1.7, kiến trúc SS-B | Chờ WP-4; phụ thuộc QĐ-0 |
| QĐ-5 | Thời gian hoạt động mục tiêu (YC-11, YC-12) | Thiết kế nguồn L1.9, kết cấu phao L1.8 | Cần đầu vào từ khái niệm vận hành thực tế |
| QĐ-6 | Chu kỳ và phương pháp hiệu chuẩn | L1.13, chi phí vận hành | Chờ WP-3 |
| QĐ-7 | Độ phân giải ADC — 16 bit có đủ cho dải động yêu cầu không | L1.3 | Kiểm chứng bằng ngân sách nhiễu WP-1 |
| QĐ-8 | Trạng thái biển và điều kiện môi trường thiết kế | L1.8, kế hoạch thử nghiệm | Cần dữ liệu khảo sát vùng biển mục tiêu |

---

## 12. THUẬT NGỮ

| Viết tắt | Tiếng Anh | Giải thích |
|---|---|---|
| ADC | Analog-to-Digital Converter | Bộ chuyển đổi tương tự – số |
| BIT | Built-In Test | Tự kiểm tra tích hợp |
| CONOPS | Concept of Operations | Khái niệm vận hành |
| DEMON | Detection of Envelope Modulation On Noise | Giải điều chế đường bao — trích tần số quay chân vịt |
| FFT | Fast Fourier Transform | Biến đổi Fourier nhanh |
| GNSS | Global Navigation Satellite System | Hệ thống định vị vệ tinh (GPS là một hệ trong đó) |
| HMI | Human-Machine Interface | Giao diện người – máy |
| PZT | Lead Zirconate Titanate | Gốm áp điện |
| SCS | Ship Control Station | Trạm điều khiển đặt trên tàu |
| SVP | Sound Velocity Profile | Profile tốc độ âm theo độ sâu |
| UHF / VHF | Ultra / Very High Frequency | Dải tần vô tuyến |
| 1/3 octave | One-third octave band | Cách chia dải tần theo tỷ lệ, chuẩn trong đo tiếng ồn |
| Beamforming | — | Tạo búp sóng — tổ hợp tín hiệu nhiều phần tử để định hướng |
| Biofouling | — | Hà bám, sinh vật bám vào bề mặt ngâm nước |
| Mức nguồn | Source level | Mức âm quy về cự ly chuẩn 1 m, dB re 1µPa @1m |
| Multilateration | — | Định vị bằng chênh lệch thời gian đến tại nhiều trạm |
| Truy xuất chuẩn | Metrological traceability | Chuỗi so sánh liên tục về chuẩn đo lường |

---

## 13. LỊCH SỬ SỬA ĐỔI

| Phiên bản | Ngày | Nội dung |
|---|---|---|
| 0.1 | 2026-08-25 | Bản thảo đầu, giả định nhiệm vụ giám sát thuỷ âm đa tĩnh |
| 0.2 | 2026-08-25 | Viết lại sau khi có tài liệu tham chiếu TMK-SAS. Phát hiện tài liệu tham chiếu mô tả nhiệm vụ **đo chữ ký âm học**, khác với giả định ban đầu — nâng thành QĐ-0 và tái cấu trúc toàn bộ theo Hướng A. Bổ sung thông số hệ tham chiếu, cơ chế chuẩn hoá cự ly, 4 chế độ vận hành trạm, yêu cầu truy xuất chuẩn đo lường |

---

## 14. PHẠM VI TÀI LIỆU

Tài liệu này cố ý **chỉ chứa nội dung kỹ thuật**. Các nội dung sau **không** thuộc phạm vi và được quản lý riêng theo kênh có kiểm soát truy cập:

- Điều khoản thương mại, giá, cấu trúc chi phí, phương án tài chính
- Danh tính đối tác, cấu trúc pháp lý, vấn đề kiểm soát xuất khẩu
- Địa điểm triển khai cụ thể và dữ liệu khảo sát thực địa
- Thông tin khách hàng, tiến trình phê duyệt, quan hệ đối tác

Kỹ sư cần các thông tin trên để ra quyết định kỹ thuật, hãy đề nghị qua Chief Engineer thay vì đưa vào tài liệu này hoặc vào repo.

---

## PHỤ LỤC — TÀI LIỆU THAM CHIẾU TRONG REPO

| File | Nội dung |
|---|---|
| `reference/6. Catalog TMK-SAS.pdf` | Catalog hệ tham chiếu Scanmatic TMK-SAS — thông số, ứng dụng, chế độ vận hành |
| `reference/con-of diagram.jpg` | Sơ đồ tổng thể hệ tham chiếu: phao + trạm điều khiển + GNSS |
