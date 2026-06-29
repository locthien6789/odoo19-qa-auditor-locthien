# Final Audit Report Template

> Đây là báo cáo CUỐI KỲ audit. CEO + Kế toán trưởng + IT Lead đều đọc.
> Phải gói gọn để CEO đọc 2 phút, kế toán đọc chi tiết.

---

# Báo cáo Audit Odoo 19 Enterprise — Lộc Thiên

**Phạm vi**: [Module / Quy trình]
**Kỳ audit**: từ [DD/MM/YYYY] đến [DD/MM/YYYY]
**Phiên bản hệ thống**: Odoo 19.0 EE, l10n_vn [version], HĐĐT [provider]
**Người thực hiện**: [Tên]
**Báo cáo cho**: Mr. TRAN NGOC QUANG (CEO) + Kế toán trưởng

---

## 0. EXECUTIVE SUMMARY (CEO đọc 2 phút)

### 0.1 Tình trạng tổng quan
🟢 GO / 🟡 GO with conditions / 🔴 NO-GO

### 0.2 Số liệu chính
| | Số lượng |
|---|---|
| Tổng test case đã chạy | XXX |
| PASS rate | XX% |
| 🔴 Critical bugs | X |
| 🟠 High bugs | X |
| 🟡 Medium bugs | X |
| 🟢 Low / Improvements | X |

### 0.3 TOP 3 rủi ro lớn nhất
| # | Rủi ro | Thiệt hại ước tính/năm |
|---|---|---|
| 1 | [Tên] | X triệu VND |
| 2 | [Tên] | X triệu VND |
| 3 | [Tên] | X triệu VND |

### 0.4 Khuyến nghị CEO
1. ✅ **Fix ngay** trước go-live: [danh sách Critical]
2. 📅 **Fix trong 30 ngày**: [danh sách High]
3. 💰 **Đầu tư thêm**: VNĐ + thời gian + nhân sự
4. 📆 **Đề xuất ngày go-live**: DD/MM/YYYY

---

## 1. PHẠM VI & GIẢ ĐỊNH

### 1.1 Trong phạm vi
- Module: ...
- Quy trình end-to-end: ...
- Volume test: ...

### 1.2 Ngoài phạm vi
- ...

### 1.3 Giả định
- DN dùng TT133
- HĐĐT đã ký hợp đồng với [nhà cung cấp]
- Có chữ ký số HSM/USB token
- Server localhost cấu hình: ...

---

## 2. PHƯƠNG PHÁP

### 2.1 Test plan
[Link]

### 2.2 Test case theo 5 lớp
| Lớp | Số TC | PASS | FAIL | Block |
|---|---|---|---|---|
| L1 Happy path | 50 | 48 | 2 | 0 |
| L2 Edge case | 60 | 52 | 8 | 0 |
| L3 Negative | 40 | 35 | 5 | 0 |
| L4 Integration | 30 | 25 | 4 | 1 |
| L5 Security/SoD | 25 | 20 | 5 | 0 |
| **Tổng** | **205** | **180** | **24** | **1** |

### 2.3 Công cụ
- Manual testing: Chrome DevTools, Postman
- API testing: XML-RPC + JSON-RPC
- DB verify: psql client + SQL queries
- Performance: K6 / JMeter
- Screenshot/Video: ShareX / Loom

---

## 3. PHÁT HIỆN CHÍNH

### 3.1 🔴 CRITICAL (X bugs)

#### BUG-001: [Title]
- **Module**: ...
- **Mô tả ngắn**: ...
- **Tác động CEO**: ...
- **Tác động Kế toán**: ...
- **Root cause**: ...
- **Đề xuất fix**: ...
- **Effort**: X người-ngày

(lặp lại cho từng critical bug)

### 3.2 🟠 HIGH (X bugs)
[Bảng tóm tắt, chi tiết ở phụ lục]

| ID | Title | Module | Effort | Đề xuất |
|---|---|---|---|---|
| BUG-... | ... | ... | Xd | ... |

### 3.3 🟡 MEDIUM (X bugs)
[Bảng]

### 3.4 🟢 LOW / Đề xuất cải tiến
[Bảng]

---

## 4. RISK MATRIX

| Rủi ro | Khả năng | Tác động | Mức rủi ro | Owner | Mitigation |
|---|---|---|---|---|---|
| Mất dữ liệu kế toán do localhost hỏng | Medium | Critical | 🔴 | IT | Backup offsite hàng giờ |
| HĐĐT phát hành sai số/trùng số | Low | Critical | 🟠 | Kế toán | Lock sequence, audit log |
| Gian lận sửa giá sau confirm SO | Medium | High | 🟠 | Sales Manager | SoD + approval workflow |
| Tỉ trọng UoM bị sửa nhầm | Low | High | 🟡 | IT | Read-only sau giao dịch đầu |
| Cloudflare Tunnel down ngoài giờ | Low | High | 🟡 | IT | VPN backup |
| ... | | | | | |

