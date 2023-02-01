## Model 3A so với origin

Với mục tiêu là tối ưu hóa giúp cho model được nhẹ hơn đồng thời phải tăng được độ chính xác của model ban đầu, vì vậy qua quá trình nghiên cứu phát triển, nhóm đã thực hiện các bước modified model ban đầu như sau để theo dỗi:

- So với model ban đầu, model mới của ta sẽ sử dụng số filter groups tương ứng với số filters tại các convolutional layer đứng ngay trước mỗi dropout layer.Từ convolution layer thứ 44 trở đi, các convolution layer với 136 filters sẽ được tăng lên 144, các convolution layer với 224 filters sẽ được tăng lên 288 (tăng thêm 8, 64), cùng với đó là số lượng filter groups của các layer này cũng bằng chính số lượng filters. Qua đó nhóm có hi vọng sẽ giúp model đạt độ chính xác cao hơn. Việc làm như vậy giúp cho model của ta có khả năng học hiệu quả hơn và đồng thời giúp giảm parameters cho model do việc tăng lên số lượng filters.

- Bỏ bớt 5 convolution layers ngay sau SPP architecture ( trước lớp yolo thứ nhất) để làm giảm kích thước model, Thay đổi số filters từ 255 thành 14, 120 thành 48 filters tại các lớp convolutional sau lớp yolo thứ nhất và vẫn thực hiện nguyên tắc số lượng groups filters bằng số filters. Thay đổi số lượng mask của lớp yolo từ 3 thành 2, số lượng classes được chuyển từ 80 thành 2 so với model ban đầu.

## Model 3B so với 3A

Sau khi thực hiện modified được model 3A, qua đánh giá được kết quả model đã nhẹ đi so với model gốc, tuy nhiên loss lại tăng lên khá nhiều, chính vì vậy nhóm tiếp tục modified model 3A thành model 3B bằng cách:

- Giảm số lượng filters đi 1 nửa tại các convolutional layer tại các vị trí 1, 2, 4, 5 giữa các lớp shortcut và dropout. Số lượng group filter của các layer trên cũng được thay đổi bằng với số filter. Restore lại các lớp convolutional layer sau SPP architecture của model ban đầu đã bị bỏ khi modified model 3A. Thay đổi mask của lớp yolo thứ nhất từ 4, 5 thành 2, 3.

Mục đích của việc modifi này là để tiếp tục giảm model size và cố lấy lại hiệu quả của model như model ban đầu. Kết quả là model đã giảm rất nhiều đồng thời loss vs mAP đã xấp xỉ model ban đầu

## Model cuối cùng so với 3B

Sau khi đã đạt được mục đích với model size, nhóm tiếp tục thực hiện mod model để có thể đạt được hiệu quả và độ chính xác cao hơn cho model, vì vậy nhóm đã thêm một cấu trúc khối gồm yolo, route, upsample layer giống cấu trúc của khối trong model vào vị trí giữa khối yolo thứ nhất và thứ hai với số lượng filters và group filter của các convolutional layer giảm đi một nửa so với các layer thuộc khối yolo thứ nhất (cụ thể là giảm từ 48 còn 24).

Việc thêm một lớp yolo với mục đích tăng tính chính xác của model và kết quả là hiệu quả và tính chính xác của model tăng lên khá nhiều và model size xấp xỉ với model 3B ( có tăng một chút)

