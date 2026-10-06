---
title: "Định tuyến tĩnh từ đầu: static route, summary route, default route và lab Packet Tracer"
topic: "static-routing"
depth: zero
scope: all
source: "Chapter04_Routing.ppt (Mạng Máy Tính Nâng Cao, HK1 26-27); Route Summarization — NetworkLessons.com; ip route Command Explained — ComputerNetworkingNotes; Hop (networking) — Wikipedia; Stub network — Wikipedia"
questions: 16
bloom-level: 4
language: vi
created: 2026-10-06
version: 1
generator: foundation-zero-qa
_width: wide
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
| --- | --- |
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

## Các thuật ngữ thường gặp trong định tuyến tĩnh

Bảng tổng hợp các khái niệm xuất hiện trong bài này và trong mọi tài liệu Cisco về định tuyến tĩnh:

| Thuật ngữ | Định nghĩa ngắn | Xuất hiện ở |
| --- | --- | --- |
| **Static route** | Tuyến do quản trị viên cấu hình tay, code `S` | Mục Tuyến tĩnh |
| **Default static route** | Tuyến khớp mọi đích (`0.0.0.0 0.0.0.0`) | Mục Tuyến mặc định |
| **Summary route** | Tuyến gộp nhiều mạng con thành một dòng | Mục Tuyến tổng hợp |
| **Next-hop** | Thiết bị liền sau trên đường đi tới đích | Mục Nền tảng |
| **Exit interface** | Cổng vật lý mà gói được đẩy ra khỏi router | Mục Nền tảng |
| **Recursive lookup** | Quá trình tra bảng tiếp để phân giải next-hop thành cổng ra | Mục Nền tảng |
| **Administrative Distance (AD)** | Độ tin cậy của nguồn tuyến; tuyến nào AD nhỏ hơn thắng | Mục Khi nào dùng cái nào |
| **Longest prefix match** | Quy tắc chọn tuyến: prefix dài hơn thắng | Mục Lời giải bài 7 |
| **Stub network** | Mạng chỉ có một tuyến ra ngoài | Mục Thuật ngữ stub |
| **Stub router** | Router chỉ có một router khác để kết nối | Mục Thuật ngữ stub |
| **Transit network** | Mạng chứa ≥ 2 router, cho phép thông tin đi xuyên qua | Mục Thuật ngữ stub |
| **Floating static route** | Tuyến tĩnh có AD cao hơn tuyến chính, dùng làm backup | Mục Khi nào dùng cái nào |
| **Point-to-point link** | Link chỉ có hai đầu, mỗi cổng đúng một hàng xóm | Mục Bẫy Ethernet |
| **Multi-access link** | Link nhiều thiết bị cùng chia sẻ (Ethernet) | Mục Bẫy Ethernet |
| **Directly connected** | Mạng được thêm vào bảng khi cổng `up`, code `C` | Mục Ba nguồn gốc |
| **DCE / DTE** | Thiết bị tạo xung nhịp / thiết bị cuối (router) trong kết nối WAN | Mục Bước 2 lab |
| **Hub-and-spoke** | Mô hình một router trung tâm nối nhiều router nhánh | Mục Khi nào chọn tuyến tĩnh |
| **Equal-cost load balancing** | Gửi song song qua nhiều cổng khi cùng metric | Mục Ba nguồn gốc |
| **CIDR / Route aggregation** | Gom tuyến, cơ sở của summary route | Mục Tuyến tổng hợp |
| **ARP** | Giao thức phân giải địa chỉ IP → MAC trên IPv4 | Mục Bẫy Ethernet |
| **NDP** | Giao thức tương tự ARP cho IPv6 | Mục Nền tảng |
| **Prefix** | Phần đầu của dải địa chỉ, viết dạng `/n` | Mục Tuyến tổng hợp |

### Hop, next-hop và mô hình chỉ đường

Trước khi đi vào ba kỹ thuật, cần chốt cách một router "nghĩ" về đường đi.

