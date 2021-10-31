# Super-Convergence
[Super-Convergence: Very Fast Training of Neural Networks Using Large Learning Rates](https://arxiv.org/pdf/1708.07120.pdf)

## 1. Super-Convergence use Cyclical learning rates(CLR) and learning rate range test(LR range test)
Để sử dụng CLR, ta cần xác định max, min learning rate và stepsize. Stepsize là số iterations sử dụng cho mỗi step, mỗi cycle gồm 2 step, một step learning rate tăng, một step giảm. Phương pháp đơn giản nhất là tăng, giảm learning rate tuyến tính (Triangular), một phương pháp khác là để learning rate tăng giảm một cách rời rạc.

LR range test có thể sử dụng để xác định liệu super-convergence có khả thi cho 1 kiến trúc mạng hay không. Quá trình này sẽ bắt đầu huấn luyện với leanring rate nhỏ hoặc bằng 0, sau đó tăng dần trong quá trình huấn luyện, từ quá trình pre-train này ta có thể xác định được max, min learning rate sử dụng cho CLR.

Một thay đổi nhỏ trong việc sử dụng CLR cho super-convergence là "1cycle". Ta sẽ chọn max, min leaning rate từ LR range test, chọn stepsize gần bằng số iterations cần train. Train mô hình 1 cycle, sau đó trong phần còn lại của quá trình, train với learning rate giảm dần từ min lr đến khoảng 1/10 (1/8,...) min lr. Các thí nghiệm cho thấy cách làm này tăng độ chính xác.

Bài báo cũng chỉ ra super-convergence có thể áp dụng rộng rãi và hướng dẫn khi nào, ở đâu và tại sao super-convergence khả thi. Đặc biệt, có rất nhiều dạng regularization như learning rate lớn, batch size nhỏ, weight decay, dropout, người dùng cần phải cân bằng giữa các dạng regularization này để đạt hiệu quả tốt cho từng bộ dữ liệu, từng kiến trúc mạng. Việc giảm các dạng regularization khác và chuẩn hóa với learning rate cực lớn làm cho quá trình train hiệu quả rõ rệt.

## 2. Estimate optimal learning rates

## 3. Intuitive explaination for super-convergence
Bài báo đưa ra hình ảnh trực quan về quá trình super-convergence xảy ra. Khi quá trình train bắt đầu, ta cần learning rate nhỏ đề đi đúng hướng, sau đó khi càng gần đến điểm cực tiểu thì độ dốc càng giảm, gặp phải vùng flat valley làm cho quá trình train không có nhiều tiến triển, lúc này learning rate tăng lên làm đẩy nhanh tốc độ vượt qua flat valley, sau đó learning rate lại giảm để mô hình có thể đi đến được điểm cực tiểu.
## 4. Learning rate regularization
Bài báo đưa ra một số bằng chứng về việc learning rate lớn cũng có tác dụng như 1 dạng chuẩn hóa. Khi train với tập dữ liệu Cifar-10 và kiến trúc mạng Resnet-56, learning rate tăng từ 0.2 lên đến 2 thì training loss tăng nhưng test loss vẫn giảm. Điều đó chứng tỏ regularization xảy ra với những giá trị learning rate lớn. Kết quả tương tự cũng xảy ra với Resnet-20 hay Resnet-110.
## 5. Relationship of super-convergence to SGD and generalization
## 6. Hyper-parameters and adaptive learning rate methods
