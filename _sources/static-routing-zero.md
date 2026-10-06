---
title: "Định tuyến tĩnh từ đầu: static route, summary route, default route và lab Packet Tracer"
topic: "static-routing"
depth: zero
scope: all
source: "Chapter04_Routing.ppt (Mạng Máy Tính Nâng Cao, HK1 26-27); Route Summarization — NetworkLessons.com; ip route Command Explained — ComputerNetworkingNotes"
questions: 14
bloom-level: 4
language: vi
created: 2026-10-06
version: 1
generator: foundation-zero-qa
---

# Định tuyến tĩnh từ đầu: static route, summary route, default route và lab Packet Tracer

## Bối cảnh: vì sao cần một bảng chỉ đường

Một gói tin (packet) rời máy tính của bạn với địa chỉ đích nằm ở một thành phố khác. Không có ai "biết" đường đi cả — mỗi thiết bị trung gian chỉ trả lời được một câu hỏi duy nhất: *gói này nên đi ra cửa nào tiếp theo?*

- **Bộ định tuyến (router)** là thiết bị chịu trách nhiệm chính về việc này: xác định đường đi tốt nhất và chuyển tiếp gói tin về phía đích [1].
- Router làm việc đó dựa trên một bảng gọi là **bảng định tuyến (routing table)**, lưu trong RAM [1].
- Nếu bảng rỗng hoặc thiếu dòng cho mạng đích, gói tin bị bỏ. Vì vậy toàn bộ bài toán định tuyến thực chất là bài toán: *làm sao để bảng định tuyến có đủ dòng đúng?*

Có ba cách để bảng định tuyến có dòng: mạng kết nối trực tiếp, tuyến tĩnh, và tuyến động. Bài viết này đi từ nền tảng đó tới ba kỹ thuật cụ thể — **tuyến tĩnh (static route)**, **tuyến tổng hợp (summary route)**, **tuyến mặc định (default route)** — rồi kết thúc bằng phần cấu hình thực hành trên Cisco Packet Tracer.

## Nền tảng: router đọc gì trong một dòng bảng định tuyến

Trước khi cấu hình, cần đọc được nội dung bảng định tuyến. Xem bằng lệnh `show ip route` [1].

Mỗi dòng trong bảng mô tả một mạng đích, gồm bốn thông tin [1]:

| Thành phần | Ý nghĩa |
|---|---|
| Nguồn thông tin định tuyến | Tuyến này do đâu mà có (code `C`, `S`, hay chữ cái giao thức) |
| Địa chỉ mạng và mặt nạ mạng con (subnet mask) | Mạng đích là mạng nào |
| Địa chỉ IP của next-hop router | Gói phải giao cho router kế tiếp nào |
| Exit interface | Gói phải đi ra cổng nào của router này |

Hai khái niệm cần chốt ngay:

- **Next-hop router** (router kế tiếp): router mà router hiện tại sẽ giao gói cho, để nó đi tiếp [1].
- **Exit interface** (cổng ra): cổng vật lý trên router hiện tại mà gói được đẩy ra [1].

Địa chỉ IP của router kế tiếp và cổng ra là hai "cách chỉ đường" khác nhau — và sự khác biệt này quyết định phần lớn nội dung cấu hình tuyến tĩnh về sau.

## Ba nguồn gốc của một dòng bảng định tuyến

Bảng định tuyến có thể nhận dòng từ ba nguồn [1]:

**1. Mạng kết nối trực tiếp (directly-connected network)** — code `C`

Khi một cổng của router được bật lên ("up"), mạng của cổng đó tự động được thêm vào bảng định tuyến như một mạng kết nối trực tiếp [1].

- Điều kiện bật: router phải được cấu hình địa chỉ IP cho cổng, và cổng phải được mở bằng lệnh `no shutdown` [1].
- Hệ quả quan trọng: **mạng kết nối trực tiếp là nền móng**. Bảng định tuyến phải có sẵn các mạng kết nối trực tiếp dùng để nối tới mạng ở xa, thì tuyến tĩnh hoặc tuyến động mới dùng được [1].

**2. Tuyến tĩnh (static route)** — code `S`