**Hop (chặng)** là một lần gói tin chuyển từ phân đoạn mạng này sang phân đoạn kế tiếp. Trên mạng IP, mỗi router trên đường đi tính là một hop, và router giảm trường TTL (Time to Live — thời gian sống) của gói tin để chặn vòng lặp vô hạn [4].

**Next-hop** là thiết bị **liền sau** trên đường đi tới đích, xét từ góc nhìn của router hiện tại [4].

Điểm cốt lõi của định tuyến IP: **mỗi gateway chỉ biết một bước** trên đường đi, không ai biết toàn bộ đường đi [4]. Không router nào có "bản đồ" đầy đủ; chúng chỉ trả lời được câu hỏi *bước tiếp theo của tôi là ai*.

**Mô hình next-hop** là hệ quả trực tiếp:

- Bảng định tuyến thực chất là **danh sách các mạng đích mà ta đã biết next-hop của chúng** [4].
- Chính vì chỉ lưu next-hop mà bảng giữ được kích thước nhỏ [4]. Nếu mỗi dòng phải ghi cả đường đi đầy đủ, bảng sẽ phình không kiểm soát.
- Thiết bị tiêu dùng (PC, điện thoại) thường chỉ có tuyến nội bộ + một cổng mặc định; router cần nhiều tuyến hơn vì phải chuyển tiếp giữa các mạng [4].
- **Ràng buộc bắt buộc:** next-hop phải **kết nối logic được** với router hiện tại, tạo chuỗi liền mạch từ nguồn tới đích [4]. Nếu router không biết cách đi tới chính hàng xóm đó, tuyến trỏ tới nó vô nghĩa.

**Tra cứu đệ quy (recursive lookup):** bảng định tuyến trả về một next-hop (địa chỉ IP hoặc giao diện), nhưng next-hop đó vẫn phải phân giải tiếp thành địa chỉ tầng liên kết dữ liệu (link-layer address) [4]:

- IPv4 dùng ARP; IPv6 dùng NDP (Neighbor Discovery Protocol — giao thức khám phá hàng xóm) [4].
- Đây là lý do exit interface tiết kiệm một lần tra: nó bỏ qua bước suy ra cổng ra, chỉ còn bước phân giải link-layer.

✦ Suy luận: địa chỉ next-hop và địa chỉ đích không nhất thiết cùng phiên bản IP — lưu lượng IPv4 có thể chuyển tiếp qua mạng IPv6 [4]. Yêu cầu không phải "cùng phiên bản", mà là "next-hop phải với tới được".

### Stub network và stub router

**Stub network (mạng cụt)** — cách nói thông dụng — là phân đoạn mạng **không biết về mạng khác** [5]. Đặc điểm nhận dạng:

- Dẫn phần lớn lưu lượng không phải nội bộ qua **một đường duy nhất**, chỉ dựa vào một tuyến mặc định [5].
- Chứa **một router** duy nhất — cổng ra — và **không chuyển tiếp lưu lượng của bên thứ ba** [5].
- Phân biệt với **transit network (mạng trung chuyển)**: transit network chứa ít nhất hai router và cho phép thông tin đi xuyên qua [5].

✦ Ví dụ minh họa: hòn đảo nối đất liền bằng một cây cầu. Dù xây thêm cầu vật lý, nó vẫn chỉ có một đường logic ra ngoài [5].

Các trường hợp thường gặp [5]:

- LAN doanh nghiệp nối mạng công ty qua **một router** duy nhất.
- LAN đơn lẻ không bao giờ chuyển tiếp gói giữa các router.
- Kết nối single-homed (một nhà cung cấp) tới ISP.
- Stub autonomous system (hệ tự trị cụt) — chỉ nối một AS khác để ra Internet. APNIC ghi nhận 22.272/25.577 AS là stub (30/06/2007) [5].

**Stub router (router cụt)** — theo bài giảng — là router **chỉ có một router khác** mà nó kết nối tới [1]. Tức thiết bị đứng ở lối thoát của stub network.

