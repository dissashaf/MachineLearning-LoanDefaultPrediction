# MachineLearning-LoanDefaultPrediction
Repository ini dibuat untuk kompetisi Hacktiv8 Talent Fair 2023 bersama Home Credit Indonesia. Repository ini mencakup eksplorasi data, model machine learning, serta evaluasi untuk analisis risiko kredit dan prediksi gagal bayar pinjaman.

# **Home Credit Indonesia Default Risk**
`Raden Dissa Shafira | FTDS RMT-019 | 14/06/2023 `

**Introduction**

PT Home Credit Indonesia adalah perusahaan keuangan yang menyediakan layanan pembiayaan kepada pelanggan yang melakukan pembelian baik secara online maupun offline. Sebagai perusahaan `pembiayaan`, Home Credit sangat memerlukan analisis keuangan yang baik untuk mengelola dana pinjaman dengan efektif dan mengatasi risiko keuangan yang mungkin terjadi. Analisis ini diperlukan untuk mengelola dana pinjaman, memperkirakan risiko kredit, menentukan suku bunga yang tepat, dan memastikan kelayakan keuangan pelanggan untuk membayar kembali pinjaman. 

Home Credit Indonesia kini sedang membutuhkan model prediksi untuk mengklasifikasikan golongan pemilik kredit yang membayar tagihan dan gagal bayar (*default*). Untuk melakukan hal tersebut akan digunakan dataset yang berisikan informasi mengenai *client*, demografi *client*, hingga transaksi *client*. Dalam proyek ini, klasifikasi terbagi atas golongan yang melaksanakan pembayaran (index = 0) serta golongan gagal bayar (index = 1).

**Problem statement:** 

Diperlukan model klasifikasi dengan akurasi tinggi untuk dapat memprediksi *default payment* kredit online. Diperlukan adanya percobaan beberapa jenis model untuk dapat membandingkan serta mendapatkan model paling akurat dengan menggunakan beberapa metrik tertentu. ROC-AUC akan digunakan sebagai metrik utama untuk menilai kinerja model secara keseluruhan--sementara metrik lain seperti akurasi dan recall juga akan digunakan sebagai alat analisis tambahan dan pendukung untuk mengukur keberhasilan model.

# Conclusion
**1. Data Exploration:**

- Debitur yang melakukan gagal bayar berjumlah lebih sedikit dengan perbandingan 1 : 4.
- Ada hubungan antara 'EXT_SOURCE...' yang menandakan daya bayar debitur dengan target.
- Ada hubungan antara umur dengan target, dimana ditemui bahwa debitur yang lebih muda cenderung untuk melakukan gagal bayar.
- Kolom mengenai informasi aset (rumah) debitur memiliki jumlah missing value yang sangat tinggi (saling berhubungan). Oleh karena itu, kolom-kolom tersebut disingkirkan dari dataset dengan pertimbangan agar tidak menimbulkan kesalahan asumsi (bila diimputasi dengan nilai lain) serta agar tidak mengurangi banyak kolom (apabila kami memutuskan untuk menyingkirkan baris dibandingkan kolom).

**2. Model Analysis:**
- Model terbaik yang dipilih adalah CatBoost dengan pertimbangan utama karena memiliki nilai ROC-AUC yang paling tinggi. Metrik ini dipilih karena dianggap dapat mewakilkan performa keseluruhan model yang diatih menggunakan data yang tidak seimbang. Selain metrik tersebut, model ini juga memiliki nilai recall score yang sedikit lebih baik dibandingkan model lainnya. 
- Jika dilihat berdasarkan perbandingan train dan test, model train memiliki nilai akurasi 92% sementara model test memiliki akurasi 92%. Namun hal ini tidak selalu menggambarkan suatu model yang good-fit. 
- Model memiliki nilai recall yang sangat rendah sehingga tidak direkomendasikan untuk digunakan untuk memprediksi index 1. 
- Hasil evaluasi model memberikan nilai yang yang tidak seimbang antara prediksi indeks 0 dan 1. Hal ini diakibarkan karena dataset memiliki ketidakseimbangan antara dua klasifikasi tersebut. Dapat dilakukan manipulasi untuk undersampling/ oversampling data sebagai bahan evaluasi.

