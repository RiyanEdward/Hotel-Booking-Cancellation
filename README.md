# Hotel-Booking-Cancellation
Data Analysis And Prediction Supervised Learning to Reduce Number of Booking Cancellation

**Problem Statement**

Dalam konteks bisnis perhotelan, apabila pengunjung melakukan pemesanan kamar maka pihak hotel akan menyiapkan beberapa hal untuk menyambut kedatangan mereka, di antaranya:  
>* Menghubungi pengunjung terkait kapan perkiraan datang ke hotel,
>* Membersihkan, merapikan, dan menyiapkan kamar sesuai pesanan pengunjung,
>* Menyiapkan makanan dan minuman untuk menyambut kedatangan pengunjung,
>* Menolak pengunjung lain yang memesan kamar yang telah dipesan (*booked room*), dan
>* Memberi layanan penjemputan di bandara/stasiun/terminal apabila diperlukan.  

Jika tamu tidak jadi menginap di hotel padahal sudah melakukan booking maka kamar hotel yang sudah di siapkan gagal untuk disewakan dan bisa merugikan,biaya operasional yang dikeluarkan untuk menyambut kedatangan tamu juga akan menjadi sia-sia.

**Goals**

Maka berdasarkan permalahan tersebut, hotel ingin memiliki kemampuan untuk memprediksi kemungkinan seorang tamu akan melakukan cancel booking atau tidak, sehingga dapat memfokuskan kamar hotel untuk di sewakan kembali jika seorang tamu melakukan cancel.

Dan juga, Hotel ingin mengetahui apa faktor yang menyebabkan seorang tamu melakukan cancel ataupun tidak, sehingga mereka dapat membuat rencana yang lebih baik untuk mengurangi tingkat cancel pada tamu.

**Data Understanding**

Dataset ini berasal dari paper Jurnal Ilmiah berjudul "Hotel booking demand datasets" yang ditulis oleh Nuno Antonio, Ana Almeida, and Luis Nunes for Data in Brief, Volume 22, February 2019. Penjelasan tiap feature/variabel dari Jurnal bisa Anda akses di  https://www.sciencedirect.com/science/article/pii/S2352340918315191

Apabila ingin mengetahui keterangan di setiap kolom, Anda bisa akses ke: https://www.kaggle.com/jessemostipak/hotel-booking-demand/data. 

# Data Preparation

terdapat missing value yang banyak di kolom agent dan company,missing value disini tidak terlihat saling berkorelasi. tetapi untuk analysis ini kami tidak menggunakan kolom-kolom tersebut karena selain missing value nya sangat banyak dan tidak berguna dalam anaylsis ini. Ada beberapa kolom-kolom yang tidak digunakan karena tidak berfungsi untuk analysis dan modeling. Untuk kolom 'stays_in_weekend_nights','stays_in_week_nights','arrival_date_day_of_month' tidak sesuai dengan pengertian yang dimaksut seperti arrival date day of month yang nilai maximal nya mencapai 31 padahal dalam setahun hanya dalam 12 bulan. jika memang kolom tersebut dibutuhkan, bisa dibuat dari kolom resevation status date yang tersedia. Seteleh itu data sudah siap digunakan ke proses selanjutnya yaitu EDA, Business Insight dan Modelling. Setelah di cleaning data yang awalnya terdiri dari 119.390 rows dan 32 kolom menjadi 87.392 rows dan 29 kolom

# Modeling

Model yang dipiliah adalah random forest. Didasari dari model tree, random forest memiliki keunggulan menjadi model yang lebih stabil karena melakukan atau membuat pohon keputusan sesuai dengan n estimator yang sudah kita tentukan, dari percobaan pohon keputusan berulang dengan feature yang berbeda lalu score nya diambil dari penilaian seluruh pohon yang dibuat. Upaya yang telah dilakukan untuk membuat model menjadi lebih baik dilakukanlah tunning dengan memasukan params grid maxdepth untuk menentukan sebarapa dalam pohon kita, min samples leaf untuk menentukan berapa minimum samples yang ada di daun pohon keputusan, dan min sample split untuk menentukan minimum sample sebelum suatu leaf di split kembali, tetapi hasil tidak berubah signikfikan karena itu cukup menggunakan model benchmark yang sudah kita tentukan.

# Evaluation

