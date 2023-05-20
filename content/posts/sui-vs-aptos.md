+++
title = "Blockchain layer 1 thế hệ mới, SUI hay APTOS?"
date = 2023-04-03T12:52:00
tags = [
"aptos",
"sui",
"blockchain"
]
thumbnail= "thumbnail/thumbnail1.jpg"
[params]
  readMore = true
+++



> Mặc dù blockchain đang ngày càng trở nên phổ biến trong cộng đồng, tuy nhiên vẫn còn một số rào cản khiến công nghệ này khó tiếp cận với phần đông mọi người. Trong đó phải kể đến như tốc độ xử lý giao dịch (TPS) còn chậm, phí giao dịch cao. Một số blockchain như Bitcoin, ... mặc dù tốc độ xử lý giao dịch thấp nhưng đảm bảo được tính bảo mật, nếu tăng tốc độ thì phải đánh đổi với khả năng bảo mật của hệ thống. Do đó rất khó để những hệ thống blockchain này có thể tạo ra đột phá mà đòi hỏi phải có những hệ thống blockchain thế hệ mới có khả năng đảm bảo về mặt security một cách mạnh mẽ, thời gian thực hiện giao dịch nhanh và chi phí thấp. Từ đó, những blockchain layer 1 như SUI và APTOS ra đời. Điểm chung của các hệ thống này là built-in smart contract blockchain. Tận dụng những kiến trúc, công nghệ mới nhằm xử lý các transaction một cách song song, giúp tăng tốc độ xử lý lên nhiều lần.

Cả hai blockchain đều có thông lượng cao, độ trễ thấp, được tách ra từ dự án Diem blockchain của Facebook. Tận dụng ngôn ngữ lập trình Move (được phát triển từ Rust) để quản lý asset, giúp tăng khả năng bảo mật của hệ thống. Ngoài ra, đội ngũ phát triển của hai dự án đều là những chuyên gia đầu ngành, từng làm việc ở các công ty công nghệ lớn như Google, Facebook, ...

APTOS bắt đầu từ tháng 12/2021, dự án nổi lên khi nhận được quỹ đầu tư 200 triệu đô vào tháng 3/2022. Đến tháng 7/2022, dự án tiếp tục nhận được số tiền đầu tư 150 triệu đô. Mainnet của APTOS chính thức hoạt động vào tháng 10/2022. Trong khi đó, SUI được phát triển bới Mysten Labs, nhận được khoảng đầu tư 300 triệu đô vào tháng 9/2022. Dự án hiện đang trong giai đoạn phát triển và dự định sẽ release mainnet vào quý 2/2023 sắp tới.

### CÁCH HỆ THỐNG XỬ LÝ TRANSACTION

APTOS sử dụng pipeline, chia quá trình thực hiện một giao dịch thành các phase riêng lẻ, độc lập với nhau. Thay vì gửi, nhận các transaction liên tục trong mạng đến các validator, hệ thống sẽ đợi gôm đủ số lượng transaction (1 batch) mới thực hiện gửi đi. Khác với Bitcoin, Etherium xử lý các transaction một các tuần tự, APTOS thực thi các transaction (là smart contract) một cách song song nhằm tăng tốc độ xử lý (TPS). Tuy nhiên, để đạt được điều này, các transaction đó không được conflict với nhau. APTOS giải quyết bằng cách sử dụng Block-STM để phát hiện và xử lý các conflict transaction, các transaction còn lại dễ dàng xử lý song song trên nhiều luồng (thread).

Hơi khác so với APTOS, validators trên SUI được gọi là authorities. Hệ thống chạy theo epochs, mỗi epoch hoạt động trong 24h, sau thời gian đó tập authorities sẽ thay đổi. Tập authorities mới sẽ vote để quyết định giá gas mới cho epoch đó. Tuy nhiên, hệ thống sẽ có những cơ chế khuyến khích để giá gas chênh lệch ít nhất có thể. Tương tự như APTOS, hệ thống sẽ cần xử lý hai loại transaction là dependent transactions và independent transactions. Với những independent transactions (chứa các owned object hoặc read-only object), hệ thống sẽ sử dụng Byzantine consistent broadcast để xử lý và với những dependent transactions (chứa các shared object) sẽ là giao thức đồng thuận dựa trên DAG để xử lý song song. Như vậy hệ thống sẽ sử dụng đồng thời hai cơ chế cho việc xử lý transaction.

### HIỆU SUẤT

Trong điều kiện phòng thí nghiệm, cả hai đều có hiệu suất rất tốt. Hơn 120000 TPS với SUI (trên Macbook Pro) và hơn 130000 TPS với APTOS (trên máy 32 cores). Tuy nhiên, ở thời điểm hiện tại APTOS mainnet giao động từ 8-10 transactions mỗi giây.