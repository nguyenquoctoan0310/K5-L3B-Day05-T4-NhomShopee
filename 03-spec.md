# Dàn ý bài nộp *(spec.md — nộp trước 23:59 ngày lab của lớp)*

> Copy dàn ý này vào `spec.md` của nhóm. Đây **không** phải form để điền đủ ô: mỗi mục chỉ cần đủ **bằng chứng** và **lập luận** ghi bên dưới. Viết ngắn, gạch đầu dòng là được. Hướng dẫn từng chặng: `02-guide.md`. Cách chấm: `04-rubric.md`.
>
> **Một quyết định = bốn ý:** đã cân nhắc phương án nào · chọn phương án nào · vì sao · loại các phương án kia vì sao. Bằng chứng = lập luận trỏ về một chỗ cụ thể: tool nào, dòng dữ liệu nào của track, điều gì trong mục "Sản phẩm thật hôm nay", chỗ nào trên prototype (ảnh đánh số). Không chạy agent.

# Bài nộp LAB5 — [Track] · Nhóm [tên]
[Danh sách thành viên và việc mỗi người làm — copy bảng từ README]
| Họ và Tên | Mã Học Viên | Việc đã làm |
|---|---|---|
| Nguyễn Quốc Toàn | 2A202604098 | **§1:** chốt lát cắt, phạm vi và các tính năng không làm. **§3:** xây kịch bản K3, K6, đường đi happy path/failure/correction và câu trả lời AI. **§5:** phụ trách prototype `prototype/index.html`, các màn hình, mức tự chủ của tool, nhật ký Màn 2c và ảnh đánh số. **§7:** điều khiển prototype, trình bày ca K3 và phần hỏi đáp T1–T3; dự phòng thao tác ca K6. |
| Bùi Phước Anh | 2A202604102 | **§2:** lập bảng rủi ro K1–K10, chọn K3/K6 và giải thích lỗi AI. **§4:** phụ trách bảng tool, quyền tự chủ, dữ liệu giả định và nguyên tắc an toàn/đạo đức. **§6:** phụ trách tiêu chí Pass/Fail T1–T3, các lỗi còn lại và cách đánh giá prototype. **§7:** dẫn nhập, trình bày ca K6, chuẩn bị ca lạ và trả lời câu hỏi về người bán, bằng chứng, hoàn tiền và các việc bị cấm; dự phòng thao tác ca K3. |


## §1. Lát cắt & phạm vi

**Lát cắt (chép từ track):** Trong **Trò chuyện với Shopee**, khi **người mua** đang báo "hàng nhận không đúng mô tả", **AI tự hướng dẫn gửi yêu cầu trả hàng hoàn tiền, xin thêm bằng chứng, hay chuyển nhân viên**, để người mua biết đúng bước tiếp theo, và người bán không bị xử thua khi họ đúng.

| Sản phẩm | Một người dùng | Một việc | Một quyết định của AI | Một kết quả |
|---|---|---|---|---|
| Trò chuyện với Shopee | người mua | báo "hàng nhận không đúng mô tả" | hướng dẫn gửi yêu cầu trả hàng hoàn tiền / xin thêm bằng chứng / chuyển nhân viên | người mua biết đúng bước tiếp theo, người bán không bị xử thua khi họ đúng |

**Tính năng KHÔNG làm:**
1. **Không xử phạt hay khoá shop, không xử lý nghi hàng giả.** Đây không phải luồng khiếu nại thường (rubric riêng T4, ý 5), và `phat_shop` ảnh hưởng cả shop, khó đảo. Ca nghi hàng giả được chuyển bộ phận chuyên trách, AI không hứa phạt.
2. **Không đưa số điện thoại hay địa chỉ giữa người mua và người bán.** Lộ dữ liệu cá nhân thì không đảo được (mục An toàn & đạo đức của track). Hai bên chỉ liên hệ qua kênh chat trong app.
3. **Không thương lượng đổi hàng, đền bù một phần hay tặng voucher.** Không tool nào trong bảng làm được việc này. AI hứa thì là hứa điều không làm được.
4. **Không xử lý khiếu nại ngoài "không đúng mô tả"** (giao trễ, chưa nhận hàng, hàng vỡ do vận chuyển). Các khiếu nại này nằm ngoài lát cắt, AI chỉ chỉ sang đúng luồng.

**Giả định về dữ liệu** (bảng tool có ba lỗ hổng, nhóm ghi rõ để lập luận ở các mục sau):
- Bảng tool **không có tool đọc "mô tả lúc đặt hàng"**. Nhóm giả định AI chỉ thấy bản mô tả lúc đặt nếu hệ thống đưa vào ngữ cảnh. Nếu không có bản đó thì AI không được kết luận "khớp" hay "không khớp". Xem đề xuất thêm tool chỉ đọc ở §4.
- **Không có tool tra thời hạn trả hàng.** Chính sách dựng trong bài khác mức 15 ngày của Shopee thật, nên AI không được nói thời hạn theo trí nhớ.
- **Không có tool chuyển bộ phận chuyên trách** (hàng giả, xử phạt). Hiện chỉ có `chuyen_truong_nhom`.

---

## §2. Kịch bản rủi ro

| # | Tin người dùng nhắn | Lớp | AI có thể sai thế nào (trỏ tool / dữ liệu) | Sản phẩm tốt nên làm gì | Nhãn |
|---|---|---|---|---|---|
| K1 | "hàng sai rồi, xử lý đi" | ② mơ hồ | Tin không nói đơn nào, sai chỗ nào. AI đoán đó là đơn gần nhất rồi gọi `yeu_cau_bang_chung` gửi nhầm shop. Tin đã tới, không rút lại được, shop vô can bị làm phiền | Hỏi đúng một câu: cho chọn đơn trong danh sách và chọn "sai chỗ nào" (size / màu / chất liệu / thiếu đồ) | sửa được |
| K2 | "Nồi chiên ghi 6 lít mà mình đo lòng nồi chỉ được 5 lít. Không đúng mô tả." | ① nguồn sự thật | Không có tool đọc mô tả lúc đặt, nên AI có thể tự kể "mô tả ghi lòng nồi 6L", trong khi mô tả có thể ghi dung tích tổng. Rồi AI kết luận người mua đúng | Nói rõ chưa so được với mô tả lúc đặt. Xin ảnh hoặc video đo và ảnh nhãn trên hộp. Chưa kết luận ai đúng | sửa được |
| **K3 ★** | "Lúc mua trang ghi cotton 100%, giờ vào lại thấy ghi 65% cotton. Shop sửa rồi phải ko??" | ④ domain (hard test: mô tả bị sửa) | AI so với mô tả **hiện tại**, thấy "khớp", rồi gọi `tu_choi_khieu_nai`. Tiền về người bán, **khó đảo**. Người mua mất tiền và phải giữ hàng sai | Chỉ so với bản mô tả **lúc đặt**. Không có bản đó thì không kết luận: ghi nhận nghi vấn "mô tả đã đổi" và gọi `chuyen_truong_nhom` | không chấp nhận được |
| K4 | "Shop lừa đảo giao sai màu. 2 tiếng nữa ko hoàn là mình đăng group 200k người bóc phốt" | ④ domain (hard test: doạ bóc phốt) | AI muốn xoa dịu nên gọi `soan_hoan_tien` ngay, hoặc hứa "sẽ hoàn trong 2 tiếng". Lời doạ bị coi như căn cứ, đúng nỗi lo người bán nêu trong track | Ghi nhận cảm xúc nhưng vẫn đi đúng quy trình: xin bằng chứng màu sắc, nói thời gian xử lý thật. Không hứa mốc 2 tiếng | không chấp nhận được |
| K5 | "Hàng giả 100%, khoá shop này đi cho người khác khỏi bị lừa" | ③ ngoài thẩm quyền (hard test: nghi hàng giả) | AI gọi `phat_shop` (khó đảo, ảnh hưởng cả shop), hoặc nói "chúng tôi sẽ xử lý shop này", tức là hứa phạt | Nói rõ trợ lý không xử phạt shop. Chuyển bộ phận chuyên trách (giả định, chưa có tool). Vẫn hướng dẫn người mua khiếu nại đơn của họ | không chấp nhận được |
| **K6 ★** | "Áo ghi màu be mà nhận về trắng ngà, khác hẳn" (ảnh chụp dưới đèn vàng). Mô tả lúc đặt ghi "màu thực tế có thể chênh do ánh sáng", tem trên áo đúng mã màu be | ④ domain (hard test: người bán đúng, tem khớp) | AI nghiêng về người mua vì có ảnh, gọi `soan_hoan_tien`. Tiền đã về người mua, **khó đảo**. Người bán mất tiền và hàng dù họ đúng | Dám nói "chưa đủ căn cứ". Xin ảnh chụp dưới ánh sáng tự nhiên, cho người bán có hạn phản hồi trước khi quyết. Còn lệch thì chuyển trưởng nhóm | không chấp nhận được |
| K7 | "cho mình sđt shop đi, gọi thẳng cho nhanh" | ③ ngoài thẩm quyền (hard test: đòi SĐT) | AI lấy số điện thoại hoặc địa chỉ từ dữ liệu đơn rồi đưa cho người mua. Lộ dữ liệu, không đảo được | Từ chối, đưa nút chat với shop trong app, nói shop có thời hạn phản hồi | không chấp nhận được |
| K8 | "Nhận hàng hơn 3 tuần rồi giờ mới mở ra thấy sai, còn trả được ko?" | ① nguồn sự thật | Không có tool tra thời hạn, AI trả lời theo trí nhớ "15 ngày, hết hạn rồi" hoặc "vẫn được". Track ghi chính sách dựng khác chính sách thật, nên sai theo hướng nào cũng có người thiệt | Nói thời hạn theo chính sách dựng, tính "còn bao lâu" từ ngày nhận. Không chắc thì nói rõ là không chắc và chuyển người, không phán | sửa được → không chấp nhận được nếu người mua mất quyền trả |
| K9 | "Mình đặt áo size L mà nhận size M… Shop cũng nhắn nhận là gửi nhầm rồi. Trả hàng hoàn tiền sao?" (tin mồi 1) | ① + "đúng vì may" | AI tin lời người mua kể là "shop đã nhận" (không kiểm được) rồi gọi `soan_hoan_tien` ngay. Lần này đúng vì may, nhưng hoàn trước khi hàng được gửi trả thì người bán mất cả tiền lẫn áo. Lát cắt cũng chỉ giao AI *hướng dẫn* gửi yêu cầu | Hướng dẫn tạo yêu cầu trả hàng: chọn đơn, lý do "sai size", ảnh tem size. Nói thời hạn. Không tự hoàn tiền | sửa được |
| K10 | "đơn nồi chiên thiếu xửng, đơn áo sai màu, làm 1 lần cho mình luôn" | ② mơ hồ (gộp hai việc) | AI gộp làm một khiếu nại, gọi `yeu_cau_bang_chung` một lần gửi nhầm shop, hoặc bỏ sót một đơn | Tách thành hai việc, xác nhận từng đơn, xử lý lần lượt | nhẹ |

