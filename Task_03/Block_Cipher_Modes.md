# Các chế độ hoạt động của mã hóa khối

## 1. Chế độ quyển mã điện tử(ECB):

- Mẫu tin được chia thành các khối độc lập, sau đó mã hóa từng khối
- Mỗi khối là giá trị cần thay thế như dùng sách mã, do đó nó có tên như vậy
- Mỗi khối được mã hóa độc lập với các mã khác Ci = EK (Pi)
- Khi dùng: truyền an toàn từng giá trị riêng lẻ

![image](image7.png)

![image](image8.png)

- Tính chất:
    + Các khối như nhau (dưới cùng một khóa) sẽ cho các khối mã giống nhau
    + Sự phụ thuộc móc xích: Các khối được mã hóa độc lập với các khối khác, việc sắp xếp thứ tự các khối mã cũng sẽ tương ứng với việc phải sắp xếp lại các khối rõ
    + Tính lan sai: Một hoặc nhiều bit sai trong một khối đơn lẻ chỉ ảnh hưởng tới chính việc giải mã khối đó
    + Khả năng xử lí song song: Có thể xử lí các khối song song.

![image](image9.jpg)
![image](image10.jpg) 

## 2. Chế độ liên kết khối mã (CBC)
- Các mẫu tin được chia thành các khối
- Nhưng chúng được liên kết với nhau trong quá trình mã hóa (giải mã)
- Các block được xếp thành dãy
- Sử dụng véc tơ ban đầu (IV) để bắt đầu quá trình mã hóa (giải mã)

$$C_{i}  = E_{K} (P_{i} \oplus C_{i - 1}), C_{0} = IV$$
$$P_{i} = D_{K}(C_{i}) \oplus C_{i - 1} , C_{0} = IV$$

-  Dùng khi: Mã hóa dữ liệu lớn, xác thực

![image](image11.png)
![image](image12.png)

- Tính chất:
    + Các bản rõ giống nhau: kết quả các khối mã sẽ như nhau khi dùng bản rõ được mã hóa dưới cùng một khóa và IV, thay đổi IV, khóa hoặc khối rõ đầu tiên thì bản mã kết quả sẽ khác nhau.
    + Sự phụ thuộc móc xích: cơ chế móc xích làm cho bản mã yj phụ thuộc vào $x_{j}$ và toàn bộ các khối rõ trước đó. Hệ quả là việc sắp xếp lại khối mã sẽ ảnh hưởng tới việc giải mã. Việc giải đúng một khối mã đòi hỏi phải giải mã đúng khối trước đó.
    + Tính lan sai: sai một bit trong khối mã $c_{j}$ sẽ ảnh hưởng tới việc việc giải mã các khối $y_{j}$ và $y_{j + 1}$ (từ chỗ  $x_{j}$ phụ thuộc vào $y_{j}$ và $y_{j - 1}$)
    + Khắc phục sai: chế độ CBC là kiểu tự đồng bộ theo nghĩa nếu một sai số (bao gồm cả việc mất một hoặc nhiều hơn các khối đầu vào) xuất hiện trong khối yj  nhưng không có ở trong khối $y_{j + 1}$ và $y_{j + 2}$ thì sẽ được giải mã chính xác tới khối $x_{j + 2}$ 

## 3. Chế độ phản hồi mã (CFB)
- Bản tin coi như các dòng bit
- Bổ sung vào đầu ra của mã khối
- Kết quả phản hồi trở lại cho giai đoạn tiếp theo
- Cho phép số bit phản hồi là 1,8,64, hoặc tùy ý: ký hiệu tương ứng là CFB1, CFB8, CFB64,…
- Được dùng cho mã hóa dữ liệu dòng, xác thực

$$C_{i} = E_{K}(C_{i-1}) \oplus P_{i} , C_{0}  = IV$$
$$P_{i} = E_{K}(C{i-1}) \oplus C_{i}$$

![image](image13.png)
![image](image14.png)

- Tính chất chế độ CFB
    + Các bản rõ giống nhau: cũng giống như chế độ CBC, sự thay đổi IV làm cho cùng một bản rõ đầu vào như nhau sẽ được mã hóa thành các bản mã khác nhau. Véc tơ IV không cần phải giữ bí mật(mặc dù trong ứng dụng thì nên dùng IV khó đoán được để an toàn)
    + Sự phụ thuộc móc xích: tương tự như chế độ CBC, cơ chế móc xích làm cho khối mã $y_{j}$ phụ thuộc vào cả $x_{j}$ và các khối rõ trước đó, hệ quả là việc thay đổi thứ tự các khối mã sẽ ảnh hưởng tới việc giải mã.
    + Tính lan sai: một hoặc nhiều hơn bit sai trong một khối mã đơn lẻ sẽ ảnh hưởng tới việc giải mã ngay tại đó và ảnh hưởng tới việc giải mã tới các khối tiếp theo. Attacker cũng có thể dự đoán sự thay đổi bit trong $x_{j}$ bằng cách thay đổi các bit tương ứng của $y_{j}$
    + Khắc phục sai: chế độ CFB tự đồng bộ tương tự như CBC, nhưng đòi hỏi phải có các khối mã để khắc phục.

## 4. Chế độ phản hồi đầu ra OFB
- Mẩu tin xem như dòng bit
- Đầu ra của mã được bổ sung cho mẫu tin
- Đầu ra do đó là phản hồi
- Phản hồi ngược là độc lập đối với bản tin
- Có thể được tính trước

$$C_{j} = P_{j} \oplus O_{j}$$

$$P_{j} = C_{j} \oplus O_{j}$$

$$O_{j} = E_{K}(I_{j}),I_{j} = O_{j-1},I_{0} = IV$$

![image](image15.png)
![image](image16.png)

- Tính chất chế độ OFB
    + Các bản rõ giống nhau: cũng giống như CBC, CFB sự thay đổi IV làm cho 1 bản rõ đầu vào như nhau sẽ được mã hóa thành các bản mã khác nhau
    + Sự phụ thuộc móc xích: Dòng khóa là độc lập với bản rõ
    + Tính lan sai: mội hay nhiều hơn những bit sai trong kí tự mã bất kì cj sẽ chỉ ảnh hưởng tới việc giải mã ngay tại kí tự đó, chính xác là ảnh hưởng tới vị trí bit sai làm cho bit bản rõ được giải ra sẽ là bit phần bù của bit rõ đúng.
    + Khắc phục sai: chế độ OFB khắc phục các bit mã sai , nhưng không thể tự đồng bộ sau khi đã mất các bit mã, chúng làm lệch đi dòng khóa để giải mã.

## 5. Chế độ CTR
- Một bộ đếm (Counter) bằng với khối văn bản gốc
- Mỗi khối nhận được 1 bộ đếm và 1 khóa để tạo khối đầu ra
- Đầu ra được XOR với khối bản rõ tương ứng để tạo bản mã

![image](image17.png)
![image](image18.png)

- Đánh giá CTR
    + Hiệu quả cao: 
        - Có thể mã hóa (giải mã) song song
        - Có thể thực hiện giải thuật mã hóa trước nếu cần
    +  Có thể xử lí bất kì khối nào trước các khối khác
    + Đơn giản, chỉ cần cài đặt thuật toán mã hóa, không cần đến giải thuật giải mã
    + Không sử dụng lại cùng giá trị khóa và biến đếm (tương tự OFB)
