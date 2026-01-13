## I. Các thành phần của hệ điều hành:
![image](https://hackmd.io/_uploads/r17tDBzSbl.png)
### 1. Quản lý tiến trình:
- Để hoàn thành công việc, một tiến trình cần:
    - CPU
    - Bộ nhớ
    - File
    - Thiết bị I/O
    - ...
- Các nhiệm vụ chính:
    - Tạo và huỷ tiến trình.
    - Tạm dừng/Thực thi tiếp tiến trình.
    - Cung cấp các cơ chế:
        - Đồng bộ hoạt động các tiến trình.
        - Giao tiếp giữa các tiến trình.
        - Khống chế tắc nghẽn.

### 2. Quản lý bộ nhớ chính:
![image](https://hackmd.io/_uploads/S1x9uBGBbl.png)
![image](https://hackmd.io/_uploads/SyLxKrMBbe.png)
![image](https://hackmd.io/_uploads/B1jfYSzB-g.png)
- Bộ nhớ chính là trung tâm của các thao tác, xử lý.
- Để nâng cao hiệu suất sử dụng CPU, hệ điều hành cần quản lý bộ nhớ thích hợp.
- Các nhiệm vụ chính:
    - Theo dõi, quản lý các vùng nhớ trống đã cấp phát.
    - Quyết định sẽ nạp chương trình nào khi có vùng nhớ trống.
    - Cấp phát và thu hồi các vùng nhớ khi cần.

### 3. Quản lý file:
![image](https://hackmd.io/_uploads/Hk-8trMHWg.png)
- Hệ thống file:
    - File
    - Folder
- Các dịch vụ chính:
    - Tạo và xoá file, thư mục.
    - Các thao tác xử lý file, folder như copy, paste,...
    - "Ánh xạ" file/folder vào thiết bị thứ cấp tương ứng.
    - Sao lưu và phục hồi dữ liệu.

### 4. Quản lý hệ thống I/O:
![image](https://hackmd.io/_uploads/H1lRtHfB-g.png)
- Che giấu sự khác biệt của các thiết bị I/O trước user.
- Có chức năng:
    - Cơ chế: buffering, caching, spooling.
    - Cung cấp giao diện chung đến các trình điều khiển thiết bị.
    - Bộ điều khiển các thiết bị phần cứng.

### 5. Quản lý hệ thống lưu trữ thứ cấp:
![image](https://hackmd.io/_uploads/ByW4cSGSWx.png)
- Bộ nhớ chính: Kích thước nhỏ, là môi trường chứa thông tin không bền vững $\rightarrow$ Cần hệ thống lưu trữ thứ cấp để lưu trữ bền vững các dữ liệu, chương trình.
- Phương tiện lưu trữ thông dụng là HDD và SSD.
- Nhiệm vụ của hệ điều hành trong quản lý đĩa:
    - Quản lý không gian sống trên đĩa (Free space management).
    - Cấp phát không gian lưu trữ.
    - Định thời gian hoạt động cho đĩa.
$\rightarrow$ Sử dụng thường xuyên $\rightarrow$ Ảnh hưởng lớn đến tốc độ của cả hệ thống $\rightarrow$ Cần hiệu quả.

### 6. Hệ thống bảo vệ:
![image](https://hackmd.io/_uploads/B1yQjHfSWx.png)
Trong hệ thống cho phép nhiều user hay nhiều process diễn ra đồng thời:
- Kiểm soát tiến trình user đăng nhập/đăng xuất và sử dụng hệ thống.
- Kiểm soát việc truy cập các tài nguyên trong hệ thống.
- Bảo đảm những user/process chỉ được phép sử dụng các tài nguyên dành cho nó.
- Nhiệm vụ:
    - Cung cấp cơ chế kiểm soát đăng nhập/đăng xuất.
    - Phân định được sự truy cập tài nguyên hợp pháp và bất hợp pháp.
    - Phương tiện thi hành các chính sách (Enforcement of Policies).

### 7. Hệ thống thông dịch lệnh:
![image](https://hackmd.io/_uploads/rkBRoHGH-l.png)
- Là giao diện chủ yếu giữa user và OS.
> Shell, mouse-based window-and-menu,...
- Khi user đăng nhập:
    - Command line interpreter (shell) chạy, chờ nhận lệnh từ user, thực thi và trả kết quả.
    - Các lệnh $\rightarrow$ Bộ điều khiển $\rightarrow$ Hệ điều hành.
    - Các lệnh chủ yếu:
        - Tạo, huỷ, quản lý tiến trình và hệ thống.
        - Kiểm soát I/O.
        - Quản lý bộ lưu trữ thứ cấp.
        - Quản lý bộ nhớ chính.
        - Truy cập hệ thống file và cơ chế bảo mật.

## II. Các dịch vụ hệ điều hành cung cấp:
### 1. Cấu trúc tổng quan các dịch vụ của hệ điều hành:
![image](https://hackmd.io/_uploads/H1o-gqQHWg.png)

### 2. Các dịch vụ hệ điều hành cung cấp:
![image](https://hackmd.io/_uploads/S1RmgcQSWe.png)
- **Thực thi chương trình**.
- **Thực hiện các thao tác I/O theo yêu cầu của chương treo yêu cầu của chương trình**.
- **Các thao tác trên hệ thống file**.
- **Trao đổi thông tin giữa các tiến trình qua hai cách**:
    - Chia sẻ bộ nhớ (Shared memory).
    - Chuyển thông điệp (Message passing).
- **Phát hiện lỗi**:
> Trong CPU, bộ nhớ, trên thiết bị I/O (dữ liệu hư,...).
> Do chương trình: Chia cho 0, truy cập đến địa chỉ bộ nhớ không cho phép,...
- **Cấp phát tài nguyên (resource allocation)**:
    - Tài nguyên: CPU, bộ nhớ chính, đĩa,...
    - OS có các routine tương ứng.
- **Kế toán (accounting)**: Lưu vết user để tính phí hoặc thống kê.
- **Bảo vệ (protection) và an ninh (security)**:
    - Hai tiến trình khác nhau không được ảnh hưởng nhau.
    - Kiểm soát được các truy xuất tài nguyên của hệ thống.
    - Chỉ các user được phép sử dụng hệ thống mới truy cập được tài nguyên của hệ thống.
- **Giao diện người dùng**: Hầu hết các hệ điều hành hiện nay đều có giao diện người dùng:
    - Giao diện Command - Line Interface (CLI).
    - Giao diện Graphics User Interface (GUI).
    - Giao diện Touch - screen.

![image](https://hackmd.io/_uploads/B16YSqmr-x.png)
![image](https://hackmd.io/_uploads/Skc9HcmBbx.png)
![image](https://hackmd.io/_uploads/SJgnScXrWg.png)

## III. System Call:
![image](https://hackmd.io/_uploads/B1eULc7HZl.png)
- Dùng để giao tiếp giữa tiến trình và hệ điều hành, nói cách khác là cung cấp giao diện giữa tiến trình và hệ điều hành bằng cách gọi đến các dịch vụ mà hệ điều hành cung cấp.
> `open`, `read`, `write`,...
> 
> ![image](https://hackmd.io/_uploads/S1h-I57rZe.png)
> ![image](https://hackmd.io/_uploads/HyKX89XHWg.png)
- Thông thường được viết bằng ngôn ngữ cấp cao (C hoặc C++) và hầu hết được truy cập thông qua các **Application Programming Interface (API)**.

![image](https://hackmd.io/_uploads/SkqHz57B-l.png)
> ![image](https://hackmd.io/_uploads/ryGRBcmrZg.png)
- Ba APIs thông dụng:
    - Win32 API cho Windows
    - POSIX API cho POSIX - based system (gồm tất cả phiên bản của UNIX, Linux và Mac OS X).
    - Java API cho các máy ảo Java (JVM).
- Ba phương pháp truyền tham số khi dùng system call:
    - Qua thanh ghi
    - Qua một vùng nhớ, địa chỉ của vùng nhớ được gửi đến hệ điều hành qua thanh ghi.
    - Qua stack

## IV. Các chương trình hệ thống (System programs):
![image](https://hackmd.io/_uploads/HyWcL5mSbe.png)
- Chương trình hệ thống gồm:
    - Quản lý hệ thống file (`create`, `delete`, `rename`, `list`,...).
    - Thông tin trạng thái (`date`, `time`, dung lượng trống).
    - Soạn thảo file (`file editor`,...).
    - Hỗ trợ ngôn ngữ lập trình (`complier`, `assembler`, `interpreter`,...).
    - Nạp, thực thi, giúp tìm lỗi chương trình (`loader`, `debugger`,...).
    - Giao tiếp (email, web browser,...).
- User chủ yếu làm việc thông qua các system program, không làm trực tiếp với system call.

## V. Cấu trúc hệ thống:
Hệ điều hành là một chương trình lớn có nhiều dạng cấu trúc khác nhau:

### 1. Cấu trúc Monolithic - Original UNIX:
![image](https://hackmd.io/_uploads/HkC-t5XHWe.png)
- Do giới hạn về chức năng phần cứng nên Original UNIX cũng có cấu trúc rất giới hạn:
- UNIX gồm hai phần tách rời nhau:
    - Nhân (cung cấp file system, CPU scheduling, memory management,...).
    - System program.

### 2. Cấu trúc Layered Approach:
![image](https://hackmd.io/_uploads/ryrYt9XBZx.png)
Hệ điều hành được chia thành nhiều lớp (layer):
- Lớp dưới cùng - hardware.
- Lớp trên cùng là giao tiếp với user.
- Lớp trên chỉ phụ thuộc lớp dưới.
- Một lớp dưới chỉ có thể gọi các hàm của lớp dưới  và các hàm của nó bởi lớp trên.
> Hệ điều hành THE,...

### 3. Cấu trúc Microkernels:
![image](https://hackmd.io/_uploads/Bk6Jc9XSZx.png)
- Phân chia module theo microkernel (CMU Mach OS, 1980).
- Chuyển một số chức năng của OS từ kernel space sang user space.
- Thu gọn kernel $\rightarrow$ microkernel bao gồm các chức năng tối thiểu như quản lý tiến trình, bộ nhớ và cơ chế giao tiếp giữa các tiến trình.
- Giao tiếp giữa các user module qua cơ chế truyền thông điệp.

### 4. Cấu trúc Modules:
- Nhiều hệ điều hành hiện đại triển khai các loadable kernel modules (LKMs):
    - Sử dụng cách tiếp cận hướng đối tượng.
    - Mỗi core thành phần là tách biệt nhau.
    - Trao đổi thông qua các interfaces.
    - Mỗi modules như là một phần của nhân.
- Nhìn chung cấu trúc Modules giống với cấu trúc Layer nhưng phức tạp hơn.
> Linux, Solaris,...

### 5. Cấu trúc Hybrid Systems:
Hầu hết các hệ điều hành hiện đại không theo một cấu trúc thuần tuý nào mà lai giữa các cấu trúc với nhau:
- Cấu trúc lai là sự kết hợp nhiều cách tiếp cận để giải quyết các nhu cầu về hiệu suất, bảo mật, nhu cầu sử dụng.
- Nhân Linux và Solaris theo cấu trúc kết hợp không gian địa chỉ kernel, cấu trúc monolithic và modules.
- Nhân Windows hầu như theo cấu trúc liền khối, cộng với cấu trúc vi nhân cho các hệ thống cá nhân khác nhau.
#### Cấu trúc của macOS và iOS:
![image](https://hackmd.io/_uploads/HJlTocXr-l.png)
#### Cấu trúc của Darwin:
![image](https://hackmd.io/_uploads/S1QCoqmr-g.png)
#### Kiến trúc Android:
![image](https://hackmd.io/_uploads/SJVg3cmrWx.png)
- Được phát triển bởi Open Handset Alliance (Google).
- Được phát triển dựa trên nhân Linux.
- Môi trường chạy gồm tập các thư biện API và máy ảo ART VM.
- Thư viện gồm các frameworks cho web browser, database, multimedia,...
