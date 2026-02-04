## I. Các khái niệm cơ bản:
### 1. Tối đa hoá hiệu quả CPU:
Hầu hết tài nguyên phải được điều phối trước khi dùng.
- CPU là tài nguyên quan trọng nhất trong hệ thống.
- Mục tiêu: Luôn có tiến trình thực thi để tối ưu công suất.
- Giảm thiểu thời gian CPU ở trạng thái nhàn rỗi (Idle).

#### a) Chu kỳ Burst CPU - I/O (CPU – I/O Burst Cycle):
![image](https://hackmd.io/_uploads/BJAWS41PWe.png)
- Tiến trình luân chuyển qua hai trạng thái chính.
- Trạng thái thực thi trên CPU (CPU Burst) - Thời gian tiến trình dùng CPU để tính toán (CPU đang bận).
- Trạng thái chờ đợi vào ra (I/O Burst): Thời gian tiến trình chờ I/O (Đọc/Ghi file, bàn phím, mạng,...), tiến trình không dùng CPU.
- Thực thi bắt đầu và kết thúc bằng CPU Burst.
- Đặc điểm các loại tiến trình:
    - I/O - bound: Nhiều CPU burst rất ngắn.
    - CPU - bound: Một vài CPU burst rất dài.
    - Hệ điều hành cần cân bằng để tối ưu hệ thống.

#### b) Bộ điều phối CPU (CPU Scheduler):
![image](https://hackmd.io/_uploads/Sy1lU4ywZl.png)
- Còn gọi là bộ điều phối ngắn hạn (Short-term scheduler).
- Chọn tiến trình từ bộ nhớ (Ready Queue) để cấp CPU.
- Hàng đợi Ready có thể tổ chức theo nhiều cấu trúc.
> FIFO, Priority Queue, Tree, Linked List
- Đối tượng điều phối:
    - Trên hệ điều hành hiện đại, luồng (Thread) là đơn vị điều phối.
    - Thuật ngữ "Điều phối tiến trình" vẫn dùng phổ biến.
    - Điều phối viên quyết định ai nhận kiểm soát lõi CPU.
    - Quá trình diễn ra liên tục (mỗi vài milisecond).

#### c) Điều phối ưu tiên và không ưu tiên (Preemptive and Nonpreemptive Scheduling):
##### Điều phối không ưu tiên:
- Tiến trình giữ CPU cho đến khi tự nguyện giải phóng.
- Chuyển từ Running sang Waiting (theo yêu cầu I/O).
- Tiến trình kết thúc (Terminate).
- Không có sự can thiệp từ bên ngoài để thu hồi CPU.
- Đặc điểm và hạn chế:
    - Dùng trong các hệ thống cũ (Windows 3.x).
    - Ưu điểm: Đơn giản, không cần phần cứng đặc biệt.
    - Nhược điểm: Một tiến trình lỗi có thể làm treo hệ thống.
    - Không phù hợp cho các hệ thống đa nhiệm hiện đại.
##### Điều phối có ưu tiên:
- Hệ điều hành có quyền ngắt tiến trình đang chạy.
- Xảy ra khi có ngắt (interrupt) hoặc tín hiệu timer.
- Cần thiết cho hệ thống chia sẻ thời gian.
- Đảm bảo mọi tiến trình đều có cơ hội dùng CPU.
- Thách thức kỹ thuật:
    - Tranh chấp dữ liệu (Race conditions) khi dùng chung.
    - Ảnh hưởng đến cơ chế thiết kế của nhân (Kernel design).
    - Cần cơ chế đồng bộ hoá phức tạp (Mutex, Semaphore).
    - Hệ điều hạnh hiện đại hầu heets là Preemptive.

#### d) Bộ phân phối (Dispatcher):
![image](https://hackmd.io/_uploads/Hk_L_VJwWl.png)
- Module thực hiện việc trao quyền điều khiển CPU.
- Nhận quyết định từ CPU Scheduler để thực thi.
- Thực hiện các thao tác mức thấp của hệ thống.
- Đảm bảo tiến trình mới bắt đầu đúng vị trí đã dừng.
- Các bước xử lý cụ thể:
    - Chuyển ngữ cảnh (Switching context).
    - Chuyển sang User mode.
    - Nhảy tới vị trí phù hợp trong User mode.
    - Thực thi các lệnh để khởi động lại tiến trình.
- Dispatch Lantency:
    - Thời gian Dispatcher dừng một tiến trình và chạy cái khác.
    - Đây là khoảng thời gian chết đối với hiệu suất.
    - Yêu cầu: Dispatcher phải cực nhanh và hiệu quả.

## II. Tiêu chí điều phối:
- CPU Utilization:
    - Mục tiêu: Giữ CPU bận rộn 100% thời gian nếu có thể.
    - Thực tế: Dao động từ 40% (tải nhẹ) đến 90% (tải nặng).
    - Cần tránh để CPU rơi vào trạng thái nhàn rỗi quá lâu.
- Throughput:
    - Số lượng tiến trình hoàn thành trong đơn vị thời gian.
    - Tiến trình dài: Thông lượng có thể là một tiến trình/giờ.
    - Tác vụ ngắn: Thông lượng có thể là 10 tiến trình/giây.
- Turnaround Time:
    - Thời gian từ lúc nộp đến khi hoàn thành.
    - Gồm: Chờ hàng đợi, thực thi, thời gian I/O.
    - Công thức: $Turnaround = Finish Time - Arrival Time$
- Waiting Time:
    - Tổng thời gian tiến trình phải nằm trong hàng đợi Ready.
    - Điều phối không ảnh hướng đến thời gian thực thi hay I/O.
    - Nó chỉ ảnh hưởng trực tiếp đến thời gian chờ.
- Response Time:
    - Thời gian từ lúc yêu cầu đến khi có phản hồi đầu tiên.
    - Quan trọng trong hệ thống tương tác.
    - User quan tâm hệ thống có phản ứng hay không.
- Tóm tắt tiêu chí tối ưu:
    - Tối đa hoá CPU Utilization và Throughput.
    - Tối thiểu hoá Turnaround, Waiting và Response Time.
    - Đảm bảo tính ổn định (Độ lệch chuẩn thấp).

## III. Các thuật toán chi tiết:
### 1. First-Come, First-Served Scheduling:
![image](https://hackmd.io/_uploads/SkRF3VJwbg.png)
- Tiến trình yêu cầu trước được thực hiện trước.
- Được quản lý bằng cấu trúc hàng đợi FIFO.
- Dễ hiểu và lậo trình đơn giản nhất.
- Thời gian chờ là 0ms cho $P_1$, 24ms cho $P_2$ và 27ms cho $P_3$. Thời gian chờ trung bình là 17ms.
- Hạn chế:
    - Thời gian chờ trung bình thường rất cao.
    - Hiệu ứng Convoy: Tiến trình ngắn đợn tiến trình dài.
    - Là thuật toán không quyền ưu tiên (nonpreemptive).

### 2. Shortest-Job-First Scheduling (SJF):
![image](https://hackmd.io/_uploads/rkygaNkPbg.png)
- Ưu tiên tiến trình có CPU burst tiếp theo ngắn nhất, nếu bằng nhau thì áp dụng FCFS.
- Cung cấp thời gian chờ trung bình tối thiểu (Optimal).
- Thời gian chờ là 3ms cho $P_1$, 16ms cho $P_2$, 9ms cho $P_3$ và 0ms cho $P_4$. Như vậy thời gian chờ trung bình là 7ms. Để so sánh, nếu dùng FCFS thì thời gian chờ trung bình là 10.25ms.
- Thách thức:
    - Khó xác định chiều dài CPU burst tiếp theo.
    - Thường dùng trong điều phối dài hạn.
    - Trong điều phối ngắn hạn phải dùng dự đoán:
        - Dự đoán dựa trên thực tế và dự đoán cũ:
        - Phương pháp trung bình luỹ thừa: $$\tau_{n + 1} = \alpha.\tau_n + (1 - \alpha)\tau_n$$ Ý nghĩa tham số $\alpha$:
            - Nếu $\alpha = 0$: Bỏ qua thực tế, dùng dự đoán cũ.
            - Nếu $\alpha = 1$: Chỉ quan tâm đến CPU burst gần nhất.
            - Giúp hệ thống học được đặc điểm tiến trình.

            ![image](https://hackmd.io/_uploads/ry1tAEkDbe.png)
            ![image](https://hackmd.io/_uploads/r1AUCNJDWl.png)
- Shortest-Remainingtime-First Scheduling (SRTF) - SJF có quyền ưu tiên: 
    - Khi tiến trình mới có CPU burst < thời gian còn lại, hệ thống thực hiện trưng dụng CPU ngay lập tức.
    - SRTF là thuật toán lập lịch có quyền ưu tiên ngắt, trong đó CPU luôn được cấp cho tiến trình có thời gian CPU còn lại nhỏ nhất. Khi một tiến trình mới đến có burst ngắn hơn tiến trình đang chạy, CPU sẽ bị ngắt và chuyển sang tiến trình mới.

![image](https://hackmd.io/_uploads/HylP1BJwZg.png)
![image](https://hackmd.io/_uploads/SJtD1rJDbl.png)
![image](https://hackmd.io/_uploads/BJvuJr1w-l.png)
![image](https://hackmd.io/_uploads/HJQFyr1vWl.png)
![image](https://hackmd.io/_uploads/By2KyryDbe.png)

### 3. Round-Robin Scheduling (RR):
![image](https://hackmd.io/_uploads/rkRulHkw-g.png)
- Dành cho hệ thống chia sẻ thời gian.
- Dùng định mực thời gian (Time Quantum/Slice).
- Hết Quantum, tiến trình về cuối hàng đợi Ready.
- Lựa chọn Time Quantum:
    - Nếu $q$ quá lớn, RR chuyển thành FCFS.
    - Nếu $q$ quá nhỏ: Chi phí chuyển ngữ cảnh quá lớn.
    - Quy tắc: $q$ nên lớn hơn nhiều thời gian chuyển ngữ cảnh.
- Hiệu quả thực tế:
    - Thời gian vòng đời thường dài hơn SJF.
    - Thời gian đáp ứng (Response Time) tốt hơn nhiều.
    - Giá trị $q$ phổ biến từ 10ms đến 100ms.

### 4. Priority Scheduling:
![image](https://hackmd.io/_uploads/B1olZrJwWx.png)
![image](https://hackmd.io/_uploads/SkYW-rkvZg.png)
- Một số nguyên được gán cho mỗi tiến trình.
- Mức ưu tiên định nghĩa nội tại hoặc bên ngoài.
- Số nhỏ thường đại diện cho ưu tiên cao (0 là cao nhất).
- Vấn đề Starvation:
    - Tiến trình ưu tiên thấp có thể không bao giờ được chạy.
    - Giải pháp: Lão hoá (Aging).
    - Mỗi khoảng thời gian, tăng độ ưu tiên cho tiến trình chờ.

### 5. Multilevel Queue Scheduling:
![image](https://hackmd.io/_uploads/BJ5KbBJvZx.png)
- Nhóm: Foreground (tương tác) và Background.
- Mỗi nhóm có hàng đợi và thuật toán riêng.
- Tiến trình không di chuyển giữa các hàng đợi.
- Điều phối giữa hàng đợi:

![image](https://hackmd.io/_uploads/HJ7ibH1vbg.png)
    - Ưu tiên cố định: Chạy hết hàng một mới đến hàng hai.
    - Phân chia thời gian: Mỗi hàng đợi nhận một % CPU.

### 6. Multilevel Feedback Queue Scheduling (MFQ):
![image](https://hackmd.io/_uploads/B12XGrJPZl.png)
- Cơ chế linh hoạt nhất, cho phép di chuyển giữa các hàng đợi.
- Dùng quá nhiều CPU sẽ bị đẩy xuống hàng thấp hơn.
- Chờ quá lâu sẽ được đẩy lên hàng cao hơn.
- Tham số cấu hình MFQ:
    - Số lượng hàng đợi.
    - Thuật toán điều phối cho mỗi hàng đợi.
    - Tiêu chuẩn nâng cấp và hạ cấp tiến trình.
> Ví dụ thực tế:
> 
> ![image](https://hackmd.io/_uploads/Hki47S1w-e.png)

## IV. Điều phối luồng (Thread Scheduling):
- User & Kernel Threads:
    - Nhân thực hiện điều phối luồng nhân.
    - Luồng user ánh xạ tới luồng nhân qua LWP.
- Process - Contention Scope:
    - Xảy ra trong các thư viện luồng (Threads).
    - Các luồng tranh chấp CPU trong cùng một tiến trình.
    - Áp dụng cho mô hình Many-to-One/Many-to-Many.
- System - Contention Scope:
    - Xảy ra ở mức toàn hệ thống.
    - Các luồng nhân tranh chấp với nhau để giành CPU.
    - Cơ chế chính của Windows, Linux, macOS.

## V. Điều phối đa bộ xử lý (Multi-Processor Scheduling):
### 1. Approaches to Multiple-Processor Scheduling:
Symmetric Multiprocessing:

![image](https://hackmd.io/_uploads/BJSsHHyvWg.png)
- Mọi bộ xử lý đều có quyền hạn như nhau.
- Cấu trúc 1: Một hàng đợi chung (Dễ tranh chấp khoá).
- Cấu trúc 2: Mỗi CPU có hàng đợi riêng (phổ biến).

### 2. Multicore Processors:
- Nhiễu lõi trên một chip vật lý duy nhất.
- Tốc độ nhanh, tiêu thụ ít năng lượng hơn.
- Memory Stall: Là lõi **đã sẵn sàng thực thi**, phải đợi dữ liệu từ bộ nhớ chính.
    - Nhưng:
        - Dữ liệu **không có trong cache**.
        - Phải chờ dữ liệu từ **RAM**.
    - Trong thời gian chờ:
        - Pipeline bị dừng.
        - Lõi gần như nhàn rỗi.
    - CPU nhanh nhưng bộ nhớ chậm nên CPU phải chờ RAM.
- Hardware Threads:
    - Mỗi lõi vật lý hỗ trợ nhiều luồng (Hyper - threading).
    - Trong khi một luồng đợi, luồng kia có thể thực thi.
    - Hệ điều hành nhìn thấy mỗi luồng phần cứng như một CPU logic.

### 3. Cân bằng tải (Load Balancing):
- Push Migration: Đẩy việc từ CPU bận sang rảnh.
- Pull Migration: CPU nhàn rỗi tự kéo công việc.
- Thách thức: Mẫu thuẫn với Processor Affinity. Processor Affinity là xu hướng:
    - Một tiến trình **nên tiếp tục chạy trên cùng CPU**.
    - Vì dữ liệu của nó đã nằm trong **cache (L1/L2/L3)** của CPU đó:
    - Điều này giúp:
        - Giảm cache miss.
        - Tăng hiệu năng thực tế.

> ![image](https://hackmd.io/_uploads/ryrAUHyvbx.png)
> ![image](https://hackmd.io/_uploads/rJwBDSkwWl.png)

### 4. Processor Affinity:
![image](https://hackmd.io/_uploads/B18WUHywbl.png)
- Hệ điều hành cố gắng giữ tiến trình chạy trên cùng một CPU.
- Lợi ích: Tận dụng dữ liệu sẵn có trong cache.
- Di chuyển sang CPU khác rất tốn kém (cache invalidation).
    - Soft Affinity: Ưu tiên CPU cũ nhưng có thể chuyển.
    - Hard Affinity: Bị ép buộc chạy trên CPU cụ thể.

## VI. Điều phối thời gian thực (Real-Time CPU Scheduling):
Phân loại Real-Time:
- Soft: Chỉ đảm bạo tiến trình ưu tiên chạy trước.
- Hard: Nhiệm vụ phải hoàn thành trong hạn định nghiêm ngặt.

### 1. Minimizing Latency:
- Interrupt Latency:

![image](https://hackmd.io/_uploads/BJUjFByDWg.png)
    - Thời gian từ khi ngắn xảy ra đến khi phục vụ ngắt.
    - Cần cực nhỏ để đáp ứng sự kiện thời gian thực.
- Dispatch Latency Real-Time:

![image](https://hackmd.io/_uploads/rJp-qB1DWx.png)
    - Gồm: Thu hồi tài nguyên + Chuyển ngữ cảnh mới.
    - Dùng kỹ thuật Preemption Points để giảm độ trễ.
    - Dispatch Latency: Khoảng thời gian từ lúc một tác vụ thời gian thực sự sẵn sàng chạy đến khi nó thực sự được CPU thực thi.

### 2. Rate-Monotonic Scheduling:
- Ưu tiên theo chu kỳ: Chu kỳ ngắn thì ưu tiên lớn.
- Giả định: Thời gian thực thi không đổi.
- Tiến trình nào có deadline gần nhất thì được CPU trước.

> ![image](https://hackmd.io/_uploads/BJgT9HkwWl.png)
> ![image](https://hackmd.io/_uploads/SyxCqS1PZx.png)

## VII. Ví dụ hệ điều hành thực tế:
### 1. Linux CFS:
- Completely Fair Scheduler (Nhân 2.6.23).
- Mục tiêu: Chia sẻ CPU công bằng dựa trên Nice value.
- Dùng cấu trúc cây đỏ đen (Red-Black Tree).
- CFS dựa vào **vruntime**.
- Luôn chọn task có vruntime nhỏ nhất.

> ![image](https://hackmd.io/_uploads/HyJq3rkP-l.png)
- Vruntime trong CFS:
    - Theo dõi thời gian thực thi thực tế.
    - Chọn tiến trình có vruntiem nhỏ nhất để chạy.
    - I/O-bound có vruntime nhỏ nên được ưu tiên cao.

> ![image](https://hackmd.io/_uploads/H1sp3BJv-e.png)
- Độ ưu tiên và trọng số:
    - Giá trị Nice (Trọng số dung trong CFS): Từ -20 (cao nhất) đến +19 (thấp nhất).
    - Nice thấp nhận tỷ lệ thời gian CPU cao hơn.
    - Hỗ trợ lớp thời gian thực: `SCHED_FIFO`, `SCHED_RR`.

    ![image](https://hackmd.io/_uploads/H1JJCr1w-x.png)

### 2. Điều phối Windows:
- Hàng đợi phản hồi đa cấp với 32 mức ưu tiên:

![image](https://hackmd.io/_uploads/HJKHASJw-g.png)
    - Mức 16 - 31: Real-time class.
    - Mức 1-15: Variable class (Dynamic).
    - Mức 0: System Idle Thread.
- Cơ chế Priority Boost:
    - Tăng tạm thời ưu tiên cho tiến trình chờ I/O.
    - Tăng ưu tiên cho cửa sổ Foreground.
    - Giảm dần ưu tiên (Decay) khi hết Quantum.

### 3. Solaris Scheduling:
![image](https://hackmd.io/_uploads/ByUfy8kwWl.png)
- Sử dụng các bảng điều phối (Dispatch Tables).
- Lớp IA có mức ưu tiên động giống Windows.
- Lớp Fair Shares (FSS) đảm bảo tài nguyên dựa trên shares.

## VIII. Đánh giá thuật toán:
### 1. Deterministic Modeling:
- Sử dụng khối lượng công việc cụ thể đã biết.
- Tính toán thủ công Waiting/Turnaround time.
- Ưu điểm: Chính xác cho dữ liệu đó.
- Nhược điểm: Không tổng quát.

### 2. Queueing Models:
Dùng lý thuyết xác suất và công thức Little: $$n = \lambda \times W$$ Với:
- $n$: Số trung bình hàng đợi.
- $\lambda$: Tốc độ đến.
- $W$: Thời gian chờ.

### 3. Simulations:
![image](https://hackmd.io/_uploads/HkTmeI1w-x.png)
- Lập trình mô hình máy tính giả lập.
- Dùng biến ngẫu nhiên hoặc tệp vết (trace tapes).
- Phương pháp phổ biến nhất để kiểm tra thuật toán mới.

### 4. Implementation:
- Viết mã và chạy trực tiếp trên hệ điều hành thực.
- Phương pháp đánh giá cuối cùng và chính xác nhất.
- Chi phí cao và rủi ro gây lỗi hệ thống.

## IX. Tóm tắt nội dung cốt lõi:
- Điều phối CPU là trung tâm của đa nhiệm.
- SJF/SRTF tối ưu thời gian chờ nhưng khó thực hiện.
- RR là tiêu chuẩn cho hệ thống tương tác hiện đại.
- MFQ là thuật toán toàn diện nhất.
- Đa CPU yêu cầu cân bằng giữa Tải và Ái lực.
- Thời gian thực dựa trên Deadline và Chu kì.
- Hệ điều hành thực tế kết hợp nhiều kỹ thuật (Priority, RR).
