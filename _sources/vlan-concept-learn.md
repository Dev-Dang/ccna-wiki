---
title: "VLAN — mạng LAN ảo: tách miền quảng bá trên cùng một bộ chuyển mạch"
tags:
  - foundation-zero
  - domain/networking
source: "https://www.networkacademy.io/ccna/ethernet/vlan-concept"
questions: 6
bloom-level: 4
language: vi
created: 2026-10-05
generator: foundation-zero-qa
wiki-links: [broadcast-domain, switch, ip-network, subnet-cung-mot-khoi-van-phai-qua-router, mac-address, frame, router, host, default-gateway, ip-address, network-mask]
---

# VLAN — mạng LAN ảo: tách miền quảng bá trên cùng một bộ chuyển mạch

> **Trạng thái file:** đây là bản research độc lập đọc từ NetworkAcademy.io, soạn để tự verify trước khi ingest vào wiki. Nội dung được viết theo góc nhìn "người mới", giải thích VLAN từ đầu — **không** dùng ngữ cảnh của bài IP trước đó, nhưng có đối chiếu tường minh ở cuối bài để bạn nối lại với những gì đã đọc.

---

## 1. Câu hỏi đặt ra

Bài IP trước để lại một chỗ trống: nó nói **một mạng con (subnet) = một miền quảng bá (broadcast domain)**, và **bộ định tuyến (router) là ranh giới chặn quảng bá (broadcast)**. Nhưng nó chưa trả lời hai câu:

- Nếu chỉ có bộ định tuyến mới cắt được miền quảng bá (broadcast domain), thì muốn tách hai nhóm người dùng ra khỏi nhau **mà không mua thêm router** thì làm thế nào?
- Một bộ chuyển mạch (switch) — vốn chỉ có một miền quảng bá (broadcast domain) — có thể đóng vai nhiều "switch ảo" được không?

**VLAN** là câu trả lời cho cả hai. Nó nằm đúng chỗ giao nhau giữa **địa chỉ IP** và **bộ chuyển mạch (switch)**: cấu hình ở tầng bộ chuyển mạch (switch), nhưng hệ quả lại là tạo ra nhiều **mạng con (subnet)** — tức nhiều mạng IP — trên cùng một phần cứng.

---

## 2. Khái niệm tiền đề: BUM traffic và flooding

Trước khi hiểu VLAN, phải hiểu **cái mà ta đang muốn chặn**: lưu lượng BUM.

**BUM** là viết tắt của **broadcast (quảng bá), unknown unicast (unicast chưa biết đích), và multicast**. Khi bộ chuyển mạch (switch) nhận một khung dữ liệu (frame) thuộc một trong ba loại này, nó **gửi khung đó ra mọi cổng trừ cổng vừa nhận vào** [1]. Hành vi này gọi là **flooding — tràn khung dữ liệu ra mọi cổng** [1].

Vì sao phải flood? Vì bộ chuyển mạch (switch) **chỉ đọc địa chỉ MAC (MAC address), không đọc địa chỉ IP** (xem [[switch]]). Khi nó chưa biết địa chỉ MAC đích nằm ở cổng nào, nó không có cách nào khác ngoài việc gửi ra tất cả các cổng và chờ đích trả lời. Đây chính là cơ chế **flood-and-learn — tràn gói tin rồi học địa chỉ** (xem [[broadcast-domain]]).

**Broadcast (quảng bá)** là loại con quan trọng nhất trong ba loại. **Unicast** là khung gửi cho **một đích duy nhất**; **multicast** là gửi cho **một nhóm đích đã đăng ký**; **broadcast** là gửi cho **mọi thiết bị trong phạm vi**.

---

## 3. Khái niệm tiền đề: miền quảng bá (broadcast domain)

**Miền quảng bá (broadcast domain)** là **tập hợp mọi thiết bị nhận được một bản sao của bất kỳ khung BUM nào được gửi trong phạm vi đó** [1]. Nói cách khác: gửi một khung quảng bá (broadcast) vào miền quảng bá (broadcast domain) này thì mọi thiết bị trong đó đều nhận được bản sao.

