# WORKSHEET — DAY 23: AI PRODUCT GROWTH

## Operating Dashboard — VFO2O-06

**Học viên:** Nguyễn Duy Khánh  
**MSSV:** 2A202602736  
**Sản phẩm:** VFO2O-06 — EV Maintenance AI Agent  
**Chủ đề:** AI Agent lập kế hoạch, nhắc lịch và hỗ trợ đặt lịch bảo dưỡng xe điện  
**Mô hình đánh giá:** B2B — Pilot  
**Mô hình kinh doanh mục tiêu:** B2B2C  
**Ngày lập:** 09/10/2026  
**Trạng thái:** Mô hình thử nghiệm, chưa có đủ dữ liệu pilot thực tế

> **Nguyên tắc dữ liệu:** Phân biệt số thực đo, số mô phỏng và mục tiêu thử nghiệm. Chỉ số chưa có log hợp lệ được đánh dấu `N/A — Chưa đo được`, không tự động gán trạng thái xanh.

---

# 0. DỮ LIỆU ĐẦU VÀO — DAY 22

## 0.1. Mô hình kinh doanh và đơn vị giá trị

VFO2O-06 sử dụng mô hình **Tiered Subscription Pricing + Usage-Based Pricing**.

- **Khách hàng trả phí:** Xưởng hoặc trung tâm dịch vụ xe điện.
- **Người dùng cuối:** Chủ xe điện.
- **Đơn vị giá trị phía chủ xe:** `booking_request_created`.
- **Đơn vị tính phí phía xưởng:** `booking_confirmed`.
- **Nguồn doanh thu dự kiến:** Phí thuê bao tháng/quý/năm và phí booking vượt hạn mức được xưởng chấp thuận.

`booking_request_created` và `booking_confirmed` là hai sự kiện khác nhau. Chỉ booking hợp lệ đã được xưởng xác nhận mới được tính vào hạn mức thu phí.

## 0.2. Bảng giá đang thử nghiệm

| Gói | Giá/tháng | Booking xác nhận/tháng |
|---|---:|---:|
| Trải nghiệm | 0đ | 10 booking hoặc 30 ngày |
| Khởi động | 2.990.000đ | 35 |
| Tăng trưởng | 4.690.000đ | 160 |
| Chuyên nghiệp | 8.990.000đ | 350 |

**Gói tham chiếu để lập dashboard:** Tăng trưởng.

Lý do: Đây là gói thể hiện đầy đủ hơn giá trị AI chăm sóc bảo dưỡng chủ động và là mức giá trung tâm được dùng trong mô hình kinh doanh dự kiến.

## 0.3. Số liệu tài chính đầu vào

| Chỉ số | Giá trị | Loại dữ liệu |
|---|---:|---|
| ARPU tham chiếu | 4.690.000đ/xưởng/tháng | Giả định theo giá gói |
| GM mô hình Day 22 ban đầu | 69,42% | Mô phỏng |
| GM mục tiêu tối thiểu | 60% | Ràng buộc lab |
| Cost/Job mô hình Day 22 ban đầu | 7.998đ | Mô phỏng |
| CAC dự kiến | 32.000.000đ/xưởng | Giả định founder-led sales |
| CAC payback mục tiêu | ≤12 tháng | Mục tiêu |
| Runway | N/A | Chưa có vốn và net burn được xác minh |

**Lưu ý thay đổi phiên bản:** Mô hình Day 22 ban đầu giả định gói Tăng trưởng bao gồm 190 booking/tháng. Bảng giá đang đề xuất sử dụng 160 booking/tháng, nên phải kiểm tra lại COGS theo quota mới.

Theo kịch bản chi phí đã phân bổ lại cho 160 booking:

- Chi phí cố định phân bổ: khoảng **767.500đ/xưởng/tháng**.
- Chi phí biến đổi: khoảng **3.717đ/booking xác nhận** trong kịch bản mô phỏng.
- COGS tháng: khoảng **1.362.249đ** khi dùng đủ 160 booking.
- Cost/Job quy đổi: khoảng **8.514đ/booking**.
- GM quy đổi: khoảng **70,95%**.

Các con số này dùng để đặt ngưỡng thử nghiệm, chưa được coi là chi phí đã xác minh.

## 0.4. Các giả định cần kiểm chứng

1. Xưởng có đủ dữ liệu xe và quyền liên hệ khách hàng để vận hành luồng nhắc bảo dưỡng.
2. AI có thể tạo yêu cầu đặt lịch hợp lệ với tỷ lệ mục tiêu 78%.
3. Xưởng xác nhận khoảng 92% yêu cầu đặt lịch hợp lệ.
4. COGS không vượt quá mức cho phép khi sản lượng tăng.
5. Xưởng nhìn thấy giá trị đủ sớm để sẵn sàng sử dụng và trả phí.
6. Giá trị kinh tế tạo ra đủ lớn để hỗ trợ mức giá thuê bao.
7. Chi phí thu hút khách hàng có thể hoàn vốn trong tối đa 12 tháng.

---

# TRẠM 1 — CHỐT LOẠI MÔ HÌNH VÀ KIỂM KÊ CHỈ SỐ

**Thời lượng:** 15 phút.

## 1.1. Phân loại mô hình

### Câu hỏi 1: Ai trả tiền?

Khách hàng trả tiền cho VFO2O-06 là các xưởng dịch vụ hoặc trung tâm bảo dưỡng xe điện.

Xưởng có thể đăng ký gói tháng, quý hoặc năm và thanh toán thêm nếu chủ động sử dụng vượt hạn mức đã đăng ký.

Chủ xe không phải đối tượng trả phí trong mô hình dự kiến.

### Câu hỏi 2: Ai sử dụng sản phẩm?

Sản phẩm có hai nhóm người dùng:

**Chủ xe điện:**

- Xem thông tin và lịch sử xe.
- Nhận nhắc lịch bảo dưỡng.
- Xem hạng mục và dự toán.
- Gửi yêu cầu đặt lịch.

**Nhân viên xưởng:**

- Tiếp nhận và kiểm tra booking request.
- Xác nhận hoặc điều chỉnh ca.
- Theo dõi xe, lịch hẹn và lịch sử bảo dưỡng.
- Sử dụng dashboard để quản lý hoạt động.

### Câu hỏi 3: Nhóm có tiếp cận và đo lường người dùng cuối thật không?