Cả hai cùng đặc điểm: **chỉ có một đường ra**.

### Tại sao Cisco khuyên dùng default static route cho stub router

Đây là khuyến nghị thường gặp nhất trong tài liệu Cisco về định tuyến tĩnh.

**Bối cảnh:** Cisco nhấn mạnh hai tình huống nên dùng tuyến mặc định [1]:

- Router biên nối ISP (không có khớp cụ thể hơn).
- **Stub router** — chỉ có một hàng xóm duy nhất.

**Bốn lý do:**

1. **Không có gì để liệt kê chi tiết.** Stub router có một lối thoát. Mọi đích đi qua cùng hàng xóm. Liệt kê từng mạng chi tiết là thừa — kết quả vẫn là "giao cho cùng router đó" [1].
2. **Một dòng thay cho N dòng.** Thay vì `ip route` cho từng mạng bên ngoài (có thể hàng chục), chỉ cần `ip route 0.0.0.0 0.0.0.0 <next-hop>`. Khi mạng ngoài đổi, không cần sửa [1].
3. **Tuyến mặc định là gộp tuyến triệt để nhất.** Gộp **toàn bộ** không gian IPv4 thành một entry (prefix `/0`, không bit nào khớp). Mọi lợi ích của route aggregation — bảng nhỏ, ít cập nhật — đạt cực đại ở đây [2].
4. **Tuyến tĩnh không tự thích ứng, nhưng stub router ít bị ảnh hưởng.** Tuyến tĩnh không tự bù khi topology đổi. Nhưng stub router chỉ có một lối thoát — lối thoát sống thì tuyến đúng; chết thì không có đường khác để chọn. Môi trường này khiến điểm yếu của static route ít tai hại nhất [1].

**Cảnh báo phạm vi:** chỉ đặt tuyến mặc định ở **router biên** hoặc **stub router**. Ở router trung tâm (nhiều lối thoát), nó kéo mọi lưu lượng chưa khớp về một hướng → đi vòng hoặc mất gói [1].

**Heuristic quyết định nhanh:** đếm số lối thoát. Một → tuyến mặc định đúng. Nhiều hơn một → cần tuyến cụ thể hoặc tuyến động.

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
| --- | --- |
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

