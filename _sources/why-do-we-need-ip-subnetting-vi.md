---
title: "Vì sao chúng ta cần chia mạng con IP (IP Subnetting)?"
aliases:
  - "IP Subnetting"
tags:
  - doc/translated
  - domain/other
source: "Why do we need IP Subnetting? (tài liệu người dùng cung cấp)"
domain: "Networking / DevOps"
doc-type: translated
language: vi
created: 2026-10-05
translator: doc-translator
_width: wide
---
# Vì sao chúng ta cần chia mạng con IP (IP Subnetting)?

Trong bài học này, chúng ta bắt đầu tìm hiểu về địa chỉ IPv4 và việc chia mạng con (subnetting). Chúng ta sẽ thấy vì sao cần chia các mạng IP có lớp (classful) thành các mạng con (subnet).

## Địa chỉ IP là gì?

> An IP address is an Internet Protocol address used as an endpoint identifier in IP-based communications.

Địa chỉ IP (Internet Protocol address) là địa chỉ giao thức Internet, được dùng làm định danh cho điểm đầu cuối (endpoint) trong các giao tiếp dựa trên IP.

Nói đơn giản, địa chỉ IP giống như số điện thoại. Khi muốn gọi cho một người khác bằng điện thoại di động, bạn dùng số điện thoại của người đó; người nhận cuộc gọi sẽ thấy số của bạn. Về bản chất, cuộc gọi diễn ra giữa các số điện thoại với nhau. Tương tự, khi một thiết bị muốn giao tiếp bằng IP, nó gửi dữ liệu đến địa chỉ IP của thiết bị ở đầu bên kia. Mỗi chiếc điện thoại có một số điện thoại, và mỗi thiết bị cũng cần có một địa chỉ IP để gửi và nhận dữ liệu qua mạng IP.

Một điểm then chốt: theo chuẩn, địa chỉ IP dài 32 bit (4 byte), nghĩa là chỉ có 2³² (4.294.967.296) địa chỉ IP duy nhất (và chúng đã được xem là cạn kiệt chính thức vào tháng 4 năm 2017). Chỉ có vậy thôi; không thể tạo thêm địa chỉ mới. Không gian địa chỉ IPv4 là tài nguyên hữu hạn, và chúng ta cần sử dụng từng địa chỉ IP một cách khôn ngoan (giống như quỹ đất của hành tinh chúng ta).