Tại thời điểm lập worksheet, sản phẩm đã có định hướng giao diện và luồng nghiệp vụ dành cho chủ xe, nhưng chưa có đủ bằng chứng về việc chủ xe ngoài nhóm thực sự sử dụng hệ thống và phát sinh các sự kiện được theo dõi hợp lệ.

Vì vậy, chưa đủ cơ sở khẳng định mô hình B2B2C đã vận hành trong thực tế.

## 1.2. Một câu chốt mô hình

> **VFO2O-06 được đánh giá theo mô hình B2B trong giai đoạn pilot vì xưởng dịch vụ là đơn vị trả phí và là nơi đầu tiên cần chứng minh giá trị vận hành, trong khi khả năng tiếp cận và đo lường người dùng cuối thực tế vẫn chưa được xác minh; định hướng dài hạn là B2B2C.**

**Quyết định:** Sử dụng bảng đèn **B2B** cho bài Day 23.

**Đèn ưu tiên:** Time-to-First-Value (TTFV).

**Điều kiện chuyển sang B2B2C:** Có ít nhất một xưởng đối tác và chủ xe thật sử dụng trực tiếp sản phẩm, cùng dữ liệu event có thể đối soát để xác minh việc kích hoạt người dùng cuối.

Khi điều kiện này được đáp ứng, North Star dự kiến chuyển sang Partner Activation Rate.

---

## 1.3. Kiểm kê các chỉ số ứng viên

Quy ước:

- ✅ Có thể lấy dữ liệu hiện tại hoặc từ mô hình đã xây dựng.
- 🔧 Chưa có số thực tế nhưng có kế hoạch đo rõ ràng trong 2 tuần.
- ❌ Chưa có đủ dữ liệu hoặc chưa xác định được cách thu thập đáng tin cậy.

**Lưu ý:** ✅ đối với số tài chính mô phỏng chỉ có nghĩa là đã có mô hình tính, không đồng nghĩa có dữ liệu thực tế.

| Chỉ số ứng viên | Nhóm | Trạng thái | Nguồn hoặc dữ liệu cần bổ sung |
|---|---|---|---|
| Time-to-First-Value | Leading | 🔧 | `workshop_go_live`, `booking_confirmed` |
| Workshop Setup Completion Rate | Leading | 🔧 | Checklist onboarding, trạng thái cấu hình |
| Vehicle Data Readiness Rate | Leading | 🔧 | Dữ liệu xe và kết quả validation |
| AI Completion Rate | Operating | 🔧 | LangGraph trace, kết quả tạo booking request |
| Booking Confirmation Rate | Operating | 🔧 | Booking status transition log |
| Cost per Confirmed Job | Operating | 🔧 | LLM usage, retry, infra, HITL, số booking |
| Projected CAC | Operating | ✅ mô hình | Sales budget, opportunity, win rate giả định |
| Gross Margin | Lagging | ✅ mô hình | Pricing/COGS worksheet; thực tế chưa đo |
| Sales Cycle Length | Operating | ❌ | CRM opportunity và hợp đồng thật |
| Trial-to-Paid Conversion | Operating | ❌ | Trial cohort và giao dịch trả phí thật |
| Customer Retention / Churn | Lagging | ❌ | Lịch sử gia hạn qua nhiều kỳ |
| Net Revenue Retention | Lagging | ❌ | Revenue cohort nhiều tháng |
| CAC Payback Actual | Lagging | ❌ | CAC thật và lợi nhuận gộp thật |
| Runway | Lagging / Finance | ❌ | Tiền khả dụng và net burn |
| LTV/CAC Actual | Lagging | ❌ | Retention, GM và CAC thực tế |

**Kết luận Trạm 1:**

Chọn 8 chỉ số tập trung vào việc xưởng có nhận được giá trị, AI có tạo ra kết quả vận hành và mô hình có bảo toàn được biên lợi nhuận hay không.

Không đưa LTV, NPV, IRR hoặc NRR vào nhóm đèn chính vì sản phẩm chưa có chu kỳ dữ liệu đủ dài để sử dụng các chỉ số này như tín hiệu điều hành hàng tuần.

---

# TRẠM 2 — NORTH STAR VÀ CÂY CHỈ SỐ 3 TẦNG

**Thời lượng:** 25 phút.

## 2.1. North Star

### Time-to-First-Value (TTFV)

**Định nghĩa:**

Số ngày kể từ thời điểm xưởng chính thức go-live đến thời điểm xưởng xác nhận booking hợp lệ đầu tiên được tạo từ khách hàng thật thông qua sản phẩm.

**Công thức cho từng xưởng:**

`TTFV = Ngày booking_confirmed đầu tiên - Ngày workshop_go_live`

**Chỉ số tổng hợp:** Trung vị TTFV của các xưởng đủ điều kiện đo.

**Loại trừ:**

- Booking tạo bởi tài khoản nội bộ.
- Booking sử dụng dữ liệu demo.
- Booking trùng lặp.
- Booking bị từ chối hoặc hủy trước xác nhận.
- Xưởng chưa có đủ thời gian quan sát để kết luận vượt ngưỡng.

**Lý do lựa chọn:**

Đối với mô hình B2B, việc đăng ký hoặc thiết lập phần mềm chưa chứng minh xưởng đã nhận giá trị. TTFV cho thấy xưởng mất bao lâu để đạt được kết quả vận hành đầu tiên.

TTFV càng ngắn, xưởng càng sớm có cơ sở đánh giá việc tiếp tục dùng sản phẩm.

**Mục tiêu thử nghiệm:** TTFV ≤14 ngày.

**Trạng thái hiện tại:** N/A — Chưa có cohort xưởng pilot thực tế đủ điều kiện.

---

## 2.2. Sơ đồ 3 tầng

```text
                    NORTH STAR
              Time-to-First-Value
                         │
                         ▼
┌───────────────────────────────────────────────┐
│ TẦNG 1 — LEADING                             │
│ L1. Time-to-First-Value                      │
│ L2. Workshop Setup Completion Rate           │
│ L3. Vehicle Data Readiness Rate              │
└───────────────────────────────────────────────┘
                         │
                         ▼
┌───────────────────────────────────────────────┐
│ TẦNG 2 — OPERATING                           │
│ O1. AI Completion Rate                       │
│ O2. Booking Confirmation Rate                │
│ O3. Cost per Confirmed Job                   │
│ O4. Projected CAC                            │
└───────────────────────────────────────────────┘
                         │
                         ▼
┌───────────────────────────────────────────────┐
│ TẦNG 3 — LAGGING                             │
│ G1. Gross Margin                             │
└───────────────────────────────────────────────┘
```

