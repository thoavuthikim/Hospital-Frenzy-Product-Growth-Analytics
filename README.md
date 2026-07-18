# Product & Growth Analytics: FRENZY HOSPITAL GAME

## 📌 Project Objective (Mục tiêu dự án)
Dự án xây dựng hệ thống báo cáo phân tích toàn diện về các chỉ số hiệu suất kinh doanh và hành vi trải nghiệm của người chơi trong tựa game mô phỏng **Hospital Frenzy** (Tháng 07/2025).

Tập trung giải quyết 3 bài toán cốt lõi:
1. **Tài chính & Tăng trưởng (Finance & Growth):** Tối ưu hóa chi phí Marketing, đo lường hiệu quả các kênh quảng cáo và phân tách rõ lợi nhuận từ IAP (nạp tiền) và IAA (xem quảng cáo).
2. **Giữ chân người chơi (Retention):** Chẩn đoán nguyên nhân người chơi mới rời bỏ game (Churn) ngay sau ngày đầu tiên tải về và điểm nghẽn trong hành trình người dùng.
3. **Cân bằng game (Gameplay Balance):** Đánh giá trải nghiệm, đo lường độ khó từng Level để tránh gây ức chế hoặc nhàm chán cho người chơi.

---

## 🛠 Data Architecture & Processing (Cấu trúc & Xử lý dữ liệu)

Dữ liệu thô được trích xuất trực tiếp từ ứng dụng game trên nền tảng Android thông qua **Google BigQuery**. 

*   **Step 1 - Data Integration:** Sử dụng **SQL** để gộp (Merge) 4 bảng sự kiện rời rạc (`ad_impression`, `in_app_purchase`, `level_end`, `tutorial`) thành một bảng tổng quan `overview_data` phục vụ tính toán chính xác lượng Daily Active Users (DAU).
*   **Step 2 - Data Transformation:** Làm sạch và chuẩn hóa kiểu dữ liệu bằng **Power Query**.
*   **Step 3 - Data Modeling:** Xây dựng mô hình **Star Schema** trên Power BI, tạo các bảng Dimension (`Dim_Date`, `Dim_Device`) kết nối với các bảng Fact để tối ưu hóa hiệu suất truy vấn.

### Data Dictionary (Các bảng dữ liệu chính)
| Bảng (Table) | Nội dung |
| :--- | :--- |
| `ad_impression` | Chi tiết lượt xem quảng cáo của người dùng theo định dạng. |
| `in_app_purchase`| Chi tiết giao dịch nạp tiền mua gói quà tặng trong game. |
| `tutorial` | Ghi nhận tiến trình qua từng bước hướng dẫn tân thủ (FTUE). |
| `level_end` | Thông tin vòng chơi (thắng/thua/thoát ngang) và max level. |
| `cost` | Chi phí chạy quảng cáo marketing hàng ngày. |

---

## 📊 Key Metrics & DAX (Các chỉ số phân tích)

Xây dựng các measures tính toán phức tạp bằng **DAX** trên Power BI, phân bổ vào 4 mảng chính:
1. **Overview Analysis:** `Total Revenue`, `DAU`, `ARPDAU`, `ARPPU`, `Buyer Rate`, `Engagement Rate`.
2. **Financial Analysis:** `ROAS D7`, `Total Cost`, `Cost Per Install (CPI)`, `Cumulative ARPU (D0, D3, D7)`, `IAA/IAP Revenue`.
3. **Customer Journey:** `D1 Retention Rate`, `FTUE Rate`, `Win/Lose/Quit Rate at Levels`, `Avg Attempts to Win`.
4. **Customer Segment:** Ứng dụng **Mô hình RFM (Recency, Frequency, Monetary)** phân loại người chơi thành các nhóm: *VIP, Big Spenders, Frequent Customers, At Risk, Churn...*

---

## 💡 Core Insights & Action Plan (Phát hiện & Đề xuất)

### 🟢 1. Bản địa hóa Monetization (Thị trường Mỹ vs. Ấn Độ)
*   **Insight:** Mỹ mang lại doanh thu nạp tiền (IAP) vượt trội nhưng số lượng người dùng vừa phải. Ngược lại, Ấn Độ có lượng Active User khổng lồ nhưng không nạp tiền (doanh thu chủ yếu từ IAA).
*   **Action Plan:** Khai thác tối đa tệp người chơi Mỹ qua các gói nạp cao cấp (High IAP). Riêng thị trường Ấn Độ, tuyệt đối không ép nạp tiền, chỉ tập trung tối ưu hiển thị quảng cáo thưởng (Rewarded Ads).

### 🔴 2. Lỗ hổng Trải nghiệm Tân thủ (FTUE) & Điểm nghẽn Level
*   **Insight:** Hành trình tân thủ (FTUE) quá tệ khiến game "mất trắng" 23.2% (12.000 users) ngay từ màn đầu tiên. Ngoài ra, Level 401 có tỷ lệ thua lên đến 96.73%, Level 135 thua 76.02% và lỗi ở Level 39 khiến tỷ lệ thoát (Quit rate) đạt đỉnh 16.88%.
*   **Action Plan:** Rút ngắn/đơn giản hóa hướng dẫn nâng cấp (Upgrade). Hạ độ khó (giảm chỉ số quái) ở Level 135 và 401. Khẩn trương fix lỗi chèn ép quảng cáo tại Level 39.

### 🟡 3. Rủi ro Phân cực người chơi & Hụt hơi Live-Ops
*   **Insight:** Người chơi bị phân cực rõ rệt (siêu nhiệt tình hoặc chơi thử rồi bỏ), tệp khách hàng tiềm năng ở giữa bị đứt gãy. Doanh thu quảng cáo (IAA) biến mất hoàn toàn vào cuối tháng (28-30/07).
*   **Action Plan:** Kích cầu bằng Live-Ops cuối tháng. Tung gói "mồi" Starter Pack ($0.99) để chuyển đổi 34.000 user cày chay thành người trả phí. Tạo chuỗi nhiệm vụ ngày (Daily Loop) để ép nhóm user ít tương tác phải mở game lại.
