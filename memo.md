# Memo Teardown — Claude (Anthropic)

**Họ tên:** Ngô Lê Thùy Tiên · **MSV:** 2A202602614

**Vì sao chọn sản phẩm này:** Claude là sản phẩm AI-native có dữ liệu công khai đầy đủ (bài công bố, Product Hunt, trang Cowork) để dựng timeline, và mình có thể tự dùng thử gói free. Mình khoanh hẹp vào hai mảng có use case rõ: claude.ai (đọc/làm việc với tài liệu) và Claude Code/Cowork (agent làm việc). Lưu ý: đây là sản phẩm của chính hãng làm ra AI hỗ trợ mình, nên mình đã cố giữ nhận định có nguồn và đánh dấu chỗ suy luận.

## §1. Timeline các cập nhật lớn

| Thời điểm | Cập nhật | Context lúc đó | Nguyên lý |
|---|---|---|---|
| 11/07/2023 | **Claude 2**: context 100K, mở claude.ai public beta. [nguồn](https://www.anthropic.com/news/claude-2) | ChatGPT đã chiếm mindshare, GPT-4 ra từ 03/2023 với context 8K/32K; Anthropic chưa có app chat cho người dùng cuối | **x10 trên một trục hẹp + chọn vấn đề đáng giải**: đọc "cả một cuốn sách" thay vì vài trang là việc người dùng tài liệu dài thật sự cần; đồng thời mở kênh B2C để học từ user |
| 21/06/2024 | **Artifacts** (ra cùng Claude 3.5 Sonnet): code, tài liệu, giao diện hiện ở khung riêng cạnh chat. [nguồn](https://www.anthropic.com/news/claude-3-5-sonnet) | Model đã đủ tốt, chat thuần dễ thành commodity | **Tránh làm "wrapper mỏng" (feature chờ bị hấp thụ)**: thay vì chỉ trả lời, Claude thành nơi làm việc nơi user sửa và lặp, kết quả ở lại trong sản phẩm |
| 22/10/2024 | **Computer use** (public beta): Claude xem màn hình, bấm, gõ. [nguồn](https://www.anthropic.com/news/3-5-models-and-computer-use) | Đối thủ chủ yếu trả lời bằng chữ; chính Anthropic nhận là còn "cumbersome and error-prone" | **"Nếu model làm được, model sẽ làm" + vòng lặp học**: chính hãng model đưa khả năng điều khiển máy vào model, ăn vào lớp mà các agent wrapper đang bán; ra sớm khi còn kém để học từ dev |
| 25/11/2024 | **MCP**: chuẩn mở nối model với công cụ/dữ liệu. [nguồn](https://www.anthropic.com/news/model-context-protocol) | Mỗi AI app tự viết tích hợp riêng, model bị "cô lập" khỏi data | **"Nếu model làm được, model sẽ làm" (hấp thụ lớp tích hợp)**: chuẩn hóa connector để các wrapper chỉ làm tích hợp mỏng mất lý do tồn tại; giá trị còn lại nằm ở domain |
| 24/02/2025 → 22/05/2025 | **Claude Code**: research preview (02/2025), GA + SDK + IDE (05/2025). [02/2025](https://www.anthropic.com/news/claude-3-7-sonnet) · [05/2025](https://www.anthropic.com/news/claude-4) | Dev là tệp dùng AI sớm và trả tiền nhiều; thị trường đang chuyển từ autocomplete sang agent | **Vertical AI = AI Expert + Domain Expert**: dựng agent theo đúng workflow lập trình viên; **vòng lặp học** vì Anthropic tự là user |
| 09/04/2025 | **Claude Max** ($100/$200 mỗi tháng, usage 5x/20x Pro). [giá và mức dùng: trang chính thức](https://support.claude.com/en/articles/11049741-what-is-the-max-plan) · [ngày ra mắt: ppc.land](https://ppc.land/anthropic-launches-new-max-plan-for-claude-ai) · [Bloomberg](https://www.bloomberg.com/news/articles/2025-04-09/anthropic-to-offer-200-monthly-claude-chatbot-subscription) | Người dùng nặng hay đụng rate limit; "top request" của nhóm active nhất | **Phán đoán và đánh đổi (định giá)**: chọn phục vụ nhóm dùng nặng (x10 giá trị) bằng gói trả nhiều, đánh đổi lấy nguồn thu ổn định và bớt rate limit |
| 12/01/2026 | **Cowork**: agent kiểu Claude Code có GUI cho knowledge worker. [bài của Simon Willison, 12/01/2026](https://simonwillison.net/2026/Jan/12/claude-cowork/) (dẫn tới thông báo chính thức của Anthropic) | Claude Code đã chứng minh mô hình agent ở dev; văn phòng chưa có agent tương đương | **Vertical AI mở rộng sang domain mới**: giữ lõi agent (AI expert), đổi domain sang marketing/finance/legal; moat chỉ bền nếu domain expertise được đưa vào hệ thống |

**Vì sao chọn những mốc này:** mình giữ các mốc làm đổi cách sản phẩm được dùng hoặc được bán (mở app chat, Artifacts, agent, chuẩn mở, gói giá, tệp mới). Mình loại các lần ra model mới (Claude 3, 3.7, họ 5.x) vì đó là tiến bộ của model, chưa phải quyết định sản phẩm; loại add-in Office/Claude Design vì là hệ quả của Cowork; loại việc nới context lên 200K vì chỉ là tăng số trên trục đã có ở mốc 1.

## §2. Tệp user & JTBD

### So sánh early adopters và tệp hiện tại

| | Early adopters | Tệp hiện tại | Cột mốc gây dịch chuyển |
|---|---|---|---|
| **A. Dev** | *Backend/full-stack dev ở startup nhỏ, theo dõi AI Twitter, dán code vào claude.ai để debug, dùng Artifacts dựng thử UI/script nhanh (2024)* | *Dev có kinh nghiệm (cả trong công ty vừa/lớn) chạy Claude Code trong terminal/VS Code để giao cả task nhiều file; dev dùng nặng trả $100–200/tháng cho Max. Ví dụ người thật: tác giả bài ["Dumping Cursor for VSCode + Claude Code"](https://danielmiessler.com/blog/dumping-cursor-for-vscode-claude-code)* | Mốc 2 (Artifacts) → mốc 5 (Claude Code) → mốc 6 (Max) |
| **JTBD A** | "Khi mình bị kẹt một lỗi hoặc cần dựng nhanh một đoạn code, mình muốn có người xem giúp ngay, để khỏi mất cả buổi tra cứu." | "Khi mình cần thêm tính năng hoặc refactor đi qua nhiều file, mình muốn giao việc rồi chỉ review diff, để dành sức cho quyết định thiết kế." | |
| **Cách cũ A** | Google/Stack Overflow, copy code qua lại với ChatGPT | Công cụ AI trong IDE (autocomplete, chat từng đoạn) và tự điều phối từng bước | |
| **B. Người làm việc với tài liệu** | *Analyst/luật sư/nhà nghiên cứu cần đọc tài liệu dài (hợp đồng, paper); dùng claude.ai vì 100K context (2023)* | *Nhân sự operations, marketing, finance, legal không viết code, dùng Cowork giao việc nhiều bước trên file (theo các bài: phần lớn dùng Cowork là công việc ngoài kỹ thuật)* | Mốc 1 (Claude 2) → mốc 7 (Cowork) |
| **JTBD B** | "Khi mình có tài liệu dài phải hiểu trong một buổi chiều, mình muốn hỏi thẳng nó, để khỏi đọc từng trang." | "Khi tuần nào cũng có những việc lặp nhiều bước (đối soát, memo, deck từ transcript), mình muốn giao cả chuỗi rồi nhận kết quả, để khỏi làm tay từng bước." | |
| **Cách cũ B** | Đọc tay, ghi chú, nhờ đồng nghiệp tóm tắt | Excel/Office thủ công, macro, chat AI từng lượt rồi tự ghép | |

**Dịch chuyển segment:** claude.ai ban đầu hút người đọc tài liệu dài (mốc 1). Mốc 5 đưa Claude vào đúng workflow developer. Mốc 7 lấy lại mô hình agent đó cho người không viết code. Mốc 6 cho thấy nhóm dev dùng nặng đủ lớn để cần gói giá riêng.

### Switching cost, 4 forces

**Tệp A: dev từ công cụ AI trong IDE (ví dụ Cursor) sang Claude Code**

| Lực | Nội dung (kèm nguồn) |
|---|---|
| **Push** | Các bài "why I switched" nói công cụ cũ vướng ở tác vụ phức tạp/refactor lớn; nhiều dev thấy trả thêm một subscription cho công cụ nữa là thừa nếu đã có Claude |
| **Pull** | Agent làm xong cả task trên toàn repo, "step function" về năng lực (lời một số dev); dùng được trong VS Code/terminal; MCP và SDK nối vào quy trình sẵn có |
| **Habit** | Quen editor, phím tắt, rule đã viết sẵn; nhiều dev vẫn dùng song song cả hai công cụ (cho vòng phản hồi ngắn, chỉnh UI) |
| **Anxiety** | Giới hạn dùng khó đoán: người dùng Max báo hết quota nhanh, phàn nàn thiếu dashboard theo dõi, và Anthropic từng siết giới hạn mà không báo trước (TechCrunch, 07/2025); sau đó có đợt tăng gấp đôi hạn mức 5 giờ (05/2026) |

Nhận xét: không có dữ liệu bị khóa trong Claude Code, và nhiều dev dùng song song, nên switching cost thấp. Giữ user ở lại chủ yếu là **Pull (chất lượng agent)**, cộng thêm ít thói quen.

**Tệp B: người làm văn phòng từ làm tay/chat thường sang Cowork**

| Lực | Nội dung |
|---|---|
| **Push** | Việc lặp, nhiều bước, chat chỉ trả lời từng lượt; ví dụ HubSpot nói một dự án nhiều tuần xong trong 3 giờ (trang khách hàng của Anthropic) |
| **Pull** | Giao mục tiêu, agent tự đi qua file và công cụ; có desktop/web/mobile |
| **Habit** | Quy trình, template, công cụ Office đã quen |
| **Anxiety** | Agent đụng file/tài khoản thật; giao diện lộ file trung gian (HTML/JS) gây rối; finance cần audit trail và quản trị dữ liệu; phải có gói trả phí |

### Câu phản biện CP2
**Lực nào giữ user mạnh nhất, và nếu mất thì sao?** Với tệp A là **Pull**: chất lượng agent của Claude Code. Vì Habit yếu và không có dữ liệu bị khóa, nếu đối thủ (Cursor, công cụ khác) làm agent ngang hoặc tốt hơn, hoặc giới hạn dùng khiến user bực quá mức, thì họ rời đi rất nhanh (thử và so sánh qua lại là chuyện thường). Đây là lý do mốc 4 (MCP) và Claude Code SDK quan trọng: chúng tạo lớp giữ chân bằng hệ sinh thái, ít phụ thuộc vào điểm benchmark từng tháng.

**Nguồn dùng cho §2 (nguồn thứ cấp):** [greythr thread](https://community.greythr.com/t/is-cursor-still-your-primary-ai-coding-tool-or-have-you-switched-to-claude-code-or-gemini-cli-why/25403) · [Miessler](https://danielmiessler.com/blog/dumping-cursor-for-vscode-claude-code) · [TechCrunch 07/2025](https://techcrunch.com/2025/07/17/anthropic-tightens-usage-limits-for-claude-code-without-telling-users/) · [techbloat](https://www.techbloat.com/why-claude-code-users-keep-hitting-usage-limits-and-what-anthropic-changed-in-2026.html) · [The New Stack, Cowork GA](https://thenewstack.io/anthropic-takes-claude-cowork-out-of-preview-and-straight-into-the-enterprise/) · [Rillet, Cowork cho finance](https://www.rillet.com/blog/claude-cowork-finance) · [HubSpot case](https://claude.com/customers/hubspot-qa)

## §3. Ba dự đoán hướng đi (6–12 tháng tới)

**D1 (mở rộng segment).**
- **Dự đoán:** Anthropic sẽ đóng gói agent theo ngành (legal, finance, sales) trên nền Cowork/Claude Code, thay vì chỉ một agent chung.
- **Lập luận:** Mốc 5 cho thấy Vertical thắng khi bám đúng workflow của dev; mốc 7 nhắm thẳng marketing/sales/legal/finance. Ở §2, tệp B còn nhiều Anxiety (audit trail, sai sót), nên gói có sẵn quy trình của ngành giúp giảm lo.
- *Kiểm chứng được:* trước 10/2027 có gói/plug-in theo ngành do Anthropic hoặc đối tác phát hành.

**D2 (thay đổi mô hình kiếm tiền).**
- **Dự đoán:** Giá sẽ chuyển từ "gói theo mức dùng" sang gói cộng tín dụng/giá theo lượng việc agent làm.
- **Lập luận:** Mốc 6 (Max) định giá theo nhóm dùng nặng vì giới hạn là nỗi bực lớn nhất; ở §2, Anxiety của tệp A xoay quanh quota khó đoán khi agent chạy dài (mốc 3, 5, 7). Giá model đang giảm (trang tin Anthropic 09/2026: Opus 5.5 rẻ hơn 40% so với Fable 5.1), nên có dư địa đổi cách tính.
- *Kiểm chứng được:* trước 10/2027 có thay đổi giá/hạn mức nhắm riêng vào agent chạy nền.

**D3 (đe dọa từ Big Tech).**
- **Dự đoán:** Microsoft và Google sẽ nhúng agent làm việc nhiều bước vào Office/Workspace; Anthropic phản ứng bằng cách đi vào trong chính các ứng dụng đó (add-in Word/Excel) và dựa vào MCP/connector thay vì tranh giành nơi người dùng làm việc.
- **Lập luận:** Habit của tệp B (§2) là Office quen thuộc, nên khó kéo họ rời đi; mốc 4 (MCP) là cách Claude trở thành lớp nối thay vì ứng dụng độc lập. Add-in Word/Excel đã thấy trong danh sách launch trên Product Hunt (04/2026, nguồn thứ cấp).
- *Kiểm chứng được:* trước 10/2027 Microsoft/Google công bố agent đa bước tích hợp sẵn trong Office/Workspace, và Anthropic mở rộng add-in/connector.

### Câu phản biện CP3
**Tự tin nhất: D2.** Xu hướng tăng dần (Max → quota → tăng hạn mức) đã thấy rõ trong dữ liệu.
**Giả định làm nó gãy:** chi phí chạy agent tiếp tục giảm nhanh đến mức Anthropic giữ gói theo mức dùng cho đơn giản, hoặc đối thủ tặng agent trong gói rẻ hơn làm Anthropic phải theo.

## §4. AI Log

| Việc | AI làm hay bạn làm? | Bạn kiểm chứng/phán đoán lại thế nào? |
|---|---|---|
| Chọn sản phẩm | Bạn quyết định chọn Claude (ban đầu AI gợi ý Cursor); AI nêu ưu/nhược của hai lựa chọn | Mình chọn Claude vì đã quen dùng hơn Cursor. AI lưu ý rủi ro thiên vị khi phân tích sản phẩm của chính hãng làm ra AI hỗ trợ, nên mình giữ nhận định có nguồn và đánh dấu chỗ suy luận |
| Tìm nguồn và tổng hợp 7 mốc | AI (Claude Code, dùng công cụ tìm kiếm và đọc trang web) | Mình tự mở lại các link và đối chiếu ngày. 5 mốc có bài gốc anthropic.com (Claude 2, Claude 3.5 Sonnet/Artifacts, Computer use, MCP, Claude 3.7/Claude Code, cùng bài Claude 4 cho GA) đều mở được và khớp. Mình sửa lại ngày Claude Max (09/04/2025) và tìm thêm trang hỗ trợ chính thức cho giá. Link Cowork của Anthropic không mở được (chuyển sang trang tải app) nên mình thay bằng bài của Simon Willison, ngày 12/01/2026. Max chưa có ngày ra mắt từ nguồn chính thức, chỉ có từ nguồn thứ cấp |
| Chọn mốc nào giữ, loại mốc nào | AI đề xuất, bạn duyệt | Mình đồng ý với danh sách 7 mốc và lý do loại (model mới, add-in Office, nới context lên 200K) mà không thêm/bớt mốc nào |
| Gắn nguyên lý cho từng mốc | AI diễn giải theo các khái niệm nêu trong đề | Mình dán nội dung bài giảng (Vertical AI, "nếu model làm được thì model sẽ làm", phán đoán và đánh đổi, vòng lặp học) để AI đối chiếu. AI đổi lại nguyên lý của 6/7 mốc theo đúng cụm của bài giảng, riêng Claude Code giữ nguyên vì đã khớp. Mình đã đọc qua cột Nguyên lý sau khi sửa. Riêng "x10" không có trong đoạn mình dán nên giữ theo đề tổng quan, và lập luận ở MCP/Computer use là suy luận của AI |
| Mô tả tệp user, JTBD, 4 forces | AI suy luận từ bài viết và thread thứ cấp (chưa mở trực tiếp Reddit/G2) | Đây là suy luận, không phải dữ liệu user thật. Mình chưa bổ sung trải nghiệm cá nhân hay review khác |
| 3 dự đoán | AI soạn nháp từ §1–§2 | Mình đọc và giữ nguyên cả 3. Mình thấy D2 (giá) có cơ sở nhất; D3 dựa vào add-in Word/Excel thấy trên Product Hunt mà chưa mở nguồn gốc nên độ chắc thấp hơn |
