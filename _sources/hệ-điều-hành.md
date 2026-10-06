---
type: Note
_width: wide
---
# Hệ điều hành Đề DH21DTB/2023

## Chapter 1. OS Overview

### 1. Asymmetric Multiprocessing (Đa xử lý bất đối xứng)

Mỗi **processor (bộ xử lý)** được giao một nhiệm vụ cụ thể. 

Một **master processor (bộ xử lý chính)** điều khiển toàn bộ hệ thống.

Các processor còn lại:

- Chờ master đưa ra chỉ thị; hoặc
- Thực hiện các nhiệm vụ đã được định trước.

Mô hình này tạo ra mối quan hệ **master–slave (chính–phụ)**.

👉 **Master processor** chịu trách nhiệm **lập lịch (scheduling)** và **phân bổ công việc** cho các **slave processor**.

### 2. Symmetric Multiprocessing – SMP (Đa xử lý đối xứng)

Đây là mô hình **được sử dụng phổ biến nhất**.

Mỗi processor đều có thể thực hiện **mọi nhiệm vụ trong hệ điều hành**. 

Tất cả các processor đều **bình đẳng (peer)** với nhau, **không tồn tại mối quan hệ master–slave** giữa chúng.

👉 Hiểu đơn giản:

| Asymmetric | Symmetric (SMP) |
| --- | --- |
| 🧑‍✈️ Có 1 processor chính | 👥 Các processor bình đẳng |
| Master phân công việc | Mỗi processor tự có thể nhận việc |
| Có quan hệ master–slave | Không có master–slave |
| Nhiệm vụ có thể được phân công cố định | Processor có thể thực hiện mọi loại nhiệm vụ |

### 3. System call

**System calls (lời gọi hệ thống)** cung cấp phương thức để một **user program (chương trình người dùng)** yêu cầu **operating system (hệ điều hành)** thực hiện các tác vụ vốn được dành riêng cho hệ điều hành, thay mặt cho chương trình người dùng.

Một **system call** có thể được gọi theo nhiều cách khác nhau, tùy thuộc vào chức năng mà **processor (bộ xử lý)** bên dưới cung cấp. Tuy nhiên, trong mọi trường hợp, system call đều là **cơ chế để một process (tiến trình) yêu cầu hệ điều hành thực hiện một hành động**.

### 4. Quản lý tiến trình

Hệ điều hành chịu trách nhiệm:

\* Tạo và xóa các tiến trình của người dùng và hệ thống.

\* Tạm dừng và tiếp tục các tiến trình.

\* Cung cấp cơ chế đồng bộ hóa tiến trình.

\* Cung cấp cơ chế giao tiếp giữa các tiến trình.

\* Cung cấp cơ chế xử lý tình trạng bế tắc.

### 5. Quản lý bộ nhớ

Hệ điều hành chịu trách nhiệm:

\* Theo dõi những vùng bộ nhớ nào đang được sử dụng và đang được tiến trình nào sử dụng.

\* Quyết định tiến trình và dữ liệu nào được đưa vào hoặc đưa ra khỏi bộ nhớ.

\* Cấp phát và giải phóng không gian bộ nhớ khi cần thiết.

### \## 3. Quản lý lưu trữ

#### \### Quản lý hệ thống tập tin

Hệ điều hành chịu trách nhiệm:

\* Tạo và xóa các tập tin.

\* Tạo và xóa các thư mục để tổ chức tập tin.

\* Cung cấp các thao tác cơ bản để xử lý tập tin và thư mục.

\* Ánh xạ các tập tin vào bộ nhớ lưu trữ thứ cấp.

\* Sao lưu các tập tin trên các thiết bị lưu trữ ổn định, không mất dữ liệu khi mất điện.

#### \### Quản lý bộ nhớ lưu trữ lớn

Hệ điều hành chịu trách nhiệm:

\* Quản lý không gian trống.

\* Cấp phát không gian lưu trữ.

\* Lập lịch truy cập đĩa.


======

**1. Trong một hệ thống Symmetric multiprocessing**

- a. Một processor định thời (schedule) và cấp phát (allocate) công việc đến các proccessor khác.
- ==b. Tất cả các processor có quan hệ ngang hàng==
- c. Mỗi proccessor thực thi một nhiệm vụ riêng biệt (specific task)
- d. Một proccessor thực hiện kiểm soát hệ thống gọi là master processor

