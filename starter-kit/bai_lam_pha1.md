## 4.1. Bảo vệ dữ liệu và chống ảo giác

### a) Ghi chú đã ẩn danh hoá

**GHI CHU DA AN DANH**

```text
Khán giả [Ten_khach_hang], số điện thoại [So_dien_thoai], số CCCD [So_CCCD], email [Dia_Chi_email], đặt 3 vé gói SVT trận bán kết Cúp Sao Vàng ngày 18/12/2026 cho 2 người lớn và 1 trẻ em 9 tuổi. Khán giả đã chuyển khoản đặt cọc 1.500.000 đồng. Khán giả yêu cầu 3 ghế ngồi cạnh nhau gần lối đi.

```

### b) Hai câu lệnh chống ảo giác

**CAU LENH**

```text
Câu 1: Dựa trên file chính sách hoàn/hủy vé mà  tôi gửi hãy trả lời câu hỏi trên của tôi. Nếu các thông tin không có trong chính sách thì không tự bịa chính sách mà trả lời trực tiếp là "Chính sách về điều khoản này không tồn tại"
Câu 2: khi bạn trả lời thì hay ghi rõ là câu trả lời được lấy từ mục hoặc điều khoản nào trong file chính sách hoàn hủy đó
```

## 4.2. Trích xuất tin nhắn đặt vé ra JSON

**PROMPT**

```text
    C-R-E-A-T-E

    [Context] Bạn là một chuyên gia p   hân tích tin nhắn khách hàng,  bạn có nhiệm vụ chuyển các tin nhắn thành định dạng JSON

    [Reguest] Bạn hãy chuyển dữ liệu TN1, TN2, TN3 trong thẻ <tin_nhan> thành một mảng JSON 3 đối tương, đúng thứ tự, đúng 6 trường như sau "ma_goi" , "ngay_su_kien" , "nguoi_lon" , "tre_em" , "loai_ve" , "ghi_chu". Như tin nhắn mẫu

    [Explanation] Khi chuyển thành JSON bạn cần bám theo quy tắc chuẩn hóa trong thẻ <quy_chuan> . Bạn có thể xem qua 2 ví dụ mẫu mà tôi đã thực hiện trong thẻ <vi_du>

    <quy_chuan>
        BẢNG MÃ GÓI
        SVT – Sân Vận Động: vé trận đấu
        FZ – Fan Zone: khu cổ vũ
        VIP – Khán đài VIP

        QUY TẮC CHUẨN HOÁ (5 quy tắc)
        1. ma_goi lấy theo bảng mã gói.
        2. ngay_su_kien dạng YYYY-MM-DD; tin nhắn không ghi năm thì lấy năm 2026; không có ngày cụ thể thì null.
        3. Khách không nhắc trẻ em thì tre_em là 0 và mọi khách là người lớn.
        4. loai_ve chỉ nhận "thuong", "vip" hoặc null.
        5. ghi_chu chỉ ghi yêu cầu đặc biệt về chỗ ngồi, hỗ trợ di chuyển hoặc ăn uống; không có thì null.

        Câu trả lời không chào hỏi, không giải thích. Chỉ có dữ liệu JSON trả về
    </quy_chuan>

    <vi_du>
        Tin nhắn mẫu 1:
        Đặt giúp mình 2 vé VIP trận chung kết ngày 15/11/2026, 2 người lớn và 1 bé. Cần chỗ ngồi cạnh lối đi.
        Kết quả đúng:
        {"ma_goi": "VIP", "ngay_su_kien": "2026-11-15", "nguoi_lon": 2, "tre_em": 1, "loai_ve": "vip", "ghi_chu": "chỗ ngồi cạnh lối đi"}

        Tin nhắn mẫu 2:
        Cho mình một vé Fan Zone, ngày nào cũng được, mình đi một mình.
        Kết quả đúng:
        {"ma_goi": "FZ", "ngay_su_kien": null, "nguoi_lon": 1, "tre_em": 0, "loai_ve": "thuong", "ghi_chu": null}
    </vi_du>

    [Action] Đọc dữ liệu của TN khách hàng mà tôi đưa => chuẩn hóa về đúng các trường dữ liệu => Trả đúng về định dạng JSON 

    [TONE] Chuyên nghiệp , ngắn gọn , xúc tích 

    [EXTRA] Định dạng đầu ra chỉ là dữ liệu JSON

    <tin_nhan>
        TN1: Mình muốn đặt gói Sân Vận Động trận tứ kết ngày 20/12 nha, 2 người lớn với 1 bé 8 tuổi, vé thường.

        TN2: Cho 3 người đi Fan Zone trận ngày 05/01/2027 ạ. Một người cần hỗ trợ xe lăn.

        TN3: Trận chung kết Cúp Sao Vàng tháng 12 còn vé không ạ? Nhà em 4 người lớn, có một người khiếm thị cần hỗ trợ tìm chỗ, lấy vé VIP.

    </tin_nhan>
```