Do quản trị viên nhập tay bằng lệnh `ip route` [1]. Đây là chủ đề chính của bài.

**3. Tuyến động (dynamic route)**

Do các giao thức định tuyến tự trao đổi giữa các router. Các giao thức IP phổ biến: RIP, IGRP, EIGRP, OSPF, IS-IS, BGP [1].

- Giao thức định tuyến là "ngôn ngữ" để router chia sẻ thông tin với nhau, từ đó xây và duy trì bảng định tuyến [1].
- Hai việc nó làm: **khám phá mạng** và **cập nhật/duy trì bảng định tuyến** [1]. Nhờ đó router tự bù đắp khi cấu trúc mạng thay đổi mà không cần quản trị viên can thiệp [1].

## Tuyến tĩnh — khi nào dùng và cơ chế hoạt động

### Khi nào chọn tuyến tĩnh

Tuyến tĩnh phù hợp khi bảng định tuyến nhỏ và ít biến động [1]:

- Mạng chỉ có vài router.
- Mạng chỉ nối Internet qua một ISP duy nhất.
- Mô hình **hub-and-spoke** (trục và nan hoa) — một router trung tâm nối nhiều router nhánh — dùng trên mạng lớn.

✦ Suy luận: ba tình huống này có chung một đặc điểm — số đường đi cần biết ít và không đổi. Tuyến động đắt giá ở chỗ nó phải chạy liên tục để tự cập nhật; nếu đường đi vốn đã cố định thì khoản chi phí đó không mua được gì.

### Cơ chế: tuyến tĩnh hoạt động qua ba phần

Tuyến tĩnh vận hành theo ba bước [1]:

1. Quản trị viên cấu hình tuyến bằng lệnh `ip route`.
2. Router cài tuyến đó vào bảng định tuyến.
3. Gói tin được định tuyến theo tuyến tĩnh đó.

Nói cách khác, tuyến tĩnh không tự xuất hiện — nó là một lời khai do con người đưa vào, và router tin lời khai đó cho tới khi bị sửa.

### Chỉ đường bằng exit interface: lợi và hại

Tuyến tĩnh có thể cấu hình kèm exit interface. Khi đó bảng định tuyến phân giải được cổng ra **chỉ bằng một lần tra cứu thay vì hai** [1].

✦ Diễn giải: nếu chỉ có next-hop, router phải tra bảng định tuyến để biết cổng ra, rồi tra tiếp một bảng khác để biết địa chỉ. Có sẵn exit interface thì bỏ được lần tra thứ hai.

Nhưng trên mạng Ethernet, cách này có một cái bẫy — phần sau sẽ nói rõ.

## Tuyến tổng hợp — gộp nhiều mạng thành một dòng

### Vấn đề nó giải quyết

Mỗi tuyến tĩnh là một dòng cấu hình và một dòng bảng định tuyến. Nếu một router nhánh cần biết 8 mạng con liền kề, bạn sẽ phải gõ 8 tuyến. **Tuyến tổng hợp (summary route)** — còn gọi là **gom tuyến (route aggregation)** — gộp nhiều prefix nhỏ thành một prefix rộng hơn bao trùm tất cả [2].

- Prefix (tiền tố): phần chung của một khối địa chỉ, viết dạng `/n`.
- Tuyến tổng hợp làm bảng định tuyến nhỏ hơn và giảm lưu lượng cập nhật định tuyến [2].

### Luật để gộp

Gộp được một tập mạng khi chúng **liền kề (contiguous)** và tạo thành một khối có kích thước là lũy thừa của 2 [2]. Cách tính [2]:

1. Đổi các địa chỉ mạng sang nhị phân.
2. Đếm số bit đầu **giống nhau** ở tất cả các mạng — đó là độ dài prefix của tuyến tổng hợp.
3. Lấy phần bit chung, đặt toàn bộ bit còn lại bằng 0 — đó là địa chỉ tổng hợp.

**Mẹo nhẩm (dạng thập phân):** độ dài tuyến tổng hợp nhân lên theo số mạng gộp: một mạng `/24` là khối 256 địa chỉ, hai mạng là `/23`, bốn mạng là `/22`, tám mạng là `/21` [2].

