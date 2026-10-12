# Implementing-Data-Preprocessing-Steps-on-a-Dataset-for-ML
Bu çalışma Yüksek Lisans Makine Öğrenmesi dersi için yapılmıştır.

# Steam Oyun Veri Seti Üzerinde Veri Ön İşleme

Makine Öğrenmesi (YL) dersi kapsamında, Steam oyun veri seti üzerinde uçtan uca veri ön işleme adımlarının uygulandığı çalışma.

## Veri Seti

- **Kaynak:** [FronkonGames / Steam Games Dataset](https://huggingface.co/datasets/FronkonGames/steam-games-dataset) (Hugging Face)
- **Boyut:** 125.855 oyun, 40 değişken
- Veri, notebook çalıştırıldığında `huggingface_hub` ile otomatik indirilir.

## Uygulanan Adımlar

| # | Adım | Öne çıkan sonuç |
|---|------|-----------------|
| 1 | Veri okuma ve veri tipleri | Kayık başlık satırı (`DiscountDLC count`) tespit edilip düzeltildi |
| 2 | Veri kalitesi değerlendirmesi | Ortalama bütünlük %81,5 |
| 3 | Veri temizleme | Tarih, aralık ve liste kolonları dönüştürüldü (40 → 47 kolon) |
| 4 | Eksik veri analizi | Gizli/yapısal eksikler bulundu, Metacritic eksikliği MAR |
| 5 | Aykırı değer yönetimi | IQR, z-skor ve alan kuralları; 7 hatalı değer NaN yapıldı |
| 6 | Tekrarlı / tutarsız / gürültülü veri | 111 tekrar ve 8.222 playtest/demo kaydı çıkarıldı |
| 7 | Dönüşüm ve ölçekleme | log1p ve Yeo-Johnson karşılaştırması, StandardScaler |
| 8 | Keşifsel veri analizi (EDA) | Dağılımlar, korelasyonlar, sınıf dengesi |
| 9 | Özellik mühendisliği ve seçimi | 38 yeni özellik; 51 adaydan 41'i seçildi |
| 10 | Modele hazır veri ve sızıntı kontrolü | Pipeline ile sızıntısız eğitim/test ayrımı |

## Sonuçlar

Hedef: Bir oyunun yalnızca yayın öncesi mağaza bilgileriyle beğenilip beğenilmeyeceğinin tahmini (olumlu yorum oranı ≥ 0,80, en az 10 yorum).

| Model | Özellik seti | ROC-AUC | F1 |
|-------|--------------|---------|-----|
| Random Forest | Temiz | 0,754 | 0,707 |
| Lojistik Regresyon | Temiz | 0,705 | 0,673 |
| Random Forest | Sızıntılı | 1,000 | 1,000 |

Yayın sonrası değişkenler (yorum, tavsiye, sahip sayısı vb.) modele eklendiğinde skor yapay olarak 1,000'e çıkmaktadır. Bu, veri sızıntısının etkisini göstermektedir.

## Dosyalar

```
├── Steam_Veri_On_Isleme.ipynb   # Tüm adımları içeren notebook
├── Veri_On_Isleme_Rapor.pdf     # Akademik rapor
└── README.md
```

Notebook çalıştırıldığında ayrıca şu çıktılar üretilir:
- `steam_clean.csv`: temizlenmiş veri (117.521 satır)
- `steam_ml_ready.csv`: modele hazır veri (56.544 satır, 41 özellik + hedef)

## Çalıştırma

```bash
pip install pandas numpy scikit-learn matplotlib seaborn huggingface_hub
jupyter notebook Steam_Veri_On_Isleme.ipynb
```

Google Colab'da doğrudan açılıp **Run All** ile çalıştırılabilir.

## Yazar

**Ahmetcan PEKTAŞ**
Bilgisayar Mühendisliği YL, Bandırma Onyedi Eylül Üniversitesi
