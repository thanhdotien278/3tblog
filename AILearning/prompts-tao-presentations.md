# Prompts tạo presentations

### Prompt tạo Outline

#### Prompt Tiếng Việt

Dựa trên tệp đính kèm, hãy xây dựng dàn ý chi tiết cho một bài thuyết trình, dùng để nhập vào các công cụ “AI presentation maker”.\
Dàn ý phải nêu rõ:

* Nội dung từng slide (với số liệu liên quan, nếu có)
* Mô tả biểu đồ (nếu có)
* Một slide có thể chứa nhiều biểu đồ khi cần thiết
* Tổng số slide không vượt quá 30

Toàn bộ nội dung phải liền mạch và định hướng rõ ràng, nhằm cung cấp cho lãnh đạo cái nhìn chiến lược về cơ hội thị trường, bối cảnh cạnh tranh, hành vi người tiêu dùng, và môi trường pháp lý của ngành sản phẩm chăm sóc tóc tự nhiên và hữu cơ tại các thị trường trọng điểm ở châu Á.\
Mục tiêu là hỗ trợ quyết định về phát triển sản phẩm, chiến lược thâm nhập thị trường, hợp tác, và marketing.

#### English Promtpt

Based on the attached file, create a detailed presentation outline suitable for input into “AI presentation maker” tools.\
The outline must clearly specify:

* Content for each slide (with related data, if any)
* Chart descriptions, if applicable
* Slides may include multiple charts when necessary
* Total slide count must not exceed 30

The presentation should form a cohesive narrative that delivers strategic insights for executives on the market opportunities, competitive landscape, consumer behavior, and regulatory environment of natural and organic hair care products across key Asian markets.\
The goal is to inform decisions on product development, market entry, partnerships, and marketing strategies.

<br>

### Prompt tạo Slide (Gemini)

Video: [Cách tạo slide thuyết trình chuyên nghiệp với Gemini (so sánh NotebookLM)](https://youtu.be/JmuyobEqZds)

#### English Prompt

Create a Vietnamese-language presentation slide following the exact content structure provided in the attached outline. Keep every slide clear, consistent, and easy to read. All slides must strictly follow the design requirements below:

1\. Color Theme

* Use the theme colors provided.
* The slide background color is #FEFAE0
* You may combine colors to maximize readability and maintain overall visual harmony.

2\. Typography\
– Slide titles: Playfair Display, size 36\
– Section headers: Playfair Display, size 26\
– Body text and bullet points: Nunito, size 16 to 18

3\. General Requirements\
– Maintain a clean, consistent layout across all slides.\
– Use only the content from the outline; do not add extra information.

#### Prompt tiếng Việt

Tạo bộ slide trình bày bằng tiếng Việt, dựa đúng theo nội dung tóm tắt trong file outline đính kèm. Bảo đảm bố cục nhất quán và dễ đọc. Toàn bộ slide phải tuân thủ đầy đủ các yêu cầu thiết kế sau:

1\. Màu sắc\
– Sử dụng bộ màu trong file đính kèm.\
– Bạn hãy phối màu để tăng tính dễ đọc và đảm bảo thẩm mỹ chung.

2\. Kiểu chữ và kích thước\
– Tiêu đề slide: Playfair Display, cỡ 36\
– Tiêu đề mục/section: Playfair Display, cỡ 26\
– Nội dung và bullet: Nunito, cỡ 16 đến 18

3\. Yêu cầu chung\
– Giữ bố cục rõ ràng, thống nhất trên toàn bộ slide.\
– Diễn đạt nội dung đúng theo outline, không thêm ý khác.

### Prompt tạo Slide (NotebookLM)

Video: [Cách làm slide NotebookLM chuẩn font Việt với ảnh tự tải, chỉnh PDF dễ dàng](https://youtu.be/JjP3bzdC3vA)

#### Prompt tạo slide thông thường, đúng font chữ tiếng Việt

Tạo bộ slide trình bày bằng tiếng Việt, dựa đúng theo nội dung file nguồn. Bảo đảm bố cục nhất quán và dễ đọc. Toàn bộ slide phải tuân thủ đầy đủ các yêu cầu thiết kế sau:

1\. Màu sắc

* Chọn bộ màu hiện đại, phù hợp với bài thuyết trình trong doanh nghiệp
* Phối màu để tăng tính dễ đọc và đảm bảo thẩm mỹ chung.

2\. Kiểu chữ và kích thước

* Tiêu đề slide: Google Sans&#x20;
* Tiêu đề mục/section: Google Sans&#x20;
* Nội dung và bullet: Nunito

3\. Yêu cầu chung

* Giữ bố cục rõ ràng, thống nhất trên toàn bộ slide.
* Diễn đạt nội dung đúng theo file nguồn, không thêm ý khác.

#### Tạo slide dùng ảnh tự tải

Thêm vào prompt trên:&#x20;

* Sử dụng X hình ảnh \[tên file ảnh đã tải lên NotebookLM] ở slide có nội dung phù hợp. Sử dụng CHÍNH XÁC hình ảnh được cung cấp, TUYỆT ĐỐI không chỉnh sửa hình ảnh và không dùng ảnh làm background.

#### Tạo slide để trống chỗ tự thêm ảnh

Dùng trong trường hợp NotebookLM từ chối tải ảnh của bạn.

Thêm vào prompt trên:&#x20;

* Để khoảng trống phù hợp trong slide để tôi tự chèn ảnh. Slide deck này cần X vị trí để chèn hình ảnh. Ở những vị trí đó, ghi chú là PLACEHOLDER + mô tả ngắn gọn nội dung ảnh cần chèn vào placeholder đó.

Thử nghiệm: Bạn có thể thử nghiệm mô tả tóm tắt những hình ảnh của bạn và yêu cầu NotebookLM đặt placeholder cho những ảnh đó ở slide phù hợp.

<br>
