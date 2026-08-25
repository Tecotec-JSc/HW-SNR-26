# HW-SNR-26 — ĐIỀU LỆ DỰ ÁN (PROJECT CHARTER)

**Phao Giám sát Thuỷ âm Thụ động — Vòng xoắn 1: Proof of Concept**
*Passive Acoustic Monitoring Buoy — Spiral 1: Proof of Concept*

| | |
|---|---|
| **Mã chương trình** | HW-SNR-26 |
| **Phiên bản** | 0.3 |
| **Ngày** | 2026-08-25 |
| **Trạng thái** | Bản thảo — chờ Chief Engineer phê duyệt |
| **Người đọc đầu tiên** | Chief Engineer |
| **Đối tượng** | Kỹ sư tham gia dự án từ ngày đầu tiên |
| **Mô hình phát triển** | Xoắn ốc (spiral), dẫn dắt bởi rủi ro |
| **Phạm vi tài liệu này** | **Vòng xoắn 1 — PoC.** Vòng 2 trở đi chỉ phác ở mục 8 |
| **Kiến trúc tham chiếu** | Scanmatic TMK-SAS / SONRAS — `reference/` |

---

## 0. CÁCH ĐỌC TÀI LIỆU NÀY

Đây là điều lệ cho **một vòng xoắn PoC**, không phải cho cả chương trình. Tài liệu cố ý
giữ ngắn: PoC mà cần đọc một tập tài liệu dày mới bắt đầu được thì đã đi sai hướng.

Bốn câu hỏi tài liệu này trả lời:

1. PoC này chứng minh điều gì, và **không** chứng minh điều gì? → Mục 2, 3
2. Hệ thống gồm những gì? → Mục 5
3. Rủi ro lớn nhất là gì, và vòng xoắn này gỡ rủi ro nào? → Mục 4, 7
4. Tôi bắt đầu từ đâu? → Mục 9

**Quy ước:**

| Ký hiệu | Ý nghĩa |
|---|---|
| `[CHỐT]` | Đã quyết định |
| `[GIẢ ĐỊNH]` | Cơ sở thiết kế nhưng **chưa xác minh** |
| `[MỞ]` | Chưa quyết định — báo Chief Engineer, không tự chọn |
| `[THAM CHIẾU]` | Số của hệ tham chiếu TMK-SAS — **không phải** yêu cầu của ta |
| `[V2+]` | Cố ý đẩy sang vòng xoắn sau |

---

## 1. NHIỆM VỤ — ĐÃ CHỐT

> ### ✅ QĐ-0 ĐÃ ĐÓNG (2026-08-25)
>
> **Sản phẩm là một phao giám sát thuỷ âm thụ động.**
>
> Thả xuống biển, **nghe liên tục**, phát hiện – ghi – báo cáo hoạt động âm học trong khu vực.
> **Thụ động** nghĩa là chỉ nghe, không phát bất kỳ tín hiệu âm nào.
>
> Các phương án từng cân nhắc và nay đã loại khỏi phạm vi: hệ **đo chữ ký âm học** (Hướng A cũ)
> và hệ **mảng đặt đáy giám sát dài ngày** (Hướng B cũ). Xem `requirements/26-waiting-room.md`.

### 1.1 SONRAS đóng vai trò gì — và không đóng vai trò gì

Đây là điểm dễ hiểu nhầm nhất trong cả dự án. Cần nói rõ ngay:

| | SONRAS / TMK-SAS | HW-SNR-26 Vòng 1 |
|---|---|---|
| **Nhiệm vụ** | **Đo** chữ ký âm học của tàu mình | **Giám sát** — nghe xem có gì trong khu vực |
| Mục tiêu | Hợp tác, biết trước, chạy theo lộ trình định sẵn | **Không hợp tác, không biết trước** |
| Cự ly tới mục tiêu | Biết (tính từ GPS hai đầu) | **Không biết** |
| Sản phẩm đầu ra | Mức nguồn đã chuẩn hoá về 1 m | **Mức thu được + danh sách phát hiện** |
| Thời gian triển khai | Vài giờ, chọn điều kiện thời tiết | Dài hơn, **không chọn được điều kiện** |

