# Báo cáo cá nhân — Day 01: Lọc tin nhắn rác bằng ML, DL, Embedding và LLM

> Đổi tên file này thành `MSSV_HoTen.md` của bạn, ví dụ `2A20260000_NguyenVanAn.md`, rồi điền từng mục. Xoá các dòng hướng dẫn in nghiêng khi xong.

- **MSSV:** 2A202603356 
- **Họ và tên:** Nguyễn Huy Thứ
- **SEED** (in ở ô đầu notebook): 4567

## 1. Bằng chứng

| Tệp | Có? |
| --- | --- |
| [`day01_lab.ipynb`](day01_lab.ipynb) — notebook đã chạy, còn output | [x]|
| [`lab1_report.json`](lab1_report.json) — do ô "Báo cáo cuối" sinh ra | [x]|
| [`hand_calc.jpg`](hand_calc.jpg) — ảnh tính tay có các bước | [x]|

## 2. Kết quả chính

1. Sức mạnh của LLM to/đắt tiền

Đoán: Dùng LLM càng to (gpt-6-terra/luna) thì điểm F1 sẽ càng cao, đè bẹp các mô hình ML cũ.

Ra: Thực tế điểm F1 lại rất thê thảm (có lúc về 0, báo lỗi Invalid 40/40 tin).

Vì sao lệch: Do LLM là mô hình tạo sinh tự do, đôi khi không tuân thủ đúng định dạng đầu ra (chỉ in 1 từ "spam"/"ham") mà sinh thêm ký tự lạ, khiến hàm parse_label thất bại toàn tập. Trong khi đó mô hình kNN + Embedding đơn giản lại dự đoán cực kỳ chuẩn xác.

2. Chi phí của Few-shot

Đoán: Nhồi thêm ví dụ vào prompt để AI học theo là cách rẻ và dễ nhất để tăng F1.

Ra: F1 có thể tăng nhẹ hoặc ổn định form hơn, nhưng chi phí (USD) tăng vọt gấp 3 - 5 lần so với Zero-shot.

Vì sao lệch: Do API tính tiền dựa trên lượng token. Prompt càng dài (chứa nhiều tin nhắn mẫu), lượng input tokens càng phình to, dẫn đến tốn kém không phù hợp cho hệ thống scale lớn.

3. Deep Learning (MLP) so với Machine Learning (LogReg)

Đoán: Mạng Nơ-ron nhiều lớp (MLP) với hàng trăm ngàn tham số chắc chắn sẽ phân loại tốt hơn LogReg tuyến tính.

Ra: F1 của MLP không nhỉnh hơn LogReg, thậm chí mô hình bị overfit (học vẹt) rất nhanh sau vài epoch.

Vì sao lệch: Do tính chất dữ liệu đầu vào là Bag-of-Words (ma trận thưa thớt, đếm từ khô khan, mất ngữ cảnh), nên MLP dù to đến mấy cũng không trích xuất thêm được đặc trưng sâu sắc nào.

4. Phân tích 50 case khó

Model tụt nhiều nhất: Mô hình dùng Bag-of-Words (BoW + LogReg) bị tụt điểm nặng nhất ở nhóm tin nhắn ngụy trang (camouflaged) và dùng từ đồng nghĩa.

2 case cụ thể:

Case 1: "Your mobile number has been chosen for a complimentary holiday. Reply YES to confirm." (Thực tế: Spam). BoW đoán là Ham (0) vì không thấy các từ khóa spam quen thuộc như "free", "win". Mô hình Embedding đoán đúng (1) vì nó hiểu "complimentary" nghĩa tương đương "free".

Case 2: "Hey are you free for lunch tomorrow? My treat" (Thực tế: Ham). BoW dễ bị lừa đoán là Spam (1) do có xuất hiện từ "free". Mô hình Embedding đoán đúng (0) vì nó nắm được bối cảnh toàn câu chỉ là lời rủ đi ăn giao tiếp bình thường.

## 3. Dự đoán và thực tế

*Chọn 3 dự đoán `predict(...)` lệch nhiều nhất so với kết quả. Mỗi cái: bạn đoán gì, ra gì, vì sao lệch.*

## 4. Phân tích 50 case khó

*Model nào tụt nhiều nhất, ở nhóm case nào? Nêu 2 case cụ thể và cơ chế khiến model sai.*

## 5. Thử thách cá nhân

*Đề theo SEED của bạn, cách làm, kết quả, giải thích.*

## 6. Phương án cho sản phẩm thật

*Với yêu cầu p95 < 50 ms/tin cho 10 triệu SMS/ngày: chọn phương án nào, dẫn số liệu nào, chấp nhận đánh đổi gì.*

## 7. AI log và điều đã học

*Mỗi lần dùng AI: hỏi gì, nhận gì, bạn sửa gì. Sau đó 2–3 câu: điều bạn hiểu ra hôm nay mà trước đó chưa hiểu.*
