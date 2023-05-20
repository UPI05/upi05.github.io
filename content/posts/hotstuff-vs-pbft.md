+++
title = "So sánh HotStuff BFT với Practical BFT"
date = 2023-05-12T14:28:00
tags = [
"hotstuff_bft",
"pbft",
"bft",
"blockchain"
]
thumbnail= "thumbnail/thumbnail6.jpg"
[params]
  readMore = true
+++



Từ khi PBFT (Practical Byzantine Fault Tolerance) ra đời, đã có nhiều giải pháp được đề xuất nhằm cải tiến PBFT như BFT-SMaRt, Zyzzyva, SBFT, ... trong đó có cả HotStuff BFT. Hotstuff BFT ra đời nhằm mục đích tăng khả năng mở rộng (Scalability) của hệ thống BFT. Với những ưu điểm của mình, nó đã được tận dụng bởi dự án Libra của Facebook và gọi là LibraBFT. Sau khi dự án Libra đổi tên thành Diem, LibraBFT cũng được gọi thành DiemBFT. Đến tháng 3/2022, dự án Diem được bán lại cho Silvergate nhưng vì DiemBFT là dự án mã nguồn mở nên được Aptos tận dụng để tạo ra AptosBFT.

(Trong bài trước mình có trình bày về PBFT, mọi người có thể tìm xem lại)

Điểm khác biệt của HotStuff BFT so với Practical BFT:

- HotStuff sử dụng kiến trúc mạng hình sao (Star network) để tăng thời gian đáp ứng. Mạng hình sao là mô hình mạng mà các node sẽ kết nối trực tiếp đến central hub (cụ thể ở đây là leader). Như vậy, khi một node mới lên làm leader thông qua pha NEW-VIEW, các node sẽ tự động kết nối trực tiếp đến leader.
- Trong khi PBFT sử dụng giao thức view-change nhằm thay đổi primary (leader) chỉ khi nào node primary bị lỗi, HotStuff thay đổi leader liên tục khi có request mới. Mặc dù điều này có thể làm tăng thời gian của mỗi view nhưng lại khá tối ưu khi so với view-change của PBFT vì giao thức view-change ở PBFT tốn khá nhiều chi phí thực hiện (thời gian).
- HotStuff sử dụng kĩ thuật "threshold signatures" để giảm số lượng signatures trong mạng (Mình sẽ tìm hiểu và trình bày trong bài sau). Đại khái là HotStuff không yêu cầu signatures của tất cả các node mà chỉ cần signatures đại diện của các edge nodes.
- HotStuff sử dụng kĩ thuật pinelined các pha nhằm tăng tốc độ xử lý.
- Ngoài ra, trong khi PBFT thực hiện gửi tất cả mọi thứ một lần và đợi phản hồi để thực hiện commit, HotStuff thực hiện commit thông qua 3 pha là pre-commit, commit và decide.

Mọi người có tìm hiểu về HotStuff BFT thì cùng thảo luận để làm rõ sự khác nhau nhé.

Tham khảo:

Yin, M., Malkhi, D., Reiter, M.K., Gueta, G.G., Abraham, I.: Hotstuff: Bft consen-
sus with linearity and responsiveness. In: Proceedings of the 2019 ACM Symposium
on Principles of Distributed Computing. pp. 347–356 (2019)

https://hackernoon.com/hotstuff-the-consensus-protocol-behind-safestake-and-facebooks-librabft
https://medium.com/ontologynetwork/hotstuff-the-consensus-protocol-behind-facebooks-librabft-a5503680b151