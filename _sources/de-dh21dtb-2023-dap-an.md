---
title: "Hệ điều hành — Đề DH21DTB/2023 (đã sửa lỗi OCR, có đáp án)"
type: Note
created: 2026-10-06
_width: wide
---
# Hệ điều hành — Đề DH21DTB/2023

> **Quy ước:** `[x]` là đáp án đúng. Phần tự luận có đáp án in đậm ngay dưới câu hỏi.

> **Đã sửa OCR:** `proccessor/proccess` → `processor/process`, `readu queue` → `ready queue`, `vậy lý` → `vật lý`, bỏ dòng thừa "Dưới đây là toàn bộ các câu hỏi còn lại trên Trang 4/6", chuẩn hóa các lựa chọn dạng `<P0, P1, P2>` ở câu 37–38.

> **Cách đọc thuật ngữ:** tên đầy đủ → viết tắt → ý nghĩa.

---

## Phần 1 — Tổng quan hệ điều hành

**1. Trong một hệ thống Symmetric multiprocessing**

- [ ] a. Một processor định thời (schedule) và cấp phát (allocate) công việc đến các processor khác.
- [x] b. Tất cả các processor có quan hệ ngang hàng
- [ ] c. Mỗi processor thực thi một nhiệm vụ riêng biệt (specific task)
- [ ] d. Một processor thực hiện kiểm soát hệ thống gọi là master processor

**Giải thích:** Symmetric Multiprocessing (SMP, đa xử lý đối xứng) nghĩa là mọi processor ngang quyền, cùng chạy OS. Các lựa chọn a, c, d mô tả Asymmetric Multiprocessing (đa xử lý bất đối xứng, có master).

**2. Cho biết mục đích của system call**

- [ ] a. Chuyển hệ thống từ user mode sang kernel mode
- [x] b. Cho phép chương trình của người dùng yêu cầu dịch vụ của hệ điều hành
- [ ] c. Cho phép chương trình người dùng điều khiển I/O
- [ ] d. Dùng để khởi động hệ thống.

**Giải thích:** System call (lời gọi hệ thống) là cửa để chương trình người dùng nhờ OS làm việc. Chuyển sang kernel mode chỉ là hệ quả đi kèm, không phải mục đích.

**3. Chọn hai trong các hoạt động chính của hệ điều hành trong quản lý tiến trình (process management)**

- [ ] a. Theo dõi các vùng bộ nhớ đang được các tiến trình sử dụng
- [x] b. Cung cấp cơ chế đồng bộ các tiến trình
- [ ] c. Xác định tiến trình (process) và dữ liệu (data) nào sẽ được đưa vào bộ nhớ hay ra khỏi bộ nhớ
- [x] d. Cung cấp cơ chế xử lý deadlock

**Giải thích:** Đồng bộ và xử lý deadlock (bế tắc) thuộc quản lý tiến trình. a và c thuộc quản lý bộ nhớ.

**4. Chọn một trong các hoạt động chính của hệ điều hành trong quản lý bộ nhớ (memory management)**

- [ ] a. Tạo và xóa user process lẫn system process
- [ ] b. Cung cấp cơ chế xử lý deadlock
- [x] c. Cấp phát và thu hồi không gian bộ nhớ khi cần
- [ ] d. Cung cấp cơ chế đồng bộ các tiến trình

**Giải thích:** a, b, d đều thuộc quản lý tiến trình.

**5. Cho biết năm hoạt động chính của hệ điều hành trong quản lý hệ thống tập tin (file system management)**

- a. **Tạo và xóa tập tin (file).**
- b. **Tạo và xóa thư mục (directory).**
- c. **Cung cấp các thao tác cơ bản (primitive) để thao tác trên tập tin và thư mục.**
- d. **Ánh xạ tập tin lên bộ nhớ phụ (secondary storage).**
- e. **Sao lưu tập tin lên thiết bị lưu trữ ổn định (stable storage).**

---

## Phần 2 — Quản lý bộ nhớ