**Ta mượn của SONRAS: kiến trúc.** Phao tự hành + trạm điều khiển trên tàu, GNSS hai đầu,
đường vô tuyến cho dữ liệu đã xử lý, dữ liệu thô lấy sau khi thu hồi, đóng gói thành bộ kit.
Kiến trúc này đã được kiểm chứng thương mại — không cần sáng tạo lại.

**Ta không mượn của SONRAS: nhiệm vụ và phép đo.** Cụ thể, **bước chuẩn hoá theo cự ly
không áp dụng được** cho ta: nó cần biết vị trí mục tiêu, mà giám sát thụ động thì không biết.
Mọi chỗ trong tài liệu trước đây nói về "mức nguồn" và "chuẩn hoá cự ly" đều đã bị loại bỏ.

> **Hệ quả trực tiếp:** sản phẩm đầu ra của ta là **mức áp suất âm thu được** (received level)
> kèm **danh sách sự kiện phát hiện được**. Không phải mức nguồn. Muốn ra mức nguồn phải biết
> cự ly — việc đó cần nhiều phao hoặc mảng định hướng, đã đẩy sang `[V2+]`.

---

## 2. MỤC TIÊU VÒNG XOẮN 1

### 2.1 Tuyên bố

> Chứng minh rằng đơn vị có thể chế tạo một phao thu thuỷ âm thụ động **đủ yên tĩnh** để
> nghe được tín hiệu quan tâm trong điều kiện biển thật, ghi lại được, và đưa dữ liệu về
> phân tích được.

Chữ then chốt là **đủ yên tĩnh**. Đó là nội dung của rủi ro số 1 (mục 4).

### 2.2 Bốn điều PoC phải chứng minh

| # | Mệnh đề cần chứng minh | Đạt khi nào |
|---|---|---|
| **PoC-1** | Chuỗi thu có nền nhiễu nội tại thấp hơn nhiễu môi trường biển | Đo nền nhiễu tại chỗ, chênh lệch đạt ngưỡng YC-01 |
| **PoC-2** | Phao giữ được thuỷ âm đủ yên tĩnh trong điều kiện biển thật | Nền nhiễu khi thả trôi không cao hơn đáng kể so với đo tĩnh |
| **PoC-3** | Hệ thống phát hiện được mục tiêu thật trong dữ liệu thật | Phát hiện tàu đi qua, đối chiếu được với quan sát mắt/AIS |
| **PoC-4** | Dữ liệu lấy về được và phân tích được | Trích xuất trọn vẹn sau thu hồi, chạy được chuỗi phân tích |

**PoC-2 là mệnh đề khó nhất và là lý do tồn tại của vòng xoắn này.** Xem mục 4.

### 2.3 Ngoài phạm vi vòng xoắn 1 — danh sách loại trừ tường minh

Đây là phần quan trọng nhất của một điều lệ PoC. Mọi mục dưới đây **cố ý không làm**:

| Hạng mục | Vì sao hoãn | Vòng |
|---|---|---|
| Định hướng nguồn âm (bearing / DOA) | Cần mảng nhiều phần tử; PoC dùng 1 thuỷ âm | `[V2+]` |
| Định vị nguồn (nhiều phao) | Cần nhiều phao và đồng bộ chặt | `[V2+]` |
| Phân loại mục tiêu tự động | Cần dữ liệu huấn luyện — mà dữ liệu đó chính là sản phẩm của vòng 1 | `[V2+]` |
| Truy xuất chuẩn đo lường đầy đủ | PoC chỉ cần hiệu chuẩn đủ để báo mức tin cậy được | `[V2+]` |
| Trực canh dài ngày (tuần/tháng) | Bài toán nguồn điện và hà bám, tách riêng | `[V2+]` |
| Mã hoá cấp quốc phòng | Không phải rủi ro kỹ thuật cần gỡ ở vòng 1 | `[V2+]` |
| Sản phẩm hoá, vỏ đẹp, tài liệu người dùng | PoC không bàn giao cho người dùng cuối | `[V2+]` |
| Chịu đựng trạng thái biển khắc nghiệt | Vòng 1 chọn cửa sổ thời tiết | `[V2+]` |
| Nguồn phát chủ động | Trái định nghĩa "thụ động" | ✖ ngoài chương trình |

