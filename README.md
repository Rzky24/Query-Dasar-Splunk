# Query-Dasar-Splunk

PASTIKAN DATA ADA: index=windows_lab
Kalau ini kosong → masalah ingest
Kalau ADA → lanjut

index=windows_lab
| table _time _raw

# tujuan

_raw = log asli
Kita membaca manual isi event
Cari di _raw:
angka 4625
kata failed
kata logon
kata administrato

# QUERY 1 — FILTER LOGIN GAGAL (DASAR)
index=windows_lab "Status=failed"
Penjelasan per bagian

index=windows_lab
 Ambil data hanya dari index windows_lab

"Status=failed"
 String search
 Splunk mencari teks persis Status=failed di _raw

 Fungsi
Menampilkan SEMUA login yang gagal
Ini langkah pertama deteksi brute force

# QUERY 2 — LIHAT STRUKTUR LOG
index=windows_lab "Status=failed"
| table _time _raw

Penjelasan
| → pipe (lanjut ke perintah berikut)
table → menampilkan field tertentu
_time → waktu event
_raw → log mentah

Fungsi
Membaca format log asli
Menentukan field apa yang bisa dianalisis

QUERY 3 — TAMPILKAN FIELD PENTING (KALAU ADA)
index=windows_lab "Status=failed"
| table _time User Status IP Host

Penjelasan
User → username login
Status → hasil login
IP → sumber akses
Host → mesin tujuan
Kalau ada field kosong, itu normal

Fungsi
Membaca siapa, dari mana, kapan

#QUERY 4 — HITUNG FAILED LOGIN PER USER
index=windows_lab "Status=failed"
| stats count by User

Penjelasan
stats → fungsi statistik
count → menghitung jumlah event
by User → dikelompokkan per user

 Fungsi
Mendeteksi user dengan gagal login terbanyak
Kandidat brute force