Cây được hiểu theo các chuỗi nhân quả chính:

1. Setup và chất lượng dữ liệu tốt hơn → AI tạo yêu cầu hợp lệ sớm hơn → TTFV giảm.
2. AI Completion và Confirmation Rate tốt hơn → có nhiều booking hợp lệ hơn trên cùng chi phí xử lý → Cost/Job được cải thiện.
3. Cost/Job và CAC được kiểm soát → Gross Margin và khả năng hoàn vốn được bảo vệ.

TTFV là North Star nằm ở tầng Leading; không tính thành đèn thứ chín.

---

## 2.3. Tám thẻ đèn chi tiết

### L1 — Time-to-First-Value

**Tầng:** Leading  
**Owner đề xuất:** Product / Pilot Lead

**Định nghĩa:** Số ngày từ lúc xưởng go-live đến booking hợp lệ đầu tiên được xưởng xác nhận.

**Công thức:**

`TTFV = first_confirmed_booking_at - workshop_go_live_at`

Dùng trung vị khi tổng hợp nhiều xưởng.

**Nhịp đo:** Hàng tuần, theo cohort xưởng.

**Nguồn:** Onboarding log và booking event.

**Báo trước cho:** Trial-to-Paid Conversion và khả năng tạo doanh thu/xưởng.

**Khi đỏ:** Dừng mở rộng xưởng pilot, ưu tiên rút ngắn quy trình đưa xưởng đến booking thật đầu tiên.

**Trạng thái:** 🔧 Chưa đo được.

### L2 — Workshop Setup Completion Rate

**Tầng:** Leading  
**Owner đề xuất:** Frontend / Onboarding

**Định nghĩa:** Tỷ lệ xưởng hoàn thành các điều kiện thiết lập bắt buộc trong 7 ngày đầu kể từ khi bắt đầu onboarding.

**Công thức:**

`Setup Rate = Xưởng hoàn thành setup trong 7 ngày / Xưởng đã bắt đầu onboarding đủ 7 ngày × 100%`

Một xưởng hoàn thành setup khi có tối thiểu:

- Thông tin xưởng hợp lệ.
- Tài khoản nhân viên phù hợp.
- Cấu hình ca và năng lực tiếp nhận.
- Dữ liệu dịch vụ và giá cần thiết.
- Kiểm thử thành công luồng tiếp nhận booking.

Không tính xưởng mới onboarding chưa đủ 7 ngày vào mẫu kết luận.

**Nhịp đo:** Hàng tuần.

**Nguồn:** Checklist onboarding và trạng thái cấu hình.

**Báo trước cho:** L1 — TTFV.

**Khi đỏ:** Thu hẹp các bước setup và hỗ trợ cấu hình thủ công cho pilot.

**Trạng thái:** 🔧 Chưa đo được.

### L3 — Vehicle Data Readiness Rate

**Tầng:** Leading  
**Owner đề xuất:** Backend / Data

**Định nghĩa:** Tỷ lệ hồ sơ xe đủ dữ liệu để hệ thống đánh giá bảo dưỡng và sử dụng đúng mục đích được phép.

**Công thức:**

`Data Readiness = Số hồ sơ xe hợp lệ / Tổng hồ sơ xe thuộc phạm vi đánh giá × 100%`

Hồ sơ hợp lệ phải có:

- Vehicle ID và model.
- Odometer hợp lệ.
- Mốc thời gian cần thiết để đánh giá bảo dưỡng.
- Lịch sử bảo dưỡng cần thiết hoặc trạng thái thiếu lịch sử được xử lý rõ ràng.
- Quyền xử lý/liên hệ dữ liệu phù hợp với luồng sử dụng.

Không tính dữ liệu mẫu nội bộ vào dữ liệu pilot khách hàng thật.

**Nhịp đo:** Hàng tuần.

**Nguồn:** Vehicle database, validation log và dữ liệu quyền liên hệ.

**Báo trước cho:** O1 — AI Completion Rate và TTFV.

**Khi đỏ:** Dừng gửi nhắc lịch trên hồ sơ không đủ điều kiện, bổ sung validation và làm sạch dữ liệu.

**Trạng thái:** 🔧 Chưa đo được.

### O1 — AI Completion Rate

**Tầng:** Operating  
**Owner đề xuất:** Agent / RAG

**Định nghĩa:** Tỷ lệ case được AI xử lý đến kết quả tạo booking request hợp lệ, không yêu cầu can thiệp ngoại lệ của đội kỹ thuật.

**Công thức:**

`AI Completion = Case tạo booking request hợp lệ / Tổng case AI attempted × 100%`

**Loại trừ khỏi tử số:**

- Case lỗi workflow.
- Case thiếu grounding quan trọng.
- Case tạo yêu cầu không hợp lệ.
- Case phải chuyển sang xử lý ngoại lệ.
- Case retry không tạo ra kết quả cuối.

Mỗi case attempted chỉ tính một lần ở mẫu số, kể cả có nhiều lần retry.

Việc khách chủ động không tiếp tục đặt lịch cần được gắn trạng thái riêng để phân tích; không tự động ghi là lỗi kỹ thuật. Định nghĩa trên là **end-to-end booking-request yield** theo kịch bản Day 22, không phải tỷ lệ trả lời đúng của LLM.

**Nhịp đo:** Hàng tuần.

**Nguồn:** LangGraph trace và booking request log.

**Báo trước cho:** O3 — Cost per Confirmed Job.

**Khi đỏ:** Thu hẹp phạm vi tự động hóa, kiểm tra RAG và fallback.

**Trạng thái:** 🔧 Chưa đo được.

### O2 — Booking Confirmation Rate

**Tầng:** Operating  
**Owner đề xuất:** Backend / Workshop Operations

**Định nghĩa:** Tỷ lệ booking request hợp lệ được nhân viên xưởng xác nhận.

**Công thức:**

`Confirmation Rate = Booking confirmed / Booking request hợp lệ đã có quyết định cuối × 100%`

Chỉ tính booking request có quyết định xác nhận hoặc từ chối trong cửa sổ đánh giá. Các yêu cầu còn pending được báo cáo riêng để tránh đánh giá sai do chưa đủ thời gian xử lý.

**Không tính:**