> **Quy tắc cho cả đội:** ai muốn thêm một hạng mục vào vòng 1 phải trả lời được:
> *"nó gỡ rủi ro nào trong bốn mệnh đề PoC?"* Không trả lời được thì đưa vào phòng chờ
> (`requirements/26-waiting-room.md`), không đưa vào vòng 1.

---

## 3. TIÊU CHÍ THÀNH CÔNG VÀ TIÊU CHÍ DỪNG

### 3.1 Thành công

Vòng xoắn 1 thành công khi **cả bốn** mệnh đề PoC-1…PoC-4 được chứng minh bằng dữ liệu
thu ngoài biển thật, không phải bằng mô phỏng hay đo trong bể.

### 3.2 Tiêu chí dừng (kill criteria)

Một PoC trung thực phải định nghĩa trước điều kiện thất bại. Dừng và báo cáo lại nếu:

| # | Điều kiện dừng |
|---|---|
| D-1 | Nền nhiễu nội tại không đạt YC-01 sau hai vòng cải tiến thiết kế |
| D-2 | Nhiễu do chuyển động phao không giảm được xuống dưới nhiễu môi trường, sau khi đã thử ít nhất hai phương án giảm chấn |
| D-3 | Chi phí hoặc thời gian vượt ngưỡng đã duyệt mà chưa đạt PoC-1 và PoC-2 |

> Dừng đúng lúc ở D-2 rẻ hơn nhiều so với phát hiện ra vấn đề đó ở vòng 3, khi đã đầu tư
> vào mảng, vào nguồn dài ngày và vào vỏ sản phẩm.

---

## 4. RỦI RO CHI PHỐI — NHIỄU DO NỀN TẢNG, KHÔNG PHẢI NHIỄU ĐIỆN TỬ

Mục này viết riêng vì nó là lý do chọn phạm vi vòng 1 như trên.

### 4.1 Vấn đề

Một phao nổi trên biển **tự sinh ra tiếng ồn** ngay tại chỗ đặt thuỷ âm:

```
   Sóng, gió, dòng chảy
          │
          ├──► Phao nhấp nhô  ──► cáp thuỷ âm giật, rung
          │                              │
          ├──► Dòng chảy qua thuỷ âm ──► nhiễu dòng chảy (flow noise)
          │
          ├──► Dây neo rung  ──► nhiễu rung dây (strum) truyền xuống
          │
          └──► Cáp uốn/rung  ──► nhiễu ma sát điện (triboelectric)
                                          │
                                          ▼
                        Tất cả tập trung ở DẢI TẦN THẤP
                        — đúng dải quan tâm nhất khi phát hiện tàu
```

### 4.2 Vì sao SONRAS không giúp ta ở điểm này

SONRAS là phao **đo**: triển khai vài giờ, **chọn được** ngày biển lặng, mục tiêu chạy qua
theo lịch. Phao **giám sát** thì ngồi đó và nghe — không chọn được điều kiện, và thời gian
tiếp xúc với sóng gió dài hơn nhiều.

Tài liệu tham chiếu vì thế **không** bàn tới bài toán này. Đây là phần ta phải tự giải, và
là lý do PoC-2 tồn tại như một mệnh đề riêng.

### 4.3 Hệ quả thiết kế

| Nguyên tắc | Diễn giải |
|---|---|
| **Tách cơ học thuỷ âm khỏi phao** | Treo mềm, không gắn cứng. Đây là hạng mục thiết kế bậc nhất, không phải phụ kiện |
| **Cáp phải là loại nhiễu thấp chuyên dụng** | Cáp đồng trục thường sẽ hỏng phép đo ở dải thấp |
| **Đo nền nhiễu ở cả hai trạng thái** | Đo tĩnh (thả từ tàu neo, biển lặng) và đo thật (phao trôi/neo). Chênh lệch giữa hai số chính là thước đo của PoC-2 |
| **Thiết kế cho phép thử nhiều phương án treo** | Vòng 1 phải thử được ít nhất hai cấu hình giảm chấn khác nhau |

