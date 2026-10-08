# Lab Day 22 — Monetization & GTM

- **Họ và tên:** Nguyễn Văn Sơn
- **MSSV / mã học viên:** 2A202602744
- **Lớp:** Track 1
- **Ngày:** 08/10/2026
- **Sản phẩm:** **SàngCV** — AI sàng lọc CV và lập shortlist có dẫn chứng, bán cho SME Việt Nam (50–300 nhân sự)

> **Câu chốt của lab:** "Chạy được là bài toán kỹ thuật. Bán được là bài toán sinh tồn."

---

## 1. Tóm tắt một đoạn

SàngCV thay phần việc **sàng lọc CV sơ loại** của HR executive. AI chấm CV theo rubric lấy từ JD, trả top-N ứng viên kèm trích dẫn nguyên văn từ CV, còn **recruiter luôn là người quyết định cuối** (AI không tự loại ứng viên). Khách trả tiền theo **shortlist hoàn tất**, cộng một phí nền hằng tháng.

## 2. Các con số chính

Mọi số truy được về `NguyenVanSon_Day22_model.xlsx`. Kịch bản cơ sở: 8 khách × 4 vị trí/tháng, containment 80%, tỷ giá 26.000 ₫/USD.

| Hạng mục | Giá trị | Ô tham chiếu |
|---|---|---|
| Job (1 đơn vị tính tiền) | 1 shortlist hoàn tất (`SHORTLIST_ACCEPTED`) | 1_Cost_Job · S1 |
| Value Metric | Hybrid: 500.000 ₫/khách/tháng + 800.000 ₫/shortlist | 2_Pricing · Offer |
| Cost/Job (COGS, 4 thành phần) | 235.650 ₫ (9,06 USD) | 1_Cost_Job |
| Cost/Job đầy đủ (5 thành phần) | 885.650 ₫ (34,06 USD) | 1_Cost_Job |
| Giá sàn (3 × COGS/job) | 706.951 ₫ — giá bán 800.000 ₫ **đạt** | 2_Pricing |
| Gross Margin (theo COGS) | **75,4%** (mục tiêu ≥60% — đạt) | 2_Pricing |
| Margin đầy tải | **7,4%** (chưa nuôi nổi overhead ở 8 khách) | 2_Pricing |
| Breakeven containment (GM 60%) | 43,3% so với giả định 80% (chưa đo) | 2_Pricing |
| Ngân sách CAC | 1.064 USD/khách | 4_Channel_Fit |
| Deal/AE/ngày | 0,59 (khả thi về số học) | 4_Channel_Fit |
| CAC Sales-Led thực tế | 31.500 USD, **gấp 29,6×** ngân sách | 4_Channel_Fit |
| Kênh đã chốt | **PLG** (25/30) | 4_Channel_Fit |

## 3. Cấu trúc thư mục

| File | Nội dung |
|---|---|
| `README.md` | File này |
| `HUONG_DAN_LAB.md` | Hướng dẫn lab của khóa (bản gốc, không sửa) |
| `NguyenVanSon_Day22_model.xlsx` | Model 7 tab (xem mục 4) |
| `NguyenVanSon_Day22_onepager.pdf` | Monetization One-Pager, 1 trang A4, 3 khối: Pricing · GTM · Evidence |
| `NguyenVanSon_Day22_AI_critique_log.md` | Log 2 prompt phản biện (§4.7.1, §4.7.2) kèm accept/partial/reject |
| `NguyenVanSon_Day22_checklist.md` | Final Checklist 10 mục và các điểm chưa đạt |

## 4. Bản đồ các tab Excel

| Tab | Vai trò |
|---|---|
| `0_README` | Quy ước màu, tỷ giá, 5 con số bắt buộc (liên kết công thức) |
| `1_Cost_Job` | Cost/Job đủ 5 thành phần: API, Infra, HITL, Retry, Overhead; chia cho job **hoàn thành** |
| `2_Pricing` | Giá sàn/trần, GM, breakeven containment, độ nhạy theo containment và số khách, stress test ×2 |
| `3_Value_Metric` | Chấm Attribution × Autonomy, Decision Note, 3 benchmark có link |
| `4_Channel_Fit` | Ngân sách CAC, deal/AE/ngày, CAC Sales-Led và PLG, scorecard 3 kênh, Pain Moment |
| `5_90Day_Plan` | Kế hoạch 90 ngày có KPI và điều kiện dừng; Evidence Pack kèm deadline; điểm chưa đạt |
| `6_Benchmarks` | Bảng giá tham chiếu kèm ngày kiểm tra và nguồn |

**Quy ước màu:** vàng = giả định nhập tay · xám = công thức (không sửa) · xanh = đạt ngưỡng · đỏ = chưa đạt.

Cách dùng: đổi giá trị ở các ô vàng của `1_Cost_Job` (số khách, containment, token, giá API...) hoặc `2_Pricing` (phí nền, giá/shortlist), mọi tab khác tự tính lại.

## 5. Điểm cần đọc trước (thành thật về giới hạn)

1. **Chưa có dữ liệu thật.** Chưa có eval, pilot hay khách hàng. Containment 80%, 300 CV/vị trí, giá, 4 phút sàng lọc/CV và lương HR 12 triệu/tháng đều là **giả định**.
2. **Margin đầy tải 7,4%.** GM theo COGS đạt, nhưng Cost/Job đầy đủ xấp xỉ giá bán ở 8 khách. Cần ≥12–20 khách để margin đầy tải lên 24–37%.
3. **Biến số nguy hiểm nhất là doanh thu.** Giá hoặc containment giảm một nửa đẩy GM xuống 57,6% (<60%); chi phí LLM không phải rủi ro chính.
4. **Rủi ro pháp lý.** Dùng AI trong tuyển dụng là lĩnh vực nhạy cảm; chưa có chuyên gia xác nhận phần dữ liệu cá nhân và quy định AI.
5. **Critique log do AI hỗ trợ.** Hai prompt được chạy trong một phiên Claude Code; cần người làm bài đọc lại và tự chạy lại ít nhất một prompt.
6. **Bài test người lạ chưa thực hiện.**
7. **Không có template gốc.** File `day22_monetization_model.xlsx` và `.docx` của lab không được cung cấp; Excel được dựng lại theo `HUONG_DAN_LAB.md`.
8. **Excel chưa lưu sẵn giá trị tính.** Excel/Google Sheets tự tính khi mở file; trình xem trước đơn giản có thể hiển thị ô trống.

## 6. Nguồn giá (kiểm tra trực tiếp ngày 08/10/2026)

- Gemini 2.5 Flash / Flash-Lite / Batch / Embedding: https://ai.google.dev/gemini-api/docs/pricing
- Workable (AI screening, 0,12 USD/lần sàng lọc): https://www.workable.com/pricing
- Manatal (theo ghế): https://www.manatal.com/pricing
- Intercom Fin (0,99 USD/outcome): https://fin.ai/pricing/

Số ICONIQ 2026 (cost per opportunity 6.300 USD, phân khúc SMB) lấy từ `HUONG_DAN_LAB.md`, chưa tra lại nguồn gốc.

## 7. Việc tiếp theo (kế hoạch, chưa thực hiện)

| Việc | Deadline dự kiến |
|---|---|
| Procurement Q&A bằng văn bản | 20/10/2026 |
| Eval 100 CV có nhãn (recall top-10, trích dẫn khớp, parse, tỷ lệ chọn theo nhóm) | 25/10/2026 |
| Liên hệ khách pilot | trước 05/11/2026 |
| Pilot Report | 10/12/2026 |