Quy tắc ngón tay cái của tài liệu [1]:

```text
Một mạng LAN  =  một miền quảng bá (broadcast domain)  =  một mạng con (subnet)
```

Ba thứ này là **ba cách nhìn vào cùng một thứ**:

| Thuật ngữ | Nhìn từ góc độ nào |
|---|---|
| LAN (mạng cục bộ) | góc nhìn **vật lý** — nhóm thiết bị nối với nhau |
| Miền quảng bá (broadcast domain) | góc nhìn **lưu lượng** — ai nhận được bản sao quảng bá (broadcast) |
| Mạng con (subnet) | góc nhìn **địa chỉ IP** — dải địa chỉ cùng mặt nạ mạng (network mask) |

[F0] ⟳ S4 — chạy Quality Gate...

*Vì sao ba thứ này trùng nhau?* Vì cả ba đều mô tả đúng một ranh giới: **thiết bị trong cùng một mạng con (subnet) nhận được quảng bá (broadcast) của nhau, và chỉ những thiết bị đó**. Mặt nạ mạng (network mask) vẽ ra ranh giới này ở tầng IP; bộ chuyển mạch (switch) thực thi nó ở tầng phần cứng.

**Diễn giải:** "LAN = Broadcast Domain = Subnet" chỉ là quy tắc ngón tay cái trong trạng thái mặc định. VLAN chính là thứ **phá vỡ** sự trùng khớp này ở tầng vật lý — nó khiến nhiều miền quảng bá (broadcast domain) cùng tồn tại trên **một** mạng LAN vật lý.

---

## 4. Vì sao cần VLAN

### 4.1. Trạng thái mặc định là một miền quảng bá (broadcast domain) duy nhất

Mặc định, **mọi cổng trên một bộ chuyển mạch (switch) Cisco đều nằm trong cùng một miền quảng bá (broadcast domain)** [1]. Nghĩa là mọi khung BUM nhận được ở một cổng đều được flood ra tất cả các cổng còn lại.

Đây gọi là **flat network — mạng phẳng**: không có phân tách giữa người dùng hay thiết bị [1].

### 4.2. Vì sao mạng phẳng là vấn đề

Tổ chức thực tế **cần** tách các nhóm người dùng ra. Ví dụ trong tài liệu [1]:

- **Phòng R&D** cần giữ bí mật cho dự án riêng.
- **Phòng HR** cần bảo vệ thông tin nhạy cảm như lương nhân viên.

Nếu hai phòng dùng chung một miền quảng bá (broadcast domain):

- Lưu lượng BUM của R&D **tràn tới** người dùng HR.
- Lưu lượng của HR **tràn tới** R&D.

Hệ quả là **vấn đề bảo mật và riêng tư** — không phải vì ai đó "hack", mà vì thiết bị này nhìn thấy lưu lượng của nhóm khác một cách hoàn toàn hợp lệ theo giao thức [1].

### 4.3. Cách giải quyết khi chưa có VLAN — và vì sao bế tắc

Muốn tách hai nhóm ra khi **chưa** có VLAN, cách duy nhất là **mỗi nhóm cắm vào một bộ chuyển mạch (switch) vật lý riêng** [1].

Cách này bế tắc ở chỗ: nếu tổ chức cần **hàng trăm hoặc hàng nghìn** miền quảng bá (broadcast domain) tách biệt, thì phải có hàng trăm/nghìn bộ chuyển mạch (switch) vật lý [1]. Không ai làm vậy được. Đây chính là **giới hạn khả năng mở rộng (scaling limitation)** — và theo tài liệu [1], **chính nó là lý do VLAN ra đời**.

---

## 5. VLAN là gì

**VLAN (Virtual Local Area Network – mạng LAN ảo)** là **một con số từ 1 đến 4096** [1]. Chỉ vậy thôi.

