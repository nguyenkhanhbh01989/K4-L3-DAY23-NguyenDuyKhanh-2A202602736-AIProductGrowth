# VFO2O-06 | OPERATING DASHBOARD — DAY 23

**Nguyễn Duy Khánh · MSSV: 2A202602736 · Ngày lập: 09/10/2026**  
**Mô hình: B2B Pilot → định hướng B2B2C**  
**Sản phẩm:** AI Agent lập kế hoạch, nhắc lịch và hỗ trợ đặt lịch bảo dưỡng xe điện.

**Lý do chọn B2B hiện tại:** Xưởng là bên trả phí và cần nhận được giá trị vận hành đầu tiên; việc chủ xe thật sử dụng và được theo dõi trực tiếp chưa có đủ bằng chứng để xác nhận B2B2C đang vận hành.

> **NORTH STAR — TIME-TO-FIRST-VALUE (TTFV)**
>
> **Mục tiêu: ≤14 ngày · Actual: N/A**
>
> Số ngày từ `workshop_go_live` đến `booking_confirmed` đầu tiên của khách thật. Không tính booking demo, nội bộ hoặc trùng lặp. Tổng hợp bằng trung vị theo cohort xưởng đủ điều kiện.
>
> **Vì sao:** Xưởng cần nhìn thấy booking thật trước khi có cơ sở gia hạn, trả phí và mở rộng sử dụng.

---

## 1. HỆ THỐNG 8 ĐÈN — 3 TẦNG

🟢 Đạt · 🟡 Cảnh báo · 🔴 Kích hoạt luật · **N/A:** Chưa đủ dữ liệu, không tự gán màu.

### TẦNG 1 — LEADING (Báo trước theo ngày/tuần)

| Đèn / Công thức | 🟢 | 🟡 | 🔴 | Nguồn · Lý do |
|---|---|---|---|---|
| **L1 · TTFV**: Ngày booking đầu − ngày go-live | ≤14 ngày | >14–30 ngày | >30 ngày | **[TB]** Trial 30 ngày; báo trước chuyển đổi trả phí |
| **L2 · Setup Rate**: Xưởng setup ≤7 ngày / xưởng đủ 7 ngày | ≥80% | 50–<80% | <50% | **[TB]** Setup chậm làm tăng TTFV |
| **L3 · Data Readiness**: Hồ sơ xe hợp lệ / hồ sơ đánh giá | ≥90% | 70–<90% | <70% | **[TB]** Dữ liệu thiếu làm giảm AI Completion |

### TẦNG 2 — OPERATING (Kiểm soát vận hành theo tuần/tháng)

| Đèn / Công thức | 🟢 | 🟡 | 🔴 | Nguồn · Lý do |
|---|---|---|---|---|
| **O1 · AI Completion**: Request hợp lệ / case attempted | ≥78% | 70–<78% | <70% | **[TB]** Mục tiêu Day 22; báo trước Cost/Job |
| **O2 · Confirmation**: Booking xác nhận / request đã có quyết định | ≥92% | 85–<92% | <85% | **[TB]** Nhiều request bị từ chối làm tăng Cost/Job |
| **O3 · Cost/Job**: COGS trực tiếp / booking xác nhận | ≤8.514đ | >8.514–9.771đ | >9.771đ | **[MH-01]** Trần 3× COGS; báo trước GM |
| **O4 · Projected CAC**: Chi phí acquisition / xưởng trả phí dự kiến | ≤32tr | >32–39,07tr | >39,07tr | **[MH-02]** Trần payback 12 tháng |

### TẦNG 3 — LAGGING (Kết quả tài chính theo tháng)

| Đèn / Công thức | 🟢 | 🟡 | 🔴 | Nguồn · Lý do |
|---|---|---|---|---|
| **G1 · Gross Margin**: (Doanh thu thuần − COGS) / doanh thu | ≥66,67% | 60–<66,67% | <60% | **[MH-03]** Sàn lab GM 60%; vùng xanh từ quy tắc 3× |

**Trạng thái hiện tại:** 8/8 đèn chưa có dữ liệu pilot đủ điều kiện để xác nhận thực tế. O4 và G1 đã có số *dự phóng*, không được coi là Actual.

**Nguồn ngưỡng:** [MH] suy từ mô hình Day 22; [TB] là ngưỡng pilot tạm thời, chưa có baseline. Thu thập baseline đầu tiên dự kiến 08/11/2026 và rà soát sau 2 chu kỳ vào 08/12/2026. Không sử dụng [BM] chưa được kiểm chứng.

---

## 2. NĂM LUẬT QUYẾT ĐỊNH

**R1 · ⏹ DỪNG MỞ RỘNG XƯỞNG — L1**  
**NẾU** TTFV >30 ngày **TRÊN** ≥2/3 xưởng go-live đủ 30 ngày và dữ liệu hợp lệ, **THÌ** dừng nhận xưởng pilot mới 14 ngày, sửa onboarding và luồng tạo booking đầu tiên. **KHÔNG THÌ** không ký thêm xưởng chỉ để tăng số lượng đối tác.

