# Hướng dẫn prompt từ cơ bản đến nâng cao P1: zero-shot và few-shot prompting

Google gần đây đã phát hành tài liệu dài 68 trang, trong đó cập nhật mới nhất về các kỹ thuật [prompt](https://tinhte.vn/tag/prompt) engineering từ cơ bản để nâng cao, có thể áp dụng trong rất nhiều các tác vụ khác nhau, từ việc đơn giản như truy vấn thông tin, suy luận, giải thích tới code hay thậm chí là các system prompt để làm việc với agent, tạo bot tự động, xây ứng dụng [AI](https://tinhte.vn/tag/ai),…\
\
Mình đã đọc hết tài liệu này và trong bài viết này, mình sẽ tổng hợp lại một cách dễ tiếp cận hơn các kỹ thuật prompt mà nghiên cứu của Google hướng dẫn. Trong đó, mình sẽ mô tả ngắn gọn kỹ thuật, cấu trúc prompt, sau đó đưa ra các thí dụ cụ thể và gợi ý kỹ thuật prompt đó sẽ phù hợp với kiểu nhu cầu nào để các bạn tiện theo dõi.\
\
&#xNAN;_&#x58;em thêm:_\


* [Hướng dẫn prompt từ cơ bản đến nâng cao P1: zero-shot và few-shot prompting](https://tinhte.vn/thread/huong-dan-prompt-tu-co-ban-den-nang-cao-p1-zero-shot-va-few-shot-prompting.4011566/)
* [Hướng dẫn prompt từ cơ bản đến nâng cao P2: Role prompt, Contextual Prompt và System Prompt](https://tinhte.vn/thread/huong-dan-prompt-tu-co-ban-den-nang-cao-p2-role-prompt-contextual-prompt-va-system-prompt.4011916/)
* [Hướng dẫn prompt từ cơ bản đến nâng cao P3: step-back prompting và Chain of Thought (CoT)](https://tinhte.vn/thread/huong-dan-prompt-tu-co-ban-den-nang-cao-p3-step-back-prompting-va-chain-of-thought-cot.4012561/)
* [Hướng dẫn prompt từ cơ bản đến nâng cao P4: Self-consistency và CoT để giải quyết các task phức tạp](https://tinhte.vn/thread/huong-dan-prompt-tu-co-ban-den-nang-cao-p4-self-consistency-va-cot-de-giai-quyet-cac-task-phuc-tap.4017511/)
* Link ebook [Google Prompt Engineering 2025](https://www.kaggle.com/whitepaper-prompt-engineering) cho bạn nào quan tâm

\


### Prompt Chung / Zero-shot prompt <a href="#menuid0" id="menuid0"></a>

\
Đây là loại prompt đơn giản nhất, chỉ cung cấp cho model mô tả về một nhiệm vụ (có thể kèm theo một số text, văn bản đầu vào để model tham khảo) và bắt đầu nhấn nút để truy vấn. Với dạng này, prompt có thể là một câu hỏi, một câu mở đầu của một câu chuyện hoặc một hướng dẫn,… Thí dụ như "hãy làm xxx cho tôi", "làm thế nào để xxx", "abc có nghĩa là gì?",… Tên gọi Zero ở đây là để chỉ trong prompt này sẽ không cung cấp bất cứ một thí dụ nào cho LLM.\
\
Vai trò của zero-shot prompt: một điểm khởi đầu đơn giản nhất để làm việc với LLM, cho phép người dùng kiểm tra cơ bản tối thiểu khả năng của một LLM trong việc hiểu và thực hiện nhiệm vụ dựa trên yêu cầu của người dùng. Khi zero-shot không mang lại kết quả mong muốn, người dùng cần chuyển sang những kỹ thuật khác như one-shot hoặc few-shot prompt bằng cách cung cấp thêm thí dụ, logic,…\
\
Prompt zero-shot sẽ phù hợp với: các tác vụ dơn giản, các câu hỏi trực tiếp, cần hướng dẫn cụ thể ở những vấn đề phổ thông mà model đã được huấn luyện kỹ những thông tin đó. Thí dụ: Hãy tóm tắt bài viết sau trong 3 câu; Dịch đoạn sau sang tiếng Việt: \[đoạn văn cần dịch]\
\
Tình huống sử dụng 1:\
Hãy phân loại các phản hồi khách hàng sau:\
![Screenshot 2025-04-19 at 5.39.25 PM.png](https://photo2.tinhte.vn/data/attachment-files/2025/04/8711044_Screenshot_2025-04-19_at_5.39.25PM.png)\
![Screenshot 2025-04-19 at 5.39.31 PM.png](https://photo2.tinhte.vn/data/attachment-files/2025/04/8711045_Screenshot_2025-04-19_at_5.39.31PM.png)\
\
Tình huống sử dụng 2:\
Thủ đô của Thái Lan là gì?\
![Screenshot 2025-04-19 at 5.40.51 PM.png](https://photo2.tinhte.vn/data/attachment-files/2025/04/8711043_Screenshot_2025-04-19_at_5.40.51PM.png)\
\
Tình huống sử dụng 3:\
Tóm tắt các nội dung chính của bài viết sau\
![Screenshot 2025-04-19 at 5.43.38 PM.png](https://photo2.tinhte.vn/data/attachment-files/2025/04/8711042_Screenshot_2025-04-19_at_5.43.38PM.png)

### Few-shot prompt <a href="#menuid1" id="menuid1"></a>

\
Nếu như zero-shot prompt không trả về kết quả mong muốn, thì one-shot hay few-shot prompt sẽ là lựa chọn tiếp theo để giải quyết vấn đề. Với one-shot prompt, người dùng sẽ cung cấp một thí dụ duy nhất trong prompt để model hiểu rõ hơn về nhiệm vụ và cách tạo ra đầu ra mong muốn. Mục đích ở đây là để cho model thấy một hình mẫu cụ thể để nó bắt chước làm theo.\
\
Với các nhiệm vụ phức tạp hơn, few-shot prompt sẽ cung cấp nhiều thí dụ hơn (thường là từ 3 - 5 thí dụ). Các thí dụ này sẽ cung cấp các hình mẫu đa dạng hơn để model thấy và nó sẽ tuân theo. Điều này sẽ tăng khả năng của model thực hiện đúng định dạng / phong cách của đầu ra mà nó trả về cho người dùng.\
\
Vai trò của few-shot prompt: khắc phục hạn chế của zero-shot một khi model không thể hiểu hoặc thực hiện nhiệm vụ một cách chính xác với những thông tin mà người dùng mô tả; cũng có những tình huống mà một prompt zero-shot rất dài nhưng kém hiệu quả hơn so với một prompt few-shot đơn giản hơn nhưng có thí dụ cụ thể. Prompt few-shot cũng sẽ có chức năng chỉ dẫn cấu trúc và format của đầu ra mà người dùng mong muốn. Một tác dụng khác nữa chính là thông qua thí dụ, người dùng đã ngầm cung cấp một ngữ cảnh / quy tắc cho model, đặc biệt là những quy tắc đó sẽ khó diễn đạt được bằng ngôn ngữ bình thường.\
\
Các nguyên tắc và lưu ý khi dùng few-shot prmpt:\


* Các thí dụ phải phù hợp với nhiệm vụ bạn muốn thực hiện.
* Đảm bảo chất lượng ví dụ: Các ví dụ nên đa dạng, chất lượng cao và được viết tốt. Một lỗi nhỏ trong ví dụ có thể gây nhầm lẫn cho mô hình.
* Bổ sung các trường hợp đặc biệt (nếu cần): Nếu bạn muốn mô hình xử lý tốt nhiều loại đầu vào, hãy thêm các trường hợp đặc biệt trong thí dụ của bạn.
* Đối với nhiệm vụ phân loại, trộn lẫn các class: Khi sử dụng few-shot cho các tác vụ phân loại, hãy trộn lẫn các class phản hồi có thể có trong các ví dụ để tránh mô hình học thuộc thứ tự của thí dụ thay vì các đặc điểm của từng class.
* Cung cấp một số lượng thí dụ hợp lý: Một quy tắc chung là bắt đầu với khoảng 6 thí dụ cho few-shot và sau đó kiểm tra độ chính xác.
* Thí dụ là công cụ dạy model mạnh mẽ: Việc cung cấp thí dụ là một trong những mẹo tốt và quan trọng nhất trong prompting vì nó giúp mô hình hiểu rõ hơn về những gì người dùng mong đợi.

\
Cấu trúc của một prompt few-shot:\
NHIỆM VỤ X\
\
THÍ DỤ:\
A1 > B1\
\
THÍ DỤ:\
A2 > B2\
…\
\
Y > ?\
\
Prompt few-shot sẽ phù hợp với: các tác vụ đòi hỏi đầu ra phải tuân thủ một định dạng / cấu trúc nhất quán (thí dụ như cần model trả về một JSON với cấu trúc mong muốn), những nhiệm vụ khó diễn tả chính xác bằng lời, những nhiệm vụ mà prompt zero-shot đã không thể thực hiện thỏa mãn nhu cầu người dùng.\
\
Tình huống 1:\
Chuyển thông tin đơn hàng thành định dạng JSON\
![Screenshot 2025-04-19 at 5.46.44 PM.png](https://photo2.tinhte.vn/data/attachment-files/2025/04/8711040_Screenshot_2025-04-19_at_5.46.44PM.png)\
![Screenshot 2025-04-19 at 5.46.35 PM.png](https://photo2.tinhte.vn/data/attachment-files/2025/04/8711041_Screenshot_2025-04-19_at_5.46.35PM.png)\
\
Tình huống 2:\
Dịch các văn bản sau bằng cách diễn đạt và các thuật ngữ chuyên ngành ít gặp\
![Screenshot 2025-04-19 at 5.49.36 PM.png](https://photo2.tinhte.vn/data/attachment-files/2025/04/8711039_Screenshot_2025-04-19_at_5.49.36PM.png)\
\
Tình huống 3:\
Phân loại chủ đề và sửa lỗi\
![Screenshot 2025-04-19 at 5.53.58 PM.png](https://photo2.tinhte.vn/data/attachment-files/2025/04/8711038_Screenshot_2025-04-19_at_5.53.58PM.png)\
\
Tiếp theo sẽ là System Prompt, Role Prompting và nhiều kỹ thuật khác nữa, mình làm gọn lại, mỗi bài một ít và nhiều thí dụ hơn cho các bạn nào cần cũng sẽ dễ hình dung hơn. Hẹn các bạn ở bài tiếp theo ha.
