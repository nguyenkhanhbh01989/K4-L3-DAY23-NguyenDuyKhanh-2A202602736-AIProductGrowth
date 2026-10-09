# DAY 23 — AI PRODUCT GROWTH
## Operating Dashboard — VFO2O-06

**Bài thực hành cá nhân | Track 1 — AI Thực Chiến | Cohort 4**

### 1. Thông tin học viên

| Thông tin | Nội dung |
|---|---|
| Họ và tên | Nguyễn Duy Khánh |
| Mã học viên (MSSV) | 2A202602736 |
| Bài lab | Day 23 — AI Product Growth |
| Sản phẩm | VFO2O-06 — EV Maintenance AI Agent |
| Chủ đề | AI Agent lập kế hoạch và hỗ trợ đặt lịch bảo dưỡng xe điện |
| Mô hình đánh giá hiện tại | B2B — Pilot |
| Mô hình kinh doanh định hướng | B2B2C |
| Thời điểm xây dựng dashboard | 09/10/2026 |
| Trạng thái dữ liệu | Mô hình giả định, chưa có đủ dữ liệu pilot thực tế |

---

### 2. Tổng quan sản phẩm

**VFO2O-06** là hệ thống AI Agent hỗ trợ chăm sóc sau bán và lập kế hoạch bảo dưỡng xe điện, hướng đến việc kết nối nhu cầu bảo dưỡng của chủ xe với năng lực tiếp nhận tại xưởng dịch vụ.

#### Vấn đề cần giải quyết

- **Đối với chủ xe:** Khó chủ động theo dõi mốc bảo dưỡng theo số km hoặc thời gian; dễ bỏ lỡ lịch bảo dưỡng và phải liên hệ xưởng thủ công.
- **Đối với xưởng:** Việc nhắc lịch, tư vấn hạng mục, tiếp nhận yêu cầu và sắp xếp ca còn tốn thời gian; khó chủ động khai thác nhu cầu bảo dưỡng từ khách hàng hiện có.

#### Giải pháp đề xuất

Hệ thống sử dụng AI Agent kết hợp dữ liệu xe, quy tắc bảo dưỡng và các công cụ nghiệp vụ để:

1. Phát hiện xe đến hoặc sắp đến hạn bảo dưỡng.
2. Đề xuất hạng mục bảo dưỡng phù hợp.
3. Tính chi phí dự kiến từ dữ liệu giá đã cấu hình.
4. Gợi ý ca bảo dưỡng khả dụng tại xưởng.
5. Cho phép chủ xe gửi yêu cầu đặt lịch.
6. Chuyển yêu cầu đến xưởng để nhân sự xác nhận.
7. Lưu lịch sử và hỗ trợ theo dõi hiệu quả vận hành.

**Nguyên tắc vận hành:** AI hỗ trợ phân tích và điều phối; các quy tắc bảo dưỡng, giá dịch vụ và quyết định xác nhận booking được kiểm soát bởi dữ liệu nghiệp vụ và con người.

---

### 3. Xác định mô hình kinh doanh

#### 3.1. Các bên tham gia

| Vai trò | Đối tượng | Giá trị nhận được |
|---|---|---|
| Khách hàng trả phí | Xưởng / trung tâm dịch vụ xe điện | Giảm thao tác thủ công, tối ưu tiếp nhận và chăm sóc khách hàng |
| Người dùng cuối | Chủ xe điện | Nhận tư vấn, nhắc lịch và đặt lịch bảo dưỡng thuận tiện |
| Người vận hành | Cố vấn dịch vụ / nhân viên xưởng | Quản lý yêu cầu, ca trống và xác nhận lịch |
| Nhà cung cấp | VFO2O-06 | Doanh thu từ gói dịch vụ và hạn mức booking |

#### 3.2. Mô hình được lựa chọn cho Day 23

**B2B — giai đoạn pilot.**