- Booking demo.
- Booking trùng lặp.
- Yêu cầu không hợp lệ.
- Booking chưa có quyết định cuối trong mẫu kết luận.

**Nhịp đo:** Hàng tuần.

**Nguồn:** Booking state transition log.

**Báo trước cho:** O3 — Cost per Confirmed Job và G1 — Gross Margin.

**Khi đỏ:** Kiểm tra năng lực xưởng, slot và nguyên nhân từ chối booking.

**Trạng thái:** 🔧 Chưa đo được.

### O3 — Cost per Confirmed Job

**Tầng:** Operating  
**Owner đề xuất:** Backend / Finance

**Định nghĩa:** Chi phí phục vụ trực tiếp, bao gồm chi phí của cả các case không thành công, phân bổ trên số booking thực được xác nhận.

**Công thức:**

`Cost/Job = Tổng COGS trực tiếp trong kỳ / Số booking confirmed hợp lệ trong kỳ`

COGS bao gồm:

- LLM/API và retrieval.
- Retry và xử lý lỗi.
- Hạ tầng phục vụ.
- Chi phí HITL/QA trực tiếp.
- Chi phí thông báo bên thứ ba nếu phát sinh.
- Phần chi phí cố định trực tiếp được phân bổ nhất quán.

Không bỏ các case thất bại khỏi tổng chi phí.

**Nhịp đo:** Hàng tuần, ưu tiên rolling 30 ngày.

**Nguồn:** Usage logs, bảng giá model, hạ tầng, HITL ledger và booking ledger.

**Báo trước cho:** G1 — Gross Margin.

**Khi đỏ:** Dừng mở rộng usage mới, tối ưu workflow và điều chỉnh hạn mức.

**Trạng thái:** 🔧 Chưa đo được thực tế; đã có mô hình chi phí dự phóng.

### O4 — Projected Customer Acquisition Cost

**Tầng:** Operating  
**Owner đề xuất:** Business / Sales

**Định nghĩa:** Chi phí dự kiến để có thêm một xưởng trả phí, tính từ chi phí thu hút và tỷ lệ chuyển đổi của pipeline.

**Công thức:**

`Projected CAC = Chi phí sales + marketing + trial acquisition dự kiến / Số xưởng trả phí dự kiến`

Không nhầm CAC dự phóng với CAC actual.

**Nhịp đo:** Hàng tháng.

**Nguồn:** Founder-led sales budget, CRM pipeline và win-rate giả định.

**Báo trước cho:** CAC Payback và khả năng mở rộng thương mại.

**Khi đỏ:** Dừng acquisition có trả phí và kiểm tra hiệu quả kênh.

**Trạng thái:** ✅ Có mô hình giả định; actual chưa đo.

### G1 — Gross Margin

**Tầng:** Lagging  
**Owner đề xuất:** Finance / Product Lead

**Định nghĩa:** Tỷ lệ doanh thu còn lại sau khi trừ các chi phí trực tiếp cung cấp dịch vụ.

**Công thức:**

`GM = (Doanh thu thuần - COGS trực tiếp) / Doanh thu thuần × 100%`

Doanh thu không bao gồm VAT thu hộ, hoàn tiền và các khoản không thuộc doanh thu sản phẩm.

COGS phải bao gồm chi phí AI và vận hành trực tiếp được phân bổ nhất quán, kể cả phần chi phí trial nếu thuộc phạm vi đánh giá.

**Nhịp đo:** Hàng tháng.

**Nguồn:** Billing ledger, usage/cost ledger và dữ liệu kế toán.

**Báo trước cho:** Runway, khả năng duy trì hoạt động và quyết định mở rộng quy mô.

**Khi đỏ:** Đóng băng mở rộng thương mại và giảm chi phí trực tiếp hoặc điều chỉnh mô hình gói.

**Trạng thái:** ✅ Có mô hình; actual chưa đo.

---

## 2.4. Kiểm tra Tier Discipline

| Tiêu chí | Kết quả | Đánh giá |
|---|---:|---|
| Tổng đèn | 8 | Đạt |
| Leading | 3 | Đạt yêu cầu ≥2 |
| Operating | 4 | Có đòn bẩy vận hành |
| Lagging | 1 | Đạt yêu cầu ≤3 |
| Đèn chi phí AI | O3 | Có |
| North Star | TTFV | Phù hợp B2B pilot |
| Đèn báo trước tầng dưới | Có liên kết nhân quả | Đạt về thiết kế |

**Kết luận:** Dashboard ưu tiên tín hiệu sớm và khả năng hành động thay vì chỉ báo cáo kết quả tài chính cuối kỳ.

---

# TRẠM 3 — ĐẶT NGƯỠNG VÀ CHỨNG MINH NGUỒN

**Thời lượng:** 30 phút.

## 3.1. Quy ước nguồn

| Nguồn | Ý nghĩa | Cách sử dụng |
|---|---|---|
| `[BM]` | Benchmark công khai | Có URL, tổ chức, năm và ngày kiểm tra |
| `[MH]` | Model-derived threshold | Có số đầu vào, công thức và kết quả |
| `[TB]` | Baseline nội bộ | Có số lịch sử hoặc kế hoạch đo baseline rõ ràng |

Trong worksheet này:

- Chưa sử dụng ngưỡng `[BM]` vì chưa có benchmark đủ tương đồng và được xác minh cho xưởng bảo dưỡng xe điện sử dụng AI Agent.
- Ba đèn O3, O4 và G1 dùng `[MH]`.
- Các đèn còn lại dùng `[TB]` ở dạng **ngưỡng pilot tạm thời**, chưa phải baseline lịch sử.

Các ngưỡng `[TB]` sẽ được đánh giá lại khi có tối thiểu hai chu kỳ dữ liệu.

**Lịch dự kiến:**

- 23/10/2026: Hoàn thiện instrumentation.
- 08/11/2026: Có snapshot baseline đầu tiên.
- 08/12/2026: Có hai chu kỳ đo để rà soát lại ngưỡng.

---

## 3.2. Bảng 8 đèn xanh — vàng — đỏ