- **Mỗi cổng trên bộ chuyển mạch (switch) luôn được gán vào một số VLAN cụ thể** [1].
- **Các cổng được gán cùng một số VLAN nằm trong cùng một miền quảng bá (broadcast domain)** [1].

Điểm cốt lõi: **một bộ chuyển mạch (switch) vật lý có thể hành xử như nhiều "bộ chuyển mạch (switch) logic"** [1]. Bộ chuyển mạch (switch) **sẽ không bao giờ** chuyển khung dữ liệu (frame) của một người dùng sang bất kỳ máy chủ nào — và ngược lại — nếu chúng nằm ở VLAN khác nhau, vì chúng thuộc hai miền quảng bá (broadcast domain) khác nhau [1].

### 5.1. Cơ chế: VLAN được gán theo cổng

VLAN hoạt động **trên cơ sở từng cổng một (port-by-port)** [1]:

```text
Bộ chuyển mạch (switch) vật lý
├── VLAN 10 (CLIENTS) — cổng Fa0/1, Fa0/2, Fa0/3, Fa0/4
│     └── miền quảng bá (broadcast domain) #1
└── VLAN 20 (SERVERS) — cổng Fa0/15, Fa0/16, Fa0/17, Fa0/18
      └── miền quảng bá (broadcast domain) #2

Khung BUM vào cổng VLAN 10 → chỉ ra cổng khác của VLAN 10
Khung BUM vào cổng VLAN 20 → chỉ ra cổng khác của VLAN 20
```

Bộ chuyển mạch (switch) **dùng số VLAN gán cho cổng để quyết định lưu lượng thuộc miền quảng bá (broadcast domain) nào** [1]. Đây là toàn bộ cơ chế — không có gì phức tạp hơn.

### 5.2. Hệ quả: hai thiết bị khác VLAN không nói chuyện được

Nếu hai thiết bị nằm trên **hai VLAN khác nhau**, thì **dù cùng cắm vào một bộ chuyển mạch (switch), chúng không thể giao tiếp trực tiếp với nhau** [1].

Đây chính là điểm chạm với địa chỉ IP: VLAN là **một mạng con (subnet) riêng**, tức **một mạng IP riêng**. Máy khách ở `192.168.1.0/24` (VLAN 10) và máy chủ ở `10.1.0.0/24` (VLAN 20) có mặt nạ mạng (network mask) nói rằng chúng khác mạng — và bộ chuyển mạch (switch) sẽ không chuyển khung dữ liệu (frame) giữa chúng.

**Diễn giải:** VLAN và mạng con (subnet) là **hai cách diễn đạt cùng một ranh giới**. VLAN cấu hình ranh giới ở tầng phần cứng (số gán cho cổng); mạng con (subnet) biểu diễn nó ở tầng địa chỉ (mặt nạ mạng — network mask). Hai thứ **phải khớp nhau**, nếu không ta sẽ có một ranh giới ở phần cứng nhưng địa chỉ lại nói "chúng cùng mạng" (hoặc ngược lại) — và kết quả là không gì định tuyến được.

---

## 6. VLAN 1 và các VLAN không xoá được

### 6.1. VLAN 1 — VLAN mặc định

Mặc định, **mọi cổng trên bộ chuyển mạch (switch) Cisco đều được gán vào VLAN 1** [1]. Điều này có nghĩa **toàn bộ bộ chuyển mạch (switch) vận hành như một miền quảng bá (broadcast domain) duy nhất** [1] — đúng trạng thái flat network ở mục 4.1.

### 6.2. Năm VLAN luôn tồn tại

Trên mọi bộ chuyển mạch (switch) Cisco, có **5 VLAN không xoá được** luôn hiện diện [1]:

| VLAN | Trạng thái | Dùng được không | Ghi chú |
|---|---|---|---|
| **1** | active | **Có** | Gọi là **Default VLAN**; mọi interface mặc định thuộc nó |
| 1002 | act/unsup | **Không** | fddi-default |
| 1003 | act/unsup | **Không** | token-ring-default |
| 1004 | act/unsup | **Không** | fddinet-default |
| 1005 | act/unsup | **Không** | trnet-default |