**6. Cấp phát bộ nhớ theo kiểu MVT (Multiprogramming with a Variable number of Tasks)**

- [ ] a. Chia bộ nhớ thành một số phần có kích thước cố định gọi là hole
- [x] b. Phần bộ nhớ khả dụng gọi là hole được cấp một phần vừa đủ cho tiến trình, phần còn lại tạo thành một hole khác.
- [ ] c. Phần bộ nhớ khả dụng là một hole và chỉ cấp cho một tiến trình, sau khi tiến trình này kết thúc thì cấp cho tiến trình khác.
- [ ] d. Tất cả a, b, c đều sai

**Giải thích:** MVT chia theo kích thước yêu cầu (biến đổi). Lựa chọn a mô tả kiểu phân vùng cố định.

**7. Trong quản lý bộ nhớ hãy cho biết khái niệm External Fragment là gì?**

**Phân mảnh ngoài (external fragmentation): tổng bộ nhớ trống đủ để đáp ứng một yêu cầu, nhưng bị chia thành nhiều hole nhỏ không liên tục nên không cấp được.**

**8. Trong quản lý bộ nhớ hãy cho biết khái niệm Internal Fragment là gì?**

**Phân mảnh trong (internal fragmentation): phần bộ nhớ cấp cho tiến trình lớn hơn nhu cầu; phần dư nằm bên trong vùng đã cấp và không ai dùng được.**

**9. Trong quản lý bộ nhớ hãy cho biết khái niệm Compaction là gì? Điều kiện để thực hiện compaction?**

**Compaction (dồn bộ nhớ): xáo trộn nội dung bộ nhớ để gom tất cả hole thành một khối trống lớn liên tục. Điều kiện: việc gắn địa chỉ (relocation) phải là động, thực hiện lúc chạy (execution time), để tiến trình vẫn chạy đúng sau khi bị dời chỗ.**

**10. Không gian địa chỉ logic có 32 trang (Page), mỗi trang 1024 byte, ánh xạ vào bộ nhớ vật lý 64 khung trang (Frame) thì**

- [ ] a. Địa chỉ luận lý có 13 bit và địa chỉ vật lý có 6 bit
- [x] b. Địa chỉ luận lý có 15 bit và địa chỉ vật lý có 16 bit
- [ ] c. Địa chỉ luận lý có 10 bit và địa chỉ vật lý có 11 bit
- [ ] d. Tất cả a, b, c đều sai

**Giải thích:** Logic: 32 trang = 2^5 (5 bit) và 1024 byte = 2^10 (10 bit), tổng 15 bit. Vật lý: 64 frame = 2^6 (6 bit) cộng 10 bit, tổng 16 bit.

**11. Không gian địa chỉ logic có 16 trang, mỗi trang 2048 byte, ánh xạ vào bộ nhớ vật lý 512 khung trang thì**

- [ ] a. Địa chỉ luận lý có 11 bit và địa chỉ vật lý có 9 bit
- [ ] b. Địa chỉ luận lý có 9 bit và địa chỉ vật lý có 11 bit
- [x] c. Địa chỉ luận lý có 15 bit và địa chỉ vật lý có 20 bit
- [ ] d. Địa chỉ luận lý có 11 bit và địa chỉ vật lý có 16 bit

**Giải thích:** Logic: 4 + 11 = 15 bit. Vật lý: 9 + 11 = 20 bit.

---

## Phần 3 — Tiến trình và thread

**12. Trên phần lớn hệ điều hành Windows và Unix, bộ định thời dài hạn (long-term scheduler) thì**

- [ ] a. Chọn process từ process pool để đưa vào bộ nhớ
- [x] b. Không có bộ định thời dài hạn
- [ ] c. Chọn process từ process pool đưa vào CPU
- [ ] d. Tất cả a, b, c đều sai

**Giải thích:** Các OS chia sẻ thời gian phổ biến đưa process thẳng vào bộ nhớ, không cần long-term scheduler.

**13. Bộ định thời trung gian (medium-term scheduler) trên một số hệ điều hành chia sẻ thời gian nhằm mục đích**