## 4.3. Báo giá vé bằng Chain-of-Thought

### a) Prompt

**PROMPT**

[Context] bạn là nhân viên kế toán chuyên nghiệp và nhiệm vụ của bạn là tính tổng tiền theo <bang_gia_ve> và tính theo quy tắc theo <quy_tac_tinh_gia> và phải suy luận từng bước bảng <yeu_cau_bao_gia> theo <thu_tu_tien> 

```text
<bang_gia_ve>
    BẢNG GIÁ VÉ (đồng/người lớn)
    SVT – Sân Vận Động: 1.200.000
    FZ – Fan Zone: 800.000
    VIP – Khán đài VIP: 2.500.000
</bang_gia_ve>

<quy_tac_tinh_gia>
    QUY TẮC TÍNH GIÁ
    1. Trẻ em từ 5 đến 11 tuổi tính 70% giá người lớn. Trẻ dưới 5 tuổi miễn phí.
    2. Nhóm từ 5 người lớn trở lên được giảm 5% trên tiền vé (tiền người lớn + tiền trẻ em).
    3. Phụ thu khu ghế ưu tiên: 300.000 đồng/người. Phụ thu không được giảm giá.
    4. Tổng tiền = tiền vé sau giảm + phụ thu.
</quy_tac_tinh_gia>

<yeu_cau_bao_gia>
    Nhóm cổ động viên đi xem trận bán kết, mua gói SVT, gồm 5 người lớn, 2 trẻ em (8 tuổi và 10 tuổi) và 1 bé 3 tuổi. Có 1 người lớn chọn khu ghế ưu tiên.
</yeu_cau_bao_gia>

<thu_tu_tien>
theo thứ tư tiền người lớn -> tiền trẻ em -> tiền vé -> giảm giá
nhóm -> phụ thu -> tổng tiền
</thu_tu_tien>

[Extra] dòng cuối chỉ được có [RESULT: <tổng tiền>đồng]
```

## 4.4. Dây chuyền xử lý khiếu nại (Prompt Chaining)

### a) Prompt Trạm 1

**PROMPT**

```text
[Context] bạn là nhân viên chăm sóc khác hàng chuyên nghiệp. Nhiệm vụ của bạn là đọc <email_khieu_nai> và chỉ trả về một đối tượng JSON đúng theo 4 trường "tran_gio" , "sai_khu_vuc" , "nhan_vien" , "khac" trong <chinh_sach>



<email_khieu_nai>

Tôi là khán giả mua vé gói SVT trận Cúp Sao Vàng ngày 02/11/2026, đi 2 người lớn. Ban tổ chức cam kết khu khán đài có mái che nhưng trong 2 hiệp đầu chúng tôi bị xếp sang khu không mái, trời mưa và ghế ướt. Nhân viên soát vé rất nhiệt tình, tôi không có gì phàn nàn về nhân viên. Tôi đề nghị ban tổ chức giải quyết thỏa đáng.

</email_khieu_nai>



<chinh_sach>

CHÍNH SÁCH ĐỀN BÙ CỦA BAN TỔ CHỨC CÚP SAO VÀNG

======================================================



tran_gio: trận đấu bắt đầu trễ trên 2 giờ do ban tổ chức: tặng voucher 200.000 đồng/khách.
sai_khu_vuc: khán giả bị xếp sang khu vực thấp hơn quyền lợi đã cam kết: hoàn 500.000 đồng/khách cho mỗi hiệp bị ảnh hưởng.
nhan_vien: khiếu nại về thái độ nhân viên: tặng voucher 300.000 đồng/khách.
khac: chuyển bộ phận Chăm sóc khán giả xem xét, chưa đề xuất mức đền bù.
Tổng mức đền bù trên 1.500.000 đồng phải được Trưởng ban Chăm sóc khán giả duyệt trước khi gửi khách.


</chinh_sach>



[Extra] buộc đầu ra là file JSON và nội dung phải đúng theo chính sách 
```

### b) Hai điều cần kiểm tra ở JSON đầu ra của Trạm 1

1. xem đã đúng định dạng JSON chưa

2. xem đã đủ 4 trường (bao gồm: "loai_su_co", "so_khach", "so_hiep_bi_anh_huong", "tom_tat") hay chưa

### c) Câu lệnh viết tiếp

**CAU LENH**

```text
câu lệnh trên đang dừng ở "Ban tổ chức Cúp Sao Vàng xin hoàn lại Quý khán giả khoản" bạn hãy tiếp tục câu lệnh trên mà không lặp lại phần đã có
```
