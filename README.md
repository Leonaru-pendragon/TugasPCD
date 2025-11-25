# TugasPCD
### Tugas Pengolahan Citra Digital -- Ikmal Nur Awaludin

## 1. Deskripsi Proyek

Proyek ini bertujuan untuk melakukan pewarnaan otomatis pada citra
grayscale menggunakan model DDColor dari ModelScope. Notebook ini
memungkinkan pengguna untuk mengunggah gambar grayscale, mengonversinya
menjadi RGB dasar, melakukan pewarnaan otomatis menggunakan AI, serta
menampilkan dan menyimpan hasilnya.

## 2. Teknologi yang Digunakan

-   Python 3
-   Google Colab
-   ModelScope
-   OpenCV
-   Pillow
-   Matplotlib

## 3. Instalasi Library

    !pip install modelscope opencv-python pillow matplotlib
    !pip install "modelscope==1.9.5"

## 4. Cara Menggunakan

### 4.1 Import Library

    import matplotlib.pyplot as plt
    from PIL import Image
    from PIL import ImageFilter
    import cv2
    from modelscope.pipelines import pipeline
    from modelscope.hub.snapshot_download import snapshot_download

### 4.2 Download Model DDColor

    model_dir = snapshot_download(
        "damo/cv_ddcolor_image-colorization",
        cache_dir="models"
    )

### 4.3 Inisialisasi Pipeline

    ddcolor = pipeline(
        task="image-colorization",
        model=model_dir,
        device="cuda"
    )

### 4.4 Upload Gambar

    from google.colab import files
    uploaded = files.upload()
    filename = list(uploaded.keys())[0]

### 4.5 Konversi Grayscale ke RGB

    gray_img = cv2.imread(filename, cv2.IMREAD_GRAYSCALE)
    rgb_img = cv2.cvtColor(gray_img, cv2.COLOR_GRAY2RGB)
    cv2.imwrite("rgb_output.png", rgb_img)

### 4.6 Pewarnaan Menggunakan DDColor

    result = ddcolor(filename)
    img = result["output_img"]
    img_rgb = cv2.cvtColor(img, cv2.COLOR_BGR2RGB)
    img_pil = Image.fromarray(img_rgb)
    img_pil.save("colorized_output.png")

### 4.7 Menampilkan Hasil

    plt.figure(figsize=(12,4))

    plt.subplot(1,3,1)
    plt.imshow(gray_img, cmap="gray")
    plt.title("Grayscale")
    plt.axis("off")

    plt.subplot(1,3,2)
    plt.imshow(rgb_img)
    plt.title("RGB Conversion")
    plt.axis("off")

    plt.subplot(1,3,3)
    plt.imshow(img_rgb)
    plt.title("Colorized (DDColor)")
    plt.axis("off")

    plt.show()

## 5. Output

-   rgb_output.png
-   colorized_output.png

## 6. Catatan

-   Gunakan file gambar yang berformat .jpg, .jpeg, atau .png.
-   Kualitas pewarnaan tergantung pada kualitas dan kontras gambar grayscale.
-   GPU sangat direkomendasikan untuk mempercepat proses pewarnaan.

## 7. Lisensi

Proyek ini dibuat untuk tugas Pengolahan Citra Digital yang menggunakan
model DDColor dari ModelScope.
