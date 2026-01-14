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
- **Thực thi chương trình**:
    - Hệ thống phải nạp chương trình vào bộ nhớ.
    - Chạy chương trình và kết thúc thực thi.
    - Xử lý kết thúc bình thường hoặc bất thường (lỗi).
- **Thực hiện các thao tác I/O theo yêu cầu của chương treo yêu cầu của chương trình**.
    - Chương trình đang chạy có thể yêu cầu I/O, có thể là yêu cầu từ tập tin hoặc thiết bị.
    - User không kiểm soát thiết bị trực tiếp.
    - Hệ điều hành đảm bảo tính an toàn và hiệu quả cho I/O.
- **Các thao tác trên hệ thống file (File - system Manipulation)**.
> - Đọc, ghi, tạo và xoá các tập tin/thư mục,...
> - Tìm kiếm tập tin và liệt kê thông tin tập tin,...
> - Quản lý quyền truy cập (Cho phép/Từ chối).
> - Nhiều hệ điều hành hỗ trợ đa dạng loại hệ thống tập tin.
- **Trao đổi thông tin giữa các tiến trình qua hai cách**: Trao đổi thông tin giữa các tiến trình trên cùng một máy hoặc trao đổi qua mạng giữa các máy tính khác nhau:
    - Chia sẻ bộ nhớ (Shared memory).
    - Chuyển thông điệp (Message passing).
- **Phát hiện lỗi**:
    - Lỗi phần cứng, phần mềm.
    - Hệ điều hành đảm bảo tính đúng đắn của thuật toán.
    - Cung cấp cơ chế gỡ lỗi (debugger).
> Trong CPU, bộ nhớ, trên thiết bị I/O (dữ liệu hư,...).
> Do chương trình: Chia cho 0, truy cập đến địa chỉ bộ nhớ không cho phép,...
- **Cấp phát tài nguyên (resource allocation)**:
    - Tài nguyên: CPU, bộ nhớ chính, đĩa,...
    - Quản lý CPU, bộ nhớ chính, lưu trữ tập tin,...
    - Lập lịch CPU để tối ưu hiệu suất.
    - Quản lý các thiết bị ngoại vi (máy in, USB,...)
    - OS có các routine tương ứng.
- **Ghi nhật ký (Logging/Accounting)**:
    - Theo dõi chương trình nào sử dụng bao nhiêu tài nguyên.
    - Giúp admin cấu hình hệ thống tốt hơn.
    - Lưu vết user để tính phí hoặc thống kê.
- **Bảo vệ (protection) và An ninh (security)**:
    - Hai tiến trình khác nhau không được ảnh hưởng nhau.
    - Kiểm soát được các truy xuất tài nguyên của hệ thống.
    - Chỉ các user được phép sử dụng hệ thống mới truy cập được tài nguyên của hệ thống.
    - Chống lại các nỗ lực truy cập trái phép.
- **Giao diện người dùng**: Hầu hết các hệ điều hành hiện nay đều có giao diện người dùng:
#### Giao diện Command - Line Interface (CLI):
- Cho phép user nhập trực tiếp command.
- Thường được coi là một chương trình đặc biệt (Shell).
> bash, csh, zsh,...
- Cách hoạt động của Shell:
    - Nhận và thực thi lệnh tiếp theo của user.
    - Command có thể tích hợp sẵn trong Shell hoặc thực thi qua các chương trình hệ thống.