- [ ] a. Cấp phát CPU cùng lúc cho nhiều process
- [ ] b. Chọn được nhiều process từ process pool để đưa vào bộ nhớ
- [x] c. Thực hiện swapping process
- [ ] d. Tất cả a, b, c đều sai

**Giải thích:** Swapping là tạm đưa process ra đĩa rồi nạp lại để giảm mức độ đa chương (degree of multiprogramming).

**14. Cho biết trong những trường hợp nào một process cha có thể kết thúc một process con?**

- a. **Process con dùng vượt quá mức tài nguyên cho phép.**
- b. **Nhiệm vụ giao cho process con không còn cần thiết.**
- c. **Process cha kết thúc mà OS không cho phép con tiếp tục chạy (kết thúc dây chuyền, cascading termination).**

**15. Khi một process cha tạo một process con thì có thể**

- [ ] a. Process cha và process con sử dụng chung nguồn tài nguyên
- [ ] b. Process cha và process con không sử dụng chung nguồn tài nguyên
- [ ] c. Process con sử dụng một phần tài nguyên của process cha
- [x] d. Tất cả a, b, c đều đúng

**Giải thích:** Có ba kiểu: chia sẻ tất cả, chia sẻ một phần, hoặc không chia sẻ.

**16. Khi process cha tạo process con thì không gian địa chỉ của hai process này có thể?**

- a. **Con là bản sao của cha (duplicate), như** `fork()` **trên Unix.**
- b. **Con có một chương trình mới được nạp vào (như** `exec()` **sau** `fork()`**).**

**17. Cho biết nhiệm vụ của các bộ định thời (scheduler)**

- a. Bộ định thời dài hạn (long-term scheduler): **chọn process từ pool (hàng đợi công việc) để đưa vào bộ nhớ, tức vào ready queue; kiểm soát mức độ đa chương.**
- b. Bộ định thời ngắn hạn (short-term scheduler): **chọn process trong ready queue để cấp CPU tiếp theo; chạy rất thường xuyên.**
- c. Bộ định thời trung gian (medium-term scheduler): **swap process ra hoặc vào bộ nhớ để giảm mức độ đa chương.**

**18. Cho biết ý nghĩa viết tắt của các mô hình quan hệ giữa user thread và kernel thread**

- a. **Many-to-One: nhiều user thread ánh xạ vào một kernel thread.**
- b. **One-to-One: mỗi user thread ánh xạ vào một kernel thread riêng.**
- c. **Many-to-Many: nhiều user thread ghép (multiplex) vào một số kernel thread.**

**19. Thư viện Win32 thread (Win32 thread library) là**

- [x] a. Thư viện được cung cấp trên hệ thống Windows ở mức kernel (kernel-level)
- [ ] b. Thư viện được cung cấp trên hệ thống Windows ở mức user (user-level)
- [ ] c. Bao gồm cả hai mức user và kernel
- [ ] d. Tất cả a, b, c đều sai

**20. Thư viện Pthread là thread mở rộng của chuẩn POSIX là**

- [ ] a. Thư viện được cung cấp ở mức kernel (kernel level)
- [ ] b. Thư viện được cung cấp ở mức user (user level)
- [x] c. Bao gồm cả hai mức user và kernel
- [ ] d. Tất cả a, b, c đều sai

**Giải thích:** Portable Operating System Interface Threads (Pthreads) chỉ là đặc tả API, nên có thể cài ở mức user hoặc mức kernel tùy hệ thống.

**21. Quan hệ giữa kernel thread và user thread trên hệ điều hành Linux là**

- [ ] a. Quan hệ One-to-Many
- [x] b. Quan hệ One-To-One
- [ ] c. Quan hệ Many-to-Many
- [ ] d. Tất cả a, b, c đều đúng

**22. Cho biết hai kỹ thuật tạo thread trong chương trình Java**