```typescript
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

```typescript
R1(config)# interface fastEthernet 0/0
R1(config-if)# ip address 172.16.1.1 255.255.255.0
R1(config-if)# no shutdown
```

Cấu hình cổng serial [1]:

```typescript
R1(config)# interface serial 0/0
R1(config-if)# ip address 172.16.2.1 255.255.255.0
R1(config-if)# no shutdown
```

### Bước 2 — Đặt xung nhịp cho phía DCE

Một kết nối WAN có hai phía [1]:

- **DCE (Data Circuit-terminating Equipment)** — thiết bị phía nhà cung cấp dịch vụ. CSU/DSU là một thiết bị DCE.
- **DTE (Data Terminal Equipment)** — thường là router.

Trong môi trường lab, **một phía của kết nối serial phải được coi là DCE**, và phía đó cần tín hiệu xung nhịp [1]. Cổng serial cần tín hiệu clock để điều khiển nhịp truyền thông [1]:

```typescript
R1(config)# interface serial 0/0
R1(config-if)# clockrate 64000
```

✦ Heuristic: trong Packet Tracer, đầu dây nào có biểu tượng đồng hồ nhỏ là đầu DCE — đặt `clockrate` ở đó. Nếu quên, cổng serial sẽ không lên `up`.

Một tiện ích khi làm việc trên cổng console: vào chế độ cấu hình line của cổng console và thêm `logging synchronous` để tách output tự động khỏi phần bạn đang gõ [1].

### Bước 3 — Cấu hình tuyến tĩnh

Tuyến tĩnh theo **next-hop**:

```
R1(config)# ip route 172.16.3.0 255.255.255.0 172.16.2.2
```

Tuyến tĩnh theo **exit interface**:

```
R1(config)# ip route 172.16.3.0 255.255.255.0 serial 0/0
```

Chọn dạng nào:

- Trên link **point-to-point** (serial) — dùng dạng nào cũng chạy; dạng exit interface giúp tra cứu nhanh hơn.
- Trên link **multi-access** (Ethernet) — bắt buộc dùng **next-hop** [3].

✦ Suy luận: hai lệnh trên cho cùng một đích đến, nhưng khác cách router "ghi nhớ". Dạng exit interface ghi thẳng cổng ra nên tra một lần; dạng next-hop ghi địa chỉ hàng xóm nên router phải tra thêm một lần nữa để tìm cổng. Trên Ethernet, dạng exit interface còn không dùng được vì lý do ở mục trước.

### Bước 4 — Cấu hình tuyến tổng hợp

Giả sử R1 cần biết bốn mạng con liền kề `172.16.4.0/24`, `172.16.5.0/24`, `172.16.6.0/24`, `172.16.7.0/24`. Thay vì bốn tuyến, viết một tuyến tổng hợp:

```
R1(config)# ip route 172.16.4.0 255.255.252.0 172.16.2.2
```

Kiểm tra bằng công thức: 4 mạng → mask octet thứ ba = 256 − 4 = 252 ✓ [2].

### Bước 5 — Cấu hình tuyến mặc định

Trên router biên — router chỉ có một đường ra ngoài — thay vì liệt kê mọi mạng, dùng một dòng [1]:

```vbscript
R1(config)# ip route 0.0.0.0 0.0.0.0 172.16.2.2
```

Hoặc chỉ định cổng ra:

```
R1(config)# ip route 0.0.0.0 0.0.0.0 serial 0/0
```

### Bước 6 — Xác minh

Bảng lệnh xác minh [1]:

| Lệnh | Xem gì |
| --- | --- |
| `show ip route` | Bảng định tuyến |
| `show ip interface brief` | Tình trạng cổng, dạng rút gọn |
| `show interfaces` | Toàn bộ cấu hình cổng |
| `show running-config` | Cấu hình đang chạy trong RAM |
| `show startup-config` | Cấu hình lưu trong NVRAM |
| `show cdp neighbors detail` | Thông tin cấu hình của hàng xóm kết nối trực tiếp |

Để quan sát router cài và gỡ tuyến theo thời gian thực, dùng [1]:

```
R1# debug ip routing
```

Lệnh này cho thấy mọi thay đổi router thực hiện khi **thêm hoặc xoá** tuyến [1].

Điểm cần kiểm tra ngay sau khi cấu hình cổng: khi router mới chỉ có cổng được cấu hình và bảng định tuyến chỉ chứa các mạng kết nối trực tiếp, **chỉ những thiết bị trên các mạng kết nối trực tiếp đó là tới được** [1]. Đây là trạng thái chuẩn trước khi thêm tuyến tĩnh — nếu ping tới mạng xa đã chạy được ở bước này thì có gì đó bất thường.

### Bước 7 — Sửa tuyến tĩnh

Một tuyến tĩnh đã cấu hình có thể cần sửa trong hai trường hợp [1]:

- Mạng đích không còn tồn tại → phải xoá tuyến.
- Cấu trúc mạng thay đổi → phải đổi địa chỉ trung gian hoặc cổng ra.

Cách sửa là xoá rồi nhập lại [1]:

```
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
| --- | --- |
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
- Next-hop trong bảng định tuyến phải "với tới được" từ router hiện tại, tạo chuỗi liền mạch tới đích [4]. Đây là ràng buộc logic, không phải khuyến nghị.
- Thuật ngữ **stub** dùng ở nhiều tầng khác nhau: `stub network` và `stub router` là khái niệm định tuyến chung [1][5], còn `stub area` / `totally stubby area` là khái niệm riêng trong OSPF — biết tuyến mặc định thay cho danh sách chi tiết [5]. Cùng ý tưởng, khác cơ chế.

## Lời giải bài tập