*(`act/unsup` = "active/unsupported" — hoạt động nhưng không được hỗ trợ.)*

- **VLAN 1** không thể xoá **nhưng dùng được** [1].
- **VLAN 1002–1005** không thể xoá **và không dùng được** [1]. Đây là **di tích còn lại từ thời FDDI và Token Ring** — hai công nghệ mạng đã lỗi thời [1].

---

## 7. Lợi ích của VLAN

Tài liệu [1] liệt kê bốn lợi ích:

- **Tăng bảo mật** — giảm số thiết bị đầu cuối nhận được bản sao lưu lượng BUM.
- **Tạo miền lỗi (fault domain) nhỏ hơn** — cô lập các nhóm thiết bị vào miền quảng bá (broadcast domain) riêng.
- **Giảm tải CPU trên mỗi thiết bị** — bằng cách giới hạn số khung quảng bá (broadcast) mà mỗi thiết bị phải nhận và xử lý.
- **Tăng hiệu năng mạng** và **tốc độ phục hồi khi có sự cố** nhanh hơn.

**Suy luận:** ba lợi ích đầu cùng chung một gốc — *ít thiết bị nhận BUM hơn*. Bảo mật tốt hơn vì ít người thấy lưu lượng của người khác; miền lỗi nhỏ hơn vì một quảng bá (broadcast) bất thường không lan sang nhóm khác; CPU giảm vì mỗi host phải xử lý ít khung quảng bá (broadcast) hơn. Lợi ích thứ tư (hiệu năng, phục hồi) là hệ quả gián tiếp của việc bớt lưu lượng thừa.

---

## 8. Tạo VLAN trên bộ chuyển mạch (switch) Cisco

### 8.1. Bộ chuyển mạch (switch) Cisco chạy được ngay không cần cấu hình

Bộ chuyển mạch (switch) Cisco **không cần cấu hình ban đầu** để hoạt động — mở hộp, cắm dây, bật nguồn là chạy [1]. Mặc định mọi interface ở VLAN 1, nên **mọi thiết bị cắm vào đều cùng miền quảng bá (broadcast domain) và phải cùng một mạng con (subnet)** [1].

Điều này đúng cả khi **nối nhiều bộ chuyển mạch (switch) còn nguyên cấu hình mặc định với nhau**: chúng tạo thành **một miền quảng bá (broadcast domain) liên bộ chuyển mạch (switch)**, nghĩa là mọi máy khách nối vào vẫn **phải cùng một mạng con IP** [1].

Đến một lúc nào đó, bạn sẽ cần nối các máy khách thuộc **mạng con (subnet) khác nhau**. Lúc đó **buộc phải tạo VLAN** [1].

### 8.2. Hai bước tạo VLAN

**Bước 1 — Tạo VLAN mới trong cơ sở dữ liệu VLAN của bộ chuyển mạch (switch):**

- Ở chế độ cấu hình toàn cục (global configuration), dùng lệnh `vlan [vlan-id]` để tạo VLAN mới trong cơ sở dữ liệu.
- *(Tuỳ chọn)* Trong chế độ cấu hình VLAN, dùng lệnh `name [name]` để gán tên cho VLAN.

**Bước 2 — Gán interface vào VLAN vừa tạo:**

- Ở chế độ cấu hình toàn cục, dùng lệnh `interface [number]` để vào chế độ cấu hình interface.
- Dùng lệnh `switchport access vlan [id]` để chỉ định số VLAN gán cho interface.
- *(Tuỳ chọn)* Dùng `switchport mode access` để cổng **luôn hoạt động ở chế độ access port**.

**Diễn giải:** *access port* là cổng chỉ thuộc **một** VLAN duy nhất — trái ngược với *trunk port* (cổng mang nhiều VLAN, sẽ học ở bài sau). Vì mỗi máy khách thường chỉ thuộc một nhóm, cổng nối tới nó gần như luôn là access port.

