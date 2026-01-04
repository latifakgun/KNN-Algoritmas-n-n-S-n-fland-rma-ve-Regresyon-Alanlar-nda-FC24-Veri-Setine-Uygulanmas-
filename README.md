# ⚽ FC 24 Player Analysis using K-Nearest Neighbors (KNN)

![Python](https://img.shields.io/badge/Python-3.8%2B-blue)
![Scikit-Learn](https://img.shields.io/badge/Library-Scikit--Learn-orange)
![Jupyter](https://img.shields.io/badge/Environment-Jupyter-Fzt)
![License](https://img.shields.io/badge/License-MIT-green)

Bu proje, **Yıldız Teknik Üniversitesi - İstatistik Bölümü "Veri Sınıflandırma Yöntemleri"** dersi kapsamında geliştirilmiştir. EA Sports FC 24 veri seti kullanılarak oyuncuların fiziksel ve teknik özelliklerine göre mevkilerinin sınıflandırılması (Classification) ve güç/piyasa değerlerinin tahminlenmesi (Regression) amaçlanmıştır.

---

## 📋 Proje Özeti

Bu çalışmada **K-En Yakın Komşu (KNN)** algoritması kullanılarak üç farklı model geliştirilmiştir:
1.  **Mevki Sınıflandırma (Classification):** Oyuncunun istatistiklerine bakarak Forvet, Orta Saha veya Defans olduğunu tahmin etme.
2.  **Güç Tahmini (Regression):** Oyuncunun genel gücünü (Overall Rating) tahmin etme.
3.  **Piyasa Değeri Tahmini (Regression):** Oyuncunun bonservis bedelini (Value) tahmin etme.

Proje süreci; veri temizliği, özellik mühendisliği (feature engineering), modelleme ve hiperparametre optimizasyonu (Grid Search) adımlarını kapsamaktadır.

---

## 📊 Veri Seti ve Ön İşleme (Data Cleaning)

Kullanılan veri seti: [EA Sports FC 24 Complete Player Dataset (Kaggle)](https://www.kaggle.com/datasets/stefanoleone992/ea-sports-fc-24-complete-player-dataset)

Modelin başarısını artırmak ve "Data Leakage" (Veri Sızıntısı) sorununu önlemek için kritik veri temizleme adımları uygulanmıştır:

* **Geçmiş Verilerin Temizlenmesi:** Veri setinde oyuncuların FIFA 15-23 arası versiyonları bulunmaktadı. Modelin ezberlemesini (Overfitting) önlemek için sadece **FC 24** verileri kullanıldı.
* **Duplicate Kayıtlar:** Aynı oyuncunun birden fazla kartı (TOTW, Gold vb.) veri setinden çıkarıldı; sadece en yüksek reytingli kart tutuldu. (180k satırdan -> 18k benzersiz oyuncuya inildi).
* **Kalecilerin Çıkarılması (Feature Space Mismatch):** Kalecilerin şut, pas, dribbling gibi özellikleri eksik (NaN) olduğu için ve saha içi oyuncularla aynı uzayda kıyaslanamayacakları için veri setinden çıkarıldı.
* **Normalizasyon:** KNN mesafe temelli bir algoritma olduğu için tüm veriler `StandardScaler` ile ölçeklendirildi.

---

## 🛠️ Kullanılan Özellikler (Features)

Modeli eğitirken "Boyutluluk Laneti"nden (Curse of Dimensionality) kaçınmak için sadece sonucu en çok etkileyen 12 temel özellik seçilmiştir:

* **Fiziksel/Teknik:** `Pace`, `Shooting`, `Passing`, `Dribbling`, `Defending`, `Physic`
* **Mental/Diğer:** `Age`, `Reactions`, `Ball Control`

---

## 🚀 Model Sonuçları ve Optimizasyon

Modeller başlangıçta standart parametrelerle (K=9, Öklid) kurulmuş, ardından **GridSearchCV** kullanılarak K değeri, Mesafe Metriği (Euclidean/Manhattan) ve Ağırlık Sistemi (Uniform/Distance) optimize edilmiştir.

| Model | Hedef Değişken | Başlangıç Skoru | Optimize Skor | En İyi Parametreler |
| :--- | :--- | :--- | :--- | :--- |
| **Sınıflandırma** | Mevki (Position) | %83.33 (Acc) | **%83.17** | K=28, Manhattan, Distance-Weighted |
| **Regresyon 1** | Güç (Overall) | %93.96 (R2) | **%94.55** | K=15, Manhattan, Distance-Weighted |
| **Regresyon 2** | Değer (Value) | %83.06 (R2) | **%83.56** | K=11, Manhattan, Distance-Weighted |

> **Bulgu:** Futbol verileri üzerinde **Manhattan (Taksi)** mesafesinin, Öklid mesafesinden daha başarılı sonuç verdiği gözlemlenmiştir.

---

## 📈 Görselleştirmeler

*(Buraya notebook çıktılarından elde ettiğin görselleri ekleyebilirsin. Örn: Confusion Matrix, Elbow Method Grafiği vb.)*

* **Elbow Metodu:** En uygun K değerinin belirlenmesi.
* **Confusion Matrix:** Mevki tahminlerindeki hata dağılımı.
* **Regresyon Grafiği:** Gerçek vs Tahmin edilen Overall puanları.

---

## 💻 Kurulum ve Çalıştırma

Projenin çalışması için aşağıdaki kütüphanelerin yüklü olması gerekir:

```bash
pip install pandas numpy matplotlib seaborn scikit-learn