> **Nhận định cho Chief Engineer:** nếu chỉ có ngân sách để gỡ **một** rủi ro trong vòng 1,
> hãy gỡ rủi ro này. Nhiễu điện tử là bài toán đã biết cách giải và giải được trên bàn.
> Nhiễu nền tảng chỉ lộ ra khi xuống nước, và nó quyết định hệ thống có dùng được hay không.

---

## 5. HỆ THỐNG VÒNG XOẮN 1

### 5.1 Sơ đồ tổng thể

```
                        ┌─────────────┐
                        │  VỆ TINH    │
                        │    GNSS     │
                        └──────┬──────┘
            ┌──────────────────┴──────────────────┐
            ▼                                     ▼
┌───────────────────────────┐        ┌────────────────────────────┐
│      PHAO GIÁM SÁT        │        │   TRẠM ĐIỀU KHIỂN (trên    │
│         (SS-A)            │        │   tàu hoặc trên bờ)  (SS-B)│
│                           │        │                            │
│  ┌─────────────────────┐  │        │  ┌──────────────────────┐  │
│  │ GNSS + đồng bộ giờ  │  │        │  │ GNSS                 │  │
│  └─────────────────────┘  │        │  └──────────────────────┘  │
│  ┌─────────────────────┐  │◄──────►│  ┌──────────────────────┐  │
│  │ Vô tuyến            │  │  UHF   │  │ Vô tuyến             │  │
│  └─────────────────────┘  │        │  └──────────────────────┘  │
│  ┌─────────────────────┐  │        │  ┌──────────────────────┐  │
│  │ Xử lý + ghi         │  │        │  │ Máy tính + phần mềm  │  │
│  │  ├ phân tích phổ    │  │        │  │  ├ hiển thị phổ      │  │
│  │  ├ phát hiện sự kiện│  │        │  │  ├ danh sách sự kiện │  │
│  │  └ ghi dữ liệu thô  │  │        │  │  └ phân tích hậu kỳ  │  │
│  └──────────▲──────────┘  │        │  └──────────────────────┘  │
│  ┌──────────┴──────────┐  │        └────────────────────────────┘
│  │ Analog front-end    │  │                     ▲
│  └──────────▲──────────┘  │                     │
│             │             │        Ethernet sau khi thu hồi phao
│  ┌──────────┴──────────┐  │        (lấy toàn bộ dữ liệu thô)
│  │ ✱ TREO GIẢM CHẤN ✱  │  │──────────────────────┘
│  │   + cáp nhiễu thấp  │  │
│  └──────────▲──────────┘  │        ✱ = hạng mục rủi ro cao nhất
│        ┌────┴────┐        │            của vòng xoắn 1
│        │ Thuỷ âm │        │
│        └─────────┘        │
│  ┌─────────────────────┐  │
│  │ Nguồn + kết cấu     │  │
│  └─────────────────────┘  │
└───────────────────────────┘
```

### 5.2 Phân rã phân hệ

| Mã | Phân hệ | Vòng 1 làm gì | Rủi ro |
|---|---|---|---|
| A1 | Cụm thuỷ âm + **treo giảm chấn** | Mua thuỷ âm; **tự thiết kế cơ cấu treo** | 🔴 cao |
| A2 | Analog front-end | Tiền KĐ nhiễu thấp + PGA + lọc | 🟡 |
| A3 | Số hoá | ADC + đồng bộ giờ GNSS | 🟢 |
| A4 | Xử lý + phát hiện | Phân tích phổ, phát hiện sự kiện đơn giản | 🟡 |
| A5 | Lưu trữ | Ghi liên tục dữ liệu thô | 🟢 |
| A6 | GNSS | Vị trí + nhãn thời gian | 🟢 |
| A7 | Vô tuyến | Một kênh dữ liệu; **không** kênh nghe ở vòng 1 | 🟡 |
| A8 | Kết cấu phao + neo | Thân phao, độ nổi, neo/thả/thu hồi | 🔴 cao |
| A9 | Nguồn | Đủ cho một đợt triển khai vòng 1 | 🟢 |
| B | Trạm điều khiển | Máy tính + phần mềm hiển thị và phân tích | 🟡 |
| C | Lõi xử lý + hiệu chuẩn | Thư viện phân tích, định dạng dữ liệu, hiệu chuẩn cơ bản | 🟡 |

