# 03_04_Burp Suite_Other Modules

## DECODER - Overview

Modul Burp Suite Decoder memungkinkan kita untuk memanipulasi data. Seperti namanya, kita dapat men*decode* kode informasi yang kita tangkap selama serangan, tetapi kita juga dapat men*encode* data kita sendiri, untuk dikirim ke target. Decoder juga memungkinkan kita untuk membuat *hashsum* data, serta menyediakan fitur Smart Decode yang mencoba mendekode data yang diberikan secara rekursif hingga kembali menjadi plaintext (seperti fungsi "Magic" dari [Cyberchef](https://gchq.github.io/CyberChef/) ).

Pilih modul Decoder pada burp suite.

![Untitled](https://tryhackme-images.s3.amazonaws.com/user-uploads/5d9e176315f8850e719252ed/room-content/3ad809fdb51d36584e0cfc809d9f18e4.png)

Antarmuka ini menawarkan kepada kita sejumlah opsi.

1. Kotak di sebelah kiri adalah tempat kita akan menempelkan atau mengetik teks yang akan di*encode* atau di*decode*. Seperti kebanyakan modul Burp Suite lainnya, kita juga dapat mengirim data ke sini dari bagian lain dari kerangka kerja dengan mengklik kanan dan memilih *Send to Decoder*.
2. Kita memiliki opsi untuk memilih antara memperlakukan input sebagai teks atau nilai byte heksadesimal di bagian atas daftar di sebelah kanan.
3. Kemudian, di bawah pilihan plain text antara bentuk text atau hex terdapat menu _Decode as, Encode as_, dan _Hash._
4. Terakhir, terdapat fitur "Smart Decode", yang mencoba mendekode input secara otomatis.

![Untitled](https://tryhackme-images.s3.amazonaws.com/user-uploads/5d9e176315f8850e719252ed/room-content/986cbf61cc820eb26720f1d9ed4b5a64.png)

Saat kita menambahkan data ke input field, antarmuka akan menduplikasi dirinya sendiri untuk menampung output dari transformasi yang kita lakukan. Kita dapat memilih untuk mengoperasikan ini menggunakan opsi yang sama:

![Untitled](https://tryhackme-images.s3.amazonaws.com/user-uploads/5d9e176315f8850e719252ed/room-content/257ef62054a79fe68172310bfcb4c002.png)

## Decoder - Encoding/ Decoding

**Metode Encoding/ Decoding:**

Berikut merupakan menu yang dapat dipilih saat encode maupun decode.

![Untitled](https://tryhackme-images.s3.amazonaws.com/user-uploads/5d9e176315f8850e719252ed/room-content/8ce7c550edf3a79cafbb7be8468793ff.png)

- **Plain**

Plaintext adalah apa yang kita miliki sebelum melakukan transformasi apapun.

- **URL**

_URL encoding_ digunakan untuk membuat data aman untuk ditransfer dalam URL _web request_. Ini melibatkan pertukaran karakter untuk kode karakter ASCII mereka dalam format heksadesimal, didahului dengan simbol persentase (`%`). *URL encoding* adalah metode yang sangat berguna untuk mengetahui segala jenis pengujian aplikasi web.

Misalnya, saat kita meng*encode* karakter _forward-slash_ (`/`). Kode karakter ASCII untuk garis miring adalah 47. Ini adalah "2F" dalam heksadesimal, membuat URL meng*encode forward-slash* `%2F`. Kita dapat mengkonfirmasi ini dengan Decoder dengan mengetikkan garis miring di kotak input, lalu memilih `Encode as`->`URL`.

![Untitled](https://tryhackme-images.s3.amazonaws.com/user-uploads/5d9e176315f8850e719252ed/room-content/72ecaaf06fc248457d61c079d6d98e8f.png)

- **HTML**

Meng*encode* teks sebagai Entitas HTML melibatkan penggantian karakter khusus dengan _ampersand_ (`&`) diikuti dengan angka heksadesimal atau referensi ke karakter yang diloloskan, lalu titik koma (`;`). Misalnya, tanda kutip memiliki referensi sendiri: `&quot;`. Ketika ini dimasukkan ke dalam halaman web, itu akan diganti dengan tanda kutip ganda (`"`). Metode _encode_ ini memungkinkan spesial karakter dalam bahasa HTML dirender dengan aman di halaman HTML dan memiliki tambahan untuk digunakan untuk mencegah serangan seperti [XSS](https://owasp.org/www-community/attacks/xss/) (Cross-Site Scripting).

Saat kita menggunakan opsi HTML di Decoder, kita dapat meng*encode* karakter apa pun sebagai HTML escaped format atau men*decode* entitas HTML yang ditangkap. Misalnya, untuk men*decode* tanda kutip yang kita sebelumnya, kita ketik varian encode `Decode as`->`HTML`.

![Untitled](https://tryhackme-images.s3.amazonaws.com/user-uploads/5d9e176315f8850e719252ed/room-content/99ccb4aba6cb243b7c3b990e82061a86.png)

- **Base64**

Base64 digunakan untuk meng*encode* data apa pun dalam format yang kompatibel dengan ASCII. Base64 dirancang untuk mengambil data biner (misalnya gambar, media, program) dan meng*encode*nya dalam format yang sesuai untuk ditransfer melalui hampir semua media.

- **ASCII Hex**

Opsi ini mengonversi data antara representasi ASCII dan representasi heksadesimal. Misalnya, kata "ASCII" dapat diubah menjadi bilangan heksadesimal "4153434949". Setiap huruf dalam data asli diambil secara individual dan diubah dari representasi numerik ASCII menjadi heksadesimal. Misalnya, huruf "A" dalam ASCII memiliki [kode karakter](https://www.asciitable.com/) desimal 65. Dalam heksadesimal, ini adalah 41. Demikian pula, huruf "S" dapat diubah menjadi heksadesimal 53, dan seterusnya.

- **Hex**, **Octal**, and **Binary**

Semua metode pengkodean ini hanya berlaku untuk input numerik. Mereka mengkonversi antara desimal, heksadesimal, oktal, dan biner.

- **Gzip**

Gzip menyediakan cara untuk *mengompresi* data. Ini banyak digunakan untuk mengurangi ukuran file dan halaman sebelum dikirim ke browser Anda. Halaman yang lebih kecil berarti waktu pemuatan yang lebih cepat, yang sangat diinginkan bagi pengembang yang ingin meningkatkan skor SEO mereka dan menghindari mengganggu pelanggan mereka. Decoder memungkinkan kita untuk secara manual menyandikan dan mendekode data gzip, meskipun ini bisa sulit untuk diproses karena seringkali tidak valid ASCII/Unicode. Sebagai contoh:

![Untitled](https://tryhackme-images.s3.amazonaws.com/user-uploads/5d9e176315f8850e719252ed/room-content/01db98e37c59a26e307eb29cd9e04139.png)

Kita dapat melakukan decode dan eccode secara bersamaan. Sebagai contoh, kita dapat mengambil sebuah frase ("Burp Suite Decoder"), mengubahnya menjadi ASCII Hex, dan kemudian menjadi oktal:

![Untitled](https://tryhackme-images.s3.amazonaws.com/user-uploads/5d9e176315f8850e719252ed/room-content/9066bc864745b22c4b151133ee5bca4c.gif)

## Decoder - Hashing

Selain fungsi Encoding/Decoding, Decoder juga memberi kita pilihan untuk menghasilkan hashsum untuk data yang kita masukkan.

**Teori:**

Hashing adalah proses satu arah yang digunakan untuk mengubah data menjadi _unique signature_. Untuk menjadi algoritma hashing, output yang dihasilkan harusnya tidak mungkin dibalik. Algoritma hashing yang baik akan memastikan bahwa setiap bagian data yang dimasukkan akan memiliki hash yang benar-benar unik. Misalnya, menggunakan [algoritme MD5](https://en.wikipedia.org/wiki/MD5) untuk menghasilkan hashsum untuk teks "MD5sum" yang dikembalikan `4ae1a02de5bd02a5515f583f4fca5e8c`. Menggunakan algoritma yang sama untuk menghasilkan hashsum untuk "MD5SUM" memberikan hash yang sama sekali berbeda, meskipun ada kesamaan input: `13b436b09172400c9eb2f69fbd20adad`. Untuk alasan ini, hash sering digunakan untuk memverifikasi integritas file dan dokumen, karena bahkan perubahan yang sangat kecil pada file akan mengakibatkan hashsum berubah secara signifikan.

**\*Catatan:** algoritme MD5 tidak digunakan lagi dan tidak boleh digunakan untuk aplikasi modern\* .

Sama halnya, hash juga digunakan untuk menyimpan kata sandi dengan aman sebagai (karena proses hashing satu arah yang berarti bahwa hash tidak akan pernah dapat dibalik) kata sandi akan (relatif) aman bahkan jika database bocor. Ketika pengguna membuat kata sandi, itu di-hash dan disimpan oleh aplikasi. Ketika pengguna mencoba masuk, aplikasi kemudian akan meng-hash kata sandi yang mereka kirimkan dan memeriksanya terhadap hash yang disimpan; jika hash cocok, maka kata sandinya benar. Saat menggunakan metodologi ini, aplikasi tidak perlu menyimpan kata sandi asli (teks biasa).

---

**_Hashing di Decoder:_**

Decoder memungkinkan kita untuk menghasilkan hashsum untuk data secara langsung di dalam Burp Suite; ini bekerja dengan cara yang sama seperti opsi encoding/decoding yang kita lihat di tugas sebelumnya. Secara khusus, kita mengklik menu tarik-turun "Hash" dan memilih algoritma dari daftar:

![Untitled](https://tryhackme-images.s3.amazonaws.com/user-uploads/5d9e176315f8850e719252ed/room-content/891371a92a48a7d0c624571e0421e62f.png)

**_Catatan:_**  *ini adalah daftar yang jauh lebih panjang dibandingkan dengan algoritma encode dan decode -- ini merupakan algoritma hashing yang tersedia di burp suite.*

Melanjutkan contoh sebelumnya, mari kita masukkan "MD5sum" ke dalam kotak input, lalu gulir daftar ke bawah hingga kita menemukan "MD5". Menerapkan ini mengirim kita secara otomatis ke tampilan Hex:

![Untitled](https://tryhackme-images.s3.amazonaws.com/user-uploads/5d9e176315f8850e719252ed/room-content/f4dbf50740b428e44b11a1389866a559.png)

Ini karena output dari algoritma hashing tidak mengembalikan teks ASCII/Unicode murni. Dengan demikian, adalah umum untuk mengambil hasil keluaran dari algoritma dan mengubahnya menjadi string heksadesimal; ini adalah bentuk "hash" yang mungkin Anda kenal.

Mari selesaikan ini dengan menerapkan pengkodean "ASCII Hex" ke hashsum untuk membuat string hex rapi dari contoh awal kita.

Proses lengkapnya bisa dilihat disini:

![Untitled](https://tryhackme-images.s3.amazonaws.com/user-uploads/5d9e176315f8850e719252ed/room-content/5ea1f43139bc40e0af3f5c8d9f3d7241.gif)

## Comparer - Overview

Comparer memungkinkan kita untuk membandingkan dua bagian data, baik dengan kata ASCII atau dengan byte.

Berikut ini merupakan interface comparer pada burp suite.

![Untitled](https://tryhackme-images.s3.amazonaws.com/user-uploads/5d9e176315f8850e719252ed/room-content/cf705ee18f321daa07befb6a05148e5d.png)

Antarmuka ini dapat dibagi menjadi tiga bagian utama:

1. Di sebelah kiri, merupakan item yang dibandingkan. Saat kita memuat data ke Comparer, itu akan muncul sebagai baris dalam tabel ini -- kita kemudian akan memilih dua kumpulan data untuk dibandingkan.
2. Di kanan atas, kita memiliki opsi untuk menempelkan data dari clipboard (Paste), memuat data dari file (Load), menghapus baris saat ini (Remove) dan menghapus semua kumpulan data (Clear).
3. Terakhir, di kanan bawah, kita memiliki opsi untuk membandingkan kumpulan data kita dengan kata atau byte.

Seperti kebanyakan modul Burp Suite, kita juga dapat memuat data ke Comparer dari modul lain dengan mengklik kanan dan memilih "_Send to Comparer_".

Ketika telah memuat data untuk dibandingkan, kita akan mendapatkan pop-up window yang menunjukkan perbandingan kepada kita:

![Untitled](https://tryhackme-images.s3.amazonaws.com/user-uploads/5d9e176315f8850e719252ed/room-content/477667fa86c1d39851bed5979b20a7ae.png)

1. Data yang dibandingkan ini dapat dilihat dalam format teks atau hex. Format awal ditentukan oleh apakah kita memilih untuk membandingkan dengan kata atau byte di jendela sebelumnya, tetapi ini dapat ditimpa dengan menggunakan tombol di atas kotak perbandingan.
2. _Key_ untuk perbandingan ada di kiri bawah, menunjukkan warna mana yang menunjukkan data yang diubah, dihapus, dan ditambahkan di antara dua kumpulan data.
3. Di kanan bawah jendela adalah kotak centang "Sync views". Saat dipilih, ini berarti kedua kumpulan data akan menyinkronkan format -- yaitu jika Anda mengubah salah satunya ke tampilan Hex, yang lain akan melakukan hal yang sama untuk mencocokkan.

Kita akan diberi jumlah total perbedaan yang ditemukan di _title window_.

## Sequencer - Overview

Sequencer memungkinkan kita untuk mengukur *entropi* (atau keacakan) dari "token" -- string yang digunakan untuk mengidentifikasi sesuatu dan, secara teori, harus dihasilkan dengan cara yang aman secara kriptografis. Misalnya, kita mungkin ingin menganalisis keacakan session cookie atau token **C**ross - **S**ite **R**equest **F**orgery (CSRF) yang melindungi _form submission_. Jika ternyata token ini tidak dihasilkan dengan aman, maka kita (secara teori) dapat memprediksi nilai token yang akan datang. Bayangkan saja implikasinya jika token yang dimaksud digunakan untuk pengaturan ulang kata sandi.

![Untitled](https://tryhackme-images.s3.amazonaws.com/user-uploads/5d9e176315f8850e719252ed/room-content/e84858b44655bb09c7f73d879333406c.png)

Terdapat dua metode utama yang dapat kita gunakan untuk melakukan analisis token dengan Sequencer:

- **Live Capture**
  Live Capture adalah sub-tab default untuk Sequencer. Live Capture memungkinkan kita meneruskan _request_ ke Sequencer, yang kita tahu akan membuat token untuk kita analisis. Misalnya, kita mungkin ingin meneruskan _request_ POST ke endpoint login ke Sequencer, karena kita tahu bahwa server akan merespons dengan memberi kita cookie. Dengan permintaan yang diteruskan, kita dapat memberi tahu Sequencer untuk memulai Live Capture: kemudian akan membuat permintaan yang sama ribuan kali secara otomatis, menyimpan sampel token yang dihasilkan untuk analisis. Setelah kita mengumpulkan sampel yang cukup, kita menghentikan Sequencer dan mengizinkannya menganalisis token yang diambil.
- **Manual Load**
  Manual Load memungkinkan kita memuat daftar sampel token yang dibuat sebelumnya langsung ke Sequencer untuk dianalisis. Menggunakan Manual Load berarti kita tidak perlu membuat ribuan _request_ ke target kita, tetapi itu berarti kita perlu mendapatkan daftar besar token yang dibuat sebelumnya.

## Sequencer - Live Capture

Kita akan mulai dengan menangkap permintaan ke `http://MACHINE_IP/admin/login/`dalam Proxy. Klik kanan pada permintaan dan pilih "Send to Sequencer":

![Untitled](https://tryhackme-images.s3.amazonaws.com/user-uploads/5d9e176315f8850e719252ed/room-content/a48b3787771b855bb76d1618960de6ec.gif)

Perhatikan di bagian "Token Location Within Response" kita memiliki opsi untuk memilih antara Cookie, Form field, dan Custom location. Dalam contoh ini, kita menguji `loginToken`, jadi pilih radio button untuk "Form Field":

![Untitled](https://tryhackme-images.s3.amazonaws.com/user-uploads/5d9e176315f8850e719252ed/room-content/9bb4ea43eb0acb59dee493699d336930.png)

Kita dapat dengan aman meninggalkan semua opsi lain secara default dalam hal ini, jadi mari kita lanjutkan dan klik tombol "Start live capture"

Pop up akan muncul memberi tahu kita bahwa kita sedang melakukan Live Capture dan menunjukkan kepada kita berapa banyak token yang telah kita tangkap sejauh ini. Kita harus menunggu sampai kita memiliki jumlah token yang masuk akal (sekitar 10.000 harus dilakukan); semakin banyak token yang kita miliki, semakin akurat analisis kita.

Setelah Anda memiliki sekitar 10.000 token, klik "Pause", lalu pilih tombol "Anayze now":

![Untitled](https://tryhackme-images.s3.amazonaws.com/user-uploads/5d9e176315f8850e719252ed/room-content/715caf9a950bdd3a3c9ec4a5360ae9ca.png)

**\*Catatan:** Kita juga dapat memilih untuk "Stop" capture; namun, dengan memilih untuk Pause, kita membiarkan opsi melanjutkan pengambilan tetap terbuka, jika laporan tidak memiliki cukup sampel untuk menghitung entropi token secara akurat.\*

Jika kita ingin menerima update berkala tentang analisis, kita juga dapat mencentang kotak "Auto analyze". Melakukan hal ini akan memberi tahu Burp untuk melakukan analisis entropi setiap 2000 permintaan atau lebih, memberi kita pembaruan yang sering yang akan semakin akurat karena lebih banyak sampel dimuat ke Sequencer.

Perlu dicatat bahwa pada titik ini, kita juga dapat memilih untuk menyalin atau menyimpan token yang diambil untuk analisis lebih lanjut nanti.

Setelah mengklik tombol "Analyze now", Burp akan melanjutkan untuk menganalisis entropi token kita dan membuat laporan.

## Sequencer - Analysis

Setelah kita memiliki laporan untuk analisis entropi token kita, saatnya untuk menganalisisnya!

Burp melakukan lusinan tes pada sampel token yang diambilnya. Kita tidak akan melihat semua ini karena akan membutuhkan lebih dari satu tugas untuk melakukannya (dan akan sangat intensif matematika untuk memisahkan setiap teknik). Sebagai gantinya, kita akan fokus pada ringkasan yang dihasilkan; namun, Anda dianjurkan untuk melihat sendiri semua hasil tes.

Laporan analisis entropi yang dihasilkan dibagi menjadi empat bagian utama -- yang pertama adalah hasil Summary:

![Untitled](https://tryhackme-images.s3.amazonaws.com/user-uploads/5d9e176315f8850e719252ed/room-content/f4dcdfcbb03e65edd6b31974f09d2254.png)

Ringkasan memberi kita hasil keseluruhan; entropi efektif ; analisis keandalan hasil; dan ringkasan sampel yang diambil.

Secara kolektif, ini seringkali cukup untuk menentukan apakah token dihasilkan dengan aman atau tidak; namun, dalam beberapa kasus, kita mungkin perlu melihat hasil pengujian secara langsung -- ini dapat dilakukan di tab "Character-level analysis" dan "Bit-level analysis". Seperti disebutkan sebelumnya, kita tidak akan membahas ini untuk menghindari menggali kedalaman matematika analisis statistik di ruang ramah pemula. Singkatnya, dengan perkiraan 1% kemungkinan salah berdasarkan data yang diberikan (Tingkat signifikansi: 1%), Burp telah menghitung bahwa entropi efektif token kami *harus* sekitar 117 bit. Ini adalah tingkat entropi yang luar biasa untuk memiliki token yang aman, meskipun harus dicatat bahwa tidak mungkin untuk mengatakan dengan jaminan mutlak bahwa perhitungan ini sepenuhnya akurat, hanya karena sifat topiknya.
