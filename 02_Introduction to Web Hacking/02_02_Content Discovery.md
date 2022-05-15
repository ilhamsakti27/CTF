# Content Discovery

## What is Content Discovery
Konten (Content) dapat berupa banyak hal, file, video, gambar, cadangan, fitur situs web. Konten dapat berupa halaman atau portal yang ditujukan untuk penggunaan staf, versi situs web yang lebih lama, file cadangan, file konfigurasi, panel administrasi, dll. 

Ketika berbicara tentang *Content Discovery*, kita tidak berbicara tentang hal-hal nyata yang dapat kita lihat di situs web; itu adalah hal-hal yang tidak segera disajikan kepada kita dan yang tidak selalu ditujukan untuk akses publik.

Ada tiga cara utama untuk menemukan konten di situs web yang akan kita bahas. Secara Manual, Otomatis dan OSINT (Open-Source Intelligence).

## Manual Discovery

### Robots.txt
Ada beberapa tempat yang dapat kita periksa secara manual di situs web untuk mulai menemukan lebih banyak konten.

File `robots.txt` adalah dokumen yang memberi tahu *search engine* halaman mana yang boleh dan tidak boleh ditampilkan di hasil *search engine* mereka atau melarang *search engine* tertentu merayapi situs web sama sekali. Ini bisa menjadi praktik umum untuk membatasi area situs web tertentu sehingga tidak ditampilkan di hasil search engine. Halaman-halaman ini dapat berupa area seperti portal administrasi atau file yang ditujukan untuk pelanggan situs web. File ini memberi kita daftar lokasi yang bagus di situs web yang pemiliknya tidak ingin kita temukan sebagai penguji penetrasi.

Seperti pada contoh: `http://MACHINE_IP/robots.txt` yang berisikan sebagai berikut
```
User-agent: *
Allow: /
Disallow: /staff-portal
```

### Favicon
Favicon adalah ikon kecil yang ditampilkan di tab alamat atau tab browser yang digunakan untuk branding situs web.

Kadang, ketika framework digunakan untuk membangun situs web, favicon yang merupakan bagian dari instalasi mendapat sisa, dan jika pengembang situs web tidak menggantinya dengan yang khusus, ini dapat memberi kita petunjuk tentang kerangka kerja apa yang digunakan. OWASP menghosting database ikon kerangka umum yang dapat kita gunakan untuk memeriksa favicon target https://wiki.owasp.org/index.php/OWASP_favicon_database. Setelah kita mengetahui framework, kita dapat menggunakan sumber daya eksternal untuk menemukan lebih banyak tentangnya.

