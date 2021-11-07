# Super-Convergence
[Super-Convergence: Very Fast Training of Neural Networks Using Large Learning Rates](https://arxiv.org/pdf/1708.07120.pdf)

## 1. Super-Convergence use Cyclical learning rates(CLR) and learning rate range test(LR range test)
Để sử dụng CLR, ta cần xác định max, min learning rate và stepsize. Stepsize là số iterations sử dụng cho mỗi step, mỗi cycle gồm 2 step, một step learning rate tăng, một step giảm. Phương pháp đơn giản nhất là tăng, giảm learning rate tuyến tính (Triangular), một phương pháp khác là để learning rate tăng giảm một cách rời rạc.

LR range test có thể sử dụng để xác định liệu super-convergence có khả thi cho 1 kiến trúc mạng hay không. Quá trình này sẽ bắt đầu huấn luyện với leanring rate nhỏ hoặc bằng 0, sau đó tăng dần trong quá trình huấn luyện, từ quá trình pre-train này ta có thể xác định được max, min learning rate sử dụng cho CLR.

### 1Cycle
Một thay đổi nhỏ trong việc sử dụng CLR cho super-convergence là "1cycle". Ta sẽ chọn max, min leaning rate từ LR range test, chọn stepsize gần bằng số iterations cần train. Train mô hình 1 cycle, sau đó trong phần còn lại của quá trình, train với learning rate giảm dần từ min lr đến khoảng 1/10 (1/8,...) min lr. Các thí nghiệm cho thấy cách làm này tăng độ chính xác.

Bài báo cũng chỉ ra super-convergence có thể áp dụng rộng rãi và hướng dẫn khi nào, ở đâu và tại sao super-convergence khả thi. Đặc biệt, có rất nhiều dạng regularization như learning rate lớn, batch size nhỏ, weight decay, dropout, người dùng cần phải cân bằng giữa các dạng regularization này để đạt hiệu quả tốt cho từng bộ dữ liệu, từng kiến trúc mạng. Việc giảm các dạng regularization khác và chuẩn hóa với learning rate cực lớn làm cho quá trình train hiệu quả rõ rệt.

## 2. Estimate optimal learning rates

## 3. Intuitive explaination for super-convergence
Bài báo đưa ra hình ảnh trực quan về quá trình super-convergence xảy ra. Khi quá trình train bắt đầu, ta cần learning rate nhỏ đề đi đúng hướng, sau đó khi càng gần đến điểm cực tiểu thì độ dốc càng giảm, gặp phải vùng flat valley làm cho quá trình train không có nhiều tiến triển, lúc này learning rate tăng lên làm đẩy nhanh tốc độ vượt qua flat valley, sau đó learning rate lại giảm để mô hình có thể đi đến được điểm cực tiểu.

<img src="images/Visualization of super-convergence.png"/>


## 4. Learning rate regularization
Bài báo đưa ra một số bằng chứng về việc learning rate lớn cũng có tác dụng như 1 dạng chuẩn hóa. Khi train với tập dữ liệu Cifar-10 và kiến trúc mạng Resnet-56, learning rate tăng từ 0.2 lên đến 2 thì training loss tăng nhưng test loss vẫn giảm. Điều đó chứng tỏ regularization xảy ra với những giá trị learning rate lớn. Kết quả tương tự cũng xảy ra với Resnet-20 hay Resnet-110.
## 5. Relationship of super-convergence to SGD and generalization
Trong bài báo này, người viết tập trung vào việc sử dụng CLR với learning rate rất lớn, learning rate sẽ thêm noise/regularization trong quá trình train. [Jastrz˛ebski et al. [2017]](https://arxiv.org/abs/1711.04623) đề cập rằng mức độ noise cao hơn sẽ dẫn SGD đến kết quả tổng quát hơn. Cụ thể, họ chỉ ra rằng tỉ lệ giữa learning rate và batch size, cùng với variance của gradient sẽ kiểm soát độ rộng của cực tiểu địa phương tìm thấy bởi SGD. Độc lập với đó, [ Chaudhari and Soatto [2017]](https://arxiv.org/abs/1710.11029) chỉ ra rằng SGD thực hiện ragularization bằng cách làm cho SGD mất cân bằng, điều này rất quan trọng để có được hiệu suất tổng quát hóa tốt. Họ cũng chỉ ra rằng tăng dữ liệu làm tăng tinhs đa dạng của gradient SGD, dẫn đến tổng quát hóa tốt hơn.

Ngoài ra một số bài báo khác cũng chỉ ra những cực tiểu rộng, phẳng thì có tính tổng quát hóa cao hơn là những cực tiểu hẹp, nhỏ. Điều này cũng phù hợp với cách mà super-convergence hoạt động.

[Goyal et al. [2017]](https://arxiv.org/abs/1706.02677) sử dụng batch-size rất lớn lên đến 8192 and thay đổi learning rate linearly với batch-size. Họ cũng đề xuất 1 giai đoạn warm up cho learning rate, phương pháp này là 1 phiên bản của CLR và match với learning rate tăng dần. Họ cũng đưa ra 1 quan điểm liên quan đến việc điều chỉnh batch-size: nếu sử dụng batch normalization thì với batch-size khác nhau sẽ dẫn đến thống kê khác nhau, điều này cần được xử lý. 

[Hoffer et al. [2017]](https://arxiv.org/abs/1705.08741) cũng đưa ra quan điểm tương tự về batch normalization và đề xuất cách giải quyết sử dụng ghost statistics. Đồng thời họ cũng chỉ ra rằng quá trình train dài cũng là 1 dạng regularization và làm tăng tính tổng quát.

Mặt khác, bài báo này đã thực hiện một cách tiếp cận toàn diện hơn đối với tất cả các hình thức regularization và kết quả đã chứng minh rằng việc regularization với learning rate rất lớn cho phép thời gian đào tạo ngắn hơn nhiều.
## 6. Hyper-parameters and adaptive learning rate methods
Với batch normalization moving_average_fraction nên giảm xuống để super-convergence có thể hoạt động. Moving_average_fraction lớn (0.99) chỉ phù hợp với quá trình trên dài, mà super-convergence train rất nhanh nên giá trị 0.99 không hiệu quả.

<img src="images/Batch-normalization with super-convergence.png"/>

Bài báo cũng tìm thử tìm hiểu xem các phương pháp adaptive learning có thích nghi để sử dụng learning rate lớn để tăng tốc quá trinh train như super-cenvergence không. Kết quả thí nghiệm cho thấy các phương pháp Nesterov momentum, AdaDelta, AdaGrad, Adam đều không đẩy nhanh quá trình train như super-convergence.

Thí nghiệm 1cycle với các phương pháp adaptive learning như Nesterov, AdaDelta, AdaGrad cũng cho phép super-cenvergence xảy ra, nhưng với Adam thì không thể. Hình 9b so sánh super-convergence với piecewise constant training regime trên phương pháp Nesterov.

<img src="images/Comparison of super-convegence to piecewise constant training regime.png"/>
