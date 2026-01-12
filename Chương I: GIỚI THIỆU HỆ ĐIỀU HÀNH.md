## TÓM TẮT CHƯƠNG I:
- OS quản lý tài nguyên và điều khiển máy tính.
- Cơ chế ngắt là nền tảng phần cứng cho OS.
- Đa xử lý và phân cụm tăng hiệu suất, tin cậy.
- Dual - mode bảo vệ hệ thống khỏi sai sót.
- Tiến trình, bộ nhớ, tệp là chức năng cốt lõi.
- Ảo hoá và đám mây là xu hướng hiện đại.

## I. Tổng quan về hệ điều hành:
### 1. Tổng quan:
#### a) Định nghĩa Hệ điều hành:
- Là phần mềm quản lý phần cứng máy tính.
- Đóng vai trò trung gian giữa người dùng và phần cứng.
- Chức năng:
    - Điều khiển và phối hợp việc sử dụng phần cứng.
    - Cung cấp các dịch vụ cơ bản cho các ứng dụng.
- Mục tiêu:
    - Giúp user dễ dàng sử dụng hệ thống.
    - Quản lý và cấp phát tài nguyên hệ thống một cách hiệu quả.
#### b) Cấu trúc hệ thống máy tính:
![image](https://hackmd.io/_uploads/Sk8LIeaEWl.png)
- Phần cứng: CPU, bộ nhớ, thiết bị I/O,...
- Hệ điều hành: Phân phối tài nguyên, điều khiển và phối hợp các hoạt động của các chương trình trong hệ thống.
- Chương trình ứng dụng: Giải quyết bài toán của user (Trình soạn thảo, trình duyệt trò chơi,...)
- User: Con người, máy tính khác hoặc thiết bị.

### 2. User View & System View:
#### a) User View:
##### PC và Workstation:
- User chiếm giữ hoàn toàn tài nguyên.
- Mục tiêu: Tối đa hoá sự tiện lợi và dễ dàng sử dụng.
- Hiệu suất sử dụng tài nguyên không ưu tiên hàng đầu.

##### Mainframe/Minicomputer:
- Nhiều người dùng kết nối qua endpoint.
- Chia sẻ tài nguyên chung của hệ thống.
- Mục tiêu: Tối ưu hoá việc tận dụng tài nguyên.

##### Mobile Devices:
- Sử dụng giao diện màn hình cảm ứng, cử chỉ.
- Hỗ trợ kết nối không dây mạnh mẽ.
- Tối ưu hoá thời lượng pin và trải nghiệm.

##### Embedded Systems:
- Máy tính trong oto, thiết bị gia dụng.
- Thường không có giao diện user trực tiếp.
- Chạy tự động không cần sự can thiệp.

#### n) System View:
##### Resource Allocator:
- OS quản lý CPU, lưu trữ, I/O.
- Giải quyết các yêu cầu tài nguyên xung đột.
- Đảm bảo hoạt động hiệu quả và công bằng.

##### Control Program:
- Quản lý việc thực thi chương trình user.
- Ngăn chặn lỗi và việc sử dụng máy sai cách.
- Đặc biệt quan trọng với việc vận hành thiết bị I/O.

## II. Hoạt động bên trong máy tính:
### 1. Bên trong hệ điều hành:
- Chương trình duy nhất luôn chạy trên máy tính là nhân (kernel).
    - Gồm chương trình hệ thống hoặc ứng dụng bên ngoài.
    - Lớp phần mềm giao tiếp trực tiếp nhất với phần cứng.

![image](https://hackmd.io/_uploads/SyXMiea4bl.png)
- Đi kèm với nhân còn có hai loại chương trình:
    - Chương trình hệ thống (system program): Được đóng gói cùng hệ điều hành nhưng không phải một phần của nhân.
    - Chương trình ứng dụng: Tất cả chương trình không có liên kết (associate) với hoạt động của hệ thống.
- Ngày nay một số hệ điều hành chứa middleware, một tập các frame/nền tảng phần mềm (software network) cung cấp các dịch vụ bổ sung hỗ trợ cho nhà phát triển.
> Database, đa phương tiện, đồ hoạ,...

### 2. Hoạt động bên trong máy tính:
#### a) Hoạt động:
![image](https://hackmd.io/_uploads/HyQmhgpV-l.png)
- Một hoặc nhiều CPU và các trình điều khiển thiết bị (device controller) kết nối nhau qua một BUS chung để truy xuất bộ nhớ chia sẻ (shared memory).
- CPU và các thiết bị chạy song song, cạnh tranh bộ nhớ.
- Các thiết bị nhập/xuất (I/O) và CPU có thể thực thi đồng thời.
- Mỗi trình điều khiển thiết bị chịu trách nhiệm một loại thiết bị cụ thể.
- Mỗi trình điều khiển thiết bị có một bộ đệm cục bộ.
- Mỗi loại trình điều khiển thiết bị có một device driver tương ứng của hệ điều hành để quản lý nó.
- CPU chuyển dữ liệu giữa bộ nhớ chính và các bộ đệm cục bộ.
- Khi trình điều khiển thiết bị hoàn tất các thao tác, nó báo hiệu cho CPU bằng cách phát sinh một Ngắt (interrupt).

#### b) Chương trình Bootstrap:
![image](https://hackmd.io/_uploads/Skz3hlT4Ze.png)
- Lưu trữ trọng ROM hoặc EEPROM (Firmware).
- Tìm kiếm và nạp nhân hệ điều hành vào bộ nhớ.
- Khởi tạo thanh ghi, bộ điều khiển, bộ nhớ.

#### c) Cơ chế Ngắt (Interrupt):
- Ngắt được tạo bởi phần mềm do một error hoặc một request của user gọi là trap hoặc exception.
- Sự kiện thường được báo hiệu bằng Ngắt.
- Phần cứng gửi tín hiệu đến CPU qua BUS hệ thống.
- Nền tảng để OS chuyển đổi giữa các tác vụ.
- Ngắt chuyển điều khiển đến Interrupt Service Routine thông qua Interrupt vector (Chứa địa chỉ của tất cả Service Routine).
- Kiến trúc Ngắt phải lưu địa chỉ của lệnh phát sinh Ngắt.
- Hệ điều hành hoạt động định hướng theo Ngắt (Interrupt Driven).

##### Quá trình xử lý ngắt:
![image](https://hackmd.io/_uploads/rJ-X6xp4Wl.png)
![image](https://hackmd.io/_uploads/SyOTpea4bx.png)
- CPU dừng việc đang làm ngay lập tức.
- Lưu địa chỉ lệnh bị ngắt để quay lại sau.
- Chuyển đến thực thi Interrupt Service Routine (ISR).

##### Quản lý ngắt:
![image](https://hackmd.io/_uploads/S1AxCe64bl.png)
- Sử dụng Vector Ngắt để tìm địa chỉ routine nhanh.
- Phân biệt ngắt phần cứng và ngắt phần mềm (Trap).
- CPU phục hồi trạng thái sau khi xử lý xong.

#### d) Cấu trúc I/O:
![image](https://hackmd.io/_uploads/rk-VM-pEbe.png)
- Device Controller quản lý một loại thiết bị.
- Sở hữu bộ đệm cục bộ và các thanh ghi.
- Hệ điều hành cung cấp Device Driver tương ứng.
- Direct Memory Access (DMA):
    - Dành cho thiết bị I/O tốc độ cao.
    - Truyền khối dữ liệu trực tiếp vào bộ nhớ chính.
    - Chỉ đạo ngắt khi hoàn tất toàn bộ khối dữ liệu.

#### e) Cấu trúc lưu trữ (storage):
![image](https://hackmd.io/_uploads/SkL90laEZe.png)
- Phân cấp lưu trữ:

![image](https://hackmd.io/_uploads/HJEM--aVbx.png)
- Tiêu chí:
    -  Tốc độ truy xuất (Speed)
    -  Chi phí (Cost)
    -  Tính bay hơi (volatility): Khả năng lưu trữ dữ liệu khi không có nguồn điện.
- Thanh ghi (Registers): Nhanh nhất, đắt nhất.
- Bộ nhớ đệm (Cache): Cấp độ trung gian.
- Main Memory.
- Đĩa trạng thái rắn SSD và Đĩa từ HDD.
- Băng từ (Magnetic Tapes): Chậm nhất, rẻ nhất.

##### Bộ nhớ chính (Main memory):
![image](https://hackmd.io/_uploads/SkI-y-aV-l.png)
![image](https://hackmd.io/_uploads/r1fmJbT4Wg.png)
- Thiết bị lưu trữ dung lượng lớn nhất mà CPU truy cập trực tiếp.
- Truy xuất ngẫu nhiên.
- Dựa trên DRAM (Dynamic Random - access Memory).
- Là bộ nhớ bay hơi (mất dữ liệu khi mất điện).

##### Bộ nhớ thứ cấp (Secondary Storage):
Mở rộng cho bộ nhớ chính để cung cấp khả năng lưu trữ không bay hơi dung lượng lớn.

###### Nonvolatile Memory (NVM):
![image](https://hackmd.io/_uploads/Hk7K1WTVbe.png)
> Flash memory, SSD (Solid State Disk),...
- Giữ được nội dung ngay cả khi ngắt điện.
- Tốc độ nhanh hơn đĩa từ nhưng chậm hơn RAM.

###### Ổ đĩa cứng (Hard Disk Drives):
![image](https://hackmd.io/_uploads/HkTUeZTN-l.png)
- Phương tiện lưu trữ thứ cấp phổ biến.
- Dữ liệu ghi lên các đĩa phủ vật liệu từ tính.
- Dung lượng lớn, giá thấp nhưng tốc độ chậm.

##### Nguyên lý Caching:
- Sao chép dữ liệu từ bộ nhớ chậm sang nhanh hơn.
- Tăng tốc truy cập cho dữ liệu dùng thường xuyên.
- Quản lý Cache là vấn đề quan trọng trong OS.

#### f) Hoạt động của máy tính hiện đại:
##### Kiến trúc Von Newman:
![image](https://hackmd.io/_uploads/SyfkSVMrZe.png)

##### Phân biệt các khái niệm về bộ xử lý:
- CPU: Thành phần phần cứng thực thi các lệnh.
- Processor (Bộ xử lý): Con chip vật lý chưa một hoặc nhiều CPU.
- Core (Lõi/Nhân): Đơn vị tính toán cơ bản của CPU.
- Multicore (Đa lõi): Nhiều lõi tính toán trên cùng một CPU.
- Multiprocessor (Đa bộ xử lý): Nhiều bộ xử lý.

## IV: Kiến trúc hệ thống máy tính:
### 1. Hệ thống đơn bộ xử lý (Single - Processor Systems):
- Chỉ có một bộ xử lý đa dụng (general - purpose processor) chính với một lõi duy nhất thực thi tập lệnh đa dụng (gồm các lệnh trong tiến trình).
- Có các bộ xử lý mục đích riêng biệt (đĩa, bàn phím,...) chỉ có thể thực thi các tập lệnh hạn chế và không thể chạy tiến trình user.

### 2. Hệ thống đa bộ xử lý (Multiprocessor System):
- Có nhiều hơn hai bộ xử lý, dùng chung BUS với bộ nhớ.
- Ưu điểm:
    - Tăng năng suất hệ thống (system throughput): Càng nhiều bộ nhớ xử lý càng nhanh.
    - Tính kinh tế: Ít tốn kém (dùng chung linh kiện).
    - Tăng độ tin cậy: Hỏng một bộ xử lý hệ thống vẫn chạy giữa các bộ xử lý còn lại
    - Graceful Degradation: Hệ thống chỉ giảm tốc độ.
    - Fault tolerant: Khả năng chịu lỗi cao.
- Phân loại:
    - Đa xử lý bất đối xứng (Asymmetric Multiprocessing): 
    - Đa xử lý đối xứng (Symmetric Multiprocessing): Mỗi bộ xử lý cùng thực hiện tất cả công việc.

#### a) Symmetric Multiprocessing (SMP):
![image](https://hackmd.io/_uploads/HyqNXZ64bg.png)
- Mỗi bộ xử lý chạy một bản sao của hệ điều hành.
- Mọi CPU bình đẳng, thực hiện mọi nhiệm vụ.
- Kiến trúc phổ biến nhất trong máy tính hiện nay.

#### b) Thiết kế nhân kép (Multicore Design):
![image](https://hackmd.io/_uploads/ry_OXWpNWx.png)
- Nhiều lõi tính toán trên cùng một chip vật lý.
- Giao tiếp giữa các lõi nhanh hơn giữa các chip.
- Tiết kiệm năng lượng hơn chip đơn lõi.

#### c) Hệ thống NUMA (Non - Uniform Memory Access):
![image](https://hackmd.io/_uploads/SJD9v4MrZl.png)

### 3. Hệ thống gom cụm (Clustered Systems):
![image](https://hackmd.io/_uploads/H1dnEbpEZg.png)
- Kết nối nhiều máy tính (nodes) thành một hệ thống, là một dạng hệ thống đa bộ xử lý nhưng gồm nhiều hệ thống làm việc với nhau.
- Các máy thường chia sẻ bộ nhớ qua mạng lưu trữ khu vực (SAN - Storage - Area Network).
- Cung cấp dịch vụ có độ sẵn sàng cao (High avaibility): Dịch vụ được cung cấp liên tục dù một phần cứng của cụm bị hỏng.
- Phân loại Clustering:
    - Gom cụm bất đối xứng (Asymmetric Clustering): Một máy ở chế độ hot - stanby, máy còn lại chạy ứng dụng.
    - Gom cụm đối xứng (Symmetric Clustering): Mọi máy đều chạy ứng dụng và giám sát nhau.
    - Parallelization: Nhiều máy truy cập cùng dữ liệu đồng thời.

## V. Các thao tác trong hệ điều hành:
### 1. Uniprogramming, Multiprogramming và Multitasking:
#### a) Đơn chương (Uniprogramming):
- Chỉ một công việc/chương trình được nạp vào bộ nhớ tại một thời điểm.
- Công việc/Chương trình được thực thi tuần tự.

#### b) Đa chương (Multiprogramming):
![image](https://hackmd.io/_uploads/BJoar-6V-l.png)
- Tổ chức nhiều công việc (mã và dữ liệu) sao cho CPU luôn có thể chọn một để thực thi:
    - Nhiều công việc được nạp đồng thời vào bộ nhớ.
    - Một công việc được chọn và chạy bởi bộ định thời công việc (job scheduling).
    - Khi một công việc phải chờ, hệ điều hành chuyển sang thực thi công việc khác.
- Trong hệ thống đa chương, công việc đang thực thi gọi là tiến trình (process).
- Tối ưu hoá hiệu suất sử dụng CPU.

#### c) Đa nhiệm (Muiltitasking):
- Là sự mở rộng của đa chương.
- CPU chuyển đổi giữa các công việc rất nhanh.
- User tương tác được khi chương trình chạy.
- Cung cấp thời gian phản hồi cực nhanh.

### 2. Các chế độ hoạt động:
Việc có nhiều chế độ hoạt động cho phép hệ điều hành bảo vệ chính nó và các thành phần khác của hệ thống:
- Hai chế độ cơ bản:
    - Chế độ người dùng (User mode)
    - Chế độ hạt nhân (Kernel mode)
- Có thể mở rộng nhiều hơn hai chế độ.

#### a) Chế độ User và Kernel:
- Phần cứng cung cấp Mode bit phân biệt khi nào hệ thống đang thực thi mã người dùng hay mã hạt nhân:
    - User mode (Bit = 1): Chạy ứng dụng.
    - Kernel mode (Bit = 0): Lệnh đặc quyền hệ thống.
- Một số lệnh được thiết kế riêng như đặc quyền (privileged), các lệnh này chỉ thực thi ở chế độ hạt nhân.
> Chuyển từ chế độ người dùng sang chế độ hạt nhân:
> ![image](https://hackmd.io/_uploads/B195UbTVWe.png)

#### b) Tầm quan trọng của Dual - Mode:
- Ngăn ứng dụng can thiệp vào nhân OS.
- Các lệnh nguy hiểm chỉ chạy ở Kernel mode.
- Lỗi ứng dụng không làm sập toàn bộ hệ thống.

#### c) Quy trình chuyển đổi:
- Hệ thống khởi động ở Kernel mode.
- Chuyển sang User mode khi chạy ứng dụng.
- Gặp Ngắt/System Call: Chuyển lại Kernel mode.

#### d) System Calls:
- Phương thức để ứng dụng yêu cầu dịch vụ nhân OS.
- Thực hiện thông qua thư viện lập trình (API).
- Chuyển quyền điều khiển từ User sang Kernel.

### 3. Timer:
- Đảm bảo OS giữ quyền kiểm soát CPU.
- Ngăn tiến trình chiếm CPU mãi mãi.
- Tạo ngắt định kỳ để OS can thiệp.

## VI. Quản lý tài nguyên:
### 1. Process Management:
![image](https://hackmd.io/_uploads/Sy5ZUmaVWl.png)
- Process là một chương trình đang thực thi.
- Cần CPU, bộ nhớ, file, thiết bị I/O.
- OS quản lý vòng đời (tạo, chạy, kết thúc).
- Nhiệm vụ của OS:
    - Lập tiến trình trên các CPU.
    - Tạm dừng và tiếp tục tiến trình.
    - Cung cấp cơ chế đồng bộ và giao tiếp.

### 2. Memory Management:
- Xác định phần bộ nhớ đang được dùng bởi ai.
- Quyết định nạp tiến trình khi có không gian.
- Cấp phát và thu hồi không gian bộ nhớ.

### 3. File - System Management:
- Tạo và xoá files, folder.
- Hỗ trợ thao tác đọc/ghi/tìm kiếm.
- Ánh xạ tệp lên thiết bị lưu trữ vật lý.

### 4. Mass - Storage Management:
- Quản lý không gian trống trên đĩa.
- Cấp phát không gian lưu trữ dữ liệu.
- Lập lịch đầu đọc đĩa để tối ưu tốc độ.

### 5. I/O System Management:
- Che giấu sự phức tạp phần cứng với user.
- Quản lý buffering, caching và spooling.
- Cung cấp giao diện Driver chung cho thiết bị.

## VII. Bảo mật và các môi trường mới:
### 1. Protection & Security:
![image](https://hackmd.io/_uploads/By43DmaVbg.png)
- Protection: Kiểm soát truy cập của tiến trình.
- Security: Chống tấn công bên ngoài (virus, hacker).
- Phân biệt user qua User ID và Group ID.

### 2. Virtulization:
- Chạy nhiều Guest OS trên một phần cứng.
- VMM quản lý các tài nguyên ảo.
- Tăng linh hoạt và tiết kiệm chi phí phần cứng.

### 3. Cloud Computing:
- Cung cấp tài nguyên tính toán qua mạng.
- Các loại cloud: Public, Private, Hybrid.
- Mô hình dịch vụ: SaaS, PaaS, Iass.

### 4. Real - time Systems:
- Dùng trong thiết bị điều khiển (robot, y tế,...).
- Ràng buộc thời gian rất nghiêm ngặt.
- Phản hồi trong khoảng thời gian xác định.

### 5. Open - source Operating Systems:
- Mã nguồn có sẵn cho mọi người nghiên cứu (Linux, FreeBSD, Android).
- Cộng đồng giúp cải thiện bảo mật nhanh chóng.
