# Research Claw

#### Bước 1: Chuẩn bị Môi trường <a href="#user-content-buoc-1-chuan-bi-moi-truong" id="user-content-buoc-1-chuan-bi-moi-truong"></a>

Vì bạn đã cài đặt mã nguồn vào thư mục `/Users/Super/.gemini/antigravity/scratch/AutoResearchClaw`, hãy di chuyển vào thư mục này và kích hoạt môi trường ảo:

```
bashcd /Users/Super/.gemini/antigravity/scratch/AutoResearchClawsource .venv/bin/activate
```

_(Lưu ý: Bạn luôn cần chạy `source .venv/bin/activate` mỗi khi mở terminal mới để sử dụng lệnh `researchclaw`)_.

#### Bước 2: Cấu hình API và Hệ thống (Config) <a href="#user-content-buoc-2-cau-hinh-api-va-he-thong-config" id="user-content-buoc-2-cau-hinh-api-va-he-thong-config"></a>

AutoResearchClaw sử dụng file cấu hình YAML để thiết lập mọi thứ từ API key, chủ đề nghiên cứu, đến thông số thực nghiệm. Bạn đã có sẵn file `config.arc.yaml`.

Mở file `config.arc.yaml` lên (hoặc dùng text editor yêu thích của bạn) và chỉnh sửa các thành phần cốt lõi sau:

**1. Khai báo LLM API (Bắt buộc):** Mặc định hệ thống dùng định dạng API tương tự OpenAI. Bạn có thể dùng OpenAI, Claude, hoặc bất kỳ provider nào hỗ trợ chuẩn này.

```
yamlllm:  provider: "openai-compatible"      base_url: "https://api.openai.com/v1"  # Thay đổi URL nếu bạn dùng hãng khác (ví dụ: OpenRouter)  api_key_env: "OPENAI_API_KEY"          # Biến môi trường chứa API Key  primary_model: "gpt-4o"                # Model chính (ưu tiên model xịn nhất)  fallback_models: ["gpt-4o-mini"]       # Model dự phòng nếu lỗi
```

**2. Khai báo Chủ đề nghiên cứu (Research Topic):**

```
yamlproject:  name: "project-nghien-cuu-cua-toi"  # Tên project lưu trữ kết quả  research:  topic: "Cải thiện độ chính xác của mô hình Transformer trong nhận dạng giọng nói tiếng Việt sử dụng cơ chế chú ý cục bộ"   domains: ["ml", "nlp", "speech"]    # Lĩnh vực nghiên cứu
```

**3. Thiết lập Môi trường chạy Code (Thực nghiệm):** Để an toàn, hệ thống mặc định chạy code sinh ra trong sandbox.

```
yamlexperiment:  mode: "sandbox"                       # Có thể đổi thành "docker" nếu bạn đã cài Docker  time_budget_sec: 300                  # Thời gian tối đa chạy code thử nghiệm  sandbox:    python_path: ".venv/bin/python"     # Trỏ vào môi trường ảo hiện tại
```

#### Bước 3: Cung cấp API Key vào Terminal <a href="#user-content-buoc-3-cung-cap-api-key-vao-terminal" id="user-content-buoc-3-cung-cap-api-key-vao-terminal"></a>

Trước khi khởi chạy, bạn cần export API key tương ứng với cấu hình `api_key_env` ở Bước 2.

```
bashexport OPENAI_API_KEY="sk-api-key-cua-ban-o-day"
```

_(Nếu bạn dùng Semantic Scholar API để lấy bài báo nhanh hơn, bạn có thể thiết lập thêm `llm.s2_api_key` trong file config)._

#### Bước 4: Khởi chạy Pipeline Nghiên cứu <a href="#user-content-buoc-4-khoi-chay-pipeline-nghien-cuu" id="user-content-buoc-4-khoi-chay-pipeline-nghien-cuu"></a>

Sử dụng lệnh sau để bắt đầu tiến trình 23 bước tự động:

```
bashresearchclaw run --config config.arc.yaml \                 --topic "Bạn có thể ghi đè chủ đề vào đây hoặc bỏ qua cờ này để dùng topic trong file config" \                 --auto-approve
```

_Lưu ý về cờ `--auto-approve`: Nếu không có cờ này, hệ thống sẽ dừng lại ở 3 "Quality Gate" (Cổng kiểm duyệt ở cuối giai đoạn Tìm kiếm tài liệu, Thiết kế thực nghiệm, và Đánh giá cuối) để hỏi ý kiến bạn. Dùng cờ này để hệ thống chạy 1 mạch 100% tự động định tuyến lại khi lỗi._

#### Bước 5: Theo dõi Tiến trình <a href="#user-content-buoc-5-theo-doi-tien-trinh" id="user-content-buoc-5-theo-doi-tien-trinh"></a>

Hệ thống sẽ chạy qua 8 giai đoạn chính:

1. **Scoping:** Bẻ nhỏ ý tưởng thành bài toán cây.
2. **Literature:** Lên arXiv và Semantic Scholar tìm bài báo liên quan nhất, đọc và chắt lọc.
3. **Synthesis:** Tự tạo các giả thuyết (Hypotheses) qua tranh luận đa agent (debate).
4. **Design:** Viết mã Python thực nghiệm (tự động nhận diện nếu máy bạn là GPU hay CPU).
5. **Execution:** Chạy thử code. Nếu thuật toán bị lỗi (ví dụ: NaN loss) nó sẽ tự "chữa lành" code.
6. **Analysis:** Đánh giá kết quả. Tự quyết định nên "Báo cáo tiếp" hay "Thử lại hướng khác".
7. **Writing:** Tự động sinh dàn ý và viết 5,000-6,500 chữ theo đúng cấu trúc tiêu chuẩn học thuật. Peer Review vòng trong.
8. **Finalization:** Xuất ra báo cáo kiểm tra trích dẫn và sinh Markdown, LaTeX chuẩn.

#### Bước 6: Nhận Kết quả Đầu ra (Deliverables) <a href="#user-content-buoc-6-nhan-ket-qua-dau-ra-deliverables" id="user-content-buoc-6-nhan-ket-qua-dau-ra-deliverables"></a>

Khi hoàn tất, hệ thống sẽ để lại file ở thư mục có định dạng: `artifacts/rc-YYYYMMDD-HHMMSS-<hash>/deliverables/`

Trong thư mục này, bạn sẽ lấy được:

* `paper_draft.md`: Phiên bản Markdown dễ đọc.
* `paper.tex`: Mã nguồn LaTeX sẵn sàng cho Overleaf (mặc định định dạng NeurIPS).
* `references.bib`: Danh sách trích dẫn thật 100%.
* `experiment runs/`: Thư mục chứa toàn bộ mã nguồn code thực nghiệm đã chạy thành công.
* `charts/`: Các biểu đồ so sánh tự động vẽ.

***

**Một số Tips khi dùng:**

* **Tiền dự kiến:** Môt pipeline này dùng khá nhiều API call (đặc biệt là tranh luận Debate vòng lặp). Nếu chạy GPT-4o, chi phí có thể từ khoảng $1 đến $5 tuỳ độ khó.
* Mới bắt đầu, bạn nên bỏ cờ `--auto-approve` để hệ thống dừng lại hỏi ý kiến bạn ở từng chặng, giúp bạn dễ hình dung quá trình hoạt động của các Agents.
* Có thể dùng lệnh `researchclaw doctor` để kiểm tra môi trường đã ổn định để chạy chưa.
