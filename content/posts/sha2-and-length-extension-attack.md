+++
title = "SHA-2 and Length extension attack"
date = 2023-07-04T01:11:00
tags = [
"sha-2",
"sha-3",
"sha-256",
"cryptography",
"MAC",
"Merkle_Damgard",
"Length_extension_attack",
"hacking"
]
thumbnail= "thumbnail/thumbnail10.jpg"
[params]
  readMore = true
+++


# SHA-2
SHA-2 (Secure Hash Algorithm 2) là hàm băm mật mã được thiết kế bởi NSA (Cơ quan An ninh Quốc gia Hoa Kỳ) và công bố năm 2001. Nó được xây dựng dựa trên kiến trúc Merkle-Damgard và hàm nén một chiều. Họ SHA-2 gồm các hàm băm SHA-224, SHA-256, SHA-384, SHA-512, SHA-512/224, SHA-512/256.

![Merkle Damgard](https://images.viblo.asia/c28a3cb9-4b42-4e5d-a235-c827d4d7add2.jpg)


Để thực hiện băm, plaintext ban đầu sẽ được chia thành các khối và thêm padding. Bắt đầu từ initial vector (iv), hàm f nhận một khối đầu vào và iv, tiến hành nén cho ra output. Lại tiếp tục lấy output đó kết hợp với khối tiếp theo làm input cho hàm f. Lần lượt duyệt qua tất cả các khối và thu được giá trị hash cuối cùng. Ngoài ra, giá trị hash có thể đưa qua hàm Finalization để nén tiếp trước khi cho ra kết quả cuối cùng, giúp giảm kích thước.

# Length extension attack

MAC (Message Authentication Code) là gì?
MAC hay còn được gọi là tag là thông tin giúp xác thực một message, MAC còn giúp đảm bảo cả về tính toàn vẹn của dữ liệu bằng cách cho phép những ai có khóa có thể phát hiện những thay đổi của message. VD: Alice và Bob đã thỏa thuận xong khóa phiên ssk và thực hiện trao đổi thông tin (không yêu cầu confidentiality), Alice gửi message m và tag=SHA-256(ssk||m) cho Bob. Bob nhận được m, tiến hành băm tag’=SHA-256(ssk||m) và kiểm tra tag’ có bằng tag hay không, nếu bằng thì chứng tỏ m đúng là giá trị mà Alice đã gửi. Với || là phép nối 2 chuỗi.

Giả sử Mark (không phải Bob) nhận được m và tag của Alice thì có thể nối thêm một chuỗi M vào sau và tính được giá trị SHA-256(ssk||m||pad||M) mà không cần biết ssk và m như sau (nhưng yêu cầu biết độ dài |ssk| + |m|): Từ m tiến hành padding sao cho độ dài là bội của 64 (với SHA-256), tức là cần thêm 64-((|ssk| +|m|) % 64) bytes padding. Sau đó nối tiếp với M và thực hiện padding tiếp, thu được ssk||m||pad||M||pad’. Do SHA-256 là một hàm băm sử dụng kiến trúc Merkel-Damgard (pineline), ta không cần biết ssk, m mà từ giá trị tag, ta xem như đã thực hiện băm được k khối (ứng với message ssk||m||pad và output của f =tag) và tiếp tục khối tiếp theo với input thứ hai là tag. Kết quả thu được m’ và tag’ hợp lệ khi gửi cho Bob kiểm tra.

# Kết luận

Ngoài SHA-256, SHA-512 còn có MD5, SHA1 đều sử dụng kiến trúc Merkle-Damgard nên có thể bị khai thác. Để tránh bị Length extension attack, chúng ta có thể tìm cách che giấu độ dài của key và message, sử dụng SHA-3 hoặc HMAC. Trong đó, SHA-3 sử dụng kiến trúc Sponge với 2 lớp Absorbing và Squeezing giúp tránh được Leng extension attack.

Tham khảo: 

https://www.geeksforgeeks.org/merkle-damgard-scheme-in-cryptography/

https://en.wikipedia.org/wiki/Merkle%E2%80%93Damg%C3%A5rd_construction#

https://en.wikipedia.org/wiki/Length_extension_attack

https://en.wikipedia.org/wiki/SHA-2

#vbi_discuss #merkel_damgard #cryptography #length_extension_attack #SHA-2 #SHA-3 #MAC #blockchain


