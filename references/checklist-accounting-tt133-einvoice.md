# Checklist Test — Accounting TT133 + Hóa đơn điện tử VN

> Module: **Accounting** + **l10n_vn** + tích hợp **HĐĐT** (TT78/2021 + NĐ123/2020)
> Chế độ kế toán: **Thông tư 133/2016/TT-BTC** (DN nhỏ và vừa)

---

## A. Hệ thống tài khoản TT133

| # | Test case | Expected | Mức |
|---|---|---|---|
| A01 | COA (Chart of Accounts) load đầy đủ các TK chính TT133 | TK 111, 112, 131, 133, 156, 211, 214, 311, 331, 333, 334, 341, 411, 421, 511, 515, 632, 642, 711, 811, 911... | 🔴 |
| A02 | Phân loại Type đúng: Asset, Liability, Equity, Income, Expense | Mỗi TK phải đúng type | 🔴 |
| A03 | TK 1311 (phải thu KH) — auto tạo sub-account theo từng KH? | Theo policy: dùng Analytic hoặc Partner-based | 🟠 |
| A04 | TK 333 — chi tiết 3331 (VAT đầu ra), 3334 (TNDN), 3338 (khác) | Đầy đủ sub-account | 🔴 |
| A05 | Tạo TK mới — có chặn không cho đặt mã trùng/sai prefix? | Validation đúng | 🟠 |

## B. Journal — Sổ nhật ký

| # | Test case | Expected | Mức |
|---|---|---|---|
| B01 | Có đủ journal: Sales, Purchase, Cash, Bank, Misc (sổ NKC) | 5 sổ chính | 🔴 |
| B02 | Mỗi journal có default account đúng | Vd: Sales journal → 511; Purchase → 156/642 | 🔴 |
| B03 | Đánh số chứng từ liên tục theo journal, không skip | Sequence không nhảy số | 🔴 |
| B04 | Khóa kỳ kế toán đã quyết toán — không cho sửa | Lock date hoạt động đúng | 🔴 |
| B05 | Multi-currency journal — tỷ giá nhập đúng (Vietcombank end-of-day?) | Theo policy DN | 🟠 |

## C. Bút toán tự động theo nghiệp vụ

### C1 — Bán hàng
| # | Test case | Expected | Mức |
|---|---|---|---|
| C01 | Confirm Invoice → bút toán | Nợ 131 / Có 511 / Có 3331 | 🔴 |
| C02 | Validate Delivery → bút toán giá vốn | Nợ 632 / Có 156 (theo FIFO lot xuất) | 🔴 |
| C03 | Register Payment (tiền mặt) | Nợ 111 / Có 131 | 🔴 |
| C04 | Register Payment (chuyển khoản) | Nợ 112 / Có 131 | 🔴 |
| C05 | Hóa đơn USD, KH thanh toán VND theo tỷ giá ngày TT | Có chênh lệch tỷ giá → 515/635 | 🔴 |
| C06 | Credit Note (hủy/giảm) | Đảo ngược đúng các bút toán trên | 🔴 |

### C2 — Mua hàng
| # | Test case | Expected | Mức |
|---|---|---|---|
| C07 | Validate Receipt → bút toán nhập kho | Nợ 156 / Có 331 (chưa hóa đơn) | 🔴 |
| C08 | Validate Bill → bút toán | Nợ 156 / Nợ 133 / Có 331 (nếu hóa đơn cùng kỳ) | 🔴 |
| C09 | Payment NCC | Nợ 331 / Có 112 | 🔴 |
| C10 | Bill thanh toán trước nhận hàng (đặt cọc) | Nợ 331x (cọc) / Có 112 → khi nhận hàng đối trừ | 🔴 |
| C11 | Nhập khẩu — thuế NK + VAT NK | Nợ 156 (thuế NK) / Nợ 133 (VAT) / Có 3333, 33312 | 🔴 |

