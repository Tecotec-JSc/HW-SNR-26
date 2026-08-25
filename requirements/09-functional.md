# 9. Functional Requirements — Yêu cầu chức năng

> **Volere v20 §9** · Đặc tả Yêu cầu HW-SNR-26
> **Trạng thái:** 🟡 Đang soạn
> **Phụ trách:** Kỹ sư hệ thống
> **Cập nhật:** 2026-08-25

## Mục đích phần này (theo Volere)

Điều sản phẩm **phải làm** — hành động, không phải chất lượng. Mỗi yêu cầu là một atomic requirement theo Volere shell, ghi trong `atomic-requirements.csv`.

---

## Nguồn

14 yêu cầu chức năng CN-01…CN-14 đã có tại `../PROJECT-CHARTER.md` §6.2, và đã được
nạp vào `atomic-requirements.csv` với đầy đủ 16 trường Volere.

## Việc cần làm để chuyển 🟢

| Việc | Ghi chú |
|---|---|
| Gán **PUC number** cho từng CN-xx | Chờ §8 hoàn thành |
| Viết **Fit Criterion** cho mọi yêu cầu | Hiện nhiều dòng còn TBD — **đây là khoảng trống lớn nhất** |
| Rà tính nguyên tử | Một số CN-xx đang gộp nhiều yêu cầu, phải tách |
| Kiểm tra xung đột | Đã biết 1 xung đột: CN-02 (kênh nghe tương tự) ↔ CN-06 (mã hoá) — xem QĐ-11 |

> ⚠ **Xung đột đã phát hiện:** CN-06 yêu cầu mã hoá mọi dữ liệu truyền, nhưng CN-02 là kênh
> tương tự không mã hoá được theo cách thông thường. Volere gọi đây là *conflicting requirements*
> và bắt buộc phải giải trước khi đóng đặc tả. → **QĐ-11**.