Phần này đi qua bảy bài, mỗi bài kèm câu trả lời đầy đủ để bạn tự kiểm tra mà không cần mở phần khác.

### Bài 1 — Cổng serial không lên `up`

**Đề bài.** Cấu hình hai router nối nhau bằng serial trong Packet Tracer, đặt `clockrate 64000` ở một phía. Kiểm tra bằng `show ip interface brief` xem cổng serial đã `up` chưa.

**Lời giải.**

Ba nguyên nhân thường gặp, theo thứ tự kiểm tra:

1. **Chưa** `no shutdown`**.** Mặc định mọi cổng serial và Ethernet đều down [1]. Chạy `show ip interface brief` — nếu cột `Status` ghi `administratively down`, nghĩa là bạn chưa bật. Vào interface và chạy `no shutdown`.
2. **Quên** `clockrate`**.** Một phía của kết nối serial phải được coi là DCE và cần tín hiệu xung nhịp [1]. Nếu cổng `Status` ghi `down` dù đã `no shutdown`, kiểm tra phía DCE. Trong Packet Tracer, đầu dây có biểu tượng đồng hồ là đầu DCE — đặt `clockrate 64000` ở đó.
3. **Chưa gán địa chỉ IP hoặc gán sai dải.** Cổng vẫn `down` nếu thiếu `ip address`, hoặc hai đầu không nằm cùng subnet.

Kết quả mong đợi: cả cột `Status` và `Protocol` đều ghi `up`.

### Bài 2 — Hai dạng tuyến tĩnh cho cùng một đích

**Đề bài.** Viết tuyến tĩnh tới mạng sau router kia bằng cả hai dạng (next-hop và exit interface), rồi so sánh dòng trong `show ip route`.

**Lời giải.**

Giả sử mạng đích là `172.16.3.0/24`, hàng xóm `172.16.2.2`, cổng nối `serial 0/0`:

```
R1(config)# ip route 172.16.3.0 255.255.255.0 172.16.2.2
R1(config)# ip route 172.16.3.0 255.255.255.0 serial 0/0
```

So sánh dòng trong `show ip route`:

| Dạng | Cột `Gateway of Last Resort` / cột đích | Ý nghĩa |
| --- | --- | --- |
| Next-hop | `S 172.16.3.0/24 [1/0] via 172.16.2.2` | Router biết đi giao cho ai, phải tra tiếp bảng để ra cổng |
| Exit interface | `S 172.16.3.0/24 is directly connected, Serial0/0` | Router biết ngay cổng ra, không cần tra lần hai |

- Cả hai cùng mã nguồn `S`.
- Nếu dùng cả hai cùng lúc, router chọn theo Administrative Distance (thường bằng nhau → có thể load-balance qua hai đường). Thực tế chỉ nên giữ một dạng cho mỗi đích.
- Trên link serial (point-to-point), cả hai hoạt động giống hệt nhau về kết quả chuyển tiếp. Khác biệt chỉ là số lần tra bảng [1][3].

### Bài 3 — Gộp tuyến bằng nhẩm và bằng nhị phân

**Đề bài.** Gộp `10.0.4.0/24` → `10.0.7.0/24` thành một tuyến tổng hợp, tính bằng cả nhị phân lẫn thập phân.

**Lời giải.**

**Cách nhị phân** [2]:

| Địa chỉ | Octet 3 (nhị phân) |
| --- | --- |
| 10.0.4.0 | 0000 0100 |
| 10.0.5.0 | 0000 0101 |
| 10.0.6.0 | 0000 0110 |
| 10.0.7.0 | 0000 0111 |

- Octet 1 và 2 giống nhau: 8 + 8 = 16 bit.
- Octet 3: so `0000 0100` đến `0000 0111` — 5 bit đầu giống nhau (`00000`), 3 bit cuối khác → +5 bit.
- Tổng: 16 + 5 = 21 bit → prefix `/21`.
- Lấy phần chung (`10.0.00000xxx.0`), đặt bit còn lại bằng 0 → `10.0.0.0/21`.
- Mask: `255.255.248.0`.