### C3 — Khác
| # | Test case | Expected | Mức |
|---|---|---|---|
| C12 | Trả lương | Nợ 642 (lương quản lý) / Nợ 622 (lương CN) / Có 334 / Có 338 | 🟠 |
| C13 | Khấu hao TSCĐ | Nợ 642/627 / Có 214 | 🟠 |
| C14 | Phân bổ chi phí trả trước | Nợ 642 / Có 242 | 🟠 |

## D. VAT (Thuế GTGT)

| # | Test case | Expected | Mức |
|---|---|---|---|
| D01 | Cấu hình thuế 0%, 5%, 8%, 10%, KCT (không chịu) | Đầy đủ, đúng quy định | 🔴 |
| D02 | Sản phẩm hóa chất — VAT 10% (verify theo danh mục NQ 142/2024 có giảm 8% hay không) | Đúng % cho từng SP | 🔴 |
| D03 | Hóa đơn nhiều dòng sản phẩm khác VAT% | Cộng dồn VAT chính xác | 🔴 |
| D04 | VAT đầu vào không được khấu trừ (ăn uống tiếp khách) | Hạch toán 642 thẳng, không qua 133 | 🟠 |
| D05 | Bảng kê hóa đơn mua vào (mẫu 01-1/GTGT) | Đầy đủ, đúng format VN | 🔴 |
| D06 | Bảng kê hóa đơn bán ra (mẫu 01-2/GTGT) | Đầy đủ, đúng format VN | 🔴 |
| D07 | Tờ khai VAT tháng/quý (mẫu 01/GTGT) | Số khớp bảng kê | 🔴 |

## E. Hóa đơn điện tử (HĐĐT) — TT78 + NĐ123

| # | Test case | Expected | Mức |
|---|---|---|---|
| E01 | Phát hành HĐĐT từ Odoo Invoice → gửi CQT thành công | Có MST CQT cấp, có XML, có PDF | 🔴 |
| E02 | Số hóa đơn liên tục theo ký hiệu (1C25TLT) | Không nhảy số, không trùng | 🔴 |
| E03 | Hóa đơn lỗi gửi CQT — có retry tự động không? | Cảnh báo + retry, không câm lặng | 🔴 |
| E04 | Hóa đơn đã gửi CQT — sửa nội dung | KHÔNG cho sửa, phải hủy + phát hành mới | 🔴 |
| E05 | Hủy hóa đơn → biên bản hủy theo NĐ123 | Có template, có chữ ký số | 🔴 |
| E06 | Điều chỉnh hóa đơn (sai số tiền/thuế) | Phát hành HĐ điều chỉnh, link với HĐ gốc | 🔴 |
| E07 | Thay thế hóa đơn (sai thông tin KH) | Phát hành HĐ thay thế, ghi rõ thay thế HĐ nào | 🔴 |
| E08 | Tra cứu HĐĐT trên cổng CQT (hoadondientu.gdt.gov.vn) | Hóa đơn xuất hiện sau X phút | 🟠 |
| E09 | Chữ ký số (USB token hoặc HSM) | Ký thành công, không lỗi expired cert | 🔴 |
| E10 | Hóa đơn cho KH cá nhân không lấy hóa đơn | Có loại hóa đơn "không lấy hóa đơn", gom cuối ngày | 🟠 |
| E11 | KH yêu cầu chuyển đổi sang hóa đơn giấy (theo NĐ123) | Có nút "Chuyển đổi", in được bản giấy hợp lệ | 🟠 |
| E12 | Hóa đơn nước ngoài (xuất khẩu) — VAT 0%, mã ký hiệu khác | Đúng format | 🟠 |

## F. Công nợ (AR/AP)

