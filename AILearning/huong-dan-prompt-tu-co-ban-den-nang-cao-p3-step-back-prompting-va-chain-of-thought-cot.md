# Hướng dẫn prompt từ cơ bản đến nâng cao P3: step-back prompting và Chain of Thought (CoT)

### Step-back Prompting <a href="#menuid0" id="menuid0"></a>

\
Step-back prompting là kỹ thuật để cải thiện hiệu suất của các model LLM bằng cách hướng dẫn mô hình trước tiên xem xét một câu hỏi chung liên quan đến nhiệm vụ cần thực hiện và sau đó mới đưa câu trả lời cho câu hỏi chung đó vào một prompt tiếp theo cho nhiệm vụ cụ thể.\
\
Nghe có vẻ khó hiểu ha, một cách nôm na thì thay vì prompt ra lệnh trực tiếp, đầu tiên sẽ đặt một câu hỏi mang tính phổ quát hoặc trừu tượng hơn để model trả lời, sau đó dùng chính câu trả lời đó của AI làm context để giải quyết vấn đề cụ thể mà người dùng muốn.\
\
Chính vì vậy nên tên gọi của kỹ thuật này chính là Step Back, nghĩa là lùi lại nhìn toàn cảnh trước rồi mới đi sâu vào chi tiết. Mục đích của bước lùi này chính là để LLM kích hoạt kiến thức nền tảng hoặc các quy trình suy luận có liên quan trước khi giải quyết vấn đề cụ thể. Bằng cách khảo sát các nguyên tắc rộng hơn và căn bản hơn trong chủ đề đó, LLM có thể tạo ra các phản hồi chính xác và sâu sắc hơn.\
\
Về mặt kỹ thuật, Step-back prompting sẽ khuyến khích model tư duy phản biện và áp dụng kiến thức của chúng theo những cách mới và sáng tạo hơn. Kỹ thuật này sẽ giúp giảm thiểu sự thiên vị trong kiến thức của LLM bằng cách "nén" nhiều kiến thức hơn từ các tham số của LLM so với khi prompt trực tiếp.\
\
Step-back prompting sẽ phù hợp để:\


* Cải thiện khả năng suy luận đối với các task phức tạp.
* Kích thích kiến thức rộng hơn
* Giảm bias thông qua việc tập trung vào nguyên tắc trước khi đi vào chi tiết.

