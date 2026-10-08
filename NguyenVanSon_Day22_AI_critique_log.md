# AI Critique Log — Day 22 Monetization Lab (SàngCV)

- **Người làm bài:** Nguyễn Văn Sơn — 2A202602744
- **Ngày:** 08/10/2026
- **Sản phẩm:** SàngCV — AI sàng lọc CV và lập shortlist có dẫn chứng, bán cho SME Việt Nam
- **Quy tắc:** AI là công cụ phản biện, không phải tác giả. Mỗi điểm được ghi là **ACCEPT / PARTIAL / REJECT** kèm lý do.

> **Minh bạch về cách chạy.** Hai prompt §4.7.1 và §4.7.2 được chạy trong một phiên Claude Code (Claude Sonnet 5.5) ngày 08/10/2026. Mọi phép tính lại dùng đúng số trong `NguyenVanSon_Day22_model.xlsx` và được đối chiếu bằng một script Python độc lập (kết quả khớp với file Excel). Prompt chạy ngoài phiên này, hoặc trên công cụ khác, chưa có. **Sơn cần đọc lại từng điểm, tự chạy lại ít nhất một prompt bằng công cụ của mình và sửa quyết định accept/reject nếu không đồng ý.** Các giá Gemini, Workable, Manatal, Intercom Fin được mở trực tiếp trên trang gốc ngày 08/10/2026.

---

## PROMPT 4.7.1 — COST/JOB STRESS TEST

**Input (tóm tắt).** Job = 1 shortlist hoàn tất (`SHORTLIST_ACCEPTED`). Kịch bản 8 khách × 4 requisition = 32 requisition bắt đầu, containment 80% → 25,6 job hoàn thành. Mỗi requisition có 300 CV. Gemini 2.5 Flash 0,30/2,50 USD mỗi 1M token. Infra 60 USD cố định + 4 USD/khách. Retry 8%. QA vendor lấy mẫu 3% CV. Overhead 400 USD + 30 USD/khách. Biến thể HITL: A.

### 1. Missing cost categories

| # | Điểm AI nêu | Quyết định | Xử lý |
|---|---|---|---|
| 1a | Chưa tính xác minh OAuth/đánh giá bảo mật khi add-on Gmail xin quyền đọc thư (restricted scope). Đây là chi phí một lần, chưa có báo giá. | **ACCEPT** | Không đưa vào COGS (chi phí một lần). Ghi thành rủi ro ở `4_Channel_Fit` và đổi MVP sang nhận CV qua email forward/thư mục Drive để tránh quyền đọc cả hộp thư. |
| 1b | Chạy lại eval mỗi lần đổi model/rubric. | **PARTIAL** | 100 CV × 0,0072 USD ≈ 0,72 USD mỗi lượt eval, quá nhỏ để thành dòng riêng. Chỉ ghi chú. |
| 1c | Lưu trữ CV tăng theo thời gian, dữ liệu cá nhân phải có hạn lưu. | **ACCEPT** | Đã có `cv_infra` 0,0003 USD/CV và đề xuất xóa sau 90 ngày. Hạn lưu chính thức đưa vào Procurement Q&A. |
| 1d | Chi phí pháp lý (đánh giá tác động dữ liệu cá nhân, rà soát điều khoản) bị bỏ sót. | **PARTIAL** | Thuộc Overhead, không thuộc COGS. Chưa có con số. Ghi là điểm chưa đạt ở `5_90Day_Plan`. |
| 1e | Thiếu giám sát/alerting. | **REJECT** | Đã nằm trong dòng infra 60 USD (logging/giám sát ~21 USD). |

### 2. Denominator check

- AI hỏi: mẫu số là job thử hay job hoàn thành?
- Kiểm tra: tử số là chi phí của 100% requisition bắt đầu (kể cả 20% thất bại), mẫu số là 25,6 job hoàn thành.
- COGS/job nếu chia nhầm cho requisition thử: 7,25 USD. COGS/job đúng: **9,06 USD**. Chia nhầm sẽ làm Cost/Job thấp hơn thực tế 20%.
- **ACCEPT.** Model đã chia đúng. Con số chênh lệch được đưa vào `1_Cost_Job` để người đọc thấy rủi ro.

