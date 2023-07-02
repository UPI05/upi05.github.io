+++
title = "Mã hóa đối xứng - Phần 1: Feistel và DES"
date = 2023-07-02T10:00:00
tags = [
"DES",
"cryptography",
"feistel"
]
thumbnail= "thumbnail/thumbnail9.png"
[params]
  readMore = true
+++



# Feistel
Feistel là một cấu trúc mã hóa khối được thiết kế bởi Horst Feistel và Don Coppersmith vào năm 1973. Feistel chia plaintext thành các khối dữ liệu sau đó mã hóa trên từng khối, thực hiện thông qua n rounds, gồm các bước thay thế và chuyển vị trên dữ liệu đầu vào.

![Feistel](https://images.viblo.asia/1abd7931-8202-4caa-a5da-9bccf8e55063.jpg)


Các bước cụ thể như sau:
* Quá trình mã hóa nhận vào một khối dữ liệu plaintext và một master key.
* Chia khối đó thành hai phần bằng nhau, nửa trái kí hiệu L0, nửa phải kí hiệu R0.
* Lặp qua n rounds (i:1->n) để mã hóa, các round sử dụng chung hàm mã hóa F nhưng khác sub-key Ki (được sinh từ master key). Thực hiện các bước sau đây:
* L\[i+1\]= Ri
* R\[i+1\]= Li  ⊕  F(Ri, Ki)
* Sau n rounds, thực hiện swap Ln, Rn và gộp lại để thu giá trị mã hóa: RnLn.


Để giải mã, ta thực hiện ngược lại quá trình trên.


**Một số lưu ý cho Feistel:**
- Kích thước block càng lớn càng an toàn tuy nhiên có thể ảnh hưởng đến tốc độ tính toán. Tương tự với kích thước Key.
- Sử dụng nhiều round giúp tăng khả năng bảo mật.
- Hàm sinh sub-key từ master key có độ phức tạp càng cao thì càng khó bị phân tích mã. Tương tự với hàm F.




# Data Encryption Standard (DES):
DES là chuẩn mã hóa dữ liệu do cơ quan An ninh Quốc gia Hoa Kỳ (NSA) đề xuất cuối những năm 1970. DES sử dụng cấu trúc mã hóa Feistel với 16 rounds, kích thước mỗi khối là 64 bits, kích thước khóa là 56 bits. Tương tự Feistel, DES sử dụng một hàm F chung trong khi mỗi round lại sử dụng một sub-key riêng (được sinh từ master key).

Đầu tiên, khối plaintext (64 bits) được đưa qua bước Initial permutation để thực hiện hoán vị các bit data (Quy tắc hoán vị được định nghĩa trong một bảng gọi là Inital permutaion table). Sau đó, cho khối chạy 16 rounds để mã hóa, các bước chạy giống như Feistel đã trình bày ở trên. Cuối cùng, thực hiện bước Final permutation để thu ciphertext (bước này thực chất là đảo ngược của quá trình Initial permutation). 

![DES](https://images.viblo.asia/3a7f16af-670b-40f0-9003-800fb92e526b.jpg)


**Sinh khóa cho các round:**

![DES key generation](https://images.viblo.asia/6c4325f6-d8f1-406c-a151-00e7f5d0af99.png)

Khóa đầu vào thực chất có tới 64 bits nhưng bị loại bỏ các bit ở vị trí 8*i (8, 16, 24, …) nên còn lại 56 bits.
Đầu tiên, 64-bit key sẽ được đưa qua bước Permuted choice 1, thực hiện bỏ đi các bit ở vị trí 8*i và hoán vị theo quy tắc định trước (Theo bảng Permuted table 1), thu được 56-bit key. Tách 56-bit key thành 2 phần bằng nhau (28 bits), nửa bên trái là C0, nửa bên phải là D0. Tiếp theo, lặp 16 lần để tạo 16 subkeys, mỗi lần thực hiện dịch trái xoay vòng Ci, Di từ 1 đến 2 bits (Số bit cần dịch cụ thể cho mỗi round xem ảnh đính kèm). Sau khi dịch, thực hiện tiếp bước Permuted choice 2 để thu được subkey cho round i (Quy tắc hoán vị Permuted choice 2 cũng được định nghĩa trước trong bảng PC2 table). Permuted choice 2 cho ra 48-bit key.

![](https://images.viblo.asia/d706b5dd-21e9-491b-9159-c497052eec4e.png)


**Hàm Feistel F:**

![Feistel function](https://images.viblo.asia/09e2ae93-aa72-4362-ab56-032afc895075.png)


Hàm F trong DES được định nghĩa như sau:

F(Ki, R\[i-1\]) = P(S(E(R\[i-1\]) ⊕ (Ki)))

Trong đó, E là hàm mở rộng, dùng để biến đổi 32-bit dữ liệu đầu vào thành 48 bits (bằng kích thước khóa).
Hàm S là các hộp S-box thực hiện thay thế đầu vào.
Hàm P là hộp P-box thực hiện hoán vị đầu vào.


**Hàm mở rộng E:**

Hàm E mở rộng 32-bit key thành 48-bit key bằng cách cho phép lặp lại một số bit trong input ban đầu. Quy tắc mở rộng được định nghĩa trong một bảng gọi là E-Bit Selection table.

**Các hộp S-box:**
Sau khi thực hiện xor E(R\[i-1\]) với Ki, ta thu được 48-bit key, chia thành 8 nhóm, mỗi nhóm 6 bits. Bước này có nhiệm vụ thay thế mỗi nhóm 6 bits bằng 1 nhóm 4 bits, cho đầu ra giảm xuống 32-bit key. Để thực hiện, người ta định nghĩa 8 bảng gọi là S-box tables là quy ước biến đổi từ input (6 bits) thành output (4 bits). Cụ thể một bảng S-box như sau: Với 6-bit input, lấy bit đầu và cuối ghép lại, tạo thành 1 số từ 0 đến 3 (decimal), 4 bits giữa tạo thành một số từ 0 đến 15. Như vậy, người ta tạo một bảng gồm 4 hàng và 16 cột, mỗi hàng chứa các giá trị hoán vị từ 0 đến 15. Mỗi một giá trị 6-bit input sẽ mapping với 1 ô trong bảng (output 4 bits vì giá trị từ 0 đến 15).

**Hộp P-box:**
Hộp P-box là một cách hoán vị 8 nhóm 4 bits đầu ra của các S-box, sử dụng một table gọi là P-box table định nghĩa quy ước vị trí hoán vị.


Để giải mã, ta làm ngược lại quy trình trên.

**Kết luận:**
DES là tiêu chuẩn đầu tiên được NSA đề xuất nhưng hiện nay đã không còn sử dụng vì độ dài khóa nhỏ (56 bits) dễ bị tấn công brute force. Mặc dù có một số phương pháp cái tiến như Triple DES nhưng cũng không còn được sử dụng. Trong phần tiếp theo, mình sẽ trình bày về Advanced Encryption Standard (AES) - một tiêu chuẩn hiện được sử dụng rộng rãi.



Tham khảo: 

https://page.math.tu-berlin.de/~kant/teaching/hess/krypto-ws2006/des.htm

https://en.wikipedia.org/wiki/Data_Encryption_Standard

https://www.geeksforgeeks.org/data-encryption-standard-des-set-1/

https://viblo.asia/p/encryption-des-Qpmleq27lrd

https://www.educative.io/answers/what-is-the-feistel-cipher-structure

https://whitehat.vn/threads/kien-truc-ma-hoa-khoi-block-cipher-p2.12349/

#vbi_discuss #des #blockchain