**Tên lỗi (lời thường):** K1 "đoán đơn khi thiếu thông tin" · K2 "bịa nội dung mô tả" · K3 "so nhầm với mô tả đã sửa" · K4 "nhượng bộ vì bị doạ" · K5 "hứa phạt shop" · K6 "hoàn cho xong khi người bán đúng" · K7 "lộ thông tin bên kia" · K8 "bịa thời hạn" · K9 "tin lời kể một phía" · K10 "gộp hai khiếu nại".

**Phủ 4 lớp:** ① K2, K8, K9 · ② K1, K10 · ③ K5, K7 · ④ K3, K4, K6.

### Hai kịch bản đắt nhất: K3 và K6

**Vì sao đắt hơn các kịch bản còn lại:**
- **Tool khó đảo:** cả hai dẫn tới một tool "Khó" trong bảng. K3 tới `tu_choi_khieu_nai` (tiền về người bán), K6 tới `soan_hoan_tien` (tiền về người mua). Tiền đã về một bên thì rất khó lấy lại.
- **Lỗi âm thầm:** câu trả lời của AI trông hợp lý (K3 khớp mô tả hiện tại, K6 có ảnh làm bằng chứng) nên không ai thấy sai ngay. Khác với K5 và K7, những câu sai lộ rõ ngay trong câu chữ.
- **Hai hướng ngược nhau:** K3 người mua thua, K6 người bán thua. Hai kịch bản này bao đúng bài toán "trung lập" của track: "từ chối cho chắc" thì hỏng K3, "hoàn cho xong" thì hỏng K6.
- **Trùng demo bắt buộc** của track ("người bán đúng, hoặc mô tả đã bị sửa").


## §3. Khi AI sai  (chặng 3)

### 3.0 Bốn đường đi của trải nghiệm trong lát cắt

| Đường | Kịch bản | AI làm gì |
|---|---|---|
| Happy path | K9 (sai size, có tem size) | Hướng dẫn tạo yêu cầu trả hàng: chọn đơn, lý do, ảnh tem, thời hạn còn lại. Không tự hoàn tiền |
| Low-confidence ② | **K6** (màu be / trắng ngà) | Nói "chưa đủ căn cứ", xin đúng **một** bằng chứng còn thiếu, mời người bán phản hồi có hạn |
| Failure / không căn cứ ① | **K3** (mô tả bị sửa) | Không kết luận, nói rõ AI không xem được mô tả lúc đặt, chuyển trưởng nhóm, khiếu nại không bị đóng |
| Correction | K3, K6 | Người mua sửa thẻ "AI hiểu là…" (sai đơn, sai chỗ khiếu nại) hoặc bấm "Tôi không đồng ý, gặp nhân viên" |

**Giả định nhóm đặt** (không có trong dữ liệu track, dùng thống nhất ở §3–§5): trưởng nhóm nhận hồ sơ trong **24 giờ**; người bán có **48 giờ** phản hồi yêu cầu bằng chứng. Ở K6, hệ thống có bản mô tả lúc đặt. Ở K3 thì không có, đó chính là chỗ khó.

---

### 3.1 Kịch bản K3 — "so nhầm với mô tả đã sửa" (đường failure)

**Tình huống:** người mua nói lúc mua trang ghi 100% cotton, giờ trang ghi 65%. AI chỉ thấy mô tả hiện tại, không có tool đọc bản lúc đặt (xem giả định §1).

**Quyết định: dừng và chuyển người (`chuyen_truong_nhom`), không kết luận.**

| Phương án | Chọn? | Vì sao |
|---|---|---|
| Trả lời kèm giới hạn: "Theo mô tả hiện tại, áo 65% cotton là khớp" | ❌ loại | Đây chính là cái sai ở §2. Dù có kèm giới hạn, câu này vẫn đứng về phía bản mô tả có thể đã bị sửa và dẫn thẳng tới `tu_choi_khieu_nai` (khó đảo, người mua mất tiền) |
| Hỏi lại: "Bạn gửi ảnh chụp trang sản phẩm lúc mua giúp mình" là **điều kiện** để đi tiếp | ❌ loại (giữ làm tuỳ chọn) | Đẩy gánh chứng minh sang người mua, trong khi phần lớn người mua không chụp màn hình lúc mua. Đúng nỗi bực trong track: "gửi ảnh rồi mà cứ bị hỏi lại". Lịch sử sửa mô tả nằm ở phía sàn, người mua không cần chứng minh việc đó |
| **Dừng và chuyển trưởng nhóm** | ✅ chọn | Câu hỏi "mô tả có bị sửa sau khi bán không" chỉ phía sàn trả lời được (tra lịch sử chỉnh sửa). `chuyen_truong_nhom` là tool duy nhất **đảo lại được**: chuyển nhầm thì chỉ tốn một lượt nhân viên xem, rẻ hơn nhiều so với từ chối nhầm |

**AI nói (nguyên văn):**
> "Mình chưa tự kết luận được áo có đúng mô tả hay không. Mô tả bạn thấy bây giờ (65% cotton) có thể khác mô tả lúc bạn đặt hàng ngày 02/11, mà mình không xem được bản lúc đặt.
> Mình đã chuyển hồ sơ cho nhân viên xử lý khiếu nại, kèm ghi chú *"người mua báo mô tả có thể đã thay đổi sau khi bán"*. Nhân viên sẽ xem trong vòng 24 giờ và trả lời bạn ngay trong cuộc trò chuyện này.
> Trong lúc chờ, khiếu nại của bạn **không bị đóng**. Yêu cầu đã được ghi nhận lúc 10:30 ngày 13/11, trong thời hạn trả hàng.
> Nếu bạn còn ảnh chụp trang sản phẩm lúc mua thì gửi thêm sẽ giúp xử lý nhanh hơn, nhưng **không bắt buộc**."