> VFO2O-06 hiện được đánh giá theo mô hình B2B vì xưởng dịch vụ là khách hàng trả phí và là đối tượng cần chứng minh giá trị vận hành đầu tiên. Mặc dù hệ thống có giao diện và luồng sử dụng dành cho chủ xe, dự án chưa có đủ bằng chứng về khả năng tiếp cận và đo lường hành vi của người dùng cuối thực tế để khẳng định mô hình B2B2C đã vận hành.

**Định hướng B2B2C:**

Khi có xưởng đối tác thật, chủ xe thật trực tiếp sử dụng sản phẩm và hệ thống thu thập được các event hợp lệ trong hành trình bảo dưỡng, dự án sẽ đánh giá chuyển sang dashboard B2B2C.

Khi đó, chỉ số ưu tiên sẽ là **Partner Activation Rate**, thay cho TTFV đang được chọn ở giai đoạn B2B.

Việc phân loại được xác định theo trạng thái có thể kiểm chứng của sản phẩm, không chỉ dựa vào kiến trúc hoặc kế hoạch kinh doanh.

---

### 4. Mô hình doanh thu và các gói dịch vụ

VFO2O-06 dự kiến triển khai mô hình **Tiered Subscription Pricing kết hợp Usage-Based Pricing**.

Chủ xe sử dụng các tính năng hỗ trợ bảo dưỡng miễn phí. Xưởng đăng ký gói trả phí dựa trên quy mô và số booking được xác nhận.

| Gói | Giá dự kiến / tháng | Hạn mức |
|---|---:|---|
| Trải nghiệm | Miễn phí | 30 ngày hoặc 10 booking xác nhận |
| Khởi động | 2.990.000 VNĐ | 35 booking/tháng |
| Tăng trưởng | 4.690.000 VNĐ | 160 booking/tháng |
| Chuyên nghiệp | 8.990.000 VNĐ | 350 booking/tháng |

**Gói Tăng trưởng** được lựa chọn làm kịch bản kinh tế tham chiếu trong bài lab.

#### Quy tắc xác định booking

- `booking_request_created`: Chủ xe đã tạo yêu cầu đặt lịch; đây là Core Action trong hành trình người dùng.
- `booking_confirmed`: Yêu cầu hợp lệ đã được xưởng xác nhận; đây là đơn vị kết quả phục vụ tính chi phí và hạn mức thu phí.
- Booking thử nghiệm, bị từ chối hoặc trùng lặp không được tính là booking thu phí.

Các mức giá và hạn mức hiện tại là **giả thuyết thương mại**, chưa phải bảng giá đã được thị trường kiểm chứng.

---

### 5. Số liệu đầu vào cho Operating Dashboard

Sử dụng dữ liệu từ bài mô hình tài chính Day 22.

| Chỉ số | Giá trị tham chiếu | Trạng thái |
|---|---:|---|
| ARPU giả định | 4.690.000 VNĐ/xưởng/tháng | Theo gói Tăng trưởng |
| Gross Margin mô hình Day 22 | Khoảng 69,4% | Dự phóng |
| Gross Margin mục tiêu tối thiểu | 60% | Ràng buộc bài lab |
| Cost per Completed Job | Khoảng 7.998 VNĐ | Mô hình Day 22 ban đầu |
| CAC theo kịch bản founder-led sales | 32.000.000 VNĐ/xưởng | Giả định |
| CAC tối đa theo payback 12 tháng | Khoảng 39.070.000 VNĐ/xưởng | Tính từ mô hình |
| CAC Payback mục tiêu | Không quá 12 tháng | Mục tiêu |
| Runway | Chưa xác định | Thiếu số dư vốn và net burn thực tế |

**Lưu ý về tính nhất quán:** Mô hình Day 22 ban đầu sử dụng mức giá 4.690.000 VNĐ với hạn mức 190 booking. Phương án đóng gói mới sử dụng 160 booking. Vì vậy, các ngưỡng liên quan đến Cost/Job, COGS và Gross Margin của từng gói phải được kiểm tra lại trước khi thương mại hóa.

