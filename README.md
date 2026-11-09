# Sales-Funnel-Analysis-TechX
Phân tích hiệu quả bán khóa học – TechX

Dự án phân tích dữ liệu CRM của một trung tâm đào tạo công nghệ: lead đến từ đâu, được xử lý ra sao, nhóm nào chuyển đổi tốt và cần cải thiện gì để bán được nhiều khóa học hơn. Dự án gồm làm sạch dữ liệu, mô hình hóa và dashboard Power BI, phân tích mô tả, chẩn đoán, hồi quy và đề xuất hành động.

Công cụ: Python (pandas, NumPy, SciPy, statsmodels, scikit-learn, Matplotlib, Seaborn) · Power BI (Power Query, DAX, mô hình dạng sao) · Google Colab

1. Bài toán

Trung tâm có hơn 18.000 lead nhưng chưa biết:

Phễu lead → tư vấn → chốt sale đang hoạt động tốt đến đâu?
Khóa học, giai đoạn chăm sóc của marketing, tư vấn viên hay nhà mạng có ảnh hưởng đến việc chốt sale không?
Nên ưu tiên cải thiện điều gì để tăng số ca chốt?
2. Dữ liệu
Mục	Mô tả
Nguồn	Xuất từ CRM của trung tâm (Excel)
Quy mô	18.961 bản ghi lead (14.481 tên khách duy nhất), 12/2023 – 12/2024
Biến chính	Khóa học quan tâm, tư vấn viên, ngày tạo lead, mốc chuyển sang tư vấn, mốc chuyển sang chốt sale, giai đoạn chăm sóc của MKT, nhà mạng, sentiment

Lưu ý bảo mật: dữ liệu gốc chứa tên khách hàng nên không được đưa lên repo. Repo chỉ chứa code, kết quả tổng hợp và tài liệu. Cũng không nên commit file .pbix vì file này nhúng sẵn dữ liệu.

3. Quy trình thực hiện
Làm sạch dữ liệu: chuẩn hóa tên cột và nhãn, xử lý giá trị trống/"0", chuẩn hóa mốc thời gian bị lỗi định dạng, kiểm tra thứ tự thời gian (chốt sale trước khi tư vấn).
Mô hình hóa & dashboard (Power BI): mô hình dạng sao gồm 1 bảng fact và 6 bảng chiều (khóa học, giai đoạn MKT, nhà mạng, sentiment, tư vấn viên, ngày). Viết các measure DAX (Consultation Rate, Lead-to-Sale Rate, thời gian tư vấn → chốt...) và dựng dashboard 6 trang có drillthrough và tooltip.
Phân tích mô tả (Python): phân phối theo tình trạng, khóa học, tư vấn viên, nhà mạng, khung giờ và mùa vụ.
Phân tích chẩn đoán: ma trận Cramér's V, kiểm định giả thuyết (Z-test hai tỷ lệ, Mann-Whitney, chi-square) có hiệu chỉnh so sánh bội Holm.
Mô hình hồi quy: hồi quy logistic (tách ảnh hưởng riêng của từng yếu tố, kiểm tra ngoài mẫu theo thời gian), hồi quy Poisson cho số ca chốt, hồi quy tuần cho tác động của số lead mới.
Đề xuất & kế hoạch kiểm chứng: giải pháp theo 5W1H, thiết kế thử nghiệm A/B kèm cỡ mẫu.
4. Kết quả chính

Tất cả so sánh giữa các nhóm dùng cửa sổ lead tạo 05–08/2024 (11.440 lead), xem mục 5.

Phát hiện	Số liệu
Tỷ lệ chốt sale	30,2% toàn kỳ · 40,8% trong cửa sổ dữ liệu đầy đủ
Khóa học ảnh hưởng mạnh nhất	PCIC 43,5% · Data Analyst 37,5% · Full time 22,6% · Fukuoka 15,2%
Giai đoạn chăm sóc của MKT	Vòng 1: 43,9% · Thu hồi: 44,8% · Vòng 2: 25,0%
Tư vấn viên	Chênh lệch 35,6%–47,3% không có ý nghĩa thống kê (p = 0,20); 10,4% lead chưa được gán người phụ trách
Yếu tố có ảnh hưởng (hồi quy, kiểm định tỷ số hợp lý)	Khóa học, giai đoạn MKT, tháng tạo lead. Tư vấn viên, khung giờ, nhà mạng, cuối tuần, sentiment: không có bằng chứng
Số lượng	Thêm 100 lead mới mỗi tuần ≈ +47 ca chốt trong 3 tuần (ước lượng thô, 28 tuần dữ liệu)
Tốc độ chốt	~52% ca chốt trong 6 ngày kể từ lúc tạo lead
Mùa vụ	Lead đỉnh tháng 6 (3.494), ca chốt đỉnh tháng 7 (1.992), lead giảm mạnh từ tháng 9
5. Các vấn đề dữ liệu quan trọng đã xử lý
Mốc chuyển đổi chỉ được ghi từ 17/05/2024. Lead tạo trước đó gần như không thể có mốc chốt, khiến tỷ lệ chốt thấp giả tạo (ví dụ lead tháng 1 chỉ 0,5%). Giải pháp: dùng cửa sổ hiệu chỉnh 05–08/2024 cho mọi so sánh.
477 ca chốt cùng dấu thời gian ngày 11/10/2024 (09:15–09:40). Nghi là cập nhật hàng loạt trên CRM (chưa được xác nhận) nên bị loại khỏi biến kết quả. Nếu giữ lại, Data Analyst trông chốt tốt hơn PCIC (47,9% so với 43,5%); khi loại thì ngược lại. Phân tích độ nhạy được đưa vào code.
Lỗi ánh xạ khóa học trong dashboard: mã Khóa bằng "0" ở 1.600 lead dù tên khóa có trong dữ liệu, khiến dashboard đếm thiếu Data Analyst và Fukuoka. Phân tích Python dùng tên khóa.
82,7% ca chốt thiếu mốc tư vấn, nên "tỷ lệ tư vấn" bị đánh giá thấp và không coi là bước bắt buộc của phễu.
6. Đề xuất
Chuẩn hóa CRM: bắt buộc nhập mốc tư vấn, khóa học, giai đoạn MKT; thêm cột nguồn lead, chi phí, nhập học, lý do từ chối.
Làm lại kịch bản "Chăm sóc vòng 2" (mục tiêu 25% → 35%) và mở rộng chương trình "Thu hồi".
Tăng lượng lead mới, chuẩn bị ngân sách từ tháng 3 cho mùa cao điểm 6–7 và duy trì lead ở mùa thấp.
Quy trình nuôi dưỡng riêng cho Fukuoka; xem xét lại định vị Full time.
Liên hệ lead mới trong 1 giờ, bố trí nhân sự theo khung giờ cao điểm (08:00–11:00 và 13:00–17:00).
Đánh giá tư vấn viên theo cohort thay vì số liệu thô.
