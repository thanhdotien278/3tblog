# Hướng dẫn prompt từ cơ bản đến nâng cao P2: Role prompt, Contextual Prompt và System Prompt

### Role Prompt <a href="#menuid0" id="menuid0"></a>

\
Thực ra đây là một thành phần trong prompt nhiều hơn là một kiểu prompt. Bằng cách gán một nhân vật hoặc một danh tính cụ thể (thí dụ như bạn là một chuyên gia xxx nào đó, bạn là tên nhân vật nào đó,…), model sẽ tập trung kiến thức của nó vào chủ để mà người dùng truy vấn, từ đó tạo ra các phản hồi nhất quán và phù hợp với yêu cầu của người dùng.\
\
Một cách dễ hình dung, Role prompt sẽ xác định rõ một tập kiến thức con nằm trong bể kiến thức khổng lồ của model được dạy, từ đó phạm vi tìm kiểm sẽ có chiều sâu và phù hợp hơn. Bằng cách này, phong cách, giọng văn đầu ra và tính chuyên môn của nội dung mà người dùng kỳ vọng sẽ được cải thiện chất lượng hơn, mức độ liên quan và hiệu quả cũng được tăng lên.\
\
Việc áp dụng Role Prompt không chỉ gán cho model một vai trò / nghề nghiệp nào đó, thí dụ như giáo viên mẫu giáo, diễn giả truyền cảm hứng, biên tập viên, chuyên gia code,… mà chúng ta cũng có thể xác định góc độ vai trò cho nó, thí dụ nhưng yêu cầu nhân vật mà model nhập vai có chuyên môn cụ thể nào, thí dụ như thay vì chuyên gia code python chuyên gia code python cho các task có liên quan để cụ thể hơn nữa. Đồng thời, chúng ta cunxgc ó thể quy định các phong cách hay giọng điệu trả lời nào, thí dụ như thẳng thắng, chi tiết, Trang trọng, Hài hước, Ảnh hưởng, Không trang trọng, Truyền cảm hứng, Thuyết phục,… để tiếp tục tối ưu đầu ra phù hợp với tác vụ mà người dùng đang yêu cầu model thực hiện.\
\
Trên thực tế, cho tới hiện tại tất cả các [hướng dẫn prompt](https://tinhte.vn/tag/huong-dan-prompt) hiệu quả (kiểu công thức dùng được ở nhiều tình huống) từ các chuyên gia, tổ chức lớn lẫn công ty phát triển model đều khẳng định vai trò của role prompt trong việc trả về kết quả phù hợp. Mình thấy một số bạn thắc mắc rằng có cần xác định role không thì mình nghĩ rằng, câu hỏi nên là "tình huống nào thì cần dùng role". Nếu kiểu zero shot đã giải quyết được vấn đề của bạn thì tất nhiên không cần rồi.\
\
Role prompt sẽ phù hợp để:\


* Kiểm soát hàm lượng chuyên môn của phản hồi do model tạo ra.
* Xác định đặc tính kiến thức chuyên ngành của đầu ra
* Kiểm soát giọng điệu, phong cách, cá tính của đầu ra.
* Hỗ trợ định hình ngữ cảnh tương tác với model

\
Cấu trúc cơ bản của một Role Prompt:\
Bạn là XXX\
Hãy YYY\
\
Tình huống 1:\
Bạn là một hướng dẫn viên du lịch, hãy gợi ý các địa điểm

Quảng cáo

\
![Screenshot 2025-04-21 081852.png](https://photo2.tinhte.vn/data/attachment-files/2025/04/8711654_Screenshot_2025-04-21_081852.png)\
\
Tình huống 2:\
Bạn là một chuyên gia lập trình python, hãy giải thích đoạn code phức tạp sau.\
![Screenshot 2025-04-21 082043.png](https://photo2.tinhte.vn/data/attachment-files/2025/04/8711656_Screenshot_2025-04-21_082043.png)\
![Screenshot 2025-04-21 082056.png](https://photo2.tinhte.vn/data/attachment-files/2025/04/8711657_Screenshot_2025-04-21_082056.png)\
\
Đây là không dùng Role Prompt\
![Screenshot 2025-04-21 090335.png](https://photo2.tinhte.vn/data/attachment-files/2025/04/8711703_Screenshot_2025-04-21_090335.png)\
\
Tình huống 3:\
Bạn là một nhà sử học luôn hoài nghi mọi thứ, hãy phân tích tài liệu sau\
![Screenshot 2025-04-21 082733.png](https://photo2.tinhte.vn/data/attachment-files/2025/04/8711665_Screenshot_2025-04-21_082733.png)\
\
Đây là không dùng Role Prompt\
![Screenshot 2025-04-21 085840.png](https://photo2.tinhte.vn/data/attachment-files/2025/04/8711697_Screenshot_2025-04-21_085840.png)\
\
Tình huống 4:\
Bạn là một thợ rèn thời Trung cổ, hãy mô tả chi tiết cách rèn một thanh kiếm\
![Screenshot 2025-04-21 082436.png](https://photo2.tinhte.vn/data/attachment-files/2025/04/8711662_Screenshot_2025-04-21_082436.png)\
\
Không dùng Role Prompt\
![Screenshot 2025-04-21 090244.png](https://photo2.tinhte.vn/data/attachment-files/2025/04/8711704_Screenshot_2025-04-21_090244.png)\
\
Tình huống 5:\
Bạn là một một kiện tướng cờ vua. Hãy gợi ý cho tôi chi tiết 3 cách khai cuộc.\
![Screenshot 2025-04-21 082618.png](https://photo2.tinhte.vn/data/attachment-files/2025/04/8711664_Screenshot_2025-04-21_082618.png)\
\
Không dùng Role Prompt\
![Screenshot 2025-04-21 090124.png](https://photo2.tinhte.vn/data/attachment-files/2025/04/8711699_Screenshot_2025-04-21_090124.png)\
\


### Contextual Prompt <a href="#menuid1" id="menuid1"></a>

\
Prompt này sẽ cung cấp chi tiết hoặc những thông tin nền cụ thể có liên quan đến cuộc hội thoại hoặc nhiệm vụ hiện tại. Prompt này giúp model hiểu được rõ hơn các đặc tính của những gì người dùng truy vấn, từ đó điều chỉnh phản hồi phù hợp. Đặc điểm của\
\
Contextual Prompt cung cấp thông tin cụ thể theo từng tác vụ mà người dùng yêu cầu. Bởi thế, Contextual Prompt sẽ mang tính động, nghĩa là nó cần thay đổi theo từng tác vụ khác nhau để đảm bảo rằng model sẽ trả về các kết quả phù hợp với từng tác vụ. Thường Contextual chính là tình huống, bối cảnh mà người dùng đang thực hiện tác vụ\
\
Contextual Prompt sẽ phù hợp để:\


* Cung cấp bối cảnh cụ thể cho một nhiệm vụ.
* Điều chỉnh phản hồi cho phù hợp với tình huống/cuộc trò chuyện hiện tại.
* Làm rõ sắc thái dựa trên thông tin được cung cấp.

\
Cấu trúc cơ bản của một Contextual Prompt:\
Ngữ cảnh: Tôi đang làm / Bạn đang có,…\
Yêu cầu: Hãy XXX\
\
Tình huống 1:\
Bạn đang đánh giá một báo cáo tài chính của một công ty nộp cho ngân hàng để xin vay số tiền 5 tỷ đồng mở rộng kinh doanh.\
Hãy viết một đoạn đánh giá dài 300 từ, nêu ra những điểm mạnh và điểm yếu tài chính của công ty này.\
![Screenshot 2025-04-21 081221.png](https://photo2.tinhte.vn/data/attachment-files/2025/04/8711649_Screenshot_2025-04-21_081221.png)\
\
Tình huống 2:\
Bạn đang tư vấn thực đơn cho một người ăn chay, dị ứng với miến và dứa.\
Hãy đề xuất thực đơn món Việt Nam 3 ngày.\
\
![Screenshot 2025-04-21 081202.png](https://photo2.tinhte.vn/data/attachment-files/2025/04/8711648_Screenshot_2025-04-21_081202.png)\
\
Tình huống 3:\
Tạo một bài viết dựa vào phong cách cụ thể\
Hãy soạn một bài viết về văn hóa công ty theo cấu trúc và văn phong giống như file tôi tải lên.\
![Screenshot 2025-04-21 081352.png](https://photo2.tinhte.vn/data/attachment-files/2025/04/8711650_Screenshot_2025-04-21_081352.png)\
\
Tình huống 4:\
Trả lời câu hỏi dựa vào thông tin trong văn bản kèm trong prompt\
![Screenshot 2025-04-21 081452.png](https://photo2.tinhte.vn/data/attachment-files/2025/04/8711651_Screenshot_2025-04-21_081452.png)\
\
Tình huống 5:\
Tóm tắt toàn bộ nội dung cuộc họp dựa vào biên bản đầy đủ và các ghi chú của cuộc họp\
![Screenshot 2025-04-21 081636.png](https://photo2.tinhte.vn/data/attachment-files/2025/04/8711652_Screenshot_2025-04-21_081636.png)\
\


### System Prompting <a href="#menuid2" id="menuid2"></a>

\
System Prompt được dùng để thiết lập một bối cảnh và mục tiêu tổng thể cho model. Prompt này sẽ định nghĩa "một bức tranh tổng thể" về những gì mà model cần thực hiện, từ những task đơn giản như dịch ngôn ngữ, phân loai, đánh giá cho tới những nhiệm vụ phức tạp hơn.\
\
System prompt sẽ đóng vai trò như một bộ những quy tắc bổ sung mà model cần tuân thủ nhất quán trong mọi lần mà người dùng truy vấn nó, thí dụ như PHẢI trả về định dạng JSON, PHẢI cho đầu ra theo một định dạng mà người dùng muốn. Nếu muốn sử dụng output này của model để đưa vào một logic khác, hoặc nằm trong một hệ thống tự động, thì system prompt sẽ cực kỳ hữu ích.\
\
System Prompt sẽ có thể phù hợp để:\


* Ép model tạo đầu ra đáp ứng được những yêu cầu cụ thể. Thí dụ dùng nó để chỉ định cách model trả về kết quả phân loại phim, chỉ trả về tiêu đề phim viết hoa, kèm theo tên 3 diễn viên chính,… Điều này đảm bảo model sẽ tạo đầu ra nhất quán theo một định dạng nhất định thỏa ý đồ của người dùng.
* Trả về đầu ra theo một cấu trúc mà người dùng muốn, thường gặp nhất là trả về một JSON. Bằng cách sử dụng System Prompt, model sẽ có thể cho ra đầu ra đã được sắp xếp thứ tự, cú pháp,… theo chỉ định và hạn chế hallucianation. Khi muốn dùng model trong một hệ thống tự động hóa xử lý dữ liệu hoặc luồng, việc dùng System Prompt gần như là việc làm bắt buộc.
* Kiểm soát thông tin được model trả về luôn an toàn và không có từ ngữ / nội dung độc hại. Thí dụ như bạn đang dùng model để tạo ra những nội dung cho trẻ em, cho một nhóm đối tượng nhạy cảm,… có thể dùng System Prompt để quy định nguyên tắc cho model, PHẢI làm xxx, KHÔNG ĐƯỢC trả về abc,…

\
Cấu trúc cơ bản của System Prompt:\
System Instruction: Bạn PHẢI \[tuân theo quy tắc xxx]\
User prompt: YYY\
\
Tình huống 1\
Chúng ta dùng System Prompt để chỉ định rằng "Bạn luôn kiểm tra thông tin cẩn thận và luôn trích dẫn nguồn". Sau đó khi người dùng nhập prompt yêu cầu tác vụ cụ thể "Kể ra 3 sự thật đáng ngạc nhiên về loài mèo", model sẽ trả về kết quả như bên dưới.\
![\[​IMG\]](https://photo2.tinhte.vn/data/attachment-files/2025/04/8711634_Screenshot_2025-04-21_004055.png)\
\
Và đây là khi không có System Prompt, để model tự bơi.\
![Screenshot 2025-04-21 004028.png](https://photo2.tinhte.vn/data/attachment-files/2025/04/8711636_Screenshot_2025-04-21_004028.png)\
\
Tình huống 2:\
Dùng prompt System để chỉ định rằng "Hãy luôn trả lời bằng 2 câu trong Truyện Kiều của Nguyễn Du." Sau đó người dùng nhập prompt yêu cầu: "Giải thích về cây thân gỗ"\
![Screenshot 2025-04-21 003908.png](https://photo2.tinhte.vn/data/attachment-files/2025/04/8711637_Screenshot_2025-04-21_003908.png)\
\
Đây là khi không có System Prompt\
![Screenshot 2025-04-21 003947.png](https://photo2.tinhte.vn/data/attachment-files/2025/04/8711638_Screenshot_2025-04-21_003947.png)\
\
Tình huống 3:\
Yêu cầu đầu ra phải luôn được Viết Hoa\
![Screenshot 2025-04-21 080018.png](https://photo2.tinhte.vn/data/attachment-files/2025/04/8711639_Screenshot_2025-04-21_080018.png)\
\
Tình huống 4:\
Yêu cầu model phong cách xưng hô khi trả lời\
![Screenshot 2025-04-21 080305.png](https://photo2.tinhte.vn/data/attachment-files/2025/04/8711641_Screenshot_2025-04-21_080305.png)\
\
Tình huống 5:\
![Screenshot 2025-04-21 080428.png](https://photo2.tinhte.vn/data/attachment-files/2025/04/8711643_Screenshot_2025-04-21_080428.png)\
\


### Phân biệt 3 kiểu prompt nói trên <a href="#menuid3" id="menuid3"></a>

\
Đầu tiên, hãy thử tóm tắt ngắn gọn nhất có thể đặc tính của 3 kiểu prompt này:\


* System prompt giúp định hình khả năng cơ bản và mục đích bao trùm của model. Nó cho phép chúng ta "lập trình sẵn" hướng dẫn model thực hiện các loại nhiệm vụ cụ thể và tuân theo các định dạng đầu ra nhất định.
* Contextual prompt đảm bảo rằng phản hồi của model có liên quan trực tiếp đến tình huống hoặc nhiệm vụ hiện tại. Điều này rất quan trọng để có được các phản hồi chính xác và hữu ích, đặc biệt trong các cuộc hội thoại hoặc các nhiệm vụ phức tạp đòi hỏi sự hiểu biết về bối cảnh cụ thể.
* Role prompt cho phép kiểm soát chiều sâu kiến thức, giọng điệu và phong cách của đầu ra. Bằng cách gán một vai trò cụ thể, chúng ta có thể giúp model tạo ra nội dung phù hợp cả về độ sâu kiến thức lẫn hình thức với một đối tượng hoặc mục đích cụ thể.

\
Mặc dù thoạt nhìn có sự chồng chéo giữa các kiểu prompt nói trên, tuy nhiên trong thực tế, người ta thường sẽ phối hợp 2 hoặc thậm chí là cả 3 kiểu prompt này trong cùng một prompt lớn khi thiết kế để tối ưu hóa đầu ra mong muốn đối với từng tác vụ, đặc biệt là những nhu cầu phức tạp. Việc hiểu tác động của từng kiểu prompt sẽ giúp chúng ta dễ dàng phân tích, xác định được đầu ra kỳ vọng khi xây dựng prompt để giải quyết các vấn đề.
