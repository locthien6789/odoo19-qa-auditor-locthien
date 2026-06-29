# Checklist Audit — Hạ tầng: Odoo Localhost + Cloudflare Tunnel

> Môi trường đặc thù: Odoo chạy **localhost** tại văn phòng, expose qua **Cloudflare Tunnel** (cloudflared) để user/KH truy cập từ internet.

---

## A. Hạ tầng vật lý

| # | Test case | Expected | Mức |
|---|---|---|---|
| A01 | Máy chủ localhost cấu hình tối thiểu | CPU 4 core, RAM 16GB, SSD 500GB cho ~20 user | 🔴 |
| A02 | UPS dự phòng điện cho server | ≥ 30 phút duy trì khi mất điện | 🔴 |
| A03 | Internet đường truyền chính + backup (4G LTE) | Tự động failover | 🔴 |
| A04 | Phòng server: nhiệt độ, độ ẩm, an ninh | Có máy lạnh riêng, khóa cửa | 🟠 |
| A05 | Network LAN dây cho user nội bộ | Gigabit, không qua WiFi nếu có thể | 🟠 |
| A06 | Firewall hardware/software | Chặn truy cập trực tiếp vào port DB | 🔴 |

## B. Cloudflare Tunnel (cloudflared)

| # | Test case | Expected | Mức |
|---|---|---|---|
| B01 | cloudflared service auto-start sau reboot | systemd unit enabled | 🔴 |
| B02 | Tunnel có ít nhất 2 instance (HA) | Loadbalance, không single point of failure | 🟠 |
| B03 | Custom domain (odoo.locthien.vn) trỏ qua Cloudflare | DNS Proxy bật | 🔴 |
| B04 | SSL/TLS mode = Full (Strict) | Verify cert | 🔴 |
| B05 | Cloudflare Access (Zero Trust) bảo vệ /web/login | Chỉ user trong tổ chức truy cập | 🟠 |
| B06 | Bypass cho webhook ngân hàng / CQT | Whitelist IP cụ thể | 🟠 |
| B07 | WAF rules — chặn SQL injection, XSS | Rule active | 🔴 |
| B08 | Rate limiting — chặn brute force login | 5 attempts/phút/IP | 🟠 |

## C. Performance qua Tunnel

| # | Test case | Expected | Mức |
|---|---|---|---|
| C01 | Page load trung bình (Dashboard) | < 3 giây | 🟠 |
| C02 | Báo cáo BCTC > 1000 dòng | < 30 giây | 🟠 |
| C03 | Upload COA PDF 10MB | < 10 giây | 🟠 |
| C04 | Upload file lớn (50-100MB) | KHÔNG fail giữa chừng | 🔴 |
| C05 | Cloudflare 100s timeout cho long-running request | Cần move sang background job (queue) | 🔴 |
| C06 | Concurrent 20 user cùng làm việc | Không lag, không lỗi 502/504 | 🟠 |
| C07 | Export Excel 50k dòng | < 60 giây hoặc background job | 🟠 |

## D. Bảo mật

| # | Test case | Expected | Mức |
|---|---|---|---|
| D01 | Port 8069 (Odoo) KHÔNG expose ra internet trực tiếp | Chỉ qua Cloudflare | 🔴 |
| D02 | Port 5432 (PostgreSQL) chỉ accept localhost | pg_hba.conf đúng | 🔴 |
| D03 | Admin password Odoo mặc định đã đổi | Strong password | 🔴 |
| D04 | Master password Odoo đã đổi | Không phải "admin" | 🔴 |
| D05 | dbfilter set đúng để chặn truy cập DB khác | `--db-filter=^locthien_prod$` | 🔴 |
| D06 | list_db = False (production) | Không list được database | 🔴 |
| D07 | proxy_mode = True (chạy sau Cloudflare) | Đúng IP client trong log | 🔴 |
| D08 | HTTPS forced — HTTP redirect HTTPS | Cloudflare rule | 🔴 |
| D09 | Cookie Secure + HttpOnly | Browser inspector check | 🟠 |
| D10 | CSP header chống XSS | Cloudflare hoặc nginx config | 🟠 |

## E. Database (PostgreSQL)