| ID | Chỉ số | 🟢 Xanh | 🟡 Vàng | 🔴 Đỏ | Nguồn |
|---|---|---|---|---|---|
| L1 | TTFV | ≤14 ngày | >14–30 ngày | >30 ngày | `[TB]` |
| L2 | Workshop Setup Rate | ≥80% | ≥50% đến <80% | <50% | `[TB]` |
| L3 | Vehicle Data Readiness | ≥90% | ≥70% đến <90% | <70% | `[TB]` |
| O1 | AI Completion | ≥78% | ≥70% đến <78% | <70% | `[TB]` |
| O2 | Booking Confirmation | ≥92% | ≥85% đến <92% | <85% | `[TB]` |
| O3 | Cost/Job | ≤8.514đ | >8.514đ đến ≤9.771đ | >9.771đ | `[MH-01]` |
| O4 | Projected CAC | ≤32 triệu | >32 đến ≤39,07 triệu | >39,07 triệu | `[MH-02]` |
| G1 | Gross Margin | ≥66,67% | ≥60% đến <66,67% | <60% | `[MH-03]` |

**Quy tắc quan trọng:** Màu chỉ được gán khi chỉ số có mẫu đủ điều kiện và dữ liệu đối soát được. Nếu chưa đủ dữ liệu, trạng thái hiển thị là `N/A`, không ép thành xanh/vàng/đỏ.

Các ngưỡng Cost/Job ở bảng này áp dụng cho **kịch bản gói Tăng trưởng với sản lượng 160 booking xác nhận/tháng và phân bổ chi phí đã nêu**. Khi sản lượng hoặc cơ cấu gói thay đổi đáng kể, phải tính lại ngưỡng thay vì dùng cố định 9.771đ.

---

## 3.3. Lý do cho từng ngưỡng

### L1 — TTFV [TB]

- **Xanh ≤14 ngày:** Xưởng thấy booking thật đầu tiên trong nửa đầu thời gian trial 30 ngày, còn thời gian đánh giá trước khi quyết định trả phí.
- **Vàng 15–30 ngày:** Giá trị đến chậm và có nguy cơ không đủ thời gian chứng minh trước khi trial kết thúc.
- **Đỏ >30 ngày:** Xưởng đã hết một chu kỳ trải nghiệm mà vẫn chưa có booking thật đầu tiên.

Đây là ngưỡng thử nghiệm, chưa dựa trên baseline đã quan sát.

**Kế hoạch baseline:** Đo TTFV trên các xưởng go-live thật; ghi cả xưởng chưa có booking, không chỉ xưởng thành công. Xác định lại sau hai chu kỳ pilot.

### L2 — Setup Rate [TB]

- **Xanh ≥80%:** Phần lớn xưởng đủ điều kiện đã hoàn thành setup trong 7 ngày.
- **Vàng 50–<80%:** Onboarding còn ma sát, cần hỗ trợ.
- **Đỏ <50%:** Hơn một nửa xưởng không hoàn thành điều kiện sử dụng sau 7 ngày; chưa nên tăng số xưởng mới.

Các mốc là tiêu chí quản trị pilot, không phải benchmark ngành.

**Kế hoạch baseline:** Đo hai cohort onboarding theo tuần và đánh giá nguyên nhân chưa hoàn thành.

### L3 — Data Readiness [TB]

- **Xanh ≥90%:** Đa số dữ liệu sẵn sàng cho xử lý bảo dưỡng.
- **Vàng 70–<90%:** Thiếu dữ liệu đủ lớn để ảnh hưởng năng lực xử lý.
- **Đỏ <70%:** Gần một phần ba hồ sơ không đủ điều kiện, cần ưu tiên chất lượng đầu vào.

Ngưỡng được dùng như tiêu chuẩn chất lượng dữ liệu thử nghiệm.

**Kế hoạch baseline:** Chạy kiểm tra schema và nghiệp vụ trên tập hồ sơ pilot thật trong hai chu kỳ.

### O1 — AI Completion [TB]

- **Xanh ≥78%:** Đạt giả định tỷ lệ hoàn thành luồng đến booking request trong mô hình Day 22.
- **Vàng 70–<78%:** Kết quả thấp hơn dự phóng, cần phân tích nguyên nhân và chi phí tăng thêm.
- **Đỏ <70%:** Sản lượng booking request có nguy cơ không đủ để giữ Cost/Job theo mô hình dự kiến.

Mốc 78% xuất phát từ **giả định mô hình**, còn 70% là giới hạn can thiệp thử nghiệm chứ không phải benchmark bên ngoài.

**Kế hoạch baseline:** Thu thập tối thiểu hai tuần case thật, phân tách lỗi AI, thiếu dữ liệu và trường hợp khách không tiếp tục.

### O2 — Booking Confirmation [TB]

- **Xanh ≥92%:** Đạt giả định Day 22.
- **Vàng 85–<92%:** Hiệu suất xác nhận thấp hơn kế hoạch.
- **Đỏ <85%:** Quá nhiều yêu cầu không chuyển thành booking xác nhận, có thể làm tăng Cost/Job.

Mốc 85% là ngưỡng can thiệp pilot cần kiểm chứng.

**Kế hoạch baseline:** Đối soát yêu cầu đã có quyết định cuối theo tuần, cùng nguyên nhân bị từ chối.

### O3 — Cost/Job [MH-01]

- **Xanh ≤8.514đ:** Không vượt chi phí quy đổi của kịch bản gói Tăng trưởng mới.
- **Vàng >8.514–9.771đ:** Chi phí đã vượt dự phóng nhưng chưa vượt giá sàn 3×.
- **Đỏ >9.771đ:** Chạm vùng không bảo đảm quy tắc Price ≥3× COGS trong kịch bản 160 booking/tháng.

Nguồn: Mô hình tài chính Day 22 và phép tính MH-01.

### O4 — Projected CAC [MH-02]

- **Xanh ≤32 triệu:** Phù hợp kịch bản founder-led sales.
- **Vàng >32–39,07 triệu:** Cao hơn giả định nhưng vẫn nằm dưới trần payback 12 tháng.
- **Đỏ >39,07 triệu:** Có nguy cơ vượt giới hạn hoàn vốn mục tiêu.

Nguồn: ARPU, GM và CAC payback của mô hình Day 22.

### G1 — Gross Margin [MH-03]

- **Xanh ≥66,67%:** Đạt mức biên gộp tương ứng với Price ≥3× COGS.
- **Vàng 60–<66,67%:** Vẫn đạt ngưỡng tối thiểu nhưng mất khoảng đệm.
- **Đỏ <60%:** Vi phạm mục tiêu Gross Margin tối thiểu của bài lab.

Nguồn: Ràng buộc Day 22 và công thức Gross Margin.

