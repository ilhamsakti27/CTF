# Principles of Security

## Introduction

Pada bab ini, frameworks dan protocols yang dibahas adalah sebagian kecil dari “*Defence in Depth*” atau “Pertahanan di Kedalaman”.

*Defence in Depth* adalah penggunaan beberapa lapisan keamanan yang bervariasi untuk sistem dan data organisasi dengan harapan beberapa lapisan akan membuat redundansi di perimeter keamanan organisasi.

## The CIA Triad

*CIA triad*  adalah sebuah model keamanan informasi yang digunakan dalam pertimbangan selama pembuatan kebijakan keamanan. Model ini juga digunakan sebagai standar industri dalam mengamankan data.

Sejarah berawal dari keamanan informasi tidak dimulai dan/ atau diakhiri dengan keamanan siber, tetapi berlaku juga untuk skenario pengarsipan, penyimpanan record, dll.

Terdiri dari 3 bagian:

- **C**onfidentialy (”kerahasiaan”)
- **I**ntegrity (”integritas”)
- **A**vailability (”ketersediaan”) (**CIA**) <br>
![CIA Triad](https://www.certmike.com/wp-content/uploads/2018/08/cia_triad.png) 
### Confidentiality (”Kerahasiaan”)

Elemen ini “*melindungi data dari akses dan penyalahgunaan yangg tidak sah”*. Memberikan kerahasiaan berarti melindungi data dari pihak-pihak yang tidak dimaksudkan.

Contoh:

Misalnya, catatan karyawan dan dokumen akuntansi akan dianggap sensitif. Kerahasiaan akan diberikan dalam arti bahwa hanya administrator SDM yang akan mengakses catatan karyawan, di mana pemeriksaan dan kontrol akses yang ketat diterapkan. Catatan akuntansi kurang berharga (dan karena itu kurang sensitif), jadi tidak ada kontrol akses yang ketat untuk dokumen-dokumen ini. Atau, misalnya, pemerintah menggunakan sistem peringkat klasifikasi sensitivitas (top-secret, classified, unclassified).

### Integrity (”Integritas”)

*“Kondisi di mana informasi disimpan secara akurat dan konsisten, kecuali jika ada perubahan yg diizinkan.”*

Ini memungkinkan informasi berubah karena akses dan penggunaan yang ceroboh, kesalahan dalam sistem informasi, atau akses dan penggunaan yang tidak sah.

Dalam triad CIA, integritas dipertahankan ketika informasi tetap tidak berubah selama penyimpanan, transmisi, dan penggunaan tidak melibatkan modifikasi informasi. Langkah-langkah harus diambil untuk memastikan data tidak dapat diubah oleh orang yang tidak berwenang (misalnya, dalam pelanggaran kerahasiaan).

### Availability (”Ketersediaan”)

Agar data bermanfaat, data harus tersedia dan dapat diakses oleh pengguna.

Perhatian utama dalam CIA Triad adalah bahwa informasi harus tersedia ketika pengguna yang berwenang perlu mengaksesnya.

*Availability* sangat sering menjadi tolak ukur utama bagi sebuah perusahaan. 

Misalnya, memiliki waktu aktif 99,99% di situs web atau sistem mereka (ini diatur dalam *Service Level Agreements*). Ketika suatu sistem tidak tersedia, seringkali mengakibatkan kerusakan pada reputasi organisasi dan kerugian keuangan.

*Availability* dicapai melalui kombinasi banyak elemen, termasuk:

- Memiliki perangkat keras yang andal dan teruji untuk server teknologi informasi mereka (yaitu server yang memiliki reputasi baik).
- Memiliki teknologi dan layanan yang *redundant* jika terjadi kegagalan pada server utama.
- Menerapkan protokol keamanan yang teruji/ berpengalaman untuk melindungi teknologi dan layanan dari serangan.

## Principles of Privileges

Ini sangat penting untuk mengatur dan mendefinisikan dengan benar berbagai tingkat level akses ke sistem teknologi informasi yang dibutuhkan individu.

***Level akses*** ***ditentukan*** dua faktor utama, yaitu:

- Peran/ fungsi individu di dalam organisasi.
- Informasi sensitif yg disimpan pada sistem.

Dua konsep ini digunakan untuk memberi dan mengatur akses yang benar pada individu. Dua konsep ini digunakan pada ***Privileged Identity Management (PIM)*** dan ***Privileged Access Management (PAM)***.

---

**PIM**: digunakan untuk menerjemahkan pengguna dalam suatu organisasi menjadi peran akses pada suatu sistem.

**PAM**: pengelolaan *privilege system* (hak istimewa) yg dimiliki peran akses.

---

Sederhananya, *privilege* merupakan pengguna yang diberikan jumlah minimum hak istimewa, dan hanya yang benar-benar diperlukan bagi mereka untuk melakukan tugasnya. Orang lain harus percaya terhadap apa yang orang tersebut tulis.

Seperti yang disebutkan sebelumnya, PAM menggabungkan lebih dari menetapkan akses. Ini juga mencakup penegakan kebijakan keamanan seperti manajemen kata sandi, kebijakan audit, dan pengurangan permukaan serangan yang dihadapi sistem.
<br>
<aside>
  💡 If you wanted to manage the privileges a system access role had, the methodology is <b>PAM</b>

</aside>
<br>
<aside>
💡 If you wanted to create a system role that is based on a users role/responsibilities with an organisation, the methodology is <b>PIM</b>

</aside>

## Security Models Continued

Sistem informasi adalah setiap sistem atau bagian dari teknologi yang menyimpan informasi. Untuk mengamankan informasi yang disimpan dibutuhkan *security model*. Berikut merupakan model keamanan populer dan efektif untuk mencapai CIA Triad.

### Model Bell-La Padula

Model ini digunakan untuk mencapai aspek **confidentiality (kerahasiaan)**. Model ini mempunyai beberapa asumsi, seperti sruktur hierarki organisasi yang digunakan, di mana tanggung jawab/ peran setiap orang di definisikan dengan baik.

Model ini bekerja dengan memberikan akses ke potongan data (objek) dengan dasar yang sangat perlu diketahui.

Model ini menggunakan aturan **“no write down, no read up”**.

Keuntungan:

- Kebijakan pada model ini dapat direplikasi ke hierarki organisasi kehidupan nyata (dan sebaliknya).
- Sederhana untuk diterapkan dan dipahami, dan telah terbukti berhasil.

Kekurangan:

- Meskipun seorang user mungkin tidak memiliki akses ke suatu objek, mereka akan mengetahui keberadaanya, jadi pada aspek ini dapat dikatakan tidak bersifat rahasia.
- Model ini bergantung pada sejumlah besar kepercayaan dalam organisasi.

![Bell-La Padula Model](https://miro.medium.com/max/800/1*Xn5qDvXU3CDlFKPC6XO8zQ.png)

Model ini populer di dalam organisasi seperti pemerintahan dan militer. Hal ini karena setiap anggota organisasi dianggap telah melalui proses *vetting. **Vetting*** adalah proses screening di mana latar belakang anggota diperiksa untuk menetapkan resiko yang mereka hadapi terhadap organisasi. Oleh karena itu, anggota yang telah melewati vetting dianggap dapat dipercaya, disitulah model ini cocok.

### Model Biba

Model ini dapat dikatakan dengan model Bell-La Padula tetapi untuk integritas CIA Triad.

Model ini menerapkan aturan untuk objek (data) dan subjek (pengguna) yang dapat diringkas sebagai **“no write up, no read down”**. Aturan ini artinya bahwa subjek dapat membuat atau menulis konten ke objek pada atau di bawah levelnya tetapi hanya dapat membaca objek di atas level subjek.

Keuntungan:

- Sederhana untuk diterapkan.
- Mengatasi keterbatasan model Bell-La Padula dengan menangani kerahasiaan dan integritas data.

Kekurangan:

- Akan ada banyak level akses dan objek. Hal ini dapat diabaikan dengan mudah dengan menerapkan kontrol keamanan.
- Sering mengakibatkan penundaan dalam bisnis. Misalnya, seorang dokter tidak akan bisa membaca catatan yang dibuat oleh seorang perawat di rumah sakit dengan model ini.

![Biba Model](https://miro.medium.com/max/800/1*6Law1m6lmmSahB6eEzGRXw.png)

## Pemodelan Ancaman dan Respon Insiden

**Pemodelan ancaman** adalah proses meninjau, meningkatkan, dan mengetes protokol keamanan yang ada di infrastruktur dan layanan teknologi informasi.

Critical stage pada proses ini adalah mengidentifikasi kemungkinan ancaman atau kerentanan yang mungkin dihadapi oleh suatu sistem atau aplikasi.

Pemodelan ancaman sangat mirip dengan penilaian resiko (*risk assessment)* yang dibuat pada tempat kerja untuk karyawan dan customers. Prinsip tersebut adalah 

![Security Incident](https://miro.medium.com/max/722/0*nL2-vNgHiskor2Zn.png)

Proses di atas merupakan proses kompleks yang membutuhkan review dan diskusi yang terus menerus dengan sebuah tim. Model ancaman yang efektif haruslah meliputi:

- Intelijan ancaman
- Identifikasi aset
- Kemampuan mitigasi
- *Risk assessment*

Untuk membantu hal tersebut, terdapat kerangka kerja seperti **STRIDE** (**S**poofing identity, **T**ampering with data, **R**epudiation threats, **I**nformation disclosure, **D**enial of Service and **E**levation of privileges) dan **PASTA** (**P**rocess for **A**ttack **S**imulation and **T**hreat **A**nalysis).

STRIDE mencakup enam prinsip utama, yaitu

- **Spoofing,** prinsip ini mengharuskan Anda untuk mengautentikasi *requests* dan pengguna yang mengakses sistem. Spoofing melibatkan pihak jahat yang secara salah mengidentifikasi dirinya sebagai pihak lain. Kunci akses seperti API keys atau signature melalui enkripsi membantu menangani masalah ini.
- **Tampering,** dengan memberikan anti-tampering pada suatu sistem atau aplikasi, ini membantu untuk integritas pada data. Data yang diakses haruslah disimpan secara integral dan akurat. Contohnya, toko menggunakan segel pada produk makanannya.
- **Repudiation,** prinsip ini menentukan penggunaan layanan seperti pencatatan aktivitas untuk dilacak oleh sistem atau aplikasi.
- **Information Disclosure,** aplikasi atau layanan yang menangani informasi beberapa pengguna perlu dikonfigurasi dengan tepat agar hanya menampilkan informasi yang relevan dengan pemilik yang ditampilkan.
- **Denial of Service,** aplikasi dan layanan menggunakan sumber daya sistem, kedua hal ini harus memiliki langkah-langkah agar penyalahgunaan aplikasi atau layanan tidak mengakibatkan penurunan pada seluruh sistem.
- **Elevation of Privilege,** ini merupakan skenario terburuk untuk aplikasi atau layanan. Yang berarti *user* dapat meningkatkan otorisasi mereka ke tingkat yang lebih tinggi yaitu administrator. Skenario ini sering mengarah pada eksploitasi lebih lanjut atau pengungkapan informasi.

***Incident Response* (IR)** merupakan tindakan yang diambil untuk menyelesaikan dan memulihkan ancaman pada suatu sistem. Pelanggaran keamanan pada kemanan siber disebut insiden.

Insiden diklasifikasikan menggunakan peringkat **urgensi** (ditentukan oleh jenis serangan yang dihadapi) dan **dampak** (ditentukan oleh sistem yang terkena dampak dan apa dampaknya terhadap operasi bisnis). 

![UrgencyImpactTable](https://miro.medium.com/max/1286/0*3ZGuB8DbRkaAxv-P.png)

Sebuah insiden ditanggapi oleh **C**omputer **S**ecurity **I**ncident **R**esponse **T**eam (**CSIRT**) yang mana merupakan kelompok yang telah diatur sebelumnya dengan pengetahuan teknis tentang sistem dan/ atau insiden saat ini. Agar berhasil menyelesaikan suatu insiden, berikut merupakan langkah-langkahnya (sering disebut *six phases of Incident Response):*

| Tindakan  | Keterangan |
| --- | --- |
| Preparation (Persiapan) | Apakah kita memiliki sumber daya dan rencana untuk menangani security incident? |
| Identification (Identifikasi) | Apakah ancaman dan attacker telah diidentifikasi dengan benar agar dapat di respon? |
| Containment (Penahanan)  | Dapatkah ancaman/insiden keamanan dibendung untuk mencegah sistem atau pengguna lain terkena dampaknya? |
| Eradication (Pemberantasan) | Hapus ancaman yang aktif. |
| Recovery (Pemulihan) | Lakukan review/ tinjau seluruh sistem yang terkena dampak untuk kembali ke operasi bisnis seperti biasa. |
| Lessons Learned (Hikmah) | Apa yang dapat dijadikan pelajaran dari insiden yang terjadi? Seperti jika karyawan terkena email phising, maka karyawan harus dilatih untuk mendeteksi email phising. |
