# Final Checklist — Lab Day 22 (Monetization & GTM)

- **Người làm bài:** Nguyễn Văn Sơn — 2A202602744
- **Ngày:** 08/10/2026
- **Sản phẩm:** SàngCV — AI sàng lọc CV và lập shortlist có dẫn chứng (SME Việt Nam)
- **Ký hiệu:** `[x]` đạt · `[~]` đạt một phần, có ghi rõ phần thiếu · `[ ]` chưa làm

Mọi con số bên dưới truy được về `NguyenVanSon_Day22_model.xlsx`.

---

## SELF-CHECK — 10 MỤC BẮT BUỘC

**[x] 1. Tab 1 — đủ 5 thành phần chi phí, không ô nào trống vô lý**
- API (LLM): 2,70 USD/job (tổng 4 thành phần đầu = COGS 9,06 USD ≈ 235.650 ₫, xem mục 3)
- Infra: 3,71 USD/job · HITL (QA vendor): 2,44 USD/job · Retry 8%: 0,22 USD/job · Overhead: 25,00 USD/job
- Chỉ có Speech = 0, và có lý do: MVP chỉ xử lý văn bản/ảnh CV.
- Retry 8% là ước tính, ghi rõ chưa có log thật. Không để 0.

**[x] 2. Tab 1 — mẫu số là JOB HOÀN THÀNH, không phải job thử**
- 8 khách × 4 requisition = 32 bắt đầu → 25,6 hoàn thành (containment 80%).
- Chi phí của 6,4 requisition thất bại vẫn nằm trong tử số.
- Chia nhầm cho job thử sẽ làm Cost/Job thấp hơn thực tế 20%.

**[~] 3. Tab 2 — Giá bán ≥ 3 × Cost/Job, Gross Margin ≥ 60%**
- COGS/job: 235.650 ₫. Giá sàn (3×): 706.951 ₫. Giá bán 800.000 ₫/shortlist → **đạt** theo COGS.
- **Gross Margin (theo COGS): 75,4%** → đạt ≥60%, và không vượt 85% (không đáng ngờ).
- **Chưa đạt:** Cost/Job đầy đủ 5 thành phần = 885.650 ₫, giá sàn 3× theo con số này = 2.656.951 ₫ → không đạt. Margin đầy tải chỉ 7,4% ở 8 khách.
- Không chỉnh giá cho đẹp. Báo cáo cả hai cách tính; cần ≥12–20 khách để margin đầy tải lên 24–37%.

**[~] 4. Tab 2 — Breakeven containment đã tính, đã so với eval**
- Breakeven containment để GM ≥ 60%: **43,3%**. Containment giả định: 80%. Biên an toàn 36,7 điểm %.
- **Chưa đạt:** chưa có eval thật, nên "80%" là giả định. Eval có deadline 25/10/2026 (xem mục 7).

**[x] 5. Tab 3 — Value Metric + Decision Note + 2 benchmark có link**
- Hybrid: 500.000 ₫/khách/tháng + 800.000 ₫/shortlist hoàn tất. Attribution 18/25, Autonomy 12/25 → ma trận gợi ý Usage; Decision Note 3 câu giải thích vì sao thêm phí nền (lý do thị trường).
- Workable: https://www.workable.com/pricing — 0,12 USD/lần sàng lọc, 299 USD/tháng/công ty.
- Manatal: https://www.manatal.com/pricing — 15–55 USD/ghế/tháng.
- Tham chiếu thêm Intercom Fin: https://fin.ai/pricing/ — 0,99 USD/outcome.
- Các trang giá đều được mở trực tiếp ngày 08/10/2026.

**[x] 6. Tab 4 — Ngân sách CAC, deal/AE/ngày, 1 kênh duy nhất**
- Ngân sách CAC: 1.064 USD/khách (ARPU 117,69 USD × GM 75,4% × 12 tháng).
- Deal/AE/ngày: 0,59 (khả thi về số học), nhưng CAC Sales-Led thực tế ≈ 31.500 USD, **lệch 29,6×** ngân sách.
- Kênh chốt: **PLG** (25/30). Partner-Led không được chọn vì chưa có tên đối tác. CAC PLG 200 USD (0,19× ngân sách) là giả thuyết chưa thử.

**[x] 7. Tab 5 — 90-day plan có số; Evidence Pack có deadline**
- Tháng 1: 15 phỏng vấn, 3 pilot, ≥6 shortlist, thử 3 mức giá. Tháng 2–3: ≥120 lượt đăng ký, activation ≥40%, 8 khách trả tiền. Mỗi giai đoạn có điều kiện dừng. Người phụ trách: Sơn.
- Eval Results: 25/10/2026. Procurement Q&A: 20/10/2026. Pilot Report: liên hệ khách trước 05/11/2026, báo cáo 10/12/2026.
- Các mốc ngày trên là **kế hoạch**, chưa phải việc đã làm.