### 3. Token math audit

| Điểm AI nêu | Quyết định | Xử lý |
|---|---|---|
| **Explicit caching làm đắt hơn.** Khối system chỉ 3.000 token. Giữ cache 0,6 giờ mỗi CV với giá 1 USD/1M token-giờ tốn 0,0018 USD, vượt xa mức tiết kiệm. Chi phí phần system: 0,00197 USD (có cache) so với 0,00090 USD (không cache). Tổng LLM/CV tăng từ 0,00718 lên 0,00825 USD (+14,9%). | **ACCEPT** | Đặt `use_cache = 0`. Ô "% tiết kiệm nhờ cache" hiển thị −119% để lộ ra thay vì giấu. Đây là điểm phản biện có giá trị nhất của lượt này. |
| Batch API (−50%) khả thi vì shortlist không cần realtime. GM tăng từ 75,4% lên 79,3%. | **PARTIAL** | Ghi con số thứ hai (`cv_batch`) và GM Batch ở `2_Pricing`. Base case vẫn dùng giá standard vì thời gian trả kết quả của Batch chưa được kiểm chứng với SLA của khách. |
| Thinking token chưa đo. Nếu gấp đôi (2.000 token) thì GM còn 72,6%. | **ACCEPT** | Vẫn trên 60%. Ghi "chưa đo" và việc đo `usage_metadata` thuộc eval. |
| Token OCR (516 token/2 trang) chỉ là giả định. Nếu tỷ lệ CV scan là 50% thay vì 25% thì GM còn 74,3%. | **ACCEPT** | Ghi là giả định cần đo. Không ảnh hưởng kết luận. |

### 4. Price volatility

- Giá Gemini lấy từ trang giá chính thức ngày 08/10/2026, không phải giá khuyến mại.
- Kịch bản giá LLM tăng gấp đôi: GM giảm từ 75,4% xuống **67,4%**, vẫn đạt ≥60%.
- **ACCEPT.** Model có kịch bản này ở `2_Pricing`. Rủi ro thật hơn là Google đổi hoặc ngừng model: khi đó phải chạy lại eval trước khi chuyển. Ghi vào `5_90Day_Plan`.

### 5. Breakeven sensitivity

Đại số (COGS không đổi theo containment vì mọi requisition thử đều tốn chi phí):

```
Rev = phí_nền×khách + giá×attempts×c ≥ COGS / (1 − 0,6)
c ≥ (COGS/0,4 − phí_nền×khách) / (giá × attempts)
  = (232,02×26.000/0,4 − 500.000×8) / (800.000×32) = 43,3%
```

| Containment | 50% | 60% | 70% | 80% | 90% |
|---|---|---|---|---|---|
| GM | 64,1% | 68,8% | 72,5% | 75,4% | 77,7% |

- Breakeven containment 43,3% so với giả định 80% (chưa đo). Biên an toàn 36,7 điểm %.
- **ACCEPT**, kèm cảnh báo: breakeven chỉ có ý nghĩa nếu giá 800.000 ₫/shortlist là đúng. Giá đó chưa được khách xác nhận.
- Theo quy mô: GM theo COGS ở 3/5/8/12/20 khách là 64,7% / 71,5% / 75,4% / 77,5% / 79,2%. Margin đầy tải là −74,0% / −21,9% / 7,4% / 23,7% / 36,7%.

### 6. The one number that kills me