- a. **Kế thừa lớp** `Thread` **và ghi đè phương thức** `run()`**.**
- b. **Cài đặt interface** `Runnable` **rồi truyền đối tượng vào** `new Thread(...)`**.**

---

## Phần 4 — Định thời CPU

> Công thức dùng chung: **waiting time = thời điểm hoàn thành − thời điểm đến − burst time**.

**23. Khái niệm turnaround time trong định thời CPU là**

- [ ] a. Tổng thời gian một process phải chờ trong hàng đợi sẵn sàng (ready queue)
- [ ] b. Tổng thời gian từ khi một yêu cầu được đưa vào hệ thống cho đến khi có đáp ứng đầu tiên xảy ra
- [x] c. Tổng thời gian từ lúc một process được đưa vào hệ thống cho đến khi nó hoàn thành
- [ ] d. Tất cả a, b, c đều sai

**Giải thích:** a là waiting time, b là response time.

**24. Average waiting time với First-Come, First-Served (FCFS)?**

| Process | Arrival time | Burst time (ms) |
| --- | --- | --- |
| P1 | 0 | 12 |
| P2 | 5 | 8 |
| P3 | 10 | 6 |
| P4 | 15 | 4 |

- [ ] a. 13.6
- [ ] b. 7.5
- [x] c. 7.0
- [ ] d. Khác

**Giải thích:** Thứ tự P1 (0–12), P2 (12–20), P3 (20–26), P4 (26–30). Waiting: 0, 7, 10, 11. Tổng 28, chia 4 = **7.0**.

**25. Average waiting time với Shortest-Job-First (SJF), tất cả đến lúc 0?**

| Process | Burst time (ms) |
| --- | --- |
| P1 | 12 |
| P2 | 8 |
| P3 | 6 |
| P4 | 4 |

- [x] a. 8.0
- [ ] b. 7.5
- [ ] c. 13.6
- [ ] d. Khác

**Giải thích:** Thứ tự P4, P3, P2, P1. Waiting: P4 = 0, P3 = 4, P2 = 10, P1 = 18. Tổng 32, chia 4 = **8.0**.

**26. Average waiting time với preemptive SJF (Shortest Remaining Time First, SRTF)?**

| Process | Arrival time | Burst time (ms) |
| --- | --- | --- |
| P1 | 0 | 12 |
| P2 | 5 | 8 |
| P3 | 10 | 6 |
| P4 | 15 | 4 |

- [ ] a. 8.0
- [ ] b. 7.5
- [x] c. 5.5
- [ ] d. Khác

**Giải thích:** Không có lần ngắt nào xảy ra vì phần còn lại của process đang chạy luôn nhỏ hơn hoặc bằng process mới đến. Thứ tự P1 (0–12), P3 (12–18), P4 (18–22), P2 (22–30). Waiting: P1 = 0, P2 = 30−5−8 = 17, P3 = 18−10−6 = 2, P4 = 22−15−4 = 3. Tổng 22, chia 4 = **5.5**.

**27. Average waiting time với preemptive SJF?**

| Process | Arrival time | Burst time (ms) |
| --- | --- | --- |
| P1 | 0 | 12 |
| P2 | 2 | 8 |
| P3 | 9 | 6 |
| P4 | 12 | 5 |

- [ ] a. 7.75
- [x] b. 6.00
- [ ] c. 8.75
- [ ] d. Khác

**Giải thích:** P1 (0–2), P2 chen vào (2–10), P3 (10–16), P4 (16–21), P1 chạy nốt (21–31). Waiting: P1 = 31−0−12 = 19, P2 = 0, P3 = 1, P4 = 4. Tổng 24, chia 4 = **6.00**.

**28. Average waiting time với nonpreemptive SJF?**

| Process | Arrival time | Burst time (ms) |
| --- | --- | --- |
| P1 | 0 | 12 |
| P2 | 2 | 8 |
| P3 | 9 | 6 |
| P4 | 16 | 1 |

- [ ] a. 4.00
- [x] b. 5.50
- [ ] c. 6.75
- [ ] d. Khác

