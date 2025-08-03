# SosanhTTOtsuvaK-means
# So sách thuật toán Otsu va thuật toán K-means
## Nhóm thực hiện: 09_NMXLA_HK242
## Giảng viên: Đỗ Hữu Quân

Giới thiệu
Đồ án này nhằm mục đích áp dụng thuật toán Otsu và thuật toán K-Means để thực hiện phân đoạn ảnh — một trong những bước cơ bản và quan trọng trong xử lý ảnh số.
Otsu Thresholding: Dùng để tự động tìm ngưỡng tối ưu, phân tách đối tượng và nền trong ảnh xám, giúp làm nổi bật các chi tiết chính.
K-Means Clustering: Dùng để phân nhóm các điểm ảnh của ảnh màu thành k cụm dựa trên sự tương đồng màu sắc, giúp đơn giản hóa ảnh hoặc tách các vùng màu đặc trưng.
Mục tiêu:
Làm rõ sự khác biệt giữa phương pháp phân ngưỡng đơn giản (Otsu) và phân đoạn dựa trên phân cụm (K-Means).
Giúp sinh viên nắm vững các thao tác tiền xử lý, tách nền và nhận diện vùng quan trọng trong ảnh.

Công nghệ sử dụng
Python: Ngôn ngữ lập trình chính.
Pillow (PIL): Đọc ảnh và chuyển đổi sang ảnh xám.
NumPy: Xử lý mảng số liệu ảnh.
Scikit-Image: Tính ngưỡng Otsu tự động.
Matplotlib: Hiển thị ảnh trực quan.
OpenCV: Xử lý ảnh nâng cao và thao tác với webcam.
scikit-learn: Thư viện thực hiện K-Means Clustering.

Chi tiết các phép biến đổi & công thức

Thuật toán Otsu (Otsu Thresholding)
Mục đích:
Tự động tìm ngưỡng t sao cho phương sai giữa các lớp foreground và background được tối đa hóa, giúp tách đối tượng ra khỏi nền ảnh xám.
Công thức:
Otsu tìm t sao cho:
<img width="417" height="66" alt="image" src="https://github.com/user-attachments/assets/287cec31-cc31-4080-8f9f-1bcc2fed5c6b" />

Thuật toán K-Means (K-Means Clustering)

Mục đích:
Phân đoạn ảnh màu thành k cụm dựa trên sự tương đồng về màu sắc (RGB), giúp đơn giản hóa ảnh hoặc làm nổi bật các vùng màu đặc trưng.
Công thức:
Tối ưu hàm mất mát:

<img width="257" height="88" alt="image" src="https://github.com/user-attachments/assets/e56ddc15-f14c-4c85-a805-9629a34db120" />

Dưới đây là ví dụ kết quả so sánh hai thuật toán trên ảnh tĩnh:
 Đánh giá phân đoạn:
 Otsu:
  Thời gian: 0.0041 giây
  SSIM: 0.7125
  PSNR: 14.32 dB

 K-Means (k=2):
  Thời gian: 0.0532 giây
  SSIM: 0.8247
  PSNR: 16.85 dB

So sánh
1. Nguyên lý hoạt động
Otsu Thresholding:
Hoạt động trên ảnh xám (grayscale).
Dựa vào histogram của ảnh, thuật toán tự động tìm ra một ngưỡng tối ưu (t) để chia ảnh thành 2 lớp: foreground (đối tượng) và background (nền).
Phù hợp với các ảnh có histogram 2 đỉnh rõ rệt (đối tượng và nền phân biệt rõ màu sắc).
K-Means Clustering:
Áp dụng trên ảnh màu (RGB).
Thuật toán phân nhóm các pixel vào k cụm dựa trên sự tương đồng về màu sắc (hoặc các đặc trưng khác như texture).
Sau mỗi lần gán pixel vào cụm, tâm cụm được cập nhật cho đến khi hội tụ.
Cho phép phân đoạn nhiều vùng (k cụm màu), không giới hạn 2 lớp như Otsu.
2. Đầu vào và đầu ra
Otsu:
Đầu vào: Ảnh xám (grayscale).
Đầu ra: Ảnh nhị phân (đen trắng).
K-Means:
Đầu vào: Ảnh màu (RGB).
Đầu ra: Ảnh phân đoạn với k màu đại diện.
3. Ưu điểm
Otsu:
Nhanh, đơn giản, không cần tham số nhiều.
Hiệu quả với ảnh có histogram 2 đỉnh rõ ràng.
K-Means:
Phân đoạn ảnh màu đa vùng.
Kết quả phân cụm màu chi tiết hơn.
4. Nhược điểm
Otsu:
Chỉ tách được 2 lớp.
Không phù hợp với ảnh màu hoặc ảnh phức tạp.
K-Means:
Chậm hơn (phải lặp nhiều lần).
Cần xác định trước số cụm k.






