+++
title = "Ngăn chặn tấn công Man-in-the-middle khi triển khai hệ thống trao đổi khóa Elliptic Curve Diffie-Hellman"
date = 2023-05-17T15:43:00
tags = [
"elliptic_curve",
"ecdh",
"elliptic curve diffie-hellman",
"cryptography",
"blockchain",
"man-in-the-middle",
"mitm",
"hacking"
]
thumbnail= "thumbnail/thumbnail8.jpg"
[params]
  readMore = true
+++



Chào mọi người, trong phần trước mình có trình bày về giao thức trao đổi khóa Diffie-Hellman trên đường cong Elliptic (ECDH). Tuy nhiên, hệ thống ECDH đơn giản có thể bị tấn công Man-in-the-middle (MITM) như sau:

> Vẫn là ví dụ như lần trước, Alice và Bob thỏa thuận khóa nhưng lần này sẽ có thêm Mark đứng giữa thực hiện giả mạo để tấn công MITM. Đầu tiên, Mark chọn private key nM và tạo public key của mình là Qm = nM * G. Khi bắt được public key từ Alice gửi đến Bob, Mark tiến hành giả mạo Bob và gửi public key Qm của mình cho Alice, đồng thời tiến hành giả mạo Alice để gửi public key Qm của mình cho Bob. Khi đó, Bob nhận được Qm sẽ gửi lại public key của mình là Qb cho Mark. Như vậy, Mark đứng giữa đã thực hiện tạo khóa phiên dùng chung với Alice là nA * Qm = nM * Qa = nA * nM * G, và với Bob là nM * Qb = nB * Qm = nM * nB * G. Do đó, mọi thông tin Alice và Bob gửi cho nhau đều bị Bob bắt được và giải mã.

Việc dễ dàng bị tấn công MITM là do hệ thống không thể xác thực các bên tham gia. Để giải quyết vấn đề này, thông thường sẽ có một số tổ chức (Certificate Authorities) đứng ra cấp phát chứng chỉ như giao thức SSL/TLS. Chứng chỉ SSL/TLS bao gồm một số thông tin sau:

- Tên miền của server được chứng thực
- Thông tin về tổ chức chứng thực
- Thời hạn hiệu lực
- Khóa công khai public key của server được chứng thực.
- Chữ ký số: Chứng chỉ SSL/TLS sẽ chứa chữ ký số được tạo ra bằng cách sử dụng khóa riêng của tổ chức chứng thực để xác thực tính xác thực của chứng chỉ.

Như vậy, Alice và Bob cần kiểm tra chứng chỉ để so khớp domain name với public key trước khi chấp nhận public key của người kia.
Tóm lại, chứng chỉ ssl/tls dùng để đảm bảo public key ứng với một domain cụ thể. Do đó, để trao đổi thông tin an toàn cần làm các công việc sau:
- Các bên tham gia đăng kí chứng chỉ SSL/TLS
- Khi các bên tương tác với nhau cần kiểm tra chứng chỉ để đảm bảo không ai bị giả mạo
- Sử dụng ECDH để tạo khóa phiên
- Sau khi có khóa phiên thì dùng một số loại mã hóa đối xứng an toàn như AES để mã hóa thông tin cần trao đổi trong phiên đó.