> **A1 và A8 phải được thiết kế cùng nhau, bởi cùng một người hoặc cùng một nhóm.**
> Chúng cùng quyết định PoC-2. Tách hai việc này cho hai nhóm độc lập là sai lầm về tổ chức.

### 5.3 Phép đo của hệ giám sát thụ động

```
   RL = V_ADC(dB)  −  M  −  G  +  K            [dB re 1 µPa]

   RL      Mức áp suất âm THU ĐƯỢC tại thuỷ âm   ← SẢN PHẨM ĐẦU RA
   V_ADC   Mức tín hiệu số đọc từ ADC
   M       Độ nhạy thuỷ âm      [dB re 1V/µPa]   ← từ hồ sơ hiệu chuẩn
   G       Tổng độ lợi chuỗi analog [dB]         ← từ metadata, PHẢI ghi lại
   K       Hệ số hiệu chuẩn hệ thống [dB]        ← từ hồ sơ hiệu chuẩn
```

**So với SONRAS: không có số hạng suy hao truyền âm, không quy về 1 m.** Ta không biết cự ly
tới mục tiêu nên không tính được mức nguồn. Sản phẩm là mức thu được, kèm danh sách sự kiện.

---

## 6. YÊU CẦU VÒNG XOẮN 1

Chỉ liệt kê yêu cầu **cần cho PoC**. Bản đầy đủ theo Volere: `requirements/`.

### 6.1 Hiệu năng

| ID | Yêu cầu | Giá trị | Trạng thái |
|---|---|---|---|
| **YC-01** | Nhiễu nội tại chuỗi điện tử thấp hơn nhiễu môi trường biển ít nhất 10 dB | ≥ 10 dB | `[CHỐT]` |
| **YC-02** | Nhiễu tổng khi phao hoạt động thật không vượt nhiễu môi trường quá ngưỡng quy định | TBD | `[MỞ]` ⭐ **định nghĩa PoC-2** |
| YC-03 | Dải tần công tác | TBD | `[MỞ]` → QĐ-1 |
| YC-04 | Dải động phủ từ nhiễu nền tới nguồn gần | Bắt buộc | `[CHỐT]` |
| YC-05 | Độ phân giải ADC | 16 bit | `[GIẢ ĐỊNH]` |
| YC-06 | Thời gian triển khai một đợt vòng 1 | TBD | `[MỞ]` → QĐ-5 |
| YC-07 | Sai số của mức thu được công bố | TBD | `[MỞ]` → QĐ-2 |

> ⚠ **YC-02 là yêu cầu quan trọng nhất và hiện chưa có số.** Nó là phát biểu định lượng của
> PoC-2. Không có nó thì không biết PoC thành công hay thất bại. **Phải đặt số trước G1.**

### 6.2 Chức năng