**Giải thích:** P1 (0–12), P3 (12–18), P4 (18–19), P2 (19–27). Waiting: P1 = 0, P2 = 17, P3 = 3, P4 = 2. Tổng 22, chia 4 = **5.5**.

**29. Average waiting time với Round-Robin (RR), quantum = 6 ms?**

| Process | Burst time (ms) |
| --- | --- |
| P1 | 12 |
| P2 | 8 |
| P3 | 16 |
| P4 | 4 |

- [x] a. 20.0
- [ ] b. 9.0
- [ ] c. 10.0
- [ ] d. Khác

**Giải thích:** Gantt: P1 0–6, P2 6–12, P3 12–18, P4 18–22 (xong), P1 22–28 (xong), P2 28–30 (xong), P3 30–36, P3 36–40 (xong). 

Waiting: P1 = 16, P2 = 22, P3 = 24, P4 = 18. Tổng 80, chia 4 = **20.0**.

---

## Phần 5 — Đồng bộ và deadlock

**30. Nonpreemptive kernel là phương pháp**

- [x] a. Kernel-mode process sẽ thực hiện cho đến khi nó tự động giải phóng CPU
- [ ] b. Cho phép một process đang chạy ở kernel mode ngừng và CPU phục vụ process khác
- [ ] c. Xảy ra race condition trong cấu trúc dữ liệu của kernel (kernel data structure)
- [ ] d. Tất cả a, b, c đều sai

**Giải thích:** b mô tả preemptive kernel. Nonpreemptive kernel không bị ngắt giữa chừng, nên tránh được race condition trong dữ liệu kernel.

**31. Đoạn** `{...S2; signal(synch);...}` **trong P2 và** `{...wait(synch); S1...}` **trong P1 có tác dụng làm cho**

- [ ] a. S2 chỉ thực hiện sau khi S1 hoàn thành
- [x] b. S1 chỉ thực hiện sau khi S2 hoàn thành
- [ ] c. S1 và S2 phải thực hiện đồng thời
- [ ] d. Tất cả a, b, c đều sai

**Giải thích:** `synch` khởi tạo bằng 0 nên P1 bị chặn ở `wait` cho tới khi P2 làm xong S2 và gọi `signal`.

**32. Một trong những điều kiện để tránh deadlock xảy ra trong hệ thống là**

- [ ] a. Tài nguyên ở trạng thái chia sẻ
- [ ] b. Tài nguyên ở trạng thái không chia sẻ
- [ ] c. Process chỉ được yêu cầu tài nguyên khi nó không giữ bất cứ tài nguyên nào
- [x] d. a và c đúng

**Giải thích:** Phòng ngừa deadlock là phá một trong bốn điều kiện. Tài nguyên chia sẻ phá mutual exclusion (loại trừ tương hỗ). Chỉ xin khi không giữ gì phá hold and wait (giữ và chờ).

**33. Ý nghĩa các thành phần trong đồ thị cấp phát tài nguyên (resource-allocation graph)**

- a. Đỉnh của đồ thị: **process (P) và loại tài nguyên (R); mỗi chấm trong R là một instance (thể hiện).**
- b. Cạnh gán (assignment edge): **R → P, một instance của R đã được cấp cho P.**
- c. Cạnh yêu cầu (request edge): **P → R, P đang yêu cầu và chờ R.**

**34. Với một đồ thị cấp phát tài nguyên, nếu**

- [x] a. Đồ thị không có vòng (cycle) thì không có process nào trong hệ thống bị deadlock
- [ ] b. Đồ thị có vòng thì tất cả các process trong hệ thống bị deadlock
- [ ] c. Đồ thị có vòng thì các process trong vòng bị deadlock
- [ ] d. Tất cả a, b, c đều đúng

**Giải thích:** Không có vòng thì chắc chắn không deadlock. Có vòng chỉ chắc chắn deadlock khi mỗi loại tài nguyên có đúng một instance; nếu có nhiều instance thì vòng chỉ là điều kiện cần. Vì vậy c không đúng tổng quát.