**Cách thập phân** [2]:

- Bốn mạng `/24` liên tiếp → độ dài prefix giảm 2 bit (2⁴ = 4... nhưng vì chỉ gộp 4 mạng → `/24 − 2 = /22`? Không — đây là kiểm chứng: `256 − 4 = 252` cho octet bị gộp là `255.255.252.0` **chỉ đúng khi bốn mạng chia hết 256 ở cấp octet thứ ba**).
- Với `10.0.4.0` đến `10.0.7.0`, octet thứ ba đi từ 4 đến 7 — đây là bốn mạng, nhưng xét ở octet thứ ba ta có: `256 − 4 = 252` → mask `255.255.252.0` → `/22`.
- ⚠ Kết quả `/22` sẽ bao `10.0.0.0` đến `10.0.3.255` — **sai** vì nó bắt đầu từ `10.0.0.0`, trong khi ta cần từ `10.0.4.0`.

✦ Heuristic: cách thập phân `256 − n` chỉ nhanh khi các mạng **bắt đầu từ 0** ở octet bị gộp. Nếu không, bắt buộc dùng nhị phân. Bài này kết quả đúng là `/21` (mạng `10.0.0.0/21` bao `10.0.0.0`–`10.0.7.255`, nhưng `10.0.0.0`–`10.0.3.255` không thuộc tập ban đầu → gộp `/21` vẫn bao mạng không có; chọn `/21` hay tách rời phụ thuộc cấu trúc thực tế).

Đối chiếu với ví dụ chính của bài: `192.168.0.0`–`192.168.3.0` bắt đầu từ 0 ở octet thứ ba → cả hai cách đều ra `/22` [2].

### Bài 4 — Sửa tuyến tĩnh cấu hình sai

**Đề bài.** Cố tình nhập sai exit interface, tìm ra bằng thứ tự gỡ lỗi, rồi sửa.

**Lời giải.**

Giả sử nhập sai: `ip route 172.16.3.0 255.255.255.0 Serial0/0/1` (trong khi cổng đúng là `Serial0/0/0`).

**Thứ tự xử lý** [1]:

1. `ping` từ nguồn tới đích — thất bại.
2. `traceroute` — xác định gói dừng ở router nào, cổng nào. Nếu kết quả dừng ngay tại router mình thì vấn đề nằm ở bảng định tuyến của router đó.
3. `show ip route` — dòng tuyến tĩnh sai vẫn hiện, nhưng cổng ra trong bảng không khớp topology.
4. Sửa bằng cách xoá rồi nhập lại:

```
R2(config)# no ip route 172.16.3.0 255.255.255.0 Serial0/0/1
R2(config)# ip route 172.16.3.0 255.255.255.0 Serial0/0/0
```

✦ Heuristic: ghi lại chính xác lệnh cũ trước khi `no` — sai một tham số là lệnh `no ip route` không gỡ được tuyến.

### Bài 5 — Đọc `debug ip routing`

**Đề bài.** Bật `debug ip routing`, xoá rồi thêm một tuyến tĩnh, đọc output.

**Lời giải.**

Kết quả mong đợi [1]:

```
R1# debug ip routing
R1(config)# no ip route 172.16.3.0 255.255.255.0 172.16.2.2
%RT: deleting route to 172.16.3.0/24
R1(config)# ip route 172.16.3.0 255.255.255.0 172.16.2.2
%RT: add 172.16.3.0/24 via 172.16.2.2, static
```

Ý nghĩa: dòng `%RT` ghi nhận mọi thay đổi router thực hiện khi thêm hoặc gỡ tuyến. Đây là cách quan sát trực tiếp bảng định tuyến thay đổi theo thời gian thực.

Sau khi kiểm tra, tắt debug: `R1# undebug all`.

### Bài 6 — Stub router và một dòng cấu hình

**Đề bài.** Ba router R1 — R2 — R3, trong đó R3 chỉ nối với R2. R3 có phải stub router không? Cấu hình cho R3 chỉ bằng một dòng.