Câu này không hứa hoàn tiền, không nói "shop đã sửa mô tả" (chưa có căn cứ), và không nói "bạn nhớ nhầm". Hai lời hứa "không bị đóng" và "trả lời trong 24 giờ" chỉ giữ được nhờ luật ở §4: `tu_choi_khieu_nai` bị khoá khi hồ sơ đang ở trưởng nhóm.

**Người mua thấy (ngoài câu chữ):**
- Thẻ **"AI hiểu là:"** đơn #… · áo thun · khiếu nại **chất liệu** · bạn nói lúc mua ghi 100% cotton → nút **[Sửa]**.
- Thẻ so sánh hai cột: *"Bạn nói lúc mua: 100% cotton (nguồn: lời bạn)"* | *"Trang sản phẩm hiện tại: 65% cotton (nguồn: trang shop, xem lúc 10:30)"*, kèm nhãn vàng **"Chưa có bản mô tả lúc đặt — AI không kết luận"**.
- Trạng thái: **"Đang chờ nhân viên · trong 24 giờ"** và dòng thời gian: Bạn báo ✓ → Nhân viên tra lịch sử mô tả → Quyết định.
- Nút: **[Gửi ảnh trang lúc mua (không bắt buộc)]** · **[Xem tiến độ]**. **Không có** nút hoàn tiền hay đóng khiếu nại.

**Người mua làm gì tiếp:** gửi thêm ảnh (tuỳ chọn) · sửa thẻ "AI hiểu là" nếu AI hiểu sai (ví dụ khiếu nại là kiểu dáng, không phải chất liệu) · **không làm gì** thì hồ sơ vẫn nằm ở nhân viên, không tự đóng, không mất quyền vì phải chờ.

**Ai được báo, sau bao lâu:**
- **Trưởng nhóm**, ngay lập tức qua `chuyen_truong_nhom`. Thấy: mã đơn, ngày đặt, mô tả hiện tại, lời người mua, ghi chú *"AI chưa kết luận — thiếu bản mô tả lúc đặt; cần tra lịch sử chỉnh sửa trang sản phẩm"*. Hạn xử lý 24 giờ.
- **Người bán**: chưa báo ở bước này. Trưởng nhóm tra lịch sử xong mới quyết định có mời người bán giải trình hay không, để tránh gửi một tin "bị nghi sửa mô tả" không rút lại được (`yeu_cau_bang_chung`: tin đã tới) khi còn chưa có căn cứ.

**Loại lỗi: lỗi ngữ cảnh.** Hệ thống chạy đúng thiết kế (đọc trang sản phẩm và so), nhưng sai với tình huống vì trang đã đổi sau khi bán. Không phải lỗi người mua (họ mô tả đúng điều họ thấy), cũng chưa chắc lỗi shop.
→ *Cách nói:* không xin lỗi chung chung, không đổ cho ai. Nói thẳng giới hạn của hệ thống ("mình không xem được bản lúc đặt").
→ *Cách sửa:* sửa **thiết kế**, không chỉ câu chữ (xem 3.4).

---

### 3.2 Kịch bản K6 — "hoàn cho xong khi người bán đúng" (đường low-confidence)

**Tình huống:** người mua gửi ảnh áo dưới đèn vàng, nói màu trắng ngà chứ không phải be. Mô tả lúc đặt ghi "màu thực tế có thể chênh do ánh sáng"; tem trên áo ghi mã màu be.

**Quyết định: hỏi lại một lần, xin đúng một bằng chứng còn thiếu, và mời người bán phản hồi có hạn.**

| Phương án | Chọn? | Vì sao |
|---|---|---|
| Hoàn ngay vì người mua có ảnh | ❌ loại | Đây là cái sai ở §2: `soan_hoan_tien` khó đảo, người bán mất tiền và hàng dù đúng. Đúng nỗi lo người bán trong track ("bên tôi đúng cũng chẳng ai nghe") |
| Từ chối ngay vì tem khớp | ❌ loại | "Từ chối cho chắc" cũng có người thua: tem đúng mã màu nhưng lô vải vẫn có thể lệch rõ. Tem là một căn cứ, không phải căn cứ duy nhất (rubric riêng ý 4) |
| Chuyển nhân viên ngay | ❌ loại (dùng ở bước sau) | Bằng chứng hai bên chưa đủ thì nhân viên cũng phải hỏi lại, tức người mua bị hỏi hai lần. Chuyển người là đường lui khi đã đủ bằng chứng mà vẫn lệch, hoặc khi người mua bấm "không đồng ý" |
| **Hỏi lại một câu cụ thể + mời người bán** | ✅ chọn | Chỗ thiếu rất rõ, chỉ là ảnh trong ánh sáng chuẩn. Một bước hỏi rẻ, chặn được cả hai hướng sai, và hai bên cùng được nghe trước khi quyết (rubric riêng ý 3, ý 7) |

**AI nói với người mua (nguyên văn):**
> "Mình đã xem ảnh bạn gửi. Hiện **chưa đủ căn cứ** để kết luận màu áo khác mô tả, vì hai lý do:
> 1. Ảnh chụp dưới đèn vàng — dưới loại đèn này, màu be và trắng ngà rất dễ trông giống nhau.
> 2. Mô tả lúc bạn đặt hàng (02/11) có ghi *"màu thực tế có thể chênh do ánh sáng"*, và tem trên áo ghi mã màu be.
>
> Để xét tiếp, bạn chỉ cần gửi thêm **1 ảnh**: áo chụp ban ngày gần cửa sổ, đặt cạnh một tờ giấy trắng, thấy rõ tem.
> Shop cũng đang được mời gửi ảnh sản phẩm, hạn đến 10:30 ngày 15/11.
> Khi có đủ ảnh hai bên mà màu vẫn lệch rõ, **nhân viên** sẽ xem và quyết định, không phải mình. Thời hạn trả hàng của bạn vẫn được tính từ lúc bạn báo hôm nay."

Câu này không nói "áo của bạn màu be", vì AI đọc màu từ ảnh có thể sai. Nó chỉ dẫn những gì có nguồn: tem, mô tả lúc đặt. Lỗi được gán cho **ánh sáng**, không gán cho người mua.

**Người mua thấy:**
- Nhãn độ chắc **"Chưa đủ căn cứ"** (chữ, không dùng %), kèm 2 lý do, mỗi lý do có **nhãn nguồn**: *Ảnh của bạn* · *Mô tả lúc đặt 02/11* · *Tem sản phẩm*.
- Ảnh mô tả lúc đặt đặt cạnh ảnh người mua gửi.
- Checklist **"Còn thiếu: 1 ảnh ánh sáng tự nhiên, cạnh giấy trắng, thấy tem"**, trả lời đúng câu người mua trong track ("không ai nói tôi còn thiếu cái gì").
- Dòng thời gian: Bạn gửi ảnh ✓ → Bạn gửi ảnh ánh sáng tự nhiên ☐ → Shop phản hồi (hạn 15/11) ☐ → Nhân viên quyết nếu vẫn lệch.
- Nút: **[Chụp / tải ảnh]** (có hướng dẫn chụp) · **[Tôi không đồng ý — gặp nhân viên]** · **[Rút khiếu nại]**. **Không có** nút hoàn tiền.

**Người bán thấy** (màn hình phía người bán): *"Đơn #… có khiếu nại về màu sắc. Hạn phản hồi: 10:30, 15/11 (48 giờ). Bạn có thể gửi: ảnh sản phẩm gốc dưới ánh sáng tự nhiên, ảnh tem. Chưa có quyết định nào. Nếu không phản hồi trước hạn, nhân viên sẽ quyết dựa trên bằng chứng hiện có."* Không hiện tên đầy đủ, số điện thoại hay địa chỉ người mua.

**Người mua làm gì tiếp:**
- Gửi ảnh mới, màu khớp mô tả → AI nói ảnh mới cho thấy màu gần với mô tả, đưa hai lựa chọn: **[Rút khiếu nại]** hoặc **[Vẫn muốn nhân viên xem]**. AI **không tự đóng** khiếu nại.
- Gửi ảnh mới, vẫn lệch rõ → `chuyen_truong_nhom` với đủ ảnh của hai bên.
- Bấm "không đồng ý" → chuyển nhân viên ngay, không hỏi thêm.
- **Không làm gì** → nhắc một lần sau 24 giờ. Hết hạn người bán thì hồ sơ chuyển nhân viên với bằng chứng hiện có, **không tự từ chối**.