Không trình bày số liệu giả định là kết quả kinh doanh đã đạt được.

---

### 6. North Star và Operating Dashboard

#### North Star: Time-to-First-Value (TTFV)

**Định nghĩa:** Số ngày kể từ khi xưởng bắt đầu vận hành hợp lệ (`workshop_go_live`) đến khi có booking bảo dưỡng đầu tiên từ khách hàng thật được xưởng xác nhận (`booking_confirmed`).

**Không tính:** Booking nội bộ, booking demo, dữ liệu test hoặc booking trùng lặp.

**Lý do lựa chọn:** Nếu xưởng chưa nhìn thấy giá trị thực tế từ hệ thống, việc mở rộng số xưởng đăng ký hoặc tối ưu doanh thu chưa phản ánh được khả năng tăng trưởng bền vững.

#### Hệ thống 8 đèn theo 3 tầng

| Tầng | Mã | Chỉ số |
|---|---|---|
| Leading | L1 | Time-to-First-Value |
| Leading | L2 | Workshop Setup Completion Rate |
| Leading | L3 | Vehicle Data Readiness Rate |
| Operating | O1 | AI Completion Rate |
| Operating | O2 | Booking Confirmation Rate |
| Operating | O3 | Cost per Confirmed Job |
| Operating | O4 | Projected Customer Acquisition Cost |
| Lagging | G1 | Gross Margin |

Dashboard được thiết kế để phát hiện vấn đề từ sớm, trước khi các chỉ số tài chính cuối kỳ phản ánh sự suy giảm.

Chi tiết định nghĩa, công thức, nhịp đo và nguồn của từng ngưỡng nằm trong `worksheet.md`.

---

### 7. Nguyên tắc đặt ngưỡng và ra quyết định

Mỗi đèn được phân thành ba trạng thái:

- **🟢 Xanh:** Đạt vùng vận hành kỳ vọng.
- **🟡 Vàng:** Có tín hiệu rủi ro, cần xác minh và xử lý theo thời hạn.
- **🔴 Đỏ:** Kích hoạt luật quyết định đã xác định trước.

Các ngưỡng sử dụng một trong ba nhóm nguồn:

| Ký hiệu | Ý nghĩa |
|---|---|
| `[BM]` | Benchmark có nguồn công bố và ngày kiểm tra |
| `[MH]` | Ngưỡng được tính từ mô hình tài chính của sản phẩm |
| `[TB]` | Ngưỡng dựa trên dữ liệu/baseline nội bộ; nếu chưa có dữ liệu thì ghi rõ là mục tiêu thử nghiệm và lịch thu thập |

Trong giai đoạn hiện tại, nhiều ngưỡng `[TB]` chưa có baseline đo thực tế. Chúng được sử dụng như giả thuyết để kiểm chứng trong pilot.

Bài lab xây dựng **5 luật quyết định**, trong đó có ít nhất **2 luật dừng**, nhằm tránh tiếp tục mở rộng sản phẩm hoặc chi tiêu khi các giả định quan trọng không còn đúng.

---

### 8. Kế hoạch kiểm chứng trong 90 ngày

Mốc dự kiến bắt đầu: **09/10/2026**.

| Cổng gác | Thời điểm | Chỉ số kiểm chứng chính | Bằng chứng |
|---|---|---|---|
| D30 — Learn | 08/11/2026 | Ít nhất 2/3 xưởng pilot hoàn thành thiết lập | Log go-live, checklist onboarding |
| D60 — Validate | 08/12/2026 | TTFV trung vị ≤14 ngày trên ít nhất 2 xưởng đủ điều kiện | Booking log, timestamp, xưởng xác nhận |
| D90 — Decide | 07/01/2027 | Gross Margin ≥66,7% trong phạm vi kiểm chứng | Doanh thu, usage log, COGS, đối soát chi phí |

Tại mỗi cổng gác, dự án lựa chọn một trong bốn quyết định: **GO / FIX / PIVOT / KILL**.