| ID | Yêu cầu | Trạng thái |
|---|---|---|
| CN-01 | Ghi liên tục tín hiệu thuỷ âm ra file, không mất mẫu | `[CHỐT]` |
| CN-02 | Phân tích phổ và hiển thị theo thời gian thực tại trạm | `[CHỐT]` |
| CN-03 | Phát hiện sự kiện âm học vượt ngưỡng và ghi vào danh sách | `[CHỐT]` |
| CN-04 | Ghi nhật ký vị trí GNSS, lưu trong phao | `[CHỐT]` |
| CN-05 | Báo cáo trạng thái và kết quả về trạm theo chu kỳ | `[CHỐT]` |
| CN-06 | Trích xuất toàn bộ dữ liệu thô sau khi thu hồi | `[CHỐT]` |
| CN-07 | Nguồn dự phòng phát tín hiệu định vị phục vụ thu hồi | `[CHỐT]` |
| CN-08 | Tự kiểm tra và báo trạng thái sức khoẻ | `[CHỐT]` |
| CN-09 | Ghi lại mọi thay đổi độ lợi kèm nhãn thời gian | `[CHỐT]` |
| CN-10 | Mỗi file dữ liệu thô tự mô tả đủ để quy ra mức thu được | `[CHỐT]` |

**Đã bỏ khỏi vòng 1 so với phiên bản trước:** kênh nghe tương tự, mã hoá, cấu hình từ xa,
phát lại đồng bộ, định vị đa phao, xuất chuẩn cho bên thứ ba, chuẩn hoá theo cự ly.

> Việc bỏ kênh nghe tương tự cũng **giải luôn xung đột** giữa yêu cầu kênh nghe và yêu cầu
> mã hoá đã ghi nhận ở phiên bản trước (QĐ-11 — nay đóng).

---

## 7. RỦI RO VÒNG XOẮN 1

| ID | Rủi ro | Mức | Gỡ bằng cách nào trong vòng 1 |
|---|---|---|---|
| **RR-01** | **Nhiễu do chuyển động phao / dòng chảy / dây neo** che mất tín hiệu | 🔴 | Thiết kế treo giảm chấn; thử ≥2 phương án; đo đối chứng tĩnh–động |
| **RR-02** | Nhiễu nội tại điện tử không đạt YC-01 | 🟡 | Ngân sách nhiễu trước khi chọn linh kiện (WP-1) |
| RR-03 | Không đặt được số cho YC-02 → không biết PoC đạt hay không | 🔴 | Đặt số trước G1 — xem QĐ-2 |
| RR-04 | Nhiễu môi trường tại vùng thử cao hơn dự kiến | 🟡 | Đo nhiễu nền thực địa sớm, trước khi chốt thiết kế |
| RR-05 | Mất phao khi thu hồi → mất toàn bộ dữ liệu thô | 🟡 | CN-07; cân nhắc truyền trước phần dữ liệu quan trọng |
| RR-06 | Cấp phép tần số vô tuyến chậm hơn tiến độ | 🟡 | Làm rõ thủ tục ngay tuần đầu |
| RR-07 | Không có tàu/thời tiết phù hợp vào cửa sổ thử nghiệm | 🟡 | Đặt lịch sớm, chuẩn bị cửa sổ dự phòng |
| RR-08 | Không có mục tiêu thật đi qua trong lúc thử → không chứng minh được PoC-3 | 🟡 | Chọn khu vực có giao thông tàu; bố trí tàu hợp tác chạy qua làm phương án dự phòng |

> **RR-08 hay bị bỏ sót.** Một PoC giám sát thụ động thả ở nơi vắng sẽ ghi được rất nhiều
> dữ liệu đẹp mà **không chứng minh được gì**. Phải chủ động bảo đảm có mục tiêu để phát hiện.

---

## 8. LỘ TRÌNH XOẮN ỐC

### 8.1 Vòng xoắn 1 — bốn cổng nội bộ

| Cổng | Nội dung | Tiêu chí thoát |
|---|---|---|
| **G1 — Đặc tả** | Đặt số cho YC-02, YC-03, YC-07; ngân sách nhiễu; chốt phương án treo giảm chấn | YC-02 có số; ngân sách nhiễu cho thấy YC-01 khả thi |
| **G2 — Bàn thí nghiệm** | Dựng chuỗi thu, đo nhiễu nội tại, chạy chuỗi phân tích trên dữ liệu ghi sẵn | YC-01 đạt trên bàn |
| **G3 — Thử nghiệm biển** | Thả phao, đo nền nhiễu tĩnh và động, thu dữ liệu có mục tiêu thật | **PoC-1…PoC-4 đều đạt** |
| **G4 — Kết luận vòng 1** | Phân tích dữ liệu, viết báo cáo, quyết định vòng 2 | Báo cáo + khuyến nghị đi tiếp / dừng / làm lại |