### 8.3. Ví dụ cấu hình đầy đủ

Ví dụ từ tài liệu [1]: tạo hai VLAN — **VLAN 10 tên CLIENTS** và **VLAN 20 tên SERVERS** — rồi gán bốn cổng access cho mỗi VLAN.

**Trước khi cấu hình — cơ sở dữ liệu VLAN mặc định:**

```text
Switch# show vlan brief

VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active    Fa0/1, Fa0/2, Fa0/3, Fa0/4
                                                Fa0/5, Fa0/6, Fa0/7, Fa0/8
                                                Fa0/9, Fa0/10, Fa0/11, Fa0/12
                                                Fa0/13, Fa0/14, Fa0/15, Fa0/16
                                                Fa0/17, Fa0/18, Fa0/19, Fa0/20
                                                Fa0/21, Fa0/22, Fa0/23, Fa0/24
                                                Gig0/1, Gig0/2
1002 fddi-default                     act/unsup
1003 token-ring-default               act/unsup
1004 fddinet-default                  act/unsup
1005 trnet-default                    act/unsup
```

**Bước 1 — Tạo VLAN 10 và VLAN 20:**

```text
Switch# configure terminal
Enter configuration commands, one per line.  End with CNTL/Z.
Switch(config)# vlan 10
Switch(config-vlan)# name CLIENTS
Switch(config-vlan)# exit
Switch(config)# vlan 20
Switch(config-vlan)# name SERVERS
Switch(config-vlan)# end
```

**Kiểm tra sau Bước 1:**

```text
Switch#show vlan brief

VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active    Fa0/1, Fa0/2, Fa0/3, Fa0/4
                                                Fa0/5, Fa0/6, Fa0/7, Fa0/8
                                                Fa0/9, Fa0/10, Fa0/11, Fa0/12
                                                Fa0/13, Fa0/14, Fa0/15, Fa0/16
                                                Fa0/17, Fa0/18, Fa0/19, Fa0/20
                                                Fa0/21, Fa0/22, Fa0/23, Fa0/24
                                                Gig0/1, Gig0/2
10   CLIENTS                          active
20   SERVERS                          active
1002 fddi-default                     act/unsup
1003 token-ring-default               act/unsup
1004 fddinet-default                  act/unsup
1005 trnet-default                    act/unsup
```

**Bước 2 — Gán interface vào VLAN:**

```text
Switch# configure terminal
Enter configuration commands, one per line.  End with CNTL/Z.
Switch(config)# interface range fastEthernet 0/1 - 4
Switch(config-if-range)# switchport access vlan 10
Switch(config-if-range)# switchport mode access
Switch(config-if-range)# exit
Switch(config)# interface range fastEthernet 0/15 - 18
Switch(config-if-range)# switchport access vlan 20
Switch(config-if-range)# switchport mode access
Switch(config-if-range)# end
```

**Kết quả cuối:**

```text
Switch#show vlan brief

VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active    Fa0/5, Fa0/6, Fa0/7, Fa0/8
                                                Fa0/9, Fa0/10, Fa0/11, Fa0/12
                                                Fa0/13, Fa0/14, Fa0/19, Fa0/20
                                                Fa0/21, Fa0/22, Fa0/23, Fa0/24
                                                Gig0/1, Gig0/2
10   CLIENTS                          active    Fa0/1, Fa0/2, Fa0/3, Fa0/4
20   SERVERS                          active    Fa0/15, Fa0/16, Fa0/17, Fa0/18
1002 fddi-default                     act/unsup
1003 token-ring-default               act/unsup
1004 fddinet-default                  act/unsup
1005 trnet-default                    act/unsup
```

**Diễn giải:** để ý rằng sau Bước 1, VLAN 10 và 20 **đã tồn tại nhưng chưa có cổng nào** — mọi cổng vẫn ở VLAN 1 [1]. Phải làm Bước 2 thì VLAN mới "có hiệu lực". Đây là chi tiết hay bị bỏ qua khi học: **tạo VLAN ≠ gán cổng vào VLAN**.

