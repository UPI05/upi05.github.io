+++
title = "Đồng hồ Lamport"
date = 2023-05-08T06:39:00
tags = [
"lamport_clock",
"distributed_system",
"blockchain"
]
thumbnail= "thumbnail/thumbnail3.jpg"
[params]
  readMore = true
+++

Trong các hệ thống phân tán (distributed system), thứ tự của các sự kiện (gửi, nhận message) cần được ghi nhận. Tuy nhiên, việc sử dụng đồng hồ vật lý có nhiều hạn chế do độ trễ lan truyền nên thời điểm ghi nhận một message đến các node có thể khác nhau (không đảm bảo giống nhau 100%). Do đó, Lamport đề xuất sử dụng đồng hồ logic (logical clock) để đồng bộ thời gian của các sự kiện trong hệ thống phân tán.
Khái niệm đồng hồ logic không phải là đồng hồ thông thường mà là sử dụng các số nguyên không âm để đánh dấu mốc thời gian của các sự kiện. Thời gian của các sự kiện được tính dựa trên thời gian của các sự kiện trước đó. Ví dụ: Nếu sự kiện A xảy ra trước sự kiện B thì thời gian của A sẽ nhỏ hơn B. Các mốc thời gian được đánh dấu có thể rời rạc nhau.

> Bài viết xem xét các sự kiện xảy ra trên các process. Các sự kiện có thể là process Pi gửi message đến process Pj, process Pi đọc, ghi…. Mỗi process i sẽ có một đồng hồ Lamport Ci riêng để đánh dấu thời gian.

### Partial order:

Mối quan hệ “xảy ra trước” (kí hiệu →) giữa 2 sự kiện bất kì được định nghĩa thỏa mãn 3 điểu kiện sau:

> Nếu a và b là 2 sự kiện trong cùng 1 process và a xảy ra trước b thì a→b
> Nếu a là sự kiện gửi một message ở process A và b là sự kiện nhận message đó ở process B thì a→b
> Nếu a→b và b→c thì a→c (tính chất bắc cầu)

Hai sự kiện khác nhau a và b được gọi là xảy ra đồng thời nếu a không xảy ra trước b và b không xảy ra trước a.

Clock condition: Cho 2 sự kiện a, b: a→b thì C(a) < C(b)

Dễ dàng nhận thấy các điều kiện sau:

> CC1: Nếu a và b là 2 sự kiện trong cùng process i và a xảy ra trước b thì Ci(a) < Ci(b).
> CC2: Nếu a là sự kiện gửi một message ở process i và b là sự kiện nhận message đó ở process j thì Ci(a) < Cj(b).

**Giải thuật**:

Để đảm bảo hệ thống thỏa mãn Clock condition thì CC1 và CC2 phải thỏa. Để giải quyết CC1, process i chỉ cần tăng Ci lên 1 đơn vị khi có sự kiện xảy ra thành công. Với CC2, khi gửi message thì process i (sender) cần gửi thêm Tm là thời điểm process i gửi message đó. Khi đó, Cj (đồng hồ ở process receiver) sẽ cập nhật lại thành max(Cj, Tm) + 1.

![Lamport clock](../../images/lamport-clock.jpg)

Để minh họa, mọi người xem qua ảnh đính kèm: Sự kiện bắt đầu của P2 được đánh dấu là 1. Sự kiện tiếp theo, P2 gửi message sang P1 nên thời điểm gửi được tăng lên 2, thời điểm nhận là max(2, 1) + 1 = 3. Tương tự, khi P3 gửi message, thời điểm gửi sẽ tăng thành 2, local clock của P2 là 4 nên thời điểm nhận sẽ là max(2, 4) + 1 = 5.

### Total order:

Giải thuật ở trên chỉ sắp xếp một phần thứ tự của các sự kiện vì khi hai sự kiện xảy ra đồng thời, chúng cũng cần được sắp xếp. Lamport đề xuất gán số thứ tự (duy nhất) cho các process, khi đó nếu hai sự kiện xảy ra đồng thời thì sự kiện nào xảy ra trên process có số thứ tự nhỏ hơn sẽ được xếp trước. Mặc dù cách làm này đơn giản nhưng lại khó thực hiện trong thực tế, đặc biệt là các hệ thống lớn.

Cho sự kiện a ở process i, b ở process j:

> Nếu Ci(a) < Cj(b) thì a→b
> Ngược lại nếu Ci(a) = Cj(b) và i < j thì a→b

Đồng hồ Lamport hoạt động khá đơn giản nhưng là tiền đề cho giải thuật nâng cao hơn sau này là đồng hồ Vector (Vector Clock).

Tham khảo: Leslie Lamport. 1978. Time, clocks, and the ordering of events in a distributed system. Commun. ACM 21, 7 (July 1978), 558-565.