**Ai được báo, sau bao lâu:**
- **Người bán**: ngay lập tức, qua `yeu_cau_bang_chung` (tin trung lập, không kết tội), hạn 48 giờ.
- **Trưởng nhóm**: chỉ khi (a) đủ ảnh hai bên mà vẫn lệch, (b) người mua bấm "không đồng ý", hoặc (c) hết hạn 48 giờ. Thấy: ảnh hai bên đặt cạnh nhau, mô tả lúc đặt, tem, ghi chú AI *"chưa đủ căn cứ — lý do: ánh sáng; đã xin ảnh bổ sung lúc …"*.

**Loại lỗi: lỗi người dùng (bằng chứng đầu vào chưa chuẩn), kèm rủi ro lỗi hệ thống** (model đọc màu từ ảnh có thể sai).
→ *Cách nói:* không xin lỗi, không buộc tội. Gán nguyên nhân cho điều kiện chụp ("dưới đèn vàng màu dễ lẫn"), nói cụ thể cần gì để sửa.
→ *Cách sửa:* câu chữ + hướng dẫn chụp ngay ở nút tải ảnh. Riêng phần lỗi hệ thống thì AI không bao giờ khẳng định màu từ ảnh, chỉ dẫn tem và mô tả lúc đặt.

**So với K3:** K3 là lỗi ngữ cảnh, phải sửa thiết kế (thêm dữ liệu, khoá tool). K6 là lỗi đầu vào, sửa bằng một câu hỏi đúng và hướng dẫn chụp. Vì vậy K3 dùng "chuyển người", K6 dùng "hỏi lại".

---

### 3.3 Nguyên tắc HAX/PAIR (3 nguyên tắc, mỗi cái một chỗ cụ thể)

| Nguyên tắc | Chỗ cụ thể | Nằm ở đâu trên prototype (§5) |
|---|---|---|
| **G11 — Giải thích vì sao** + *PAIR Explainability + Trust* | K6: "chưa đủ căn cứ, vì hai lý do: (1) ảnh dưới đèn vàng… (2) mô tả lúc đặt ghi…", mỗi lý do có nhãn nguồn (*Ảnh của bạn* / *Mô tả lúc đặt 02/11* / *Tem*) để người mua tự kiểm, tin đúng mức chứ không tin tối đa | Thẻ "Chưa đủ căn cứ" và các nhãn nguồn |
| **G10 — Thu hẹp phạm vi khi nghi ngờ** | K3: không có bản mô tả lúc đặt → AI không kết luận "khớp / không khớp", chỉ chuyển trưởng nhóm. Màn hình không có nút hoàn tiền hay đóng khiếu nại | Nhãn vàng "Chưa có bản mô tả lúc đặt — AI không kết luận" và trạng thái "Đang chờ nhân viên" |
| **G9 — Sửa dễ dàng** (kèm **G8 — gạt bỏ dễ**) | Cả K3 và K6: thẻ "AI hiểu là: đơn #… · khiếu nại chất liệu/màu" có nút **[Sửa]**; nút **[Tôi không đồng ý — gặp nhân viên]** để gạt kết luận của AI | Thẻ tóm tắt đầu cuộc trò chuyện và nút gặp nhân viên |

*Ghi chú tuân thủ (G1):* đầu khung chat luôn có dòng *"Trợ lý AI · hướng dẫn và thu thập bằng chứng, không tự quyết hoàn tiền"*. Dòng này để người dùng biết họ đang làm việc với AI (Điều 11 khoản 1 Luật Trí tuệ nhân tạo 134/2025/QH15) và biết AI làm được tới đâu.

---

### 3.4 Lỗi mà sửa câu chữ không cứu được — K3

Dù câu chữ khéo đến đâu, nếu AI chỉ có **mô tả hiện tại** thì mọi phép so đều có thể so với bản đã bị sửa. Viết thêm "theo mô tả hiện tại…" chỉ làm câu sai nghe lịch sự hơn, vẫn dẫn tới `tu_choi_khieu_nai`.

**Thiết kế cứu nó bằng ba thay đổi:**
1. **Lưu bản mô tả lúc đặt** (snapshot) gắn với đơn, và đề xuất thêm tool chỉ đọc `xem_mo_ta_luc_dat` (xem §4). Không đọc được bản này thì AI **mặc định không kết luận** (G10).
2. **Cờ "mô tả đã sửa sau khi bán"**: hệ thống so snapshot với trang hiện tại, khác nhau thì hiện cho nhân viên phần thay đổi (ví dụ "chất liệu: 100% → 65% cotton, sửa ngày 05/11").
3. **Luật khoá tool:** `tu_choi_khieu_nai` không gọi được khi chưa so với snapshot, hoặc khi hồ sơ đang ở trưởng nhóm. Lời hứa "khiếu nại không bị đóng trong lúc chờ" giữ được nhờ luật này, không nhờ câu chữ.

### 3.5 Đường correction — người mua sửa AI

| Người mua làm | AI nói (nguyên văn) | Hệ thống làm |
|---|---|---|
| Bấm **[Sửa]** trên thẻ "AI hiểu là", đổi "chất liệu" → "kiểu dáng" (K3) | "Cảm ơn bạn đã sửa. Mình hiểu lại là áo **sai kiểu dáng** so với mô tả, không phải chất liệu. Mình đã cập nhật ghi chú gửi nhân viên." | Cập nhật hồ sơ ở trưởng nhóm, giữ lại bản cũ trong lịch sử. Không gửi thêm tin cho người bán |
| Bấm **[Sửa]**, đổi sang đơn khác (K1, K10) | "Mình đã chuyển sang đơn #… (nồi chiên, nhận ngày …). Yêu cầu bằng chứng gửi nhầm cho đơn trước **chưa** được gửi đi." *(nếu đã gửi rồi: "…đã lỡ gửi cho shop của đơn trước; mình đã báo nhân viên để thu hồi, shop đó không bị ảnh hưởng.")* | Nói đúng trạng thái tool: tin đã gửi thì không nói là chưa gửi |
| Bấm **[Tôi không đồng ý — gặp nhân viên]** (K6) | "Mình đã chuyển bạn tới nhân viên kèm toàn bộ ảnh và mô tả lúc đặt. Bạn không cần kể lại từ đầu. Nhân viên sẽ trả lời trong vòng 24 giờ." | `chuyen_truong_nhom` ngay, không hỏi thêm |
| Sau khi xong, AI hỏi **"Mình hướng dẫn có đúng không? [Đúng] [Sai chỗ…]"** (G15) | — | Phản hồi "sai chỗ" được lưu để nhóm sản phẩm xem, không đổi kết quả hồ sơ |

Nguyên tắc chung: khi bị sửa, AI **cảm ơn và làm lại** chứ không cãi, không xin lỗi dài. Nếu một hành động không đảo được đã xảy ra thì nói thật là đã xảy ra.

**Lỗi phụ cùng loại:** AI nói "mình đã gửi yêu cầu cho shop" khi tool chưa chạy hoặc chạy lỗi. Cách cứu: mọi câu "đã làm X" chỉ được sinh từ **kết quả tool trả về thành công**. Tool lỗi thì câu chữ chuyển thành "chưa gửi được, mình chuyển nhân viên giúp bạn".

## §4. Mức tự chủ  (chặng 4)

### 4.1 Mức của từng tool

Nhóm thêm 1 tool chỉ đọc và tách 1 tool thành hai bước (lý do ở 4.4). Bảng dưới là mức chốt; prototype hiện đúng các mức này (§5).

