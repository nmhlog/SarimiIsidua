# 10.4 Segmentation By region Growing and By region Splitting and Merging

## REGION GROWING
Region growing is a procedure that group pixels or subregion into large based on predefined criteria for growth.

---> Start with a set of  "seed" points, and from these grow regions by appending to each seed those neighboring pixels that have predefine properties similar to the seed (such as ranges of intensity or color)

when a priori information is not available, the procedure is to compute at every pixel the same set of properties that ultimately will be used to assign pixels to the region during the growing process.

Jika hasil penghitungan ini menunjukkan kelompok nilai, piksel yang propertinya menempatkannya di dekat pusat massa dari kelompok ini dapat digunakan sebagai seed.

Pemilihan kriteria kemiripan tidak hanya bergantung pada masalah yang dipertimbangkan, tetapi juga pada jenis data gambar yang tersedia. Misalnya, analisis citra satelit penggunaan lahan sangat bergantung pada penggunaan warna. Masalah ini akan jauh lebih sulit, atau bahkan tidak mungkin, dipecahkan tanpa informasi yang melekat yang tersedia dalam gambar berwarna.

Jika citra berwarna monokrom, analisis wilayah harus dilakukan dengan seperangkat deskriptor berdasarkan tingkat intensitas dan sifat spasial (seperti momen atau tekstur).

Kriteria tambahan yang dapat meningkatkan kekuatan algoritme pengembangan wilayah menggunakan konsep ukuran, kemiripan antara piksel kandidat dan piksel yang tumbuh sejauh ini (seperti perbandingan intensitas kandidat dan intensitas rata-rata kawasan yang ditumbuhkan) , dan bentuk wilayah yang sedang tumbuh. Penggunaan jenis deskriptor ini didasarkan pada asumsi bahwa model hasil yang diharapkan tersedia setidaknya sebagian.

Permasalah pada region grown:
1. Deskriptor saja dapat memberikan hasil yang menyesatkan jika properti konektivitas tidak digunakan dalam proses pengembangan wilayah.
Sebagai contoh: Visualisasikan susunan acak piksel yang memiliki tiga nilai intensitas berbeda. Mengelompokkan piksel dengan nilai intensitas yang sama.
2. Permasalah lainnya dalam region growing is the formulation of a stopping rule.: Region growth harus berhenti jiga tidak ada pixel yang memenuhi kriteria untuk pencocokan didalam region tersebut. Kriteria seperti nilai intensitas, tekstur, dan warna bersifat lokal dan tidak memperhitungkan “sejarah” pertumbuhan wilayah. 

Misalkan: f (x, y) menunjukkan gambar masukan; 
S (x, y) menunjukkan larik benih yang berisi 1 lokasi titik benih dan 0 di tempat lain; 
Q menunjukkan predikat yang akan diterapkan di setiap lokasi (x, y). 
Array f dan S diasumsikan memiliki ukuran yang sama.


Sebuah dasar dari region-growing algorithm based on 8-connectivity yang dapat di jelaskan sebagai berikut:
1. Cari semua component yang terkoneksi dapa komponen di S(x,y) dan kurangi setiap komponent tersebut menjadi satu pixel. setiap label seperti pixels yang ditemukan sebagai 1. dan pixels lainnya di S dilebelkan sebagai 0.
2. Bentuk gambar dari Fx supaya setiap point (x,y), fq(x,y)=1 jika input gambar memenuhi pridate, Q pada setiap kordinat dan fq(x,y)=0 dan otherwise.
3. Misalkan g adalah gambar yang dibentuk dengan menambahkan ke setiap titik benih di S semua titik bernilai 1 di fQ yang terhubung ke 8 ke titik benih tersebut.
4. Beri label setiap komponen yang terhubung dalam g dengan label wilayah yang berbeda (mis., Bilangan bulat atau huruf). Ini adalah citra tersegmentasi yang diperoleh berdasarkan pertumbuhan wilayah.

## REGION SPLITTING AND MERGING

Misalkan R mewakili seluruh wilayah gambar dan pilih predikat Q.
Salah satu pendekatan untuk mensegmentasi R adalah dengan membaginya secara berturut-turut menjadi daerah kuadran yang lebih kecil dan lebih kecil sehingga, untuk setiap daerah Ri.
Q (Ri) = BENAR. Kami mulai dengan seluruh wilayah, R.
Jika Q (R) = False, kita membagi gambar menjadi kuadran.
Jika Q adalah False untuk kuadran mana pun, kami membagi kuadran itu menjadi subkuadran dan seterusnya

Teknik pemisahan memiliki representasi yang tepat dalam bentuk yang disebut pohon segi empat.

Perhatikan bahwa root dari pohon berhubungan dengan seluruh gambar, dan setiap node berhubungan dengan subdivisi dari sebuah node menjadi empat node turunan. Dalam hal ini, hanya R4 yang dibagi lagi.

