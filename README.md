# Customer Churn Analysis and Prediction in Telecom
---


Latar belakang: Dalam industri telekomunikasi, tingkat churn (pelanggan yang berhenti menggunakan layanan) adalah indikator penting dalam menilai kepuasan dan loyalitas pelanggan. Untuk mempertahankan pelanggan dan meningkatkan pendapatan, perusahaan harus memahami karakteristik dan perilaku pelanggan melalui data.

Pertanyaan bisnis:

1. Segementasi Pelanggan
- Apa Apa saja segmen pelanggan yang dapat dibentuk berdasarkan perilaku penggunaan layanan seperti jumlah panggilan, SMS, dan data yang digunakan?

2. Churn dan Risiko
- Apakah pelanggan dari provider tertentu memiliki tingkat churn yang lebih tinggi
- Adakah hubungan antara jumlah pemakaian layanan (panggilan/SMS/data) dengan kemungkinan churn?
- Apakah pelanggan dengan estimasi gaji rendah memiliki risiko churn yang lebih tinggi?

3. Strategi CRM
- Segmen pelanggan mana yang memiliki pemakaian tinggi tetapi tetap menunjukkan risiko churn tinggi?
- Bagaimana churn rate berdasarkan usia atau jumlah tanggungan?


## Data Understanding
---
Dataset yang digunakan dalam proyek ini bernama "Telecom Churn Dataset" dan dapat diakses melalui [tautan](https://www.kaggle.com/datasets/suraj520/telecom-churn-dataset/code). Dataset ini berisi 243552 baris data pelanggan dari empat mitra telekomunikasi utama di india: Airtel, Reliance Jio, Vodafone, dan BSNL. Kumpulan data ini mencakup berbagai variabel demografi, lokasi, dan pola penggunaan untuk setiap pelanggan, serta variabel biner yang menunjukkan apakah pelanggan tersebut telah berhenti berlangganan atau tidak.

![Image](https://github.com/user-attachments/assets/217c1ba3-8644-47b1-bb3f-66f3cf339f96)

Dari gambar diatas, terdapat 9 variabel bertipe int64 dan 5 variabel bertipe object

| Nama Variabel          | Keterangan                                                            | Contoh Nilai       |
|------------------------|------------------------------------------------------------------------|------------------------------|
| customer_id            | ID unik untuk setiap pelanggan                                         | 243549                        |
| telecom_partner        | Nama penyedia layanan telekomunikasi                                  | Reliance Jio, BSNL      |
| gender                 | Jenis kelamin pelanggan                                                | M / F                    |
| age                    | Usia pelanggan                                                         | 28, 52, 59                   |
| state                  | Negara bagian tempat pelanggan tinggal                                | Mizoram, Assam        |
| city                   | Kota tempat pelanggan tinggal                                          | Delhi, Kolkata           |
| pincode                | Kode pos pelanggan                                                     | 110295              |
| date_of_registration   | Tanggal pelanggan mendaftar                                            | '2020-01-01'                |
| num_dependents         | Jumlah tanggungan (anak/keluarga) pelanggan                           | 0, 1, 2, 3                      |
| estimated_salary       | Perkiraan gaji pelanggan (dalam INR)                                  | 38722, 148828               |
| calls_made             | Jumlah panggilan yang dilakukan                                        | 44, 28, 80                   |
| sms_sent               | Jumlah SMS yang dikirim                                                | 9, 45, 7                   |
| data_used              | Jumlah data internet yang digunakan                 | 4102, 7521, 1125              |
| churn                  | Status churn (1 = berhenti, 0 = tetap pelanggan)                       | 0 atau 1                     |


## Data Cleansing

Pada tahap ini, dilakukan beberapa langkah penting untuk memastikan kualitas data sebelum proses analisis lebih lanjut. Pertama, dilakukan penghapusan kolom yang dinilai tidak relevan. hasil dari deskripsi statistik menemukan beberapa kolom seperti calls_made, sms_sent, dan data_used memiliki nilai negatif pada beberapa baris sehingga diputuskan untuk menghapus baris yang nilainya negatif. Proses selanjutnya adalah pemeriksaan terhadap missing values. Hasil pemeriksaan menyeluruh menunjukkan bahwa tidak ada data yang hilang atau kosong di seluruh kolom dataset. dalam proses pengecekan data duplikat tidak ditemukan data duplikat atau data ganda pada dataset.

## Exploratory Data Analysis (EDA)
---
pada tahap ini, langkah pertama adalah melihat distribusi data:

![Image](https://github.com/user-attachments/assets/70794c37-17c8-4875-bcff-45377c70a49c)

Dari gambar diatas jumlah distribusi per kategori:
1. Reliance Jio = 56123
2. Vodafone = 55990
3. Airtel = 55937
4. BSNL = 55916


![Image](https://github.com/user-attachments/assets/74c57b57-38a4-4c6f-909c-3da72d94144b)

Dari gambar diatas jumlah distribusi per kategori:
1. M = 134287
2. F = 89779 

![Image](https://github.com/user-attachments/assets/994a1eab-e40d-4d13-88ab-55c5411b99df)

1. Uttarakhand      =           8149
2. Madhya Pradesh    =          8112
3. Arunachal Pradesh  =         8104
4. Maharashtra         =        8099
5. Karnataka            =       8094
6. Jharkhand             =      8043
7. Rajasthan              =     8038
8. Haryana                 =    8037 
9. Uttar Pradesh            =   8033
10. Mizoram                  =   8028
11. Tamil Nadu               =   8022
12. Sikkim      =                8003
13. Kerala       =               8002
14. Odisha        =              8002
15. Himachal Pradesh    =        8001
16. Goa                  =       7995
17. Chhattisgarh          =      7992
18. Telangana              =     7988
19. Andhra Pradesh          =    7981
20. Bihar                    =   7972
21. Tripura         =            7960
22. Meghalaya        =           7959
23. Manipur           =          7929
24. Gujarat            =         7895
25. Punjab              =        7890
26. Nagaland             =       7884
27. West Bengal           =      7883
28. Assam                  =     7871

![Image](https://github.com/user-attachments/assets/1f033569-4969-4850-9545-fbb7a35b3a1c)

Dari gambar diatas distribusi per kategori:
1. Chennai      =      37535
2. Kolkata       =     37423
3. Hyderabad      =    37373 
4. Bangalore       =   37305      
5. Delhi            =  37210      
6. Mumbai            = 37120 


![Image](https://github.com/user-attachments/assets/e3bb1137-e01b-43df-afee-1f48f17ec770)

Dari gambar diatas menggunakan histogram untuk melihat distribusi dari variabel numerik seperti age, num_dependents, estimated_salary, calls_made, sms_sent, data_used, dan churn.


### 1. Apa saja segmen pelanggan yang dapat dibentuk berdasarkan perilaku penggunaan layanan seperti jumlah panggilan, SMS, dan data yang digunakan?
### 1.1 Panggilan

![Image](https://github.com/user-attachments/assets/e8bce759-8c78-4b93-89f0-44ac7cd0b12c)

dari grafik diatas, pelanggan dikelompokkan ke dalam tiga segmen berdasarkan intensitas penggunaan layanan panggilan:
- Sedikit: 52.605 pelanggan (23.5%)
- Sedang: 82.638 pelanggan (36.9%)
- Banyak: 88.723 pelanggan (39.6%)

Dari distribusi ini, kita bisa melihat bahwa mayoritas pelanggan berada pada kategori sedang dan banyak melakukan panggilan, dengan total gabungan mencapai sekitar 76% dari total populasi.

![Image](https://github.com/user-attachments/assets/9131b038-c4e9-47c7-9a4d-5d8f3bcbf78a)

Grafik ini menunjukkan bahwa rata-rata usia pelanggan pada ketiga kategori penggunaan panggilan nyaris sama:
- Sedikit: 46.1 tahun
- Sedang: 46.1 tahun
- Banyak: 46.0 tahun

Selisihnya sangat kecil, hanya 0.1 tahun — sehingga secara statistik bisa dianggap tidak signifikan.


![Image](https://github.com/user-attachments/assets/1e92d5c6-738e-422d-b1a6-3821632f31b7)

Grafik ini menunjukkan distribusi pelanggan tetap dan berhenti (churn) pada masing-masing kategori penggunaan panggilan:
| Kategori Panggilan | Tetap (%) | Berhenti (%) |
| ------------------ | --------- | ------------ |
| **Banyak**         | 79.8%     | 20.2%        |
| **Sedang**         | 80.2%     | 19.8%        |
| **Sedikit**        | 80.1%     | 19.9%        |


![Image](https://github.com/user-attachments/assets/090f79db-6108-4f50-ab10-e8076f5668fc)

Dari distribusi ini bisa kita lihat bahwa laki-laki cenderung mendominasi semua kategori penggunaan panggilan, bahkan makin tinggi intensitas penggunaan, persentase laki-laki makin besar. Artinya, dari sisi perilaku, gender laki-laki menunjukkan kecenderungan lebih aktif dalam komunikasi via telepon.

Dari sisi pemasaran, bisa jadi pendekatan campaign segmented berdasarkan gender dapat dipertimbangkan untuk promosi paket panggilan.



![Image](https://github.com/user-attachments/assets/660d7843-a856-42aa-83a0-7c2bec0f8c4c)

Distribusi pelanggan dalam tiga kategori pendapatan (Rendah, Sedang, Tinggi) di tiap kategori panggilan sangat mirip:
- Masing-masing segmen punya distribusi ±30% untuk pendapatan rendah & sedang
- Sekitar 38.5% pelanggan di setia kategori panggilan berasal dari kelompok berpendapatan tinggi

Pemakaian layanan panggilan tidak menunjukkan deviasi besar antar level pendapatan, tapi kelompok pendapatan tinggi secara konsisten menjadi porsi terbesar. Dari perspektif strategi revenue, pelanggan high income yang aktif menelpon mungkin bisa dijadikan target utama program loyalitas atau bundling premium, karena mereka punya daya beli yang tinggi dan potensi untuk dilibatkan dalam produk bernilai tambah.

| Variabel             | Pengaruh terhadap Penggunaan Panggilan | Insight Utama                                           |
| -------------------- | -------------------------------------- | ------------------------------------------------------- |
| **Jumlah Panggilan** | —                                      | Mayoritas pelanggan ada di kategori “Banyak” & “Sedang” |
| **Usia**             | Tidak signifikan                       | Rata-rata hampir sama (sekitar 46 tahun)                |
| **Churn**            | Tidak berhubungan langsung             | Rasio churn merata di semua kategori                    |
| **Gender**           | Signifikan                             | Laki-laki lebih aktif menggunakan layanan panggilan     |
| **Pendapatan**       | Konsisten                              | Pendapatan tinggi sedikit lebih dominan                 |


### 1.2 SMS

![Image](https://github.com/user-attachments/assets/38abdcfe-71e0-4872-af3f-9eba8e72f88e)

Dari grafik diatas:
- Mayoritas pelanggan berada pada kategori Sedikit SMS (49,7% dari total pelanggan).
- Sisanya hampir seimbang antara kategori Sedang (24,5%) dan Banyak (25,8%).

Penggunaan SMS tidak lagi menjadi preferensi utama sebagian besar pelanggan, mengingat hampir separuh populasi berada di level pemakaian rendah. Ini bisa disebabkan oleh pergeseran ke layanan komunikasi berbasis data seperti WhatsApp, Telegram, atau aplikasi chatting lainnya.


![Image](https://github.com/user-attachments/assets/79a5bc7f-2bb9-486b-800c-82a7e4f2ad05)

Dari grafik diatas:
- Rata-rata usia pelanggan cenderung seragam di seluruh kategori penggunaan SMS, yakni sekitar 46 tahun.
- Kategori “Sedikit” dan “Banyak” menunjukkan angka 46.1 tahun, sedangkan “Sedang” sedikit lebih muda di angka 46.0 tahun.

Tidak ada perbedaan signifikan dalam usia yang berkorelasi dengan tingkat penggunaan SMS. Artinya, usia bukanlah faktor utama yang memengaruhi intensitas penggunaan SMS.


![Image](https://github.com/user-attachments/assets/544dc116-9668-4c4c-b6a3-fd02e1a584bf)

dari grafik diata, persentase pelanggan yang berhenti (churn) relatif serupa di semua kategori. hal ii mengindikasikan bahwa frekuensi SMS tidak berdampak signifikan terhadap churn rate.

![Image](https://github.com/user-attachments/assets/089ed4a7-2977-4234-9cc5-f96d5e623c42)

dari grafik diatas:
- Pada kategori Sedikit, proporsi pria lebih besar (60.0% M vs 40.5% F).
- Pada kategori Sedang, proporsi wanita sedikit lebih besar (60.2% F vs 40.0% M).
- Pada kategori Banyak, dominan pria (59.5% M vs 39.8% F).

terlihat bahwa penggunaan SMS lebih banyak dilakukan oleh pria, kecuali pada kategori sedang.

![Image](https://github.com/user-attachments/assets/bb6f2806-3dd6-4bc8-b6b1-02edc0cd4472)

pelanggan dengan pendapatan tinggi merupakan mayoritas, dengan porsi mencapai 38% pada tiap kategori SMS

| **Variabel**   | **Pengaruh terhadap Penggunaan SMS** | **Insight Utama**                                                          |
| -------------- | ------------------------------------ | -------------------------------------------------------------------------- |
| **Jumlah SMS** | —                                    | Mayoritas pelanggan ada di kategori “Sedikit”                              |
| **Usia**       | Tidak signifikan                     | Rata-rata usia hampir sama (sekitar 46 tahun) di semua kategori            |
| **Churn**      | Tidak berhubungan langsung           | Churn rate relatif stabil di semua kategori (±20%)                         |
| **Gender**     | Cukup signifikan                     | Pria mendominasi kategori “Sedikit” & “Banyak” wanita dominan di “Sedang” |
| **Pendapatan** | konsisten             | Sebagian besar pelanggan berpendapatan tinggi (38%) secara umum          |


### 1.3

![Image](https://github.com/user-attachments/assets/3d5b5045-9ba6-4a85-bb4a-32f90470707f)

Dari grafik diatas, kategori sedang mendominasi dengan setengah dari total populasi pelanggan sedangkan kategori sedikit dan banyak memiliki proporsi seimbang. ini mengindikasikan bahwa mayoritas pelanggan menggunakan data dalam jumlah moderat, sementara kategori sedikit dan banyak memiliki seperempat porsi.

![Image](https://github.com/user-attachments/assets/bdf7251a-76ea-4095-b78f-dd0edebd7665)

Grafik ini menunjukkan bahwa rata-rata usia pelanggan pada ketiga kategori penggunaan panggilan nyaris sama:
- Sedikit: 46.1 tahun
- Sedang: 46.0 tahun
- Banyak: 46.1 tahun

Selisihnya sangat kecil, hanya 0.1 tahun — sehingga secara statistik bisa dianggap tidak signifikan.

![Image](https://github.com/user-attachments/assets/9514e786-794a-42b7-af65-6e25b5137545)

Dari grafik diatas menunjukkan proporsi pelanggan yang tetap menggunakan layanan (tidak churn) dan yang berhenti (churn) untuk tiap kategori data. Tingkat churn relatif stabil di semua kategori penggunaan data (selisih di bawah 1%). Hal ini menunjukkan bahwa intensitas pemakaian data tidak memiliki hubungan kuat terhadap risiko churn. Tidak ada indikasi bahwa pengguna data rendah atau tinggi lebih rentan berhenti dari layanan.

![Image](https://github.com/user-attachments/assets/8a22e7e1-008a-40cf-a22d-e2e3df6f26b0)

dari grafik diatas menunjukkan bahwa laki-laki secara konsisten mendominasi diseluruh kategori penggunaan data. tidak ada perbedaan mencolok antar kategori.

![Image](https://github.com/user-attachments/assets/effb7d7a-f6d7-4a02-8dd9-aafc25746173)

terlihat dari grafik diatas menunjukkan proporsi pendapatan stabil di semua kategori, dengan dominasi pelanggan berpendapatan tinggi (38%), disusul oleh kelompok pendapatan sedang dan rendah yang hampir seimbang. Namun, bisa disimpulkan bahwa pengguna data secara umum sedikit lebih didominasi oleh pelanggan berpendapatan tinggi.

| **Variabel**         | **Pengaruh terhadap Penggunaan Data** | **Insight Utama**                                                              |
| -------------------- | ------------------------------------- | ------------------------------------------------------------------------------ |
| **Jumlah Data Used** | —                                     | Mayoritas pelanggan ada di kategori “Sedang” (50%)                             |
| **Usia**             | Tidak signifikan                      | Rata-rata usia hampir sama (sekitar 46 tahun) di semua kategori                |
| **Churn**            | Tidak berhubungan langsung            | Churn rate sangat mirip di semua kategori (sekitar 20%)                        |
| **Gender**           | Konsisten                             | Laki-laki mendominasi semua kategori penggunaan data (\~60%)                   |
| **Pendapatan**       | Stabil & mirip di semua kategori      | Pendapatan tinggi sedikit lebih dominan (\~38%), tidak ada pengaruh signifikan |


### Segmen Pelanggan

![Image](https://github.com/user-attachments/assets/48f76951-0917-411a-87a1-28e15ceae12a)

Heavy Users menunjukkan konsumsi paling tinggi di semua jenis layanan, terutama pada data dan panggilan, menjadikan mereka target potensial untuk layanan premium atau bundling. Heavy Users bisa diprioritaskan untuk promosi loyalitas atau paket eksklusif. Medium Users cocok untuk strategi upselling karena masih berpotensi naik ke kategori heavy. Light Users dapat ditargetkan dengan penawaran bundle hemat agar penggunaan meningkat secara bertahap.




### 2.1 Apakah pelanggan dari provider tertentu memiliki tingkat churn yang lebih tinggi

![Image](https://github.com/user-attachments/assets/bcc2dfce-093c-4a9c-8965-00c0174596da)
Semua provider memiliki churn rate yang hampir identik (~20%), dengan variasi yang sangat kecil (sekitar ±0,5%). Airtel memiliki churn rate tertinggi (20,32%), sedangkan BSNL yang paling rendah (19,83%). Namun, perbedaan ini masih dalam batas confidence interval yang saling tumpang tindih, sehingga belum signifikan secara statistik. Tidak ada satu provider pun yang tampak secara jelas lebih berisiko dalam hal churn, meskipun tren kecil menunjukkan Airtel sedikit lebih rentan. Tidak ada perbedaan yang mencolok dalam tingkat churn antar provider. Semua memiliki churn rate yang konsisten di kisaran 19,8%–20,3%

![Image](https://github.com/user-attachments/assets/c4e1ec1e-ec4a-4847-a75b-136d0128cf71)

Distribusi churn menggambarkan proporsi jumlah pelanggan churn dari masing-masing provider terhadap total churn.
- Airtel mendominasi tipis dengan 25,4% dari seluruh pelanggan churn, diikuti Reliance Jio (25,0%), Vodafone (24,9%), dan BSNL (24,8%).
- Ini berarti jumlah pelanggan churn relatif merata di semua provider.


Tidak ada provider yang menunjukkan tingkat churn yang secara signifikan lebih tinggi dari yang lain. Meskipun Airtel sedikit lebih tinggi dalam hal distribusi churn dan churn rate, perbedaannya tidak cukup besar untuk dinilai sebagai masalah khusus. Hal ini menunjukkan bahwa churn mungkin lebih dipengaruhi oleh karakteristik pelanggan atau pengalaman layanan, bukan nama provider.


### 2.2 Adakah hubungan antara jumlah pemakaian layanan (panggilan/SMS/data) dengan kemungkinan churn?

![Image](https://github.com/user-attachments/assets/0a7c1f15-c228-42f3-9616-83a14ac364d6)


![Image](https://github.com/user-attachments/assets/8dbf9f06-b8cb-4227-ac94-a0578821ee63)

dari gambar diatas menunjukkan tidak ada korelasi kuat antara intensitas penggunaan layanan dengan risiko churn.

![Image](https://github.com/user-attachments/assets/b236a0e7-a34c-4c40-8ba8-01f9fa1dd1bd)

| Segmen Penggunaan | Jumlah Pelanggan | Jumlah Churn | Churn Rate (%) |
| ----------------- | ---------------- | ------------ | -------------- |
| **Low**           | 56,004           | 11,032       | 19.70%         |
| **Medium**        | 111,988          | 22,639       | 20.22%         |
| **High**          | 55,974           | 11,125       | 19.87%         |

Gambar dan tabel diatas menunjukkan churn rate sedikit lebih tinggi pada segmen penggunaan sedang, namun perbedaannya tidak signifikan secara praktis.

![Image](https://github.com/user-attachments/assets/23c10727-201c-4d76-bd9c-3b665e5e97f3)

Hasil dari uji Mann-Whitney (U-Test) menunjukkan Semua p-value > 0.05 Tidak ada perbedaan signifikan antara pelanggan churn dan tidak churn dalam hal penggunaan layanan.

ANOVA p-value = 0.0306
Secara statistik, ada perbedaan kecil dalam rata-rata penggunaan antar grup churn vs non-churn, tapi efeknya sangat kecil.

![Image](https://github.com/user-attachments/assets/15e9c429-410e-4fd0-90af-c0218a4395ee)

LLR p-value = 0.5776 Model tidak signifikan secara keseluruhan. Model prediksi churn berbasis pemakaian layanan saja tidak memiliki kekuatan prediktif berarti.

Berdasarkan hasil korelasi, segmentasi, uji statistik, dan regresi logistik, jumlah pemakaian layanan (panggilan/SMS/data) tidak memiliki hubungan signifikan dengan risiko churn.

### 2.3 Apakah pelanggan dengan estimasi gaji rendah memiliki risiko churn yang lebih tinggi?

![Image](https://github.com/user-attachments/assets/ada9adbb-cfb4-413b-a6e1-4e6ea2f8b504)

Pelanggan dengan gaji rendah memiliki churn rate sedikit lebih tinggi, namun selisihnya kecil (<0.4%). Rata-rata gaji hampir sama antara pelanggan churn dan tidak churn.

![Image](https://github.com/user-attachments/assets/c0b4a64f-59ec-4922-964c-35c023366cfd)

p-value > 0.05 tidak signifikan secara statistik. Tidak cukup bukti bahwa rata-rata gaji berbeda secara signifikan antara pelanggan churn dan tidak churn.

![Image](https://github.com/user-attachments/assets/585a5cec-1d52-45bc-8159-951c6d80fc74)

p-value > 0.05 tidak ada hubungan signifikan antara segmen gaji dan churn berdasarkan distribusi kategori.

![Image](https://github.com/user-attachments/assets/5428ecf5-15e7-4dd8-b5be-b75fdfede96e)

 Insight: Meskipun koefisien bernilai negatif (semakin tinggi pendapatan cenderung lebih kecil churn),
nilai p > 0.05 → tidak signifikan secara statistik.

Berdasarkan hasil uji statistik, tidak ditemukan hubungan yang signifikan antara estimasi gaji pelanggan dan kemungkinan mereka melakukan churn.
- Rata-rata gaji pelanggan churn dan tidak churn hampir sama.
- Hasil uji-T (p = 0.083) menunjukkan perbedaan tersebut tidak cukup kuat secara statistik untuk dianggap berarti.
- Hasil logistic regression juga mendukung hal ini — variabel estimated_salary memiliki pengaruh yang sangat kecil dan tidak signifikan terhadap churn.

Kesimpulan utama: Estimasi gaji bukanlah faktor utama yang memengaruhi risiko churn dalam data ini.


### 3.1 Segmen pelanggan mana yang memiliki pemakaian tinggi tetapi tetap menunjukkan risiko churn tinggi?

![Image](https://github.com/user-attachments/assets/8d73e813-ca17-4650-962d-f66d8adefd8a)

![Image](https://github.com/user-attachments/assets/2a37f77d-6e1e-48b1-95a8-1fa64f490503)

![Image](https://github.com/user-attachments/assets/05a56ecb-597c-4ca2-978e-d5a6ab06f65d)

Insight: 
- Dari grafik dan tabel terlihat bahwa segmen "Medium usage" memiliki tingkat churn paling tinggi, yaitu 20.2%.
- Meski secara pemakaian tidak setinggi segmen “High usage”, justru kelompok “Medium” menunjukkan jumlah pelanggan churn tertinggi (22.639 orang).
- Di sisi lain, segmen “High usage” memiliki churn rate 19.9%, yang relatif tidak berbeda jauh dari Low dan Medium, meski berada di kuartil atas pemakaian (threshold > 7632).

Interpretasi Strategis:
Dari sudut pandang manajemen hubungan pelanggan (CRM), segmen “Medium usage” muncul sebagai kelompok paling berisiko. Mereka menggunakan layanan secara cukup aktif, tapi churn rate mereka justru lebih tinggi dibanding segmen “High usage”, yang notabene pengguna berat. Ini menunjukkan bahwa aktivitas penggunaan tidak otomatis menjamin loyalitas. Bisa jadi pelanggan di segmen ini merasa bahwa value atau kepuasan yang mereka terima tidak sebanding dengan biaya atau ekspektasi mereka, sehingga cenderung berpindah ke kompetitor. Sementara itu, segmen High usage juga patut diperhatikan, karena meskipun churn rate-nya sedikit lebih rendah, mereka merupakan kelompok yang menyumbang penggunaan dan potensi revenue tertinggi.

### 3.2 Bagaimana churn rate berdasarkan usia atau jumlah tanggungan?

![Image](https://github.com/user-attachments/assets/eb01a04f-dfeb-431f-95b6-15b1bbdaff26)

Semua kelompok menunjukkan churn rate yang sangat mirip (hampir datar di 20%). Ini mengindikasikan bahwa usia bukanlah faktor utama yang memengaruhi keputusan pelanggan untuk berhenti berlangganan. Usia pelanggan tidak bisa dijadikan dasar segmentasi risiko churn secara efektif.

![Image](https://github.com/user-attachments/assets/b71b2cfe-b964-4cfc-8206-60f1d8048cf9)

Pelanggan dengan 0 tanggungan sedikit lebih kecil risiko churn-nya dibanding yang memiliki 1–4 tanggungan, namun selisihnya sangat tipis dan tidak signifikan secara bisnis. Mirip seperti usia, jumlah tanggungan tidak memberikan sinyal yang kuat terhadap churn.

Tidak ada perbedaan signifikan dalam churn rate antar kelompok usia maupun tanggungan. Perubahan persentase sangat kecil (maksimum ±0.1%), sehingga tidak relevan secara operasional. Ini menunjukkan bahwa faktor demografis usia dan tanggungan tidak menjadi driver utama churn dalam dataset ini.


## Prediction
### Eksplorasi data

![Image](https://github.com/user-attachments/assets/764c6f4f-72fa-45f3-84cf-f2034b668987)

Gambar diatas merupakan informasi dataset yang berisi 223966 baris dan 22 kolom terdiri dari 11 object, 8 int64, dan 3 category.

### Seleksi fitur dan Pembersihan
dilakukan proses penghapus beberapa kolom

![Image](https://github.com/user-attachments/assets/e028bfdd-10c7-4bad-9932-ad028d7d53d2)

setelah dilakukan pengurangan kolom, dataset menjadi 8 kolom.

### Pemeriksaan Kualitas data
Dataset diperiksa dari kemungkinan Missing Value, Duplikat Data, dan Outlier. Hasil pemeriksaan menunjukkan bahwa data bersih dan dari Missing value, Duplikat Data, dan Outlier.

### Transformasi Data
Kolom jenis kelamin diubah menjadi representasi numerik:
- Laki-laki = 1
- Perempuan = 0

Hal ini dilakukan untuk memudahkan proses pemodelan berbasis numerik.

### Analisis Korelasi

![Image](https://github.com/user-attachments/assets/05cfa684-7921-4f09-9643-b4de852622ff)

Analisis korelasi antar variabel numerik dilakukan menggunakan heatmap dan metode .corr(). Hasil menunjukkan bahwa tidak ada korelasi kuat antar variabel.

### Pembagian Data
Dataset dibagi menjadi dua bagian:
- 80% data latih (training set)
- 20% data uji (testing set)

Pembagian ini dilakukan untuk memastikan model bisa belajar dari data dan diuji pada data yang belum pernah dilihat sebelumnya.


### Stadarisasi Fitur
Semua fitur numerik distandarisasi menggunakan StandardScaler. Langkah ini diperlukan agar semua variabel berada pada skala yang seragam

### Penangan Ketidakseimbangan Data

![Image](https://github.com/user-attachments/assets/2b999040-1085-4738-958e-9ff32d9cdee4)

Distribusi churn pada dataset asli menunjukkan bahwa kategori pelanggan churn hanya sekitar 20% dari total.
Untuk mengatasi hal ini, digunakan metode SMOTE (Synthetic Minority Oversampling Technique) yang secara sintetis menyeimbangkan data antara kelas churn dan tidak churn.


![Image](https://github.com/user-attachments/assets/04841ad9-8987-4df5-994d-a149a108178d)

Distribusi label kini seimbang 50:50, yang akan membantu model menghindari bias terhadap kelas mayoritas (non-churn).
### Random Forest

![Image](https://github.com/user-attachments/assets/a6ad3bdf-fea3-423c-85d1-72ebdf49ece6)

Akurasi sebesar 74%, Macro Avg F1-Score 0.49 (Rendah), meskipun kelas imbang tapi performa kelas churn masih sangat rendah.

![Image](https://github.com/user-attachments/assets/d22e6f40-a744-4514-9397-a6512854b297)

- TP (Churn diklasifikasi benar) = 858 → sangat sedikit
- FN (Churn tapi diprediksi Not Churn) = 7,992 → sangat tinggi
- Model banyak gagal mendeteksi pelanggan churn

Kesimpulan Random Forest:
Recall untuk kelas churn = 10% berarti model hanya mendeteksi 1 dari 10 pelanggan churn. Meskipun sudah pakai SMOTE, model masih bias ke kelas mayoritas (Not Churn).

### KNN

![Image](https://github.com/user-attachments/assets/cfdd57dc-dacd-4ba8-8123-9a08c367b8b4)

Akurasi keselurahan 54%
Recal churn (kelas 1) 43%
KNN sedikit lebih baik dalam mengenali pelanggan churn. Namun, trade-off nya adalah lebih banyak false positive (orang yang tidak churn tapi prediksi churn)


![Image](https://github.com/user-attachments/assets/e7659e6e-645e-4373-8509-fb351d798fc8)

- False Negatives (FN): 5064 → Pelanggan churn yang gagal terdeteksi.
- False Positives (FP): 15361 → Pelanggan tidak churn yang dikira akan churn.
- True Positives (TP): 3786 → Pelanggan churn yang berhasil dideteksi.

Kesimpulan KNN:
Model masih jauh dari optimal, namun jauh lebih baik daripada Random Forest dalam mendeteksi churn. Recall churn meningkat drastis (43% vs 10%), artinya lebih banyak pelanggan churn berhasil diidentifikasi, walaupun banyak juga false positive. Ini mungkin lebih berguna dalam konteks CRM, karena lebih baik mencoba mencegah churn (meski ada salah target) daripada tidak mendeteksi sama sekali.

### Naive Bayes

![Image](https://github.com/user-attachments/assets/4b21d05f-262c-4164-94fd-89f41190f3ea)

Akurasi 49%
Recal churn (kelas 1) 51%
Model ini paling sensitif dalam mendeteksi pelanggan churn dibanding model sebelumnya. Namun, akurasi dan presisi churn sangat rendah, artinya banyak kesalahan prediksi (false positives tinggi).

![Image](https://github.com/user-attachments/assets/3b4d70d3-354c-40d8-bb1b-15e3cb24309f)

- False Negatives (FN): 4,523 → churn tapi tidak terdeteksi 
- False Positives (FP): 18,611 → tidak churn tapi diprediksi churn
- True Positives (TP): 4,327 → churn yang berhasil dikenali

Kesimpulan Naive Bayes:
Recall churn paling tinggi (51%), artinya lebih banyak churn yang dikenali. Tapi precision churn sangat rendah (20%), sehingga tingkat kesalahan prediksi churn sangat besar. Akurasi keseluruhan turun jadi 49%, yang menunjukkan trade-off besar antara recall dan precision.

### Kesimpulan utama:
Berdasarkan hasil evaluasi dari model Random Forest, KNN, dan Naive Bayes, dapat disimpulkan bahwa tidak ada model yang memberikan performa yang optimal secara keseluruhan.  Random Forest menunjukkan akurasi tertinggi sebesar 74%, namun sangat lemah dalam mengenali pelanggan yang churn dengan recall hanya 10%. Di sisi lain, Naive Bayes mampu mendeteksi pelanggan churn dengan lebih baik (recall 51%), meskipun akurasi umumnya rendah (49%) dan banyak menghasilkan false positive. KNN menjadi model kompromi, dengan recall churn mencapai 43% dan akurasi 54%. Oleh karena itu, jika tujuan utama adalah mengenali sebanyak mungkin pelanggan yang berisiko churn, maka Naive Bayes lebih unggul. Namun, untuk hasil yang lebih seimbang, KNN dapat menjadi pilihan yang lebih moderat