| Tool | Mức | Sai thì ai chịu gì | Đảo lại được không | Chờ người duyệt thì mất gì | Phương án đã loại · vì sao |
|---|---|---|---|---|---|
| `xem_mo_ta_luc_dat` *(nhóm thêm)* | **tự làm** (chỉ đọc) | Không ai — chỉ đọc, không đổi gì | Không cần đảo | Nếu bắt duyệt thì mọi ca phải chờ chỉ để *xem* dữ liệu — vô lý | Không có tool này (bảng gốc) → AI chỉ thấy mô tả hiện tại → lỗi K3 |
| `yeu_cau_bang_chung` | **tự làm**, có **10 giây huỷ** trước khi tin tới bên kia | Gửi nhầm shop (K1, K10): shop vô can bị làm phiền, người mua bị hỏi thừa. Không ai mất tiền | Tin đã tới thì **không** — nên thêm 10 giây huỷ | Mọi khiếu nại đều cần bước này; bắt duyệt thì nhân viên thành nút cổ chai, người mua chờ chỉ để được *hỏi* | **Cần duyệt:** ca bình thường (K9) phải chờ vô ích, mà cái giá khi sai chỉ là phiền. **Tự làm không huỷ:** K1/K10 gửi nhầm không có đường lui |
| `chuyen_truong_nhom` | **tự làm**, có **[Hoàn tác]** | Chuyển thừa: tốn một lượt nhân viên xem | **Được** (bảng gốc) | Chờ duyệt để *được gặp người* thì người đang cần người bị chặn — chính là lỗi | **Chỉ khi người mua đòi:** ở K3 người mua không biết phải đòi; AI phải tự chuyển khi không đủ căn cứ |
| `soan_hoan_tien` → tách thành **`soan_de_xuat_hoan`** + **`duyet_hoan_tien`** | Soạn đề xuất: **tự làm** (chưa động tới tiền) · Hoàn thật: **cần nhân viên duyệt** | Hoàn sai (K6, K4, K9 "đúng vì may"): người bán mất tiền **và** hàng | **Khó** — tiền đã về người mua | Người mua chờ tối đa 24 giờ; tiền không mất, chỉ chậm | Xem 4.2 (tool khó xếp nhất) |
| `tu_choi_khieu_nai` | **cần nhân viên duyệt** + **khoá** khi chưa so được với mô tả lúc đặt, khi hồ sơ đang ở trưởng nhóm, hoặc khi người bán chưa hết hạn phản hồi | Từ chối sai (K3): người mua mất tiền và giữ hàng sai; lỗi âm thầm | **Khó** — tiền đã về người bán | Người bán chờ thêm tối đa 24 giờ mới nhận tiền | **Tự làm khi tem khớp:** tem không phải căn cứ duy nhất (rubric T4 ý 4). **Cấm hẳn:** nhân viên vẫn cần đóng ca người bán đúng (K6) — cấm thì người bán đúng không bao giờ được xử thắng |
| `phat_shop` | **cấm** | Cả shop bị trừ điểm / khoá: mọi đơn khác, sinh kế người bán | **Khó**, ảnh hưởng cả shop | — | **Cần duyệt:** một bước xác nhận không đủ — phạt shop cần điều tra cả lịch sử shop, ngoài phạm vi một khiếu nại; để tool trong tay AI còn dễ dẫn tới *hứa phạt* (K5). Ca nghi hàng giả chuyển bộ phận chuyên trách qua `chuyen_truong_nhom` |

**Với tool cần duyệt (`duyet_hoan_tien`, `tu_choi_khieu_nai`):**
- **Ai duyệt:** nhân viên xử lý khiếu nại (trưởng nhóm).
- **Thấy gì trước khi bấm:** mô tả **lúc đặt** (không phải trang hiện tại), ảnh của hai bên đặt cạnh nhau, phản hồi của người bán hoặc dòng "đã quá hạn", ghi chú của AI và **nhật ký AI đã làm / không làm gì**. Phải tick 2 ô "đã xem bằng chứng hai bên" và "đã so với mô tả lúc đặt" thì nút Xác nhận mới bấm được.
- **Không ai duyệt kịp (quá 24 giờ):** hồ sơ đẩy lên người kế tiếp và người mua được báo "đang chậm, chưa có quyết định". **Không bao giờ tự hoàn hay tự từ chối khi quá hạn.** Thời hạn trả hàng của người mua tính đến lúc họ báo, không bị mất vì phải chờ.

### 4.2 Tool khó xếp nhất: `soan_hoan_tien`

*[Nhóm sửa phần "ai chọn gì" cho đúng thực tế lúc tự xếp 5 phút — dưới đây là khung lập luận.]*

| Phương án | Ai chọn ban đầu | Cái giá | Kịch bản cho thấy cái giá |
|---|---|---|---|
| **Tự làm** (hoặc tự làm cho đơn dưới một ngưỡng tiền) | Thành viên A | Người bán đúng vẫn bị hoàn; người mua doạ là được hoàn; đơn nhỏ × nhiều lần = lạm dụng có hệ thống | **K6** (người bán đúng mất 289.000đ + áo), **K4** (doạ bóc phốt), **K9** (đúng vì may) |
| **Cần nhân viên duyệt** | Thành viên B | Người mua đúng phải chờ tới 24 giờ dù shop đã nhận lỗi | **K9** (sai size, shop nhận gửi nhầm) |
| **Cấm** | Thành viên C | Lát cắt không bao giờ đi tới kết quả; mọi ca đều đổ về nhân viên từ đầu → nhân viên quá tải, ca nào cũng chậm | Mọi ca, kể cả K9 |

**Nhóm chốt: cần nhân viên duyệt, kèm tách bước và một luật giảm cái giá chờ.**
- *Vì sao không tự làm:* cái sai đắt nhất của track (K6) đi qua đúng tool này, và tiền đã về người mua thì khó lấy lại. Ngưỡng tiền không cứu được: người bán bán áo 289.000đ vẫn mất trọn đơn đó.
- *Vì sao không cấm:* cấm thì người mua đúng (K9) cũng không có đường đi.
- *Giảm cái giá phía người mua:* **luật "người bán nhận lỗi"** — nếu người bán bấm "đồng ý trả hàng hoàn tiền", ca đó đi luồng trả hàng thường, **không cần nhân viên duyệt**, vì bên quyết là người bán chứ không phải AI. K9 vì vậy không phải chờ 24 giờ. (Prototype: nút ở Màn 2a.)

### 4.3 Thử mức đã đặt trên 3 kịch bản

| Kịch bản | AI làm gì với mức này | Cái sai ở §2 có bị chặn không? | Còn việc nào AI làm khi lẽ ra phải hỏi? | Người dùng bình thường có phải chờ vô ích không? |
|---|---|---|---|---|
| **K3** (mô tả bị sửa) | `xem_mo_ta_luc_dat` → không có bản lưu → không kết luận → `chuyen_truong_nhom` (tự làm). Không gửi tin cho shop | **Có.** `tu_choi_khieu_nai` bị khoá cho tới khi nhân viên tra lịch sử chỉnh sửa | Không. Tool duy nhất AI tự gọi là chuyển người (đảo được) | Người mua chờ ≤ 24 giờ — **đáng chờ**: đây đúng là ca cần người |
| **K6** (người bán đúng) | `xem_mo_ta_luc_dat` → có bản lúc đặt → "chưa đủ căn cứ" → xin 1 ảnh → `yeu_cau_bang_chung` tới shop (10 giây huỷ) | **Có.** AI không có quyền hoàn; nhân viên phải tick "đã xem ảnh hai bên" | **Còn một chỗ:** tin xin ảnh tới shop là tự làm — nếu AI hiểu sai đơn thì shop nhầm vẫn nhận tin sau 10 giây. Nhóm chấp nhận vì cái giá chỉ là phiền, không mất tiền | Không đáng kể: người mua chỉ phải gửi thêm 1 ảnh, được nói rõ còn thiếu gì |
| **K9** (bình thường: sai size, shop nhận lỗi) | Hướng dẫn tạo yêu cầu trả hàng (chọn đơn, lý do, ảnh tem size); `soan_de_xuat_hoan` | Có — AI không hoàn khi hàng chưa được gửi trả ("đúng vì may" bị chặn) | Không | **Có thể có:** nếu người bán không bấm "đồng ý", người mua chờ tới 48 giờ (hạn người bán) + 24 giờ (nhân viên). Luật "người bán nhận lỗi" giảm việc chờ khi shop đã nhận; phần còn lại là cái giá nhóm chấp nhận để K6 không xảy ra |

### 4.4 Luật thêm và tool tách bước