**Quy ước**:
- Khả năng: Low / Medium / High
- Tác động: Low / Medium / High / Critical
- Mức rủi ro = Khả năng × Tác động (ma trận 4×4)

---

## 5. ROADMAP FIX

### 5.1 Quick win (< 1 tuần) — Tự DN làm được
- [ ] Config phân quyền lại (SoD) — IT, 2 ngày
- [ ] Bật log audit cho 10 model quan trọng — IT, 1 ngày
- [ ] Setup backup automation — IT, 1 ngày
- [ ] Đổi mật khẩu admin/master — IT, 1 giờ
- [ ] Training kế toán quy trình HĐĐT chuẩn — Kế toán trưởng, 2 buổi

**Tổng effort**: X người-ngày
**Ước tính ROI**: giảm rủi ro X triệu VND/năm

### 5.2 Medium (1-4 tuần) — Cần dev/partner
- [ ] Customize UoM conversion theo tỉ trọng — Dev, 5 ngày
- [ ] Customize approval workflow nhiều cấp — Dev, 8 ngày
- [ ] Tích hợp backup S3/Cloud — IT + Dev, 3 ngày
- [ ] Báo cáo TT133 (B01-B09) chuẩn VN — Dev/Partner, 10 ngày

**Tổng effort**: X người-ngày
**Chi phí ước tính**: VNĐ
**ROI**: tăng X% năng suất, giảm Y% sai sót

### 5.3 Long-term (> 1 tháng) — Strategic
- [ ] Tích hợp B2B Customer Portal — 2 tháng
- [ ] Mobile app cho sales — 3 tháng
- [ ] Tích hợp ngân hàng auto reconcile — 1 tháng
- [ ] AI/ML cho forecast demand — 6 tháng

---

## 6. ĐỀ XUẤT CẢI TIẾN (không phải bug)

### 6.1 Process
- ... (vd: rút gọn quy trình duyệt SO từ 3 cấp xuống 2 cấp)

### 6.2 Technology
- ... (vd: chuyển từ long-polling sang Server-Sent Events)

### 6.3 People
- ... (vd: cử 1 IT đi đào tạo Odoo Certification)

### 6.4 Data
- ... (vd: chuẩn hóa master data sản phẩm với SKU code)

---

## 7. TÀI CHÍNH (ROI dự kiến)

### 7.1 Chi phí fix
| Mục | Chi phí | Thời gian |
|---|---|---|
| Quick win | 0 - 5tr | 1 tuần |
| Medium customization | X tr | 1 tháng |
| Long-term | XX tr | 6 tháng |
| **Tổng năm 1** | **XXX tr** | |

### 7.2 Lợi ích (giảm chi phí + tăng doanh thu)
| Hạng mục | VNĐ/năm |
|---|---|
| Giảm sai sót kho (chênh lệch UoM) | X tr |
| Giảm sai sót kế toán (bút toán/thuế) | X tr |
| Giảm thời gian xuất hóa đơn | X tr |
| Tăng tốc thu hồi công nợ (giảm DSO) | X tr |
| Giảm rủi ro gian lận (SoD) | X tr |
| **Tổng năm 1** | **XXX tr** |

### 7.3 Payback period
**[X tháng]** sau go-live

---

## 8. KẾT LUẬN & QUYẾT ĐỊNH

### 8.1 Khuyến nghị Test Lead
[Go / Go with conditions / No-go]

### 8.2 Điều kiện Go-live
- [ ] Fix 100% Critical bugs
- [ ] Fix ≥ 80% High bugs
- [ ] Setup backup + DR
- [ ] Training tối thiểu 16 giờ cho key user
- [ ] Có SOP văn bản cho 5 quy trình chính
- [ ] CEO + Kế toán trưởng + IT Lead sign-off

### 8.3 Plan B (nếu chưa go-live được)
- Chạy song song hệ thống cũ + Odoo trong 1 tháng
- Hoặc go-live module Sales/Inventory trước, Accounting sau

---

## 9. PHỤ LỤC

### A. Chi tiết test case (Google Sheets link)
### B. Bug list đầy đủ (CSV)
### C. SQL queries verify
### D. Screenshot/Video bằng chứng
### E. Báo cáo performance
### F. Báo cáo security scan
### G. Tài liệu tham khảo
- Odoo 19 Documentation
- TT133/2016/TT-BTC
- TT78/2021/TT-BTC + NĐ123/2020/NĐ-CP
- Luật Kế toán 2015

---

## 10. SIGN-OFF

| Vai trò | Họ tên | Ngày | Chữ ký |
|---|---|---|---|
| Test Lead | | | |
| Kế toán trưởng | | | |
| IT Lead | | | |
| CEO | TRAN NGOC QUANG | | |