**3.  Domain:**

Dalam konteks model prediksi, prediksi *default payment* seharusnya membutuhkan nilai recall yang tinggi karena keperluan untuk meminimalisir jumlah False Negatives (ketika debitur *default* salah terprediksi seakan telah membayar).

Sementara itu dengan membandingkan konteks tersebut terhadap model yang telah kami buat, jika dilihat dari perbandingan nilai aktual dan prediksi, diketahui bahwa model masih memiliki kekurangan dimana model memiliki tendensi untuk salah memperkirakan index 1 atau *default* yang terlihat dari jumlah prediksi yang seharusnya berjumlah 4263 namun hanya tergambarkan 216 pada data prediksi. 

Model cenderung untuk mengklasifikasikan debitur gagal bayar sebagai tidak gagal bayar. Hal ini dapat memberikan kerugian serta resiko yang lebih besar dibandingkan kesalahan prediksi pada klasifikasi lainnya. Kesalahan prediksi dapat menyebabkan kerugian dimana kelompok debitur yang seharusnya sudah mendapatkan status *default* dapat terus meminjam dan menggunakan kredit. Kesalahan tersebut dapat menimbulkan kerugian perusahaan pembiayaan yang harus membayar tunggakan yang tidak dapat dibayarkan oleh debitur yang gagal bayar. Selain itu, perusahaan juga dapat kehilangan kepercayaan dari vendor dan partner bisnis yang bekerja sama dengan perusahaan pembiayaan. 

Dengan demikian, kami merasa model memang memiliki kelebihan yang dapat membantu perusahaan penerbit kartu kredit dalam memprediksi kondisi telah membayar atau index 0 dengan cukup baik (f1-score skitar 96%). Namun kami `tidak merekomendasikan` untuk menggunakan model ini sebagai referensi untuk memprediksi kondisi *default* karena dapat memberikan resiko yang besar. 

**4. Further Improvement:**

- Model dapat mencoba menyaring fitur-fitur yang lebih relevan. Dalam proyek ini, karena model yang akan digunakan adalah beberapa model, pemilihan fitur mungkin menjadi kurang fokus sehingga tidak tersaring dengan baik. Untuk selanjutnya, selektor yang akan dipakai juga harusnya lebih sesuai dan berfokus pada model yang ingin dilatih.

- Untuk selanjutnya, bila tidak dianggap menjadi hal yang akan memakan waktu, dapat juga dilakukan tuning dengan pencarian `random/ grid` pada model-model lainnya. Karena bisa jadi model lain memiliki potensi akurasi yang lebih tinggi bila kita juga melakukan *tuning* pada hyperparameternya. 

- Menggunakan SMOTENC atau teknik lainnya untuk untuk membuat dataset menjadi lebih seimbang. Teknik ini seharusnya dapat menaikkan performa model secara signifikan, namun belum kami terapkan karena keterbatasan waktu. 

- Dapat dilakukan transformasi, normalisasi, serta proses lainnya yang mungkin lebih sesuai dengan data guna membuat model dapat bekerja dengan lebih optimal. 

- Memperbarui `data yang belum tersedia`. Sebelumnya telah diketahui bahwa beberapa kolom memiliki data tidak terisi yang mencapai hingga 70% dari total baris data. Data tersebut dapat dicoba untk dilengkapi dengan *follow-up* client. 

- Untuk evaluasi selanjutnya dapat mencoba menggunakan `tabel lain` yang sepertinya juga memiliki peran penting dalam memprediksi pembayaran debitur. Seperti tabel 'Bureau', 'Previous Application', dan lainnya yang tidak sempat kami sertakan karena keterbatasan waktu. 