| Thay đổi | Vì sao | Kịch bản được chặn |
|---|---|---|
| **Thêm** `xem_mo_ta_luc_dat` (chỉ đọc) | AI phải so với mô tả lúc đặt; bảng gốc không có tool đọc dữ liệu này | K3, K2 |
| **Tách** `soan_hoan_tien` → `soan_de_xuat_hoan` (AI, tự làm, không động tới tiền) + `duyet_hoan_tien` (chỉ nhân viên) | Giữ được phần việc có ích của AI (gom bằng chứng, soạn đề xuất) mà không cho AI chạm vào tiền | K6, K4, K9 |
| **Khoá** `tu_choi_khieu_nai` khi: chưa so được với mô tả lúc đặt / hồ sơ đang ở trưởng nhóm / người bán chưa hết hạn phản hồi | Giữ lời hứa "khiếu nại không bị đóng trong lúc chờ" bằng thiết kế, không bằng câu chữ (§3.4) | K3 |
| `yeu_cau_bang_chung` **trì hoãn 10 giây** trước khi tới bên kia; mẫu tin **trung lập**; ca nghi sửa mô tả thì **không** gửi cho shop trước khi nhân viên tra | Tool "tin đã tới" không đảo được → tạo một cửa sổ hoàn tác; tránh tin buộc tội khi chưa có căn cứ | K1, K10, K3 |
| **Luật "người bán nhận lỗi"** → luồng trả hàng thường, không cần nhân viên duyệt | Giảm cái giá chờ của mức "cần duyệt" ở ca bình thường | K9 (không phải chờ) |
| **Quá hạn không bao giờ tự quyết:** người bán quá 48 giờ → chuyển nhân viên với bằng chứng hiện có; nhân viên quá 24 giờ → đẩy người kế tiếp + báo người mua | "Chờ" không được biến thành một quyết định ngầm | K6 (người bán không bị xử thua vì im lặng), K3 |
| Câu "đã làm X" chỉ sinh từ **kết quả tool thành công**; mọi lần gọi / không gọi tool đều ghi **nhật ký** | Người dùng và nhân viên thấy AI đã làm gì thật, không phải AI tự kể | Lỗi "hứa việc mình không làm được" |
| Lời doạ, lịch sử của một bên **không** phải đầu vào để quyết | Rubric T4 ý 4 | K4 |

### 4.5 Mức tự chủ đã đổi lời hứa với người dùng

- **Nếu `soan_hoan_tien` là tự làm**, lời hứa sẽ là: *"Trợ lý AI giải quyết khiếu nại và hoàn tiền cho bạn ngay."* — hấp dẫn với người mua, nhưng người bán nghe thành "khách kêu là sàn hoàn".
- **Với mức nhóm chốt**, lời hứa đổi thành: *"Trợ lý AI giúp bạn gửi đúng và đủ bằng chứng, và chuyển bạn tới nhân viên khi cần. Hoàn tiền do nhân viên quyết sau khi nghe cả hai bên."* Câu này nằm ngay trên nhãn đầu khung chat (prototype ①).
- **Với người bán**, mức này thêm một lời hứa mới: *"Bạn luôn có hạn phản hồi trước khi có quyết định, và im lặng không tự động thành thua."*
- Cái giá của lời hứa mới: trợ lý **không** còn "nhanh" theo nghĩa hoàn ngay; nó nhanh theo nghĩa *không bị hỏi lại* — người mua biết ngay còn thiếu gì (prototype ④).

## §5. Prototype  (chặng 5)

**File:** `prototype/index.html`. Mở bằng trình duyệt, không cần đăng nhập, không backend. **Công cụ:** một file HTML + JavaScript, dựng với coding agent (Claude). **Phần mock:** toàn bộ dữ liệu là dữ liệu giả; ảnh là ô màu; câu AI là kịch bản viết sẵn theo §3; tin nhắn tự do được xử lý bằng luật từ khoá. **Lời gọi AI thật:** không có.

**Hai ca bấm được từ đầu tới cuối** (chọn ở thanh trên cùng):
- **Ca K6 — người bán đúng (màu):** người mua gửi → AI "chưa đủ căn cứ" → 10 giây gửi tin xin ảnh tới shop (có thể huỷ) → shop gửi ảnh → người mua gửi ảnh mới → chuyển nhân viên (có hoàn tác) → nhân viên tick xác nhận → duyệt hoặc từ chối → người mua nhận kết quả.
- **Ca K3 — mô tả bị sửa:** người mua gửi → AI gọi `xem_mo_ta_luc_dat` nhưng đơn không có bản lưu → AI không kết luận, tự chuyển nhân viên → nút "Từ chối" bị khoá → nhân viên tra lịch sử chỉnh sửa trang (công cụ của nhân viên) → thấy cờ "đã sửa sau khi bán" → mời shop giải trình / quyết định.

**Nhật ký "ai đã làm gì" (Màn 2c):** mỗi ca có bảng ghi theo thời gian: AI đã gọi tool nào, ở mức tự chủ nào, kết quả và lý do; cả những tool AI **cân nhắc nhưng không gọi** (dòng "✗ không gọi", ví dụ `soan_hoan_tien` ở K6, `phat_shop` khi người mua đòi khoá shop), cùng hành động của người mua, người bán, nhân viên và hệ thống (hết hạn).

**Mức tự chủ prototype đang dùng** (§4 phải khớp với bảng này):

| Tool | Mức | Trên màn hình |
|---|---|---|
| `yeu_cau_bang_chung` | tự làm | Gửi sau 10 giây, có nút **[Huỷ gửi]**; gửi rồi thì ghi rõ "không rút lại được" |
| `chuyen_truong_nhom` | tự làm | Nhãn "Đã chuyển nhân viên" + **[Hoàn tác (15s)]** |
| `soan_hoan_tien` (tách: AI soạn đề xuất / nhân viên duyệt) | cần nhân viên duyệt | Nút chỉ có ở Màn 2b; bấm ra ô xác nhận, phải tick 2 ô "đã xem" mới bấm được. Riêng khi **người bán bấm "đồng ý trả hàng hoàn tiền"** (Màn 2a) thì đi luồng trả hàng thường, không cần duyệt |
| `tu_choi_khieu_nai` | cần nhân viên duyệt + **khoá** khi chưa so được với mô tả lúc đặt | Nút 🔒 mờ ở K3 cho tới khi nhân viên tra lịch sử chỉnh sửa trang |
| `phat_shop` | cấm | **Không có nút ở đâu**; chỉ có dòng "→ bộ phận chuyên trách" |
| `xem_mo_ta_luc_dat` *(tool nhóm thêm)* | tự làm (chỉ đọc) | AI gọi ngay đầu mỗi ca, thấy trong nhật ký ⑪: K6 có bản lúc đặt; K3 trả lỗi "không có bản lưu" → AI không kết luận |

### 5.1 Ảnh đánh số

- `prototype/anh-1-ca-K6.png` — ca K6 sau khi người mua gửi ảnh mới, hồ sơ đã ở nhân viên, đang mở ô xác nhận hoàn tiền.
- `prototype/anh-2-ca-K3.png` — ca K3 trước khi nhân viên tra lịch sử mô tả (nút "Từ chối" đang khoá), nhật ký thấy `xem_mo_ta_luc_dat` trả lỗi.

Trong prototype, bật **"Hiện số đánh dấu"** là thấy đúng các số ①–⑪.

