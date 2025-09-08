# Network Security - Week 2 Summary
|             Nama              |     NRP    |
|-------------------------------|------------|
| Danar Bagus Rasendriya        | 5027231055 |

## TCP/IP Vulnerabilities
***
TCP/IP adalah protokol yang menjadi dasar dari hampir seluruh komunikasi di internet dan jaringan modern. Jika protokol ini diserang, maka semua lapisan di atasnya ikut terancam.
### Kenapa protokol ini rentan diserang?
Karena TCP/IP adalah protokol universal yang digunakan oleh semua layanan, mulai dari Web, Email, sampai Cloud. Selain itu TCP/IP tidak berfokus pada keamanan, karena pada saat itu autentikasi dan enkripsi belum menjadi standar bawaan.

### Kerentanan pada IP - Layer 3
Protokol IP (Internet Protocol) beroperasi di Layer 3, atau biasa disebut Network Layer. Protokol ini bertanggung jawab untuk pengalamatan dan routing paket data. Karena sifatnya yang connectionless, IP hanya dapat mengirim paket tanpa menjamin urutan pengiriman. Artinya IP tidak memverivikasi keaslian alamat sumbernya.

#### Header IPv4 dan IPv6
- IPv4: Memiliki header yang lebih kompleks dengan beberapa field yang rentan dieksploitasi.
- IPv6: Header sederhana dan efisien. Field Checksum dan yang berhubungan dengan fragmentasi dihilangkan untuk mempercepat proses oleh router.

#### Jenis serangan pada layer IP
**IP Spoofing:** Penyerang memalsukan alamat IP sumber dalam sebuah paket, dengan tujuan untuk menyembunyikan identitas dan menyamar sebagai host lain yang "terpercaya".
IP Spoofing dibagi menjadi dua jenis:
- Non-blind Spoofing: Attacker masuk ke jaringan yang sama dengan host. Attacker dapat "melihat" dan mendapat paket balasan.
- Blind-Spoofing: Attacker tidak bisa melihat lalu lintas balasan, biasanya berupa serangan DoS (Denial-of-Service).
***
**Serangan ICMP (Internet Control Message Protocol):** Contohnya seperti perintah ```ping```, tapi sering disalahgunakan untuk:
- Reconnaissance: Memetakan topologi jaringan, menemukan host yang aktif, dan identifikasi OS dari target.
- DoS: Membanjiri target dengan pesan ICMP (ICMP Flood).

Amplification and Reflection Attack: Serangan dengan volume yang signifikan, contohnya Smurf Attack.
1. Attacker mengirim Echo Request (```ping```) ke alamat broadcast jaringan.
2. Alamat IP sumber dipalsukan menjadi alamat IP target (Spoofed).
3. Semua host di jaringan tersebut membalas (reflect) ICMP Request ke alamat target.
4. Target akan dibanjiri (Flooded) oleh lalu lintas balasan dari puluhan atau ratusan host, yang menyebabkan Denial of Service (DoS).
***
### Kerentanan pada Transport Protocol (TCP & UDP) - Layer 4
Transport Layer (Layer 4) bertanggung jawab dalam pengelolaan sesi komunikasi antar aplikasi dengan menggunakan protokol TCP dan UDP.

#### TCP (Transmission Control Protocol)
Protokol yang connection-oriented. Sebelum data dikirim, TCP akan membangun koneksi melalui proses yang disebut **Three-Way-Handshake** (SYN, SYN-ACK, ACK). Keamanan dijamin dengan penggunaan sequence number untuk pengurutan data dan ACK (Acknowledgment) untuk konfirmasi penerimaan.

#### Serangan umum pada TCP
- TCP SYN Flood: Mengekploitasi proses Three-Way-Handshake dengan mengirim paket SYN dalam jumlah besar menggunakan alamat IP palsu. Target akan merespons dengan SYN-ACK dan mengalokasikan sumber daya untuk menunggu balasan ACK yang tidak akan pernah datang. Akibatnya koneksi server penuh dan server tidak bisa menerima koneksi baru dari pengguna yang sah.
- TCP Session Hijacking: Attacker mengambil alih sesi TCP yang sudah terjalin dan terautentikasi. Dengan pemalsuan alamat IP dari pihak dalam satu komunikasi dan memprediksi sequence number berikutnya.
- TCP Reset Attack: Attacker mengirim segmen TCP dengan ```flag``` RST (reset) yang diaktifkan ke ujung komunikasi, menyebabkan koneksi diputus secara paksa.

#### UDP (User Datagram Protocol)
UDP adalah kebalikan dari TCP, bersifat connectionless. UDP tidak melakukan handshake atau menggunakan ACK, sehingga punya overhead yang lebih rendah dan cepat. UDP cocok untuk aplikasi yang memprioritaskan kecepatan daripada keandalan, seperti Streaming Video, VoIP, dan DNS.

#### Serangan umum pada UDP
- UDP Flood Attack: Serangan sederhana dengan cara mengirimkan paket UDP dengan jumlah besar ke berbagai port di server target. Dengan begitu bandwidth jaringan dan sumber daya server akan habis. Jika paket UDP dikirim ke port yang tertutup, server akan respons dengan ICMP Port Unreachable. Banjir respons ICMP tersebut semakin membebani jaringan dan menyebabkan Dos (Denial of Service).