- Đảo độ nhạy ×2: giảm giá/shortlist một nửa hoặc giảm containment một nửa đều đẩy GM xuống **57,6%**, dưới 60%. Số CV mỗi requisition ×2 còn 62,5%. Infra ×2 còn 65,3%. HITL ×2 còn 68,7%. LLM ×2 còn 67,4%.
- Kết luận của AI: biến số nguy hiểm nhất là **doanh thu**, tức willingness-to-pay và containment. Cả hai đều chưa đo. Chi phí LLM không phải rủi ro chính.
- **ACCEPT.** Điều chỉnh kế hoạch tháng 1: thử giá với ≥15 HR (3 mức giá) và đo containment bằng bộ 100 CV trước khi làm bất cứ việc gì khác.

### 7. Điểm AI nêu thêm

| Điểm | Quyết định | Xử lý |
|---|---|---|
| Margin đầy tải ở 8 khách chỉ 7,4%; Cost/Job đầy đủ (885.650 ₫) xấp xỉ giá bán. Giá sàn 3× theo Cost/Job đầy đủ không đạt. | **ACCEPT** | Không chỉnh giá để cho đẹp. Báo cáo cả hai cách tính; giá sàn theo COGS đạt (706.951 ₫ so với giá 800.000 ₫). Điểm chưa đạt ghi rõ ở One-Pager. |
| Lấy mẫu QA 3% quá thấp để phát hiện thiên lệch theo nhóm. | **PARTIAL** | Tăng lên 6% chỉ làm GM giảm còn 70,8%, nên chi phí không phải rào cản. Nhưng thiên lệch theo nhóm không phát hiện được bằng mẫu nhỏ. Giữ 3% ở base case, thêm chỉ số tỷ lệ chọn theo nhóm (quy tắc 4/5) vào Eval ở `5_90Day_Plan`. |

---

## PROMPT 4.7.2 — VALUE METRIC CHALLENGER

**Input (tóm tắt).** Job = 1 shortlist hoàn tất. Khách: SME 50–300 nhân sự, ngân sách nhân sự/vận hành. Autonomy hôm nay: AI không tự loại ứng viên, recruiter luôn chốt. Attribution hôm nay: ID duy nhất, audit trail, trích dẫn kiểm chứng được, nhưng chưa có eval. Cost/Job (COGS) 9,06 USD, giá đề xuất 500.000 ₫/khách/tháng + 800.000 ₫/shortlist. Lựa chọn của tôi: Hybrid.

### 1. Khuyến nghị theo ma trận

- Attribution 18/25 (cao), Autonomy 12/25 (thấp) → ma trận cho **Usage**.
- Outcome (trả theo tuyển được người) bị loại: không đo được nhân quả AI → tuyển dụng thành công, và chưa có eval.
- Lệch sang **Hybrid** có lý do thị trường: các ATS bán cho SME dùng phí nền theo công ty (Workable 299 USD/tháng) hoặc theo ghế (Manatal 15–55 USD/ghế/tháng). Phí nền cũng gánh chi phí cố định khi doanh thu theo job còn nhỏ.
- **PARTIAL ACCEPT.** Giữ Hybrid nhưng ghi rõ đây là giả thuyết cần kiểm bằng phỏng vấn tháng 1, vì "SME chưa quen trả theo kết quả" hiện chưa có dữ liệu thật.

### 2. Tấn công vào lựa chọn Hybrid (3 câu hỏi khó nhất)

| Câu hỏi | Quyết định | Xử lý |
|---|---|---|
| "Tháng ít tuyển, tôi vẫn trả 500.000 ₫ mà không dùng gì?" | **ACCEPT** | Phí nền phải mua được giá trị cụ thể (thiết lập rubric, báo cáo kiểm toán thiên lệch hàng tháng). Thêm cam kết hoàn 50% phí nền nếu tháng đó không có shortlist hoàn tất nào. |
| "Recruiter không bấm 'Chốt' thì bạn không thu tiền dù đã tốn chi phí xử lý 300 CV. Khách có thể cố tình không chốt." | **ACCEPT** | Sửa định nghĩa job: tự động tính là hoàn tất nếu recruiter xuất danh sách hoặc mở ≥3 hồ sơ top trong 14 ngày. Theo dõi tỷ lệ requisition bị hủy sau khi có shortlist. |
| "Sao không tính theo CV như Workable (0,12 USD/CV)?" | **REJECT** | Giữ tính theo shortlist. Giá hiệu dụng của SàngCV là 0,098 USD/CV (đã gồm phí nền), thấp hơn Workable. Tính theo CV ở mức này không đủ nuôi chi phí cố định, và khách mua "danh sách ứng viên đã sẵn sàng phỏng vấn" chứ không mua từng lượt chấm. |

