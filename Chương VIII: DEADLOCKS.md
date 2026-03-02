> Tắc nghẽn trong Hệ thống Đa chương trình

##### Deadlock là gì?
- Một tập hợp các luồng bị tắc nghẽn khi mọi luồng trong tập hợp đều đang chờ một sự kiện.
- Sự kiện này chỉ có thể được gây ra bởi một luồng khác trong cùng tập hợp đó.
- Các luồng không thể tiếp tục thực thi và tài nguyên bị chiếm giữ vô ích.
> Minh hoạ thực tế - Luật Kansas: Khi hai tàu hoả tiến đến chỗ giao nhau, cả hai phải dừng lại hoàn toàn và không tàu nào được đi tiếp cho đến khi tàu kia đi rồi $\rightarrow$ Cả hai tàu dừng lại vĩnh viễn.

##### Thách thức trong hệ thống hiện đại:
- Nhu cầu tăng tính đồng thời và song song trên các hệ thống đa lõi (multicorte).
- Số lượng tài nguyên và luồng tăng mạnh làm tăng nguy cơ tắc nghẽn.
- Hệ điều hành thường không cung cấp sẵn các công cụ ngăn ngừa tự động.

## I. Mô hình hệ thống:
- Hệ thống bao gồm một số lượng tài nguyên hữu hạn.
- Các tài nguyên được phân chia thành nhiều loại (classes/types).
> CPU, không gian bộ nhớ, tệp tin, thiết bị I/O,...
- Mỗi loại tài nguyên R có một số thực thể (instances) đồng nhất.
> Nếu hệ thống có 4 CPU, loại tài nguyên CPU có 4 thực thể.
- Bất kỳ thực thể nào của loại đó đều có thể thoả mãn yêu cầu của luồng.
- Các công cụ đồng bộ hoá (mutex locks, semaphores) cũng là tài nguyên hệ thống.
- Đây là nguồn gây tắc nghẽn phổ biến nhất trong các hệ thống hiện đại.
- Mỗi khoá thường được gán cho một loại tài nguyên riêng biệt.

### 1. Chu trình sử dụng tài nguyên:
Một luồng phải sử dụng tài nguyên theo trình tự:
- Yêu cầu (Request): Xin cấp tài nguyên. Nếu không có sẵn, luồng phải chờ.
- Sử dụng (Use): Thao tác trên tài nguyên.
- Giải phóng (Release): Trả lại tài nguyên.

### 2. Yêu cầu và Giải phóng tài nguyên:
- Thông qua các lời gọi hệ thống: `request()`/`release()` thiết bị, `open()`/`close()` tệp, `allocate()`/`free()` bộ nhớ.
- Hoặc qua các thao tác đồng bộ: `wait()`/`signal()` trên semaphore, `acquire()`/`release()` trên mutex.

### 3. Cách hệ điều hành quản lý tài nguyên:
- Hệ điều hành duy trì một bảng hệ thống ghi lại:
    - Tài nguyên nào là tự do hay đã được cấp phát.
    - Tài nguyên đã cấp phát được giao cho luồng nào.
- Nếu luồng yêu cầu tài nguyên đang bận, nó sẽ được đưa vào hàng đợi.

## II. Tắc nghẽn trong ứng dụng đa luồng:
### 1. Tắc nghẽn với POSIX Mutex:
Giả sử có hai mutex locks được tạo:
``` code
pthread_mutex_t first_mutex;
pthread_mutex_t second_mutex;
pthread_mutex_init(&first_mutex, NULL);
pthread_mutex_init(&second_mutex, NULL);
```
- Thread 1: Yêu cầu locks theo thứ tự: (1) `first_mutex`, (2) `second_mutex`.
- Thread 2: Yêu cầu locks theo thứ tự: (1) `second_mutex`, (2) `first_mutex`.
- Tắc nghẽn xảy ra nếu luồng 1 giữ khoá 1 và luồng 2 giữ khoá 2 cùng lúc.

#### Luồng 1 (do_work_one):
``` code
pthread_mutex_lock(&first_mutex, NULL);
pthread_mutex_lock(&second_mutex, NULL);
pthread_mutex_unlock(&second_mutex, NULL);
pthread_mutex_unlock(&first_mutex, NULL);
```

#### Luồng 2 (do_work_two):
``` code
pthread_mutex_lock(&second_mutex, NULL);
pthread_mutex_lock(&first_mutex, NULL);
pthread_mutex_unlock(&first_mutex, NULL);
pthread_mutex_unlock(&second_mutex, NULL);
```

