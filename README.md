# Xây Dựng Chiến Lược Đề Xuất Sản Phẩm Từ Market Basket Analysis


## Tình hình doanh nghiệp

Đội ngũ Marketing nhận thấy khách hàng thường mua nhiều sản phẩm trong cùng một đơn hàng nhưng hệ thống recommendation hiện tại chưa tận dụng được các mối liên hệ này để đề xuất sản phẩm phù hợp.

**Mục tiêu của dự án:**

- Khám phá các nhóm sản phẩm thường được mua cùng nhau.
- Xác định các combo có tiềm năng thương mại cao.
- Đề xuất chiến lược cross-sell và recommendation dựa trên hành vi mua hàng thực tế.

---

## Câu hỏi mục tiêu phân tích:

Để giải quyết được vấn đề của phòng Marketing, dự án phân tích sẽ tập trung trả lời các câu hỏi phân tích quan trọng:

1. Combo sản phẩm nào có mối liên kết mạnh nhất?
2. Có tồn tại các nhóm hành vi mua hàng đặc thù không?
3. Động lực mua hàng nào đứng sau các combo này?
4. Doanh nghiệp nên triển khai recommendation và promotion như thế nào?

---

## 📂 Tổng quan về tệp dữ liệu

Phân tích thực hiện trên 25.728 giỏ hàng (basket).

Danh mục sản phẩm gồm khoảng **3.576 SKU**, được chia thành **17 subcategories** thuộc **5 ngành hàng chính**:

- Face Care
- Hair Care
- Body Care
- Makeup
- Home & Accessories

Phân tích được thực hiện ở cấp độ **Subcategory** để cân bằng giữa:

- Độ chi tiết của hành vi mua hàng
- Khả năng tổng quát hóa kết quả
- Tránh dữ liệu quá phân mảnh ở cấp SKU

---

## 🔍 Phương pháp phân tích

Áp dụng thuật toán **Apriori** để khai phá Association Rules.

### Tham số lọc rules

| Chỉ số | Ý nghĩa | Ngưỡng lọc |
|----------|----------|----------|
| Support | Tỷ lệ đơn hàng chứa tổ hợp sản phẩm 1 & 2 | 0.1% |
| Confidence | Xác suất xuất hiện của nhóm sản phẩm 2 khi nhóm sản phẩm 1 xuất hiện | 10% |
| Lift | Độ mạnh của mối liên hệ giữa các nhóm sản phẩm so với ngẫu nhiên | 1.2 |

### Kết quả lọc rules

- 994 Association Rules thỏa điều kiện lọc.
- Đánh giá rule dựa trên:
  - Support
  - Confidence
  - Lift
  - Leverage

---

## Giỏ hàng của tệp khách hàng hiện tại nói chúng ta nghe điều gì?

### 1. Nail Care là ngành hàng phổ biến nhất

Nail Care xuất hiện trong khoảng **20% đơn hàng**.

Do tỷ lệ xuất hiện nền cao nên các rule có Nail Care ở nhóm sản phẩm thứ 2 (Consequent) thường đạt confidence lớn.

**Tuy nhiên:**

- Confidence cao không đồng nghĩa với quan hệ nhân quả.
- Không thể kết luận rằng sản phẩm ở nhóm sản phẩm thứ 1 (antecedent) tạo ra nhu cầu mua Nail Care.

### 2. Khách hàng có xu hướng mua theo **thói quen chăm sóc cá nhân**

Ngoài Nail Care, các nhóm thường xuất hiện cùng nhau bao gồm:

- Bath Oils
- Body Moisturizers
- Hand Creams
- Shampoos & Conditioners

Dữ liệu cho thấy khách hàng không mua từng sản phẩm đơn lẻ mà thường xây dựng một routine chăm sóc cơ thể, tóc và bàn tay trong cùng một đơn hàng.

**Business Implication:**

- Tiềm năng xây dựng combo sản phẩm.
- Tăng hiệu quả cross-sell theo routine thay vì theo từng SKU riêng lẻ.

### 3. Xuất hiện phân khúc khách hàng yêu thích **tự chăm sóc tại nhà**

Một số combo kết hợp:

- Hair Care
- Face Care
- Body Care
- Home & Relaxation Products

có Lift dao động từ **1.75 - 1.90**.

Tuy nhiên:

- Support thấp
- Leverage thấp

=> Đây là hành vi của một phân khúc khách hàng nhỏ nhưng có sở thích rõ rệt.

**Business Implication:**

Không phù hợp triển khai đại trà nhưng có tiềm năng cho recommendation cá nhân hóa.


---

## Đề xuất hành động Marketing:

994 association rules được phân tích và ưu tiên thành 3 nhóm hành động:

| Ưu tiên | # Rules | Mục tiêu | Hành động |
|-----------|---------:|-----------|-----------|
| 🟢 1 | confidence > 0.3; lift = [1.6 - 1.8]; leverage > 90 | Triển khai ngay | Recommendation, Cross-sell, Bundle Promotion |
| 🔵 2 | lift > 1.7; confidence < 0.25 | Cá nhân hóa | Segment-based Recommendation, CRM |
| 🟡 3 |  lift = [1.35–1.70]; confidence = [0.15–0.3]; leverage > 90 | Theo dõi & thử nghiệm | A/B Testing, Validation |

---

## Hạn chế của phân tích hiện tại

Framework hiện tại mới phân loại được khoảng **13% trong tổng số 994 rules**.

Một số hạn chế:

- Ngưỡng phân loại được xác định thủ công.
- Các nhóm có thể chồng lấn nhau.
- Chưa đảm bảo tính MECE.

---

## 🔜 Hướng phát triển & cải thiện:

### 1. Xây dựng Composite Score

Kết hợp:

- Lift
- Confidence
- Leverage

thành một chỉ số tổng hợp giúp so sánh và xếp hạng rules hiệu quả hơn.

### 2. Phân cụm Association Rules

- Khám phá các nhóm hành vi tự nhiên.
- Giảm phụ thuộc vào business rule thủ công.

### 3. Sequential Pattern Mining

Market Basket Analysis chỉ trả lời:

> "Khách hàng mua A và B cùng nhau"

Nhưng chưa trả lời được:

> "Khách hàng mua A trước hay mua B trước?"

Nếu có dữ liệu theo thời gian, Sequential Pattern Mining có thể giúp xây dựng hệ thống recommendation chính xác hơn dựa trên hành trình mua hàng thực tế.

---

## Kỹ thuật phân tích

- Python
- Pandas
- Mlxtend (Apriori)
- NumPy
- Matplotlib
- Seaborn

---
## Cấu trúc dự án

├── data/
│   ├── EcomSales.csv
│   │   └── Dữ liệu giao dịch với 51.290 dòng ở cấp độ Order Line Item
│   └── Product.csv
│       └── Danh mục sản phẩm và thông tin phân loại
├── Market Basket Analysis_Ecommerce dataset.ipynb
│   └── Tiền xử lý dữ liệu, xây dựng mô hình Apriori 
│       và phân tích Association Rules
└── README.md
    └── Tổng quan dự án, kết quả phân tích và khuyến nghị kinh doanh                                                  