### 8.2 Các vòng sau — chỉ phác, chưa cam kết

| Vòng | Rủi ro chính cần gỡ | Nội dung chính |
|---|---|---|
| **Vòng 2** | Trực canh dài ngày có khả thi không? | Nguồn điện, chống hà bám, độ bền, mã hoá |
| **Vòng 3** | Có định hướng được nguồn âm không? | Mảng nhiều phần tử, tạo búp sóng, định hướng |
| **Vòng 4** | Có bán được không? | Sản phẩm hoá, tài liệu, đào tạo, hỗ trợ |

> **Nguyên tắc xoắn ốc:** mỗi vòng gỡ **rủi ro lớn nhất còn lại**, không phải làm thêm tính năng.
> Nội dung vòng 2 chỉ được chốt **sau khi** có kết quả vòng 1 — vì kết quả đó có thể đổi hẳn
> thứ tự rủi ro.

---

## 9. VIỆC BẮT ĐẦU NGAY

| # | Việc | Ai | Đầu ra | Chặn bởi |
|---|---|---|---|---|
| 1 | **Đặt số cho YC-02** — ngưỡng nhiễu khi phao hoạt động thật | Chief Engineer + kỹ sư đo lường | Con số + lập luận | — |
| 2 | **Ngân sách nhiễu** chuỗi điện tử | Kỹ sư analog | Bảng tính, kết luận YC-01 | — |
| 3 | **Thiết kế cơ cấu treo giảm chấn**, ≥2 phương án | Kỹ sư cơ khí | Bản vẽ + luận cứ | — |
| 4 | **Đo nhiễu nền vùng thử nghiệm** | Kỹ sư field | Phổ nhiễu nền thực tế | Cần thuỷ âm + tàu |
| 5 | Đặc tả định dạng dữ liệu + khung phân tích ngoại tuyến | Kỹ sư phần mềm | Đặc tả + code đọc file chạy được | — |
| 6 | Khảo sát hiệu chuẩn thuỷ âm mức PoC | Kỹ sư đo lường | Phương án + chi phí | — |
| 7 | Làm rõ cấp phép tần số | Kỹ sư hệ thống | Thủ tục, thời gian | — |
| 8 | Chọn khu vực thử + bảo đảm có mục tiêu (RR-08) | Kỹ sư field | Kế hoạch thử nghiệm | — |

**Việc 1, 2, 3 là đường găng.** Ba việc này quyết định vòng xoắn 1 thành hay bại.

Việc 4 nên làm **sớm nhất có thể** — nó cho biết mức nhiễu môi trường thật, mà mọi ngưỡng
trong YC-01 và YC-02 đều tham chiếu về đó. Làm việc này bằng thiết bị đi mượn cũng được;
không cần chờ phần cứng của mình.

---

## 10. QUYẾT ĐỊNH CÒN MỞ

| # | Quyết định | Chặn | Mức |
|---|---|---|---|
| ~~QĐ-0~~ | ~~Nhiệm vụ chính~~ | — | ✅ **đã đóng 2026-08-25** |
| **QĐ-2** | Ngưỡng cho YC-02 và sai số công bố YC-07 | Tiêu chí đạt/không đạt của cả PoC | 🔴 |
| QĐ-1 | Dải tần công tác | Chọn thuỷ âm, ADC, tần số lấy mẫu | 🔴 |
| QĐ-3 | Phương án treo giảm chấn | A1, A8 | 🔴 |
| QĐ-4 | Neo cố định hay thả trôi | A8, kế hoạch thử nghiệm, RR-01 | 🔴 |
| QĐ-5 | Thời gian một đợt triển khai vòng 1 | Nguồn điện, dung lượng lưu trữ | 🟡 |
| QĐ-6 | Mức hiệu chuẩn cần cho PoC | C, chi phí | 🟡 |
| QĐ-7 | Thuật toán phát hiện sự kiện dùng ở vòng 1 | A4 | 🟡 |
| QĐ-8 | Khu vực và cửa sổ thời gian thử nghiệm | Kế hoạch, RR-04, RR-08 | 🟡 |
| ~~QĐ-11~~ | ~~Kênh nghe tương tự vs mã hoá~~ | — | ✅ đóng — bỏ kênh nghe khỏi vòng 1 |

