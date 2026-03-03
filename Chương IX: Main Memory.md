## I. Bối cảnh:
- Trung tâm vận hành hệ thống:
    - Bộ nhớ là trung tâm của hoạt động hệ thống máy tính hiện đại.
    - Gồm một mảng lớn các byte, mỗi byte có địa chỉ riêng.
    - CPU lấy lệnh từ bộ nhớ theo giá trị của bộ đếm chương trình.
- Chu kỳ thực thi lệnh:
    - Lấy lệnh từ bộ nhớ (Fetch).
    - Giải mã lệnh (Decode).
    - Có thể lấy thêm toán hạng (Operands) từ bộ nhớ.
    - Sau khi thực thi, kết quả có thể được lưu lại vào bộ nhớ.
- Góc nhìn của Đơn vị bộ nhớ:
    - Đơn vị bộ nhớ chỉ thấy một luồng các địa chỉ bộ nhớ.
    - Nó không biến địa chỉ được tạo ra thế nào hoặc để làm gì.
    - Ta chỉ quan tâm đến chuỗi địa chỉ được tạo bởi chương trình đang chạy.
- Các vấn đề quản lý bộ nhớ:

### 1. Phần cứng cơ bản:
- Bộ nhớ chính và các thanh ghi tích hợp là kho lưu trữ duy nhất CPU truy cập trực tiếp.
- Lệnh máy lấy địa chỉ bộ nhớ làm đối số nhưng không lấy địa chỉ đĩa.
- Dữ liệu phải được di chuyển vào bộ nhớ trước khi CPU hoạt động trên đó.
- Tốc độ truy cập phần cứng:
    - Thanh ghi tích hợp: Truy cập trong vòng một chu kỳ clock CPU.
    - Bộ nhớ chính: Truy cập qua BUS bộ nhớ, mất nhiều chu kỳ.
    - CPU thường cần "stall" (Tạm dừng) vì không có dữ liệu để hoàn thành lệnh.
- Bộ nhớ đệm (Cache):
    - Giải pháp là thêm bộ nhớ nhanh giữa CPU và bộ nhớ chính (thường nằm trên chip).
    - Phần cứng tự động tăng tốc độ truy cập mà không cần OS kiểm soát.
    - Cần đảm bảo hoạt động chính xác và bảo vệ hệ thống.
- Không gian bộ nhớ riêng biệt:
    - Bảo vệ các tiến trình khỏi nhau là nền tảng để thực thi đa nhiệm.
    - Cần xác định phạm vi địa chỉ hợp lệ mà một tiến trình có thể truy cập.
    - Đảm bảo tiến trình chỉ có thể truy cập vào các địa chỉ hợp lệ này.
- Thanh ghi Base và Limit:
    - Thanh ghi Base giữ địa chỉ vật lý nhỏ nhất hợp lệ.
    - Thanh ghi Limit xác định kích thước của phạm vi.