**Công thức subnet mask:** mask của octet bị gộp = 256 − (số mạng được gộp) [2]. Ví dụ gộp 4 mạng trong cùng một octet: 256 − 4 = 252 → mask `255.255.252.0`.

### Ví dụ tính đầy đủ

Gộp bốn mạng sau [2]:

- 192.168.0.0/24
- 192.168.1.0/24
- 192.168.2.0/24
- 192.168.3.0/24

Bước 1 — viết octet thứ ba dạng nhị phân [2]:

| Địa chỉ | Octet 3 (nhị phân) |
|---|---|
| 192.168.0.0 | 0000 0000 |
| 192.168.1.0 | 0000 0001 |
| 192.168.2.0 | 0000 0010 |
| 192.168.3.0 | 0000 0011 |

Bước 2 — đếm bit chung: octet 1 và 2 giống nhau hoàn toàn (8 + 8 = 16 bit); octet 3 giống nhau 6 bit đầu → cộng 6. Tổng = **22 bit** [2].

Bước 3 — kết quả: tuyến tổng hợp **192.168.0.0/22**, mask **255.255.252.0** [2]. Kiểm tra bằng công thức mask: 4 mạng → 256 − 4 = 252 ✓ [2].

### Cái giá của gộp tuyến

✦ Suy luận: nếu tập mạng không tạo thành khối lũy thừa 2 đúng biên, tuyến tổng hợp buộc phải **rộng hơn** tập mạng thật, tức nó bao luôn cả những mạng bạn không có [2]. Router sẽ tưởng những mạng đó nằm phía sau nó.

- Hệ quả: chỉ nên gộp khi các mạng thật sự liền kề và căn đúng biên. Nếu không, tuyến tổng hợp có thể kéo gói tin đi sai hướng.
- Đây cùng một cơ chế với tuyến mặc định — chỉ khác mức độ, phần sau sẽ rõ.

## Tuyến mặc định — trường hợp cực đoan của gộp tuyến

**Tuyến mặc định (default static route)** là tuyến khớp với **mọi** gói tin [1].

### Khi nào dùng

Dùng trong hai tình huống [1]:

- Khi không còn tuyến nào khác trong bảng định tuyến khớp địa chỉ đích — tức không tồn tại khớp cụ thể hơn. Trường hợp thường gặp: nối router biên của công ty với mạng ISP.
- Khi router chỉ có đúng **một router khác** mà nó kết nối tới. Tình trạng này gọi là **stub router** (router cụt).

Liên quan: **stub network** (mạng cụt) là mạng chỉ được truy cập qua **một tuyến duy nhất** [1]. Tuyến tĩnh thường được dùng khi định tuyến từ một mạng tới stub network [1].

### Cú pháp

Tuyến mặc định thực chất là một tuyến tĩnh đặc biệt, dùng đúng format sau [1]:

```text
ip route 0.0.0.0 0.0.0.0 [ next-hop-address | outgoing interface ]
```

✦ Diễn giải: `0.0.0.0 0.0.0.0` là prefix `/0` — không bit nào bắt buộc phải khớp, nên mọi địa chỉ đều lọt. Đây là tuyến tổng hợp mức cao nhất có thể: gộp **toàn bộ** không gian IPv4 thành một dòng [2].

## Bẫy trên mạng Ethernet: next-hop so với exit interface

Đây là phần dễ sai nhất khi cấu hình tuyến tĩnh.

Trên một link Ethernet, nếu tuyến tĩnh đẩy gói tới next-hop router [1]:

- Địa chỉ MAC đích của khung dữ liệu (frame) sẽ là địa chỉ của cổng Ethernet phía next-hop.
- Router tìm địa chỉ này bằng cách tra **bảng ARP** (Address Resolution Protocol — giao thức phân giải địa chỉ).
- Nếu bảng ARP chưa có dòng, router sẽ phát một **ARP request** (yêu cầu phân giải địa chỉ) ra mạng.

Nhưng với **Ethernet làm exit interface**, vấn đề khác hẳn [1]:

