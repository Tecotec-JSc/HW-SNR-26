# 13. Operational and Environmental Requirements — Yêu cầu vận hành và môi trường

> **Volere v20 §13** · Đặc tả Yêu cầu HW-SNR-26
> **Trạng thái:** 🟡 Đang soạn
> **Phụ trách:** Kỹ sư hệ thống
> **Cập nhật:** 2026-08-25

## Mục đích phần này (theo Volere)

**13a** Expected physical environment · **13b** Wider environment · **13c** Interfacing with adjacent systems · **13d** Productization · **13e** Release · **13f** Backwards compatibility.

---

## 13a. Môi trường vật lý

Đã nêu tại `../PROJECT-CHARTER.md` §3.6 — phần lớn còn `[MỞ]`:

| Tham số | Trạng thái |
|---|---|
| Vùng nước nông ven bờ | 🟡 giả định |
| Biển nhiệt đới, nhiễu gió mùa, giao thông tàu dày | 🟡 giả định |
| Trạng thái biển thiết kế | 🔴 **chưa có** → QĐ-8 |
| Nhiệt độ nước | 🔴 chưa có |
| Độ sâu làm việc | 🟡 phụ thuộc QĐ-0 |

## 13c. Giao diện với hệ thống lân cận

| Hệ thống lân cận | Giao diện | Trạng thái |
|---|---|---|
| Vệ tinh GNSS | Thu tín hiệu định vị + PPS | 🟢 |
| Hệ thống bên thứ ba (xuất dữ liệu) | CN-14 — định dạng chưa chốt | 🔴 |
| Phòng hiệu chuẩn | Hồ sơ, chứng chỉ hiệu chuẩn | 🟡 |

## 13d. Productization

> **Chưa xét.** Bộ kit 5 kiện K-1…K-5 (`../PROJECT-CHARTER.md` §6.4) là điểm khởi đầu.
> Cần thêm: yêu cầu vận chuyển (đường không? quy định pin?), lắp đặt, nghiệm thu tại chỗ.

