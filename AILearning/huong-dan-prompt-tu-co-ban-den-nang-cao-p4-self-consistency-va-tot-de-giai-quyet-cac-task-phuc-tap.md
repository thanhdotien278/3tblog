# Hướng dẫn prompt từ cơ bản đến nâng cao P4: Self-consistency và ToT để giải quyết các task phức tạp

Trong bài viết này, mình sẽ tiếp tục chia sẻ về 2 kỹ thuật là Self Consistent và Tree of Though - một trong những kỹ thuật tiên tiến trong prompt engineering nhằm giải quyết những vấn đề phức tạp, đòi hỏi suy luận chồng chéo và nhiều bước như phân tích tài chính, đánh giá thẩm định, cho vay,….\
\


### Self-consistency <a href="#menuid0" id="menuid0"></a>

\
Self-consistency (tạm dịch: tự thống nhất) là một kỹ thuật nhằm cải thiện khả năng suy luận của model LLM. Đây có thể xem như một cách để tiếp tục nâng cấp khả năng của kỹ thuật [Chain of Thought](https://tinhte.vn/tag/chain-of-thought) lên một tầm mới. Bản chất của kỹ thuật này là kết hợp việc lấy mẫu (sampling) và bỏ phiếu đa số (major voting) để tạo ra một "đường dẫn" suy luận nhằm chọn ra câu trả lời nhất quán nhất, từ đó cải thiện độ chính xác và tính mạch lạc của phản hồi do LLM trả về.\
\
Nghe có vẻ lý thuyết phức tạp ha. Một cách dễ hiểu thì các bạn cứ hình dung là mỗi lần bạn hỏi chatbot AI một câu hỏi phức tạp, mặc dù nó rất "thông minh" nhưng đôi khi sẽ trả về những câu trả lời ngẫu hứng (do bản chất hoạt động dựa trên dự đoán từ của nó). Do đó, nếu bạn chỉ hỏi một lần, bạn có thể nhận được câu trả lời tốt, nhưng chưa chắc là tốt nhất hoặc cũng có thể là hơi lệch một chút.\
\
Kỹ thuật self-consistency giống như việc bạn hỏi chatbot cùng một câu hỏi nhiều lần. Mỗi lần, do tính "ngẫu hứng" (trong LLM gọi là temperature cao, cái này mình sẽ nói chi tiết hơn ở một bài khác ha), LLM có thể diễn giải hoặc suy nghĩ theo một hướng hơi khác nhau để đi đến câu trả lời. Sau đó, bạn xem xét tất cả các câu trả lời đó và chọn ra câu trả lời hoặc ý tưởng xuất hiện thường xuyên nhất, hoặc lấy giá trị trung bình/trung vị nếu đó là con số. Câu trả lời cuối cùng này thường sẽ đáng tin cậy và chính xác hơn nhiều so với chỉ một lần hỏi.\
\
Cách áp dụng self-consistency: Hãy soạn một prompt phù hợp với mục đích của bạn (one shot cũng được luôn). Sau đó thử chạy prompt này x lần và đọc kết quả của từng lần. Bạn có thể sẽ nhận thấy được những thông tin sẽ được lặp lại trong các kết quả, đó sẽ là thông tin "cốt lõi" và cần thiết nhất, những cái "khác" có thể là không quan trọng, hoặc có thể dùng như cung cấp thêm ý tưởng cho bạn.\
\
Self-consistency có thể được dùng để cải thiện độ chính xác và chất lượng của những thông tin mà LLM trả về. Quan trọng hơn, nó có thể được dùng để cải thiện chất lượng khi áp dụng kỹ thuật CoT.\
\
Bên dưới đây là một số tình huống để các bạn thử test cái này. Các bạn nên vào Google AI Studio hoặc một playground AI nào đó test cho tiện hơn ha.\
\


#### Tình huống 1: Đề xuất sản phẩm/dịch vụ phù hợp cho khách hàng dựa trên hồ sơ <a href="#menuid1" id="menuid1"></a>

\
Prompt:\
&#xNAN;_&#x4D;ột khách hàng 32 tuổi, là trưởng phòng marketing với thu nhập hàng tháng 45 triệu VNĐ, đã có gia đình (vợ và 1 con nhỏ). Khách hàng này hiện có một khoản tiết kiệm khoảng 500 triệu VNĐ, đang quan tâm đến các giải pháp đầu tư an toàn, sinh lời ổn định và các sản phẩm bảo hiểm cho gia đình. Hãy đề xuất 3-4 sản phẩm/dịch vụ ngân hàng phù hợp nhất._

_Quảng cáo_

\
&#xNAN;_&#x47;iải thích rõ lý do tại sao mỗi sản phẩm/dịch vụ lại phù hợp với hồ sơ và nhu cầu của khách hàng này_.\
\
Chạy prompt này 7 lần với temperature = 0.9. Ghi lại tất cả các bộ sản phẩm/dịch vụ được đề xuất và lý do đi kèm. Sau đó, xác định những sản phẩm/dịch vụ nào thường xuyên xuất hiện nhất trong các đề xuất. Đồng thời, tổng hợp các lý do thuyết phục nhất cho từng sản phẩm đó để xây dựng một gói tư vấn toàn diện và đáng tin cậy.\
\
![\[​IMG\]](https://photo2.tinhte.vn/data/attachment-files/2025/05/8728532_Screenshot_2025-05-10_153752.png)\
\


#### Tình huống 2: Xây dựng ý tưởng cho các hoạt động học tập sáng tạo trong một chủ đề cụ thể <a href="#menuid2" id="menuid2"></a>

\
Prompt:\
&#xNAN;_&#x54;ôi là giáo viên dạy môn Khoa học tự nhiên lớp 6. Sắp tới tôi sẽ dạy bài 'Sự đa dạng của thực vật'. Hãy gợi ý 5 hoạt động học tập sáng tạo, tương tác cao và phù hợp với lứa tuổi học sinh lớp 6 để giúp các em khám phá và hiểu rõ hơn về sự đa dạng của các loài thực vật xung quanh._\
&#xNAN;_&#x4D;ô tả ngắn gọn cách thức tổ chức mỗi hoạt động._\
\
Chạy prompt này 6 lần với temperature = 1.0. Mỗi lần chạy có thể đưa ra những ý tưởng hoạt động khác nhau, từ thực hành ngoài trời, trò chơi tương tác, đến dự án nhỏ. Thu thập tất cả các ý tưởng. Những hoạt động nào được gợi ý lặp lại nhiều lần hoặc có những biến thể thú vị sẽ được ưu tiên lựa chọn. Điều này giúp giáo viên có một danh sách phong phú các hoạt động tiềm năng, đảm bảo tính sáng tạo và phù hợp.\
\
![Screenshot 2025-05-10 153956.png](https://photo2.tinhte.vn/data/attachment-files/2025/05/8728533_Screenshot_2025-05-10_153956.png)\
\


#### Tình huống 3: Kiểm tra và hoàn thiện ý tưởng cho một kế hoạch đơn giản <a href="#menuid3" id="menuid3"></a>

\
Prompt:\
&#xNAN;_&#x54;ôi đang lên kế hoạch tổ chức một buổi cắm trại cuối tuần cho nhóm bạn khoảng 10 người. Các hạng mục chính cần chuẩn bị là gì để buổi cắm trại diễn ra suôn sẻ và vui vẻ?_\
&#xNAN;_&#x4C;iệt kê các hạng mục và giải thích ngắn gọn tầm quan trọng của từng hạng mục._\
\
Chạy prompt này 6 lần với temperature = 1.4. Mỗi lần chạy có thể sẽ đưa ra một số hạng mục hơi khác nhau hoặc nhấn mạnh các khía cạnh khác nhau. Tổng hợp tất cả các hạng mục được đề xuất. Những hạng mục nào xuất hiện lặp đi lặp lại nhiều lần nhất thì đó là những hạng mục cốt lõi và quan trọng nhất. Bạn cũng có thể phát hiện ra những ý tưởng hay mà có thể bạn đã bỏ sót nếu chỉ chạy prompt một lần.\
\
![Screenshot 2025-05-10 154309.png](https://photo2.tinhte.vn/data/attachment-files/2025/05/8728540_Screenshot_2025-05-10_154309.png)\
\


#### Tình huống 4: Tăng cường độ tin cậy khi tóm tắt một văn bản phức tạp và tìm ý chính <a href="#menuid4" id="menuid4"></a>

\
Prompt:\
&#xNAN;_&#x54;óm tắt bài viết sau đây \[paste một đoạn văn bản về một chủ đề mà bạn muốn] thành 3 gạch đầu dòng chính, nêu bật những luận điểm quan trọng nhất._\
&#xNAN;_&#x47;iải thích tại sao bạn chọn những luận điểm đó là quan trọng nh&#x1EA5;_&#x74;.\
\
Chạy prompt này 5 lần với temperature = 0.7. So sánh các bản tóm tắt và các luận điểm được chọn. Tìm ra những luận điểm nào được mô hình liên tục xác định là quan trọng nhất qua các lần chạy. Điều này giúp đảm bảo bạn không bỏ lỡ ý chính và bản tóm tắt cuối cùng thực sự phản ánh được cốt lõi của văn bản gốc.\
\
![Screenshot 2025-05-10 154641.png](https://photo2.tinhte.vn/data/attachment-files/2025/05/8728536_Screenshot_2025-05-10_154641.png)\
![Screenshot 2025-05-10 154701.png](https://photo2.tinhte.vn/data/attachment-files/2025/05/8728537_Screenshot_2025-05-10_154701.png)\
\


#### Tình huống 5: Đưa ra các giải pháp sáng tạo cho một vấn đề thường gặp <a href="#menuid5" id="menuid5"></a>

\
Prompt:\
&#xNAN;_&#x110;ề xuất 5 giải pháp sáng tạo và thực tế để giúp một người làm việc văn phòng giảm căng thẳng và cải thiện sự tập trung ngay tại bàn làm việc._\
&#xNAN;_&#x4D;ô tả ngắn gọn cách thực hiện mỗi giải pháp._\
\
Chạy prompt này 8 lần với temperature = 1.0. Bạn sẽ nhận được một loạt các giải pháp khác nhau. Hãy nhóm các giải pháp tương tự lại và xem những ý tưởng nào được lặp lại nhiều nhất hoặc có những biến thể nào đáng chú ý hay không. Bằng cách này, bạn có thể thu thập được một danh sách đa dạng và phong phú hơn các giải pháp, đồng thời xác định được những giải pháp nào có tính khả thi và được mô hình tin tưởng nhất qua nhiều lần tạo sinh.\
\
![Screenshot 2025-05-10 154939.png](https://photo2.tinhte.vn/data/attachment-files/2025/05/8728542_Screenshot_2025-05-10_154939.png)\
![Screenshot 2025-05-10 154957.png](https://photo2.tinhte.vn/data/attachment-files/2025/05/8728538_Screenshot_2025-05-10_154957.png)\
\
Tới đây có thể các bạn sẽ nghĩ sao phải cần phiền vậy đúng không? Các thí dụ trên đây về self-consistency sẽ giúp chúng ta hiểu được rõ hơn bản chất hoạt động, giới hạn kỹ thuật của LLM và tầm quan trọng của việc tận dụng nhiều hướng "suy nghĩ" khác nhau của LLM để chắt lọc ra kết quả tốt nhất cho chúng ta.\
\
Đây sẽ là nền tảng, kết hợp với kỹ thuật mà chúng ta đã biết trước đó là Chain of Thought để tiếp cái bên dưới là [Tree of Thought](https://tinhte.vn/tag/tree-of-thought) (ToT), kỹ thuật prompt tổng quát hóa để giải quyết những task phức tạp đòi hỏi khám phá, suy luận liên tục trong quá trình làm việc với LLM.\
\


### Tree of thought <a href="#menuid6" id="menuid6"></a>

\
Thay vì chỉ đi theo một chuỗi suy luận tuyến tính duy nhất như CoT, ToT cho phép các LLM khám phá đồng thời nhiều hướng suy luận khác nhau. Kỹ thuật này hoạt động bằng cách tạo ra một cây suy nghĩ (tree of thoughts), trong đó mỗi suy nghĩ đại diện cho một chuỗi suy luận ngôn ngữ rõ ràng, đóng vai trò là bước trung gian để giải quyết vấn đề. Mô hình sau đó có thể khám phá các đường dẫn suy luận khác nhau bằng cách phân nhánh từ các nút (node) khác nhau trong cây. Các bạn có thể coi hình minh họa bên dưới để dễ hình dung CoT và ToT khác nhau ra sao.\
\
![Screenshot 2025-05-10 165745.png](https://photo2.tinhte.vn/data/attachment-files/2025/05/8728548_Screenshot_2025-05-10_165745.png)\
\
Cho dễ hình dung, thí dụ như bạn đang ở một chỗ lạ và cần tìm đường đi tới một địa điểm nào đó. Lúc này, bạn có thể hỏi đường một người và đi theo chỉ dẫn đó (một luồng suy nghĩ duy nhất). Đây chính là prompt Chain of Thought. Nếu chọn cách làm này, nếu đi tới giữa đường và thấy có gì đó sai sai, bạn có thể sẽ bối rối không biết làm sao.\
\
Trong khi đó đối với Tree of Thought thì bạn hoạch định sẵn "à, mình có thể đi đường A, đường B và đường C". Sau đó nhận xét Đường A có vẻ ngắn nhưng nghe đồn hay kẹt xe, đường B xa hơn nhưng được cái rộng và dễ đi, Đường C có đi qua vài quán ăn ngon có thể ghé vào,… Tiếp theo bạn sẽ cân nhắc ưu nhược điểm, loại bỏ bớt những đường không khả thi. Thậm chí nếu ngồi tính đi một con đường nhưng sau đó không hài lòng, chúng ta có thể quay lại lần tẻ nhánh trước đó để phân tích theo hướng khác. Cuối cùng chúng ta sẽ có con đường tốt nhất để đi.\
\
ToT sẽ phù hợp để giải quyết các vấn đề phức tạp, đòi hỏi phải khảo sát nhiều hướng tiếp cận, so sánh giữa chúng. Đây là một bước nâng cấp của prompt CoT.\
\
Cách áp dụng CoT: Tạo ra x nhánh suy luận để giải quyết vấn đề A. Mỗi nhánh được dùng để đánh giá A1, A2, A3,… kêu LLM liên tục đánh giá và đào sâu vào những nhánh tốt nhất để có kết quả tốt nhất.\
\


#### Tình huống 1: Xây dựng kế hoạch cải thiện sức khỏe cá nhân. <a href="#menuid7" id="menuid7"></a>

\
Prompt:\
&#xNAN;_&#x54;ôi 35 tuổi, là nhân viên văn phòng, ít vận động. Tôi muốn cải thiện sức khỏe tổng thể trong 3 tháng tới._\
\
&#xNAN;_&#x59;êu cầu:_\
&#xNAN;_&#x47;iai đoạn 1: Đề xuất chiến lược tổng quát_\
&#xNAN;_&#x48;ãy đề xuất 3 chiến lược tổng thể riêng biệt để tôi cải thiện sức khỏe. Mỗi chiến lược cần có một tên gọi ngắn gọn và mô tả cốt lõi._\
\
&#xNAN;_&#x47;iai đoạn 2: Phân tích chi tiết từng chiến lược_\
&#xNAN;_&#x56;ới mỗi chiến lược đã đề xuất ở Giai đoạn 1:_\
&#xNAN;_&#x61;. Liệt kê ít nhất 3-5 hành động cụ thể cần thực hiện._\
&#xNAN;_&#x62;. Nêu rõ 2-3 ưu điểm chính._\
&#xNAN;_&#x63;. Nêu rõ 2-3 nhược điểm hoặc thách thức chính._\
\
&#xNAN;_&#x47;iai đoạn 3: Đánh giá và lựa chọn chiến lược_\
&#xNAN;_&#x44;ựa trên phân tích ở Giai đoạn 2, và xét đến lối sống của tôi (nhân viên văn phòng, bận rộn, ngân sách vừa phải), hãy:_\
&#xNAN;_&#x61;. Đánh giá ngắn gọn từng chiến lược về tính khả thi và bền vững đối với tôi._\
&#xNAN;_&#x62;. Chọn ra MỘT chiến lược mà bạn cho là phù hợp nhất. Nêu rõ lý do lựa chọn._\
\
&#xNAN;_&#x47;iai đoạn 4: Xây dựng kế hoạch hành động chi tiết_\
&#xNAN;_&#x56;ới chiến lược đã được chọn ở Giai đoạn 3, hãy xây dựng một kế hoạch hành động chi tiết gồm đúng mười bước cụ thể, có thể theo dõi được hàng tuần, để tôi bắt đầu thực hiện ngay. Kế hoạch này nên bao gồm các mục tiêu nhỏ và cách đo lường tiến độ nếu có thể._\
\
\


#### Tình huống 2: chọn quà sinh nhật cho bạn <a href="#menuid8" id="menuid8"></a>

\
Prompt:\
&#xNAN;_&#x42;ạn thân của tôi là nữ, 28 tuổi, có sở thích đọc sách, nấu ăn và đi du lịch bụi. Ngân sách của tôi cho món quà là dưới 1 triệu đồng. Sinh nhật bạn trong vòng một tuần tới._\
\
&#xNAN;_&#x59;êu cầu:_\
&#xNAN;_&#x47;iai đoạn 1: Đề xuất ý tưởng quà tặng ban đầu_\
&#xNAN;_&#x48;ãy gợi ý ba ý tưởng quà tặng độc đáo và ý nghĩa, phù hợp với sở thích và ngân sách đã nêu._\
\
&#xNAN;_&#x47;iai đoạn 2: Phân tích chi tiết từng ý tưởng quà tặng_\
&#xNAN;_&#x56;ới mỗi ý tưởng quà tặng đã đề xuất ở Giai đoạn 1:_\
&#xNAN;_&#x61;. Mô tả chi tiết về món quà._\
&#xNAN;_&#x62;. Giải thích tại sao nó phù hợp với sở thích của bạn tôi._\
&#xNAN;_&#x63;. Ước tính chi phí cụ thể và gợi ý nơi có thể mua hoặc đặt làm._\
\
&#xNAN;_&#x47;iai đoạn 3: Đánh giá và lựa chọn ý tưởng quà tặng_\
&#xNAN;_&#x44;ựa trên phân tích ở Giai đoạn 2, hãy:_\
&#xNAN;_&#x61;. Đánh giá từng ý tưởng dựa trên các tiêu chí: mức độ thể hiện sự quan tâm và thấu hiểu, sự phù hợp với ngân sách, và tính khả thi để chuẩn bị trong vòng một tuần._\
&#xNAN;_&#x62;. Chọn ra MỘT (1) ý tưởng quà tặng mà bạn cho là tốt nhất. Nêu rõ lý do lựa chọn._\
\
&#xNAN;_&#x47;iai đoạn 4: Hoàn thiện ý tưởng được chọn_\
&#xNAN;_&#x56;ới ý tưởng quà tặng đã được chọn ở Giai đoạn 3:_\
&#xNAN;_&#x61;. Gợi ý 1-2 cách gói quà sáng tạo và phù hợp với món quà._\
&#xNAN;_&#x62;. Viết một lời chúc sinh nhật ngắn gọn (khoảng 2-3 câu) nhưng chân thành và ý nghĩa, có liên quan đến món quà hoặc sở thích của bạn tôi._\
\
![Screenshot 2025-05-10 164707.png](https://photo2.tinhte.vn/data/attachment-files/2025/05/8728541_Screenshot_2025-05-10_164707.png)\
\


#### Tình huống 3: Tư vấn vay cho khách hàng doanh nghiệp <a href="#menuid9" id="menuid9"></a>

\
Prompt:\
&#xNAN;_&#x54;ôi là chuyên viên quan hệ khách hàng tại ngân hàng ABC. Một khách hàng SME hoạt động trong lĩnh vực sản xuất hàng tiêu dùng nhanh (FMCG) được 3 năm, có doanh thu ổn định, cần vay một khoản vốn 2 tỷ VNĐ để mở rộng nhà xưởng và đầu tư thêm máy móc. Khách hàng ưu tiên lãi suất cạnh tranh và thủ tục giải ngân nhanh chóng._\
\
&#xNAN;_&#x59;êu cầu:_\
&#xNAN;_&#x47;iai đoạn 1: Đề xuất các gói sản phẩm vay tiềm năng_\
&#xNAN;_&#x44;ựa trên thông tin khách hàng và nhu cầu vay, hãy đề xuất ba (3) gói sản phẩm vay vốn khác nhau của ngân hàng (hoặc các loại hình vay phổ biến) mà khách hàng SME này có thể xem xét. (Ví dụ: Vay đầu tư trung dài hạn, Vay vốn lưu động có tài sản đảm bảo, Thấu chi doanh nghiệp)._\
\
&#xNAN;_&#x47;iai đoạn 2: Phân tích chi tiết từng gói sản phẩm vay_\
&#xNAN;_&#x56;ới mỗi gói sản phẩm vay đã đề xuất ở Giai đoạn 1:_\
&#xNAN;_&#x61;. Mô tả các đặc điểm chính (mục đích vay, hạn mức, thời hạn vay, loại tài sản đảm bảo yêu cầu nếu có)._\
&#xNAN;_&#x62;. Nêu rõ 2-3 ưu điểm nổi bật của gói vay đó đối với nhu cầu của khách hàng này (ví dụ: lãi suất ưu đãi, thời gian phê duyệt nhanh, linh hoạt trong trả nợ)._\
&#xNAN;_&#x63;. Nêu rõ 2-3 nhược điểm hoặc điều kiện ràng buộc chính (ví dụ: yêu cầu tài sản đảm bảo giá trị cao, phí trả nợ trước hạn, quy trình thẩm định phức tạp hơn)._\
\
&#xNAN;_&#x47;iai đoạn 3: Đánh giá và lựa chọn gói sản phẩm phù hợp nhất_\
&#xNAN;_&#x44;ựa trên phân tích ở Giai đoạn 2, và ưu tiên của khách hàng (lãi suất cạnh tranh, giải ngân nhanh), hãy:_\
&#xNAN;_&#x61;. So sánh, đánh giá mức độ phù hợp của từng gói sản phẩm với nhu cầu cụ thể và khả năng đáp ứng của khách hàng._\
&#xNAN;_&#x62;. Chọn ra MỘT (1) gói sản phẩm vay mà bạn cho là tối ưu nhất cho khách hàng này. Giải thích chi tiết lý do lựa chọn, nhấn mạnh việc nó đáp ứng tốt nhất các tiêu chí của khách hàng như thế nào._\
\
&#xNAN;_&#x47;iai đoạn 4: Xây dựng kế hoạch tư vấn và các bước tiếp theo_\
&#xNAN;_&#x56;ới gói sản phẩm vay đã được chọn ở Giai đoạn 3:_\
&#xNAN;_&#x61;. Liệt kê những thông tin và giấy tờ chính mà khách hàng cần chuẩn bị để tiến hành làm hồ sơ vay._\
&#xNAN;_&#x62;. Đề xuất các điểm chính cần nhấn mạnh khi tư vấn trực tiếp cho khách hàng về gói vay này để thuyết phục họ._\
&#xNAN;_&#x63;. Phác thảo các bước tiếp theo trong quy trình xử lý hồ sơ vay cho khách hàng tại ngân hàng._\
\


#### Tình huống 4: Thẩm định dự án <a href="#menuid10" id="menuid10"></a>

\
Prompt:\
&#xNAN;_&#x54;ôi là chuyên viên thẩm định rủi ro. Công ty tôi đang xem xét đầu tư hoặc cho vay một dự án xây dựng khu chung cư cao cấp tại một quận mới nổi của thành phố lớn. Dự án có quy mô 500 căn hộ, chủ đầu tư có kinh nghiệm vừa phải. Thời gian dự kiến hoàn thành là 3 năm._\
\
&#xNAN;_&#x59;êu cầu:_\
&#xNAN;_&#x47;iai đoạn 1: Xác định các nhóm rủi ro chính_\
&#xNAN;_&#x48;ãy xác định ba (3) nhóm rủi ro chính mà dự án bất động sản này có thể phải đối mặt trong quá trình phát triển và vận hành. (Ví dụ: Rủi ro thị trường, Rủi ro pháp lý, Rủi ro xây dựng/vận hành)._\
\
&#xNAN;_&#x47;iai đoạn 2: Phân tích chi tiết các rủi ro trong từng nhóm. Với mỗi nhóm rủi ro đã xác định ở Giai đoạn 1:_\
&#xNAN;_&#x61;. Liệt kê ít nhất 2-3 rủi ro cụ thể thuộc nhóm đó (ví dụ, trong nhóm Rủi ro thị trường: biến động giá bán, tỷ lệ hấp thụ thấp; trong nhóm Rủi ro pháp lý: thay đổi quy hoạch, chậm cấp phép)._\
&#xNAN;_&#x62;. Đánh giá sơ bộ mức độ ảnh hưởng tiềm tàng của từng rủi ro cụ thể đó đến dự án (Cao, Trung bình, Thấp)._\
&#xNAN;_&#x63;. Gợi ý một số yếu tố có thể làm tăng hoặc giảm khả năng xảy ra của rủi ro đó._\
\
&#xNAN;_&#x47;iai đoạn 3: Đánh giá tổng thể và ưu tiên các rủi ro cần quan tâm nhất. Dựa trên phân tích ở Giai đoạn 2:_\
&#xNAN;_&#x61;. Đưa ra nhận định tổng thể về mức độ rủi ro của dự án này dựa trên các yếu tố đã phân tích._\
&#xNAN;_&#x62;. Xác định 2-3 rủi ro cụ thể (từ tất cả các nhóm) mà bạn cho là nghiêm trọng nhất và cần được ưu tiên xem xét, đánh giá sâu hơn. Nêu rõ lý do._\
\
&#xNAN;_&#x47;iai đoạn 4: Đề xuất các biện pháp giảm thiểu và câu hỏi thẩm định sâu. Với các rủi ro nghiêm trọng nhất đã được ưu tiên ở Giai đoạn 3:_\
&#xNAN;_&#x61;. Đề xuất 1-2 biện pháp giảm thiểu hoặc kiểm soát tiềm năng cho mỗi rủi ro đó._\
&#xNAN;_&#x62;. Xây dựng một danh sách gồm 5-7 câu hỏi chi tiết cần đặt ra cho chủ đầu tư hoặc các bên liên quan để thu thập thêm thông tin, thẩm định sâu hơn về các rủi ro này._\
\
Các bạn hãy chạy thử các prompt bên trên vào các chatbot mà bạn đang xài để xem kết quả ha. Về bản chất, ToT chính là phần cốt lõi trong hoạt động của các model reasoning của ChatGPT hay Gemini, Grok đều làm sẵn hiện tại. Họ sẽ dựa trên yêu cầu người dùng nhập vào, tự hoạch định các hướng suy luận của LLM, tẻ nhánh liên tục để có được kết quả tốt nhất trả về.\
\
Việc hiểu được hoạt động của ToT không chỉ giúp chúng ta hiểu được LLM reasoing chạy như thế nào, mà ngoài ra, chúng ta sẽ có thể tự vạch ra các prompt để giải quyết vấn đề của bản thân. Có một số tình huống mà chúng ta cần "tiết kiệm" khi làm việc với LLM, thí dụ như code, suy luận,… và dùng API / hoặc các LLM có windows context không lớn, việc tối ưu prompt bằng ToT sẽ là một trong những giải pháp giúp tiết kiệm số token ít ỏi thay vì cứ thoải mái bung cho LLM nó tự tạo, bất kể là nó có tính năng đó hay không.\
\
Tới đây, qua 4 bài viết, gần như chúng ta đã đi qua hết toàn bộ những kỹ thuật trong prompt engineering, từ đơn giản như zero shot, tới single shot hay few shot, rồi tới các kỹ thuật phức tạp như Chain of Thought hay Tree of Thought. Áp dụng linh hoạt tất cả những thứ này sẽ giải quyết được mọi bài toán làm việc với LLM mà chúng ta gặp phải. Ở bài tới, mình sẽ đề cập tới việc tự động hóa, dùng LLM để tạo prompt LLM,…. Cám ơn các bạn đã đọc bài nhé.
