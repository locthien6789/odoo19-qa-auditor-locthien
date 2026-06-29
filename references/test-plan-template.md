# Test Plan Template — Odoo 19 Enterprise Lộc Thiên

## 1. Thông tin chung
| Mục | Nội dung |
|---|---|
| Tên dự án | Audit Odoo 19 — [Module/Quy trình] |
| Ngày bắt đầu | YYYY-MM-DD |
| Ngày kết thúc dự kiến | YYYY-MM-DD |
| Test Lead | [Tên] |
| Phiên bản Odoo | 19.0 Enterprise |
| Môi trường | Localhost + Cloudflare Tunnel |
| URL test | https://[subdomain].locthien.vn |

## 2. Mục tiêu
- [ ] Xác minh quy trình [X] chạy đúng end-to-end
- [ ] Tìm tối thiểu 80% lỗi tiềm ẩn trước go-live
- [ ] Đảm bảo tuân thủ TT133 và HĐĐT
- [ ] Đo lường performance với volume thực tế

## 3. Phạm vi

### Trong phạm vi
- Module: ...
- Quy trình: ...
- Tích hợp: ...

### Ngoài phạm vi
- Module: ...
- Lý do loại trừ: ...

## 4. Dữ liệu test
| Loại | Nguồn | Volume |
|---|---|---|
| Khách hàng B2B | Export từ CRM cũ | ~500 |
| Sản phẩm hóa chất | Master data | ~120 SKU |
| Pricelist | Excel hợp đồng năm | ~20 pricelist |
| Lot/COA | PDF mẫu | ~50 lot |
| Hóa đơn mẫu | Tự generate | ~200 invoice |

## 5. Môi trường

### Hardware
- CPU/RAM/Disk
- Database size

### Software
- Odoo 19.0 Enterprise (commit hash: ...)
- PostgreSQL version: ...
- Python version: ...
- l10n_vn version: ...
- Module HĐĐT: ... (nhà cung cấp nào?)

### Mạng
- Cloudflare Tunnel: tên tunnel, region
- Bandwidth nội bộ

## 6. Tiêu chí PASS/FAIL

### Điều kiện PASS để go-live
- 0 bug Critical
- ≤ 3 bug High có workaround
- 100% test case L1 (happy path) PASS
- ≥ 90% test case L4 (integration) PASS
- Báo cáo TT133 cân số liệu với sổ kế toán cũ trong 3 tháng gần nhất

### Điều kiện FAIL (block go-live)
- Bất kỳ bug Critical chưa fix
- HĐĐT phát hành sai bất kỳ
- Mất dữ liệu khi backup/restore

## 7. Lịch trình
| Tuần | Nội dung |
|---|---|
| W1 | Setup môi trường, import data, smoke test |
| W2 | L1-L2 (happy + edge case) toàn bộ module core |
| W3 | L3-L4 (negative + integration) |
| W4 | L5 (security, SoD, performance) |
| W5 | UAT với key user, fix bug, retest |
| W6 | Final report, sign-off |

## 8. Tài nguyên
| Vai trò | Người | %FTE |
|---|---|---|
| Test Lead | ... | 100% |
| Functional Tester (Sales/CRM) | ... | 50% |
| Functional Tester (Accounting) | ... | 100% |
| Technical Tester (API/DB) | ... | 30% |
| Key User (Kế toán) | ... | UAT only |
| Key User (Kho) | ... | UAT only |

## 9. Rủi ro & Mitigation
| Rủi ro | Mức | Mitigation |
|---|---|---|
| Cloudflare Tunnel down | High | Có VPN backup + IP cố định |
| Mất dữ liệu test | Medium | Backup DB hàng ngày |
| Thiếu key user UAT | Medium | Lên lịch sớm, có backup person |
| l10n_vn không support TT133 đầy đủ | High | Verify trước, có plan B custom |

## 10. Deliverables
- [ ] Test Plan (file này)
- [ ] Test Cases — Google Sheets
- [ ] Bug Reports — Format chuẩn
- [ ] Final Report — Markdown + PDF
- [ ] Sign-off document

## 11. Sign-off
| Vai trò | Tên | Ngày | Chữ ký |
|---|---|---|---|
| CEO | TRAN NGOC QUANG | | |
| Kế toán trưởng | | | |
| IT Lead | | | |
| Test Lead | | | |