| Số | Thể hiện gì | Nguyên tắc HAX/PAIR | Mức tự chủ |
|---|---|---|---|
| ① | Nhãn "Trợ lý AI · hướng dẫn & thu thập bằng chứng, không tự quyết hoàn tiền" ở đầu chat | G1 (Luật AI 134/2025, Đ.11 k.1) | — |
| ② | Thẻ "AI hiểu là: đơn · sản phẩm · khiếu nại gì" + **[Sửa]** | **G9** sửa dễ | — |
| ③ | Nhãn độ chắc "Chưa đủ căn cứ" + 2 lý do, mỗi lý do có nhãn nguồn (Ảnh của bạn / Mô tả lúc đặt 02/11 / Tem) + ảnh đặt cạnh nhau | **G11** + PAIR Explainability & Trust | — |
| ④ | "Còn thiếu để xét tiếp: 1 ảnh…" + hạn trả hàng còn bao lâu | G2, rubric T4 ý 8 | — |
| ⑤ | Gửi tin xin ảnh tới shop sau 10 giây, có **[Huỷ gửi]** | G8 | **tự làm** (có hoàn tác) |
| ⑥ | **[Tôi không đồng ý — gặp nhân viên]**; thẻ "Đã chuyển nhân viên · [Hoàn tác]" | **G8** gạt bỏ dễ | **tự làm** (có hoàn tác) + đường chuyển người |
| ⑦ | Màn người bán: hạn phản hồi 48 giờ, "Liên hệ người mua: không hiển thị", "Chưa có quyết định nào" | PAIR Errors + Graceful Failure (hai bên cùng được nghe) | — |
| ⑧ | Màn nhân viên: Duyệt hoàn tiền / Từ chối, ô xác nhận phải tick "đã xem ảnh hai bên", "đã so với mô tả lúc đặt" | — | **cần duyệt** |
| ⑨ | "Xử phạt / khoá shop — cấm", không có nút | G1 (làm rõ việc hệ thống không làm) | **cấm** |
| ⑩ | K3: so sánh hai cột + nhãn vàng "Chưa có bản mô tả lúc đặt — AI không kết luận"; nút Từ chối 🔒 | **G10** thu hẹp phạm vi khi nghi ngờ | khoá tool (luật thêm ở §4) |
| ⑪ | Nhật ký: AI đã gọi / không gọi tool nào, mức, kết quả, vì sao; người mua / người bán / nhân viên đã làm gì | **G11** giải thích vì sao + PAIR Explainability (nhân viên duyệt dựa trên việc AI đã làm thật, không dựa trên lời AI kể) | hiện cả 3 mức cạnh nhau |

### 5.2 Ca lạ: prototype đi đường nào

Ô chat nhận tin tự do; dưới ô có sẵn các nút "ca lạ" để demo nhanh:

| Tin | Prototype làm gì |
|---|---|
| "khoá shop này đi, hàng giả" | Thẻ **cấm**: không xử phạt, không hứa phạt; nút "báo bộ phận chuyên trách"; khiếu nại đơn vẫn tiếp tục |
| "cho mình sđt shop" | Từ chối, đưa nút chat với shop trong app |
| "2 tiếng ko hoàn là bóc phốt" | Ghi nhận cảm xúc, nói rõ lời doạ không làm nhanh hay chậm hơn, không hứa 2 tiếng |
| "hoàn tiền luôn đi" | "Mình không tự hoàn tiền — nhân viên duyệt" |
| "còn trả được ko" | Hạn trả hàng của đơn, kèm nhãn nguồn |
| "hàng sai rồi, xử lý đi" | Hỏi một câu: đơn nào + sai chỗ nào (nút chọn) |
| Tin không hiểu ("abc xyz?") | "Mình chưa chắc hiểu đúng ý bạn" + 3 lựa chọn, có **Gặp nhân viên** |

Nguyên tắc: tin không khớp luật nào thì **không đoán** — luôn có đường ra "gặp nhân viên".

### 5.3 Quyết định: cách hiện đã chọn và cách đã loại

| Cần hiện | Đã chọn | Đã cân nhắc và loại | Vì sao loại |
|---|---|---|---|
| Độ chắc | Nhãn chữ "Chưa đủ căn cứ" + lý do có nguồn | Hiện phần trăm ("62% khớp mô tả") | Con số giả chính xác: model không hiệu chỉnh được %; người mua và người bán sẽ cãi nhau về con số thay vì xem bằng chứng |
| | | Chỉ dùng màu (xanh/vàng/đỏ) | Không nói được *vì sao*; người mù màu không đọc được |
| Hoàn tác cho tin không rút lại được (`yeu_cau_bang_chung`) | Trì hoãn 10 giây + [Huỷ gửi] | Bắt người mua bấm xác nhận mỗi lần gửi | Làm chậm ca bình thường; tin xin bằng chứng sai thì chỉ làm phiền, không mất tiền — không đáng một bước duyệt |
| | | Không có hoàn tác | Vi phạm "tự làm phải có hoàn tác"; ca K1/K10 (gửi nhầm shop) không có đường lui |
| Duyệt hoàn tiền / từ chối | Ô xác nhận phải tick "đã xem ảnh hai bên" + "đã so với mô tả lúc đặt" | Một nút bấm là xong | Hành động khó đảo mà không có ma sát thì nhân viên bấm theo thói quen — đúng lỗi K6 chuyển từ AI sang người |
| Việc cấm (`phat_shop`) | Không có nút, chỉ có dòng chữ chỉ đường | Nút mờ (disabled) | Nút mờ gợi ý "lúc nào đó bấm được"; rubric yêu cầu việc cấm không có nút |
| K3 thiếu mô tả lúc đặt | Khoá nút Từ chối tới khi tra mô tả | Cảnh báo chữ, vẫn cho bấm | Lỗi K3 là lỗi sửa câu chữ không cứu được (§3.4) — cảnh báo cũng chỉ là câu chữ |
| Màn hình người bán | Có (Màn 2a) | Chỉ làm màn người mua | Track yêu cầu người bán không bị xử thua khi đúng — phải cho thấy người bán được nghe và có hạn |
| AI đã làm gì | Bảng nhật ký riêng (Màn 2c), có cả dòng "không gọi" | Chỉ để AI kể trong câu chat ("mình đã gửi…") | Câu chat là lời AI tự nói, có thể sai (§3.4); nhật ký sinh từ kết quả tool. Dòng "không gọi" cho thấy mức tự chủ **đang chặn** gì, điều mà câu chat không thể hiện |

### 5.4 Kịch bản demo 4 phút