#### Practical Exercise
- Buka browser lalu ketikkan url [https://static-labs.tryhackme.cloud/sites/favicon/](https://static-labs.tryhackme.cloud/sites/favicon/). Jika sudah, maka ada note bertuliskan "Website coming soon...". Jika kita mengamati tab, maka ada icon favicon disana.
- Inspect page, kita akan melihat baris berisi tautan ke file `images/favicon.ico`.
- Buka terminal dan ketikkan `curl https://static-labs.tryhackme.cloud/sites/favicon/images/favicon.ico | md5sum` untuk mendownload image favicon dan mendapatkan nilai hash md5. Jika kita mencocokkannya ke web [https://wiki.owasp.org/index.php/OWASP_favicon_database](https://wiki.owasp.org/index.php/OWASP_favicon_database), maka dapat diketahui web ini menggunakan framework cgiirc.

### Sitemap.xml
Tidak seperti file `robots.txt`, yang membatasi apa yang dapat dilihat oleh search engine, file `sitemap.xml` memberikan daftar setiap file yang ingin dicantumkan pemilik situs web di search engine. Ini terkadang dapat berisi area situs web yang sedikit lebih sulit untuk dinavigasi atau bahkan mencantumkan beberapa halaman web lama yang tidak lagi digunakan oleh situs saat ini tetapi masih berfungsi di belakang layar.

Pada machine ini, `sitemap.xml` dapat ditemukan di [http://10.10.231.155/sitemap.xml](http://10.10.231.155/sitemap.xml).

### HTTP Headers
Saat kita membuat request ke server web, server mengembalikan berbagai header HTTP. Header ini terkadang dapat berisi informasi yang berguna seperti perangkat lunak server web dan mungkin bahasa pemrograman/script yang digunakan. Pada contoh di bawah ini, kita dapat melihat server web adalah NGINX versi 1.18.0 dan menjalankan PHP versi 7.4.3. Dengan menggunakan informasi ini, kita dapat menemukan versi perangkat lunak yang rentan digunakan. Coba jalankan perintah curl di bawah ini terhadap server web, di mana -v switch mengaktifkan mode verbose, yang akan menampilkan header.
```     
user@machine$ curl http://10.10.231.155 -v
*   Trying 10.10.231.155:80...
* TCP_NODELAY set
* Connected to 10.10.231.155 (10.10.170.158) port 80 (#0)
> GET / HTTP/1.1
> Host: MACHINE_IP
> User-Agent: curl/7.68.0
> Accept: */*
> 
* Mark bundle as not supporting multiuse
< HTTP/1.1 200 OK
< Server: nginx/1.18.0 (Ubuntu)
< X-Powered-By: PHP/7.4.3
< Date: Mon, 19 Jul 2021 14:39:09 GMT
< Content-Type: text/html; charset=UTF-8
< Transfer-Encoding: chunked
< Connection: keep-alive
```

### Framework Stack
Setelah kita membuat framework situs web, baik dari contoh favicon di atas atau dengan mencari petunjuk di sumber halaman seperti komentar, pemberitahuan hak cipta, atau kredit, kita kemudian dapat menemukan situs web framework tersebut. Dari sana, kami dapat mempelajari lebih lanjut tentang perangkat lunak dan informasi lainnya, yang mungkin mengarah ke lebih banyak konten yang dapat kami temukan.

Dalam url http://10.10.170.158, jika kita meng-inspect element. Dibagian bawah script html terdapat comment yang memberitahu bahwa website ini menggunakan framework tersebut.

## OSINT (Open-Source Intellegence)

### OSINT - Google Hacking / Dorking
Ada juga sumber daya eksternal yang tersedia yang dapat membantu menemukan informasi tentang situs web target kita; sumber daya ini sering disebut sebagai OSINT (Open-Source Intellegence) karena merupakan alat yang tersedia secara bebas untuk mengumpulkan informasi.

**Google Hacking/Dorking** menggunakan fitur search engine canggih Google, yang memungkinkan kita memilih konten khusus. kita dapat, misalnya, memilih hasil dari nama domain tertentu menggunakan filter site:, misalnya (site:tryhackme.com), kita kemudian dapat mencocokkannya dengan istilah pencarian tertentu, misalnya, kata admin (site :tryhackme.com admin) ini kemudian hanya akan mengembalikan hasil dari situs tryhackme.com yang berisi kata admin di isinya. kita juga dapat menggabungkan beberapa filter. Berikut adalah contoh filter lainnya yang dapat kita gunakan:

| Filter | Example | Description |
| ------ | ------- | ----------- |
| site | site:tryhackme.com | Mengembalikan hasil hanya dari url tersebut |
| inurl | inurl:admin | Mengembalikan hasil yang memiliki kata spesifik hanya dari url tersebut |
| filetype | filetype:pdf | Mengembalikan hasil yang dimana adalah sebuah file extension |
| intitle | intitle:admin | Mengembalikan hasil dari title yang mengandung kata tersebut |

> Informasi lebih lengkapnya dapat dilihat [disini](https://en.wikipedia.org/wiki/Google_hacking).

### OSINT - Wappalyzer
Wappalyzer [https://www.wappalyzer.com/](https://www.wappalyzer.com/) adalah alat online dan ekstensi browser yang membantu mengidentifikasi teknologi apa yang digunakan situs web, seperti framework, Content Management System (CMS), pemroses pembayaran, dan banyak lagi, dan bahkan dapat menemukan nomor versinya juga.

### OSINT - Wayback Machine
The Wayback Machine [https://archive.org/web/](https://archive.org/web/) adalah arsip historis situs web yang berasal dari akhir tahun 90-an. kita dapat mencari nama domain, dan itu akan menunjukkan kepada kita setiap kali layanan menggores halaman web dan menyimpan kontennya. Layanan ini dapat membantu mengungkap halaman lama yang mungkin masih aktif di situs web saat ini.

### OSINT - GitHub
Kita dapat menggunakan fitur pencarian GitHub untuk mencari nama perusahaan atau nama situs web untuk mencoba dan menemukan repositori milik target kita. Setelah ditemukan, kita mungkin memiliki akses ke kode sumber, kata sandi, atau konten lain yang belum kita temukan.

### OSINT - S3 Buckets
S3 Buckets adalah layanan penyimpanan yang disediakan oleh Amazon AWS, memungkinkan orang untuk menyimpan file dan bahkan konten situs web statis di cloud yang dapat diakses melalui HTTP dan HTTPS. Pemilik file dapat mengatur izin akses untuk menjadikan file publik, pribadi, dan bahkan dapat ditulis. Terkadang izin akses ini tidak disetel dengan benar dan secara tidak sengaja mengizinkan akses ke file yang seharusnya tidak tersedia untuk umum. Format ember S3 adalah http(s)://{name}.s3.amazonaws.com di mana {name} ditentukan oleh pemiliknya, seperti tryhackme-assets.s3.amazonaws.com. S3 Buckets dapat ditemukan dengan banyak cara, seperti menemukan URL di sumber halaman situs web, repositori GitHub, atau bahkan mengotomatiskan proses. Salah satu metode otomatisasi yang umum adalah dengan menggunakan nama perusahaan diikuti dengan istilah umum seperti {name}-assets, {name}-www, {name}-public, {name}-private, dll.