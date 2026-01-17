## TÓM TẮT CHƯƠNG III:
- Tiến trình gồm Text, Data, Heap và Stack.
- PCB lưu trữ mọi thông tin quản lý.
- Hệ điều hành sử dụng hàng đợi để điều phối.
- Chuyển ngữ cảnh là thao tác then chốt nhưng tốn phí.
- `fork()` và `exec()` tạo tiến trình mới trong UNIX.
- Cần quản lý trạng thái Zombie và Orphan.
- IPC giúp các tiến trình hợp tác.
- Shared memory nhanh, Message passing an toàn/dễ cài đặt.
- Đồng bộ và Đệm là hai yếu tố quan trọng trong IPC.
- Sockets dùng cho giao tiếp mạng qua IP:Port.
- RPC trừu tượng hoá cuộc gọi từ xa.
- Pipes cung cấp kênh dữ liệu giữa các tiến trình.

## I. Khái niệm tiến trình (Process Concept):
### 1. Tiến trình là gì?
![image](https://hackmd.io/_uploads/SJmbNu_Sbx.png)
#### a) Định nghĩa:
- Một chương trình đang thực thi (In execution).
- Là đơn vị công việc trong hệ thống máy tính hiện đại.
- Hệ thống gồm tập hợp các tiến trình user và hệ thống.
- Tiến trình là thực thể chủ động (Active Entity).
- Sử dụng bộ đếm chương trình (Program Counter) để chỉ định lệnh tiếp theo.
- Bao gồm trạng thái hiện tại của hoạt động hệ thống.
#### b) Một tiến trình gồm:
- Text section (program code).
- Data section (chứa global variables).
- Program counter, processor registers.
- Heap section (chứa bộ nhớ cấp phát động).
- Stack section (chứa dữ liệu tạm thời).
    - Function parameters.
    - Return address.
    - Local variables.

#### c) Các bước khởi tạo tiến trình:
- Cấp phát một định danh duy nhất cho tiến trình.
- Cấp pháp không gian nhớ để nạp tiến trình.
- Khởi tạo khối dữ liệu Process Control Block (PCB) cho tiến trình.
- Thiết lập các mối quan hệ cần thiết.

#### a) Chương trình và Tiến trình:
![image](https://hackmd.io/_uploads/SJHyBuOBWe.png)
- Chương trình: Thực thể thụ động (tệp thực thi trên đĩa).
- Tiến trình: Thực thể chủ động (khi tếp được nạp vào bộ nhớ).
- Một chương trình có thể tạo ra nhiều tiến trình khác nhau.
- Chương trình trở thành tiến trình khi một tập tin thực thi được nạp vào bộ nhớ.
> ![image](https://hackmd.io/_uploads/ry4hzHtHZl.png)

#### b) Bố cục bộ nhớ:
![image](https://hackmd.io/_uploads/SJmbNu_Sbx.png)
![image](https://hackmd.io/_uploads/Bku0zrKHZg.png)
- Đoạn văn bản (Text section): Chứa mã lệnh thực thi.
- Đoạn dữ liệu (Data section): Chứa các biến toàn cục.
- Kích thước hai đoạn này là cố định.
- Ngắn xếp (Stack): Chứa dữ liệu tạm thời (tham số hàm, biến cục bộ).
- Vùng nhớ Heap: Bộ nhớ cấp phát động trong thời gian chạy.
- Stack và Heap phát triển ngược chiều nhau.
- Heap cần thiết cho các cấu trúc dữ liệu tạo bằng new hoặc malloc.
- Hệ điều hành quản lý việc ngăn Stack và Heap chồng lấn.
- Mỗi khi gọi một hàm, một "Activation record" được đẩy vào Stack.

### 2. Các trạng thái của tiến trình:
![image](https://hackmd.io/_uploads/rJ5QUduBWx.png)
- `new`: Tiến trình đang được tạo.
- `running`: Các lệnh đang được thực thi.
- `waiting`/`blocked`: Đợi một sự kiện xảy ra (I/O hoặc nhận tín hiệu).
- `ready`: Đợi được cấp phát cho bộ xử lý.
- `terminated`: Đã hoàn thành thực thi, kết thúc.
- Chỉ có một tiến trình chạy trên một lõi CPU tại một thời điểm.
- Chuỗi trạng thái của tiến trình test (trường hợp tốt nhất): `new` $\rightarrow$ `ready` $\rightarrow$ `running` $\rightarrow$ `waiting` (chờ I/O khi gọi `printf`) $\rightarrow$ `ready` $\rightarrow$ `running` $\rightarrow$ `terminated`.
> ![image](https://hackmd.io/_uploads/r1_iEBYSZe.png)

### 3. Process Control Block (PCB):
![image](https://hackmd.io/_uploads/SkinLOuSWx.png)
- Là một trong các cấu trúc dữ liệu quan trọng nhất của hệ điều hành
- Mỗi tiến trình được đại diện bởi một PCB trong nhân.
- Gồm:
    - Trạng thái tiến trình và số hiệu PID.
    - Bộ đếm chương trình (Program Counter).
    - Các thanh ghi CPU (Registers).
    - Thông tin lập lịch: Độ ưu tiên, con trỏ hàng đợi.
    - Thông tin quản lý bộ nhớ: Base/Limit registers hoặc Page tables.
    - Thông tin kế toán: Thời gian CPU đã dùng.
    - Trạng thái I/O: Danh sách tệp đang mở, thiết bị I/O được gán.

### 4. Khái niệm Luồng (Threads):
- Tiến trình truyền thống chỉ có một luồng thực thi đơn lẻ.
- Đa luồng cho phép thực hiện nhiều tác vụ cùng lúc.
- PCB được mở rộng để lưu trữ thông tin cho từng luồng.

## II. Định thời tiến trình (Process Scheduling):
### 1. Mục tiêu điều phối tiến trình:
- Đa chương trình: Luôn có tiến trình chạy để tối đa hiệu suất CPU.
- Chia sẻ thời gian: Chuyển CPU cực nhanh để user tương tác.
- Duy trì các hàng đợi để quản lý tiến trình.
- Các hàng đợi định thời:

    ![image](https://hackmd.io/_uploads/rk9OSStHWx.png)
    - Hàng đợi công việc (Job queue).
    - Hàng đợi sẵn sàng (Ready queue).
    - Hàng đợi thiết bị (Devices queue).
    - ...

#### a) Hàng đợi sẵn sàng (Ready queue):
![image](https://hackmd.io/_uploads/Hyq__uOSZg.png)
- Chứa các tiến trình nằm trong bộ nhớ chính sẵn sàng chạy.
- Thường được lưu trữ dưới dạng danh sách liên kết (Linked list).
- PCB chứa con trỏ đến tiến trình tiếp theo trong hàng.

#### b) Hàng đợi đợi (Wait Queues):
![image](https://hackmd.io/_uploads/Hyq__uOSZg.png)
- Chứa các tiến trình đang đợi thiết bị I/O cụ thể.
- Mỗi thiết bị có một hàng đợi riêng.
- Tiến trình chuyển từ Running sang Wait khi yêu cầu I/O.

#### c) Biểu diễn hàng đợi:
![image](https://hackmd.io/_uploads/Hy-0t_urZl.png)
- Tiến trình mới được đưa vào Ready queue.
- Có thể bị ngắt (Interrupt) hoặc đợi I/O.

### 2. Bộ định thời:
- Phân loại:
    - Bộ định thời công việc (Job scheduler) hay bộ định thời dài (Long - term scheduler).
    - Bộ định thời CPU hay bộ định thời ngắn
- Chọn tiến trình từ Ready queue để cấp phát lõi CPU.
- Thực hiện lựa chọn rất thường xuyên.
- Cần tốc độ cực nhanh để tránh lãng phí tài nguyên.
- Bộ định thời trung gian/vừa: Đôi khi hệ điều hành có thêm medium - term scheduling để điều chỉnh mức độ tối đa chương của hệ thống:
    - Chuyển tiến trình từ bộ nhớ sang đĩa (swap out).
    - Chuyển tiến trình từ đĩa vào bộ nhớ (swap in).
> ![image](https://hackmd.io/_uploads/H1tZvHFHbx.png)

#### a) Điều phối trên Mobile (iOS/Android):
- Hệ thống cũ chỉ chạy một ứng dụng (Foreground).
- Các ứng dụng khác bị treo (Suspended) trong bộ nhớ.
- iOS sử dụng cơ chế Background.
- Android cho phép tiến trình chạy ngầm qua Service.
- Service không có giao diện nhưng vẫn chiếm tài nguyên.
- Hệ thống tự động diệt các tiến trình ít quan trọng khi thiếu bộ nhớ.

#### b) Chuyển ngữ cảnh (Context Switch):
![image](https://hackmd.io/_uploads/Byidh_uBZg.png)
- Xảy ra khi CPU chuyển từ tiến trình này sang tiến trình khác.
- Hệ thống lưu trạng thái cũ và nạp trạng thái mới.
- Ngữ cảnh gồm: Giá trị thanh ghi, trạng thái tiến trình, bộ nhớ.
- Thời gian chuyển ngữ cảnh là thời gian chết (Overhead).
- Tốc độ phụ thuộc vào hỗ trợ phần cứng.
- Một số CPU cung cấp nhiều bộ thanh ghi để chuyển đổi nhanh.
- Càng nhiều dữ liệu cần lưu, thời gian overhead càng lớn.
- Đơn vị quản lý bộ nhớ (MMU) cũng phải được cập nhật.
- Ảnh hưởng hiệu năng hệ thống chia sẻ thời gian.

##### Multicore Scheduling:
- Có thể chạy nhiều tiến trình thực sự song song.
- Mỗi lõi có thể có hàng đợi riêng hoặc dùng chung hàng đợi.
- Phức tạp hơn hệ thống đơn lõi về mặt cân bằng tải.

## III. Thao tác trên tiến trình (Operations):
### 1. Tạo tiến trình:
![image](https://hackmd.io/_uploads/SJ_pCuOHWg.png)

Một tiến trình có thể tạo nhiều tiến trình mới thông qua một lời gọi hệ thống (create - process):
- Tiến trình cha (Parent) tạo tiến trình con (Children), tiến trình con nhận tài nguyên từ hệ điều hành hoặc tiến trình cha.
- Tạo thành một cấu trúc cây tiến trình.
- Mỗi tiến trình có một số hiệu PID duy nhất.
- Cây tiến trình Linux bắt đầu từ `systemd` (PID 1).
- `kthreadd` quản lý các tiến trình nhân.
- `sshd` quản lý các kết nối đăng nhập từ xa.
- Chia sẻ tài nguyên: Cha và con có thể chia sẻ toàn bộ, một phần hoặc không gì cả.
- Thực thi: Cha chạy đồng thời với con hoặc cha đợi con xong.
- Không gian địa chỉ con:
    - Được nhân bản từ cha.
    - Khởi tạo từ template.
> Ví dụ trong UNIX/Linux:
> - System call `fork()` tạo một tiến trình mới.
> - System call `exec()` để nạp một chương trình mới vào không gian nhớ của tiến trình mới.

#### a) Lời gọi `fork()` trong UNIX:
![image](https://hackmd.io/_uploads/r1E6JFuS-e.png)
- Tạo ra một tiến trình mới là bản sao của cha:
- Trả về 0 cho tiến trình con.
- Trả về PID của con (giá trị >0) cho tiến trình cha.
> ![image](https://hackmd.io/_uploads/rkuV_HtBZg.png)
> ![image](https://hackmd.io/_uploads/rJuSdSKHbx.png)

#### b) Lời gọi `exec()` trong UNIX:
- Sử dụng sau `fork()` để thay thế bộ nhớ tiến trình bằng chương trình mới.
- Nạp tệp thực thi vào bộ nhớ và bắt đầu chạy.
- Không trả về nếu thực hiện thành công.
> Ví dụ mã nguồn:
> 
> ![image](https://hackmd.io/_uploads/BJqNxtdBWe.png)

#### c) `CreateProcess()` trong Windows:
- Khác với UNIX, Windows yêu cầu chương trình cụ thể ngay khi tạo.
- Sử dụng cấu trúc `STARTUPINFO` VÀ `PROCESS_INFORMATION`.
- Có hơn mười tham số truyền vào hàm `CreateProcess()`.

### 2. Kết thúc tiến trình:
![image](https://hackmd.io/_uploads/S1WHZY_Hbl.png)
- Tiến trình hoàn thành lệnh cuối và dùng lời gọi `exit()`.
- Tiến trình kết thúc do tiến trình khác:
    - Dữ liệu trạng thái được trả về cho cha (qua `wait()`).
    - Cha kết thúc con (Gọi system routine abort với tham số là PID - Process Indentifier) nếu con dùng quá tài nguyên hoặc khi tác vụ giao cho con không còn cần thiết hoặc cha kết thúc và hệ điều hành không cho con chạy tiếp.
- Hệ điều hành giải phóng bộ nhớ, tệp đang mở, bộ đệm.

#### a) Cascadung Termination:
- Xảy ra trong một số hệ thống khi tiến trình cha thoát.
- Tất cả các con, cháu của nó đều bị hệ thống chấm dứt.
- Hiện tượng này được khởi xướng bởi hệ điều hành.

#### b) Tiến trình Zombie:
- Xảy ra khi con đã thoát nhưng cha vẫn chưa gọi `wait()`.
- Tiến trình vẫn nằm trong bảng tiến trình của nhân.
- Tất cả tiến trình đều trải qua trạng thái Zombie ngắn ngủi.

#### c) Tiến trình Orphan (Mồ côi):
- Xảy ra khi cha thoát và chưa gọi `wait()`, con vẫn đang thực thi.
- Linux giải quyết bằng cách gán init (PID 1) làm cha mới cho chúng.

#### d) Android Process Management:
Phân cấp độ ưu tiên để giải phóng bộ nhớ:
- Foreground proces: Ứng dụng đang dùng (ưu tiên cao nhất).
- Visible process: Hiển thị nhưng không tương tác.
- Service process: Chạy ngầm (như phát nhạc).
- Background process: Đã đóng nhưng vẫn trong bộ nhớ.
- Empty process: Không có thành phần nào chạy (ưu tiên thấp nhất).

## IV. Giao tiếp liên tiến trình (IPC):
### 1. Tại sao cần IPC?
IPC (Inter Process Communication) là cơ chế cung cấp bởi hệ điều hành nhằm giúp các tiến trình:
- Hợp tác chia sẻ thông tin.
- Tăng tốc tính toán bằng cách chia nhỏ tác vụ.
- Tính module hoá hệ thống.
- Sự tiện lợi cho user.
- Đồng bộ hoạt động.

### 2. Mô hình IPC:
![image](https://hackmd.io/_uploads/HkEeNt_S-l.png)
#### a) Bộ nhớ được chia sẻ - Shared Memory:
- Một vùng nhớ dùng chung (Chia sẻ chung) giữa các tiến trình cần giao tiếp với nhau.
- Quá trình giao tiếp được thực hiện dưới sự điều khiển của các tiến trình, không phải của hệ điều hành.
- Cần có cơ chế đồng bộ hoạt động của các tiến trình khi chúng cùng truy xuất bộ nhớ chung.
- Tốc độ cao, do lập trình viên quản lý đồng bộ.

#### b) Hệ thống truyền thông điệp - Message Passing:
- Nhân quản lý, phù hợp lượng dữ liệu nhỏ. Thường dùng trong hệ thống phân tán.
- Làm thế nào các tiến trình giao tiếp với nhau?

##### Producer - Consumer Problem:
![image](https://hackmd.io/_uploads/HyCU4tOH-e.png)
- Producer tạo thông tin, Consumer tiêu thụ nó.
- Tạo vùng đệm (Buffering): Dùng queue để tạm chứa các message:
    - Unbounded buffer: Không giới hạn kích thước, không bao giờ đợi.
    - Bounded buffer: Kích thước cố định.
    - Khả năng chứa là 0 (Zero capacity hay no buffering), phải đợi nhau.

##### Message Passing:
![image](https://hackmd.io/_uploads/HyCU4tOH-e.png)
- Hai thao tác: `send(message)`, `receive(message)`
- Kích thước tin nhắn: Cố định hoặc thay đổi.
- Cần thiết lập liên kết giao tiếp (Communication Link).

###### Định danh:
- Giao tiếp trực tiếp: Phải ghi rõ tên tiến trình nhận/gửi.
- Giao tiếp gián tiếp: Thông qua Mailboxes hoặc Ports.
- Mailbox có thể thuộc sở hữu của tiến trình hoặc hệ điều hành.

###### Đồng bộ:
- Blocking (Đồng bộ): Bên gửi/nhận phải đợi.
- Non-blocking (Bất đồng bộ): Bên gửi/nhận tiếp tục thực hiện.
- Rendezvous: Khi cả send và receive đều là blocking.

### 6. Ví dụ về các hệ thống IPC:
#### a) POSIX Shared Memory:
![image](https://hackmd.io/_uploads/HyeH8YdHZe.png)
- Tạo đối tượng: `shm_open()`
- Thiết lập kích thước: `ftruncate()`
- Ánh xạ vào bộ nhớ: `mmap()`
- Ghi dữ liệu: `sprintf()`
- Đọc dữ liệu: Dùng con trỏ bộ nhớ.
- Xoá đối tượng: `shm_unlink()`

#### b) Hệ điều hành Mach (Truyền tin):
![image](https://hackmd.io/_uploads/rJsnUYuHbg.png)
- Dựa trên cổng (ports).
- Hầu như mọi giao tiếp là truyền tin.
- Sử dụng cơ chế copy-on-write để tối ưu hiệu năng.

#### c) Windows Local Procedure Call:
![image](https://hackmd.io/_uploads/rJQZDYdHbe.png)
- Giao tiếp giữa các tiến trình trên cùng một máy.
- Sử dụng các cổng kết nối và cổng giao tiếp.
- Sử dụng bộ nhớ chia sẻ cho các tin nhắn lớn.

#### d) Khái niệm Pipes:
![image](https://hackmd.io/_uploads/rk5pOtdSWg.png)
- Kênh giao tiếp cho hai tiến trình.
- Dữ liệu chảy một chiều hoặc hai chiều.
- Cần xác định mối quan hệ giữa các tiến trình.

##### Ordinary Pipes:
- Chỉ chảy một chiều (Unidirectional).
- Yêu cầu quan hệ cha con.
- Tự động biến mất khi các tiến trình kết thúc.

##### Named Pipes:
- Không yêu cầu quan hệ cha - con.
- Có thể giao tiếp hai chiều.
- Tồn tại trên hệ thống tệp ngay cả khi tiến trình thoát.

##### So sánh Pipes (UNIX & Windows):
- UNIX Pipe: Sử dụng hàm `pipe()`, `read()`, `write()`.
- Windows Pipe: Phân biệt Anonymous và Named pipes.
- Windows hỗ trợ giao tiếp qua mạng và Named pipes.
> Ví dụ Pipe trong C:
> 
> ![image](https://hackmd.io/_uploads/BybRYt_Sbl.png)

## V. Giao tiếp trong hệ thống Client - Server:
### 1. Giao tiếp qua Sockets:
- Là một điểm cuối (Endpoint) cho giao tiếp.
- Kết hợp giữa địa chỉ IP và số hiệu cổng.
> `161.25.19.8:1625`
> 
> ![image](https://hackmd.io/_uploads/rkPIFEtSWe.png)
- Các cổng dưới `1024` là cổng chuẩn (Well-known).
- TCP Sockets: Hướng kết nối (Connection - oriented).
- UDP Sockets: Không kết nối (Connectionless).

### 2. Remote Procedure Calls:
![image](https://hackmd.io/_uploads/HyhmoNKSWg.png)
- Trừu tượng hoá việc gọi hàm qua mạng.
- Sử dụng các Stubs (Client - side và Server - side).
- Marshaling: Đóng gói tham số vào gói tin.
- Xử lý khác biệt về biểu diễn dữ liệu (Big - endian & Little - endien).
- Dùng XDR (External Data Representation).
- Đảm bảo ngữ nghĩa gọi hàm "Chính xác một lần".
- RCP trên Android (Binder):
    - Sử dụng AIDL (Android Interface Definition Language).
    - Cho phép các ứng dụng giao tiếp với Service của hệ thống.
    - Binder quản lý việc truyền dữ liệu giữa các tiến trình.

## VI. Tiểu trình:
### 1. Tổng quan về tiểu trình:
![image](https://hackmd.io/_uploads/r1wuiSFrbx.png)
- Là một đơn vị cơ bản dùng CPU gồm Thread ID, PC, Registers, Stack và chia sẻ chung code, data, resources (files).
> ![image](https://hackmd.io/_uploads/r1vKoSKBZg.png)
- Lợi ích của tiến trình đa luồng:
    - Đáp ứng nhanh, cho phép chương trình tiếp tục thực thi khi một bộ phận bị khoá hoặc một hoạt động dài.
    - Chia sẻ tài nguyên, tiết kiệm không gian nhớ.
    - Kinh tế: Tạo và chuyển ngữ cảnh nhanh hơn tiến trình.
    - Trong multiprocessor: Có thể thực hiện song song.
> ![image](https://hackmd.io/_uploads/ry01nrYHZg.png)
> ![image](https://hackmd.io/_uploads/SkplnrtHWe.png)

### 2. Các mô hình đa tiểu trình:
#### a) Mô hình Nhiều - Một (Many-to-One):
![image](https://hackmd.io/_uploads/HySd2HtBWe.png)
- Nhiều tiểu trình người dùng được ánh xạ đến một tiểu trình hạt nhân.
- Một tiểu trình bị block sẽ dẫn đến tất cả tiểu trình bị block.
- Các tiểu trình không thể chạy song song trên các hệ thống đa lõi vì chỉ có một tiểu trình có thể truy xuất nhân tại một thời điểm.
- Rất ít hệ thống dùng mô hình này.

#### b) Mô hình Một - Một (One-to-One):
![image](https://hackmd.io/_uploads/r1fg6BYrWe.png)
- Mỗi tiểu trình người dùng ứng với một tiểu trình hạt nhân.
- Tạo một tiểu trình người dùng cũng đồng thời tạo một tiểu trình hạt nhân.
- Tính đồng thời (Concurrency) tốt hơn mô hình Nhiều - Một vì các tiểu trình khác vẫn hoạt động bình thường khi một tiểu trình bị block.
- Nhược điểm: Số lược tiểu trình của mỗi tiến trình có thể bị hạn chế.
- Nhiều hệ điều hành dùng (Windows, Linux).

#### c) Mô hình Nhiều - Nhiều (Many-to-Many):
![image](https://hackmd.io/_uploads/S1tSTHKSbg.png)
- Các tiểu trình người dùng được ánh xạ với nhiều tiểu trình hạt nhân.
- Cho phép hệ điều hành tạo đủ số lượng tiểu trình hạt nhân $\Rightarrow$ Giải quyết được hạn chế của hai mô hình trên.
- Khó cài đặt nên ít phổ biến.