Jika hanya pemisahan yang digunakan, partisi terakhir biasanya berisi region yang berdekatan dengan properti yang identik. Kelemahan ini dapat diatasi dengan mengizinkan penggabungan serta pemisahan. Pemenuhan batasan segmentasi yang diuraikan dalam Bagian 10.1 membutuhkan penggabungan hanya wilayah yang berdekatan yang piksel gabungannya memenuhi predikat Q.

Diskusi sebelumnya dapat diringkas dengan prosedur berikut di mana, pada langkah apa pun,

1. Split into four disjoint quadrants any region Ri for which Q(Ri)= FALSE.
2. When no further splitting is possible, merge any adjacent regions Rj and Rk for which Q( Rj U Rk ) = TRUE.
3. Stop when no further mergin is possible.

Berbagai variasi dari tema dasar ini dimungkinkan. Misalnya, hasil penyederhanaan yang signifikan
jika di Langkah 2 kami mengizinkan penggabungan dua wilayah yang berdekatan Rj dan Rk
jika masing-masing memenuhi predikat secara individual. Ini menghasilkan algoritme yang jauh lebih sederhana (dan lebih cepat),

karena pengujian predikat terbatas pada kuadregion individu. Seperti yang ditunjukkan pada contoh berikut, penyederhanaan ini masih mampu memberikan hasil segmentasi yang baik.


# 10.5 Region Segmentation Using Clustering And Superpixel

# Region Segmentation Using K-means Clustering
Ide dasar di balik pendekatan pengelompokan yang digunakan dalam bab ini adalah untuk mempartisi himpunan, Q, pengamatan ke dalam jumlah tertentu, k, kluster.

K-means clustering, setiap observasi ditugaskan ke cluster dengan mean terdekat (oleh karena itu metode ini), dan setiap mean disebut prototipe clusternya.

K-means algrithm is a iterative procedure the successively refines the means util convergence is achieved.

Misalkan {z, z,, z} 1 2… Q adalah himpunan observasi vektor (sampel). Vektor-vektor ini memiliki bentuk dalam segmentasi citra, setiap komponen dari vektor z merepresentasikan atribut piksel numerik.

Jika segmentasi hanya didasarkan pada intensitas skala abu-abu, maka z = z adalah skalar yang mewakili intensitas piksel. Jika kita mengelompokkan gambar berwarna RGB, z biasanya adalah vektor 3-D, yang masing-masing komponennya adalah intensitas piksel di salah satu dari tiga gambar warna primer.

Menemukan minimum ini adalah masalah NP-hard yang tidak diketahui solusi praktisnya. Akibatnya, sejumlah metode heuristik yang mencoba menemukan perkiraan minimum telah diajukan selama bertahun-tahun

Algorithm is as follows:
1. Initialize the algorithm
2. Assign sample to clusters
3. Update the cluster center (means)
4. Test for completion.

gambar fi halaman 772

Di mana mi adalah vektor rata-rata (atau sentroid) sampel di himpunan Ci dan || arg || adalah norma vektor dari argumen tersebut. Norma eucliden digunakan, jadi istilah || z-mi || adalah JARAK EUCLIDEN yang sudah dikenal dari sampel di Ci menjadi mi. Singkatnya, persamaan ini syas bahwa kita tertarik untuk mencari himpunan C = {C1, C2, C3 .... Ck} seperti jumlah jarak dari setiap titik dalam suatu himpunan ke rata-rata himpunan tersebut adalah minimum.

ketika T = 0, algoritma ini diketahui berkumpul dalam jumlah terbatas dari iterasi ke minimum lokal. tidak dijamin untuk menghasilkan minimum global yang diperlukan untuk meminimalkan.

Hasil pada konvergensi bergantung pada nilai awal yang dipilih untuk mi. Pendekatan yang sering digunakan dalam analisis data adalah dengan menentukan nilai tengah awal sebagai k sampel yang dipilih secara acak dari kumpulan sampel yang diberikan, dan untuk menjalankan algoritme beberapa kali, dengan kumpulan sampel initila acak baru setiap kali. Ini untuk menguji "stabilitas" solusi. Dalam segmentasi citra, hal yang penting adalah nilai yang dipilih untuk k karena menentukan jumlah wilayah yang tersegmentasi; dengan demikian, banyak gerakan printhead jarang digunakan.

## REGION SEGMENTATION USING SUPERPIXELS

Ide di balik superpixel adalah mengganti grid piksel standar dengan mengelompokkan piksel ke dalam wilayah primitif yang lebih bermakna secara perseptual daripada piksel individual.

Tujuannya adalah untuk mengurangi beban komputasi dan meningkatkan kinerja algoritma segementasi dengan mengurangi detail yang tidak relevan.