### Dạng bài Banker's Algorithm (thuật toán nhà băng)

> **Cách làm:** Need = Max − Allocation. Available = Tổng − Σ Allocation. Lặp: tìm process có Need ≤ Work, cho chạy xong, cộng Allocation của nó vào Work. Nếu hết process thì an toàn; nếu kẹt thì không an toàn.

**35. Hệ thống 4 process P0–P3, tài nguyên A(9), B(7), C(6). Hệ thống có an toàn không? Nếu có, nêu chuỗi cấp phát.**

|  | Allocation (A B C) | Max (A B C) |
| --- | --- | --- |
| P0 | 2 2 2 | 3 5 5 |
| P1 | 3 2 1 | 4 2 2 |
| P2 | 1 2 0 | 5 6 2 |
| P3 | 1 0 2 | 3 1 5 |

- Cho biết hệ thống có an toàn hay không?: **KHÔNG an toàn (unsafe).**
- Chuỗi cấp phát: **không có.**

**Giải thích:** Available = (9−7, 7−6, 6−5) = **(2, 1, 1)**. Need: P0 (1,3,3), P1 (1,0,1), P2 (4,4,2), P3 (2,1,3). Chỉ P1 thỏa Need ≤ Work, chạy xong thì Work = (5, 3, 2). Sau đó P0 cần C=3 > 2, P2 cần B=4 > 3, P3 cần C=3 > 2, cả ba đều kẹt.

**36. Hệ thống 5 process P0–P4, tài nguyên A(12), B(9), C(7). Hệ thống có an toàn không? Nếu có, nêu chuỗi cấp phát.**

|  | Allocation (A B C) | Max (A B C) |
| --- | --- | --- |
| P0 | 2 3 0 | 5 4 2 |
| P1 | 3 2 0 | 5 3 4 |
| P2 | 4 0 0 | 5 3 5 |
| P3 | 1 2 3 | 4 7 4 |
| P4 | 0 1 2 | 2 2 2 |

- Cho biết hệ thống có an toàn hay không?: **AN TOÀN (safe).**
- Chuỗi cấp phát: **<P4, P1, P0, P3, P2>**

**Giải thích:** Available = (12−10, 9−8, 7−5) = **(2, 1, 2)**. Need: P0 (3,1,2), P1 (2,1,4), P2 (1,3,5), P3 (3,5,1), P4 (2,1,0).

| Bước | Chọn | Work trước | Work sau |
| --- | --- | --- | --- |
| 1 | P4 | (2,1,2) | (2,2,4) |
| 2 | P1 | (2,2,4) | (5,4,4) |
| 3 | P0 | (5,4,4) | (7,7,4) |
| 4 | P3 | (7,7,4) | (8,9,7) |
| 5 | P2 | (8,9,7) | (12,9,7) |

**37. Hệ thống 3 process P0, P1, P2, tài nguyên A(10), B(7), C(5). Biết hệ thống an toàn, chọn chuỗi cấp phát.**

|  | Allocation (A B C) | Max (A B C) |
| --- | --- | --- |
| P0 | 2 3 1 | 3 5 3 |
| P1 | 5 0 2 | 6 5 3 |
| P2 | 1 2 1 | 3 3 2 |

- [ ] a. <P0, P1, P2>
- [ ] b. <P1, P0, P2>
- [x] c. <P2, P0, P1>
- [ ] d. <P2, P0, P1> (lỗi OCR ở ký tự phân cách; nội dung gốc cần đối chiếu lại bản giấy)

**Giải thích:** Available = **(2, 2, 1)**. Need: P0 (1,2,2), P1 (1,5,1), P2 (2,1,1). Chỉ P2 chạy được đầu tiên, Work = (3,4,2). Tiếp theo P0 (Work = 5,7,3), cuối cùng P1. Chuỗi duy nhất là <P2, P0, P1>.

**38. Hệ thống 4 process P0–P3, tài nguyên A(12), B(8), C(7). Biết hệ thống an toàn, chọn chuỗi cấp phát.**

