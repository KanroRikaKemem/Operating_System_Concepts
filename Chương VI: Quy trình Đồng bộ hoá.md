## I. Bối cảnh:
- Tiến trình có thể thực thi đồng thời (concurrently) hoặc song song (parallel).
- Việc lập lịch CPU có thể ngắt quãng tiến trình bất kỳ lúc nào.
- Truy cập không kiểm soát vào dữ liệu dùng chung dẫn đến mất nhất quán.
> Ví dụ: Bài toán sản xuất - tiêu thụ:
> - Sử dụng một bộ đệm giới hạn (bounded buffer).
> - Biến `count` dùng để theo dõi số mục trong bộ đệm.
> - Producer tăng `count` khi thêm mục, Consumer giảm `count` khi lấy mục.
> 
> Mã nguồn Producer:
> 
> ![image](https://hackmd.io/_uploads/H1SX0NWFWe.png)
> Mã nguồn Consumer:
> 
> ![image](https://hackmd.io/_uploads/HkQBCEbtZg.png)
- Hiện tượng tranh chấp dữ liệu:
    - `count++` và `count--` không phải thao tác nguyên tử (atomic).
    - Ở mức ngôn ngữ máy, `count++` gồm ba lệnh là nạp thanh ghi, tăng, lưu lại. Tương tự với `count--`.
    > Giả sử `count` đang là `5`. Producer và Consumer cùng thực hiện đồng thời, kết quả cuối cùng có thể là `4` hoặc `6` thay vì `5`.
    - Xảy ra khi nhiều tiến trình truy cập/thao tác chung dữ liệu.
    - Kết quả phụ thuộc vào thứ tự thực thi của các lệnh.
    - Cần cơ chế đồng bộ hoá để ngăn chặn.

## II. Vấn đề miền găng (The Critical-Section Problem):
> Các yêu cầu và cấu trúc giải pháp cho miền găng.
### 1. Khái niệm miền găng:
- Là đoạn mã mà tiến trình truy cập vào dữ liệu dùng chung.
- Khi một tiến trình ở trong miền găng, không tiến trình nào khác được phép vào.

### 2. Cấu trúc tiến trình:
![image](https://hackmd.io/_uploads/rklTkBbtbg.png)
- Đoạn mã nhập - Entry Section.
- Miền găng - Critical Section.
- Đoạn mã thoát - Exit Section.
- Đoạn mã còn lại- Remainder Section.

### 3. Yêu cầu:
- Loại trừ tương hỗ (Mutual Exclusion): Nếu tiến trình $P_i$ đang thực thi miền găng, không tiến trình nào khác có thể thực thi trong đó.
- Progress: Nếu không có ai trong miền găng, việc quyết định ai vào tiếp theo không thể bị trì hoãn vô thời hạn.
- Chờ đợi hữu hạn (Bounded Waiting): Có giới hạn về số lần các tiến trình khác được vào miền găng sau khi một tiến trình đã yêu cầu.

### 4. Giải pháp Peterson:
![image](https://hackmd.io/_uploads/Syx8-BZFZe.png)
- Dựa trên phần mềm cho hai tiến trình.
- Sử dụng hai biến dùng chung `int turn` và `boolean flag[2]`.
- `flag[i] = true` nghĩa là tiến trình `i` sẵn sàng vào miền găng.
    - `flag[i] = true` báo muốn vào CS.
    - `turn = j` nhường quyền ưu tiên cho tiến trình còn lại.
    - Điều kiện `while` đảm bảo chỉ một tiến trình vào CS.
- Hạn chế: Có thể không hoạt động trên kiến trúc hiện đại do sắp xếp lại lệnh (reordering).

![image](https://hackmd.io/_uploads/rkA3-rZtbx.png)

## III. Hỗ trợ phần cứng cho đồng bộ hoá (Hardware Support for Synchronization):
### 1. Rào cản bộ nhớ (Memory Barriers):
- Là các lệnh đảm bảo các thay đổi bộ nhớ được hiển thị cho các tiến trình khác.
- Ngăn chặn việc sắp xếp lại lệnh gây lỗi logic đồng bộ.

### 2. Các lệnh nguyên tử (Hardware Instructions):
Là các lệnh thực hiện như một đơn vị không thể chia cắt.
> `test_and_test()`:
> 
> ![image](https://hackmd.io/_uploads/ByvtfS-FWg.png)
> ![image](https://hackmd.io/_uploads/SJtqfH-FZe.png)
> 
> `compare_and_swap()`:
> 
> ![image](https://hackmd.io/_uploads/HkyhGrZYbe.png)
> ![image](https://hackmd.io/_uploads/SyU6MHWtbg.png)
> ![image](https://hackmd.io/_uploads/B1YAGHbKbx.png)
> - So sánh giá trị biến với một giá trị dự kiến, nếu bằng thì cập nhật biến sang giá trị mới.
> - Thực hiện nguyên tử hoàn toàn bằng phần cứng.

### 3. Biến nguyên tử (Atomic Variables):
- Cung cấp thao tác nguyên tử trên kiểu dữ liệu cơ bản.
- Thường dùng CAS để cập nhật mà không cần dùng lock.

![image](https://hackmd.io/_uploads/HyK_mHZFbl.png)

### 4. Ưu điểm và hạn của giải pháp hỗ trợ phần cứng:
- Ưu điểm:
    - Hiệu suất cao cho các kịch bản tranh chấp thấp.
    - Làm nền tảng để xây dựng các công cụ phần mềm cấp cao.
- Hạn chế:
    - Lập trình phức tạp cho user thông thường.
    - Khó giải quyết các vấn đề đồng bộ phức tạp.

## IV. Mutex Locks:
> Công cụ đơn giản nhất để bảo vệ miền găng.

### 1. Khái niệm:
![image](https://hackmd.io/_uploads/r1_rVSbFZl.png)
- `Mutex` là viết tắt của `Mutual Exclusion`.
- Thao tác:
    - `acquire()`: Chờ đến khi khoá khả dụng rồi lấy khoá.
    - `release()`: Giải phóng khoá cho tiến trình khác.
    - Cả hai đều phải thực hiện nguyên tử.
> Ví dụ:
> 
> ![image](https://hackmd.io/_uploads/r1RcEHZKWe.png)
> 
> Ví dụ mã giả Mutex:
> ```
> acquire() {while (!available); available = false;}
> release() {available = true;}
> ```

### 2. Vấn dề chờ đợi bận:
- Tiến trình liên tục lặp lại kiểm tra khoá khi chờ.
- Gây lãng phí chu kỳ CPU đáng kể.

### 3. Spinlocks:
- Loại Mutex lock sử dụng cơ chế chờ bận.
- Lợi thế: Không mất thời gian chuyển ngữ cảnh.
- Hữu ích nếu thời gian giữ khoá ngắn.
- Ứng dụng:
    - Thường dùng trong các hệ đa nhân (multicore).
    - Phổ biến trong mã nguồn hệ điều hành hiện đại.

## V. Semaphores:
> Công cụ đồng bộ hoá mạnh mẽ và linh hoạt hơn Mutex.

### 1. Định nghĩa:
- Là một biến số nguyên `S`.
- Truy cập qua thao tác nguyên tử `wait()` và `signal()`.
- Do nhà khoa học Edsger Dijkstra đề xuất.

### 2. Thao tác `wait()`:
- Nguyên gốc là P (Proberen - Kiểm tra).
- Nếu không còn tài nguyên nghĩa là thread bị chặn.
``` code
wait(S) {
    while (S <= 0); //busy wait
    S--;
}
```
> ![image](https://hackmd.io/_uploads/HJk_CDbF-e.png)

### 3. Thao tác `signal()`:
- Nguyên gốc là V (verhogen - tăng).
- Trả tài nguyên là đánh thức thread đang chờ.
``` code
signal(S) {
    S++;
}
```
> `V()` $\rightarrow$ `value = 0` $\rightarrow$ Gọi một người vào.

### 4. Counting Semaphore:
- Giá trị nằm trong khoảng không hạn chế.
- Dùng quản lý tài nguyên có nhiều thực thể (instances).

### 5. Binary Semaphore:
- Giá trị chỉ có thể là $0$ hoặc $1$.
- Hoạt động tương tự như Mutex Lock.

### 6. Quản lý tài nguyên:
- Khởi tạo semaphore bằng số lượng tài nguyên sẵn có.
- Tiến trình thực hiện `wait()` để yêu cầu sử dụng tài nguyên.

![image](https://hackmd.io/_uploads/S1g91u-tZe.png)
- Thực hiện `signal()` khi trả lại tài nguyên. Nếu giá trị về $0$, các yêu cầu sau sẽ bị chặn cho đến khi giải phóng:

![image](https://hackmd.io/_uploads/BJf0ku-F-x.png)

### 7. Giải quyết ràng buộc thứ tự:
- Đảm bảo lệnh S1 chạy trước lệnh S2.
- Dùng semaphore khởi tạo bằng `0`.

### 8. Triển khai không chờ đợi bận:
- Thay vì lặp, tiến trình tự block và vào hàng đợi.
- Được đánh thức bởi lệnh `signal()`.

## VI. Monitors:
### 1. Tại sao cần Monitors?
- Dùng Semaphore sai cách dễ gây lỗi logic khó phát hiện.
- Monitor cung cấp cấu trúc đóng gói dữ liệu và hàm đồng bộ.
- Mutex: Dễ quên unlock.
- Semaphore: Dễ `signal()` nhầm.
- Code: Bug lặt vặt, khó debub.

### 2. Đặc điểm của Monitor:
- Chỉ duy nhất một tiến trình hoạt động bên trong monitor tại một thời điểm.
- Tự động đảm bảo loại trừ tương hỗ.

### 3. Biến điều kiện:
- Dùng đồng bộ tiến trình dựa trên điều kiện logic.
- Hai thao tác chính:

![image](https://hackmd.io/_uploads/rJ_c-u-tbe.png)

#### Thao tác `wait()`:
- `x.wait()`: Tiến trình gọi lệnh sẽ bị dừng đến khi có `signal()`.
> `wait()` = nhả mutex + ngủ(atomic)
- `x.wait()` làm thread hiện tại nhả mutex của monitor, đi vào trạng thái chờ và chỉ được đánh thức khi có thread khác gọi `x.signal()`.

#### Thao tác `signal()`:
- `x.signal()`: Đánh thức một thread khác đang chờ trên `x`.
- Các kiểu ngữ nghĩa của `signal`:
    - Signal and Wait: Tiến trình phát tín hiệu chờ tiến trình được đánh thức.
    - Signal and Continue: Tiến trình phát tín hiệu tiếp tục thực thi.

### 4. Ví dụ Monitor:
![image](https://hackmd.io/_uploads/HJ0W7_Ztbl.png)

## VII. Tổng kết:
### 1. So sánh các công cụ:
- Mutex: Đơn giản, chuyên biệt cho loại trừ tương hỗ.
- Semaphore: Linh hoạt, quản lý đa tài nguyên hiệu quả.
- Monitor: Cấp cao, an toàn, quản lý hướng đối tượng tốt.
- Hardware: Hiệu năng cao nhất, làm nền tảng hệ thống.

### 2. Tóm tắt nội dung:
- Đồng bộ giúp chặn tranh chấp dữ liệu (race condition).
- Ba yêu cầu miền găng:
    - Loại trừ tương hỗ.
    - Progress.
    - Chờ hữu hạn.
- Khoá (Locking): Cơ chế đồng bộ phổ biến nhất.
- Ngôn ngữ hiện đại đều tích hợp Monitors và Semaphore.