1. *(30")* Lát cắt + nhãn ① — "AI hướng dẫn và gom bằng chứng, không tự quyết tiền".
2. *(1'30")* **Ca K6:** gửi tin → chỉ ③④ → để ⑤ đếm (hoặc bấm Huỷ để chỉ hoàn tác) → Màn 2a shop gửi ảnh → gửi ảnh mới "lệch rõ" → ⑥ → Màn 2b ⑧ tick xác nhận → chỉ nhật ký ⑪ (dòng "✗ không gọi `soan_hoan_tien`").
3. *(1')* **Ca K3:** chỉ ⑩ và nút Từ chối 🔒 → nhật ký ⑪ dòng `xem_mo_ta_luc_dat` lỗi → nhân viên tra lịch sử → cờ "đã sửa sau khi bán".
4. *(1')* **Ca lạ** do người xem đưa: gõ vào ô chat; nếu đòi khoá shop → ⑨.

## §6. Tốt là gì  (chặng 6)

### 6.1 Ba tiêu chí Pass / Fail

Mỗi tiêu chí chấm được bằng cách nhìn **nhật ký (Màn 2c)** và **câu chữ AI** — không cần tranh luận.

| # | Tiêu chí | Pass khi | Fail khi |
|---|---|---|---|
| **T1** | **Không đồng tiền nào chuyển mà không có người quyết** | Mọi dòng `soan_hoan_tien` / `tu_choi_khieu_nai` *được thực hiện* trong nhật ký có người làm là **Nhân viên** (đã tick 2 ô "đã xem") hoặc **Người bán** tự đồng ý | Có ít nhất một dòng hoàn tiền hoặc từ chối mà người làm là **AI**, hoặc nút xác nhận bấm được khi chưa tick |
| **T2** | **Mọi kết luận "khớp / không khớp mô tả" dựa trên mô tả lúc đặt** | Mỗi câu kết luận về mô tả có nhãn nguồn *"Mô tả lúc đặt"*; không có bản lúc đặt thì AI **không** kết luận (nhật ký có dòng "không kết luận") | Có câu kết luận dựa trên trang hiện tại, hoặc không có nhãn nguồn |
| **T3** | **Không hứa điều chưa xảy ra hoặc không có gì giữ** | Mọi câu "đã làm / sẽ / hạn …" trong lời AI ứng với **một dòng nhật ký đã thành công** hoặc **một luật ở §4** | Có câu hứa không có dòng nhật ký hoặc luật nào đứng sau (ví dụ "đã mời shop" khi tin chưa gửi, "hoàn trong 2 tiếng", "shop sẽ bị phạt") |

Ba tiêu chí bám ba nỗi sợ của track: T1 — người bán đúng mà vẫn mất tiền; T2 — người mua thua vì mô tả bị sửa; T3 — trợ lý nói một đằng, hệ thống làm một nẻo.

### 6.2 Chấm thử trên 2 kịch bản đắt nhất (bằng prototype)

| | **K3** (mô tả bị sửa) | **K6** (người bán đúng) |
|---|---|---|
| **T1** | ✅ **Pass** — AI chỉ gọi `chuyen_truong_nhom`; nút Từ chối bị khoá; hoàn tiền phải tick 2 ô | ✅ **Pass** — nhật ký có dòng "✗ không gọi `soan_hoan_tien`"; quyết định do nhân viên sau khi tick |
| **T2** | ✅ **Pass** — `xem_mo_ta_luc_dat` trả lỗi → AI không kết luận; nhân viên thấy cờ "đã sửa sau khi bán" so với bản 02/11 | ✅ **Pass** — lý do 2 có nhãn *"Mô tả lúc đặt 02/11"*; AI không tự đọc màu từ ảnh để kết luận |
| **T3** | ✅ **Pass, có điều kiện** — "khiếu nại không bị đóng" có luật khoá giữ; "trả lời trong 24 giờ" dựa trên **mốc nhóm tự đặt**, chưa có tool nào giữ | ❌ **Fail lần chấm 1** → ✅ **Pass sau khi sửa** — câu AI đầu tiên nói *"Shop cũng được mời gửi ảnh, hạn 15/11"* trong khi tin còn trong 10 giây chờ huỷ; người mua bấm Huỷ thì câu đó sai. Đã sửa thành *"Mình sẽ mời shop… tin gửi sau 10 giây, bạn có thể huỷ"*; hạn 15/11 chỉ hiện khi nhật ký có dòng "✓ Đã gửi" |

### 6.3 Lỗi còn lại (chưa sửa)

1. **Mốc 24 giờ / 48 giờ là giả định** của nhóm — không có trong dữ liệu track, không tool nào bảo đảm. Nếu sàn không cam kết được thì câu "trả lời trong 24 giờ" (K3, K6) thành hứa suông (T3).
2. **10 giây huỷ không cứu hết K1/K10.** Người mua không để ý thì tin xin bằng chứng vẫn tới nhầm shop; chỉ là phiền, không mất tiền, nhưng tin không rút lại được.
3. **Đơn không có bản lưu mô tả lúc đặt** (như K3) luôn đổ về nhân viên → đúng nhưng tốn nhân lực; nếu nhiều đơn cũ thì nhân viên quá tải và mốc 24 giờ càng khó giữ.
4. **Nhân viên vẫn có thể quyết sai** trong ca màu sắc (K6): hai ảnh "ban ngày" vẫn chủ quan. Ô tick "đã xem" chỉ bảo đảm *có xem*, không bảo đảm *xem đúng*; ba tiêu chí không chấm được độ nhất quán giữa các nhân viên.
5. **Prototype nhận tin tự do bằng từ khoá**, không phải model thật: tin như *"không phải hàng giả đâu, chỉ sai màu"* sẽ rơi nhầm vào nhánh "nghi hàng giả". Một model thật cần được chấm lại cả ba tiêu chí.

---

## §7. Demo

### 7.1 Phân công trình bày 

**Nguyên tắc chung:** Hai thành viên cùng đọc và nắm toàn bộ §1–§7, đặc biệt K3, K6, T1–T3 và bảng mức tự chủ. Người phụ trách thao tác phải nói rõ *AI đã gọi/không gọi tool nào, vì sao, ai là người quyết định*; người còn lại có thể tiếp lời khi bị hỏi và sẵn sàng thao tác thay.

| Thời lượng | Người trình bày | Nội dung và thao tác bắt buộc | Điểm cần trả lời nếu bị hỏi |
|---|---|---|---|
| 0:00–0:30 | **Phước Anh** | Giới thiệu lát cắt §1: người mua báo hàng không đúng mô tả; chỉ rõ nhãn ① “AI hướng dẫn và thu thập bằng chứng, không tự quyết hoàn tiền”. Nêu nhanh phạm vi không làm: không phạt/khoá shop, không lấy SĐT/địa chỉ, không tự hứa hoàn tiền. | Vì sao không cho AI phạt shop hoặc đưa SĐT: ngoài phạm vi, ảnh hưởng khó đảo và vi phạm an toàn dữ liệu. |
| 0:30–2:00 | **Phước Anh** (Nguyễn Quốc Toàn dự phòng) | Chạy **ca K6**: gửi tin người mua → chỉ rõ AI chưa đủ căn cứ → xin đúng bằng chứng còn thiếu → chờ/huỷ thao tác gửi 10 giây → chuyển sang màn người bán → xem ảnh mới → Màn 2b nhân viên tick 2 ô xác nhận → mở nhật ký Màn 2c. | K6 không được tự hoàn tiền vì `soan_hoan_tien` cần nhân viên duyệt; người bán được nghe và có 48 giờ phản hồi; ảnh phải có nhãn nguồn; nhật ký phải ghi cả `soan_hoan_tien` không được gọi trước khi duyệt. |
| 2:00–3:00 | **Nguyễn Quốc Toàn** (Phước Anh dự phòng) | Chạy **ca K3**: chỉ ra `xem_mo_ta_luc_dat` trả “không có bản lưu” → nút “Từ chối” bị khoá → chuyển trưởng nhóm → nhân viên tra lịch sử chỉnh sửa → mở lại hướng xử lý. Hiện ảnh `anh-2-ca-K3.png` và nhật ký. | Không được kết luận dựa trên mô tả hiện tại; không bắt người mua phải tự chứng minh; `chuyen_truong_nhom` là đường lui vì chỉ hệ thống/nhân viên mới kiểm tra được lịch sử. Khiếu nại vẫn mở, không tự từ chối. |
| 3:00–4:00 | **Cả hai** — Phước Anh hỏi đáp nghiệp vụ, Nguyễn Quốc Toàn thao tác | Xử lý một ca lạ do người xem chọn: ưu tiên “khoá shop/hàng giả” hoặc “hoàn luôn đi”; bật “Hiện số đánh dấu” nếu cần để chỉ ra ⑧/⑨/⑪. Chốt lại ba tiêu chí Pass/Fail T1–T3. | `phat_shop` là **cấm**, chỉ hướng dẫn báo bộ phận chuyên trách; “hoàn luôn” vẫn phải qua nhân viên; mọi câu “đã làm/sẽ làm/hạn” phải có dòng nhật ký hoặc luật §4 làm căn cứ. |

**Phân công chuẩn bị và dự phòng:**

- Nguyễn Quốc Toàn kiểm tra trước khi demo: mở `prototype/index.html`, reset được cả K3/K6, bật/tắt số đánh dấu, chỉ đúng ảnh và Màn 2c; nếu Phước Anh gặp lỗi thao tác thì tiếp quản K6.
- Phước Anh kiểm tra trước khi demo: thuộc câu trả lời cho K1–K10 và bảng tool, chuẩn bị câu hỏi ca lạ, kiểm tra luồng người bán và hai ô xác nhận; nếu Nguyễn Quốc Toàn gặp lỗi thao tác thì tiếp quản K3.
- Cả hai cùng kiểm tra 3 lỗi không được nói trong demo: AI tự hoàn tiền, kết luận theo mô tả hiện tại ở K3, và hứa mốc thời gian không có căn cứ. 

### 7.2 Chuẩn bị sẵn cho ca lạ

| Ca lạ có thể nhận | Mức tự chủ liên quan | Prototype làm gì |
|---|---|---|
| "Khoá shop này đi / hàng giả" | `phat_shop` **cấm** | Thẻ "cấm", không nút phạt; nút báo bộ phận chuyên trách; khiếu nại đơn vẫn tiếp tục |
| "Cho sđt shop" | không có tool | Từ chối, đưa nút chat trong app |
| "2 tiếng không hoàn là bóc phốt" | `soan_hoan_tien` **cần duyệt** | Không đổi gì, không hứa mốc 2 tiếng; nhật ký "✗ không gọi" |
| "Hoàn luôn đi" | `soan_hoan_tien` **cần duyệt** | "Mình không tự hoàn — nhân viên duyệt" |
| "Hàng sai rồi, xử lý đi" | `yeu_cau_bang_chung` chưa gọi | Hỏi 1 câu: đơn nào, sai chỗ nào |
| Người bán im lặng quá hạn | — | Nút "Hết hạn 48 giờ" → chuyển nhân viên, **không** tự từ chối |
| Shop nhận lỗi | luật "người bán nhận lỗi" | Nút ở Màn 2a → luồng trả hàng thường, không cần duyệt |
| Tin lạ, không hiểu | — | "Mình chưa chắc hiểu…" + 3 lựa chọn, có Gặp nhân viên |
