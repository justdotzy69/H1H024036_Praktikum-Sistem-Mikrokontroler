# Laporan Praktikum Sistem Mikrokontroler
## Modul 3: Protokol Komunikasi

**Nama:** Khoirul Rosyid Gunawan  
**NIM:** H1H024036  

---

### 3.5.4 Pertanyaan Praktikum : 

1. **Jelaskan proses dari input keyboard hingga LED menyala/mati!**
   
   Proses dimulai saat karakter diketikkan pada Serial Monitor di PC. Data tersebut dikirim sebagai bit serial melalui kabel USB (protokol UART) menuju pin RX Arduino. Program di dalam Arduino secara kontinu mengecek buffer; jika data tersedia, fungsi `Serial.read()` akan mengambil karakter tersebut. Jika karakter yang dibaca adalah '1', perintah `digitalWrite(PIN_LED, HIGH)` akan dieksekusi untuk menyalakan LED, dan sebaliknya untuk '0'.

2. **Mengapa digunakan Serial.available() sebelum membaca data? Apa yang terjadi jika baris tersebut dihilangkan?**

   `Serial.available()` digunakan untuk memastikan bahwa data benar-benar sudah masuk ke dalam buffer sebelum Arduino melakukan pembacaan. Apabila baris ini dihilangkan, fungsi `Serial.read()` akan terus dieksekusi di setiap siklus `loop()`. Karena program berjalan sangat cepat, ia akan sering membaca nilai kosong (-1), yang mengakibatkan logika error (seperti pesan "Perintah tidak dikenal") muncul terus-menerus meskipun tidak ada input.

3. **Modifikasi program agar LED berkedip (blink) ketika menerima input '2' dengan kondisi jika '2' aktif maka LED akan terus berkedip sampai perintah selanjutnya diberikan dan berikan penjelasan disetiap baris kode nya!**
   
   ```cpp
   void loop() {
     // Mengecek apakah ada data yang masuk di Serial Monitor
     if (Serial.available() > 0) {
       char input = Serial.read(); // Membaca data yang masuk
       // Memastikan input bukan karakter enter/newline
       if (input != '\n' && input != '\r') {
         mode = input; // Menyimpan input status ke variabel 'mode'
       }
     }

     // Logika untuk membuat LED berkedip saat mode '2' aktif
     if (mode == '2') {
       digitalWrite(PIN_LED, HIGH); // Menyalakan LED
       delay(500);                  // Jeda 0.5 detik
       digitalWrite(PIN_LED, LOW);  // Mematikan LED
       delay(500);                  // Jeda 0.5 detik
     } 
     // ... (logika mode '1' dan '0' menyesuaikan seperti biasa)
   }

   ```

4. **Tentukan apakah menggunakan delay() atau millis()! Jelaskan pengaruhnya terhadap sistem!**
   
Sangat disarankan menggunakan millis() untuk sistem yang responsif. Penggunaan delay() bersifat blocking, yang berarti Arduino akan "berhenti" dan tidak bisa menerima perintah baru dari keyboard selama masa tunggu (misalnya saat LED sedang blink). Sebaliknya, millis() bersifat non-blocking, sehingga Arduino tetap bisa membaca input Serial secara instan meskipun sedang menjalankan tugas lain.

   ### 3.6.4 Pertanyaan Praktikum :

   1. **Jelaskan bagaimana cara kerja komunikasi I2C antara Arduino dan LCD pada rangkaian tersebut!**
      
       Komunikasi I2C menggunakan sistem Master-Slave, di mana Arduino (Master) mengontrol LCD (Slave) melalui dua kabel: SDA (Data) dan SCL (Clock). Arduino mengirimkan instruksi ke alamat unik LCD (misal 0x27). Sinyal clock (SCL) memastikan pengiriman data (SDA) berjalan sinkron, sehingga teks atau nilai ADC yang dikirim dapat diterjemahkan dengan benar oleh modul I2C backpack pada LCD

   2. **Apakah pin potensiometer harus seperti itu? Jelaskan yang terjadi apabila pin kiri dan pin kanan tertukar!**

      Pin tengah potensiometer wajib ke pin analog (A0) sebagai pembagi tegangan. Apabila pin kiri (GND) dan pin kanan (5V) tertukar, potensiometer tetap akan berfungsi secara teknis. Namun, arah pembacaan nilainya akan terbalik: memutar knop searah jarum jam yang seharusnya memperbesar nilai ADC justru akan memperkecilnya, begitu juga sebaliknya

3. Modifikasi program dengan menggabungkan antara UART dan I2C (keduanya sebagai
output) sehingga:
- Data tidak hanya ditampilkan di LCD tetapi juga di Serial Monitor
- Adapun data yang ditampilkan pada Serial Monitor sesuai dengan table berikut:
ADC: 0 Volt: 0.00 V Persen: 0%
Tampilan jika potensiometer dalam kondisi diputar paling kiri
- ADC: 0 0% | setCursor(0, 0) dan Bar (level) | setCursor(0, 1)
```cpp

// Mengirim data ke Serial Monitor agar bisa dibaca di PC
Serial.print("ADC: "); Serial.println(nilai); 

// Menampilkan data yang sama ke layar LCD I2C
lcd.setCursor(0, 0); // Mengatur posisi kursor di baris pertama
lcd.print("ADC: ");  // Menulis label teks
lcd.print(nilai);    // Menampilkan nilai numerik dari potensiometer

```


4. **Tabel Pengamatan Nilai Konversi**


| ADC | Volt (V) | Persen (%) |
| --- | -------- | ---------- |
| 1   | 0.00     | 0%         |
| 21  | 0.10     | 2%         |
| 49  | 0.24     | 4%         |
| 74  | 0.36     | 7%         |
| 96  | 0.47     | 9%         |