**2. Cho biết mục đích của system call**

- a. Chuyển hệ thống từ user mode sang kernel mode
- ==b. Cho phép chương trình của người dùng yêu cầu dịch vụ của hệ điều hành==
- c. Cho phép chương trình người dùng điều khiển I/O
- d. Dùng để khởi động hệ thống.

**3. Chọn hai trong các hoạt động chính của hệ điều hành trong quản lý tiến trình (process management)**

- a. Theo dõi các vùng bộ nhớ đang được các tiến trình sử dụng
- ==b. Cung cấp cơ chế đồng bộ các tiến trình==
- c. Xác định tiến trình (proccess) và dữ liệu (data) nào sẽ được đưa vào bộ nhớ hay ra khỏi bộ nhớ
- ==d. Cung cấp cơ chế xử lý deadlock==

**4. Chọn một trong các hoạt động chính của hệ điều hành trong quản lý bộ nhớ (memory management)**

- a. Tạo và xóa user process lẫn system process
- b. Cung cấp cơ chế xử lý deadlock
- ==c. Cấp phát và thu hồi không gian bộ nhớ khi cần==
- d. Cung cấp cơ chế đồng bộ các tiến trình

**5. Cho biết năm hoạt động chính của hệ điều hành trong quản lý hệ thống tập tin (file system management)**

- a. ............................................................................................
- b. ............................................................................................
- c. ............................................................................................
- d. ............................................................................................
- e. ............................................................................................

**6. Cấp phát bộ nhớ theo kiểu MVT (Multiprogramming with a Variable number of Tasks)**

- a. Chia bộ nhớ thành một số phần có kích thước cố định gọi là hole
- b. Phần bộ nhớ khả dụng gọi là hole được cấp một phần vừa đủ cho tiến trình, phần còn lại tạo thành một hole khác.
- c. Phần bộ nhớ khả dụng là một hole và chỉ cấp cho một tiến trình, sau khi tiến trình này kết thúc thì cấp cho tiến trình khác.
- d. Tất cả a, b, c đều sai

**7. Trong quản lý bộ nhớ hãy cho biết khái niệm External Fragment là gì?**


...................................................................................................

**8. Trong quản lý bộ nhớ hãy cho biết khái niệm Internal Fragment là gì?**


...................................................................................................

**9. Trong quản lý bộ nhớ hãy cho biết khái niệm Compaction là gì? Điều kiện để thực hiện compaction?**


...................................................................................................

**10. Cho biết không gian địa chỉ logic có 32 trang (Page) và mỗi trang có 1024 từ nhớ (byte) được ánh xạ vào bộ nhớ vật lý 64 khung trang (Frame) thì**

- a. Địa chỉ luận lý có 13 bit và địa chỉ vật lý có 6 bit
- b. Địa chỉ luận lý có 15 bit và địa chỉ vật lý có 16 bit
- c. Địa chỉ luận lý có 10 bit và địa chỉ vật lý có 11 bit
- d. Tất cả a, b, c đều sai

**11. Cho biết không gian địa chỉ logic có 16 trang (Page) và mỗi trang có 2048 từ nhớ (byte) được ánh xạ vào bộ nhớ vật lý 512 khung trang (Frame) thì**

- a. Địa chỉ luận lý có 11 bit và địa chỉ vật lý có 9 bit
- b. Địa chỉ luận lý có 9 bit và địa chỉ vật lý có 11 bit
- c. Địa chỉ luận lý có 15 bit và địa chỉ vật lý có 20 bit
- d. Địa chỉ luận lý có 11 bit và địa chỉ vậy lý có 16 bit

**12. Trên phần lớn hệ điều hành Window và Unix bộ định thời dài hạn (long-term scheduler) thì**

- a. Chọn process từ process pool để đưa vào bộ nhớ
- b. Không có bộ định thời dài hạn
- c. Chọn process từ process pool đưa vào CPU
- d. Tất cả a, b, c đều sai

**13. Trên một số hệ điều hành chia sẻ thời gian (Time-sharing) người ta sử dụng một bộ định thời trung gian (medium-term scheduler) nhằm mục đích**