- Các loại Shell trong UNIX/Linux:
    - Bourne Shell (sh) và C Shell (csh).
    - Bourne - Again Shell (bash): Tiêu chuẩn hiện nay.

    ![image](https://hackmd.io/_uploads/r1YPPyHrZe.png)
    - Korn Shell (ksh) và Z Shell (zsh).
    - User có thể thay đổi Shell tuỳ ý.
- Admin và lập trình viên thường ưu tiên CLI vì nhanh.
#### Giao diện Graphics User Interface (GUI).
- Sử dụng hệ thống cửa sổ, menu, chuột.
- Ẩn dụ "Màn hình nền" (Desktop metaphor).
- Sử dụng các Icons trực quan.
- User phổ thông thường ưu tiên GUI.
#### Giao diện Touch - screen.
![image](https://hackmd.io/_uploads/B16YSqmr-x.png)
![image](https://hackmd.io/_uploads/Skc9HcmBbx.png)
![image](https://hackmd.io/_uploads/SJgnScXrWg.png)
- Phổ biến trên Smartphone và Tablet.
- Thao tác qua cử chỉ (Gestures) và chạm.
- Hỗ trợ bàn phím ảo và ra lệnh qua giọng nói.

## III. System Call:
![image](https://hackmd.io/_uploads/B1eULc7HZl.png)
- Dùng để giao tiếp giữa tiến trình và hệ điều hành, nói cách khác là cung cấp giao diện giữa tiến trình và hệ điều hành bằng cách gọi đến các dịch vụ mà hệ điều hành cung cấp.
> `open`, `read`, `write`,...
> 
> ![image](https://hackmd.io/_uploads/S1h-I57rZe.png)
> ![image](https://hackmd.io/_uploads/HyKX89XHWg.png)
- Các chương trình thực hiện hàng ngàn lời mời gọi mỗi giây.
- Thông thường được viết bằng ngôn ngữ cấp cao (C hoặc C++) và hầu hết được truy cập thông qua các **Application Programming Interface (API)**:
    - API giúp xác định các hàm, tham số và giá trị trả về.
    - Giúp mã nguồn dễ dàng chuyển đổi (Portability).

![image](https://hackmd.io/_uploads/SkqHz57B-l.png)
> ![image](https://hackmd.io/_uploads/ryGRBcmrZg.png)
- Ba APIs thông dụng:
    - Win32 API cho Windows
    - POSIX API cho POSIX - based system (gồm tất cả phiên bản của UNIX, Linux và Mac OS X).
    - Java API cho các máy ảo Java (JVM).
- Ưu điểm của API:
    - Đơn giản hoá việc lập trình ứng dụng.
    - Tăng tính di động cho mã nguồn.
    - Che giấu sự phức tạp của việc giao tiếp với Nhân.
- Cách thức thực thi:
    - Mỗi lời gọi hệ thống có một số hiệu (number).
    - System - call Interface duy trì bảng chỉ mục.
    - Chuyển sang Kernel mode để thực thi.
- Giao diện System call:
    - Là trạm trung chuyển giữa API và nhân hệ điều hành.
    - Nhân thực hiện yêu cầu và trả kết quả về.
    - User không cần biết chi tiết bên trong nhân.
- Ba phương pháp truyền tham số khi dùng system call:
    - Qua thanh ghi
    - Qua một khối bộ nhớ (block), địa chỉ của vùng nhớ được gửi đến hệ điều hành qua thanh ghi.
    - Qua stack:
        - Tham số được "push" vào stack bởi chương trình.
        - Hệ điều hành "pop" tham số ra để xử lý.
- Các nhóm System call chính:
    - Kiểm soát tiến trình (Pricess control).
    - Quản lý tập tin và thiết bị.
    - Duy trì thông tin.
    - Giao tiếp và bảo vệ.

## IV. Các chương trình hệ thống (System programs):
![image](https://hackmd.io/_uploads/HyWcL5mSbe.png)
- Chương trình hệ thống:
    - Cung cấp môi trường phát triển và thực thi.
    - Là phần mở rộng của các System call.
    - Đóng vai trò giao diện trực tiếp với user.
- Gồm:
    - Quản lý hệ thống file (`create`, `delete`, `rename`, `list`,...).
    - Thông tin trạng thái (`date`, `time`, dung lượng trống).
    - Soạn thảo file (`file editor`,...).
    - Hỗ trợ ngôn ngữ lập trình (`complier`, `assembler`, `interpreter`,...).
    - Nạp, thực thi, giúp tìm lỗi chương trình (`loader`, `debugger`,...).
    - Giao tiếp (email, web browser,...).
- User chủ yếu làm việc thông qua các system program, không làm trực tiếp với system call.
- Các dịch vụ nền (Daemons):
    - Chạy liên tục trong suốt quá trình hệ thống hoạt động..
    - Xử lý các tác vụ như mạng, in ấn, lỗi,...
    - Không tương tác trực tiếp qua GUI.

## V. Thiết kế & Cấu trúc hệ thống:
### 1. Thiết kế hệ thống:
#### Mục tiêu:
- Mục tiêu user: Dễ dùng, nhanh, tin cậy.
- Mục tiêu hệ thống: Dễ bảo trì, hiệu quả, linh hoạt.
- Không có thiết kế hoàn hảo cho mọi mục đích.
#### Cơ chế và Chính sách (Mechanisms and Policies):
- Cơ chế: Cách thức thực hiện (How).
- Chính sách: Cái gì sẽ được thực hiện (What).
- Việc tách biệt giúp thay đổi chính sách mà không sửa cơ chế.

#### Ngôn ngữ cài đặt hệ điều hành:
- Ban đầu là hợp ngữ Assembly.
- Hiện nay C và C++ là chủ đạo $\rightarrow$ Ngôn ngữ bậc cao giúp phát triển nhanh và dễ chuyển đổi.

### 2. Cấu trúc hệ thống:
Hệ điều hành là một chương trình lớn có nhiều dạng cấu trúc khác nhau:

#### Cấu trúc MS - DOS:
- Thiết kế đơn giản cho phần cứng hạn chế.
- Các lớp không được tách biệt rõ ràng.
- Ứng dụng có thể truy cập trực tiếp phần cứng.

#### Cấu trúc Monolithic - Original UNIX:
![image](https://hackmd.io/_uploads/HkC-t5XHWe.png)
- Do giới hạn về chức năng phần cứng nên Original UNIX cũng có cấu trúc rất giới hạn, mọi chức năng nằm chung trong một không gian.
- UNIX gồm hai phần tách rời nhau:
    - Nhân (cung cấp file system, CPU scheduling, memory management,...).
    - System program.
- Hiệu năng cực cao vì không tốn phí giao tiếp.
- Rất khó gỡ lỗi và mở rộng.

#### Cấu trúc Layered Approach:
![image](https://hackmd.io/_uploads/ryrYt9XBZx.png)

Hệ điều hành được chia thành nhiều lớp (layer):
- Lớp dưới cùng - hardware.
- Lớp trên cùng là giao tiếp với user.
- Lớp trên chỉ phụ thuộc lớp dưới, lớp dưới phục vụ lớp trên.
- Một lớp dưới chỉ có thể gọi các hàm của lớp dưới và các hàm của nó bởi lớp trên.
- Dễ gỡ lỗi nhưng hiệu năng kém.
> Hệ điều hành THE,...

#### Cấu trúc vi nhân (Microkernels):
![image](https://hackmd.io/_uploads/Bk6Jc9XSZx.png)
- Phân chia module theo microkernel (CMU Mach OS, 1980).
- Chuyển một số chức năng của OS từ kernel space sang user space.
- Thu gọn kernel $\rightarrow$ microkernel bao gồm các chức năng tối thiểu như quản lý tiến trình, bộ nhớ và cơ chế giao tiếp giữa các tiến trình.
- Giao tiếp giữa các user module qua cơ chế truyền thông điệp (IPC).
- Ưu điểm: Bảo mật cao, dễ mở rộng, tin cậy.
- Nhược điểm: Hiệu năng giảm do overhead giao tiếp.
> QNX, Mach,...

#### Cấu trúc Modules:
- Nhiều hệ điều hành hiện đại triển khai các loadable kernel modules (LKMs):
    - Sử dụng cách tiếp cận hướng đối tượng.
    - Mỗi core thành phần là tách biệt nhau.
    - Trao đổi thông qua các interfaces.
    - Mỗi modules như là một phần của nhân.
- Nhân chứa các thành phần cốt lõi, các thành phần khác nạp vào khi cần (driver, file system).
- Nhìn chung cấu trúc Modules giống với cấu trúc Layer nhưng phức tạp hơn.
> - Linux, Solaris,...
> - Nhân Solaris:
>   
>    ![image](https://hackmd.io/_uploads/S1eX_MSBbe.png)
>    - Sử dụng kiến trúc module hoá cao độ.
>    - Các modules được liên kết động tại thời điểm chạy.
>    - Hiệu quả hơn phân lớp và linh hoạt hơn đơn khối.

#### Cấu trúc Hybrid Systems:
Hầu hết các hệ điều hành hiện đại không theo một cấu trúc thuần tuý nào mà lai giữa các cấu trúc với nhau:
- Cấu trúc lai là sự kết hợp nhiều cách tiếp cận để giải quyết các nhu cầu về hiệu suất, bảo mật, nhu cầu sử dụng.
- Nhân Linux và Solaris theo cấu trúc kết hợp không gian địa chỉ kernel, cấu trúc monolithic và modules.
- Nhân Windows hầu như theo cấu trúc liền khối, cộng với cấu trúc vi nhân cho các hệ thống cá nhân khác nhau.
- Hầu hết hệ điều hành hiện đại là hệ thống lai.
- Tận dụng tốc độ của đơn khối và tính linh hoạt của modules.
##### Cấu trúc của macOS và iOS:
![image](https://hackmd.io/_uploads/HJlTocXr-l.png)
- Cocoa Touch: Lớp giao diện và cảm ứng.
- Media Services: Âm thanh, hình ảnh.
- Core Services: Dịch vụ lõi.
- Core OS: Dựa trên Darwin.

##### Cấu trúc của Darwin:
![image](https://hackmd.io/_uploads/S1QCoqmr-g.png)
- Kết hợp vi nhân Mach và BSD UNIX.
- Sử dụng kiến trúc phân lớp cho các dịch vụ đồ hoạ.
- Hỗ trợ modules nạp động (kexts).

##### Kiến trúc Android:
![image](https://hackmd.io/_uploads/SJVg3cmrWx.png)
- Được phát triển bởi Open Handset Alliance (Google).
- Được phát triển dựa trên nhân Linux (Đã chỉnh sửa).
- Môi trường chạy gồm tập các thư biện API và máy ảo ART VM, thư viện C/C++.
- Thư viện gồm các frameworks cho web browser, database, multimedia,... cho lập trình viên.
- Môi trường thực thi:
    - Chạy các file thực thi định dạng `.dex`.
    - Quản lý bộ nhớ và thu gom rác tự động.
    - Giúp ứng dụng chạy tương thích trên nhiều phần cứng.

## VI. Xây dựng & Khởi động:
### 1. Xây dựng hệ điều hành:
- Xác định cấu hình phần cứng mục tiêu.
- Biên dịch mã nguồn nhân hệ điều hành.
- Cài đặt các chương trình hệ thống cần thiết.

### 2. Khởi động hệ thống:
- Mã Bootsrap loader tìm kiếm nhân trên đĩa.
- Nạp nhân vào bộ nhớ chính.
- Nhân bắt đầu khởi tạo các tiến trình đầu tiên.
- Firmware (BIOS/UEFI) quản lý bước khởi đầu.
- Hệ thống thực hiện kiểm tra phần cứng (POST).
- Hoàn tất khi giao diện người dùng xuất hiện.
- Vai trò của BIOS UEFI:
    - BIOS: Chuẩn cũ, giới hạn dung lượng đĩa.
    - UEFI: Chuẩn mới, bảo mật hơn, hỗ trợ đĩa lớn.
    - Cung cấp các thiết lập phân cứng cơ bản.

## VII. Giám sát hiệu năng:
### 1. Giám sát hiệu năng hệ điều hành:
- Theo dõi việc sử dụng CPU, RAM I/O.
- Phát hiện các điểm nghẽn (bottlenecks).
- Sử dụng dữ liệu để tối ưu hoá hệ thống.

### 2. Công cụ giám sát hệ thống:
- Windows: Task Manager, Resource Monitor.
- UNIX/Linux: top, ps. iostat, vmstat.
- Hiển thị thông tin theo thời gian thực.
#### Nâng cao:
- BBC (BPF Complier Collection).
- Công cụ phân tích động nhân Linux.
- Theo dõi các sự kiện cụ thể mà không làm chậm máy.

### 3. Kỹ thuật Vết (Tracing):
- Ghi lại chuỗi sự kiện theo thời gian.
- DTrace: Công cụ nổi tiếng trên Solaris/macOS.
- Giúp gỡ các lỗi phức tạp liên quan đến tương tác.
