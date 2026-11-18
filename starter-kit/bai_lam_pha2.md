## 4.2. Trích xuất tin nhắn đặt vé ra JSON

**KET QUA AI**

```text
[
{
"ma_goi": "SVT",
"ngay_su_kien": "2026-12-20",
"nguoi_lon": 2,
"tre_em": 1,
"loai_ve": "thuong",
"ghi_chu": null
},
{
"ma_goi": "FZ",
"ngay_su_kien": "2027-01-05",
"nguoi_lon": 3,
"tre_em": 0,
"loai_ve": "thuong",
"ghi_chu": "hỗ trợ xe lăn"
},
{
"ma_goi": "SVT",
"ngay_su_kien": null,
"nguoi_lon": 4,
"tre_em": 0,
"loai_ve": "vip",
"ghi_chu": "hỗ trợ tìm chỗ cho người khiếm thị"
}
]
```

**Link trò chuyện:**https://share.gemini.google/fXDDS6E3XolT

## 4.3. Báo giá vé bằng Chain-of-Thought

**KET QUA AI**

```text
```python
# Data & Pricing calculation
price_adult = 1200000
num_adults = 5
num_children_paid = 2 # ages 8 and 10
num_children_free = 1 # age 3
surcharge_count = 1
surcharge_per_person = 300000

# 1. tiền người lớn
total_adult = price_adult * num_adults

# 2. tiền trẻ em
child_price = price_adult * 0.7
total_child = child_price * num_children_paid

# 3. tiền vé (tiền người lớn + tiền trẻ em)
total_ticket = total_adult + total_child

# 4. giảm giá nhóm (Nhóm từ 5 người lớn trở lên được giảm 5% trên tiền vé)
# num_adults = 5, which is >= 5
discount = total_ticket * 0.05
total_ticket_after_discount = total_ticket - discount

# 5. phụ thu
total_surcharge = surcharge_count * surcharge_per_person

# 6. tổng tiền = tiền vé sau giảm + phụ thu
total_amount = total_ticket_after_discount + total_surcharge

print(f"Tiền người lớn: {total_adult}")
print(f"Tiền trẻ em: {total_child}")
print(f"Tiền vé: {total_ticket}")
print(f"Giảm giá nhóm: {discount}")
print(f"Tiền vé sau giảm: {total_ticket_after_discount}")
print(f"Phụ thu: {total_surcharge}")
print(f"Tổng tiền: {total_amount}")


```

```text
Tiền người lớn: 6000000
Tiền trẻ em: 1680000.0
Tiền vé: 7680000.0
Giảm giá nhóm: 384000.0
Tiền vé sau giảm: 7296000.0
Phụ thu: 300000
Tổng tiền: 7596000.0


```

Dưới đây là chi tiết bảng báo giá và quá trình tính toán từng bước theo đúng quy định:

1. **Tiền người lớn**:
* Gói SVT có giá vé là 1.200.000 đồng/người lớn.
* Số lượng người lớn: 5 người.
* Thành tiền: $5 \times 1.200.000 = 6.000.000$ đồng.


2. **Tiền trẻ em**:
* Có 2 trẻ em (8 tuổi và 10 tuổi) nằm trong độ tuổi từ 5 đến 11 tuổi, giá vé tính bằng 70% giá người lớn: $2 \times (1.200.000 \times 70\%) = 2 \times 840.000 = 1.680.000$ đồng.
* Có 1 bé 3 tuổi dưới 5 tuổi được miễn phí vé (0 đồng).
* Tổng tiền trẻ em: $1.680.000$ đồng.


3. **Tiền vé**:
* Tổng tiền vé ban đầu (tiền người lớn + tiền trẻ em): $6.000.000 + 1.680.000 = 7.680.000$ đồng.


4. **Giảm giá**:
* Nhóm có 5 người lớn (đạt điều kiện từ 5 người lớn trở lên) được giảm 5% trên tổng tiền vé: $7.680.000 \times 5\% = 384.000$ đồng.
* Tiền vé sau giảm: $7.680.000 - 384.000 = 7.296.000$ đồng.


5. **Phụ thu**:
* Có 1 người lớn chọn khu ghế ưu tiên, mức phụ thu là 300.000 đồng/người (phụ thu không được giảm giá): $1 \times 300.000 = 300.000$ đồng.


6. **Tổng tiền**:
* Tổng tiền thanh toán = Tiền vé sau giảm + Phụ thu = $7.296.000 + 300.000 = 7.596.000$ đồng.



[RESULT: 7.596.000đồng]	
```

**Link trò chuyện:**https://share.gemini.google/OIWdewpnTARi

### b) Kiểm tra kết quả AI

AI kết luận đúng

## 4.4. Dây chuyền xử lý khiếu nại – kết quả Trạm 1

```text
(Dán câu trả lời của AI vào đây (bấm nút Copy dưới câu trả lời) – xoá dòng này)
```

**Link trò chuyện:**
