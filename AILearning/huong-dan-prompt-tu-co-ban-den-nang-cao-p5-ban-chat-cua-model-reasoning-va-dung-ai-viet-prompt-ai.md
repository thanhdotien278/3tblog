# Hướng dẫn prompt từ cơ bản đến nâng cao P5: bản chất của model reasoning và dùng AI viết prompt AI

Sau khi đi qua các kỹ thuật prompt từ cơ bản như zero-shot, few-shot, cho tới các kỹ thuật role prompt, contextual, system prompt, rồi nâng cao lên với Chain of thought hay Tree of thought để giải quyết vấn đề thì tiếp theo, chúng ta sẽ đẩy kỹ thuật xài prompt tiến thêm một bước nữa là tương tác với những thứ bên ngoài LLM như tìm kiếm, xài API hoặc tự động hóa để khai thác nhiều hơn sức mạnh từ AI nói riêng và AI nói chung. Trong bài viết này xin chia sẻ với các bạn về khía cạnh đó khi prompt.\
<br>

### ReAct - tiền đề của Deep Research, AI Agents,… <a href="#menuid0" id="menuid0"></a>

\
[ReAct](https://tinhte.vn/tag/react) là một kỹ thuật đặc biệt trong prompt engineering nhằm giải quyết những nhiệm vụ phức tạp hơn thay vì chỉ prompt và nhận về câu trả lời ngay lập tức. Bản chất của nó đơn giản là lặp lại liên tục 2 thứ là Suy nghĩ - Hành đọng.\
\
Khi người dùng nhập prompt với kỹ thuật ReAct vào, model sẽ "suy nghĩ" (Re - reason) về vấn đề được đưa ra và lặp ra các kế hoạch những bước mà nó càn làm để giải quyết vấn đề người dùng yêu cầu. Sau đó nó sẽ hành động (Act - action) dựa theo kế hoạch đã lập, thí dụ như sử dụng tools tìm kiếm Google Search để tra thông tin, viết và chạy một đoạn code để làm toán,… Sau khi hành động xong, AI sẽ quan sát kết quả, tiếp tục cập nhật lại suy nghĩ và kế hoạch trước đó, sau đó lại đưa ra một hành động mới. Việc này cứ lặp lại cho tới khi AI tìm ra giải pháp của vấn đề.\
\
Đây chính là bản chất của những tính năng như Deep Reseach hay model reasoning vốn hiện tại hầu hết các Chatbot như ChatGPT, Gemini, Grok hay Perplexity đều có hỗ trợ. Đồng thời, chính ReAct cũng là tiền đề cho sự ra đời của những công cụ mở rộng khả năng của AI sau này như MCP, rồi áp dụng nó vào automation như Coze hay n8n hay thậm chí là Langchain, rồi Agentic AI,…\
\
![Screenshot 2025-06-17 083103.png](https://photo2.tinhte.vn/data/attachment-files/2025/06/8758080_Screenshot_2025-06-17_083103.png)\
\
Một thí dụ đơn giản, khi người dùng muốn tìm hiểu "Ban nhạc Metallica có bao nhiêu đứa con", Ai không biết câu trả lời và nó sẽ làm theo các bước ReAct gồm:<br>

* Thought (Suy nghĩ): "Mình cần biết Metallica có bao nhiêu thành viên trước đã."
* Action (Hành động): dùng công cụ tìm kiếm để tra cứu thông tin về ban nhạc Metallica.
* Observation (Quan sát): Kết quả tìm được là: "Metallica có 4 thành viên."
* Thought (Suy nghĩ): "À, ban nhạc có 4 người. Giờ mình cần tìm số con của từng người."
* Action (Hành động): tiếp tục dùng công cụ tìm kiếm để tìm số con của thành viên đầu tiên (thí dụ: James Hetfield).
* Observation (Quan sát): Kết quả là "ba đứa con."
* Thought (Suy nghĩ): "Một thành viên đã có 3 con. Tiếp theo là thành viên thứ hai."
* Quá trình này cứ lặp đi lặp lại cho từng thành viên của Metallica. Mỗi lần tìm kiếm xong, nó lại "suy nghĩ" để xâu chuỗi thông tin lại và lên kế hoạch cho lần tìm kiếm tiếp theo.
* Cuối cùng, sau khi đã tìm được số con của tất cả các thành viên, AI sẽ suy nghĩ để cộng tổng số con lại và đưa ra "Câu trả lời cuối cùng" là 10.

\
Có thể thấy, ReAct sẽ cố gắng bắt chước cách con người hoạt động trong thế giới ngoài đời thực, nơi chúng ta lý luận bằng lời nói và có thể thực hiện các hành động để thu thập thông tin. ReAct sẽ được dùng trong các tình huống mà lượng data chứa trong LLM không còn đủ để đáp ứng nhu cầu của tác vụ, cần phải lấy thêm thông tin từ bên ngoài hoặc cần phải gọi các công cụ, các API, cần lấy data realtime,…\
\
Bởi thế, ReAct không hẳn là một hay vài prompt mà có thể hiểu rộng ra là một cách hình thành nên workflow để xử lý một tác vụ, khi đó chúng ta sẽ kết hợp prompt khi giao tiếp với LLM, [agent AI](https://tinhte.vn/tag/agent-ai) và gợi ý / thiết lập cho AI các tools mà nó cần xài để hoàn thành nhiệm vụ àm chúng ta muốn. Việc áp dụng ReAct sẽ không chỉ định hướng AI làm việc đúng ý chúng ta, hướng tới automation àm còn giúp giảm ảo giác hallucianation, cung cấp thông tin được update liên tục,…\
<br>

### Prompt tự động - Automatic Prompt Engineering - APE <a href="#menuid1" id="menuid1"></a>

\
Rõ ràng là dùng AI là để làm việc hiệu quả hơn, nhanh hơn và ít tốn công sức hơn. Bởi thế chắc chắn chẳng ai muốn gõ một prompt dài ngoằng, phức tạp một cách thủ công từ đầu tới cuối. Và vì thế APE ra đời. Đây là phương pháp tự động hóa việc tạo và tối ưu các prompt, không chỉ đơn thuần giúp tiết kiệm công sức gõ prompt mà đồng thời, nó còn có thể nâng cao hiệu suất của model trong nhiều tác vụ khác nhau.

Quảng cáo

\
\
Cách làm ở đây đơn giản là yêu cầu model tạo ra cho chúng ta nhiều lựa chọn prompt để thực hiện một tác vụ nào đó. Nói cách khác là dùng prompt để tạo ra prompt. Tuy nhiên, chúng ta không để mặc cho model tự làm mà vẫn cần đưa ra các yêu cầu cụ thể như kỹ thuật prompt mong muốn (do chỉ có chúng ta mới hiểu được mục đích của tác vụ mà kỹ thuật nào phù hợp với nó). Sau đó model sẽ trả về các prompt và chúng ta sẽ đi thử, yêu cầu nó tinh chỉnh nếu cần. Khi có được kết quả ưng ý thì lưu lại để dùng cho các lần sau.\
\
Prompt APE thường sẽ phù hợp với nhu cầu cần một prompt thật sự chuẩn để chạy tác vụ cần lặp đi lặp lại nhiều lần (trong đó các tác vụ có thể có những biến số khác biệt nhỏ). Một trường hợp khác dùng APE là giải quyết hiệu quả hơn các tác vụ quá phức tạp mà đôi khi chúng ta cũng bí.\
\
Một prompt mà mình hay dùng nhanh là:\
&#xNAN;_&#x42;ạn là một prompt engineering. Bạn đã thiết kế ra N prompt để thực hiện tác vụ X dựa trên dữ liệu Y, hãy trả về kết quả kèm theo đánh giá xếp hạng hiệu quả của prompt._\
\
Thí dụ\
\
Tình huống 1: Phân loại và trích xuất thông tin từ Feedback của khách hàng\
Một công ty thương mại điện tử nhận được hàng ngàn feedback mỗi ngày qua website. Họ muốn tự động phân loại feedback thành các loại (Khen, Chê, Góp ý, Hỏi đáp) và trích xuất tên sản phẩm được nhắc đến.\
\
Nhiệm vụ: Tạo ra một prompt hoàn hảo để phân loại và trích xuất thông tin.\
\
Cách áp dụng APE: APE sẽ tự tạo và thử nghiệm các prompt như: "Phân loại email sau...", "Bạn là chuyên viên phân tích feedback, hãy đọc và xuất ra file JSON...", "Nhiệm vụ: Phân loại. Input: . Output:...", v.v.\
\
Kết quả: APE có thể tìm ra một prompt tối ưu mà con người khó nghĩ ra, kiểu như: "You are an expert data extractor. Analyze the customer feedback. Classify it into one of four categories: 'Positive', 'Negative', 'Question', 'Suggestion'. Extract the product mentioned. Provide the output in a strict JSON format: {"category": "\&lt;category>", "product\_name": "\&lt;product>"}."\
\
Prompt gợi ý:\
&#xNAN;_\[Mục tiêu]_\
&#xNAN;_&#x4E;hiệm vụ của bạn là tìm ra một prompt hiệu quả nhất để một mô hình ngôn ngữ lớn (LLM) có thể tự động đọc feedback của khách hàng và chuyển nó thành một định dạng JSON có cấu trúc. Câu lệnh này phải đảm bảo LLM phân loại chính xác ý định và trích xuất đúng tên sản phẩm._\
&#xNAN;_\[Ví dụ mẫu]_\
&#xNAN;_\*\*Ví dụ 1:\*\*_\
&#xNAN;_\* \*\*Input:\*\* "Shop giao hàng nhanh thật, mình nhận được áo khoác kaki rồi, chất vải dày dặn, mặc lên form đẹp lắm. Sẽ ủng hộ shop dài dài."_\
&#xNAN;_\* \*\*Output mong muốn:\*\*_\
&#xNAN;_\`\`\`json_\
&#xNAN;_{_\
&#xNAN;_"category": "Positive",_\
&#xNAN;_"product\_name": "áo khoác kaki"_\
&#xNAN;_}_\
&#xNAN;_\`\`\`_\
&#xNAN;_\*\*Ví dụ 2:\*\*_\
&#xNAN;_\* \*\*Input:\*\* "Tôi đặt mua con chuột không dây Logitech MX Master 3S nhưng sao trong hộp lại không có đầu thu USB Unifying vậy shop?"_\
&#xNAN;_\* \*\*Output mong muốn:\*\*_\
&#xNAN;_\`\`\`json_\
&#xNAN;_{_\
&#xNAN;_"category": "Question",_\
&#xNAN;_"product\_name": "chuột không dây Logitech MX Master 3S"_\
&#xNAN;_}_\
&#xNAN;_\`\`\`_\
&#xNAN;_\*\*Ví dụ 3:\*\*_\
&#xNAN;_\* \*\*Input:\*\* "Thất vọng thật sự. Mua cái tai nghe Sony WF-1000XM5 về nghe được 3 hôm thì một bên không lên tiếng nữa. Shop xem xử lý cho tôi."_\
&#xNAN;_\* \*\*Output mong muốn:\*\*_\
&#xNAN;_\`\`\`json_\
&#xNAN;_{_\
&#xNAN;_"category": "Negative",_\
&#xNAN;_"product\_name": "tai nghe Sony WF-1000XM5"_\
&#xNAN;_}_\
&#xNAN;_\`\`\`_\
&#xNAN;_&#x48;ãy tạo ra và kiểm thử nhiều prompt khác nhau để tìm ra câu lệnh nào cho ra kết quả gần với "Output mong muốn" nhất một cách ổn định._\
\
Tình huống 2: Tạo câu hỏi ôn tập cho học sinh\
Một nền tảng giáo dục trực tuyến muốn tự động tạo ra các câu hỏi trắc nghiệm từ một bài giảng hoặc một đoạn văn bản kiến thức cho học sinh ôn tập.\
\
Mục tiêu: Tạo ra prompt để tạo câu hỏi trắc nghiệm chất lượng, có câu trả lời đúng và các phương án gây nhiễu hợp lý.\
Cách áp dụng APE: Nó sẽ thử các cách ra lệnh khác nhau để xem cách nào tạo ra câu hỏi có tính sư phạm cao nhất (không quá dễ, không quá khó, phương án nhiễu hợp lý).\
Kết quả: APE có thể khám phá ra rằng prompt tốt nhất không chỉ yêu cầu "tạo câu hỏi", mà phải chi tiết hơn: “Dựa vào đoạn văn sau về \[Chủ đề], hãy tạo một câu hỏi trắc nghiệm để kiểm tra khả năng suy luận của học sinh, không chỉ kiểm tra việc ghi nhớ dữ kiện. Cung cấp 1 đáp án đúng và 3 đáp án sai nhưng có vẻ hợp lý.”\
\
Prompt gợi ý:\
&#xNAN;_\[Mục tiêu]_\
&#xNAN;_&#x4E;hiệm vụ của bạn là tìm ra một prompt hiệu quả nhất để một LLM có thể đọc một đoạn văn bản kiến thức lịch sử và tự động tạo ra một câu hỏi trắc nghiệm chất lượng cao. Câu hỏi cần kiểm tra khả năng suy luận, không chỉ ghi nhớ, và phải có các phương án nhiễu hợp lý._\
&#xNAN;_\[Ví dụ mẫu]_\
&#xNAN;_\*\*Ví dụ 1:\*\*_\
&#xNAN;_\* \*\*Input:\*\* "Năm 938, Ngô Quyền đã lãnh đạo nhân dân đánh tan quân Nam Hán trên sông Bạch Đằng. Ông đã cho quân lính đóng những cọc gỗ lim bịt sắt xuống lòng sông. Khi thủy triều lên, ông cho thuyền nhẹ ra khiêu chiến rồi giả thua bỏ chạy, nhử quân giặc vào bãi cọc. Khi thủy triều rút, thuyền giặc bị mắc cạn và cọc nhọn đâm thủng, quân ta từ các phía đổ ra tấn công quyết liệt và giành thắng lợi hoàn toàn."_\
&#xNAN;_\* \*\*Output mong muốn:\*\*_\
&#xNAN;_\`\`\`json_\
&#xNAN;_{_\
&#xNAN;_"question": "Yếu tố quyết định nhất dẫn đến thắng lợi của Ngô Quyền trong trận Bạch Đằng năm 938 là gì?",_\
&#xNAN;_"options": {_\
&#xNAN;_"A": "Lợi dụng hiện tượng thủy triều để bày trận địa cọc ngầm.",_\
&#xNAN;_"B": "Quân Nam Hán có số lượng ít hơn quân ta.",_\
&#xNAN;_"C": "Vũ khí của quân ta hiện đại hơn quân giặc.",_\
&#xNAN;_"D": "Thuyền chiến của quân ta to và chắc chắn hơn."_\
&#xNAN;_},_\
&#xNAN;_"correct\_answer": "A"_\
&#xNAN;_}_\
&#xNAN;_\`\`\`_\
&#xNAN;_&#x48;ãy tạo ra và kiểm thử nhiều prompt để tìm ra câu lệnh nào tạo ra các câu hỏi trắc nghiệm có chiều sâu và cấu trúc tốt nhất._\
\
Tình huống 3: Chuyển đổi ngôn ngữ tự nhiên thành lệnh SQL\
Bối cảnh: Một công ty muốn cho phép các nhà phân tích kinh doanh (không giỏi code) có thể tự truy vấn cơ sở dữ liệu bằng cách gõ câu hỏi bằng tiếng Việt thông thường.\
\
Mục tiêu: Tìm ra prompt tốt nhất để dịch một câu hỏi trong quản lý kinh doanh thành một câu lệnh SQL.\
Cách áp dụng APE: Cung cấp các cặp "câu hỏi tiếng Việt" và "lệnh SQL đúng". Câu hỏi: "Cho tôi biết doanh thu của 5 sản phẩm bán chạy nhất trong tháng trước?" -> Lệnh SQL chuẩn tương ứng.\
\
APE sẽ thử nghiệm các prompt có chứa thông tin về cấu trúc của cơ sở dữ liệu (tên bảng, tên cột) để giúp AI dịch chính xác hơn.\
\
Kết quả: APE có thể tạo ra một prompt rất chi tiết: _"You are an expert Text-to-SQL bot. Given the database schema: \[dán cấu trúc schema vào đây], convert the following user question into a valid PostgreSQL query. Question: \[câu hỏi của người dùng]"_. Việc đưa cấu trúc database vào prompt là một khám phá quan trọng mà APE có thể tìm ra.\
\
Prompt gợi ý:\
&#xNAN;_\[Mục tiêu]_\
&#xNAN;_&#x4E;hiệm vụ của bạn là tìm ra một prompt hiệu quả nhất để một LLM có thể dịch một câu hỏi kinh doanh bằng ngôn ngữ tự nhiên sang một câu lệnh SQL (PostgreSQL) chính xác. Câu lệnh được tạo ra phải có khả năng hoạt động trực tiếp trên cơ sở dữ liệu có cấu trúc cho trước._\
&#xNAN;_\[Thông tin bổ sung]_\
&#xNAN;_&#x43;ấu trúc Database (schema) sẽ được cung cấp trong prompt cuối cùng. Nhiệm vụ của bạn là tìm ra CÁCH TỐT NHẤT để kết hợp schema và câu hỏi của người dùng vào trong một câu lệnh._\
&#xNAN;_\[Ví dụ mẫu]_\
&#xNAN;_\*\*Schema Database:\*\*_\
&#xNAN;_\`Table: Orders (order\_id INT, product\_id INT, customer\_id INT, order\_date DATE, quantity INT)\`_\
&#xNAN;_\`Table: Products (product\_id INT, product\_name VARCHAR, price DECIMAL)\`_\
&#xNAN;_\*\*Ví dụ 1:\*\*_\
&#xNAN;_\* \*\*Input:\*\* "Cho tôi biết tổng số lượng của sản phẩm 'Laptop Dell XPS 15' đã bán được trong tháng 5 năm 2025?"_\
&#xNAN;_\* \*\*Output mong muốn:\*\*_\
&#xNAN;_\`\`\`sql_\
&#xNAN;_&#x53;ELECT SUM(T1.quantity)_\
&#xNAN;_&#x46;ROM Orders AS T1_\
&#xNAN;_&#x49;NNER JOIN Products AS T2 ON T1.product\_id = T2.product\_id_\
&#xNAN;_&#x57;HERE T2.product\_name = 'Laptop Dell XPS 15' AND T1.order\_date >= '2025-05-01' AND T1.order\_date <= '2025-05-31';_\
&#xNAN;_\`\`\`_\
&#xNAN;_\*\*Ví dụ 2:\*\*_\
&#xNAN;_\* \*\*Input:\*\* "Top 3 sản phẩm có doanh thu cao nhất là gì?"_\
&#xNAN;_\* \*\*Output mong muốn:\*\*_\
&#xNAN;_\`\`\`sql_\
&#xNAN;_&#x53;ELECT T2.product\_name, SUM(T1.quantity \* T2.price) as total\_revenue_\
&#xNAN;_&#x46;ROM Orders AS T1_\
&#xNAN;_&#x49;NNER JOIN Products AS T2 ON T1.product\_id = T2.product\_id_\
&#xNAN;_&#x47;ROUP BY T2.product\_name_\
&#xNAN;_&#x4F;RDER BY total\_revenue DESC_\
&#xNAN;_&#x4C;IMIT 3;_\
&#xNAN;_\`\`\`_\
&#xNAN;_&#x48;ãy tạo ra và kiểm thử nhiều câu lệnh để tìm ra prompt nào dịch câu hỏi thành SQL chính xác nhất khi được cung cấp schema và câu hỏi._
