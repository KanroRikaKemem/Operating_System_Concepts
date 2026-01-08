## TÓM TẮT CHƯƠNG I:
- OS quản lý tài nguyên và điều khiển máy tính.
- Cơ chế ngắt là nền tảng phần cứng cho OS.
- Đa xử lý và phân cụm tăng hiệu suất, tin cậy.
- Dual - mode bảo vệ hệ thống khỏi sai sót.
- Tiến trình, bộ nhớ, tệp là chức năng cốt lõi.
- Ảo hoá và đám mây là xu hướng hiện đại.

## Phần I: Hệ điều hành làm gì?
### 1. Định nghĩa Hệ điều hành:
- Là phần mềm quản lý phần cứng máy tính.
- Cung cấp nền tảng cho các ứng dụng.
- Đóng vai trò trung gian giữa người dùng và phần cứng.
- Các thành phần chính:
![image](https://hackmd.io/_uploads/Sk8LIeaEWl.png)

    - Phần cứng: CPU, bộ nhớ, thiết bị I/O,...
    - Hệ điều hành: Điều phối việc sử dụng phần cứng.
    - Chương trình ứng dụng: Giải quyết bài toán của user (Trình soạn thảo, trình duyệt trò chơi,...)
    - User: Con người, máy tính khác hoặc thiết bị.

### 2. User View:
#### PC và Workstation:
- User chiếm giữ hoàn toàn tài nguyên.
- Mục tiêu: Tối đa hoá sự tiện lợi và dễ dàng sử dụng.
- Hiệu suất sử dụng tài nguyên không ưu tiên hàng đầu.

#### Mainframe/Minicomputer:
- Nhiều người dùng kết nối qua endpoint.
- Chia sẻ tài nguyên chung của hệ thống.
- Mục tiêu: Tối ưu hoá việc tận dụng tài nguyên.

#### Mobile Devices:
- Sử dụng giao diện màn hình cảm ứng, cử chỉ.
- Hỗ trợ kết nối không dây mạnh mẽ.
- Tối ưu hoá thời lượng pin và trải nghiệm.

#### Embedded Systems:
- Máy tính trong oto, thiết bị gia dụng.
- Thường không có giao diện user trực tiếp.
- Chạy tự động không cần sự can thiệp.

### 3. System View:
#### Resource Allocator:
- OS quản lý CPU, lưu trữ, I/O.
- Giải quyết các yêu cầu tài nguyên xung đột.
- Đảm bảo hoạt động hiệu quả và công bằng.

#### Control Program:
- Quản lý việc thực thi chương trình user.
- Ngăn chặn lỗi và việc sử dụng máy sai cách.
- Đặc biệt quan trọng với việc vận hành thiết bị I/O.

### 4. Khái niệm Kernel:
- Chương trình duy nhất luôn chạy trên máy tính.
- Gồm chương trình hệ thống hoặc ứng dụng bên ngoài.
- Lớp phần mềm giao tiếp trực tiếp nhất với phần cứng.

![image](https://hackmd.io/_uploads/SyXMiea4bl.png)

## Phần II: Tổ chức hệ thống máy tính:
### 1. Hoạt động chung của hệ thống:
- Một hoặc nhiều CPU và các bộ điều khiển thiết bị.
- Kết nối qua một BUS chung.
- CPU và các thiết bị chạy song song, cạnh tranh bộ nhớ.

![image](https://hackmd.io/_uploads/HyQmhgpV-l.png)

### 2. Chương trình Bootstrap:
- Lưu trữ trọng ROM hoặc EEPROM (Firmware).
- Tìm kiếm và nạp nhân hệ điều hành vào bộ nhớ.
- Khởi tạo thanh ghi, bộ điều khiển, bộ nhớ.

![image](https://hackmd.io/_uploads/Skz3hlT4Ze.png)

### 3. Cơ chế Ngắt (Interrupt):
- Sự kiện thường được báo hiệu bằng Ngắt.
- Phần cứng gửi tín hiệu đến CPU qua BUS hệ thống.
- Nền tảng để OS chuyển đổi giữa các tác vụ.

![image](https://hackmd.io/_uploads/rJ-X6xp4Wl.png)

#### Quá trình xử lý ngắt:
![image](https://hackmd.io/_uploads/SyOTpea4bx.png)
- CPU dừng việc đang làm ngay lập tức.
- Lưu địa chỉ lệnh bị ngắt để quay lại sau.
- Chuyển đến thực thi Interrupt Service Routine (ISR).

#### Quản lý ngắt:
![image](https://hackmd.io/_uploads/S1AxCe64bl.png)
- Sử dụng Vector Ngắt để tìm địa chỉ routine nhanh.
- Phân biệt ngắt phần cứng và ngắt phần mềm (Trap).
- CPU phục hồi trạng thái sau khi xử lý xong.

### 4. Cấu trúc lưu trữ:
![image](https://hackmd.io/_uploads/SkL90laEZe.png)
Phân cấp lưu trữ:
![image](https://hackmd.io/_uploads/HJEM--aVbx.png)
- Tiêu chí: Tốc độ, chi phí, tính bay hơi.
- Thanh ghi (Registers): Nhanh nhất, đắt nhất.
- Bộ nhớ đệm (Cache): Cấp độ trung gian.
- Main Memory.
- Đĩa trạng thái rắn SSD và Đĩa từ HDD.
- Băng từ (Magnetic Tapes): Chậm nhất, rẻ nhất.

#### Main memory:
![image](https://hackmd.io/_uploads/SkI-y-aV-l.png)
![image](https://hackmd.io/_uploads/r1fmJbT4Wg.png)
- Tài nguyên duy nhất CPU truy cập trực tiếp.
- Dựa trên DRAM (Dynamic RAM).
- Là bộ nhớ bay hơi (mất dữ liệu khi mất điện).

#### Nonvolatile Memory (NVM):
![image](https://hackmd.io/_uploads/Hk7K1WTVbe.png)
> Flash memory, SSD (Solid State Disk),...
- Giữ được nội dung ngay cả khi ngắt điện.
- Tốc độ nhanh hơn đĩa từ nhưng chậm hơn RAM.

#### Ổ đĩa cứng (Hard Disk Drives):
![image](https://hackmd.io/_uploads/HkTUeZTN-l.png)
- Phương tiện lưu trữ thứ cấp phổ biến.
- Dữ liệu ghi lên các đĩa phủ vật liệu từ tính.
- Dung lượng lớn, giá thấp nhưng tốc độ chậm.

#### Nguyên lý Caching:
- Sao chép dữ liệu từ bộ nhớ chậm sang nhanh hơn.
- Tăng tốc truy cập cho dữ liệu dùng thường xuyên.
- Quản lý Cache là vấn đề quan trọng trong OS.

### 5. Cấu trúc I/O:
![image](https://hackmd.io/_uploads/rk-VM-pEbe.png)
- Device Controller quản lý một loại thiết bị.
- Sở hữu bộ đệm cục bộ và các thanh ghi.
- Hệ điều hành cung cấp Device Driver tương ứng.
- Direct Memory Access (DMA):
    - Dành cho thiết bị I/O tốc độ cao.
    - Truyền khối dữ liệu trực tiếp vào bộ nhớ chính.
    - Chỉ đạo ngắt khi hoàn tất toàn bộ khối dữ liệu.

## Phần III: Kiến trúc hệ thống máy tính:
### 1. Single - Processor Systems:
- Chỉ có một CPU chính thực hiện lệnh tổng quát.
- Có các bộ xử lý mục đích đặc biệt (đĩa, bàn phím,...).
- Bộ xử lý đặc biệt không chạy chương trình user.

### 2. Multiprocessor System:
- Có nhiều hơn hai bộ xử lý, dùng chung BUS với bộ nhớ.
- Ưu điểm:
    - Tăng thông lượng (throughput).
    - Tính kinh tế (dùng chung linh kiện).
    - Tăng độ tin cậy: Hỏng một CPU hệ thống vẫn chạy.
    - Graceful Degradation: Hệ thống chỉ giảm tốc độ.
    - Fault tolerant: Khả năng chịu lỗi cao.

#### Symmetric Multiprocessing (SMP):
![image](https://hackmd.io/_uploads/HyqNXZ64bg.png)
- Mỗi bộ xử lý chạy một bản sao của hệ điều hành.
- Mọi CPU bình đẳng, thực hiện mọi nhiệm vụ.
- Kiến trúc phổ biến nhất trong máy tính hiện nay.

#### Multicore Design:
![image](https://hackmd.io/_uploads/ry_OXWpNWx.png)
- Nhiều lõi tính toán trên cùng một chip vật lý.
- Giao tiếp giữa các lõi nhanh hơn giữa các chip.
- Tiết kiệm năng lượng hơn chip đơn lõi.

### 3. Clustered Systems:
![image](https://hackmd.io/_uploads/H1dnEbpEZg.png)
- Kết nối nhiều máy tính (nodes) thành một hệ thống.
- Các máy thường chia sẻ bộ nhớ qua mạng SAN.
- Cung cấp tính sẵn sàng cao (High avaibility).
- Phân loại Clustering:
    - Asymmetric Clustering: Một máy dự phòng, một máy chạy.
    - Symmetric Clustering: Mọi máy đều chạy và giám sát nhau.
    - Parallelization: Nhiều máy truy cập cùng dữ liệu đồng thời.

## Phần IV: Hoạt động của hệ điều hành:
### 1. Multiprogramming và Multitasking:
#### Cơ chế Multiprogramming:
![image](https://hackmd.io/_uploads/BJoar-6V-l.png)
- Giữ nhiều công việc trong bộ nhớ đồng thời.
- CPU luôn có việc làm, không chờ I/O.
- Tối ưu hoá hiệu suất sử dụng CPU.

#### Cơ chế Muiltitasking:
- Chuyển đổi giữa các công việc rất nhanh.
- User tương tác được khi chương trình chạy.
- Cung cấp thời gian phản hồi cực nhanh.

### 2. Dual - Mode và Multimode Operation:
![image](https://hackmd.io/_uploads/B195UbTVWe.png)

#### Chế độ User và Kernel:
Phần cứng cung cấp Mode bit phân biệt:
- User mode (Bit = 1): Chạy ứng dụng.
- Kernel mode (Bit = 0): Lệnh đặc quyền hệ thống.

#### Tầm quan trọng của Dual - Mode:
- Ngăn ứng dụng can thiệp vào nhân OS.
- Các lệnh nguy hiểm chỉ chạy ở Kernel mode.
- Lỗi ứng dụng không làm sập toàn bộ hệ thống.

#### Quy trình chuyển đổi:
- Hệ thống khởi động ở Kernel mode.
- Chuyển sang User mode khi chạy ứng dụng.
- Gặp Ngắt/System Call: Chuyển lại Kernel mode.

#### System Calls:
- Phương thức để ứng dụng yêu cầu dịch vụ nhân OS.
- Thực hiện thông qua thư viện lập trình (API).
- Chuyển quyền điều khiển từ User sang Kernel.

### 3. Timer:
- Đảm bảo OS giữ quyền kiểm soát CPU.
- Ngăn tiến trình chiếm CPU mãi mãi.
- Tạo ngắt định kỳ để OS can thiệp.

## Phần V: Quản lý tài nguyên:
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

## Phần VI: Bảo mật và các môi trường mới:
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