![IPv4 address space is a finite resource](https://cdn.networkacademy.io/sites/default/files/2022-04/ipv4-space-is-a-finite-resource.svg)

*Hình 1. Không gian địa chỉ IPv4 là tài nguyên hữu hạn*

Như vậy, mọi thiết bị trên thế giới đều cần một địa chỉ IP, trong khi không gian địa chỉ IPv4 là hữu hạn và chỉ có 2³² địa chỉ.

## Mạng IP là gì?

Quay lại phép so sánh với điện thoại để hiểu mạng IP (IP network) là gì. Khi gọi một số điện thoại bàn trong cùng khu vực, bạn bấm số đó trực tiếp. Nhưng khi gọi một số bàn ở khu vực khác, bạn không bấm số trực tiếp mà phải quay mã vùng trước.

> An IP network is a group of IP addresses that share the same broadcast domain and don't need a router to communicate.

Mạng IP (IP network) là một nhóm địa chỉ IP cùng chung một miền quảng bá (broadcast domain) và không cần bộ định tuyến (router) để giao tiếp với nhau, tương tự việc chúng ta bấm trực tiếp số điện thoại trong cùng khu vực.

Trong một mạng IP, mỗi thiết bị có thể phân giải địa chỉ MAC (Media Access Control) của từng địa chỉ IP thông qua giao thức phân giải địa chỉ (Address Resolution Protocol – ARP), rồi giao tiếp trực tiếp ở tầng Ethernet. Lý do là mọi địa chỉ IP đều cùng miền quảng bá, nên đều nhận được bản sao các gói tin quảng bá (broadcast) của nhau.

![What is an IP network?](attachments/1791179926966-what-is-an-ip-network.gif)

*Hình 2. Mạng IP là gì?*

Ở hình 2, host 1, 2 và 3 thuộc cùng mạng IP-A và dùng chung một phân đoạn Ethernet (Ethernet segment). Khi host 1 gửi các gói tin quảng bá, chẳng hạn yêu cầu ARP, mọi host khác (2, 3 và router) đều nghe được và có thể phản hồi. Hành vi này được gọi là flood-and-learn (tràn gói tin rồi học địa chỉ). Host 1 "tràn" gói tin ra toàn bộ miền quảng bá, nói rằng: "Tôi đang tìm host 3". Mỗi host trong mạng con (subnet) đều nhận được gói quảng bá này, kể cả host 3; host 3 trả lời lại, và hai host bắt đầu giao tiếp trực tiếp với nhau.

Tuy nhiên, khi một thiết bị muốn giao tiếp với một địa chỉ IP thuộc mạng khác, nó phải gửi gói tin đến router (cổng mặc định – default gateway). Router chuyển tiếp gói tin dựa trên bảng định tuyến (routing table) của mình, hướng về địa chỉ IP đích. Router chấm dứt kiểu giao tiếp flood-and-learn và đưa vào cơ chế chọn đường dựa trên các giao thức định tuyến (routing protocol).

Chúng ta có thể tóm tắt những gì đã trình bày bằng ba quy tắc thiết yếu về mạng IP mà người làm mạng nào cũng cần luôn ghi nhớ:

- Quy tắc #1. Một subnet = một miền quảng bá (broadcast domain) = một VLAN (Virtual Local Area Network – mạng LAN ảo)
- Quy tắc #2. Các địa chỉ IP trong cùng một subnet giao tiếp trực tiếp qua chuyển mạch Ethernet (Ethernet switching) và không bị router ngăn cách
- Quy tắc #3. Các địa chỉ IP thuộc các subnet khác nhau bị ngăn cách bởi một hoặc nhiều router và giao tiếp với nhau thông qua định tuyến IP (IP routing)

## Vì sao cần nhiều mạng IP?

Đến đây, có thể bạn đang thắc mắc: "Sao không gán địa chỉ IP cho mọi thiết bị trên thế giới, gom hết vào một mạng IP khổng lồ cho xong? Tại sao phải có subnet và mặt nạ mạng con (subnet mask)?"

Hình 3 cho thấy điều gì xảy ra nếu có một mạng IP khổng lồ trải rộng khắp hành tinh. Giả sử PC 1 muốn giao tiếp với PC 2, và hai máy này nằm rất gần nhau. Mọi host khác trong mạng IP vẫn sẽ nhận được bản sao của các gói quảng bá mà PC 1 gửi đi. Về cơ bản, mỗi gói quảng bá sẽ được tất cả các host trên toàn thế giới nghe thấy. Ngay cả khi hai host sát cạnh nhau giao tiếp, gói quảng bá của chúng vẫn phải đi sang tận bên kia hành tinh.

![One Giant IP Network](attachments/1791179936487-one-giant-ip-network.gif)

*Hình 3. Một mạng IP khổng lồ*

Hãy tưởng tượng bạn đang ở trong một nhóm vài người và nói: "Tôi đang tìm Bob". Bob nhiều khả năng sẽ nghe thấy và đáp lại. Nhưng nếu bạn ở trong một sân vận động đầy người và ai cũng hét lên "Tôi đang tìm người X", rất có thể không ai giao tiếp được vì môi trường truyền tin bị quá tải. Trong trường hợp đó, chúng ta nhờ một nhân viên trật tự sân vận động: "Tôi đang tìm Bob, anh ấy ở tầng 2, khu 4B, ghế 16". Nhân viên này sẽ chuyển lời nhắn tới Bob thông qua các nhân viên khác mà không để cả sân vận động phải nghe. Đây chính xác là việc router làm: chuyển tiếp thông điệp giữa các mạng IP bằng đường đi ngắn nhất, hiệu quả nhất tới đích.

Rõ ràng, kiểu giao tiếp flood-and-learn này kém hiệu quả và không khả thi ở quy mô lớn, khoảng cách xa. Đó là lý do miền quảng bá chỉ là các phân đoạn Ethernet có ít hơn 255 host, nằm trong phạm vi vài trăm mét.

## Mạng IP có lớp (Classful IP Networks)

Thuở đầu của kỷ nguyên mạng máy tính, toàn bộ không gian địa chỉ IPv4 được chia thành 256 mạng. Mọi địa chỉ IP có 8 bit đầu giống nhau được coi là thuộc cùng một mạng IP. Ví dụ, 144.1.1.1 và 144.56.78.123 thuộc cùng mạng 144.0.0.0/8; 1.1.1.1 và 1.255.255.254 cũng thuộc cùng một mạng, và cứ thế. Tuy nhiên, với sự phát triển nhanh chóng của Internet vào đầu thập niên 1980, rõ ràng việc chia toàn bộ không gian IPv4 thành chỉ 256 mạng là không đủ. Khi đó, cơ quan quản lý Internet đã quyết định chia không gian địa chỉ IP hiệu quả hơn và đưa ra mô hình địa chỉ có lớp (classful address model).

Mô hình địa chỉ có lớp chia không gian địa chỉ IP (từ 0.0.0.0 đến 255.255.255.255) thành năm lớp riêng biệt: A, B, C, D và E, như trong bảng dưới đây. Mô hình mới này quy định: với địa chỉ lớp A, mặt nạ mạng (network mask) là /8 (255.0.0.0), nghĩa là mọi địa chỉ IP có 8 bit đầu giống nhau thuộc cùng một mạng. Với địa chỉ lớp B, chỉ những IP có 16 bit đầu giống nhau mới thuộc cùng một mạng. Còn với lớp C, chỉ những địa chỉ có 24 bit đầu giống nhau mới thuộc cùng một mạng IP. Chẳng hạn, giờ đây địa chỉ 200.1.1.1 và 200.1.2.1 thuộc hai mạng IP lớp C khác nhau.

*Mạng IP có lớp (Classful IP Networks)*

| Lớp | Mặt nạ mạng | Số lượng mạng IP | Số địa chỉ IP mỗi mạng | Dải địa chỉ |
| --- | --- | --- | --- | --- |
| A | 255.0.0.0 (/8) | 128 | 16.777.216 | 0.0.0.0 – 127.255.255.255 |
| B | 255.255.0.0 (/16) | 16.384 | 65.536 | 128.0.0.0 – 191.255.255.255 |
| C | 255.255.255.0 (/24) | 2.097.152 | 256 | 192.0.0.0 – 223.255.255.255 |
| D | Multicast (đa hướng) |  |  | 224.0.0.0 – 239.255.255.255 |
| E | Dự trữ (không sử dụng) |  |  | 240.0.0.0 – 255.255.255.255 |

Với thời điểm đó, cách đánh địa chỉ có lớp này đã chia không gian IPv4 hiệu quả hơn nhiều.

## Địa chỉ có lớp hoạt động như thế nào

Như đã nói, khi một thiết bị giao tiếp với một địa chỉ IP từ xa thuộc cùng mạng IP, nó dùng kỹ thuật flood-and-learn với ARP. Ngược lại, khi muốn giao tiếp với địa chỉ IP nằm ngoài mạng IP của mình, nó gửi gói tin đến router trên phân đoạn đó (cổng mặc định). Vì vậy, khi một thiết bị muốn giao tiếp bằng IP, nó phải biết địa chỉ IP của thiết bị từ xa có cùng mạng IP với mình hay không. Đó chính là lúc mặt nạ mạng (network mask) xuất hiện.

> The network mask tells the device which part of the IP address is identifying the network and which is identifying a particular host on that network.

Mặt nạ mạng cho thiết bị biết phần nào của địa chỉ IP dùng để nhận diện mạng và phần nào dùng để nhận diện một host cụ thể trên mạng đó. Các số 255 trong mặt nạ mạng xác định phần mạng (network portion) của địa chỉ, còn các số 0 xác định phần host (host portion), như thể hiện trong hình 4 bên dưới.

![Classful IP addresses Network Mask](https://cdn.networkacademy.io/sites/default/files/2022-04/classful-ip-addresses-network-mask.svg)

*Hình 4. Mặt nạ mạng của địa chỉ IP có lớp*

Hãy xem mặt nạ mạng hoạt động ra sao qua một vài ví dụ.

### Ví dụ lớp A

Mặt nạ mạng trong không gian địa chỉ lớp A được cố định là 255.0.0.0. Điều này có nghĩa octet đầu tiên của địa chỉ IP nhận diện mạng, còn ba octet còn lại nhận diện host cụ thể, như hình 5 bên dưới.

![Class A Netmask Example](https://cdn.networkacademy.io/sites/default/files/2022-04/ipv4-subnetting-class-a-example.svg)

*Hình 5. Ví dụ mặt nạ mạng lớp A*

Khi một thiết bị được cấp địa chỉ IP 10.4.21.43 với mặt nạ 255.0.0.0, nó lập tức hiểu rằng mọi địa chỉ bắt đầu bằng octet đầu tiên là 10. đều thuộc cùng một mạng IP (do đó cùng miền quảng bá, cùng VLAN). Ví dụ, khi thiết bị muốn giao tiếp với 10.122.45.155, nó biết IP này cùng mạng và có thể gửi ARP trực tiếp để hỏi địa chỉ MAC. Ngược lại, nếu thiết bị muốn giao tiếp với 13.1.2.3, nó gửi gói tin tới cổng mặc định đã cấu hình (nếu có).

### Ví dụ lớp B

Mặt nạ mạng trong không gian địa chỉ lớp B được cố định là 255.255.0.0. Điều này có nghĩa hai octet đầu của địa chỉ IP nhận diện mạng, hai octet còn lại nhận diện host cụ thể, như hình 6.

![Class B Netmask Example](https://cdn.networkacademy.io/sites/default/files/2022-04/ipv4-subnetting-class-b-example.svg)

*Hình 6. Ví dụ mặt nạ mạng lớp B*

Khi một host được cấp địa chỉ IP 144.1.32.45 với mặt nạ 255.255.0.0, nó lập tức hiểu rằng mọi IP bắt đầu bằng hai octet đầu là 144.1. đều thuộc cùng một mạng IP (do đó cùng miền quảng bá, cùng VLAN). Ví dụ, khi host muốn giao tiếp với 144.1.255.243, nó biết IP này cùng mạng và có thể gửi ARP trực tiếp để hỏi địa chỉ MAC.

### Ví dụ lớp C

Khi một host được cấp địa chỉ IP 192.168.1.45 với mặt nạ 255.255.255.0, nó lập tức hiểu rằng mọi IP bắt đầu bằng ba octet đầu là 192.168.1. đều thuộc cùng một mạng IP (do đó cùng miền quảng bá, cùng VLAN). Ví dụ, khi host muốn giao tiếp với 192.168.1.243, nó biết IP này cùng mạng và có thể gửi ARP trực tiếp để hỏi địa chỉ MAC.

![Class C Netmask Example](https://cdn.networkacademy.io/sites/default/files/2022-04/subnetting-classc-example.svg)

*Hình 7. Ví dụ mặt nạ mạng lớp C*

Khi một host được cấp địa chỉ IP 192.168.1.45 với mặt nạ 255.255.255.0, nó lập tức hiểu rằng mọi IP bắt đầu bằng ba octet đầu là 192.168.1. đều thuộc cùng một mạng IP (do đó cùng miền quảng bá, cùng VLAN). Ví dụ, khi host muốn giao tiếp với 192.168.1.243, nó biết IP này cùng mạng và có thể gửi ARP trực tiếp để hỏi địa chỉ MAC.

## Địa chỉ có lớp vẫn chưa đủ

Thời gian trôi qua, người ta lại nhận ra rằng cần sử dụng không gian địa chỉ IPv4 hiệu quả hơn nhiều so với những gì mô hình có lớp cho phép. Ví dụ, một công ty bán lẻ có năm mươi cửa hàng nhỏ ở các thành phố khác nhau trên khắp nước Mỹ. Mỗi cửa hàng chỉ có mười host mạng. Công ty sẽ cần ít nhất 100 mạng IP khác nhau: một mạng cho mỗi cửa hàng và ít nhất một mạng cho đường truyền WAN (Wide Area Network – mạng diện rộng) tới cửa hàng. Như vậy, với tổng cộng 50 cửa hàng × 10 host (500 IP), công ty cần hơn 100 mạng lớp C, mỗi mạng 256 IP; trong kịch bản tốt nhất, điều này tiêu tốn 25.600 địa chỉ IP. Rõ ràng một lượng khổng lồ địa chỉ IP đã bị lãng phí vào những khối quá lớn, được cấp cho nhu cầu của chỉ vài host.

## Giới thiệu chia mạng con và địa chỉ không phân lớp (Classless Addressing)

Vào một thời điểm trong thập niên 1990, người ta nhận ra rằng kích thước của các mạng IP không nhất thiết phải bị cố định theo mặt nạ mạng của từng lớp. Ý tưởng này đã tạo ra một kỹ thuật gọi là mặt nạ mạng con có độ dài thay đổi (Variable-Length Subnet Masking – VLSM). VLSM cho phép các tổ chức chia mạng có lớp của mình thành những subnet nhỏ hơn, vừa khít nhất với nhu cầu. Ý tưởng này được minh họa trong hình 8 bên dưới.

![Subnetting Example](https://cdn.networkacademy.io/sites/default/files/2022-04/ipv4-subnetting-example.svg)

*Hình 8. Ví dụ chia mạng con*

Giả sử một công ty bán lẻ muốn mở ba cửa hàng mới ở các thành phố khác nhau. Cửa hàng ở New York cần 32 địa chỉ host, còn hai cửa hàng nhỏ hơn cần 16 IP mỗi cửa hàng. Công ty đã mua mạng lớp C 200.1.1.0/24. Ý tưởng chính của việc chia mạng con IP là tổ chức có thể dùng duy nhất mạng lớp C này hiệu quả nhất có thể mà không lãng phí khối IP lớn nào.

Như bạn thấy ở hình 8, mạng lớp C mà công ty sở hữu quá lớn so với nhu cầu của từng cửa hàng. Để dùng được, công ty cắt nó thành các khối địa chỉ nhỏ hơn, gọi là subnet, rồi gán các subnet đó cho những phần khác nhau của liên mạng (internetwork). Cách này hiệu quả hơn nhiều và ít lãng phí hơn so với việc chỉ dùng các khối có độ dài cố định.

## Chia mạng con không phân lớp hoạt động như thế nào?

Ở mức rất tổng quát, chia mạng con không phân lớp (classless subnetting) cho phép gán cho địa chỉ IP các mặt nạ mạng tùy ý, không phụ thuộc vào "lớp". Nghĩa là các mặt nạ /8 (255.0.0.0), /16 (255.255.0.0) và /24 (255.255.255.0) có thể được gán cho bất kỳ địa chỉ nào mà theo truyền thống thuộc dải lớp A, B hoặc C. Hơn nữa, chúng ta không còn bị ràng buộc với /8, /16 và /24 là các lựa chọn duy nhất, và đó là điểm khiến địa chỉ không phân lớp trở nên thú vị. Hình 9 minh họa một ví dụ mà mặt nạ mạng không bị cố định theo lớp địa chỉ.

![Examples of Classless Network Masks](https://cdn.networkacademy.io/sites/default/files/2022-04/ipv4-subnetting-classless-mask.svg)

*Hình 9. Ví dụ các mặt nạ mạng không phân lớp*

Xét địa chỉ IP 10.1.1.1. Với địa chỉ IP có lớp, đây là địa chỉ lớp A, nên mặt nạ mạng bị cố định là 255.0.0.0 (/8). Trong địa chỉ có lớp, bản thân địa chỉ IP ngụ ý mặt nạ mạng; không có lựa chọn nào khác.

Tuy nhiên, với địa chỉ không phân lớp, chỉ biết địa chỉ IP thì chưa suy ra được mặt nạ mạng. Bạn cần được cho biết rõ mặt nạ là gì. Ví dụ, địa chỉ IP 10.1.1.0 có thể mang mặt nạ mạng 255.255.255.0.

Chúng ta tạm dừng phần địa chỉ IP, chia mạng con và mặt nạ mạng ở đây, và tóm tắt những gì đã thấy cho đến giờ.

## Những điểm chính cần nhớ

- Không gian địa chỉ IPv4 là tài nguyên hữu hạn: chỉ tồn tại 4.294.967.296 địa chỉ.
- Mạng IP là một nhóm địa chỉ IP cùng miền quảng bá và không cần router để giao tiếp.
- Một subnet = miền quảng bá = một VLAN.
- Các địa chỉ IP trong cùng một subnet giao tiếp trực tiếp qua chuyển mạch Ethernet và không bị router ngăn cách.
- Các địa chỉ IP thuộc các subnet khác nhau bị ngăn cách bởi một hoặc nhiều router và giao tiếp với nhau thông qua định tuyến IP.
- Mô hình địa chỉ có lớp chia không gian địa chỉ IP (từ 0.0.0.0 đến 255.255.255.255) thành năm lớp riêng biệt: A, B, C, D và E. Chỉ ba lớp đầu có thể dùng cho host.

### Địa chỉ có lớp so với không phân lớp

| Địa chỉ có lớp (Classful) | Địa chỉ không phân lớp (Classless) |
| --- | --- |
| Phương pháp cấp phát địa chỉ IP, gán các khối địa chỉ theo năm lớp định sẵn (A, B, C, D, E). | Phương pháp cấp phát địa chỉ IP dùng các khối địa chỉ có độ dài thay đổi, không thuộc lớp nào. |
| Kém thực tiễn và kém hiệu quả hơn. | Thực tiễn và hiệu quả hơn. |
| Phần định danh mạng (Network ID) và phần định danh host (Host ID) thay đổi tùy theo lớp. | Không có ranh giới cố định giữa phần định danh mạng và phần định danh host. |
| Mặt nạ mạng luôn cố định theo lớp. Địa chỉ IP ngụ ý mặt nạ mạng. Ví dụ, địa chỉ lớp A như 10.1.1.2 luôn có mặt nạ 255.0.0.0 (/8). | Kích thước mạng IP không bị cố định theo mặt nạ của từng lớp. Địa chỉ IP nào cũng có thể mang mặt nạ mạng bất kỳ. Ví dụ, 10.1.1.2 có thể có mặt nạ 255.255.224.0 (/19). |
| Địa chỉ có lớp lãng phí một lượng khổng lồ địa chỉ IP vào những khối quá lớn cấp cho nhu cầu của vài host. | Địa chỉ không phân lớp cho phép chúng ta kiểm soát kích thước mạng IP theo nhu cầu. Đây chính là điều chúng ta gọi là chia mạng con (subnetting, tức chia nhỏ mạng). |