Model memprediksi user akan *cancel booking* (membatalkan pesanan), padahal sebenarnya/realisasinya user tidak membatalkan pesanan. **FP**

kerugian yang didapat ketika kita mengabaikan **FP** : 
* dapat merugikan hotel karena bisa dilihat dari data analysis diatas ketika kita memprediksi user akan cancel booking, padahal realitanya user tidak cancel, akan membuat customer merasa dirugikan dan hotel akan kehilangan customer
* dari barplot market segment diatas, 47,43% customer memesan kamar hotel melalui online travel agent, dan yang sudah kita ketahui ketika kita memesan kamar di online travel agent akan ada rating yang diberikan oleh customer kepada hotel. Ketika customer yang kita prediksi merasa dirugikan memberikan rating yang buruk, akan berdampak pada rating hotel di suatu online travel agent dan dapat menurunkan minat customer lain untuk memesan hotel

Model memprediksi user tidak membatalkan pesanan, padahal sebenarnya/realisasinya user *cancel booking* (membatalkan pesanan). **FN**

kerugian yang didapat ketika mengabaikan **FN**:
* kerugian pada FP bisa dilihat dari benefit dan persiapan yang kita berikan ketika tamu diprediksi tidak cancel booking. mulai dari layanan penjemputan jika diperlukan, makan dan minum yang sudah disiapkan dan yang paling merugikan adalah ketika ternyata pengunjung yang diprediksi tidak cancel dan aktual nya cancel, hotel akan rugi karena tidak bisa menyewakan kamar hotel tersebut. 

Scoring yang dipilih adalah f1 score karena kedua kelas tersebut sama-sama dapat memberikan kerugian yang sama besar untuk hotel

# Conclusion

 dari analysis dan visualisasi barplot berdasarkan kolom is_canceled, 37,13% tamu melakukan cancel terhadap pemesanannya. berarti 44.152 orang yang melakukan pembatalan pesanan, jika menggunakan metrics f1 dengan score 74%, maka 26% dari tamu `datang/tidak cancel` tetapi diprediksi cancel, dengan market segment online TA sekitar 30.912 orang yang kemungkinan memberikan rating buruk terhadap hotel karena ketika dia tiba dihotel padahal sudah di prediksi akan melakukan cancel.

Jika diasumsikan kamar di hotel ini adalah 1jt per malam, mungkin hotel bisa dibilang rugi 44.152.000.000 karena kamar tersebut gagal di gunakan padahal sudah dibooking, saat suatu kamar di booking hotel tentu tidak dapat menyewakan kamar tersebut ke tamu lainnya. jika menggunakan model dari metrics f1 score ini, 26% tamu yang diprediksi tidak cancel actualnya cancel maka kita dapat mengurangi kerugian dari gagal nya penyewaan kamar menjadi 30.912.000.000. menurut kami hal tersebut sudah cukup baik karena dapat mengurangi kerugian hotel sebanyak 14 milyar.

* FP dapat merugikan hotel walaupun dampak yang di dapat tidak langsung, tetapi akan berdampak besar di masa yang akan datang karena customer yang dirugikan mungkin tidak akan memesan hotel ini, dan jika mengabaikan FP akan membuat rating buruk di online travel agent yang sudah menjadi market segment terbesar di hotel ini. Sedangkan FN berdampak langsung bagi hotel, bisa dihitung kerugian dari FN mencapai 30M. 

# Recomendation

* Saya merekomendasikan hotel untuk menggunakan model ini karena bisa mengurangi 14ribu orang yang melakukan cancel, jika diasumsikan kamar 1 malam adalah 1jt, maka model ini mampu mengurangi kerugian sebanyak 14M, tetapi untuk meminimalisir FN yaitu tamu yang di prediksi tidak cancel tetapi aktual nya cancel bisa meninjau perjanjian kerja sama dengan travel agent baik itu Online atau Offline dimana sudah menjadi market segment terbesar dari hotel ini untuk menentukan batas minimum hari sebelum melakukan cancel, sehingga hotel dapat menyewakan kamar yang tidak jadi disewa.

* Dan Untuk mengurangi FP atau tamu yang diprediksi cancel tetapi aktualnya tidak, tim customer service bisa menelfon atau melakukan whatsapp untuk mengkonfirmasi kedatangan sehingga dapat mengurangi kemungkinan mengecewakan tamu yang benar benar akan datang sehingga membuat rating hotel tetap baik.