- Trên mạng Ethernet có thể có rất nhiều thiết bị cùng chia sẻ một **multi-access network** (mạng đa truy cập — mạng mà nhiều thiết bị cùng gắn vào).
- Vì nhiều thiết bị cùng nằm ở đó, router **không biết địa chỉ IP của next-hop** và do đó **không xác định được địa chỉ MAC đích** cho khung Ethernet.

✦ Heuristic: trên link **điểm-điểm (point-to-point)** — mỗi cổng chỉ có một mạng — dùng exit interface hoặc next-hop đều được. Trên link **multi-access**, luôn chỉ định địa chỉ IP của next-hop router [3].

- Lý do: link point-to-point chỉ có đúng một "hàng xóm", nên "đi ra cổng đó" là chỉ đích danh. Multi-access có nhiều hàng xóm, nên phải nói rõ giao cho ai.

## Cấu hình thực hành trên Cisco Packet Tracer

Phần này dựng lại đúng topology và cách đánh địa chỉ trong bài lab của chương, để bạn cấu hình được ngay trên Packet Tracer.

### Bối cảnh lab

Mô hình tối thiểu cần ba router và một chuỗi mạng `172.16.x.0/24` nối tiếp nhau:

- `172.16.1.0/24` — mạng LAN phía R1 (nơi có PC1).
- `172.16.2.0/24` — link nối R1 với R2.
- `172.16.3.0/24` — mạng phía sau R2 (nơi có PC2).

✦ Ví dụ minh họa: đây là lược đồ tối giản, không phải topology nguyên bản trong slide. Slide dùng cùng dải `172.16.x.0` với R1, R2 và các cổng serial `0/0/x`.

### Bước 1 — Cấu hình cổng, mở cổng

Mặc định **mọi cổng serial và Ethernet đều ở trạng thái down** [1]. Phải mở bằng `no shutdown` [1].

Cấu hình cổng Ethernet [1]:

```text
R1(config)# interface fastEthernet 0/0
R1(config-if)# ip address 172.16.1.1 255.255.255.0
R1(config-if)# no shutdown
```

Cấu hình cổng serial [1]:

```text
R1(config)# interface serial 0/0
R1(config-if)# ip address 172.16.2.1 255.255.255.0
R1(config-if)# no shutdown
```

### Bước 2 — Đặt xung nhịp cho phía DCE

Một kết nối WAN có hai phía [1]:

- **DCE (Data Circuit-terminating Equipment)** — thiết bị phía nhà cung cấp dịch vụ. CSU/DSU là một thiết bị DCE.
- **DTE (Data Terminal Equipment)** — thường là router.

Trong môi trường lab, **một phía của kết nối serial phải được coi là DCE**, và phía đó cần tín hiệu xung nhịp [1]. Cổng serial cần tín hiệu clock để điều khiển nhịp truyền thông [1]:

```text
R1(config)# interface serial 0/0
R1(config-if)# clockrate 64000
```

✦ Heuristic: trong Packet Tracer, đầu dây nào có biểu tượng đồng hồ nhỏ là đầu DCE — đặt `clockrate` ở đó. Nếu quên, cổng serial sẽ không lên `up`.

Một tiện ích khi làm việc trên cổng console: vào chế độ cấu hình line của cổng console và thêm `logging synchronous` để tách output tự động khỏi phần bạn đang gõ [1].

### Bước 3 — Cấu hình tuyến tĩnh

Tuyến tĩnh theo **next-hop**:

```text
R1(config)# ip route 172.16.3.0 255.255.255.0 172.16.2.2
```

Tuyến tĩnh theo **exit interface**:

```text
R1(config)# ip route 172.16.3.0 255.255.255.0 serial 0/0
```

Chọn dạng nào:

- Trên link **point-to-point** (serial) — dùng dạng nào cũng chạy; dạng exit interface giúp tra cứu nhanh hơn.
- Trên link **multi-access** (Ethernet) — bắt buộc dùng **next-hop** [3].

