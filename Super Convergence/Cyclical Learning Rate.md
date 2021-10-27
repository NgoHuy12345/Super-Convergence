# CLR Method (Cyclical learning rate)
[Cyclical Learning Rates for Training Neural Networks (Leslie N. Smith)](https://arxiv.org/pdf/1506.01186.pdf)
Bài báo chứng minh một hiện tượng đáng ngạc nhiên là việc thay đổi learning rate trong quá trình traning thì có lợi về tổng thể. CLR cũng giảm sự cần thiết của việc tuning learning rate nhưng vẫn đạt đến gần giá trị tối ưu. Hơn thế nữa không giống như adaptive learning rate (AdaGrad, Adam, RMSProp...), CLR không yêu cầu tính toán thêm.

## 1. Cyclical learning rate
Bản chất của phương pháp này đến từ việc quan sát được rằng việc tăng learning rate gây hại tạm thời cho mô hình nhưng về lâu dài thì lại có lợi. Quan sát này dẫn đến ý tưởng về việc tăng và giảm learning rate trong 1 khoảng giá trị thay vì áp dụng 1 giá trị cố định hoặc giảm theo cấp số nhân.
CLR hiệu quả tốt bởi nó có thể giúp vượt qua các saddle point. Tại saddle point gradient rất nhỏ và quá trình học rất tốn thời gian, việc tăng learning rate sẽ giúp vượt qua các vị trí này nhanh hơn.
Phương pháp tăng giảm learning rate đơn giản, có hiệu quả tương đương các phương pháp khác: **Triangular** . 

**Triangular2**:  Tương tự **Triangular** nhưng khoảng cách giữa 2 boundary sẽ giảm đi một nửa sau mỗi chu kỳ.
 
Learning rate sẽ tăng linear từ **baseLr** đến **maxLr** trong khoảng thời gian  *stepsize* (tính bằng Iterations) và cũng giảm Linear trong khoảng thời gian tương tự.
1 lượt tăng và 1 lượt giảm như vậy gọi là 1 chu kỳ (cyclical).
Accuracy được chỉ ra là đạt đỉnh tại cuối mỗi cyclical
## 2. How can one estimate a good value for the cycle length?
Các thí nghiệm chỉ ra rằng *stepsize* khoảng 2-10 lần số Iterations của mỗi epoch là hợp lý.
## 3. How can one estimate reasionable minimum and maximum boundary values?
Một cách đơn giản để xác định min và max boundary values là sử dụng "LR range test", chạy mô hình một vài epoch trong khi tăng learning rate linear. Note lại learning rate mà mô hình bắt đầu tăng độ chính xác và learning rate mà độ chính xác của mô hình giảm hoặc không ổn định nữa (lúc lên lúc xuống). Learning rate thứ nhất sẽ chọn làm **baseLr**, leaning rate thứ 2 chọn làm **maxLr**.  