### 8.4. Kiểm chứng bằng ping

Sau khi gán cổng, tài liệu [1] kiểm chứng bằng hai lần ping:

**Máy khách 1 (`192.168.1.10/24`) ping máy khách 2 (`192.168.1.11`) — cùng VLAN 10:**

```text
C:\> ipconfig

FastEthernet0 Connection:(default port)
   Link-local IPv6 Address.........: FE80::20A:41FF:FE83:371A
   IP Address......................: 192.168.1.10
   Subnet Mask.....................: 255.255.255.0
   Default Gateway.................: 0.0.0.0

C:\>ping 192.168.1.11
Pinging 192.168.1.11 with 32 bytes of data:
Reply from 192.168.1.11: bytes=32 time<1ms TTL=128
Reply from 192.168.1.11: bytes=32 time<1ms TTL=128
Reply from 192.168.1.11: bytes=32 time<1ms TTL=128
Reply from 192.168.1.11: bytes=32 time<1ms TTL=128
Ping statistics for 192.168.1.11:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss)
```

→ **Thành công**. Cùng VLAN = cùng miền quảng bá (broadcast domain) = nói chuyện trực tiếp được.

**Máy khách 1 (`192.168.1.10`) ping máy chủ (`10.1.0.10`) — khác VLAN:**

```text
C:\> ping 10.1.0.10
Pinging 10.1.0.10 with 32 bytes of data:
Request timed out.
Request timed out.
Request timed out.
Request timed out.
Ping statistics for 10.1.0.10:
    Packets: Sent = 4, Received = 0, Lost = 4 (100% loss)
```

→ **Thất bại 100%**. Khác VLAN = bị cô lập hoàn toàn.

**Diễn giải:** để ý `Default Gateway.................: 0.0.0.0` trong `ipconfig` — máy khách **không có cổng mặc định (default gateway)**. Điều này không ảnh hưởng tới ping thành công trong cùng VLAN 10 (vì cùng mạng con — subnet thì không cần cổng mặc định — default gateway), nhưng nó cũng đảm bảo rằng ping thất bại sang VLAN 20 **không phải vì thiếu gateway** — mà vì VLAN chặn ở tầng phần cứng. Đây là chi tiết làm phép kiểm chứng có giá trị: nó cô lập được nguyên nhân.

### 8.5. Muốn hai VLAN nói chuyện được thì sao

Để chuyển dữ liệu **giữa các VLAN**, **phải dùng một bộ chuyển mạch (switch) Layer 3 hoặc một bộ định tuyến (router)** [1]. Đây chính là lý do bài IP nói **bộ định tuyến (router) là ranh giới chặn quảng bá (broadcast)**: VLAN tạo ra ranh giới, và chỉ thiết bị tầng 3 mới vượt qua được ranh giới đó. Tài liệu [1] hẹn bài sau sẽ nói về cách này.

---

## 9. Bảng tổng kết: VLAN đối chiếu với mô hình trong bài IP

Phần này **không restate nội dung từ tài liệu [1]** — nó dùng **wiki notes đã có** để chỉ ra VLAN nằm ở đâu trong bức tranh địa chỉ IP. Dùng để bạn đối chiếu trực tiếp với bài IP.