**R2 · ⏹ ĐÓNG BĂNG MỞ RỘNG AI — O3**  
**NẾU** Cost/Job >9.771đ **TRONG** 2 tuần liên tiếp, **VÀ** ≥20 booking xác nhận/tuần trong kịch bản gói Tăng trưởng tương ứng, **THÌ** đóng băng mở rộng trial/quota và tối ưu token, retry, cache, HITL. **KHÔNG THÌ** không tăng hạn mức AI khi chi phí chưa được kiểm soát.

**R3 · DỪNG ACQUISITION TRẢ PHÍ — O4**  
**NẾU** CAC dự phóng >39,07 triệu/xưởng **TRONG** 2 cửa sổ 30 ngày với pipeline đủ dữ liệu, **THÌ** dừng chi tiêu acquisition trả phí, chuyển sang founder-led sales và rà soát phân khúc. **KHÔNG THÌ** không giảm giá đại trà để che CAC cao.

**R4 · THU HẸP PHẠM VI AGENT — O1**  
**NẾU** AI Completion <70% **TRONG** 2 tuần liên tiếp, **VÀ** ≥100 case attempted hợp lệ, **THÌ** thu hẹp workflow, sửa lỗi RAG/tool và bật fallback HITL. **KHÔNG THÌ** không để AI suy đoán giá, quy tắc bảo dưỡng hoặc tự xác nhận booking.

**R5 · SỬA NĂNG LỰC TIẾP NHẬN — O2**  
**NẾU** Confirmation Rate <85% **TRONG** 2 tuần liên tiếp, **VÀ** ≥50 request có quyết định cuối, **THÌ** tạm dừng mở slot chưa kiểm chứng, đối soát capacity và nguyên nhân từ chối. **KHÔNG THÌ** không tự chuyển booking request sang confirmed để làm đẹp tỷ lệ.

---

## 3. BA CỔNG GÁC 90 NGÀY

**Thời điểm bắt đầu dự kiến: 09/10/2026**

| Cổng | Một metric và ngưỡng | Bằng chứng bắt buộc | Quyết định nếu trượt |
|---|---|---|---|
| **D30 · 08/11/2026** LEARN | Setup Rate ≥66,7% (≥2/3 xưởng pilot) | Onboarding checklist, go-live log | **FIX** onboarding một lần |
| **D60 · 08/12/2026** VALIDATE | TTFV trung vị ≤14 ngày (≥2 xưởng đủ mẫu) | Timestamp booking thật và go-live | **FIX** lần đầu; **PIVOT** nếu đã FIX cùng vấn đề |
| **D90 · 07/01/2027** DECIDE | GM thực tế ≥66,67% (≥2 xưởng trả phí để kiểm chứng) | Doanh thu thuần, LLM/HITL/infra cost ledger | **FIX** một lần nếu có nguyên nhân mới; **PIVOT/KILL** nếu giả thuyết kinh tế thất bại |

**Quy tắc cổng:** GO khi đạt và có bằng chứng; FIX tối đa một lần cho cùng vấn đề; PIVOT khi giả thuyết ban đầu sai; KILL khi đã FIX nhưng điều kiện sống còn tiếp tục thất bại. Không có dữ liệu đạt điều kiện không được tính là GO.

---

## 4. KILL CRITERIA

**Đến 07/01/2027**, nếu sau một chu kỳ FIX mà **≥2/3 xưởng pilot vẫn có TTFV >30 ngày**, nhóm **dừng thương mại hóa gói hiện tại cho phân khúc đã thử nghiệm**, ngừng tuyển thêm xưởng thuộc phân khúc đó và đánh giá lại giả thuyết giá trị.

---

## 5. CHƯA ĐO ĐƯỢC & CAM KẾT MINH BẠCH

| Thiếu dữ liệu | Cần bổ sung | Dự kiến |
|---|---|---|
| TTFV, Setup, Data Readiness | Onboarding + vehicle validation log | 23/10: tracking; 08/11: baseline |
| AI Completion, Confirmation | LangGraph trace, booking state log | 08/11: baseline |
| Cost/Job, GM thực tế | Token/API, retry, infra, HITL, billing ledger | 08/12: đối soát |
| CAC thực tế, Runway | Sales cost, hợp đồng trả phí, vốn và net burn | D90 hoặc khi đủ dữ liệu |

**Giả định tài chính tham chiếu:** Gói Tăng trưởng 4.690.000đ/tháng, 160 booking/tháng, COGS mô phỏng ~1.362.249đ, GM mô phỏng ~70,95%, CAC mục tiêu 32 triệu. Đây không phải kết quả đã đạt.

**Phụ lục phép tính [MH] nằm trong `worksheet.md`:**

- **MH-01:** 4.690.000 ÷ (3 × 160) = **9.771đ/booking**; COGS mô phỏng ÷160 ≈ **8.514đ**.
- **MH-02:** 4.690.000 × 69,42% × 12 ≈ **39,07 triệu CAC tối đa**.
- **MH-03:** GM = 1 − 1/3 = **66,67%**; GM tối thiểu của lab = **60%**.

**Nguyên tắc ra quyết định:** Chỉ mở rộng khi xưởng thấy giá trị đủ sớm, booking được xác nhận hợp lệ và kinh tế đơn vị vẫn nằm trong giới hạn cho phép.