\
Cấu trúc của kỹ thuật Step-back Prompting:\
Nhìn chung / một cách tổng quát, X là gì?\
Sử dụng X, giải quyết vấn đề Y.\
\
Tình huống 1:\
Nhìn chung, những yếu tố nào quyết định liệu một thành phố ven biển có bị ngập lụt trong cơn bão hay không?​![Screenshot 2025-04-23 at 5.29.58 PM.png](https://photo2.tinhte.vn/data/attachment-files/2025/04/8714312_Screenshot_2025-04-23_at_5.29.58PM.png)

\
\
Sử dụng các yếu tố đó, hãy đánh giá rủi ro lũ lụt cho Đà Nẵng, Hội An, khi có cơn bão cấp 3.​![Screenshot 2025-04-23 at 5.30.11 PM.png](https://photo2.tinhte.vn/data/attachment-files/2025/04/8714313_Screenshot_2025-04-23_at_5.30.11PM.png)\
\
Tình huống 2:\
Nhìn chung, điều gì giúp một tin tuyển dụng việc làm hấp dẫn đối với các kỹ sư phần mềm?​![Screenshot 2025-04-23 at 5.34.41 PM.png](https://photo2.tinhte.vn/data/attachment-files/2025/04/8714318_Screenshot_2025-04-23_at_5.34.41PM.png)\
\
Áp dụng các tiêu chí đó để đánh giá tin tuyển dụng này từ Ahababa Inc.​![Screenshot 2025-04-23 at 5.37.03 PM.png](https://photo2.tinhte.vn/data/attachment-files/2025/04/8714319_Screenshot_2025-04-23_at_5.37.03PM.png)\
\
Tình huống 3:\
Tạo cốt truyện cho trò chơi bằng cách đầu tiên yêu cầu các yếu tố chính của thể loại​![Screenshot 2025-04-23 at 5.39.56 PM.png](https://photo2.tinhte.vn/data/attachment-files/2025/04/8714324_Screenshot_2025-04-23_at_5.39.56PM.png)\
![Screenshot 2025-04-23 at 5.41.41 PM.png](https://photo2.tinhte.vn/data/attachment-files/2025/04/8714325_Screenshot_2025-04-23_at_5.41.41PM.png)\
![Screenshot 2025-04-23 at 5.41.55 PM.png](https://photo2.tinhte.vn/data/attachment-files/2025/04/8714326_Screenshot_2025-04-23_at_5.41.55PM.png)\
\
Tình huống 4:\
Giải quyết một bài toán vật lý bằng cách trước tiên tìm ra những nguyên lý cơ bản liên quan.​\
![Screenshot 2025-04-23 at 5.49.53 PM.png](https://photo2.tinhte.vn/data/attachment-files/2025/04/8714332_Screenshot_2025-04-23_at_5.49.53PM.png)\
![Screenshot 2025-04-23 at 5.51.44 PM.png](https://photo2.tinhte.vn/data/attachment-files/2025/04/8714333_Screenshot_2025-04-23_at_5.51.44PM.png)\
\
Tình huống 5:\
Đánh giá quyết định chính sách phức tạp bằng cách đầu tiên hỏi về các tiêu chí chung có liên quan.​\
![Screenshot 2025-04-23 at 5.56.49 PM.png](https://photo2.tinhte.vn/data/attachment-files/2025/04/8714337_Screenshot_2025-04-23_at_5.56.49PM.png)\
![Screenshot 2025-04-23 at 5.58.02 PM.png](https://photo2.tinhte.vn/data/attachment-files/2025/04/8714338_Screenshot_2025-04-23_at_5.58.02PM.png)\
\


### Prompt Chain of Thought (CoT) <a href="#menuid1" id="menuid1"></a>

\
Một cách ngắn gọn, cấu trúc của kỹ thuật này là trình bày vấn đề hoặc câu hỏi, sau đó thêm chi tiết rõ ràng để lý giải từng bước, hoặc cung cấp thí dụ dạng few-shot prompt để diễn giải đầy đủ quá trình suy luận.\
\
Hiện tại các chat bot như ChatGPT 4o, Claude Sonnet 3.7 hay Gemini 2.5 đều có thể tự tạo ra CoT ngay cả khi người dùng nhập prompt zero shot vào, tuy nhiên việc hiểu được bản chất hoạt động và tạo yêu cầu rõ ràng bằng kỹ thuật CoT vẫn mang lại lợi ích đáng kể trong các tình huống phức tạp, cần độ chính xác cao hoặc cần một lộ trình rõ ràng.\
\
Chain of Thought (CoT) prompting là một kỹ thuật để cải thiện khả năng suy luận của model LLM bằng cách hướng dẫn mô hình tạo ra các bước suy luận trung gian, giúp nó đưa ra các câu trả lời chính xác hơn. Chúng ta cần kết hợp CoT với few-shot prompting để có kết quả tốt hơn đối với các nhiệm vụ phức tạp hơn đòi hỏi suy luận trước khi trả lời. CoT hoạt động bằng cách yêu cầu LLM giải thích từng bước thay vì chỉ trả về câu trả lời. Thí dụ, khi giải một bài toán, trong prompt sẽ yêu cầu rằng "Hãy nghĩ từng bước một".\
\
Ưu điểm của CoT:\


* Dễ thực hiện nhưng rất hiệu quả.
* Hoạt động tốt với các LLM phổ quát (không cần tinh chỉnh).
* Có tính giải thích cao. Chúng ta có thể kiểm tra các phản hồi của LLM và xem các bước suy luận đã được thực hiện để phát hiện lỗi nếu có.
* Cải thiện tính hiệu quả khi chuyển đổi giữa cácmodel khác nhau. Hiệu suất prompt của bạn sẽ ít bị ảnh hưởng hơn khi dùng các LLM khác nhau so với khi prompt không sử dụng CoT.
* Có thể hữu ích cho nhiều trường hợp sử dụng, chẳng hạn như tạo code (chia yêu cầu thành các bước nhỏ và ánh xạ chúng tới các dòng code cụ thể) và tạo dữ liệu tổng hợp.
* Nói chung, bất kỳ nhiệm vụ nào có thể giải quyết bằng cách "nói rõ từng bước" đều là thích hợp sử dụng CoT. Nếu bạn có thể giải thích các bước để giải quyết một vấn đề nào đó, hãy thử CoT để chạy hàng loạt những tác vụ đó.

Nhược điểm của CoT:\


* Prompt thường dài và phức tạp do chứa nhiều thông tin, chỉ dẫn.
* Đầu ra thường dài, do đó cũng tiêu tốn nhiều token đầu ra, dẫn tới chi phí sử dụng cũng cao hơn, thời gian xử lý lâu hơn.

\
Một lưu ý chính là CoT sẽ rất mạnh nếu kết hợp nó với Few shot Prompting thay vì chỉ dừng lại ở câu Hãy suy nghĩ từng bước. Cách này đặc biệt có ích khi muốn tiết kiệm số token đầu ra, để output ngắn gọn hơn, giới hạn suy luận của model lại, không quá lan man. Đặc biệt phù hợp khi dùng CoT để code.\
\
Prompt CoT sẽ phù hợp với:\


* Tạo code: CoT cực kỳ thích hợp để cùng với model Code.
* Các tác vụ số học
* Các lý luận thông thường lẫn lý luận tượng trưng trong đó các bước trung gian đóng vai trò quan trọng đối với độ chính xác của đầu ra.
* Cải thiện khả năng diễn giải.

\
Cấu trúc cơ bản của CoT:\
Vấn đề X\
Hãy suy nghĩ từng bước một X1, X2, X3,… để đạt mục đích Y\
\
Tình huống 1:\
Hai thành phố cách nhau 208,5 km, một xe máy đi từ thành phố A đến thành phố B với vận tốc là 38,6 km/h. Một ô tô khởi hành cùng một lúc với xe máy đi từ thành phố B đến thành phố A với vận tốc 44,8 km/h.\
a) Hỏi xe máy và ô tô gặp nhau lúc mấy giờ biết hai xe khởi hành lúc 8 giờ 30 phút\
b) Chỗ gặp nhau cách thành phố A bao nhiêu km?\
Hãy suy nghĩ từng bước để tìm đáp án\
\
![Screenshot 2025-04-23 at 6.01.43 PM.png](https://photo2.tinhte.vn/data/attachment-files/2025/04/8714341_Screenshot_2025-04-23_at_6.01.43PM.png)\
![Screenshot 2025-04-23 at 6.01.58 PM.png](https://photo2.tinhte.vn/data/attachment-files/2025/04/8714342_Screenshot_2025-04-23_at_6.01.58PM.png)\
\
Tình huống 2:\
Chín chiếc túi lần lượt đựng 3, 5, 8, 15, 17, 18, 21, 27 và 36 quả trứng.\
Bất kỳ ai muốn mua trứng phải mua hết số trứng trong một túi. Bác Biên và bà Bích mỗi người mua bốn túi trứng. Biết số trứng của bà Bích gấp ba lần số trứng của bác Biên. Hỏi túi trứng còn lại có bao nhiêu quả?\
Hãy suy nghĩ từng bước để tìm đáp án\
![Screenshot 2025-04-23 at 6.03.27 PM.png](https://photo2.tinhte.vn/data/attachment-files/2025/04/8714344_Screenshot_2025-04-23_at_6.03.27PM.png)\
![Screenshot 2025-04-23 at 6.03.46 PM.png](https://photo2.tinhte.vn/data/attachment-files/2025/04/8714345_Screenshot_2025-04-23_at_6.03.46PM.png)\
\
Tình huống 3:\
Giải quyết một tính năng code đòi hỏi nhiều bước trung gian\
\
Đây là kết quả với prompt zero shot\
![Screenshot 2025-04-23 at 6.04.27 PM.png](https://photo2.tinhte.vn/data/attachment-files/2025/04/8714346_Screenshot_2025-04-23_at_6.04.27PM.png)\
![Screenshot 2025-04-23 at 6.05.06 PM.png](https://photo2.tinhte.vn/data/attachment-files/2025/04/8714348_Screenshot_2025-04-23_at_6.05.06PM.png)\
\
Và prompt CoT\
![Screenshot 2025-04-23 at 6.06.44 PM.png](https://photo2.tinhte.vn/data/attachment-files/2025/04/8714354_Screenshot_2025-04-23_at_6.06.44PM.png)\
![Screenshot 2025-04-23 at 6.06.54 PM.png](https://photo2.tinhte.vn/data/attachment-files/2025/04/8714352_Screenshot_2025-04-23_at_6.06.54PM.png)\
![Screenshot 2025-04-23 at 6.07.11 PM.png](https://photo2.tinhte.vn/data/attachment-files/2025/04/8714353_Screenshot_2025-04-23_at_6.07.11PM.png)\
\
Tình huống 4:\
Giải thích các bước hợp lý cần thiết để đi đến kết luận cụ thể.\
Prompt: _Hãy phân tích từng bước lý do tại sao mô hình kinh doanh Freemium có thể là một chiến lược hiệu quả cho một ứng dụng SaaS quản lý dự án mới ra mắt. Bắt đầu bằng việc xác định đặc điểm của thị trường SaaS quản lý dự án (thí dụ: cạnh tranh cao, người dùng cần trải nghiệm trước khi mua). Sau đó, giải thích cách mô hình Freemium giải quyết các thách thức này:_\
&#xNAN;_&#x31;. Phân tích thị trường: Mức độ cạnh tranh, hành vi người dùng mục tiêu (doanh nghiệp nhỏ, cá nhân, đội nhóm)._\
&#xNAN;_&#x32;. Xác định rào cản gia nhập: Làm sao để người dùng dùng thử sản phẩm mới khi đã quen với công cụ cũ?_\
&#xNAN;_&#x33;. Lợi ích của việc cho dùng miễn phí: Cách nó giúp thu hút người dùng ban đầu, giảm chi phí marketing, tạo hiệu ứng mạng lưới._\
&#xNAN;_&#x34;. Thiết kế gói Free: Cần giới hạn những gì (số dự án, số thành viên, tính năng nâng cao) để khuyến khích nâng cấp?_\
&#xNAN;_&#x35;. Con đường nâng cấp lên Premium: Lý do người dùng sẽ trả tiền (cần thêm tính năng X, Y, Z; cần hỗ trợ nhiều người dùng hơn)._\
&#xNAN;_&#x36;. Kết luận: Tổng hợp lại tại sao Freemium là lựa chọn hợp lý dựa trên các phân tích trên._\
\
Tình huống 5:\
Lên kế hoạch cho một chuỗi hành động để đạt được mục tiêu.\
Prompt: Hãy lập một kế hoạch học tập chi tiết từng bước trong 3 tháng để một người đã biết HTML, CSS và JavaScript cơ bản có thể thành thạo ReactJS. Chia kế hoạch thành các tuần hoặc giai đoạn (ví dụ: Tháng 1, Tháng 2, Tháng 3) và xác định các mục tiêu cụ thể cho mỗi giai đoạn. Hãy suy nghĩ về trình tự hợp lý:\
1\. Tháng 1: Nền tảng và khái niệm cốt lõi.\
2\. Tháng 2: Các khái niệm nâng cao và hệ sinh thái.\
3\. Tháng 3: Thực hành dự án và tối ưu hóa.\
Với mỗi giai đoạn, hãy đề xuất loại tài nguyên học tập phù hợp (thí dụ: đọc tài liệu chính thức, xem khóa học video, làm bài tập, xây dựng dự án).\
\
![Screenshot 2025-04-23 at 6.12.32 PM.png](https://photo2.tinhte.vn/data/attachment-files/2025/04/8714380_Screenshot_2025-04-23_at_6.12.32PM.png)