| Khái niệm trong bài IP | Trong bài VLAN | Liên hệ với nhau thế nào |
|---|---|---|
| Một mạng con (subnet) = một mạng IP | Một VLAN = một miền quảng bá (broadcast domain) | Hai cách diễn đạt **cùng một ranh giới**: VLAN ở tầng phần cứng, subnet ở tầng địa chỉ. Xem [[ip-network]] |
| Bộ định tuyến (router) là ranh giới chặn quảng bá (broadcast) | Muốn nối hai VLAN phải dùng router hoặc Layer 3 switch | VLAN tạo ranh giới; router là thiết bị **vượt qua** ranh giới đó. Xem [[router]] |
| Bộ chuyển mạch (switch) chỉ đọc MAC, không đọc IP | Bộ chuyển mạch (switch) dùng **số VLAN gán cho cổng** để quyết định miền | VLAN là **cấu hình ở switch**, không phải ở IP. Xem [[switch]] |
| Chia một khối thành nhiều subnet vẫn phải qua router | Tách một switch thành nhiều VLAN vẫn phải qua router | **Cùng một sự thật nhìn từ hai tầng khác nhau.** Xem [[subnet-cung-mot-khoi-van-phai-qua-router]] |

**Suy luận:** mô hình tinh thần đúng là — **VLAN là cách tạo mạng con (subnet) bằng cấu hình phần cứng thay vì bằng chia dây vật lý.** Trong bài IP, bạn chia một khối địa chỉ bằng mặt nạ mạng (network mask). Trong bài VLAN, bạn chia một bộ chuyển mạch (switch) bằng số VLAN gán cho cổng. Kết quả ở tầng IP là **giống hệt nhau**: hai nhóm thiết bị bị ngăn cách và phải đi qua bộ định tuyến (router). Sự khác biệt chỉ ở **công cụ**: một bên là mặt nạ mạng (network mask), một bên là VLAN ID.

---

## 10. Key takeaways

- Bộ chuyển mạch (switch) LAN **flood mọi khung BUM ra mọi cổng trừ cổng vào** — quá trình gọi là **flooding** [1].
- Mặc định mọi thiết bị nằm cùng một miền quảng bá (broadcast domain) (**VLAN 1**) — vừa là **giới hạn khả năng mở rộng**, vừa là **lỗ hổng bảo mật** [1].
- **VLAN** ra đời để **tách thiết bị vào các miền quảng bá (broadcast domain) khác nhau** [1].
- Quy tắc ngón tay cái: **một VLAN = một miền quảng bá (broadcast domain) = một mạng con (subnet)** [1].
- VLAN được **cấu hình và gán theo từng cổng bộ chuyển mạch (switchport)** [1].
- **VLAN chỉ là một số từ 1 đến 4096** — toàn bộ sức mạnh nằm ở chỗ số đó tách lưu lượng ra sao [1].
- **Tạo VLAN và gán cổng vào VLAN là hai bước khác nhau** — làm bước 1 mà bỏ bước 2 thì VLAN tồn tại nhưng không có tác dụng [1].
- Muốn hai VLAN giao tiếp, **phải có bộ định tuyến (router) hoặc Layer 3 switch** [1].

---

## 11. Câu hỏi tự kiểm

1. Một khung **unicast đã biết đích** có bị flood không? Vì sao? *(gợi ý: xem định nghĩa BUM)*
2. Vì sao nói "LAN = Broadcast Domain = Subnet" chỉ đúng **ở trạng thái mặc định**, còn VLAN phá vỡ đẳng thức này?
3. Hai máy khách cắm cùng một bộ chuyển mạch (switch), một máy ở VLAN 10 và một máy ở VLAN 20. Máy ở VLAN 10 có ping được máy ở VLAN 20 không? Cần thêm thiết bị gì?
4. Trong ví dụ cấu hình, sau Bước 1 (`vlan 10`, `vlan 20`) nhưng **trước** Bước 2 (`switchport access vlan`), các cổng Fa0/1–Fa0/4 thuộc VLAN nào?
5. `Default Gateway: 0.0.0.0` trong `ipconfig` nói lên điều gì về máy khách trong ví dụ? Nếu máy khách có cổng mặc định (default gateway), kết quả hai lần ping có đổi không? Vì sao?

---

## Tài liệu tham khảo

[1] NetworkAcademy.io, "VLAN Concept," *CCNA: Ethernet*, 2025. [Online]. Available: https://www.networkacademy.io/ccna/ethernet/vlan-concept. [Accessed: Oct. 5, 2026].