### 2. Tắc nghẽn và bộ điều phối (Scheduler):
- Tắc nghẽn có thể không xảy ra nếu một luồng hoàn thành trước khi luồng kia bắt đầu.
- Thứ tự chạy của các luồng phụ thuộc hoàn toàn vào bộ điều phối CPU.
- Điều này khiến việc phát hiện và kiểm thử tắc nghẽn trở nên cực kì khó khăn.

## III. Livelock:
- Là một dạng thất bại tính sống tương tự như tắc nghẽn.
- Các luồng không bị chặn nhưng không thể tiến triển.
- Thường xảy ra khi luồng liên tục thực hiện một hành động lặp đi lặp lại nhưng thất bại.
> Minh hoạ thực tế: Hành lang:
> - Hai người đi ngược chiều nhau trong hành lang.
> - Một người tránh sang phải, người kia cũng tránh sang phải tương ứng.
> - Họ tiếp tục đổi hướng để tránh nhau nhưng vẫn chắn đường nhau.
> - Họ không bị đứng yên nhưng không thể đi tiếp được.
- Livelock trong lập trình:
    - Dùng hàm `pthread_mutex_trylock()` (cố lấy khoá mà không bị chặn).
    - Nếu luồng lấy được khoá 1 nhưng không lấy được khoá 2, nó giải phóng khoá 1 và thử lại từ đầu.
    - Nếu hai luồng cùng làm việc này cùng lúc, chúng sẽ liên tục lấy khoá rồi nhả khoá vô tận.
    - Điều này tạo ra một vòng lặp không tiến triển mặc dù các luồng vẫn đang "chạy".
- Cách tránh livelock:
    - Yêu cầu các luồng thực hiện lại các thao tác thất bại tại các thời điểm ngẫu nhiên.
    - Cơ chế này giống như giải quyết va chạm (collision) trong mạng Ethernet.

## IV. Đặc điểm của tắc nghẽn:
### 1. Bốn điều kiện cần (Necessary Conditions):
Tắc nghẽn xảy ra nếu và chỉ nếu bốn điều kiện sau đồng thời xảy ra:
#### Loại trừ tương hỗ (Mutural exclusion):
- Ít nhất một tài nguyên phải được giữ ở chế độ không chia sẻ.
- Chỉ một luồng tại một thời điểm có thể sử dụng tài nguyên đó.
- Nếu luồng khác yêu cầu, nó phải đợi cho đến khi tài nguyên được giải phóng.

#### Giữ và đợi (Hold and wait):
Một luồng phải đang giữ ít nhất một tài nguyên và đang đợi để có thêm tài nguyên khác hiện đang bị luồng khác chiếm giữ.

#### Không chiếm dụng (No preemption):
- Tài nguyên không thể bị thu hồi (chiếm dụng).
- Tài nguyên chỉ có thể được giải phóng tự nguyện bởi luồng đang giữ nó sau khi hoàn thành nhiệm vụ.

#### Chờ đợi vòng lặp (Circular wait):
- Tồn tại một tập hợp $\{T_0, T_1,..., T_n\}$ các luồng đang chờ.
- $T_0$ chờ tài nguyên của $T_1$, $T_1$ chờ $T_2$,..., $T_n$ chờ tài nguyên của $T_0$.
$\rightarrow$ Tạo thành một chu kỳ chờ khép kín.

### 2. Đồ thị cấp phát tài nguyên (RAG):
- Dùng để mô tả chính xác tình trạng tắc nghẽn.
- Gồm tập các đỉnh $V$ và tập các cạnh $E$.
- $V$ được chia thành hai loại:
    - $T = \{T_1, T_2,..., T_n\}$: Tập các luồng.
    - $R = \{R_1, R_2,..., R_m\}$: Tập các loại tài nguyên.

### 3. Cạnh trong đồ thị RAG:
- Cạnh yêu cầu (Request edge): $T_i \rightarrow R_j$. Luồng $T_i$ đang đợi một thực thể của loại $R_j$.
- Cạnh cấp phát (Assignment edge): $R_j \rightarrow T_i$. Một thực thể của loại $R_j$ đã được giao cho luồng $T_i$.