- a. Cấp phát CPU cùng lúc cho nhiều process
- b. Chọn được nhiều process từ process pool để đưa vào bộ nhớ
- c. Thực hiện swapping process
- d. Tất cả a, b, c đều sai

**14. Cho biết trong những trường hợp nào một process cha (parent process) có thể kết thúc một process con (children process)?**

- a. ............................................................................................
- b. ............................................................................................
- c. ............................................................................................

**15. Khi một process cha (parent process) tạo một process mới (children process) thì có thể**

- a. Process cha và process con sử dụng chung nguồn tài nguyên
- b. Process cha và process con không sử dụng chung nguồn tài nguyên
- c. Process con sử dụng một phần tài nguyên của process cha
- d. Tất cả a, b, c đều đúng

**16. Khi một process cha (parent process) tạo một process mới (children process) thì không gian địa chỉ của hai process này có thể ?**

- a. ............................................................................................
- b. ............................................................................................

**17. Cho biết nhiệm vụ của các bộ định thời (scheduler)**

- a. Bộ định thời dài hạn (long-term scheduler): ....................................................................
- b. Bộ định thời ngắn hạn (short-term scheduler): ...................................................................
- c. Bộ định thời trung gian (Medium-term scheduler): ...................................................................

**18. Cho biết ý nghĩa viết tắt của các mô hình quan hệ giữa user thread và kernel thread**

- a. ............................................................................................
- b. ............................................................................................
- c. ............................................................................................

**19. Thư viện Win32 thread (Win32 thread library) là**

- a. Thư viện được cung cấp trên hệ thống window ở mức kernel (kernel-level)
- b. Thư viện được cung cấp trên hệ thống window ở mức user (user-level)
- c. Bao gồm cả hai mức user và kernel
- d. Tất cả a, b, c đều sai

**20. Thư viện Pthread là thread mở rộng của chuẩn POSIX là**

- a. Thư viện được cung cấp ở mức kernel (kernel level)
- b. Thư viện được cung cấp ở mức user (user level)
- c. Bao gồm cả hai mức user và kernel
- d. Tất cả a, b, c đều sai

**21. Quan hệ giữa kernel thread và user thread trên hệ điều hành Linux là**

- a. Quan hệ One-to-Many
- b. Quan hệ One-To-One
- c. Quan hệ Many-to-Many
- d. Tất cả a, b, c đều đúng

**22. Cho biết hai kỹ thuật tạo thread trong chương trình Java**

- a. ............................................................................................
- b. ............................................................................................

**23. Khái niệm turnaround time trong định thời CPU (CPU scheduling) là**

- a. Tổng thời gian một process phải chờ trong hàng đợi sẵn sàng (readu queue)
- b. Tổng thời gian từ khi một yêu cầu được đưa vào hệ thống cho đến khi có đáp ứng đầu tiên xảy ra
- c. Tổng thời gian từ lúc một process được đưa vào hệ thống cho đến khi nó hoàn thành
- d. Tất cả a, b, c đều sai

**24. Giả sử hệ thống có các process như hình dưới đây. Thời gian chờ trung bình (average waiting time) các process với giải thuật định thời FCFS (First-Come, First-Served scheduling) là bao nhiêu?**

| **Process** | **Arrival time** | **Burst time (ms)** |
| --- | --- | --- |
| P1 | 0 | 12 |
| P2 | 5 | 8 |
| P3 | 10 | 6 |
| P4 | 15 | 4 |

- a. 13.6
- b. 7.5
- c. 7.0
- d. Khác

**25. Giả sử tại thời điểm đang xét, hệ thống có các process như hình dưới đây. Thời gian chờ trung bình (average waiting time) các process với giải thuật định thời SJF (Shortest-Job-First scheduling) là bao nhiêu?**

| **Process** | **Burst time (ms)** |
| --- | --- |
| P1 | 12 |
| P2 | 8 |
| P3 | 6 |
| P4 | 4 |

- a. 8.0
- b. 7.5
- c. 13.6
- d. Khác

Dưới đây là toàn bộ các câu hỏi còn lại trên Trang 4/6:

**26. Giả sử hệ thống có các process như hình dưới đây. Thời gian chờ trung bình (average waiting time) các process với giải thuật định thời preemptive SJF (Shortest-Job-First scheduling) là bao nhiêu?**

| **Process** | **Arrival time** | **Burst time (ms)** |
| --- | --- | --- |
| P1 | 0 | 12 |
| P2 | 5 | 8 |
| P3 | 10 | 6 |
| P4 | 15 | 4 |

- a. 8.0
- b. 7.5
- c. 5.5
- d. Khác

**27. Giả sử hệ thống có các process như hình dưới đây. Thời gian chờ trung bình (average waiting time) các process với giải thuật định thời preemptive SJF (Shortest-Job-First scheduling) là bao nhiêu?**

| **Process** | **Arrival time** | **Burst time (ms)** |
| --- | --- | --- |
| P1 | 0 | 12 |
| P2 | 2 | 8 |
| P3 | 9 | 6 |
| P4 | 12 | 5 |

- a. 7.75
- b. 6.00
- c. 8.75
- d. Khác

**28. Giả sử hệ thống có các process như hình dưới đây. Thời gian chờ trung bình (average waiting time) các process với giải thuật định thời nonpreemptive SJF (Shortest-Job-First scheduling) là bao nhiêu?**

| **Process** | **Arrival time** | **Burst time (ms)** |
| --- | --- | --- |
| P1 | 0 | 12 |
| P2 | 2 | 8 |
| P3 | 9 | 6 |
| P4 | 16 | 1 |

- a. 4.00
- b. 5.50
- c. 6.75
- d. Khác

**29. Giả sử tại thời điểm đang xét, hệ thống có các process như hình dưới đây. Thời gian chờ trung bình (average waiting time) các process với giải thuật định thời RR (Round-Robin scheduling) là bao nhiêu nếu quantum là 6 miligiây (ms)?**

| **Process** | **Burst time (ms)** |
| --- | --- |
| P1 | 12 |
| P2 | 8 |
| P3 | 16 |
| P4 | 4 |

- a. 20.0
- b. 9.0
- c. 10.0
- d. Khác

**30. Để xử lý vùng critical section, hệ điều hành có thể sử dụng một trong hai phương pháp là preemptive kernel và nonpreemptive kernel. Trong đó nonpreemptive kernel là phương pháp**

- a. Kernel-mode process sẽ thực hiện cho đến khi nó tự động giải phóng CPU
- b. Cho phép một process đang chạy ở kernel mode ngừng và CPU phục vụ process khác
- c. Xảy ra race condition trong cấu trúc dữ liệu của kernel (kernel data structure)
- d. Tất cả a, b, c đều sai

**31. Giả sử process P1 và P2 cùng chia sẻ chung một semaphore synch. Trong P1 có phát biểu S1 và P2 có phát biểu S2. Cho biết đoạn chương trình {...S2; signal(synch);...} trong P2 và {...wait(synch); S1...} trong P1 có tác dụng làm cho**

- a. S2 chỉ thực hiện sau khi S1 hoàn thành
- b. S1 chỉ thực hiện sau khi S2 hoàn thành
- c. S1 và S2 phải thực hiện đồng thời
- d. Tất cả a, b, c đều sai

**32. Một trong những điều kiện để tránh deadlock xảy ra trong hệ thống là**

- a. Tài nguyên ở trạng thái chia sẻ
- b. Tài nguyên ở trạng thái không chia sẻ
- c. Process chỉ được yêu cầu tài nguyên khi nó không giữ bất cứ tài nguyên nào
- d. a và c đúng

**33. Cho biết ý nghĩa của các thành phần sau trong một đồ thị cấp phát tài nguyên (resource-allocation graph)**

- a. Đỉnh của đồ thị: ....................................................................
- b. Cạnh gán (assignment edge): ....................................................................
- c. Cạnh yêu cầu (request edge): ....................................................................

**34. Với một đồ thị cấp phát tài nguyên của một hệ thống (resource-allocation graph) nếu**

- a. Đồ thị không có vòng (cycle) thì không có process nào trong hệ thống bị deadlock
- b. Đồ thị có vòng thì tất cả các process trong hệ thống bị deadlock
- c. Đồ thị có vòng thì các process trong vòng bị deadlock
- d. Tất cả a, b, c đều đúng