**Lời giải.**

- **R3 là stub router** — nó chỉ có đúng một router khác mà nó kết nối tới (R2) [1].
- Stub network là mạng chỉ được truy cập qua một tuyến duy nhất [1]. LAN sau R3 chính là stub network.
- Một dòng là đủ vì **mọi** gói tin rời R3 đều đi qua R2. Không có đường nào khác để cần mô tả:

```
R3(config)# ip route 0.0.0.0 0.0.0.0 172.16.2.1
```

(Trong đó `172.16.2.1` là địa chỉ cổng của R2 hướng về R3.)

Kết quả: `ping` từ PC sau R3 tới PC sau R1 thành công. Bảng định tuyến của R3 chỉ có: mạng kết nối trực tiếp (code `C`) + tuyến mặc định (code `S*`).

Tại sao một dòng đủ: R3 không cần biết chi tiết các mạng bên ngoài vì chỉ có một lối thoát. Mọi đích đều đi qua cùng một cửa — tuyến `/0` đã bao trùm tất cả.

### Bài 7 — Tuyến mặc định trên router trung tâm

**Đề bài.** Đặt tuyến mặc định lên R2 (router trung tâm, hai lối ra), thêm một tuyến cụ thể, xem router chọn gì.

**Lời giải.**

Giả sử topology: R1 — R2 — R3, và R2 có hai lối ra (sang R1 và sang R3). Đặt:

```
R2(config)# ip route 0.0.0.0 0.0.0.0 172.16.1.1
R2(config)# ip route 172.16.3.0 255.255.255.0 172.16.2.2
```

`show ip route` cho thấy [1]:

```
S*    0.0.0.0/0 [1/0] via 172.16.1.1
S     172.16.3.0/24 [1/0] via 172.16.2.2
```

**Router chọn tuyến cụ thể** `172.16.3.0/24`, vì:

1. **Longest prefix match** — `/24` dài hơn `/0`, nên khớp tốt hơn cho địa chỉ trong dải `172.16.3.0/24`. Đây là quy tắc chọn tuyến quan trọng nhất trong bảng định tuyến.
2. Tuyến mặc định chỉ được chọn khi **không tồn tại khớp cụ thể hơn** [1].

Vấn đề: nếu `ip route 0.0.0.0 0.0.0.0 172.16.1.1` trỏ sai (tới R1 thay vì R3), mọi gói tin không khớp tuyến cụ thể sẽ bị gửi sai hướng. Đây là lý do **không nên đặt tuyến mặc định ở router trung tâm** — nó kéo mọi lưu lượng chưa xác định về một hướng duy nhất, dễ gây đi vòng hoặc mất gói.

✦ Heuristic: đếm số lối thoát. Một → tuyến mặc định đúng. Nhiều hơn một → cần tuyến cụ thể hoặc tuyến động để phân biệt hướng.

## Tài liệu tham khảo

[1] "Chương 4: Routing — Introduction to Routing and Packet Forwarding; Static Routing," *Mạng Máy Tính Nâng Cao*, HK1 2026–2027, tài liệu bài giảng (Chapter04_Routing.ppt).

[2] NetworkLessons.com, "Route Summarization," *CCNA Routing & Switching*. [Online]. Available: https://networklessons.com/cisco/ccna-routing-switching-icnd1-100-105/route-summarization

[3] ComputerNetworkingNotes, "ip route Command Explained with Examples," *CCNA Study Guide*. [Online]. Available: https://www.computernetworkingnotes.com/ccna-study-guide/ip-route-command-explained-with-examples.html

[4] Wikipedia, "Hop (networking)," *Wikipedia, The Free Encyclopedia*. [Online]. Available: https://en.wikipedia.org/wiki/Hop\_(networking)

[5] Wikipedia, "Stub network," *Wikipedia, The Free Encyclopedia*. [Online]. Available: https://en.wikipedia.org/wiki/Stub_network