### 3. Ba sản phẩm thật cùng loại job (đã mở trang gốc ngày 08/10/2026)

| Sản phẩm | Đơn vị tính tiền và giá | Nguồn | Nhận xét |
|---|---|---|---|
| Workable (AI screening) | Standard 299 USD/tháng/công ty. AI Agent: 1 credit/lần sàng lọc; gói 5.000 credit = 600 USD (0,12 USD/credit). | https://www.workable.com/pricing | Hybrid: phí nền + usage. Gần nhất với SàngCV. |
| Manatal | Theo ghế: 15 / 35 / 55 USD/user/tháng (trả theo năm). | https://www.manatal.com/pricing | Seat. Cho thấy SME quen mua phần mềm tuyển dụng theo ghế. |
| Intercom Fin | 0,99 USD/outcome; tối thiểu 50 outcome/tháng. | https://fin.ai/pricing/ | Outcome. Hợp lý vì AI tự chạy; SàngCV chưa đủ Autonomy nên không áp dụng. |

- **ACCEPT.** Ghi vào `3_Value_Metric` và `6_Benchmarks`.

### 4. Hành vi khách làm mô hình mất tiền

- Khách đưa số lượng CV rất lớn vào một requisition (hàng nghìn CV) để lấy một shortlist với giá cố định.
- Khách sửa JD liên tục để đổi rubric mà vẫn chỉ trả một job.
- **ACCEPT.** Thêm giới hạn 500 CV/requisition (vượt thì 1.500 ₫/CV); đổi >30% rubric thì tính job mới; mỗi `requisition_id` chỉ tính một lần.
- Phí nền bảo đảm doanh thu tối thiểu mỗi khách, nhưng không bảo vệ nếu khách rời đi. Đó là lý do tháng 1 phải kiểm retention.

---

## TỔNG HỢP

| Prompt | ACCEPT | PARTIAL | REJECT |
|---|---|---|---|
| 4.7.1 Cost/Job Stress Test | 10 | 4 | 1 |
| 4.7.2 Value Metric Challenger | 4 | 1 | 1 |

**Thay đổi lớn nhất sau phản biện**

1. Tắt explicit cache (`use_cache = 0`): phí lưu cache làm LLM/CV đắt hơn 14,9%.
2. Cost/Job được tính chia cho job hoàn thành và báo cáo hai cách: COGS 235.650 ₫/job, đầy đủ 5 thành phần 885.650 ₫/job.
3. Định nghĩa job thêm quy tắc auto-confirm (xuất danh sách hoặc mở ≥3 hồ sơ top) để tránh khách né phí.
4. Thêm cam kết hoàn 50% phí nền và giới hạn 500 CV/requisition.
5. Xác định biến số nguy hiểm nhất là giá và containment (cả hai chưa đo), nên tháng 1 ưu tiên thử giá và eval trước mọi việc khác.
6. Eval bổ sung chỉ số tỷ lệ chọn theo nhóm (quy tắc 4/5).

**Điều AI không giải quyết được**

- Không AI nào xác nhận được rằng SME Việt Nam sẵn sàng trả 800.000 ₫/shortlist. Chỉ phỏng vấn và pilot mới trả lời được.
- Containment 80%, số CV mỗi requisition, thời gian sàng lọc thủ công 4 phút/CV và lương HR 12 triệu/tháng là giả định của người làm bài.