---

# 3.4. PHỤ LỤC [MH] — CÁC PHÉP TÍNH CÓ THỂ KIỂM CHỨNG

## [MH-01] Cost per Confirmed Job và giá sàn

**Mục tiêu:** Tìm mức chi phí cho phép của một booking khi xưởng sử dụng đủ hạn mức gói Tăng trưởng.

### Đầu vào

| Biến | Giá trị |
|---|---:|
| Giá gói Tăng trưởng/tháng | 4.690.000đ |
| Booking bao gồm/tháng | 160 |
| Hệ số sàn giá | 3× |
| COGS mô phỏng ở 160 booking | 1.362.249đ |

### Phép tính 1 — Cost/Job mô phỏng

`Cost/Job = 1.362.249 / 160`

**Kết quả: khoảng 8.514đ/booking.**

### Phép tính 2 — Trần chi phí theo quy tắc 3×

`Maximum COGS/month = 4.690.000 / 3`

`= 1.563.333đ/tháng`

`Maximum Cost/Job = 1.563.333 / 160`

**Kết quả: khoảng 9.771đ/booking.**

### Kết luận

- Xanh: Cost/Job ≤8.514đ.
- Vàng: >8.514đ đến ≤9.771đ.
- Đỏ: >9.771đ.

**Giới hạn áp dụng:** Đây là ngưỡng suy từ kịch bản giá và quota cố định, không phải trần chi phí bất biến. Khi booking thực tế thấp hơn 160, chi phí cố định/booking tăng; phải kiểm tra thêm GM thực tế và tổng COGS trên doanh thu, không chỉ nhìn một con số Cost/Job.

**Nguồn:** File tài chính cá nhân Day 22, các tab Cost/Job và Pricing; bảng gói Tăng trưởng 160 booking đang đề xuất.

---

## [MH-02] Customer Acquisition Cost tối đa

**Mục tiêu:** Tìm CAC lớn nhất để hoàn vốn trong 12 tháng.

### Đầu vào

| Biến | Giá trị |
|---|---:|
| ARPU tham chiếu | 4.690.000đ/tháng |
| GM bảo thủ từ Day 22 | 69,42% |
| Payback mục tiêu | 12 tháng |
| CAC giả định | 32.000.000đ |

### Phép tính

`Gross Profit/month = ARPU × GM`

`≈ 4.690.000 × 69,42%`

`≈ 3.255.638đ/tháng`

`CAC_max = Gross Profit/month × 12`

`≈ 39.067.658đ`

### Kết luận

**CAC tối đa khoảng 39,07 triệu đồng/xưởng.**

- Xanh: ≤32 triệu.
- Vàng: >32 triệu đến ≤39,07 triệu.
- Đỏ: >39,07 triệu.

Giả định CAC = 32 triệu thì:

`CAC Payback ≈ 32.000.000 / 3.255.638`

**≈ 9,83 tháng.**

**Lý do chọn ngưỡng:** CAC lớn hơn 39,07 triệu không phù hợp mục tiêu hoàn vốn trong 12 tháng nếu ARPU và GM giữ như mô hình.

**Giới hạn:** Dùng GM 69,42% của phiên bản Day 22 như giả định bảo thủ; chưa tính churn, thay đổi giá gói, hỗ trợ phát sinh và dòng tiền trả trước.

**Nguồn:** Mô hình tài chính cá nhân Day 22, ARPU, GM và CAC Payback.

---

## [MH-03] Gross Margin và quy tắc giá 3× COGS

**Mục tiêu:** Xác định vùng lợi nhuận đủ an toàn để vận hành SaaS AI.

### Công thức

`GM = (Revenue - COGS) / Revenue × 100%`

Theo nguyên tắc giá sàn của bài lab:

`Price ≥ 3 × COGS`

Tại ranh giới:

`COGS / Price = 1 / 3`

`GM = (1 - 1/3) × 100%`

**GM = 66,67%.**

### Kiểm tra với mô hình gói Tăng trưởng

`GM = (4.690.000 - 1.362.249) / 4.690.000 × 100%`

**GM mô phỏng ≈ 70,95%.**

### Kết luận

- Xanh: GM ≥66,67%.
- Vàng: 60% ≤ GM <66,67%.
- Đỏ: GM <60%.

**Giải thích:** Dưới 60%, mô hình không còn đạt ràng buộc GM mục tiêu tối thiểu. Mức 66,67% là vùng xanh theo quy tắc Price ≥3× COGS, không phải chuẩn chung của mọi công ty SaaS.

**Nguồn:** Ràng buộc Day 22 và COGS/Price của gói Tăng trưởng.

---

## 3.5. Đánh giá chất lượng ngưỡng

| Kiểm tra | Kết quả |
|---|---|
| Có đủ 8/8 ngưỡng xanh-vàng-đỏ | Có |
| Mỗi đèn có ký hiệu nguồn | Có |
| Mỗi đèn có lý do lựa chọn | Có |
| Có tối thiểu 2 phép tính [MH] | Có 3 |
| Có số liệu từ mô hình của sản phẩm | Có |
| Ngưỡng [BM] có nguồn/ngày kiểm tra | Không sử dụng [BM] |
| Ngưỡng [TB] có lịch đo baseline | Có |
| Có phân biệt mô phỏng và số thực | Có |
| Chỉ số chưa đo được được khai báo | Có |

**Kết luận Trạm 3:** Các ngưỡng [MH] có căn cứ tài chính và có thể tính lại. Các ngưỡng [TB] được công bố là mục tiêu thử nghiệm, phải xác minh bằng dữ liệu pilot trước khi sử dụng như ngưỡng vận hành lâu dài.

---

# TRẠM 4 — NĂM LUẬT QUYẾT ĐỊNH

**Thời lượng:** 30 phút.

## 4.1. Nguyên tắc

Mỗi luật phải có:

- **NẾU:** Một chỉ số chạm đúng ngưỡng đỏ đã định.
- **TRONG/TRÊN:** Cửa sổ quan sát hoặc số lượng quan sát.
- **VÀ:** Điều kiện đủ mẫu khi cần.
- **THÌ:** Hành động cụ thể, có thể thực thi.
- **KHÔNG THÌ:** Phản xạ sai hoặc hành động bị cấm.

**Quy ước:** ⏹ là luật dừng.

Chỉ kích hoạt luật khi dữ liệu đã qua validation, mẫu đủ điều kiện và không bị trộn với booking demo.