| # | Test case | Expected | Mức |
|---|---|---|---|
| E01 | PostgreSQL version tương thích Odoo 19 | PG 14/15/16 | 🔴 |
| E02 | shared_buffers, work_mem, maintenance_work_mem tuned | Theo RAM | 🟠 |
| E03 | VACUUM autovacuum bật | Default OK với DB nhỏ | 🟠 |
| E04 | Backup pg_dump hàng ngày | Cron job | 🔴 |
| E05 | WAL archiving (PITR) | Recovery về bất kỳ phút | 🟠 |
| E06 | Replication slave (read-only) | Optional, cho báo cáo nặng | 🟢 |
| E07 | Encrypt database at rest (LUKS/dm-crypt) | Disk encryption | 🟠 |
| E08 | Audit pg_stat_activity định kỳ | Phát hiện slow query, lock | 🟡 |

## F. Backup & Disaster Recovery

| # | Test case | Expected | Mức |
|---|---|---|---|
| F01 | Backup full DB hàng ngày 2h sáng | Cron, lưu local + cloud | 🔴 |
| F02 | Backup folder filestore (file đính kèm như COA) | Đầy đủ, không quên | 🔴 |
| F03 | Backup config Odoo, addons custom | Git repo riêng | 🔴 |
| F04 | Lưu backup ở 3 nơi: local + cloud (GDrive/S3) + offline (USB) | 3-2-1 rule | 🔴 |
| F05 | Test restore định kỳ (quý/lần) | Restore thành công vào staging | 🔴 |
| F06 | Documentation quy trình restore | Bất kỳ IT mới đều làm được | 🔴 |
| F07 | DR plan: nếu mất localhost, switch sang VPS cloud | Có sẵn snapshot AMI/image | 🟠 |
| F08 | RTO mục tiêu | ≤ 4 giờ | 🟠 |
| F09 | RPO mục tiêu | ≤ 24 giờ | 🟠 |

## G. Monitoring & Alerting

| # | Test case | Expected | Mức |
|---|---|---|---|
| G01 | Uptime monitoring (UptimeRobot, BetterStack) | Alert khi down > 2 phút | 🔴 |
| G02 | CPU/RAM/Disk alert > 80% | Email/SMS/Telegram CEO | 🟠 |
| G03 | Disk full < 10% — alert sớm | Tránh DB crash | 🔴 |
| G04 | Cloudflare Tunnel status check | API monitoring | 🟠 |
| G05 | Postgres connection count > 80% max | Alert | 🟠 |
| G06 | Odoo error log monitor (errors/5 phút) | Tự collect, dashboard | 🟠 |
| G07 | Login failed > 10 lần/phút | Security alert | 🟠 |

## H. Update & Maintenance

| # | Test case | Expected | Mức |
|---|---|---|---|
| H01 | Quy trình update Odoo minor (19.0 → 19.1) | Staging test → backup → upgrade prod | 🔴 |
| H02 | Update OS (Ubuntu) — schedule downtime | Cuối tuần, có announce | 🟠 |
| H03 | Update PostgreSQL major version | Plan rõ, test ở staging | 🟠 |
| H04 | Custom modules có version control (Git) | GitHub private repo | 🔴 |
| H05 | Update custom module — không ảnh hưởng data | Migration script đúng | 🔴 |
| H06 | Rollback plan nếu update fail | Backup pre-update, restore < 1h | 🔴 |

## I. Compliance hạ tầng

| # | Quy định | Check |
|---|---|---|
| I01 | Dữ liệu cá nhân KH lưu trong VN (theo NĐ 53/2022) | Server vật lý ở VN | ✅/❌ |
| I02 | Lưu log truy cập tối thiểu 24 tháng (NĐ 53/2022) | Log retention | ✅/❌ |
| I03 | Có biên bản chỉ định người chịu trách nhiệm bảo vệ dữ liệu (DPO) | Có văn bản nội bộ | ✅/❌ |
| I04 | Đăng ký xử lý dữ liệu cá nhân (NĐ 13/2023) | Đã đăng ký với BCA | ✅/❌ |

---

## J. Câu hỏi đặc thù Cloudflare Tunnel cần test thực tế

1. **WebSocket** (Odoo bus/longpolling) qua tunnel có hoạt động không? — Cần cấu hình `--protocol http2` hoặc `quic` cho cloudflared.
2. **Long polling > 100s** có bị Cloudflare cắt không? — Free plan timeout 100s, Enterprise có thể nâng. Cân nhắc dùng **Server-Sent Events** thay long-polling.
3. **POST request > 100MB** có bị reject không? — Free plan limit 100MB. Cần workaround cho file COA lớn (split upload hoặc presigned URL).
4. **Webhook từ CQT/Ngân hàng** — IP của họ có bị Cloudflare bot detection chặn nhầm không? Cần whitelist.
5. **Browser tải file PDF qua tunnel** — có lỗi MIME type không? Verify Content-Type headers.