**Kill criteria:** Đến ngày 07/01/2027, nếu sau một chu kỳ sửa chữa mà ít nhất 2 trong 3 xưởng pilot vẫn mất trên 30 ngày để có booking thật đầu tiên, dự án sẽ dừng thương mại hóa gói hiện tại cho phân khúc đang thử nghiệm và đánh giá lại giả thuyết sản phẩm.

Các cổng gác là kế hoạch kiểm chứng, không phải kết quả đã hoàn thành.

---

### 9. Giới hạn dữ liệu và tính minh bạch

Tại thời điểm xây dựng bài lab:

- Chưa có đủ dữ liệu pilot với các xưởng trả phí thực tế.
- Chưa xác minh TTFV và tỷ lệ kích hoạt bằng dữ liệu khách hàng thật.
- Cost/Job và Gross Margin chủ yếu đến từ mô hình tài chính giả định.
- CAC chưa được kiểm chứng trên chu kỳ bán hàng thực tế.
- Runway chưa thể xác định nếu thiếu dữ liệu vốn khả dụng và mức tiêu tiền hàng tháng.
- Những lợi ích như tăng booking, giảm thời gian xử lý và tăng tỷ lệ quay lại cần được đo bằng baseline và pilot.

**Nguyên tắc:** Không làm đẹp dashboard bằng số liệu tự tạo. Chỉ số chưa có dữ liệu được ghi `N/A — Chưa đo được`, kèm nguồn dữ liệu cần thu thập và thời hạn đo.

---

### 10. Cấu trúc repository

```text
K4-L3-DAY23-NguyenDuyKhanh-2A202602736-AIProductGrowth/
├── README.md
├── worksheet.md
├── dashboard.md
└── dashboard.pdf
```

| File | Nội dung |
|---|---|
| `README.md` | Tổng quan sản phẩm, học viên, mô hình kinh doanh, dữ liệu đầu vào |
| `worksheet.md` | Kết quả Trạm 1–4: phân loại mô hình, thẻ đèn, ngưỡng, phép tính [MH], 5 luật |
| `dashboard.md` | Operating Dashboard cô đọng trên một trang |
| `dashboard.pdf` | Bản trình bày để chấm, tối đa 2 trang kể cả phụ lục |

**Thứ tự đánh giá được đề xuất:** `dashboard.pdf` → `worksheet.md` → `README.md`.

---

### 11. Tài liệu tham khảo

1. [Day 23 — AI Product Growth (Repo đề bài)](https://github.com/VinUni-AI20k/K4-L3-Day23-AI-Product-Growth)
2. `HANDBOOK.md` — Hướng dẫn Leading / Operating / Lagging, thresholds, decision rules và 90-day gates.
3. `RUBRIC.md` — Tiêu chí đánh giá và quy định mất điểm.
4. Mô hình tài chính cá nhân Day 22 — Cost/Job, Pricing, CAC, Gross Margin.
5. Bài Value Metric Day 20 — Định nghĩa Core Action và hành trình người dùng.

---

### 12. Kết luận

Operating Dashboard của VFO2O-06 được xây dựng nhằm trả lời ba câu hỏi:

1. **Sản phẩm có tạo ra giá trị đủ sớm cho xưởng không?**
2. **AI có xử lý hiệu quả mà vẫn kiểm soát được chi phí không?**
3. **Khi giả thuyết tăng trưởng hoặc kinh tế thất bại, nhóm sẽ dừng hay thay đổi điều gì?**

Mục tiêu của bài lab không phải tạo ra một dashboard có nhiều chỉ số, mà là xây dựng **một hệ thống cảnh báo và ra quyết định có thể kiểm chứng**, giúp dự án phát triển dựa trên dữ liệu thay vì cảm tính.

---

**Người thực hiện:** Nguyễn Duy Khánh  
**MSSV:** 2A202602736  
**Sản phẩm:** VFO2O-06 — EV Maintenance AI Agent  
**Lab:** Day 23 — AI Product Growth