---

## LUẬT 1 — ⏹ DỪNG MỞ RỘNG XƯỞNG KHI TTFV QUÁ DÀI

**Liên quan:** L1 — TTFV

**NẾU:** TTFV của một xưởng vượt **30 ngày**.

**TRÊN:** Ít nhất **2 trong 3 xưởng pilot** đã go-live và đủ 30 ngày quan sát.

**VÀ:** Các xưởng đã hoàn tất điều kiện kỹ thuật tối thiểu, có dữ liệu hợp lệ và không bị gián đoạn do sự cố ngoài phạm vi sản phẩm.

**THÌ:**

1. **Dừng** tiếp nhận thêm xưởng pilot mới trong 14 ngày.
2. Product Lead kiểm tra thời gian thực hiện từng bước onboarding.
3. Xác định công đoạn gây chậm nhất: dữ liệu, AI, khách tạo yêu cầu hay xưởng xác nhận.
4. Thu hẹp pilot xuống một luồng đặt lịch có dữ liệu rõ ràng.
5. Kiểm tra lại TTFV trên các xưởng hiện có trước khi mở rộng.

**KHÔNG THÌ:** Không tiếp tục ký thêm xưởng chỉ để tăng chỉ số số lượng đối tác.

**Owner:** Product / Pilot Lead.

**Hạn bắt đầu hành động:** Trong 1 ngày làm việc sau khi điều kiện kích hoạt được xác nhận.

**Bằng chứng:** Cohort TTFV, log onboarding, danh sách booking thật và biên bản dừng mở rộng.

**Mục tiêu của luật:** Ngăn việc tăng số đối tác khi đối tác hiện tại chưa nhận được giá trị.

---

## LUẬT 2 — ⏹ DỪNG MỞ RỘNG USAGE KHI COST/JOB VƯỢT TRẦN

**Liên quan:** O3 — Cost per Confirmed Job

**NẾU:** Cost/Job vượt **9.771đ/booking** trong kịch bản gói Tăng trưởng mục tiêu 160 booking/tháng.

**TRONG:** 2 tuần liên tiếp.

**VÀ:** Mỗi tuần có ít nhất 20 booking xác nhận hợp lệ, dữ liệu COGS được đối soát và phạm vi gói phù hợp với kịch bản đang đánh giá.

**THÌ:**

1. **Đóng băng** mở rộng trial và tăng hạn mức AI cho xưởng mới.
2. Kiểm tra token, retrieval, retry và thời gian HITL.
3. Chuyển các bước có quy tắc xác định sang Rule Engine khi phù hợp.
4. Tối ưu caching, giới hạn vòng lặp Agent và giảm các lời gọi không tạo giá trị.
5. Tính lại Cost/Job và GM trước khi khôi phục mở rộng.

**KHÔNG THÌ:** Không tăng quota miễn phí hoặc thay toàn bộ logic xác định bằng lời gọi LLM để che lỗi vận hành.

**Owner:** Backend / Agent / Finance.

**Hạn bắt đầu hành động:** Trong 1 ngày làm việc.

**Bằng chứng:** Cost ledger, LangGraph trace, báo cáo retry, số booking và GM.

**Lưu ý:** Với xưởng có volume khác 160 booking/tháng, ưu tiên dùng ngưỡng COGS/Revenue được tính lại từ sản lượng thực tế. Không áp dụng máy móc mức 9.771đ cho mọi gói.

---

## LUẬT 3 — KIỂM SOÁT CAC TRƯỚC KHI MỞ RỘNG BÁN HÀNG

**Liên quan:** O4 — Projected CAC

**NẾU:** CAC dự phóng vượt **39,07 triệu đồng/xưởng trả phí**.

**TRONG:** 2 cửa sổ đo 30 ngày liên tiếp.

**VÀ:** Pipeline có chi phí được ghi nhận và đủ dữ liệu cơ hội bán hàng để ước tính tỷ lệ chuyển đổi.

**THÌ:**

1. **Dừng** chi tiêu acquisition có trả phí chưa cam kết.
2. Tập trung vào kênh founder-led sales.
3. Phân loại khách hàng theo quy mô, nhu cầu và khả năng chi trả.
4. Loại bỏ các phân khúc có chi phí bán quá cao so với giá trị hợp đồng.
5. Điều chỉnh sales motion và kiểm tra lại CAC dự phóng trong 30 ngày.

**KHÔNG THÌ:** Không giảm giá đại trà hoặc bơm thêm quảng cáo để bù pipeline yếu khi chưa xác định nguyên nhân.

**Owner:** Business / Sales Lead.

**Bằng chứng:** Sales budget, CRM pipeline, opportunity-to-win assumptions và CAC calculation.

**Mục tiêu:** Bảo vệ CAC Payback không vượt 12 tháng theo giả định Day 22.

---

## LUẬT 4 — GIẢM PHẠM VI AI KHI COMPLETION RATE THẤP

**Liên quan:** O1 — AI Completion Rate

**NẾU:** AI Completion Rate thấp hơn **70%**.

**TRONG:** 2 tuần liên tiếp.

**VÀ:** Có tối thiểu 100 case attempted hợp lệ trong cửa sổ đánh giá.

**THÌ:**

1. **Thu hẹp** luồng Agent về các loại bảo dưỡng có quy tắc rõ ràng và tài liệu đủ tin cậy.
2. Phân loại lỗi theo RAG, dữ liệu xe, timeout, tool execution và business validation.
3. Bật cơ chế chuyển sang HITL cho trường hợp không đủ căn cứ.
4. Sửa các node có tỷ lệ lỗi cao nhất.
5. Chỉ bật lại phạm vi cũ khi đã kiểm thử regression và đo lại tỷ lệ hoàn thành.

**KHÔNG THÌ:** Không cho AI tự suy đoán bảng giá, mốc bảo dưỡng hoặc chính sách bảo hành để làm tăng tỷ lệ hoàn thành giả tạo.

**Owner:** Agent / RAG Lead.

**Bằng chứng:** Evaluation dataset, trace log, failure classification và regression results.

**Mục tiêu:** Bảo vệ chất lượng sản phẩm và hạn chế chi phí của các lượt xử lý thất bại.

---

## LUẬT 5 — SỬA NĂNG LỰC TIẾP NHẬN KHI CONFIRMATION RATE THẤP

**Liên quan:** O2 — Booking Confirmation Rate

