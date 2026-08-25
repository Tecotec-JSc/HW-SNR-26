# HW-SNR-26

**Phao Giám sát Thuỷ âm Thụ động — Vòng xoắn 1: Proof of Concept**
*Passive Acoustic Monitoring Buoy — Spiral 1: Proof of Concept*

---

## Vòng xoắn này chứng minh điều gì

> Đơn vị có chế tạo được một phao thu thuỷ âm thụ động **đủ yên tĩnh** để nghe được tín hiệu
> quan tâm trong điều kiện biển thật hay không.

Bốn mệnh đề phải chứng minh, bằng dữ liệu thu ngoài biển thật:

| # | Mệnh đề | Chứng minh ở |
|---|---|---|
| **PoC-1** | Chuỗi thu có nhiễu nội tại thấp hơn nhiễu môi trường | G2 bàn thí nghiệm → G3 biển |
| **PoC-2** | **Phao giữ được thuỷ âm đủ yên tĩnh khi hoạt động thật** | **G3 biển** ⭐ |
| **PoC-3** | Phát hiện được mục tiêu thật trong dữ liệu thật | G3 biển |
| **PoC-4** | Dữ liệu lấy về được và phân tích được | G3 biển |

**PoC-2 là mệnh đề khó nhất và là lý do tồn tại của vòng xoắn này.**

---

## Rủi ro chi phối: nhiễu do nền tảng, không phải nhiễu điện tử

Một phao nổi **tự sinh ra tiếng ồn** ngay tại chỗ đặt thuỷ âm — dòng chảy qua cảm biến, rung
dây neo, chuyển động phao truyền qua cáp, nhiễu ma sát điện khi cáp uốn. Tất cả tập trung ở
**dải tần thấp**, đúng dải quan tâm nhất khi phát hiện tàu.

Nhiễu điện tử là bài toán đã biết cách giải và giải được trên bàn. Nhiễu nền tảng chỉ lộ ra
khi xuống nước, và nó quyết định hệ thống có dùng được hay không.

> Vì thế **cơ cấu treo giảm chấn (A1.4)** và **kết cấu phao/neo (A8)** là hai hạng mục thiết
> kế bậc nhất của vòng xoắn này, phải do cùng một nhóm làm. Chi tiết: `PROJECT-CHARTER.md` §4.

---

## SONRAS đóng vai trò gì

Hệ tham chiếu **Scanmatic TMK-SAS / SONRAS** (`reference/`) cho ta **kiến trúc**, không cho
ta nhiệm vụ:

| | SONRAS | HW-SNR-26 |
|---|---|---|
| **Nhiệm vụ** | **Đo** chữ ký âm học tàu mình | **Giám sát** — nghe xem có gì trong khu vực |
| Mục tiêu | Hợp tác, biết trước | **Không hợp tác, không biết trước** |
| Cự ly tới mục tiêu | Biết | **Không biết** |
| Đầu ra | Mức nguồn, chuẩn hoá về 1 m | **Mức thu được + danh sách phát hiện** |

**Mượn:** phao tự hành + trạm điều khiển, GNSS hai đầu, vô tuyến cho dữ liệu đã xử lý, dữ liệu
thô lấy sau khi thu hồi, đóng gói thành kit.

**Không mượn:** bước **chuẩn hoá theo cự ly**. Nó cần biết vị trí mục tiêu — giám sát thụ động
thì không biết. Đây là khác biệt quan trọng nhất giữa hai hệ.

---

## Tài liệu

| Tài liệu | Nội dung | Đọc khi nào |
|---|---|---|
| **`PROJECT-CHARTER.md`** | Nhiệm vụ, phạm vi vòng 1, danh sách loại trừ, rủi ro, lộ trình xoắn ốc | **Đọc đầu tiên** |
| **`SYSTEM-SPECIFICATION.md`** | Cấu trúc, sơ đồ khối, ngân sách nhiễu, đặc tả phân hệ | Khi bắt đầu thiết kế |
| **`requirements/`** | Đặc tả yêu cầu Volere v20 — 27 mục + 20 yêu cầu nguyên tử | Khi viết hoặc rà yêu cầu |
| `reference/` | Catalog hệ tham chiếu + mẫu Volere | Tra cứu |

### Thứ tự đọc cho kỹ sư mới

```
1. README.md (file này)              — 5 phút
2. PROJECT-CHARTER.md §1, §2, §4     — 30 phút: nhiệm vụ, phạm vi, rủi ro chi phối
3. PROJECT-CHARTER.md toàn bộ        — 1 giờ
4. SYSTEM-SPECIFICATION.md §1–§3     — 1 giờ: cấu trúc và ngân sách
5. Phần đặc tả phân hệ của mình      — SYSTEM-SPECIFICATION.md §4
6. requirements/README.md            — khi cần viết yêu cầu
```