✦ Suy luận: hai lệnh trên cho cùng một đích đến, nhưng khác cách router "ghi nhớ". Dạng exit interface ghi thẳng cổng ra nên tra một lần; dạng next-hop ghi địa chỉ hàng xóm nên router phải tra thêm một lần nữa để tìm cổng. Trên Ethernet, dạng exit interface còn không dùng được vì lý do ở mục trước.

### Bước 4 — Cấu hình tuyến tổng hợp

Giả sử R1 cần biết bốn mạng con liền kề `172.16.4.0/24`, `172.16.5.0/24`, `172.16.6.0/24`, `172.16.7.0/24`. Thay vì bốn tuyến, viết một tuyến tổng hợp:

```text
R1(config)# ip route 172.16.4.0 255.255.252.0 172.16.2.2
```

Kiểm tra bằng công thức: 4 mạng → mask octet thứ ba = 256 − 4 = 252 ✓ [2].

### Bước 5 — Cấu hình tuyến mặc định

Trên router biên — router chỉ có một đường ra ngoài — thay vì liệt kê mọi mạng, dùng một dòng [1]:

```text
R1(config)# ip route 0.0.0.0 0.0.0.0 172.16.2.2
```

Hoặc chỉ định cổng ra:

```text
R1(config)# ip route 0.0.0.0 0.0.0.0 serial 0/0
```

### Bước 6 — Xác minh

Bảng lệnh xác minh [1]:

| Lệnh | Xem gì |
|---|---|
| `show ip route` | Bảng định tuyến |
| `show ip interface brief` | Tình trạng cổng, dạng rút gọn |
| `show interfaces` | Toàn bộ cấu hình cổng |
| `show running-config` | Cấu hình đang chạy trong RAM |
| `show startup-config` | Cấu hình lưu trong NVRAM |
| `show cdp neighbors detail` | Thông tin cấu hình của hàng xóm kết nối trực tiếp |

Để quan sát router cài và gỡ tuyến theo thời gian thực, dùng [1]:

```text
R1# debug ip routing
```

Lệnh này cho thấy mọi thay đổi router thực hiện khi **thêm hoặc xoá** tuyến [1].

Điểm cần kiểm tra ngay sau khi cấu hình cổng: khi router mới chỉ có cổng được cấu hình và bảng định tuyến chỉ chứa các mạng kết nối trực tiếp, **chỉ những thiết bị trên các mạng kết nối trực tiếp đó là tới được** [1]. Đây là trạng thái chuẩn trước khi thêm tuyến tĩnh — nếu ping tới mạng xa đã chạy được ở bước này thì có gì đó bất thường.

### Bước 7 — Sửa tuyến tĩnh

Một tuyến tĩnh đã cấu hình có thể cần sửa trong hai trường hợp [1]:

- Mạng đích không còn tồn tại → phải xoá tuyến.
- Cấu trúc mạng thay đổi → phải đổi địa chỉ trung gian hoặc cổng ra.

Cách sửa là xoá rồi nhập lại [1]:

```text
R2(config)# no ip route 172.16.3.0 255.255.255.0 serial0/0/1
R2(config)# ip route 172.16.3.0 255.255.255.0 serial0/0/0
```

✦ Heuristic: đây là lý do nên viết lệnh cũ ra nháp trước khi xoá — `no ip route` phải khớp **chính xác** dòng đã nhập, sai một tham số là lệnh không gỡ được tuyến.

## Gỡ lỗi khi thiếu tuyến

Khi ping thất bại, đừng đoán — đi theo thứ tự [1].

**Bộ công cụ** [1]:

- `ping` — kiểm tra kết nối đầu-cuối.
- `traceroute` — phát hiện toàn bộ các chặng (router) trên đường đi giữa hai điểm.
- `show ip route` — hiển thị bảng định tuyến và xác định quá trình chuyển tiếp.
- `show ip interface brief` — hiển thị tình trạng cổng router.
- `show cdp neighbors detail` — thu thập thông tin cấu hình của hàng xóm kết nối trực tiếp.

**Thứ tự thao tác** [1]:

1. Bắt đầu bằng `ping`. Nếu ping thất bại, chuyển sang `traceroute` để xác định gói tin tắc ở đâu.
2. Chạy `show ip route` để kiểm tra bảng định tuyến.
3. Nếu phát hiện tuyến tĩnh cấu hình sai, **xoá tuyến cũ rồi cấu hình lại tuyến mới** [1].

✦ Suy luận: thứ tự này đi từ triệu chứng tới nguyên nhân. `ping` trả lời "có hỏng không", `traceroute` thu hẹp "hỏng ở đâu", `show ip route` trả lời "vì sao" — còn việc xoá và nhập lại là cách sửa vì router không có lệnh "sửa tại chỗ" cho tuyến tĩnh.

## Khi nào dùng cái nào

| Tình huống | Chọn |
|---|---|
| Mạng vài router, đường đi cố định | Tuyến tĩnh [1] |
| Nối Internet qua một ISP duy nhất | Tuyến mặc định về phía ISP [1] |
| Router biên, chỉ có một router hàng xóm (stub router) | Tuyến mặc định [1] |
| Mạng chỉ truy cập qua một tuyến (stub network) | Tuyến tĩnh [1] |
| Router nhánh cần biết nhiều mạng con liền kề, căn đúng biên | Tuyến tổng hợp [2] |
| Cấu trúc mạng lớn, nhiều đường đi, hay thay đổi | Tuyến động [1] |
| Link multi-access (Ethernet) | Tuyến tĩnh chỉ định **next-hop** [3] |
| Link point-to-point (serial) | Next-hop hoặc exit interface đều được; exit interface tra nhanh hơn [1][3] |

## Phạm vi và giới hạn

- Tuyến tĩnh không tự thích ứng khi cấu trúc mạng đổi — không có gì bù đắp thay bạn [1].
- Tuyến tổng hợp chỉ đúng khi tập mạng liền kề và căn biên lũy thừa 2; nếu không, nó bao luôn mạng không tồn tại [2].
- Tuyến mặc định khớp mọi đích, nên chỉ nên đặt ở router biên hoặc stub router. Đặt ở router trung tâm có thể kéo gói tin đi vòng.
- `show cdp neighbors detail` chỉ thấy được hàng xóm kết nối trực tiếp [1].
- ✦ Suy luận: nếu bảng định tuyến không có mạng kết nối trực tiếp làm nền, mọi tuyến tĩnh trỏ tới next-hop đều vô nghĩa — router không biết đi tới chính cái next-hop đó bằng cách nào.

## Luyện tập

1. Cấu hình hai router nối nhau bằng serial trong Packet Tracer, đặt `clockrate 64000` ở một phía. Kiểm tra bằng `show ip interface brief` xem cổng serial đã `up` chưa. Nếu chưa, tìm nguyên nhân.
2. Viết tuyến tĩnh tới mạng sau router kia bằng cả hai dạng (next-hop và exit interface). So sánh dòng tương ứng trong `show ip route`.
3. Gộp bốn mạng `10.0.4.0/24` → `10.0.7.0/24` thành một tuyến tổng hợp. Tính prefix và mask bằng cả cách nhị phân lẫn cách thập phân, rồi đối chiếu.
4. Cố tình nhập sai exit interface trong một tuyến tĩnh. Đi theo đúng thứ tự gỡ lỗi (`ping` → `traceroute` → `show ip route`) để tìm ra, rồi sửa bằng cách xoá và nhập lại.
5. Bật `debug ip routing`, lần lượt xoá rồi thêm một tuyến tĩnh, và đọc các dòng router in ra.

## Tài liệu tham khảo

[1] "Chương 4: Routing — Introduction to Routing and Packet Forwarding; Static Routing," *Mạng Máy Tính Nâng Cao*, HK1 2026–2027, tài liệu bài giảng (Chapter04_Routing.ppt).

[2] NetworkLessons.com, "Route Summarization," *CCNA Routing & Switching*. [Online]. Available: https://networklessons.com/cisco/ccna-routing-switching-icnd1-100-105/route-summarization

[3] ComputerNetworkingNotes, "ip route Command Explained with Examples," *CCNA Study Guide*. [Online]. Available: https://www.computernetworkingnotes.com/ccna-study-guide/ip-route-command-explained-with-examples.html