**NẾU:** Booking Confirmation Rate thấp hơn **85%**.

**TRONG:** 2 tuần liên tiếp.

**VÀ:** Có tối thiểu 50 booking request hợp lệ đã nhận quyết định cuối.

**THÌ:**

1. **Tạm dừng** mở thêm slot chưa được xưởng kiểm chứng.
2. Đối soát năng lực tiếp nhận thực tế với dữ liệu WorkshopSlot.
3. Phân tích lý do booking bị từ chối.
4. Điều chỉnh logic gợi ý ca phù hợp với tải xưởng.
5. Yêu cầu cố vấn dịch vụ xác nhận kết quả trước khi ghi booking thành công.

**KHÔNG THÌ:** Không tự chuyển booking request sang `confirmed`, không sửa log hoặc loại bỏ request hợp lệ khỏi mẫu số để làm đẹp tỷ lệ.

**Owner:** Backend / Workshop Operations.

**Bằng chứng:** Booking state transitions, workshop capacity, rejection reasons và slot reconciliation.

**Mục tiêu:** Đảm bảo những yêu cầu do AI tạo ra phù hợp với khả năng tiếp nhận thật của xưởng.

---

## 4.2. Bảng tổng hợp 5 luật

| Luật | Đèn | Điều kiện kích hoạt | Hành động chính | Loại |
|---|---|---|---|---|
| R1 | TTFV | >30 ngày trên ≥2/3 xưởng | Dừng thêm xưởng pilot | ⏹ Luật dừng |
| R2 | Cost/Job | >9.771đ trong 2 tuần | Đóng băng mở rộng usage | ⏹ Luật dừng |
| R3 | CAC | >39,07 triệu trong 2 cửa sổ | Dừng acquisition trả phí | Kiểm soát kinh tế |
| R4 | AI Completion | <70% trong 2 tuần | Thu hẹp AI workflow | Kiểm soát chất lượng |
| R5 | Confirmation Rate | <85% trong 2 tuần | Sửa slot và capacity | Kiểm soát vận hành |

## 4.3. Kiểm tra Decision Rule Quality

| Tiêu chí | Kết quả |
|---|---|
| Có đúng 5 luật | Có |
| Mỗi luật sử dụng ngưỡng đỏ từ Trạm 3 | Có |
| Có đủ NẾU | 5/5 |
| Có TRONG/TRÊN | 5/5 |
| Có điều kiện mẫu khi cần | Có |
| Có hành động THÌ cụ thể | 5/5 |
| Có KHÔNG THÌ | 5/5 |
| Có ≥2 luật dừng | Có |
| Có người phụ trách | Có |
| Có bằng chứng để kiểm tra | Có |

**Kết luận Trạm 4:**

Bộ luật được thiết kế để ngăn ba sai lầm chính:

1. Mở rộng đối tác khi xưởng hiện tại chưa thấy giá trị.
2. Tăng AI usage khi chi phí đã vượt khả năng kinh tế của gói.
3. Làm đẹp tỷ lệ thành công bằng cách bỏ qua lỗi hoặc xác nhận booking không hợp lệ.

---

# 5. TỔNG KẾT WORKSHEET

## 5.1. Kết quả bốn trạm

| Trạm | Đầu ra | Trạng thái |
|---|---|---|
| Trạm 1 | B2B Pilot + kiểm kê 15 chỉ số ứng viên | Hoàn thành bản thiết kế |
| Trạm 2 | 1 North Star + 8 đèn/3 tầng | Hoàn thành bản thiết kế |
| Trạm 3 | 8 bộ ngưỡng + nguồn + 3 phép tính [MH] | Hoàn thành, chờ baseline |
| Trạm 4 | 5 luật quyết định + 2 luật dừng | Hoàn thành bản thiết kế |

## 5.2. Những gì chưa đo được

| Dữ liệu | Trạng thái | Cần bổ sung |
|---|---|---|
| TTFV pilot thật | N/A | Go-live và booking đầu tiên |
| Setup Rate thật | N/A | Onboarding event |
| Data Readiness thật | N/A | Validation log xe pilot |
| AI Completion thật | N/A | Agent trace và booking request |
| Confirmation Rate thật | N/A | Booking transitions |
| Cost/Job thật | N/A | COGS ledger và booking ledger |
| CAC actual | N/A | Sales cost và xưởng trả phí |
| GM actual | N/A | Doanh thu thuần và COGS |
| Runway | N/A | Tiền khả dụng và net burn |

**Ngày dự kiến có baseline ban đầu:** 08/11/2026.

**Ngày dự kiến có dữ liệu hai chu kỳ để hiệu chỉnh:** 08/12/2026.

Đây là kế hoạch đo, không phải cam kết rằng pilot chắc chắn có đủ dữ liệu vào những ngày trên.

## 5.3. Các quyết định cần mentor phản biện

1. Với trạng thái sản phẩm hiện tại, chọn B2B thay cho B2B2C có phù hợp không?
2. TTFV tính đến booking đầu tiên được xưởng xác nhận đã đủ phản ánh giá trị, hay cần thêm điều kiện booking được thực hiện?
3. Ngưỡng 3× COGS có phù hợp cho mọi mức quota, đặc biệt khi usage thấp?
4. Có nên tách AI Technical Success khỏi tỷ lệ khách chủ động tạo booking request?
5. Với số lượng xưởng pilot nhỏ, cần bổ sung điều kiện mẫu thế nào để tránh kích hoạt luật sai?

## 5.4. Kết luận

Operating Dashboard của VFO2O-06 ưu tiên việc kiểm chứng giá trị đầu tiên của xưởng và khả năng kiểm soát chi phí AI trước khi mở rộng.

**Thông điệp chính:**

> Không mở rộng chỉ vì có thêm xưởng đăng ký. Chỉ mở rộng khi xưởng đã nhìn thấy giá trị, quy trình AI tạo được booking hợp lệ và chi phí phục vụ vẫn nằm trong giới hạn kinh tế cho phép.

**Trạng thái cuối:** Đã hoàn thành thiết kế worksheet cho Lab Day 23. Các ngưỡng còn phụ thuộc baseline sẽ được hiệu chỉnh sau pilot.

---

**Người thực hiện:** Nguyễn Duy Khánh  
**MSSV:** 2A202602736  
**Lab:** Day 23 — AI Product Growth  
**Sản phẩm:** VFO2O-06 — EV Maintenance AI Agent