|  | Allocation (A B C) | Max (A B C) |
| --- | --- | --- |
| P0 | 2 1 3 | 3 2 5 |
| P1 | 1 3 2 | 4 8 4 |
| P2 | 3 2 0 | 5 3 1 |
| P3 | 4 0 1 | 6 3 1 |

- [ ] a. <P0, P1, P2, P3>
- [ ] b. <P2, P0, P1, P3>
- [ ] c. <P3, P0, P1, P2>
- [ ] d. <P3, P0, P1, P2>

**Kết quả tính: chuỗi an toàn duy nhất là <P2, P3, P0, P1>, nhưng không có lựa chọn nào khớp.** Có khả năng số liệu hoặc các lựa chọn bị OCR sai; cần đối chiếu lại đề gốc.

**Giải thích:** Available = (12−10, 8−6, 7−6) = **(2, 2, 1)**. Need: P0 (1,1,2), P1 (3,5,2), P2 (2,1,1), P3 (2,3,0).

| Bước | Chọn | Work trước | Work sau |
| --- | --- | --- | --- |
| 1 | P2 | (2,2,1) | (5,4,1) |
| 2 | P3 | (5,4,1) | (9,4,2) |
| 3 | P0 | (9,4,2) | (11,5,5) |
| 4 | P1 | (11,5,5) | (12,8,7) |

- Các lựa chọn bắt đầu bằng P3 sai vì P3 cần B=3 mà Work chỉ có B=2.
- Lựa chọn b có P2 đứng đầu nhưng P0 kế tiếp cần C=2 > 1 nên kẹt.
- Lựa chọn a bắt đầu bằng P0 cần C=2 > 1 nên cũng kẹt.

---

## Phần 6 — Nhập xuất (I/O)

**39. Lợi ích của việc các nhà phát triển hệ điều hành thiết kế các hệ thống con nhập xuất (I/O subsystem) độc lập với phần cứng**

- a. **Người phát triển OS không phải viết lại phần I/O cho mỗi thiết bị phần cứng mới, vì device driver (trình điều khiển thiết bị) che giấu khác biệt giữa các thiết bị.**
- b. **Nhà sản xuất phần cứng có thể thiết kế thiết bị tương thích với giao diện controller sẵn có hoặc chỉ cần cung cấp driver, nên thiết bị mới chạy được trên nhiều OS.**

**40. Khi controller đặt một tín hiệu vào interrupt-request line thì CPU sẽ thực hiện**

- [x] a. Lưu lại trạng thái hiện tại và chuyển điều khiển đến đoạn thủ tục xử lý ngắt trong bộ nhớ.
- [ ] b. Hủy bỏ các tính toán hiện tại và thực hiện tính toán lại từ đầu
- [ ] c. CPU đọc dữ liệu từ I/O
- [ ] d. Tất cả a, b, c đều sai

**Giải thích:** CPU lưu trạng thái, nhảy tới interrupt handler (trình xử lý ngắt), xử lý xong thì khôi phục trạng thái và chạy tiếp.

---

## Bảng đáp án nhanh

| Câu | Đáp án | Câu | Đáp án | Câu | Đáp án | Câu | Đáp án |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 1 | b | 11 | c | 21 | b | 31 | b |
| 2 | b | 12 | b | 22 | tự luận | 32 | d |
| 3 | b, d | 13 | c | 23 | c | 33 | tự luận |
| 4 | c | 14 | tự luận | 24 | c | 34 | a |
| 5 | tự luận | 15 | d | 25 | a | 35 | Không an toàn |
| 6 | b | 16 | tự luận | 26 | c | 36 | <P4,P1,P0,P3,P2> |
| 7 | tự luận | 17 | tự luận | 27 | b | 37 | c |
| 8 | tự luận | 18 | tự luận | 28 | b | 38 | ⚠ <P2,P3,P0,P1> |
| 9 | tự luận | 19 | a | 29 | a | 39 | tự luận |
| 10 | b | 20 | c | 30 | a | 40 | a |