> Ví dụ:
> 
> ![image](https://hackmd.io/_uploads/BkiFafVtWg.png)
- Bảo vệ bộ nhớ bằng phần cứng:
    - CPU so sánh mọi địa chỉ được tạo ra trong chế độ user với các thanh ghi.
    - Truy cập trái phép gây ra một "trap" tới hệ điều hành (lỗi fatal).
    - Ngăn chương trình user sửa đổi mã hoặc cấu trúc dữ liệu của OS/user khác.
- Sơ đồ Bảo vệ địa chỉ phần cứng:

![image](https://hackmd.io/_uploads/HkSlCzEK-e.png)
- Nạp thanh ghi Base/Limit:
    - Các thanh ghi này chỉ có thể được nạp bởi hệ điều hành.
    - Sử dụng các lệnh đặc quyền (Privileged Instructions).
    - Chỉ thực thi được trong Kernel Mode.
    - Ngăn chương trình người dùng thay đổi nội dung thanh ghi.
- Quyền truy cập của Hệ điều hành:
    - Hệ điều hành có quyền truy cập không hạn chế vào bộ nhớ OS và user.
    - Cho phép nạp chương trình user, xử lý lỗi, gọi hệ thống và thực hiện I/O.
> Ví dụ: Lưu trạng thái tiến trình cũ và nạp trạng thái tiến trình mới khi chuyển ngữ cảnh.

### 2. Ràng buộc địa chỉ:
- Chương trình cư trú trên đĩa dưới dạng file thực thi nhị phân.
- Để chạy, chương trình được đưa vào bộ nhớ và đặt vào ngữ cảnh tiến trình.
- Hầu hết các hệ thống cho phép tiến trình user nằm ở bất kỳ đâu trong bộ nhớ vật lý.
- Các dạng đại diện địa chỉ:

![image](https://hackmd.io/_uploads/rysJkXVKWg.png)
    - Mã nguồn: Địa chỉ thường là biểu tượng (Ví dụ: Biến `count`).
    - Biên dịch: Ràng buộc biểu tượng sang địa chỉ tái định vụ (Ví dụ: 14 bytes từ đầu module).
    - Linker/Loader: Ràng buộc địa chỉ tái định vị sang địa chỉ tuyệt đối.
- Các giai đoạn Ràng buộc:
    - Compile Time: Nếu biết vị trí bộ nhớ lúc biên dịch, có thể tạo mã tuyệt đối. Phải biên dịch lại nếu vị trí thay đổi.
    - Load time: Nếu không biết vị trí lúc biên dịch, tạo mã tái định vị. Binding bị trì hoãn cho đến khi nạp.
- Giai đoạn Thực thi (Run time):
    - Execution time: Nếu tiến trình có thể di chuyển khi đang chạy, ràng buộc bị trì hoãn đến lúc thực thi.
    - Cần hỗ trợ phần cứng đặc biệt.
    - Hầu hết các hệ điều hành hiện nay dùng phương pháp này.

### 3. Không gian địa chỉ Logic và Vật lý:
- Địa chỉ Logic: Địa chỉ được tạo ra bởi CPU.
- Địa chỉ Vật lý: Địa chỉ thực sự được đơn vị bộ nhớ nhìn thấy và nạp vào thanh ghi địa chỉ, nghĩa là thứ RAM thực sự dùng.
- Binding lúc compile/load: Địa chỉ logic và địa chỉ vật lý trùng nhau.
- Binding lúc thực thi: Địa chỉ logic và vật lý khác nhau.
- Hai cái chỉ trùng nhau trong hệ đơn giản, hệ hiện đại thì luôn khác nhau.
- Địa chỉ ảo (Virtual Address):
    - Địa chỉ logic trong sơ đồ ràng buộc lúc thực thi được gọi là địa chỉ ảo.
    - Không gian địa chỉ logic: Tập hợp tất cả địa chỉ ảo được tạo bởi chương trình.
    - Không gian địa chỉ vật lý: Tập hợp các địa chỉ vật lý tương ứng.
- Memory Management Unit (MMU):

![image](https://hackmd.io/_uploads/B1vZW7EKWe.png)
    - Thiết bị phần cứng thực hiện ánh xạ địa chỉ ảo sang vật lý khi chạy.
    - Một sơ đồ MMU đơn giản: Giá trị trong thanh ghi tái định vị (relocation register) được cộng vào mọi địa chỉ tạo bởi tiến trình người dùng.
> Ví dụ Tái định vị tự động:
> Nếu relocation register = 14000:
> - Truy cập địa chỉ ảo 0 thì được ánh xạ thành 14000 vật lý.
> - Truy cập địa chỉ ảo 346 thì được ánh xạ thành 14346 vật lý.
> - Tiến trình người dùng không bao giờ thấy được địa chỉ vật lý thực.
- Tái định vị bằng thanh ghi:

![image](https://hackmd.io/_uploads/Hy6tWX4F-l.png)

### 4. Nạp động (Dynamic Loading):
- Để tận dụng bộ nhớ tốt hơn, một routine không được nạp cho đến khi nó được gọi.
- Tất cả routines được giữ tren đĩa dưới dạng định dạng nạp tái định vị.
- Đặc biệt hữu ích khi cần lượng lớn code để xử lý các trường hợp hiếm gặp (Ví dụ: Routines xử lý lỗi).
> Ví dụ: Chương trình có 100 hàm nhưng người dùng chỉ dùng 20 hàm, 80 hàm còn lại không cần nằm trong RAM.
- Ưu điểm của Nạp động:
    - Kích thước tiến trình không bị giới hạn bởi kích thước bộ nhớ vật lý.
    - Không yêu cầu hỗ trợ đặc biệt từ Hệ điều hành (trách nhiệm của lập trình viên).
    - Hệ điều hành có thể cung cấp thư viện để hỗ trợ nạp động.

### 5. Liên kết động (Dynamic Linking):
- DLLs (Dynamically Linked Libraries): Thư viện hệ thống được liên kết khi chương trình chạy.
- Liên kết tĩnh (Static linking): Thư viện hệ thống được liên kết khi chương trình chạy.
- Liên kết tĩnh (Static linking): Thư viện được gộp trực tiếp vào file nhị phân (tốn chỗ).
- Liên kết động: Việc liên kết bị trì hoãn cho đến thời gian thực thi.
- Thư viện chia sẻ (Shared Libraries):
    - Nhiều tiến trình có thể dùng chung một bản sao duy nhất của DLL trong bộ nhớ chính.
    - Tiết kiệm bộ nhớ và không gian file thực thi.
    - Hỗ trợ cập nhật thư viện dễ dàng mà không cần liên kết lại toàn bộ chương trình.
- Hỗ trợ từ hệ điều hành:
    - Khác với nạp động, liên kết động cần sự giúp đỡ từ OS.
    - OS kiểm tra routine yêu cầu có nằm trong không gian của tiến trình khác hay không.
    - Cho phép nhiều tiến trình truy cập vào cùng một địa chỉ bộ nhớ chứa DLL.

## II. Cấp phát bộ nhớ liên tục:
Nguyên tắc cấp phát liên tục:
- Bộ nhớ thường được chia làm hai phần, một cho hệ điều hành, một cho các tiến trình người dùng.
- Nhiều hệ điều hành (Linux, Windows) đặt OS ở vùng địa chỉ cao.
- Mỗi tiến trình được chứa trong một phần bộ nhớ liên tục kề nhau.

### 1. Bảo vệ bộ nhớ:
- Kết hợp thanh ghi tái định vị (Base) và thanh ghi giới hạn (Limit).
- Thanh ghi Limit chứa phạm vi địa chỉ logic.
- Địa chỉ logic phải nhỏ hơn giá trị trong thanh ghi Limit.
- Dịch địa chỉ & Bảo vệ:
    - Địa chỉ logic được MMU ánh xạ động bằng cách cộng giá trị thanh ghi tái định vị.
    - Khi chuyển ngữ cảnh, dispatcher nạp các giá trị Base/Limit.
    - Ngăn tiến trình đang chạy sửa đổi OS hoặc dữ liệu của user khác.
- Sơ đồ Hỗ trợ phần cứng tái định vị:

![image](https://hackmd.io/_uploads/S1a7xEVF-g.png)

### 2. Cấp phát bộ nhớ:
- Phân vùng kích thước thay đổi (Variable-partition scheme).
- Hệ điều hành duy trì bảng ghi những phần nào trống và bị chiếm.
- Lỗ trống (Hole): Một khối bộ nhớ sẵn dụng có kích thước khác nhau.
- Sơ đồ Biến đổi phân vùng bộ nhớ:

![image](https://hackmd.io/_uploads/H1QAWNNKWe.png)
Trong cấp phát liên tục, tiến trình cần một khối bộ nhớ liên tục. Khi nhiều tiến trình vào/ra, bộ nhớ trống bị chia nhỏ.
- Quản lý lỗ trống (Holes):
    - Khi tiến trình đến: Tìm một lỗ trống đủ lớn để chứa nó.
    - Nếu lỗ trống quá lớn, nó được chia làm hai: Một cấp phát, một trả về tập lỗ trống.
    - Khi tiến trình kết thúc: Giải phóng bộ nhớ, merge với các lỗ trống lân cận.
- Bài toán cấp phát lưu trữ động:
    - Fisrt-fit: Cấp lỗ trống đầu tiên đủ lớn. Tìm kiếm nhanh.
    - Best-fit: Cấp lỗ trống nhỏ nhất mà vẫn đủ lớn. Tạo ra lỗ dư nhỏ nhất.
    - Worst-fit: Cấp lỗ trống lớn nhất. Tạo ra lỗ dư lớn nhất.
- So sánh các chiến lược:
    - First-fit và Best-fit tốt hơn Worst-fit về thời gian và sử dụng không gian.
    - First-fit thường nhanh hơn vì không cần duyệt toàn bộ danh sách.
    - First-fit = "Thấy đủ là lấy"
    - Best-fit = "Khít nhất có thể"
    - Worst-fit = "Lấy cái to nhất"

### 3. Phân mảnh (Fragmentation):
- Phân mảnh ngoại vi (External fragmentation): Tổng bộ nhớ đủ để thoả mãn yêu cầu, nhưng các vùng không liên tục.
    - Xảy ra bên ngoài vùng cấp phát.
    - Tổng bộ nhớ còn nhiều nhưng không dùng được.
    - Thường xảy ra khi tiến trình vào/ra liên tục.
- Phân mảnh nội vi (Internal fragmentation): Bộ nhớ được cấp phát lớn hơn một chút so với yêu cầu (phần dư nằm bên trong phân vùng).
    - Xảy ra khi cấp phát theo kích thước cố định.
    - Tiến trình không dùng hết vùng được cấp.
- Quy tắc 50%:
    - Phân tích thống kê cho thấy với $N$ khối được cấp phát, $N/2$ khối khác bị mất do phân mảnh.
    - Một phần ba bộ nhớ có thể không dùng được.
    - Đây là một vấn đề nghiêm trọng trong cấp phát liên tục.
- Nén bộ nhớ (Compaction):
    - Mục tiêu: Xáo trộn nội dung bộ nhớ để gom tất cả vùng trống về một khối lớn.
    - Chỉ khả thi nếu việc tái định vị là động và được thực hiện lúc chạy.
    - Chi phí di chuyển dữ liệu có thể rất tốn kém.

## III. Phân trang (Paging):
Nguyên lý của Phân trang:
- Cho phép không gian địa chỉ vật lý của tiến trình không liên tục.
- Tránh phân mảnh ngoại vi và nhu cầu nén bộ nhớ.
- Sử dụng trong hầu hết các OS hiện đại (Server, Mobile).

### 1. Phương pháo cơ bản:
- Chia bộ nhớ vật lý thành các khối kích thước cố định gọi là **Khung** (Frames).
- Chia bộ nhớ logic thành các khối cùng kích thước gọi là **Trang** (Pages).
- Địa chỉ từ CPU được chia làm hai phần:
    - Page number (p).
    - Page offset (d).
- Sơ đồ Phần cứng Phân trang:

![image](https://hackmd.io/_uploads/H1w1yDEFWe.png)
- Bảng trang:
    - Số trang (p) dùng làm chỉ số index vào bảng trang.
    - Bảng trang chứa địa chỉ cơ sở của frame tương ứng trong RAM.
    - Kết hợp cơ sở frame với offset (d) để ra địa chỉ vật lý.
- Mô hình bộ nhớ Phân trang:

![image](https://hackmd.io/_uploads/SJDBkwEtZl.png)
- Tính toán địa chỉ Phân trang:
    - Kích thước trang là luỹ thừa của $2$ (thường 4KB - 1GB).
    - Nếu địa chỉ logic là $2^m$ và kích thước trang là $2^n$ thì $m - n$ bit cao chỉ định số trang, $n$ bit thấp chỉ định offset.
- Ưu điểm của Phân trang:
    - Mọi khung trống đều có thể cấp phát cho tiến trình.
    - Không còn phân mảnh ngoại vi.
    - Chỉ còn phân mảnh nội vi ở khung cuối cùng được cấp phát.
- Bảng khung của OS:

![image](https://hackmd.io/_uploads/H19ZlvVt-l.png)
    - OS duy trì **Frame Table** để biết khung nào trống, khung nào bị chiếm.
    - Duy trì một bản sao bảng trang cho mỗi tiến trình để dịch địa chỉ thủ công và xử lý I/O.

### 2. Hỗ trợ phần cứng:
- Bảng trang lớn không thể lưu trong thanh ghi, phải lưu trong RAM.
- PTBR (Page-table Base Register) trỏ vào bảng trang.
- Vấn đề: Cần hai lần truy cập bộ nhớ cho mỗi lệnh (chậm).
- Translation Look-aside Buffer (TLB):
    - Bộ nhớ cache phần cứng đặc biệt, nhanh, so sánh song song.
    - Lưu trữ một phần nhỏ của bảng trang.
    - TLB hit: Frame được tìm thấy ngay, không tốn thêm lần truy cập RAM.
- Sơ đồ Phần cứng Phân trang với TLB:

![image](https://hackmd.io/_uploads/rJWhxPEt-g.png)
- Thời gian truy cập hiệu dụng (EAT):
    - Giả sử RAM tốn 10ns.
    - 80% hit: $EAT = 0.8*10 + 0.2*20 = 12ns$
    - 99% hit: $EAT = 0.99*10 + 0.01*20 = 10.1ns$
    - Các CPU hiện đại có nhiều cấp độ TLB.
### 3. Chia sẻ trang:
- Cho phép chia sẻ mã chung giữa các tiến trình.
> Ví dụ: 40 tiến trình dùng chung thư viện C (`libc`) 2MB. Thay vì 80MB, chỉ tốn 2MB và dữ liệu riêng.
- Yêu cầu: Mã phải là mã Reentrant (không tự sửa đổi).

## IV. Cấu trúc bảng trang cho địa chỉ lớn:
- Phân trang phân cấp (Hierarchical Paging): Phân trang chính bảng trang.
- Bảng trang băm (Hashed Page Tables): Dùng hàm băm cho địa chỉ ảo.
- Bảng trang nghịch đảo (Inverted Page Tables): Một mục cho mỗi khung vật lý.

## V. Hoán đổi (Swapping):
- Tiến trình có thể bị hoán đổi tạm thời ra đĩa (Backing Store).
- Cho phép tổng không gian địa chỉ vượt qua RAM vật lý.
- Hệ thống hiện đại (Linux, Windows) hoán đổi các trang (Paging) thay vì cả tiến trình.
- Swapping trên hệ thống di động:
    - iOS và Android thường không hỗ trợ swapping ra đĩa/flash.
    - Flash có số lần ghi hạn chế và thông lượng kém.
    - OS yêu cầu app giải phóng bộ nhớ hoặc terminate ứng dụng nếu thiếu RAM.
> - Ví dụ: Intel x86-64:
>     - Hỗ trợ địa chỉ ảo 48-bit.
>     - Cấp bậc phân trang bốn cấp độ.
>     - Kích thước trang 4KB, 2MB hoặc 1GB.
> - Ví dụ: ARMv8 (64-bit):
>     - Phổ biến trên điện thoại thông minh và máy tính bảng.
>     - Hỗ trợ kích thước trang 4KB, 16KB hoặc 64KB.
>     - Sử dụng TLB hai cấp độ (Micro-TLB và Main TLB).

## V. Tổng kết chương:
- Bộ nhớ là tập byte có địa chỉ.
- CPU lấy lệnh theo PC.
- MMU dịch địa chỉ logic sang vật lý.
- Cấp phát liên tục dùng thanh ghi Base/Limit, dễ bị phân mảnh.
- Phân trang (Paging) loại bỏ phân mảnh ngoại vi bằng cách dùng khung/trang kích thước cố định.
- TLB tăng hiệu năng dịch địa chỉ.
- Hoán đổi (Swapping) cho phép thực thi đa chương vượt mức RAM.
