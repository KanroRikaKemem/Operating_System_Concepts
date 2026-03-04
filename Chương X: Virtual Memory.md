## I. Giới thiệu chung:
### 1. Bối cảnh và yêu cầu cơ bản:
- Các chiến lược quản lý bộ nhớ yêu cầu mã lệnh phải ở RAM.
- Mã lệnh đang thực thi bắt buộc phải nằm trong RAM vật lý.
- Cách tiếp cận cũ: Nạp toàn bộ không gian địa chỉ logic.
- Hạn chế: Kích thước chương trình bị giới hạn bởi RAM.

### 2. Tại sao không cần nạp toàn bộ?
- Mã xử lý các điều kiện lỗi hiếm khi xảy ra.
- Mảng, danh sách thường được cấp phát dư thừa.
- Các tuỳ chọn/Tính năng của chương trình hiếm khi dùng.
- Ngay cả khi cần cả chương trình, không phải lúc nào cũng cần tất cả cùng lúc.

### 3. Bộ nhớ ảo là gì?
![image](https://hackmd.io/_uploads/r1J37XrF-e.png)
- Là kỹ thuật cho phép thực thi tiến trình không nằm hoàn toàn trong RAM.
- Cho phép chương trình lớn hơn bộ nhớ vật lý.
- Trừu tượng hoá bộ nhớ thành một mảng lưu trữ lớn, đồng nhất.
- Tách biệt bộ nhớ Logic (người lập trình thấy) khỏi bộ nhớ vật lý.

### 4. Lợi ích của bộ nhớ ảo:
- Giải phóng lập trình viên khỏi giới hạn lưu trữ vật lý.
- Tăng mức độ đa chương trình (nhiều tiến trình chạy cùng lúc).
- Tăng hiệu suất CPU và thông lượng (throughput).
- Giảm tải I/O cho việc nạp hoặc hoán đổi tiến trình.

### 5. Không gian địa chỉ ảo (Virtual Address Space):
![image](https://hackmd.io/_uploads/r1x1uEQStWl.png)
- Là cái nhìn logic về cách tiến trình được lưu trữ.
- Thường bắt đầu từ địa chỉ 0 và tồn tại liên tục.
- Đơn vị quản lý bộ nhớ (MMU) ánh xạ trang ảo sang khung vật lý.
- RAM vật lý thực tế có thể không liên tục (sử dụng Page frames).

### 6. Cấu trúc của một tiến trình:
![image](https://hackmd.io/_uploads/r1x1uEQStWl.png)
- **Stack:** Phát triển xuống dưới (cho các lời gọi hàm).
- **Heap:** Phát triển lên trên (cấp phát động).
- Khoảng trống (Hole) giữa Heap và Stack gọi là **Sparse Address Spaces**.
- Dữ liệu (Data) và Mã lệnh (Text) nằm ở vùng thấp.

### 7. Chia sẻ File và Bộ nhớ:
![image](https://hackmd.io/_uploads/BkEBBmrtZx.png)
- Thư viện hệ thống (như C library) có thể chia sẻ giữa nhiều tiến trình.
- Mỗi tiến trình coi thư viện là một phần không gian ảo của mình.
- Trang vật lý thực tế được chia sẻ bởi tất cả tiến trình.
- Tăng tốc độ tạo tiến trình qua lời gọi `fork()`.

## II. Nạp trang theo yêu cầu:
### 1. Chiến lược nạp trang khi cần:
- Nạp các trang vào bộ nhớ chỉ khi chúng được yêu cầu.
- Tránh nạp những trang không bao giờ được truy cập.
- Tương tự như phân trang với hoán đổi (Swapping).
- Dùng "Lazy Swapper" (bộ hoán đổi lười biếng).

### 2. Phân biệt trang trong RAM và đĩa:
- Bảng trang (Page Table) dùng bit **Valid-Invalid**.
    - **Valid (v):** Trang hợp lệ và đang nằm trong RAM.
    - **Invalid (i):** Trang không hợp lệ hoặc đang nằm ở thiết bị lưu trữ.
- Mặc định ban đầu, tất cả các mục nhập được đặt là `i`.

### 3. Truy cập trang "Invalid":
- Nếu tiến trình truy cập trang có bit `i`, xảy ra **Page Fault** (Lỗi trang).
- Phần cứng MMU nhận thấy bit `i` và gây ra Trap cho OS.
- Lỗi này không nhất thiết là lỗi chương trình mà là tín hiệu cần nạp trang.
- Xử lý lỗi trang:

![image](https://hackmd.io/_uploads/rJzZtXrYZl.png)
    - Bước 1: OS kiểm tra bảng nội bộ (thường trong PCB) để xác định truy cập hợp lệ hay không.
    - Bước 2: Nếu truy cập không hợp lệ thì kết thúc tiến trình. Nếu hợp lệ nhưng chưa nạp thì tiến hành nạp.
    - Bước 3: Tìm một khung trang trống (Free frame).
    - Bước 4: Lập lịch thao tác đĩa để đọc trang cần thiết vào khung vừa tìm được.
    - Bước 5: Khi đọc xong, cập nhật bảng nội bộ và bảng trang (đổi bit thành `v`).
    - Bước 6: Khởi động lại lệnh bị gián đoạn trước đó.
- Kết quả:
    - Tiến trình giờ đây có thể truy cập trang như thể nó đã có sẵn trong bộ nhớ.
    - Trường hợp cực đoan: Chạy tiến trình với không có trang nào trong RAM (Pure demand paging).
    - Tiến trình sẽ gây lỗi trang ngay lập tức cho lệnh đầu tiên.
- Tại sao hiệu suất vẫn tốt?
    - Về lý thuyết, một lệnh có thể gây ra nhiều lỗi trang (lệnh + dữ liệu).
    - Thực tế: Chương trình có **Tính cục bộ tham chiếu**.
    - Các lệnh và dữ liệu thường tập trung ở một vùng trong một khoảng thời gian.
    - Điều này giúp Demand Paging đạt hiệu suất chấp nhận được.
- Yêu cầu về phần cứng:
    - Bảng trang: Có bit Valid-Invalid.
    - Bộ nhớ phụ: HDD hoặc NVM tốc độ cao để lưu các trang (Swap Space).
    - Khả năng **khởi động lại lệnh** sau lỗi trang là yêu cầu tối thiểu quan trọng.
- Thách thức khi khởi động lại lệnh:
    - OS phải lưu trạng thái (thanh ghi, mã điều kiện) của tiến trình bị ngắt.
    - Lỗi trang có thể xảy ra khi lấy lệnh (fetch) hoặc lấy toán hạng (operand).
    - Thách thức lớn: Các lệnh thay đổi nhiều vị trí bộ nhớ (Ví dụ: Lệnh MVC của IBM 360).
    - Giải pháp: Kiểm tra cả hai đầu khối trước khi thực hiện hoặc dùng thanh ghi tạm.

#### 4. Free-Frame List:
![image](https://hackmd.io/_uploads/ByFHj7HtWg.png)
- Hầu hết OS duy trì một "pool" khung trang tự do để phục vụ lỗi trang.
- Dùng kỹ thuật **Zero-fill-on-demand** (xoá sạch dữ liệu cũ trước khi cập nhật).
- Khi hệ thống khởi động, tất cả RAM được đưa vào danh sách này.
- Danh sách co lại khi tiến trình yêu cầu trang.
> Ví dụ: Process cần một page, frame 7 không còn trong free list nữa.

#### 5. Hiệu suất: Effective Access Time (EAT):
Là thời gian truy cập bộ nhớ trung bình thực tế, tính cả trường hợp bị page fault. $$EAT = (1 - p)*ma + p*Page\_Fault\_Time$$ Trong đó:
- $p$: Tỉ lệ lỗi trang ($0 \leq p \leq 1$)
- $ma$: Thời gian truy cập bộ nhớ (Memory Access Time).
- Thời gian xử lý lỗi trang gồm ba phần:
    - Ngắt.
    - Đọc trang.
    - Khởi động lại.
> Ví dụ về sự sụt giảm hiệu suất: Giả sử $ma = 200ns$, $Page\_Fault\_Time = 8ms$, $p =$ Tỉ lệ Page Fault. $$EAT = (1 - P)*200 + p(8ms) = (1 - p)*200 + p*8000000 = 200 + 7999800*p$$ Điều đó có nghĩa là chỉ cần tăng $p$ thêm một lỗi/một triệu lần thì $EAT$ tăng gần tám triệu ns. Tức là:
> - Truy cập RAM: $20ns$
> - Dính page fault: Đắt gấp ~ $40000$ lần

#### 6. Quản lý không gian hoán đổi:
OS phải làm mọi cách để tránh page fault không cần thiết:
- I/O cho Swap Space nhanh hơn I/O hệ thống file (Do dùng khối lớn hơn).
- Tuỳ chọn: Copy toàn bộ file nhị phân vào Swap khi bắt đầu.
- Tuỳ chọn: Chỉ dùng Swap cho bộ nhớ ẩn danh (stack, heap).
- Mobile OS (iOS, Android): Thường không hỗ trợ hoán đổi trang mà thu hồi bộ nhớ read-only.

## III. Sao chép khi ghi:
![image](https://hackmd.io/_uploads/HkeY0XHKWl.png)
![image](https://hackmd.io/_uploads/SynDCQrFbg.png)
- Đừng copy gì nếu chưa cần ghi.
- Tối ưu hoá `fork()`:
    - `fork()` truyền thống copy toàn bộ không gian địa chỉ cha sang con.
    - Copy-on-Write (COW): Cha và con ban đầu chia sẻ cùng trang.
    - Trang được đánh dấu là "copy-on-write".
    - Nếu bất kỳ tiến trình nào thực hiện ghi vào trang chia sẻ thì tạo bản sao.
- Lợi ích của COW:
    - Giảm thiểu số lượng trang mới được cấp phát.
    - Tạo tiến trình cực nhanh.
    - Chỉ các trang bị sửa đổi mới cần bản sao riêng.
    - Sử dụng trong Windows, Linux, mac0S.
    - `vfork()`: Một biến thể hiệu quả hơn nhưng cần thận trọng (không dùng COW).

## IV. Thay thế trang:
### 1. Nhu cầu thay thế trang:
![image](https://hackmd.io/_uploads/HkagBOBK-x.png)
- Mức độ đa chương trình tăng $\rightarrow$ Bộ nhớ vật lý bị cạn kiệt.
- Lỗi trang xảy ra nhưng không còn khung trang trống.
- OS phải chọn một trang trong RAM để loại bỏ để nạp trang mới.
- Trang bị loại bỏ được gọi là **Trang nạn nhân (Victim page)**.

### 2. Giảm thiểu Overhead thay thế:
![image](https://hackmd.io/_uploads/ByndB_HK-g.png)
- Dùng Modify bit (Dirty bit) cho mỗi trang.
- Nếu trang chưa bị sửa (dirty = 0): Chỉ việc xoá khỏi RAM (tiết kiệm một lần ghi đĩa).
- Nếu trang bị sửa (dirty = 1): Bắt buộc phải ghi lại vào bộ nhớ phụ.
- Giúp giảm thời gian phục vụ lỗi trang xuống một nửa trong nhiều trường hợp.

### 3. Tiêu chí đánh giá thuật toán thay thế:
- Dùng chuỗi tham chiếu.
- Theo dõi số lỗi trang ứng với một số lượng khung trang cố định.
- Nguyên tắc: Số khung trang tăng thì số lỗi trang giảm.
- Ngoại lệ: Nghịch lý Belady.

### 4. First-In, First-Out (FIFO):
![image](https://hackmd.io/_uploads/SJd4P_SF-x.png)
- Khi xảy ra page fault, RAM đã đầy, FIFO chọn trang cũ nhất (nạp vào RAM sớm nhất) để thay thế.
- Dễ hiểu, dễ cài đặt bằng hàng đợi FIFO.
- Nhược điểm: Trang cũ có thể là trang quan trọng đang được dùng liên tục.
- Nghịch lý Belady: Tỉ lệ lỗi trang có thể tăng khi được cấp thêm khung trang.
> Ví dụ: Chuỗi 1, 2, 3, 4, 1, 2, 5, 1, 2, 3, 4, 5:
> 
> ![image](https://hackmd.io/_uploads/B1s2L_BYWe.png)
> ![image](https://hackmd.io/_uploads/Sk3TL_HYWl.png)

### 5. Optimal Algorithm (OPT):
![image](https://hackmd.io/_uploads/BJ-LvuBY-l.png)
- Thay thế trang sẽ không được dùng trong thời gian lâu nhất tương lai.
- Đảm bảo tỉ lệ lỗi trang thấp nhất có thể.
- Không bị nghịch lý Belady.
- Bất khả thi trong thực tế (vì yêu cầu biết trước tương lai).
- Dùng làm chuẩn để so sánh hiệu quả các thuật toán khác.

### 6. Least Recently Used (LRU):
![image](https://hackmd.io/_uploads/BJzhvdrtWl.png)
- Thay thế trang đã không được truy cập trong thời gian lâu nhất quá khứ.
- Dựa trên giả định: Quá khứ gần phản ánh tương lai gần.
- Được coi là một thuật toán tốt, không bị Belady.
- Cài đặt yêu cầu hỗ trợ phần cứng đáng kể.

#### LRU sử dụng bộ đếm (Counters):
- Mỗi mục bảng trang có một trường "Thời gian sử dụng".
- Khi trang được tham chiếu, copy "Logical Clock" vào trường này.
- Khi cần thay thế: Tìm trang có giá trị thời gian nhỏ nhất.
- Hạn chế: Phải tìm kiếm trong bảng trang mỗi lần thay thế.

#### LRU sử dụng ngăn xếp (Stack):
![image](https://hackmd.io/_uploads/SJ___OrK-l.png)
- Duy trì một Stack các số trang (dưới dạng danh sách liên kết đôi).
- Khi trang được dùng: Đưa nó lên đỉnh stack.
- Trang ở đáy stack là trang LRU.
- Ưu điểm: Không cần tìm kiếm khi thay thế.
- Nhược điểm: Mỗi lần truy cập tốn chi phí cập nhật sáu con trỏ.

#### Xấp xỉ LRU - Kỹ thuật Bit Tham chiếu:
- Phần cứng hỗ trợ một Reference Bit cho mỗi trang.
- Bit được đặt thành `1` mỗi khi trang được đọc/ghi.
- Định kỳ, OS xoá bit này về `0`.
- Dùng để biết trang nào được/không được dùng trong một khoảng thời gian.

#### Second-Chance Algorithm (Clock):
- Cải tiến từ FIFO + Bit Tham chiếu.
- Dùng con trỏ vòng tròn (Như kim đồng hồ).
- Nếu Bit = 0: Thay thế trang.
- Nếu Bit = 1: Đặt lại Bit = 0, cho "cơ hội thứ hai" và chuyển sang trang tiếp theo.

#### Enhanced Second-Chance:
![image](https://hackmd.io/_uploads/BkZJqOBFWg.png)
Kết hợp cả Reference bit và Modify bit thành cặp `(R, M)`:
- `(0, 0)`: Không dùng gần đây, không sửa (Tốt nhất để thay thế).
- `(0, 1)`: Không dùng gần đây nhưng bị sửa (Cần ghi đĩa trước).
- `(1, 0)`: Có dùng gần đây, không sửa.
- `(1, 1)`: Có dùng, có sửa (Ưu tiên giữ lại nhất).

#### Xem thêm: LFU và MFU:
- Least Frequently Used (LFU): Thay thế trang có số lần đếm thấp nhất.
- Most Frequently Used (MFU): Dựa trên lý luận trang đếm thấp nhất là trang mới nạp và chưa kịp dùng.
- Cả hai đều không phổ biến vì chi phí cao và không xấp xỉ tốt thuật toán tối ưu.

## V. Cấp phát khung trang:
### 1. Minimum Number of Frames:
- Mỗi tiến trình cần một số khung trang tối thiểu để thực thi.
- Phụ thuộc vào kiến trúc máy tính.
- Nếu cấp phát dưới mức này thì tiến trình gây lỗi trang liên tục và không thể hoàn thành.

### 2. Equal vs Proportional Allocation:
- Equal Allocation: Chia đều khung trang cho $n$ tiến trình.
- Proportional Allocation: Chia theo kích thước tiến trình.
- Công thức: $$S = \displaystyle \sum{S_i}$$ $$a_i = s_i/S \times m$$
- Tiến trình lớn hơn sẽ được cấp nhiều RAM hơn để giảm Page Fault.

### 3. Global vs Local Replacement:
- Global Replacement: Chọn trang nạn nhân từ bất kỳ tiến trình nào (có thể lấy của tiến trình khác).
- Local Replacement: Chỉ chọn từ các khung trang đã cấp cho chính nó.
- Global thường cho thông lượng hệ thống tốt hơn nhưng làm hiệu suất tiến trình khó dự đoán.

## VI. Hiện tượng Trì trệ - Thrashing:
![image](https://hackmd.io/_uploads/SJ5ylKrtWx.png)
- Xảy ra khi tiến trình dành nhiều thời gian cho Paging hơn là thực thi mã.
- Nguyên nhân: Tiến trình không có đủ khung trang để chứa Working Set.
- Hệ quả: Hiệu suất hệ thống sụt giảm thê thảm.
- CPU Scheduler thấy hiệu suất thấp lại nạp thêm tiến trình nên càng nặng thêm Thrashing.
- Biện pháp:
    - Working-Set Model:
  
    ![image](https://hackmd.io/_uploads/H1TxxKSF-l.png)
        - Dựa trên giả định về Tính cục bộ.
        - Working-set Window ($\Delta$): Xét các tham chiếu trang trong $\Delta$ thời gian gần nhất.
        - WSS: Số lượng trang duy nhất trong cửa sổ đó.
        - Tổng nhu cầu RAM: $D = \displaystyle \sum{WSS_i}$
        - Chống Thrashing với Working-Set:
            - Nếu $D > M >$ RAM khả dụng): Thrashing sẽ xảy ra.
            - Chiến lược: OS theo dõi WSS của mỗi tiến trình.
            - Nếu RAM không đủ đáp ứng Working Set thì OS tạm đình chỉ một tiến trình để giải phóng khung trang.
            - Giúp tối ưu hoá sử dụng CPU mà không gây trì trệ.
    - Page-Fault Frequency (PFF):
        - Cách tiếp cận trực tiếp hơn để kiểm soát Thrashing.
        - Thiết lập ngưỡng trên và ngưỡng dưới cho tỉ lệ lỗi trang.
        - Nếu tỉ lệ lỗi trang vượt ngưỡng trên thì cấp thêm khung trang.
        - Nếu dưới ngưỡng dưới thì thu hồi khung trang.

## VII. Cấp phát bộ nhớ Kernel:
### 1. Yêu cầu riêng của Kernel:
- Thường yêu cầu bộ nhớ cho các cấu trúc dữ liệu nhỏ (ít hơn một trang).
- Cần tiết kiệm bộ nhớ, tối thiểu hoá phân mảnh.
- Một số thiết bị phần cứng yêu cầu bộ nhớ phải liên tục về vật lý.
- Do đó, Kernel không dùng cơ chế nạp trang thông thường như User.

### 2. Cơ chế Buddy System:
- Cấp phát bộ nhớ theo luỹ thừa của $2$ (4KB, 8KB, 16KB,...).
- Chia một khối lớn thành hai khối "buddy" nhỏ hơn cho đến khi vừa đủ nhu cầu.
- Khi giải phóng: Coalescing (gộp các buddy liền kề lại thành khối lớn).
- Ưu điểm: Gộp bộ nhớ nhanh.
- Nhược điểm: Phân mảnh nội bộ lớn.

### 3. Cơ chế Slav Allocation:
Là cơ chế quản lý bộ nhớ của Kernel, trong đó mỗi loại cấu trúc dữ liệu có một cache riêng gồm nhiều slab, mỗi slab chứa các object cùng loại, nhằm giảm phân mảnh và tăng tốc độ cấp phát bộ nhớ.
- Slab: Một hoạt nhiều trang vật lý liên tục.
- Cache: Chứa một hoặc nhiều Slab.
- Mỗi loại cấu trúc dữ liệu Kernel (ví dụ process descriptor) có một cache riêng.
- Cache được lấp đầy bởi các "Objects" (Các thực thể của cấu trúc đó).
- Lợi ích của slab:
    - Không phân mảnh: Mỗi object có kích thước đúng bằng nhu cầu.
    - Cực nhanh: Objects được khởi tạo trong cache, chỉ việc lấy ra dùng và trả lại.
    - Dùng trong Solaris, Linux (SLUB allocator là mặc định hiện nay).

## VIII. Các vấn đề khác:
### 1. Kỹ thuật Prepaging:
- Nỗ lực ngăn chặn số lượng lỗi trang lớn khi bắt đầu tiến trình.
- OS nạp trước tất cả hoặc một phần các trang dự kiến sẽ cần.
- Nếu các trang được nạp không được dùng thì lãng phí I/O và RAM.
- Sử dụng khi chi phí nạp trước thấp hơn chi phí xử lý nhiều Page Fault đơn lẻ.

### 2. Lựa chọn kích thước trang:
- Trang nhỏ: Ít phân mảnh nội bộ, khớp tính cục bộ tốt hơn.
- Trang lớn: Bảng trang nhỏ hơn, I/O hiệu quả hơn (Đọc một khối lớn nhanh hơn nhiều khối nhỏ).
- Xu hướng hiện đại: Kích thước trang ngày càng tăng (Ví dụ: Huge pages 2MB, 1GB).

### 3. TLB Reach:
- Là lượng bộ nhớ có thể truy cập từ TLB: $TLB\_Reach = TLB\_Size \times Page\_Size$
- Lý tưởng: Working Set nằm gọn trong TLB Reach.
- Nếu không: Xảy ra TLB Miss liên tục.
- Giải pháp: Tăng kích thước trang cho các ứng dụng (Ví dụ: Database).

## IX. Tổng kết:
- Bộ nhớ ảo tách biệt địa chỉ logic và vật lý, cho phép chạy chương trình lớn hơn RAM.
- Demand Paging và thay thế trang (LRU, OPT) là các cơ chế cốt lõi.
- Phân bổ khung trang hợp lý giúp ngăn chặn Thrashing.
- Kernel cần các cơ chế cấp phát riêng như Buddy và Slab.
