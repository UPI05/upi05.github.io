+++
title = "Synchrony, Asynchrony và Partial Synchrony"
date = 2023-05-11T19:44:00
tags = [
"distributed_system",
"network",
"blockchain"
]
thumbnail= "thumbnail/thumbnail5.jpg"
[params]
  readMore = true
+++


Trong hệ thống phân tán, người ta thường phân mạng (network) thành 3 loại:

- Synchronous network (mạng đồng bộ): Thời gian delay tối đa của một gói tin trong mạng phải là một giá trị Δ hữu hạn (xác định cụ thể).
- Asynchronous network (mạng bất đồng bộ): Thời gian delay tối đa của một gói tin là hữu hạn nhưng không được xác định cụ thể. Gói tin phải được chuyển đến người nhận.
- Partial synchrony (mạng bán đồng bộ): Mang tính chất trung gian giữa mạng đồng bộ và bất đồng bộ. Giả sử tồn tại một giá trị hữu hạn Δ và một sự kiện gọi là GST (Global Stabilization Time) sao cho:
    - Sau một thời gian delay nhất định (hữu hạn) thì sự kiện GST phải xảy ra.
    - Bất kỳ gói tin nào được gửi tại thời điểm x phải được chuyển đến người nhận trước thời điểm Δ+max(x,GST).

Như vậy, trong mạng bán đồng bộ (partial synchrony), hệ thống sẽ hoạt động như mạng bất đồng bộ (asynchrony) trước sự kiện GST và hoạt động như mạng đồng bộ sau sự kiện GST. Sự kiện GST có thể được delay trong một khoảng thời gian hữu hạn bất kỳ nên không có cách nào để phát hiện được sự kiện GST đã xảy ra chưa.
Mặc dù mạng đồng bộ có vẻ đơn giản nhưng lại gặp một số vấn đề sau: Nếu giá trị Δ lớn (vd: 10 phút) thì hệ thống sẽ phải đợi timeout rất lâu trong khi các gói tin đã gửi đến đích. Nếu giá trị Δ nhỏ (vd 0.1 giây) thì lại không an toàn cho hệ thống vì có thể còn nhiều gói tin chưa đến đích. Ngay cả khi chọn được giá trị Δ rất tốt thì vẫn có nguy cơ mất an toàn trong thực tế. Vì vậy, mạng bất đồng bộ là một giải pháp có vẻ tốt hơn vì nó không phụ thuộc vào giá trị Δ để xác định biên cho độ trễ. Tuy nhiên, "FLP impossibility" nói rằng không có giao thức đồng thuận nào có thể chống lỗi Byzantine trong mạng bất đồng bộ ngay cả khi chỉ có một node bị lỗi. Khái niệm mạng bán đồng bộ được đề xuất vào năm 1988 bởi Dwork, Lynch, and Stockmeyer. Dựa trên khái niệm này, nhiều giao thức đồng thuận BFT ra đời như PBFT, Zyzzyva, .... PBFT đảm bảo tính chất safety thông qua mạng bất đồng bộ trong khi tính chất liveness lại được đảm bảo qua mạng đồng bộ.

Cũng theo mình tìm hiểu, một số giao thức như PoW, PoS, ... có thể hoạt động được trên mạng bất đồng bộ (như internet) (mặc dù có FLP impossibility) là vì PoW, PoS không hoàn toàn đảm bảo BFT (Byzantine Fault Tolerance). Ngoài ra, trên mạng bất đồng bộ, người ta vẫn có thể tạo ra các cơ chế đồng thuận BFT bằng cách sử dụng "các kĩ thuật ngẫu nhiên".

Nguồn: https://decentralizedthoughts.github.io/2019-06-01-2019.../