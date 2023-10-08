+++
title = "Kĩ thuật Sharding (Tóm lược)"
date = 2023-10-08T11:50:00
tags = [
"blockchain",
"sharding"
]
thumbnail= "thumbnail/thumbnail11.PNG"
[params]
  readMore = true
+++


Giải pháp scale blockchain layer 1 thường chia làm 3 hướng chính:
* **Reducing overhead**: Giảm lượng dữ liệu gửi qua lại giữa các node và giảm các công việc tính toán trên dữ liệu đó.
* **Vertical scaling**: Liên quan đến việc thêm tài nguyên vào các node. Ví dụ: Tăng số lượng transaction cho mỗi block hoặc giảm thời gian sinh một block.
* **Horizontal scaling**: Sharding.

Sharding cơ bản là một kĩ thuật chia để trị, thực hiện chia một blockchain lớn thành nhiều shard nhỏ với mục đích tăng throughput của hệ thống (tăng số lượng giao dịch mỗi giây) thông qua việc cố gắng xử lý các giao dịch ở mỗi shard một cách song song với nhau. Như vậy, giả sử ta chia blockchain thành n shards thì sẽ có thể tăng tốc độ lên nhiều lần. Các thành phần trong sharding bao gồm các node, các giao dịch đang đợi xử lý và sổ cái (Ledger). Trong đó, mỗi shard có thể sử dụng các cơ chế đồng thuận khác nhau như PoS, PoW, BFT-based,...

![](https://images.viblo.asia/bd87c462-b664-4437-b2fe-885ea3da33cb.PNG)


Khi áp dụng sharding, có hai vấn đề chính cần chú ý là **Intra-consensus-safety** và **Cross-shard-atomicity**. Việc quyết định số lượng shard cần chia cũng là một vấn đề quan trọng vì số lượng shard càng nhiều thì sức mạnh tính toán cần để kiểm soát một shard càng ít và ngược lại thì khả năng scale sẽ không được tốt (đánh đổi giữa bảo mật và khả năng mở rộng).
# Intra-consensus-safety
Intra-consensus-safety liên quan đến việc xử lý các transaction chỉ nằm trong một shard. Khi đó, người ta cần tìm cách để tránh bị tấn công 1% (1% attack).

**Tấn công 1%**: Giả sử sức mạnh tính toán của cả hệ thống blockchain là **PT**, cần có hơn **50%** sức mạnh tính toán để kiểm soát hệ thống. Nếu sử dụng sharding, chia blockchain thành n shards, khi đó sức mạnh tính toán cần có để kiểm soát hệ thống là **P > PT/n\*(50%)**. Nói đơn giản hơn thì khi chia blockchain thành nhiều shard, người ta chỉ cần kiểm soát một shard thay vì cả hệ thống. Mà sức mạnh tính toán ở mỗi shard ít hơn nhiều so với cả hệ thống nên sẽ dễ bị tấn công hơn.

Thông thường để giảm thiểu tấn công 1%, người ta sẽ sử dụng các phương pháp chia ngẫu nhiên các node vào các shard khác nhau vào thời điểm bắt đầu block mới. Ngoài ra, việc lựa chọn các leader, … cũng dựa trên phương pháp chọn ngẫu nhiên. Ví dụ như giao thức RandHound trên Omniledger.
# Cross-shard-atomicity
Cross-shard transaction là các transaction có nguồn và đích không thuộc cùng một shard. Do đó, việc xử lý các transaction này thường phức tạp và tốn thời gian hơn. Hơn nữa, các transaction này chiếm tỉ lệ rất lớn nên cần có phương pháp xử lý tối ưu. Cross-shard-atomicity là một yêu cầu khi xử lý: Nếu transaction transaction xử lý bị lỗi thì tiền phải được hoàn về tài khoản gửi, không được xảy ra double-spending. Để giải quyết vấn đề này, người ta thường sử dụng cơ chế Locking/Unlocking.

![](https://images.viblo.asia/192c3771-58f6-47d4-a7b0-1f8414e99d90.png)


**Cơ chế Locking/Unlocking**: Đầu tiên, client sẽ gửi cross-shard transaction đến các input shards để thu thập bằng chứng chứng thực (tiền đủ không, đã Lock chưa, ...) từ các input shards. Khi đó, các input shards sẽ thực hiện khóa (Lock) tiền của client, đồng thời gửi các bằng chứng. Tiếp đến, nếu tất cả bằng chứng đều hợp lệ (Accepted) thì client sẽ gửi đến output shard để xử lý, thực hiện việc Unlock tiền để chuyển đến tài khoản đích. Ngược lại, nếu bằng chứng không hợp lệ (Rejected) thì thực hiện việc Unlock tiền để trả về cho client, đảm bảo tính Atomicity.

Cơ chế Locking/Unlocking hoạt động trên các blockchain như Omniledger, RapidChain, ChainSpace, ....

Ngoài Locking/Unlocking, còn có một số cơ chế như Eventual Atomicity hoặc Junking, .... Trong đó, Eventual Atomicity được sử dụng trong blockchain Monoxide cùng với cơ chế minning Chu-ko-nu.
Một số blockchain dựa trên sharding

**Academic**: Elastico (BFT), Omniledger (BFT), Rapidchain(BFT), Monochain (Nakamoto)​, ChainSpace (BFT)

**Industrial**: Etherium 2.0, Zilliqa, Harmony, Near protocol


Tham Khảo: G. Yu, X. Wang, K. Yu, W. Ni, J. A. Zhang and R. P. Liu, "Survey: Sharding in Blockchains," in IEEE Access, vol. 8, pp. 14155-14181, 2020, doi: 10.1109/ACCESS.2020.2965147.

#vbi_discuss #sharding #blockchain
