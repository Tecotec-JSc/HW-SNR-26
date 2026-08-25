# HW-SNR-26 — ĐẶC TẢ YÊU CẦU (VOLERE)

Bộ đặc tả yêu cầu theo **Volere Requirements Specification Template v20**
(`../reference/Requirements Specification Template v20.doc`).

| | |
|---|---|
| **Phiên bản** | 0.1 — khung đã dựng, nội dung đang soạn |
| **Ngày** | 2026-08-25 |
| **Ngôn ngữ** | Tiếng Việt; giữ nguyên tên mục tiếng Anh của Volere để đối chiếu |
| **Tài liệu liên quan** | `../PROJECT-CHARTER.md` · `../SYSTEM-SPECIFICATION.md` |

---

## ⚠ ĐỌC TRƯỚC

Toàn bộ đặc tả này đang chờ **QĐ-0** — chưa chốt nhiệm vụ chính của hệ thống là
**đo chữ ký âm học** hay **giám sát thuỷ âm**. Xem `../PROJECT-CHARTER.md` §1 và
`18-open-issues.md`.

Khung hiện tại viết theo **Hướng A (đo chữ ký)**. Nếu chốt Hướng B, các mục
1, 3, 6, 8, 9, 12, 13 phải soạn lại.

---

## Cấu trúc

Volere chia yêu cầu thành 5 nhóm. Trạng thái hiện tại:

### Project Drivers — Động lực dự án
| File | Mục | Trạng thái |
|---|---|---|
| `01-purpose.md` | The Purpose of the Project | 🟡 |
| `02-stakeholders.md` | Stakeholders | 🟡 |

### Project Constraints — Ràng buộc
| File | Mục | Trạng thái |
|---|---|---|
| `03-constraints.md` | Constraints | 🟡 |
| `04-terminology.md` | Naming Conventions and Terminology | 🟡 |
| `05-facts-assumptions.md` | Relevant Facts and Assumptions | 🟡 |

### Functional Requirements — Yêu cầu chức năng
| File | Mục | Trạng thái |
|---|---|---|
| `06-scope-of-work.md` | The Scope of the Work (BUC) | 🔴 |
| `07-data-model.md` | Business Data Model and Data Dictionary | 🟡 |
| `08-scope-of-product.md` | The Scope of the Product (PUC) | 🟡 |
| `09-functional.md` | Functional Requirements | 🟡 |

### Non-functional Requirements — Yêu cầu phi chức năng
| File | Mục | Trạng thái |
|---|---|---|
| `10-look-and-feel.md` | Look and Feel | 🔴 |
| `11-usability.md` | Usability and Humanity | 🔴 |
| `12-performance.md` | Performance | 🟡 |
| `13-operational.md` | Operational and Environmental | 🟡 |
| `14-maintainability.md` | Maintainability and Support | 🔴 |
| `15-security.md` | Security | 🟡 |
| `16-cultural.md` | Cultural | 🔴 |
| `17-compliance.md` | Compliance | 🟡 |

### Project Issues — Vấn đề dự án
| File | Mục | Trạng thái |
|---|---|---|
| `18-open-issues.md` | Open Issues | 🟢 |
| `19-off-the-shelf.md` | Off-the-Shelf Solutions | 🟡 |
| `20-new-problems.md` | New Problems | 🔴 |
| `21-project-planning.md` | Project Planning | 🟡 |
| `22-migration.md` | Migration to the New Product | 🔴 |
| `23-risks.md` | Risks | 🟡 |
| `24-costs.md` | Costs | ⬛ ngoài phạm vi repo |
| `25-documentation-training.md` | User Documentation and Training | 🔴 |
| `26-waiting-room.md` | Waiting Room | 🟡 |
| `27-ideas-for-solutions.md` | Ideas for Solutions | 🟡 |

🔴 trống · 🟡 đang soạn · 🟢 đủ để review · ⬛ cố ý không ghi ở repo này

---

## Bản ghi yêu cầu

| File | Nội dung |
|---|---|
| `atomic-requirements.csv` | **Bản ghi chính thức.** 25 yêu cầu, 16 trường Volere |
| `_requirement-shell-template.md` | Mẫu shell + hướng dẫn viết + ba lỗi hay gặp |

CSV được chọn thay vì XLS để **diff được trong git** — xem lịch sử thay đổi từng yêu cầu.
Mở bằng Excel/WPS bình thường (UTF-8 BOM, không lỗi font tiếng Việt).

---

## Quy tắc làm việc

**1. Số hiệu yêu cầu là vĩnh viễn.** Không tái sử dụng số của yêu cầu đã bỏ. Nếu bỏ,
đánh dấu trong cột History, giữ nguyên dòng.

**2. Không có Fit Criterion thì yêu cầu chưa xong.** Đây là quy tắc cốt lõi của Volere.
Một yêu cầu không kiểm chứng được khách quan là một mong muốn, không phải yêu cầu.

**3. Một yêu cầu = một hành động.** Thấy chữ "và" nối hai hành động thì tách.

**4. Yêu cầu nói *cần gì*, không nói *làm thế nào*.** Giải pháp ghi vào `27-ideas-for-solutions.md`.

**5. Xung đột phải ghi ra, không giấu.** Cột Conflicts. Xung đột đã biết: yêu cầu
9.0 (kênh nghe tương tự) ↔ 13.0 (mã hoá) — xem QĐ-11.

**6. Hoãn thì cho vào phòng chờ, đừng xoá.** `26-waiting-room.md`.

---

## Tình trạng hiện tại

| Chỉ số | Giá trị |
|---|---|
| Số yêu cầu đã ghi | 25 |
| Có Fit Criterion đo được | 24 |
| **Thiếu Fit Criterion** | **1** (yêu cầu 2.0 — chặn bởi QĐ-2) |
| Chưa gán PUC | 7 (chờ `08-scope-of-product.md`) |
| Xung đột chưa giải | 1 (9.0 ↔ 13.0) |
| Quyết định mở | 15 (QĐ-0 … QĐ-15) |

### Ba việc cần làm ngay

| # | Việc | Ai | Chặn |
|---|---|---|---|
| 1 | **Chốt QĐ-0** — nhiệm vụ chính của hệ thống | Chief Engineer | toàn bộ |
| 2 | Đọc **ISO 17208**, đối chiếu các mục còn trống | 1 kỹ sư | §17, §9, §12 |
| 3 | Đặt số cho **YC-13 / yêu cầu 2.0** (sai số công bố) | Kỹ sư đo lường | §12, tiêu chí nghiệm thu G3 |

Việc 2 và 3 **không** bị chặn bởi QĐ-0 và bắt đầu được ngay.