### 4. Biểu diễn RAG (ký hiệu):
![image](https://hackmd.io/_uploads/ByKOTmmt-e.png)
- Luồng $T_i$: Hình tròn.
- Loại tài nguyên $R_j$: Hình chữ nhật.
- Thực thể của $R_j$: Các dấu chấm trong hình chữ nhật $R_j$.

### 5. Chu trình và Tắc nghẽn (Quy tắc):
![image](https://hackmd.io/_uploads/SytEA77tbl.png)
- Nếu đồ thị không có chu trình thì không có tắc nghẽn.
- Nếu đồ thị có chu trình:
    - Mỗi loại tài nguyên chỉ có một thực thể: Có tắc nghẽn.
    - Mỗi loại có nhiều thực thể: Có thể có tắc nghẽn hoặc không.
> Ví dụ: Chu trình không gây tắc nghẽn: Chu trình tồn tại nhưng một luồng ngoài chu trình giải phóng tài nguyên, phá vỡ thế bế tắc.
> 
> ![image](https://hackmd.io/_uploads/rJLXR7XKbg.png)

## V. Các phương pháp xử lý:
Ba phương pháp xử lý chính:
- Bỏ qua vấn đề (Giả vờ như chưa từng xảy ra): Phương pháp "Bỏ qua" (Ostrich Algorithm):
    - Dùng cho Linux và Windows.
    - Lý do: Tắc nghẽn ít khi xảy ra, các phương pháp khác quá tốn kém.
    - Nếu xảy ra tắc nghẽn kéo dài, hiệu năng hệ thống sẽ giảm sút và cuối cùng cần khởi động lại thủ công.
- Ngăn ngừa hoặc Tránh (Đảm bảo hệ thống không bao giờ bị tắc nghẽn): Ngăn ngừa tắc nghẽn (Deadlock Prevention):
    - Đảm bảo rằng ít nhất một trong bốn điều kiện cần không bao giờ xảy ra.
    - Hạn chế cách thức luồng đưa ra yêu cầu tài nguyên.
- Phá bỏ Loại trừ tương hỗ:
    - Tài nguyên có thể chia sẻ (file read-only,...) không gây tắc nghẽn.
    - Tuy nhiên nhiều tài nguyên bản chất là không chia sẻ được (mutex lock).
    $\rightarrow$ Khó thực hiện triệt để.
        - Phá bỏ Giữ và Đợi:
            - Giao thức: Mỗi luồng phải yêu cầy tất cả tài nguyên trước khi bắt đầu.
            - Hoặc: Luồng chỉ được yêu cầu tài nguyên mới khi không giữ bất kỳ tài nguyên nào.
            - Hạn chế: Hiệu suất thấp, nguy cơ đói tài nguyên (Starvation).
        - Phá bỏ Không chiếm dụng:
            - Nếu luồng đang giữ tài nguyên yêu cầu cái mới mà phải đợi, tất cả tài nguyên nó đang giữ bị thu hồi (preempted).
            - Áp dụng tốt cho CPU registers và database transactions.
        - Phá bỏ Chờ đợi vòng lặp (Hierachy):
            - Gán số thứ tự cho mọi tài nguyên.
            - Luồng phải yêu cầu tài nguyên theo thứ tự tăng dần.
            - Đây là giải pháp thực tế nhất cho các nhà phát triển ứng dụng.
    - Tránh tắc nghẽn (Deadlock Avoidance):
        - Cần thông tin trước về việc các tài nguyên sẽ được yêu cầu như thế nào.
        - Hệ điều hành dùng thông tin này để quyết định có cấp phát ngay hay bắt luồng phải chờ để giữ hệ thống an toàn.
- Cho phép xảy ra, phát hiện và phục hồi.

### 1. Trạng thái an toàn (Safe State):
- Trạng thái là an toàn nếu tồn tại một **Chuỗi an toàn** (Safe sequence).
- Chuỗi an toàn đảm bảo mọi luồng đều có thể hoàn thành công việc với tài nguyên hiện có và tài nguyên của các luồng trước đó.
- Safe >< Unsafe >< Deadlock:

![image](https://hackmd.io/_uploads/Hy9_G4XY-l.png)
    - An toàn là không có tắc nghẽn.
    - Không an toàn là có thể xảy ra tắc nghẽn.
    - Hệ điều hành tránh rơi vào trạng thái không an toàn để ngăn Deadlock.

### 2. Thuật toán Banker:
- Áp dụng cho hệ thống nhiều thực thể tài nguyên.
- Tên gọi xuất phát từ hệ thống ngân hàng: Không bao giờ cho vay hết tiền mặt nếu không đảm bảo trả lời được cho mọi khách hàng.
- Khi luồng mới vào, nó phải khai báo số nhu cầu tối đa.
- Dữ liệu trong Banker:

![image](https://hackmd.io/_uploads/SkBMmVXY-e.png)
    - **Available:** Vector $m$, số tài nguyên còn trống mỗi loại.
    - **Max:** Ma trận $n$ x $m$, nhu cầu tối đa của mỗi luồng.
    - **Allocation:** Ma trận $n$ x $m$, tài nguyên hiện đã cấp cho luồng.
    - **Need:** Ma trận $n$ x $m$, tài nguyên còn thiếu (**Need = Max - Allocation**).
- Safety Algorithm (Kiểm tra an toàn):
    - Gán `Work = Available`, `Finish[i] = false`.
    - Tìm `i` sao cho `Finish[i] == false` và `Need[i] <= Work`.
    - `Work = Work + Allocation[i]`, `Finish[i] = true`.
    - Nếu mọi `Finish[i] == true`: An toàn.
- Resource-Request Algorithm:
    - Nếu `Request_i <= Need_i`, sang bước 2.
    - Nếu `Request_i <= Available`, sang bước 3.
    - Giả định cấp phát tài nguyên, chạy thuật toán an toàn:
        - Nếu an toàn: Hoàn tất cấp phát.
        - Nếu không an toàn: Luồng phải chờ, khôi phục trạng thái cũ.

## VI. Phát hiện tắc nghẽn:
- Nếu hệ thống không dùng Ngăn ngừa hay Tránh, nó cần:
    - Thuật toán kiểm tra trạng thái hệ thống định kỳ.
    - Cơ chế để phục hồi sau khi phát hiện.
- Phát hiện: Đơn thực thể tài nguyên:
    - Dùng đồ thị Wait-for-graph (biến thể của RAG).
    - Loại bỏ các nút tài nguyên, vẽ cạnh tranh tiếp giữa các luồng.
    - Tắc nghẽn tồn tại nếu và chỉ nếu đồ thị có chu trình.
- Phát hiện: Đa thực thể tài nguyên:
    - Sử dụng cấu trúc tương tự thuật toán Banker.
    - Khác biệt: Không cần ma trận Max, tập trung vào ma trận Request hiện tại.
    - Thuật toán tìm xem có chuỗi nào giải quyết được mọi yêu cầu không.
- Tần suất chạy thuật toán phát hiện:
    - Phụ thuộc vào:
        - Khả năng xảy ra tắc nghẽn thường xuyên hay không.
        - Có bao nhiêu luồng bị ảnh hưởng khi bế tắc xảy ra.
    - Có thể chạy mỗi khi yêu cầu không được đáp ứng ngay, hoặc định kỳ (mỗi giờ, khi CPU rảnh,...).

## VII. Phục hồi tắc nghẽn:
### 1. Tổng quan phục hồi (Recovery):
Hai phương án chính:

#### Kết thúc luồng (Thread Termination):
- Huỷ tất cả các luồng bế tắc: Nhanh nhưng tốn kém (phải tính toán lại từ đầu).
- Huỷ từng luồng một: Chạy lại thuật toán phát hiện sau mỗi lần huỷ cho đến khi hết bế tắc.
- Lựa chọn luồng để kết thúc:
    - Độ ưu tiên của luồng.
    - Thời gian luồng đã chạy và thời gian còn lại.
    - Tài nguyên luồng đã sử dụng.
    - Cần huỷ bao nhiêu luồng nữa để thoát bế tắc.

#### Chiếm dụng tài nguyên (Resource Preemption):
Thu hồi tài nguyen từ một luồng và giao nó cho luồng khác. Cần giải quyết:
- Chọn nạn nhân (Selecting a victim): Giảm thiểu chi phí.
- Quay lui (Rollback): Trở về trạng thái an toàn gần nhất.
    - Khó để xác định trạng thái an toàn cụ thể để quay lại.
    - Giải pháp đơn giản: Tổng rollback (Huỷ luồng và chạy lại từ đầu).
    - Giải pháp nâng cao: Giữ lại "điểm kiểm tra" (checkpoints) nhưng tốn bộ nhớ.
- Đói tài nguyên (Starvation): Đảm bảo một luồng không bị thu hồi tài nguyên vĩnh viễn.
    - Nếu hệ thống chỉ dựa trên chi phí, cùng một luồng có thể bị chọn làm nạn nhân mãi mãi.
    - Giải pháp: Bao gồm số lần bị thu hồi/quay lui vào trong công thức tính toán chi phí (tăng dần chi phí chọn nó).

## VIII. Tổng kết chương:
- Tắc nghẽn xảy ra khi mọi luồng trong nhóm chờ nhau vĩnh viễn.
- Bốn điều kiện cần:
    - Loại trừ tương hỗ.
    - Giữ và Đợi.
    - Không chiếm dụng.
    - Chờ đợi vòng lặp.
- Hệ điều hành thực tế thường bỏ qua vấn đề này để tối ưu hiệu năng.
- Ngăn ngừa bằng cách phá bỏ các điều kiện cần.
- Tránh bằng thuật toán Banker để giữ hệ thống trong Safe State.
- Phát hiện bằng vòng lặp trên đồ thị và phục hồi bằng cách huỷ luồng hoặc chiếm dụng tài nguyên.