> **QĐ-4 (neo hay trôi) ảnh hưởng trực tiếp tới RR-01** và hay bị coi là chi tiết triển khai.
> Nó không phải chi tiết: phao neo chịu lực dây neo và rung dây; phao trôi thì không, nhưng
> lại trôi khỏi khu vực quan tâm và khó thu hồi hơn. Hai bài toán nhiễu khác nhau.

---

## 11. THUẬT NGỮ

| Viết tắt | Tiếng Anh | Giải thích |
|---|---|---|
| ADC | Analog-to-Digital Converter | Bộ chuyển đổi tương tự – số |
| BIT | Built-In Test | Tự kiểm tra tích hợp |
| CONOPS | Concept of Operations | Khái niệm vận hành |
| DEMON | Detection of Envelope Modulation On Noise | Giải điều chế đường bao — trích tần số quay chân vịt |
| GNSS | Global Navigation Satellite System | Hệ thống định vị vệ tinh |
| PAM | Passive Acoustic Monitoring | Giám sát thuỷ âm thụ động |
| PGA | Programmable Gain Amplifier | Khuếch đại lập trình được |
| PoC | Proof of Concept | Chứng minh khả thi |
| PZT | Lead Zirconate Titanate | Gốm áp điện |
| **RL** | **Received Level** | **Mức áp suất âm thu được — sản phẩm đầu ra của ta** |
| SL | Source Level | Mức nguồn — **không phải** đầu ra của hệ này |
| Flow noise | — | Nhiễu do dòng chảy qua thuỷ âm |
| Strum | — | Rung dây neo/cáp do dòng chảy, truyền nhiễu xuống thuỷ âm |
| Triboelectric | — | Nhiễu ma sát điện sinh ra khi cáp bị uốn hoặc rung |
| Spiral | — | Mô hình phát triển xoắn ốc, mỗi vòng gỡ rủi ro lớn nhất còn lại |

---

## 12. LỊCH SỬ SỬA ĐỔI

| Phiên bản | Ngày | Nội dung |
|---|---|---|
| 0.1 | 2026-08-25 | Bản đầu; giả định nhiệm vụ giám sát đa tĩnh, mảng đặt đáy |
| 0.2 | 2026-08-25 | Viết lại sau khi có catalog TMK-SAS; phát hiện tài liệu tham chiếu mô tả hệ **đo chữ ký**, nâng thành QĐ-0 |
| **0.3** | **2026-08-25** | **QĐ-0 đóng: sản phẩm là phao giám sát thuỷ âm thụ động.** SONRAS hạ xuống vai trò *kiến trúc tham chiếu*, không phải nhiệm vụ tham chiếu — bỏ toàn bộ cơ chế chuẩn hoá cự ly và mức nguồn. Tái cấu trúc theo mô hình xoắn ốc, phạm vi thu về **vòng 1 PoC** với danh sách loại trừ tường minh. Nâng **nhiễu do nền tảng** thành rủi ro chi phối và là lý do tồn tại của vòng xoắn. Bỏ 7 yêu cầu chức năng khỏi vòng 1, qua đó đóng luôn QĐ-11 |

---

## 13. PHẠM VI TÀI LIỆU

Kho này chỉ chứa **nội dung kỹ thuật**. Không chứa điều khoản thương mại và giá, danh tính
đối tác và vấn đề kiểm soát xuất khẩu, địa điểm triển khai cụ thể, thông tin khách hàng và
tiến trình phê duyệt. Cần các thông tin đó để ra quyết định kỹ thuật → đề nghị qua Chief Engineer.