**[x] 8. Ghi ngày kiểm tra giá API ở tab 6_Benchmarks**
- Gemini 2.5 Flash (0,30 / 2,50 / 0,03 USD), Flash-Lite, Batch, Embedding: kiểm tra 08/10/2026 tại https://ai.google.dev/gemini-api/docs/pricing.
- Các dòng giá dễ đổi đánh dấu ⏳. Có kịch bản giá LLM ×2 (GM còn 67,4%).

**[x] 9. One-Pager — 3 khối, mọi số khớp Excel**
- Khối 1 Pricing, khối 2 GTM, khối 3 Evidence trên 1 trang A4 (`NguyenVanSon_Day22_onepager.pdf`).
- Các con số trong PDF được lấy từ kết quả tính lại của file Excel và đối chiếu với script Python độc lập (khớp).

**[~] 10. Đã chạy ít nhất 2 prompt ở §4.7 và ghi lại accept/reject**
- Prompt 4.7.1 (Cost/Job Stress Test): 10 ACCEPT, 4 PARTIAL, 1 REJECT.
- Prompt 4.7.2 (Value Metric Challenger): 4 ACCEPT, 1 PARTIAL, 1 REJECT.
- Chi tiết: `NguyenVanSon_Day22_AI_critique_log.md`.
- **Lưu ý:** hai prompt được chạy trong một phiên Claude Code, không phải do Sơn tự chạy. Sơn cần đọc lại và tự chạy lại ít nhất một prompt trước khi nộp.

---

## BÀI TEST NGƯỜI LẠ (2 phút, 3 câu hỏi)

**Chưa thực hiện.** Chưa đưa One-Pager cho nhóm khác đọc. Dưới đây là câu trả lời tự kiểm tra từ chính One-Pager, không thay thế bài test thật.

1. **Bán gì, cho ai, tính tiền theo đơn vị nào?** AI sàng lọc CV cho SME 50–300 nhân sự, tính theo shortlist hoàn tất: 500.000 ₫/khách/tháng + 800.000 ₫/shortlist.
2. **Có lãi trên mỗi đơn vị không, số nào chứng minh?** Có theo COGS: 235.650 ₫ chi phí/job, GM 75,4%. Nhưng margin đầy tải chỉ 7,4% ở 8 khách, và containment 80% chưa đo.
3. **Tiếp cận khách qua đâu, vì sao?** PLG qua email forward và Google Sheets add-on, vì pain moment là 9h sáng thứ Hai trong Gmail/Excel; Sales-Led lệch ngân sách CAC 29,6×.

---

## ĐIỂM CHƯA ĐẠT / THỪA NHẬN

1. **Không có dữ liệu thật.** Chưa có eval, pilot hay khách hàng. Containment 80%, 300 CV/requisition, 4 requisition/khách/tháng, 4 phút sàng lọc/CV, lương HR 12 triệu và ARPU đều là giả định.
2. **Margin đầy tải 7,4% ở 8 khách.** GM theo COGS đạt, nhưng chưa nuôi nổi overhead ở quy mô này.
3. **Giá chưa kiểm chứng.** Giá bán nằm trên dải neo giá trị (10–25% tiết kiệm) và chỉ nằm trong dải neo nhân công theo lương giả định. Giá hoặc containment giảm một nửa đẩy GM xuống 57,6%.
4. **Rủi ro pháp lý và bảo mật.** Tuyển dụng bằng AI là lĩnh vực nhạy cảm (Lab 21). Chưa có chuyên gia xác nhận phần dữ liệu cá nhân và quy định AI. Add-on Gmail có thể cần xác minh bảo mật của Google (chưa có báo giá).
5. **Nguồn chưa kiểm tra lại.** Số ICONIQ 2026 (6.300 USD/opportunity) lấy từ `HUONG_DAN_LAB.md`, chưa tra nguồn gốc. Các căn cứ pháp lý lấy từ báo cáo Lab 21.
6. **Không có template gốc.** File `day22_monetization_model.xlsx` và template `.docx` không được cung cấp; Excel được dựng lại theo `HUONG_DAN_LAB.md`.
7. **Excel chưa có giá trị lưu sẵn.** File chứa công thức; Excel/Google Sheets tính khi mở. Công cụ xem trước đơn giản có thể hiển thị ô trống.

---

## FILES TRONG THƯ MỤC NÀY

| File | Nội dung |
|---|---|
| `HUONG_DAN_LAB.md` | Hướng dẫn lab (bản gốc của khóa, không sửa) |
| `NguyenVanSon_Day22_model.xlsx` | Model 7 tab: README, Cost/Job, Pricing, Value Metric, Channel Fit, 90-Day Plan, Benchmarks |
| `NguyenVanSon_Day22_onepager.pdf` | Monetization One-Pager (1 trang A4, 3 khối) |
| `NguyenVanSon_Day22_AI_critique_log.md` | Log 2 prompt phản biện kèm accept/reject |
| `NguyenVanSon_Day22_checklist.md` | File này |
