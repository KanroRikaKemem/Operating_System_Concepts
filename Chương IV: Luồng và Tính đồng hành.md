## Tóm tắt chương:
- Luồng là đơn vị sử dụng CPU cơ bản, chia sẻ tài nguyên tiến trình.
- Bốn lợi ích: Tính đáp ứng, chia sẻ tài nguyên, kinh tế, khả năng mở rộng.
- Hệ thống đa lõi đòi hỏi thuật toán lập lịch song song hiệu quả.
- Phân tích giữa tính Đồng hành (xen kẽ) và Song song (đồng thời).
- Ba mô hình đa luồng chính: Many-to-One, One-to-One, Many-to-Many
- Thư viện luồng: Windows, Pthreads, Java cung cấp API tạo và quản lý.
- Đa luồng ngầm định: Thread pools, Fork-join, OpenMP, GCD giúp giảm gánh nặng cho lập trình viên.
- Huỷ luồng có thể theo kiểu không đồng bộ hoặc có trì hoãn (An toàn hơn).
- TLS cung cấp dữ liệu riêng cho từng luồng.
- Linux coi cả tiến trình và luồng là các tasks linh hoạt.

## I. Tổng quan về luồng:
### 1. Luồng là gì?:
![image](https://hackmd.io/_uploads/B1Qesk0r-g.png)
- Là đơn vị cơ bản nhất đề dùng CPU.
- Gồm: Thread ID, PC, tập thanh ghi và ngăn xếp (stack).
- Chia sẻ với các luồng cùng tiến trình: code, dữ liệu và tài nguyên (files, signals).
- Tiến trình đa luồng có thể thực hiện nhiều tác vụ cùng lúc.

### a) Động lực:
#### Ứng dụng phần mềm:
- Hầu hết phần mềm hiện đại chạy trên PC/di động là đa luông.
- Ứng dụng tạo ảnh thu nhỏ (thumbnails): Mỗi ảnh được xử lý bởi một luồng riêng.
- Trình duyệt Web: Một luồng hiển thị nội dung, luồng khác tải dữ liệu từ mạng.
- Xử lý văn bản: Luồng đồ hoạ, luồng nhận phím, luồng kiểm tra chính tả chạy ngầm.

#### Web Server và Tính đa luồng:
- Server đơn luồng chỉ phục vụ một khách tại thời điểm.
- Khách hàng khác phải chờ đợi rất lâu.
- Giải pháp cũ: Tạo tiến trình mới (tốn kém thời gian và tài nguyên).
- Giải pháp mới: Tạo luồng mới cho mỗi request, server tiếp tục lắng nghe.

#### Kiến trúc Server đa luồng:
![image](https://hackmd.io/_uploads/Skec2kCHWg.png)
- B1: Nhận yêu cầu từ client.
- B2: Tạo luồng mới phục vụ yêu cầu.
- B3: Tiếp tục nghe yêu cầu khác.

#### Hạt nhân (Kernel) đa luồng:
- Hầu hết các kernel hệ điều hành hiện nay đều đa luồng.
- Linux: Tạo nhiều luồng Kernel khi khởi động hệ thống.
- Tác vụ: Quản lý thiết bị, bộ nhớ, xử lý ngắt.
- Lệnh `ps -ef` giúp xem các luồng hạt nhân trên Linux (`kthreadd` có PID = 2).

### b) Lợi ích:
#### Tính đáp ứng:
- Cho phép chương trình tiếp tục chạy khi một phần bị chặn (blocked).
- Đặc biệt hữu ích trong thiết kế giao diện người dùng (User Interface).
> Click nút thực hiện tác vụ nặng, luồng riêng xử lý giúp UI không bị đơ.
- Ứng dụng vẫn phản hồi tốt với user.

#### Chia sẻ tài nguyên:
- Tiến trình chỉ có thể chia sẻ qua Shared Mememory hoặc Message Passing.
- Luồng chia sẻ bộ nhớ và tài nguyên của tiến trình cha theo mặc định.
- Cho phép nhiều luồng hoạt động trong cùng một không gian địa chỉ.
- Giúp việc trao đổi dữ liệu giữa các thực thể cực kỳ thuận tiện.

#### Tính kinh tế:
- Cấp phát tài nguyên cho tiến trình (Process Creation) rất tốn kém.
- Tạo luồng ít tốn thời gian và bộ nhớ hơn nhiều.
- Chuyển đổi ngữ cảnh (Context switching) giữa các luồng nhanh hơn giữa tiến trình.
- Luồng tận dụng các cấu trúc dữ liệu đã có của tiến trình cha.

#### Khả năng mở rộng:
- Đặc biệt quan trọng trong kiến trúc đa xử lý (Multiprocessor).
- Luồng có thể chạy song song thực sự trên các lõi khác nhau.
- Tiến trình đơn luồng chỉ cạy trên một CPU dù máy có bao nhiêu lõi.
- Tăng cường đáng kể hiệu năng cho các tác vụ tính toán nặng.

## II. Lập trình đa lõi (Multicore):
#### Xu hướng hệ thống đa lõi:
![image](https://hackmd.io/_uploads/SkAayxRBbl.png)
- Tiến hoá từ hệ thống đơn CPU sang hệ thống đa CPU.
- Đặt nhiều lõi tính toán trên một chip xử lý duy nhất.
- Hệ điều hành nhìn nhận mỗi lõi như một CPU riêng biệt.
- Đa luồng giúp sử dụng hiệu quả các lõi này và tăng tính đồng hành.
- Tính đồng hành (Concurrency): Hỗ trợ nhiều tác vụ bằng cách cho phép chúng cùng tiến triển.
- Tính song song (Parallelism): Có thể thực hiện nhiều tác vụ cùng lúc.
- Hệ thống đơn lõi chỉ có tính đồng hành (nhờ chuyển đổi nhanh giữa các luồng).
- Hệ thống đa lõi mới có tính song song thực thụ.

> ![image](https://hackmd.io/_uploads/BkD_egABbl.png)

### 1. Thách thức lập trình:
- Nhận diện tác vụ (Identifying tasks): Chia ứng dụng thành các phần độc lập có thể chạy song song.
- Tính cân bằng (Balance): Đảm bảo các tác vụ thực hiện khối lượng công việc có giá trị tương đương.
- Chia tách dữ liệu (Data spliting): Dữ liệu phải được chia sẻ để phù hợp với các tác vụ chạy trên lõi khác nhau.
- Sự phụ thuộc dữ liệu (Data dependency): Đảm bảo các tác vụ phụ thuộc dữ liệu của nhau được đồng bộ hoá.
- Kiểm thử và gỡ lỗi: Nhiều lộ trình thực thi khác nhau làm tăng độ khó khi gỡ lỗi.
- Yêu cầu cách tiếp cận mới trong thiết kế hệ thống phần mềm và giáo dục Công nghệ Thông tin.
> Định luật Amdahl:
> - Công thức tính Speedup tối đa khi thêm lõi xử lý: $$speedup \leq \frac{1}{S + \frac{1 - S}{N}}$$ Với:
>    - $S$: Phần của ứng dụng bắt buộc phải thực hiện nối tiếp (serially).
>    - $N$: Số lượng lõi xử lý (processing cores).
> 
> ![image](https://hackmd.io/_uploads/HJPOExAHWg.png)
> - Hệ quả:
>    - Nếu 50% chạy nối tiếp, Speedup tối đa chỉ là 2.0 dù có bao nhiêu lõi.
>    - Phần nối tiếp có ảnh hưởng không cân xứng đến hiệu năng cải thiện được.
>    - $N$ tăng đến vô cực, Speedup hội tụ về $\frac{1}{S}$.
>    - Thách thức: Giảm thiểu tối đa phần nối tiếp ($S$) trong thuật toán.

### 2. Song song dữ liệu (Data Parallelism):
![image](https://hackmd.io/_uploads/SJKAUxCS-g.png)
- Tập trung phân phối các tập con của cùng một dữ liệu trên nhiều lõi.
- Thực hiện cùng một thao tác trên mỗi lõi.
> Cộng mảng kích thước $N$. Core 0 cộng từ $[0]$ đến $[\frac{N}{2} - 1]$, Core 1 cộng từ $[\frac{N}{2}]$ đến $[N - 1]$.
- Tận dụng tối đa sức mạng khi dữ liệu có thể chia nhỏ độc lập.
- Phân phối các tác vụ (luồng) khác nhau trên nhiều lõi.
- Mỗi luồng thực hiện một hoạt động duy nhất (unique operation).
- Các luồng có thể hoạt động trên cùng một dữ liệu hoặc dữ liệu khác nhau.
> Một luồng tính giá trị trung bình, luồng khác tính giá trị lớn nhất của mạng.

## III. Các mô hình đa luồng:
![image](https://hackmd.io/_uploads/SyBtwg0SWe.png)
- User Threads: Được hỗ trợ phía trên hạt nhân, quản lý không cần sự can thiệp của hạt nhân.
- Kernel Threads: được hỗ trợ và quản lý trực tiếp bởi hệ điều hành.
- Mối quan hệ ánh xạ giữa hai loại này quyết định cách luồng được thực thi.
- Hệ điều hành hiện đại (Windows, Linux, macOS) đều hỗ trợ luồng Kernel.

### 1. Mô hình Many-to-One:
![image](https://hackmd.io/_uploads/Skvv_e0rWl.png)
- Ánh xạ nhiều luồng người dùng vào một luồng hạt nhân duy nhất.
- Quản lý bởi thư viện luồng trong không gian người dùng (hiệu quả).
- Nhược điểm: Toàn bộ tiến trình bị chặn (blocked) nếu một luồng gọi system call chặn.
- Không thể chạy song song trên đa lõi (chỉ một luồng được vào kernel tại một thời điểm).

### 2. Mô hình One-to-One:
![image](https://hackmd.io/_uploads/HyseKeAHbl.png)
- Mỗi luồng người dùng ánh xạ vào một luồng hạt nhân tương ứng.
- Tính đồng hành cao: Luồng khác vẫn chạy được khi một luồng bị chặn.
- Cho phép chạy song song thực sự trên đa xử lý.
- Nhược điểm: Tạo quá nhiều luồng người dùng sẽ gây áp lực lên hiệu năng hệ thống.
- Được dùng bởi Windows, Linux.

### 3. Mô hình Many-to-Many:
![image](https://hackmd.io/_uploads/HyP8YlCH-l.png)
- Ghép nhiều luồng người dùng vào một số lượng ít hơn hoặc bằng luồng hạt nhân.
- Linh hoạt nhất: Người dùng tạo bao nhiêu luồng tuỳ ý, kernel điều phối song song.
- Khi một luồng bị chặn, kernel có thể lập lịch cho luồng hạt nhân khác.
- Khó hiện thực trong thực tế.

#### Mô hình Two-level:
![image](https://hackmd.io/_uploads/Sy0aFgCHZl.png)
- Biến thể của mô hình Many-to-Many.
- Cho phép ghép nhiều luồng người dùng vào số lượt ít hơn luồng hạt nhân.
- Đặc biệt: Vẫn cho phép RÀNG BUỘC một luồng người dùng vào một luồng hạt nhân cụ thể.
- Kết hợp ưu điểm của cả One-to-One và Many-to-Many.

## IV. Thư viện luồng (Thread Libraries):
#### Các loại thư viện Luồng:
- Cách 1: Thư viện hoàn toàn trong User Space (không có sự hỗ trợ của Kernel).
- Cách 2: Thư viện cấp Kernel được hỗ trợ trực tiếp bởi hệ điều hành.
- Ba thư viện chính: POSIX Pthreads, Windows và Java Threads.
- Java thường dùng thư viện của hệ điều hành chủ (Windows API hoặc Pthreads).

#### Luồng không đồng bộ:
- Asynchronous Threading: Cha tạo luồng con và TIẾP TỤC thực thi.
- Cha và con thực thi đồng hành và độc lập.
- Rất ít sự chia sẻ dữ liệu giữa các luồng.
- Ứng dụng: Thiết kế Server đa luồng, thiết kế UI đáp ứng nhanh.

#### Luồng đồng bộ:
- Synchronous Threading: Cha tạo một hoặc nhiều con và PHẢI CHỜ con kết thúc.
- Chiến lược "Fork-join": Cha chờ con thực hiện xong tác vụ mới tiếp tục.
- Thường có sự chia sẻ dữ liệu đáng kể giữa các luồng.
> Cha chia tác vụ tính toán cho các con, sau đó thu thập kết quả để tổng hợp.

### 1. Pthreads (POSIX Threads):
- Tiêu chuẩn IEEE 1003.1c định nghĩa API cho việc tạo và đồng bộ luồng.
- Là một đặc tả (specification), không phải một bản thực thi cụ thể.
- Dùng phổ biến trong các hệ thống UNIX, Linux, macOS.
- Các luồng bắt đầu thực thi tại một hàm được chỉ định (hàm `runner`).

### 2. Windows Thread API:
- Sử dụng thư viện `windows.h`.
- Dữ liệu toàn cục được chia sẻ giữa các luồng trong cùng tiến trình.
- Hàm tạo: `CreateThread()`
- Hàm đồng bộ: `WaitForSingleObject()` hoặc `WaitForMultipleObjects()`

### 3. Java Threads:
- Mọi chương trình Java đều có ít nhất một luồng (hàm `main`).
- Hai kỹ thuật tạo luồng: Kế thừa lớp Thread hoặc hiện thực interface Runnable.
- Gọi phương thức `start()` để tạo luồng (phương thức này sẽ tự gọi `run()`).
- Phương thức `join()` dùng để đồng bộ cha chờ con.

## V. Đa luồn ngầm định (Implicit Threading):
Tại sao cần Luồng ngầm định?
- Khi ứng dụng có hàng trăm/nghìn luồng, quản lý thủ công là cực kỳ khó khăn.
- Chuyển trách nhiệm quản lý cho trình biên dịch và thư viện `runtime`.
- Lập trình viên chỉ cần xác định nhiệm vụ (Task), không phải xác định Luồng (Thread).
- Thư viện tự quyết định việc tạo, quản lý và ánh xạ nhiệm vụ vào luồng.

### 1. Thread Pools:
- Tạo một số lượng luồng nhất định khi khởi động và đặt chúng vào pool.
- Khi nhận yêu cầu mới, lấy một luồng rảnh từ pool để phục vụ.
- Nếu pool không có luồng rảnh, công việc sẽ được đưa vào hàng đợi.
- Khi hoàn thành, luồng quay lại pool để đợi công việc tiếp theo.
- Phục vụ yêu cầu nhanh hơn (luồng đã có sẵn, không mất công tạo).
- Giới hạn số luồng tối đa (Tránh làm cạn kiệt CPU hoặc bộ nhớ).
- Tách biệt nhiệm vụ khỏi cơ chế tạo tác vụ (cho phép các chiến lược chạy khác nhau).
- Hệ thống có thể tự động điều chỉnh kích thước pool theo tải (như GCD của Apple).

### 2. Mô hình Fork - Join ngầm định:
![image](https://hackmd.io/_uploads/HJo5bWABWl.png)
- Phiên bản đồng bộ của Thread Pools.
- Thư viện quản lý số lượng luồng và tự động gán nhiệm vụ.
- Java Fork/Join library (từ v1.7): Dùng cho các thuật toán đệ quy chia để trị.
- Kỹ thuật "Work stealing": Luồng rảnh có thể "đánh cắp" nhiệm vụ từ hàng đợi của luồng bận.

### 3. Thư viện OpenMP:
![image](https://hackmd.io/_uploads/ByC7zbCSZx.png)
- Bộ các chỉ thị trình biện dịch và API cho C, C++, FORTRAN.
- Dùng cho môi trường bộ nhớ dùng chung (Shared Memory).
- Xác định vùng song song bằng directive: `#pragma omp parallel`.
- Tự động tạo số luồng bằng số lượng lõi xử lý trong hệ thống.

### 4. Grand Central Dispatch (Apple):
- Kết hợp thư viện runtime, API và mở rộng ngôn ngữ cho macOS/iOS.
- Quản lý hầu hết các chi tiết của việc lập luồng.
- Sử dụng Dispatch Queues: Serial (nối tiếp - FIFO) và Concurrent (đồng thời - FIFO).
- Có bốn cấp độ Quality-of-Service (QoS): User-interactive, User-initiated, Utility, Background.

### 5. Intel Thread Building Blocks (Intel TBB):
- Thư viện template hỗ trợ thiết kế ứng dụng song song bằng C++.
- Không yêu cầu hỗ trợ đặc biệt từ trình biên dịch hay ngôn ngữ.
- Lập trình viên chỉ định tasks, TBB scheduler ánh xạ vào luồng.
- Cung cấp cấu trúc loop song song, thao tác nguyên tử (atomic) và đồng bộ.

## VI. Các vấn đề về luồng (Threading Issues):
### 1. Lệnh `fork()` và `exec()`:
- Nếu một luồng gọi `fork()`, một số hệ thống UNIX có hai phiên bản `fork`:
    - Nhân bản toàn bộ các luồng của tiến trình.
    - Chỉ nhân bản luồng đã gọi fork.
- Nếu gọi `exec()` ngay sau `fork()`, thường chỉ cần nhân bản luồng hiện tại vì `exec` sẽ thay thế toàn bộ tiến trình.

### 2. Xử lý tín hiệu (Signal Handling):
- Tín hiệu dùng để thông báo cho tiến trình một sự kiện đã xảy ra.
- **Tín hiệu đồng bộ**: Gửi cho chính tiến trình gây ra (lỗi chia cho 0, truy cập bộ nhớ bất hợp pháp).
- **Tín hiệu không đồng bộ**: Đến từ bên ngoài tiến trình (`Ctrl` + `C`, hết hạn Timer).
- Có hai trình xử lý: Mặc định (Default) và Do người dùng định nghĩa (User-defined).
- Gửi tín hiệu đa luồng ở đâu?
    - Gửi đến luồng mà tín hiệu đó áp dụng (tín hiệu đồng bộ).
    - Gửi đến mọi luồng trong tiến trình (như lệnh thoát tiến trình).
    - Gửi đến một số luồng nhất định trong tiến trình.
    - Chỉ định một luồng cụ thể nhận mọi tín hiệu của tiến trình.

### 3. Huỷ luồng (Thread Cancellation):
![image](https://hackmd.io/_uploads/S14zLW0r-l.png)
- Chấm dứt luồng trước khi nó hoàn tất.
> Trang web đang tải, bấm `Stop` $\rightarrow$ Các luồng tải ảnh bị huỷ.
- Luồng bị huỷ gọi là Target Thread.
- Khó khăn: Giải phóng tài nguyên và cập nhật dữ liệu dùng chung an toàn.
- Kịch bản huỷ luồng:
    - **Asynchronous cancellation**: Một luồng chấm dứt ngay lập tức luồng mục tiêu (nguy hiểm cho tài nguyên).
    - **Deferred cancellation**: Luồng mục tiêu định kỳ kiểm tra xem mình có nên dừng hay không.
    - Pthreads hỗ trợ cả hai: Default là Deferred.
    - Luồng chỉ bị huỷ khi đặt đến "Cancellation point" (Điểm an toàn).

### 4. Thread - Local Storage (TLS):
- Luồng chia sẻ dữ liệu tiến trình nhưng đôi khi cần bản sao dữ liệu riêng.
- T;S khác với biến cục bộ (Local variables) ở chỗ nó tồn tại xuyên suốt các lần gọi hàm.
- Giống như dữ liệu tĩnh (static data) nhưng duy nhất cho mỗi luồng.
> Gán ID giao dịch duy nhất cho mỗi luồng xử lý giao dịch.

### 4. Lightweight Process (LWP):
![image](https://hackmd.io/_uploads/HyQozGRrWg.png)
- Dùng cho mô hình Many-to-Many và Two-level.
- LWP là cấu trúc trung gian, đóng vai trò như "Bộ xử lý ảo".
- Hệ điều hành lập lịch cho luồng Kernel chạy trên CPU thực.
- LWP gắn với luồng Kernel, nếu Kernel Thread block, LWP block theo.
- Cơ chế Upcall:
    - Giao tiếp giữa Kernel và thư viện luộng để duy trì số lượng luồng kernel tối ưu.
    - Khi một luồng sắp bị chặn, Kernel gửi một **Upcall** cho thư viện luồng.
    - Upcall Handler (chạy trên LWP mới) lưu trạng thái luồng bị chặn và lập lịch cho luồng khác.
    - Khi sự kiện gây chặn kết thúc, Kernel lại gửi upcall thông báo luồng đã sẵn sàng.

## VII. Ví dụ về hệ điều hành:
### 1. Windows Threads:
![image](https://hackmd.io/_uploads/B1BJNMASWg.png)
- Ánh xạ One-to-One.
- Thread Context gồm: Register set, Program Counter, User/Kernel stack, Private storage area.
- Cấu trúc dữ liệu chính: ETHREAD (Executive block), KTHREAD (Kernel block), TEB (Environment block).
- ETHREAD và KTHREAD nằm trong Kernel Space, TEB nằm trong User Space.

### 2. Linux Threads:
- Linux dùng hệ thống lệnh `fork()` và `clone()`.
- Linux không phân biệt tiến trình và luồng, gọi chung là task.
- Lệnh `clone()` nhận một tập hợp các flags để quyết định mức độ chia sẻ.
- Cờ quan trọng: `CLONE_FS`, `CLONE_VM`, `CLONE_SIGHAND`, `CLONE_FILES`.
- Dùng cấu trúc `struct task_struck` trong Kernel.
- Chứa các con trỏ trỏ đến các cấu trúc dữ liệu khác (Files, Memory, Signals).
- Nếu các cờ clone được thiết lập, tiến trình con sẻ trỏ chung vào cấu trúc của cha.
- Sự linh hoạt này cho phép Linux hỗ trợ cả Container (Ảo hoá mức OS).