| # | Test case | Expected | Mức |
|---|---|---|---|
| F01 | Báo cáo công nợ theo tuổi nợ (Aging Report) | 0-30, 31-60, 61-90, >90 ngày | 🔴 |
| F02 | Đối chiếu công nợ — gửi email tự động cho KH | Template có sẵn, gửi định kỳ | 🟠 |
| F03 | Khóa giao dịch với KH có công nợ quá hạn | Workflow phê duyệt | 🔴 |
| F04 | Partial payment — payment 50% của 1 invoice | Reconcile đúng, còn dư | 🔴 |
| F05 | 1 payment thanh toán nhiều invoice | Reconcile theo FIFO hay theo chọn? | 🔴 |
| F06 | Bù trừ công nợ KH ↔ NCC (cùng 1 đối tác là cả KH + NCC) | Bút toán Nợ 331 / Có 131 đúng | 🟠 |

## G. Báo cáo tài chính TT133

| # | Test case | Expected | Mức |
|---|---|---|---|
| G01 | **B01-DNN** — Báo cáo tình hình tài chính (Bảng cân đối kế toán) | Tài sản = Nguồn vốn; mapping đúng TK theo TT133 | 🔴 |
| G02 | **B02-DNN** — Báo cáo kết quả hoạt động kinh doanh | Doanh thu, giá vốn, LNST đúng | 🔴 |
| G03 | **B03-DNN** — Báo cáo lưu chuyển tiền tệ | Phương pháp trực tiếp hoặc gián tiếp | 🔴 |
| G04 | **B09-DNN** — Bản thuyết minh BCTC | Có template, có dữ liệu auto-fill | 🟠 |
| G05 | **S03a-DNN** — Sổ nhật ký chung | Đầy đủ, có thể export Excel/PDF | 🔴 |
| G06 | **S03b-DNN** — Sổ cái | Theo từng TK | 🔴 |
| G07 | **S11-DNN** — Sổ chi tiết vật liệu, dụng cụ | Theo từng SP/lot | 🔴 |
| G08 | Báo cáo có khớp số liệu khi chạy 2 lần liên tiếp | Idempotent, không sai do timing | 🟠 |
| G09 | Kết chuyển cuối kỳ (911) — tự động hay manual? | Theo policy DN | 🔴 |
| G10 | Đóng kỳ kế toán (lock date) — sau khi đóng không sửa được | Lock cứng, ghi audit | 🔴 |

## H. Multi-company (nếu có)

| # | Test case | Expected | Mức |
|---|---|---|---|
| H01 | Lộc Thiên có nhiều pháp nhân? Tách company hay analytic? | Theo decision của CEO | 🟠 |
| H02 | Intercompany transaction — bán nội bộ giữa 2 cty | Tự sinh SO ↔ PO mirror | 🟠 |
| H03 | Consolidated report giữa các cty | Loại trừ giao dịch nội bộ | 🟠 |

## I. Tích hợp ngân hàng

| # | Test case | Expected | Mức |
|---|---|---|---|
| I01 | Import sao kê MB Bank / Vietcombank (file MT940/CSV/Excel) | Parse đúng, không lỗi encoding | 🟠 |
| I02 | Auto-reconcile với invoice dựa trên reference | Match đúng theo nội dung CK | 🟠 |
| I03 | Manual reconcile khi reference không match | UI dễ dùng, có search | 🟠 |
| I04 | Phí ngân hàng → tự hạch toán Nợ 642 / Có 112 | Đúng tài khoản | 🟠 |

## J. Edge cases & Negative

| # | Test case | Expected | Mức |
|---|---|---|---|
| J01 | Post invoice với số tiền 0 | Chặn hoặc warning | 🟠 |
| J02 | Post invoice ngược kỳ (back-date 6 tháng trước) | Chặn nếu kỳ đã khóa | 🔴 |
| J03 | Xóa invoice đã post | KHÔNG cho phép, chỉ Reset to draft (nếu chưa khóa kỳ) | 🔴 |
| J04 | 2 user post cùng 1 invoice cùng lúc | Lock, người sau báo lỗi | 🟠 |
| J05 | Số tiền > 1 tỷ — có vấn đề về precision/rounding không? | Test số to | 🟠 |
| J06 | Tỉ giá ngày phát sinh khác ngày thanh toán — chênh lệch | Bút toán 515/635 đúng | 🔴 |