**35. Cho hệ thống có 4 process P0, P1, P2, P3 và 3 tài nguyên A, B, C. Mỗi tài nguyên có số thể hiện là A(9 instances), B(7 instances), C (6 instances). Tại một thời điểm trạng thái cấp phát tài nguyên của hệ thống như sau**

|  | **Allocation (A B C)** | **Max (A B C)** |
| --- | --- | --- |
| P0 | 2 2 2 | 3 5 5 |
| P1 | 3 2 1 | 4 2 2 |
| P2 | 1 2 0 | 5 6 2 |
| P3 | 1 0 2 | 3 1 5 |

- Cho biết hệ thống có an toàn hay không?:...............
- Nếu hệ thống an toàn thì cho biết chuỗi cấp phát tài nguyên là:.... ... ... ... ... ...

**36. Cho hệ thống có 5 process P0, P1, P2, P3, P4 và 3 tài nguyên A, B, C. Mỗi tài nguyên có số thể hiện là A(12 instances), B(9 instances), C (7 instances). Tại một thời điểm trạng thái cấp phát tài nguyên của hệ thống như sau**

|  | **Allocation (A B C)** | **Max (A B C)** |
| --- | --- | --- |
| P0 | 2 3 0 | 5 4 2 |
| P1 | 3 2 0 | 5 3 4 |
| P2 | 4 0 0 | 5 3 5 |
| P3 | 1 2 3 | 4 7 4 |
| P4 | 0 1 2 | 2 2 2 |

- Cho biết hệ thống có an toàn hay không?:...............
- Nếu hệ thống an toàn thì cho biết chuỗi cấp phát tài nguyên là:..... ..... ..... ..... .....

**37. Cho hệ thống có 3 process P0, P1, P2 và 3 tài nguyên A, B, C. Mỗi tài nguyên có số thể hiện là A(10 instances), B(7 instances), C(5 instances). Tại một thời điểm hệ thống có trạng thái cấp phát tài nguyên như sau**

|  | **Allocation (A B C)** | **Max (A B C)** |
| --- | --- | --- |
| P0 | 2 3 1 | 3 5 3 |
| P1 | 5 0 2 | 6 5 3 |
| P2 | 1 2 1 | 3 3 2 |

Cho biết hệ thống là an toàn, hãy chọn chuỗi cấp phát tài nguyên của hệ thống

- a. <P0, P1, P2>   
- b. <P1, P0, P2>   
- c. <P2, P0, P1>   
- d. <P2, P0 P1,>   

**38. Cho hệ thống có 4 process P0, P1, P2, P3 và 3 tài nguyên A, B, C. Mỗi tài nguyên có số thể hiện là A(12 instances), B(8 instances), C(7 instances). Tại một thời điểm hệ thống có trạng thái cấp phát tài nguyên như sau**

|  | **Allocation (A B C)** | **Max (A B C)** |
| --- | --- | --- |
| P0 | 2 1 3 | 3 2 5 |
| P1 | 1 3 2 | 4 8 4 |
| P2 | 3 2 0 | 5 3 1 |
| P3 | 4 0 1 | 6 3 1 |

Cho biết hệ thống là an toàn, hãy chọn chuỗi cấp phát tài nguyên của hệ thống

- a. <P0, P1 P2, P3,>   
- b. <P2, P0, P1 P3,>   
- c. <P3, P0 P1, P2,>   
- d. <P3, P0, P1, P2>   

**39. Cho biết lợi ích của việc các nhà phát triển hệ điều hành thiết kế các hệ thống con nhập xuất (I/O subsystem) độc lập với phần cứng**

- a. ............................................................................................
- b. ............................................................................................

**40. Khi controller đặt một tín hiệu vào interrupt-request line thì CPU sẽ thực hiện**

- a. Lưu lại trạng thái hiện tại và chuyển điều khiển đến đoạn thủ tục xử lý ngắt trong bộ nhớ.
- b. Hủy bỏ các tính toán hiện tại và thực hiện tính toán lại từ đầu
- c. CPU đọc dữ liệu từ I/O
- d. Tất cả a, b, c đều sai
