# SosanhTTOtsuvaK-means
# So sách thuật toán Otsu va thuật toán K-means
## Nhóm thực hiện: 09_NMXLA_HK242
## Giảng viên: Đỗ Hữu Quân
Giới thiệu
Đồ án này mở rộng việc áp dụng thuật toán Otsu và K-Means để thực hiện phân đoạn ảnh, nhưng nâng cao hơn so với phiên bản cơ bản trước đó:

Với Otsu, thử nghiệm thêm các trường hợp ảnh bị làm mờ (blur), ảnh có nhiễu Gaussian, và phân ngưỡng đa mức (Multi-level Otsu).

Với K-Means, áp dụng phân cụm không chỉ trên không gian RGB, mà còn thử nghiệm trên không gian HSV và kết hợp thêm tọa độ không gian (x, y) của pixel để tăng độ chính xác phân vùng.
Mục tiêu chính:

Đánh giá hiệu quả các kỹ thuật tiền xử lý (blur, noise) ảnh hưởng đến thuật toán Otsu.

So sánh các biến thể của K-Means (RGB, HSV, RGB + tọa độ) để thấy sự khác biệt.

Làm rõ giới hạn và ưu điểm của từng phương pháp trong phân đoạn ảnh màu phức tạp.

Công nghệ sử dụng
Python: Ngôn ngữ lập trình chính.

OpenCV (cv2): Đọc ảnh, xử lý màu sắc, làm mờ ảnh, thêm nhiễu Gaussian.

NumPy: Xử lý dữ liệu ảnh dạng mảng.

Matplotlib (pyplot): Hiển thị ảnh và kết quả trực quan.

Scikit-Image (skimage.filters.threshold_multiotsu): Phân ngưỡng đa mức (Multi-level Otsu).

Scikit-Learn (KMeans): Thực hiện thuật toán phân cụm K-Means.

Chi tiết các phép biến đổi & công thức
1. Thuật toán Otsu (Nâng cao)
Mục đích:
Tự động tìm ngưỡng phân chia ảnh thành foreground và background.

Xử lý thêm các trường hợp: ảnh mờ, ảnh nhiễu, và phân nhiều lớp (multi-level).

Công thức toán học:
Giống như Otsu cơ bản, tìm ngưỡng t sao cho phương sai giữa các lớp là lớn nhất:

σ<sub>b</sub><sup>2</sup>(t) = ω<sub>1</sub>(t) * ω<sub>2</sub>(t) * (μ<sub>1</sub>(t) - μ<sub>2</sub>(t))<sup>2</sup>

Với Multi-level Otsu, ảnh được chia thành n lớp dựa trên n-1 ngưỡng.
# Otsu cơ bản
_, otsu_result = cv2.threshold(gray, 0, 255, cv2.THRESH_BINARY + cv2.THRESH_OTSU)

# Otsu sau khi làm mờ Gaussian
blurred = cv2.GaussianBlur(gray, (5, 5), 0)
_, otsu_blur = cv2.threshold(blurred, 0, 255, cv2.THRESH_BINARY + cv2.THRESH_OTSU)

# Multi-level Otsu (3 lớp)
thresholds = threshold_multiotsu(gray, classes=3)
multi_region = np.digitize(gray, bins=thresholds)

# Otsu trên ảnh bị nhiễu Gaussian
def add_gaussian_noise(img, mean=0, std=20):
    noise = np.random.normal(mean, std, img.shape).astype(np.uint8)
    return cv2.add(img, noise)

gray_noisy = add_gaussian_noise(gray)
_, otsu_noisy = cv2.threshold(gray_noisy, 0, 255, cv2.THRESH_BINARY + cv2.THRESH_OTSU)
2. Thuật toán K-Means (Nâng cao)
Mục đích:
Phân cụm pixel dựa trên màu sắc RGB, HSV hoặc kết hợp tọa độ không gian (x, y).

Xử lý tốt hơn với ảnh phức tạp, có nhiễu.

Công thức toán học:
Tối ưu hàm mất mát:
J = Σ ||x<sub>i</sub> - μ<sub>k</sub>||²
x<sub>i</sub>: Pixel (RGB hoặc HSV hoặc [RGB, x, y])
μ<sub>k</sub>: Tâm cụm thứ k

Các phương pháp thử nghiệm:
K-Means trên RGB.

K-Means trên HSV (chuyển ảnh sang không gian màu HSV).

K-Means kết hợp [R, G, B, x, y] để tăng độ chính xác biên vùng.
# KMeans RGB
def segment_rgb_kmeans(img, k):
    pixel_values = img.reshape((-1, 3))
    kmeans = KMeans(n_clusters=k, n_init='auto')
    labels = kmeans.fit_predict(pixel_values)
    centers = np.uint8(kmeans.cluster_centers_)
    segmented = centers[labels.flatten()].reshape(img.shape)
    return segmented

# KMeans HSV
def segment_hsv_kmeans(img, k):
    hsv = cv2.cvtColor(img, cv2.COLOR_BGR2HSV)
    pixel_values = hsv.reshape((-1, 3))
    kmeans = KMeans(n_clusters=k, n_init='auto')
    labels = kmeans.fit_predict(pixel_values)
    centers = np.uint8(kmeans.cluster_centers_)
    segmented = centers[labels.flatten()].reshape(img.shape)
    return segmented

# KMeans RGB + tọa độ
def segment_with_position(img, k):
    h, w, c = img.shape
    features = np.zeros((h * w, 5), dtype=np.float32)
    for i in range(h):
        for j in range(w):
            pixel = img[i, j]
            features[i * w + j] = np.array([pixel[0], pixel[1], pixel[2], i / h, j / w])
    kmeans = KMeans(n_clusters=k, n_init='auto')
    labels = kmeans.fit_predict(features)
    centers = np.uint8(kmeans.cluster_centers_[:, :3])
    segmented = centers[labels.flatten()].reshape(img.shape)
    return segmented
