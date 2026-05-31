sing kurang folder node_modules e instalen dewe ko terminal
gawe 2 folder server.a karo server.b
server.a isine server-a.js
server.b isine server-b.js mbi server-b.wsdl
terus index.html
package_lock.json  karo package.json e install dewe ko terminal

[ Frontend / Client ]
         |
         | 1. POST /api/checkout (JSON)
         v
+---------------------------------------+
| SERVER A: E-Commerce (REST API)       |

| ------------------------------------- |
| - Validasi stok ke Database           |
| - Konversi data JSON -> XML Envelope  |
+---------------------------------------+
         |
         | 2. Kirim SOAP Request (XML) via HTTP POST
         v
+---------------------------------------+
| SERVER B: Payment Gateway (SOAP RPC)  |
| ------------------------------------- |
| - Proses transaksi keuangan           |
| - Update tabel `transaksi` ke DB      |
| - Generate XML Response               |
+---------------------------------------+
         |
         | 3. Kembalikan SOAP Response (XML)
         v
+---------------------------------------+
| SERVER A: E-Commerce (SOAP Client)    |
| ------------------------------------- |
| - Parsing XML -> JSON Object          |
| - Kurangi stok di tabel `produk`      |
+---------------------------------------+
         |
         | 4. Response Akhir (JSON Status 200 OK)
         v
[ Frontend / Client ]


database e gawe dewe
CREATE DATABASE IF NOT EXISTS db_minyakku;
USE minyakku;

CREATE TABLE produk (
    id INT AUTO_INCREMENT PRIMARY KEY,
    nama_produk VARCHAR(100) NOT NULL,
    harga DECIMAL(10, 2) NOT NULL,
    stok INT NOT NULL,
    deskripsi TEXT
);

CREATE TABLE transaksi (
    id INT AUTO_INCREMENT PRIMARY KEY,
    invoice_no VARCHAR(50) UNIQUE NOT NULL,
    produk_id INT NOT NULL,
    jumlah_beli INT NOT NULL,
    total_bayar DECIMAL(10, 2) NOT NULL,
    status_pembayaran ENUM('PENDING', 'SUCCESS', 'FAILED') DEFAULT 'PENDING',
    nomor_rekening_pembeli VARCHAR(30) NOT NULL,
    referensi_soap VARCHAR(100) NULL
);

-- Masukkan data sampel produk
INSERT INTO produk (nama_produk, harga, stok, deskripsi) VALUES ....
