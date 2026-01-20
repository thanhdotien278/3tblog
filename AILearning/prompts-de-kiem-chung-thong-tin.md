# Prompts để kiểm chứng thông tin

### Prompt kiểm chứng thông tin

#### Prompt tiếng Việt

<br>

Kiểm chứng thông tin sau và cung cấp các nguồn đáng tin cậy trực tiếp báo cáo hoặc hỗ trợ thông tin đó. Các nguồn phải bao gồm con số hoặc giá trị dữ liệu CHÍNH XÁC (không chỉ là diễn giải lại).

<br>

“\[Chèn thông tin của bạn vào đây]”

<br>

Liệt kê các nguồn kèm liên kết và chỉ rõ dữ liệu được đề cập ở đâu trong mỗi nguồn.

#### Prompt tiếng Anh

Fact-check the following data point and provide credible sources that directly report or support it. The sources must include the EXACT number or data value (not just paraphrased context).

"\[Insert your data point here]"

List the sources with links and specify where the data is mentioned in each.

### Prompt check nguồn NotebookLM

#### PROMPT TIẾNG VIỆT

Xác định và liệt kê tất cả các điểm mâu thuẫn, không thống nhất hoặc đáng ngờ giữa các tài liệu, nhằm phát hiện nguồn dữ liệu kém tin cậy trước khi sử dụng cho nghiên cứu và phân tích sâu.

Yêu cầu phân tích chi tiết:

1. So sánh chéo thông tin giữa các tài liệu, tập trung vào:
2. Số liệu định lượng (con số, tỷ lệ, thống kê, mốc thời gian).
3. Định nghĩa khái niệm, thuật ngữ chuyên môn.
4. Kết luận, nhận định, hoặc tuyên bố mang tính khẳng định.
5. Phạm vi nghiên cứu, đối tượng, thời gian, phương pháp.
6. Chỉ ra rõ ràng các dạng không nhất quán, bao gồm nhưng không giới hạn:
7. Cùng một chỉ tiêu nhưng số liệu khác nhau.
8. Cùng một sự kiện nhưng mô tả hoặc mốc thời gian khác nhau.
9. Kết luận trái ngược nhau dựa trên cùng hoặc tương tự dữ liệu.
10. Giả định ngầm mâu thuẫn với dữ liệu được trình bày.

#### ENGLISH PROMPT

Identify and document all contradictions, inconsistencies, or red flags across the sources in order to assess data credibility and eliminate unreliable inputs before deeper research and analysis.

Detailed instructions:

1. Perform cross-document comparison, focusing on:
2. Quantitative data (figures, statistics, ratios, timelines).
3. Definitions of key concepts and technical terms.
4. Claims, conclusions, and assertive statements.
5. Study scope, subjects, timeframes, and methodologies.
6. Explicitly flag any form of inconsistency, including but not limited to:
7. Different numerical values for the same metric.
8. Conflicting descriptions or timelines of the same event.
9. Opposing conclusions drawn from similar or identical data.
10. Implicit assumptions that contradict presented evidence.
