# HW-SNR-26 — ĐẶC TẢ YÊU CẦU (VOLERE)

**Vòng xoắn 1 — Proof of Concept · Phao giám sát thuỷ âm thụ động**

Theo **Volere Requirements Specification Template v20**
(`../reference/Requirements Specification Template v20.doc`).

| | |
|---|---|
| **Phiên bản** | 0.2 — tái phạm vi về vòng xoắn 1 PoC |
| **Ngày** | 2026-08-25 |
| **Ngôn ngữ** | Tiếng Việt; giữ tên mục tiếng Anh của Volere để đối chiếu |
| **Tài liệu liên quan** | `../PROJECT-CHARTER.md` v0.3 · `../SYSTEM-SPECIFICATION.md` v0.2 |

---

## Phạm vi

Bộ đặc tả này **chỉ cho vòng xoắn 1**. Nhiệm vụ đã chốt: **phao giám sát thuỷ âm thụ động**.

Nhiều mục Volere được đánh dấu ⬛ **không áp dụng ở vòng 1** — đó là quyết định có chủ ý của
một PoC, không phải bỏ sót. Mỗi mục như vậy đều ghi rõ lý do và trỏ sang `26-waiting-room.md`.

**Sản phẩm đầu ra của hệ thống:** mức áp suất âm **thu được** (RL) + danh sách sự kiện phát hiện.
**Không phải** mức nguồn — giám sát thụ động không biết cự ly tới mục tiêu nên không chuẩn hoá được.

---

## Cấu trúc

### Project Drivers
| File | Mục | Trạng thái |
|---|---|---|
| `01-purpose.md` | The Purpose of the Project | 🟢 |
| `02-stakeholders.md` | Stakeholders | 🟡 |

### Project Constraints
| File | Mục | Trạng thái |
|---|---|---|
| `03-constraints.md` | Constraints | 🟡 |
| `04-terminology.md` | Naming Conventions and Terminology | 🟡 |
| `05-facts-assumptions.md` | Relevant Facts and Assumptions | 🟡 |

### Functional Requirements
| File | Mục | Trạng thái |
|---|---|---|
| `06-scope-of-work.md` | The Scope of the Work (BUC) | 🟡 |
| `07-data-model.md` | Business Data Model and Data Dictionary | 🟡 |
| `08-scope-of-product.md` | The Scope of the Product (PUC) | 🟡 |
| `09-functional.md` | Functional Requirements | 🟡 |

### Non-functional Requirements
| File | Mục | Trạng thái |
|---|---|---|
| `10-look-and-feel.md` | Look and Feel | ⬛ |
| `11-usability.md` | Usability and Humanity | ⬛ |
| `12-performance.md` | Performance | 🟡 |
| `13-operational.md` | Operational and Environmental | 🟡 |
| `14-maintainability.md` | Maintainability and Support | ⬛ |
| `15-security.md` | Security | ⬛ |
| `16-cultural.md` | Cultural | 🟡 |
| `17-compliance.md` | Compliance | 🟡 |

### Project Issues
| File | Mục | Trạng thái |
|---|---|---|
| `18-open-issues.md` | Open Issues | 🟢 |
| `19-off-the-shelf.md` | Off-the-Shelf Solutions | 🟡 |
| `20-new-problems.md` | New Problems | 🟡 |
| `21-project-planning.md` | Project Planning | 🟡 |
| `22-migration.md` | Migration | ⬛ |
| `23-risks.md` | Risks | 🟢 |
| `24-costs.md` | Costs | ⬛ ngoài phạm vi repo |
| `25-documentation-training.md` | User Documentation and Training | 🟡 |
| `26-waiting-room.md` | Waiting Room | 🟢 |
| `27-ideas-for-solutions.md` | Ideas for Solutions | 🟡 |

🔴 trống · 🟡 đang soạn · 🟢 đủ để review · ⬛ không áp dụng ở vòng 1 (có ghi lý do)

---

## Bản ghi yêu cầu

| File | Nội dung |
|---|---|
| `atomic-requirements.csv` | **Bản ghi chính thức.** 20 yêu cầu, 16 trường Volere |
| `_requirement-shell-template.md` | Mẫu shell + hướng dẫn + ba lỗi hay gặp |

CSV thay vì XLS để **diff được trong git**. Mở bằng Excel/WPS bình thường (UTF-8 BOM).

---

## Quy tắc làm việc

1. **Số hiệu yêu cầu là vĩnh viễn.** Không tái sử dụng số của yêu cầu đã bỏ.
2. **Không có Fit Criterion thì yêu cầu chưa xong.** Quy tắc cốt lõi của Volere.
3. **Một yêu cầu = một hành động.** Thấy chữ "và" nối hai hành động thì tách.
4. **Yêu cầu nói *cần gì*, không nói *làm thế nào*.** Giải pháp ghi vào `27-ideas-for-solutions.md`.
5. **Xung đột phải ghi ra, không giấu.** Cột Conflicts.
6. **Hoãn thì cho vào phòng chờ, đừng xoá.** `26-waiting-room.md`.
7. **Thêm vào vòng 1 phải trả lời được:** *nó gỡ rủi ro nào trong PoC-1…PoC-4?* Không trả lời
   được thì vào phòng chờ.

---

## Tình trạng

| Chỉ số | Giá trị | Ghi chú |
|---|---|---|
| Số yêu cầu | 20 | giảm từ 25 sau khi tái phạm vi về PoC |
| Có Fit Criterion đo được | 19 | |
| **Thiếu Fit Criterion** | **1** | yêu cầu 2.0 — chặn bởi QĐ-2 |
| Đã gán PUC | 20 / 20 | |
| **Xung đột chưa giải** | **0** | xung đột cũ tự hết khi bỏ kênh nghe tương tự |
| Quyết định mở | 10 | |
| Quyết định đã đóng | 7 | gồm QĐ-0 |
| Mục Volere ⬛ không áp dụng | 6 / 27 | có chủ ý, đã ghi lý do |

### Yêu cầu quan trọng nhất

**Yêu cầu 2.0** — nhiễu tổng khi phao hoạt động thật. Đây là phát biểu định lượng của PoC-2,
mệnh đề khó nhất của vòng xoắn. **Nó chưa có ngưỡng.** Không có ngưỡng thì không phán quyết
được PoC đạt hay không đạt.

### Ba việc cần làm ngay

| # | Việc | Ai | Chặn |
|---|---|---|---|
| 1 | **Đặt ngưỡng cho yêu cầu 2.0 (YC-02)** | Chief Engineer + đo lường | tiêu chí đạt/không đạt của cả PoC |
| 2 | **Ngân sách nhiễu** chuỗi điện tử | Kỹ sư analog | yêu cầu 1.0 |
| 3 | **Thiết kế >= 2 phương án treo giảm chấn** | Kỹ sư cơ khí | yêu cầu 2.0, 17.0 |