---

## Phép đo trung tâm của vòng xoắn 1

Không cần mô hình lý thuyết phức tạp. Vòng 1 dùng **phép đo so sánh**:

```
   Đo A — TĨNH:  thuỷ âm thả từ tàu neo, biển lặng, cáp chùng, không phao
   Đo B — THẬT:  thuỷ âm treo trên phao, đúng cấu hình triển khai

   ĐÓNG GÓP CỦA NỀN TẢNG = Đo B − Đo A      (theo từng dải tần)
```

Phải đo **cả hai trong cùng một chuyến**, cùng điều kiện môi trường, với **ít nhất hai cấu
hình treo** khác nhau. Toàn bộ kế hoạch thử nghiệm biển nên được thiết kế quanh phép đo này.

---

## Phép đo của hệ giám sát thụ động

```
   RL = V_ADC(dB) − M − G + K            [dB re 1 µPa]

   RL   Mức áp suất âm THU ĐƯỢC        ← sản phẩm đầu ra
   M    Độ nhạy thuỷ âm  [dB re 1V/µPa]  ← hồ sơ hiệu chuẩn
   G    Tổng độ lợi      [dB]            ← metadata, PHẢI ghi mọi lần đổi nấc
   K    Hệ số hiệu chuẩn [dB]            ← hồ sơ hiệu chuẩn
```

**Không có số hạng suy hao truyền âm, không quy về 1 m** — khác SONRAS.

---

## Tình trạng

| Chỉ số | Giá trị |
|---|---|
| Cổng hiện tại | **G1 — Đặc tả** |
| Yêu cầu đã ghi | 20 |
| Thiếu Fit Criterion | **1** — yêu cầu 2.0 (ngưỡng YC-02) |
| Xung đột yêu cầu | 0 |
| Quyết định mở | 10 |
| Quyết định đã đóng | 7 (gồm QĐ-0 — nhiệm vụ) |
| Mục Volere không áp dụng ở vòng 1 | 6 / 27 (có chủ ý, đã ghi lý do) |

### Khoảng trống nghiêm trọng nhất

**Ngưỡng cho YC-02 chưa có.** Đó là phát biểu định lượng của PoC-2. Không có ngưỡng thì không
phán quyết được vòng xoắn đạt hay không đạt. Chặn bởi **QĐ-2**, phải đóng trước G1.

### Ba việc cần làm ngay — đường găng

| # | Việc | Ai |
|---|---|---|
| 1 | **Đặt ngưỡng cho YC-02** | Chief Engineer + kỹ sư đo lường |
| 2 | **Ngân sách nhiễu** chuỗi điện tử | Kỹ sư analog |
| 3 | **Thiết kế ≥2 phương án treo giảm chấn** | Kỹ sư cơ khí |

Ngoài đường găng, nên làm sớm nhất có thể: **đo nhiễu nền vùng thử nghiệm** — mọi ngưỡng
trong YC-01 và YC-02 đều tham chiếu về nhiễu môi trường thật. Làm bằng thiết bị đi mượn cũng
được, không cần chờ phần cứng của mình.

---

## Ngoài phạm vi vòng xoắn 1

Định hướng và định vị nguồn · phân loại tự động · trực canh dài ngày · truy xuất chuẩn đo
lường đầy đủ · mã hoá · kênh nghe tương tự · sản phẩm hoá · chịu biển khắc nghiệt.

Tất cả nằm ở `requirements/26-waiting-room.md` kèm vòng xoắn dự kiến.

> **Quy tắc:** ai muốn thêm một hạng mục vào vòng 1 phải trả lời được *"nó gỡ rủi ro nào
> trong PoC-1…PoC-4?"* Không trả lời được thì vào phòng chờ.

---

## Quy ước trong tài liệu

| Ký hiệu | Ý nghĩa |
|---|---|
| `[CHỐT]` | Đã quyết định |
| `[GIẢ ĐỊNH]` | Cơ sở thiết kế nhưng **chưa xác minh** |
| `[MỞ]` | Chưa quyết định — báo Chief Engineer, không tự chọn |
| `[THAM CHIẾU]` | Số của hệ tham chiếu TMK-SAS — **không phải** yêu cầu của ta |
| `[TÍNH]` | Giá trị suy ra bằng tính toán, có trình bày trong tài liệu |
| `[V2+]` | Cố ý đẩy sang vòng xoắn sau |

---

## Phạm vi kho tài liệu

Chỉ chứa **nội dung kỹ thuật**. Không chứa điều khoản thương mại và giá, danh tính đối tác và
vấn đề kiểm soát xuất khẩu, địa điểm triển khai cụ thể, thông tin khách hàng và tiến trình
phê duyệt. Cần các thông tin đó để ra quyết định kỹ thuật → đề nghị qua Chief Engineer.
